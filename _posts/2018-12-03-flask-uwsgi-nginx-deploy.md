---
layout: post
comments: true
title: "Deploy Keras Model with Flask+uWSGI+NGINX with docker"
date: "2018-12-03 15:21:00"
category: [Operating System, Deep Learning]
tag: [docker, flask, uwsgi, nginx, keras, tensorflow]
---

> After spending more than 200 credits on google cloud GPU for training a **Plant Disease Recoginition (PDR)** model with Keras, I've got a decent model that have reached more than 85% accuracy. The model is ready and the next thing to do is to deploy the model for inference as a service.

 <!--more-->

## 1. Background

### 1.1. Objective
The main objective is to build a web service API which responds with a inference result as a json file after receiving a request with an image in it.

### 1.2. What we have?
At the moment, we have a well-built keras model, trained with `keras==2.2.4` and `tensorflow==1.11.0` in `python==3.5.2`.

### 1.3. What strategy we choose?
Basically, it's **"Flask + uWSGI + NGINX"**:

- [Flask](http://flask.pocoo.org) is a good python microframework for web development. It is pretty easy to make an improvised API with Flask. But it's not recommended to use it to build a formal production.
- [uWSGI](https://uwsgi-docs-additions.readthedocs.io/en/latest/) aims at developing a full stack for building hosting services. uWSGI is implemented as a linker between Nginx(does not support python) and Flask(written in python).
- [NGINX](https://www.nginx.com/resources/wiki/)  ( [/ˌɛndʒɪnˈɛks/ EN-jin-EKS](https://en.wikipedia.org/wiki/Nginx)) is a free, open-source, high-performance HTTP server and reverse proxy.


## 2. Deploy the **Flask + uWSGI + NGINX** (FUN) Combo Manually

Before everything, there's a complete tutorial on `digitalocean` about this:

[>> **How to serve Flask Application with uWSGI and NGINX on Ubuntu 18.04**](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uswgi-and-nginx-on-ubuntu-18-04)

### 2.1. Flask
Here are the basic steps for setting up Flask:

In your main python script (e.g. `myproject.py`),
Import related tools:
```python
import flask
from flask import Flask, request, Response
```

Initialize the Flask application:
```python
app = Flask(__name__)
```

Setup the app route and method, return the response as JSON file:
```python
@app.route('/predict', methods=['POST'])
def predict():
...
response = ...

return flask.jsonify(response)

```

A complete sample code of `myproject.py` is listed below:
```python
# Filename   :  myproject.py
# Written by :  KF 

import flask
from flask import Flask, request, Response
import numpy as np
from keras.preprocessing.image import load_img
from tensorflow.keras.models import load_model
from keras.preprocessing.image import img_to_array
import tensorflow as tf

# Initialize the Flask application
app = Flask(__name__)

def init():

global model, img_shape, idx_dict
global graph

# Basic parameters
model_file_name = "disease_224.model"
label_file_name = "labels.npz"
img_shape = (224, 224, 3)

# Load Keras model
model = load_model(model_file_name)
# Initialize a global graph for Keras/tensorflow
graph = tf.get_default_graph()
# Load index-label list (not important here in this article)
labels = np.load(label_file_name)
label_dict = labels['class_idx'].tolist()
idx_dict = {y:x for x, y in label_dict.items()}

# route http posts to this method
@app.route('/predict', methods=['POST'])
def predict():

# Get the image in the POST request 
image_file = request.files['image']

# Load the image for Keras model
img = load_img(image_file, target_size=img_shape)

img_np = img_to_array(img)/255.0
img_np = np.expand_dims(img_np, axis=0)

# Predict
with graph.as_default():
	print('Start predicting ...')
	proba = model.predict(img_np, verbose=1)[0]
	#proba = [1,2,3,4,5]
	print('Prediction complete！')

res_idx = np.argmax(proba)

# Mapping the result index to label (not important for this article)
best_prediction_label = int(idx_dict[res_idx])

# Build a response dict to send back to client
response = {}
response['message'] = 'image received!'
response['best_prediction'] = best_prediction_label

return flask.jsonify(response)


# Initialize first no matter if it's main or not ...
init()

if __name__ == "__main__":
print(("* Loading Keras model and Flask starting server..."
"please wait until server has fully started"))
# start the flask app, allow remote connection
app.run(host='0.0.0.0', threaded=True)
```

> **Note**:
> 1. Anytime this main `myproject.py` script is called, the model loading and parameters loading steps should be put in the `init()` in order to avoid running the slow process everytime when predicting.
> 2. In the initialization, a global `graph` for keras/tensorflow is initialized. Without this, there'll be an error.
> 3. To change the port (e.g. change to 8000, simply change the last line to `app.run(host='0.0.0.0', port='8000')`.

Run/Test the API with Flask simply by:
```sh
$ python myproject.py
```

If the log is as below, it means the Flask API is working well,
```sh
* Loading Keras model and Flask starting server...please wait until server has fully started
* Serving Flask app "myproject" (lazy loading)
* Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
* Debug mode: off
* Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)

```

### 2.2. Test the API

On another computer, write a `POST` request in python using `requests` library.
In `api_test.py`:
```python
# Filename   :  api_test.py
# Written by :  KF 

import requests
import os

addr = 'http://192.168.1.235:5000'
#addr = 'http://192.168.1.235'
test_url = os.path.join(addr, 'predict')

files = {'image': open('test_image.JPG', 'rb')}

# send http request with image and receive response
response = requests.post(test_url, files=files)

#decode response
print(response.content)
```

If you followed the initial server setup guide, you should have a UFW firewall enabled. To test the application, you need to allow access to port 5000:
```sh
$ sudo ufw allow 5000
```

Make sure that the ip address and port number is correct. Then it should work.


### 2.3. uWSGI
As we can see in the log info of running Flask, it warns that **`"Do not use the development server in a production environment"`**, which means that Flask by itself is "OK" for testing in development but **not** designed for production. Therefore, we need to deploy it in a more professional environment, as it says, **"Use a production WSGI server instead"**. 

[uWSGI (WSGI - Web Server Gateway Interface)](https://uwsgi-docs-additions.readthedocs.io/en/latest/) is used here as a tool for connecting **Flask** and **NGINX**.

#### 2.3.1. Create a uWSGI Configuration File
Let's place that file in our project directory and call it `myproject.ini`, in the file, add the following snippet:
```ini
[uwsgi]
module = wsgi:app

processes = 1
vacuum = true
die-on-term = true
socket = /tmp/myproject.sock
chmod-socket = 666

#master = true
master = false
```
> **Note**:
> 1. `socket` points to a temporery file generated later when the service is on, pointing it to `/tmp/` and change its permission to 666 make sure there's not a permission problem.
> 2. `processes` is set to `1` in my case. If not, my API will be stuck at the prediction step.
> 3. `master` is set to `false`. If not, my API will be stuck at t he prediction step.

#### 2.3.2. Creating a systemd Unit File
Next, let's create the systemd service unit file. Creating a systemd unit file will allow Ubuntu's init system to automatically start uWSGI and serve the Flask application whenever the server boots.

Create a unit file ends with .service (e.g. `myproject.service`) within the `/etc/systemd/system` directory with the following snippet:
```ini
[Unit]                                                                                                                
Description=uWSGI instance to serve myproject
After=network.target

[Service]
User=kefeng
Group=www-data

WorkingDirectory=/home/kefeng/PlantDiseaseRecognition/myproject
Environment="PATH=/home/kefeng/anaconda3/bin"

ExecStart=/usr/local/bin/uwsgi --ini myproject.ini

[Install]
WantedBy=multi-user.target
```

We can now start the uWSGI service we created and enable it so that it starts at boot:
```sh
$ sudo systemctl start myproject
$ sudo systemctl enable myproject
```

Check the status:
```sh
$ sudo systemctl status myproject
```

The output should be like this:
```
● myproject.service - uWSGI instance to serve myproject
   Loaded: loaded (/etc/systemd/system/myproject.service; disabled; vendor preset: enabled)
   Active: active (running) since Mon 2018-12-03 17:58:25 CST; 1h 7min ago
 Main PID: 3889 (uwsgi)
    Tasks: 13 (limit: 4915)
   CGroup: /system.slice/myproject.service
           └─3889 /usr/local/bin/uwsgi --ini myproject.ini

```

### 2.4. Configuring Nginx to Proxy Requests
Our uWSGI application server should now be up and running, waiting for requests on the socket file in the project directory. Let's configure Nginx to pass web requests to that socket using the `uwsgi` protocol.

Create a new server block configuration file in Nginx's sites-available directory (e.g. `myproject`):
```sh
$ sudo vi /etc/nginx/sites-available/myproject
```

Code in `/etc/nginx/sites-available/myproject`:
```
server {
    listen 80;
    server_name 192.168.1.235;

    location / {
        try_files $uri @app;
    }
    location @app {
        include uwsgi_params;
        uwsgi_pass unix:///tmp/myproject.sock;
    }
}
```

To enable the Nginx server block configuration you've just created, link the file to the sites-enabled directory:
```sh
$ sudo ln -s /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled
```

With the file in that directory, we can test for syntax errors by typing:
```sh
$ sudo nginx -t
```

If this returns without indicating any issues, restart the Nginx process to read the new configuration:
```sh
$ sudo systemctl restart nginx
```

Adjust the firewall again. We no longer need access through port 5000, so we can remove that rule. We can then allow access to the Nginx server:
```sh
$ sudo ufw delete allow 5000
$ sudo ufw allow 'Nginx Full'
```

If you encounter any errors, trying checking the following:
- `sudo less /var/log/nginx/error.log: checks the Nginx error logs.`
- `sudo less /var/log/nginx/access.log: checks the Nginx access logs.`
- `sudo journalctl -u nginx: checks the Nginx process logs.`
- `sudo journalctl -u myproject: checks your Flask app's uWSGI logs.`

### 2.5 Test the newly established API
Use the same python file created in 2.2, only remove the port number since the nginx api use the default port 80.

In `api_test.py`, make the following change:
```python
addr = 'http://192.168.1.235'
```

It should work fine as the same as with Flask alone.


## 3. Deploy **Flask + uWSGI + NGINX** (FUN) combo with Docker 

Using Docker to deploy the service is much easier. You can save most work described above.

Check out [`tiangolo/uwsgi-nginx-flask-docker`](https://github.com/tiangolo/uwsgi-nginx-flask-docker) to find and download a suitable version of `Dockerfile`.

For Docker basics, see my previous article about Docker.

### 3.1. Build a customized version of docker image
In my case, I use `python3.6` version image. Besides, it is good practice to build a customized version of image for yourself since
1. Several additional python package need to be installed in the Docker;
2. Some minor modifications are needed for the nginx service;

First, create a folder with a `Dockerfile` in it;

Create a `requirements.txt` file for additional python packages to be installed in the Docker image.

In my `requirements.txt`:
```
keras==2.2.4
tensorflow==1.11.0
pillow
numpy
```

The default nginx body buffer size is too small for a POST request with an image in it. Therefore, you need to modifiy this parameter by copy a `.conf` file into the Docker image.

Create a file `kf_upload.conf`, in which add:
```
client_body_buffer_size 5m;
```

Then, write the `Dockerfile` as below:
```
FROM tiangolo/uwsgi-nginx-flask:python3.6

COPY requirements.txt /
COPY kf_upload.conf /etc/nginx/conf.d/
RUN pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r /requirements.txt
```

Finally, in the directory including `Dockerfile` build a customized version of Docker image by:
```sh
$ docker build -t kf-customized-image .
```
> Note that the `kf-customized-image` is the name of the new image you build. You may change it to anything you like.

### 3.2. Create and run a Docker container with the customized image
Create a new folder for deploying you api, in which there's a `Dockerfile` and a subfolder named `app`.

Copy the `myproject.py` file mentioned in the previous section into the `app` folder, rename it as `main.py`. Also remember to copy the files used in the `main.py` (e.g. `disease.model`, `labels.npz`) in to `app` folder too.

In the base folder (which inlcudes th `app` folder and the `Dockerfile`), create a `uwsgi.ini` file, add the following code:
```ini
[uwsgi]
socket = /tmp/uwsgi.sock
chown-socket = nginx:nginx
chmod-socket = 664
cheaper = 0
processes = 1
master = false
```

Then, add the following code into the `Dockerfile`:
```
FROM kf-customized-image
COPY uwsgi.ini /etc/uwsgi/

COPY ./app /app
```

Build the final version of image that is ready to use, in the base folder:
```sh 
$ docker build -t kf-ready-to-deploy-image .
```

Now, everything is ready for deployment. You can check if your image is ready in your Docker by:
```sh
$ docker image ls
```

Finally, one last step you need to do is to run the image as a container. 

In any working directory, just run:
```sh
$ docker run -p 80:80 kf-ready-to-deploy-image
```

> Note that the `-p` parameter is to map the Docker internal port (e.g. `80`) to your actual machine's port (e.g. `80`). 

Then the  API should be working fine.

You may use the same `api-test.py` on another computer (in the same internal network) to test if the API works appropriately. Remember to make sure that the testing port is consistent with the one set in your service.

### 3.3. Wrap it up and deploy it anywhere else
The advantage of using Docker is for its compatibility. As long as Docker is installed on your platform, no matter its Windows, Linux or macOS, you can simply deploy your service by running the image you built.

There are two ways to scale your self-built image:
1. Log in your Dockerhub account and publish your image, after which, import your image by enter its unique name:
	```sh
	$ docker login             # Log in this CLI session using your Docker credentials
	$ docker tag <image> username/repository:tag  # Tag <image> for upload to registry
	$ docker push username/repository:tag            # Upload tagged image to registry
	```

2. Save the Docker image into a `.tar` file. Load the `.tar` file on any destination machine.

	Save the Docker image with (the two commands belows are the same):
	```sh
	$ docker save --output kf-ready-to-deploy-image.tar kf-ready-to-deploy-image
	$ docker save -o kf-ready-to-deploy-image.tar kf-ready-to-deploy-image
	```

	Load the Docker image with (the two commands belows are the same):
	```sh
	$ docker save --input kf-ready-to-deploy-image.tar
	$ docker save -i kf-ready-to-deploy-image.tar
	```

## 4. Conclusion
There're not a lot of articles on deploying Keras models for production, thus I write this complete instruction on how to deploy Keras model with Flask+uWSGI+NGINX strategy. There are two ways to manage the work: 1) configure everything step by step; 2) apply docker to save your time. Both methods can provide the same production-level API with your well-trained Keras model.



<br><br>***KF*** 
