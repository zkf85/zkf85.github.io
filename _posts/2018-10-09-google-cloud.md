---
layout: post
comments: true
title: "Experiencing google cloud"
date: "2018-10-09 09:00:00"
category: [Deep Learning]
tag: [google cloud, deep learning]
---

>After experiencing google cloud for couple of week, I'd like to write something down here. On the one hand, there're lots of tips, necessary knowledge and pitfalls to remember and check. One the other hand, just cannot resist to complain about this giant and complicated service system.

<!--more-->

## Why Google Cloud 
So where shall we start? OK, first why Google Cloud? The reason for me is that it's actually free to use. Well although it's not literally free. As long as you apply an "free trial" account and enter your credit card info (they won't automatically charge you, for sure), they will grant you $300 credit within one year to use for free completely. I have to say the google is really generous in this case. 

This screen shot below is my current balance as I've already used the google cloud for training with or without GPUs for about 2 weeks. $300 is worth several month decent using.

![](/public/img/20181009-01.png)

## Bunch of services - in a nut shell
Google cloud provides a huge set of services. If you have no experience of using cloud service, it's pretty hard to get started becuase you'd be dazzled by the the countless stuff.

To get started, especially for those who want to leverage google cloud as a tool for deep learning, I'd like to list the frequently used services.

### 1. Cloud computing
Cloud computing is most frequently used service in my usage. As introduced by google itself, it provides *"Scalable, High-Perfoermance Virtual Machines"*.
To make it clear in a few words, this computing engine provides **computing resource** such as CPU/GPU, memories, with a separate  pricing rule (only based on how many CPUs, GPUs, RAMs you are using) excluding the storage info.

When your VM is started, cloud computing service starts charging your credit. But as long as you shut it down. The charge from compute engine is stopped.

### 2. Storage
The compute engine cannot run without a disk, which is related to the storage service. For instance, when you create a VM, you always also apply a disk of 10G or 30G space for the computing engine you chosed. (But remember, the fee for storage is charged separately. ) Basically, there are two different kind of  storage service that is most frequently use.

#### 2.1 Persistent Disk
In short, the persistent disk is block storage for virtual machine instances and storage to be mounted to VMs. 
- It's just like normal disks.
- You have to let your VM running before you can access this kind of storage.
- It's easy to manipulate the files in the disks.
- The persistent disk charges with it's own rates.

#### 2.2 Cloud Storage
It's object storage, in the actual use, each cloud storage block is  called a 'bucket'. 
- You can access this kind of storage by using their `gsutil` tool from cloud shell or even from your computer using google cloud sdk. 
- You do not need to start any VM to access your data stored in the 'buckets'. 
- It's a great tool for uploading and downloading files from google cloud. Multi-thread `gsutil -m scp` makes it much faster than normal `scp` in POSIX file system.
- You have to use `gsutil` to access and manipulate the data in the storage, which is a little bit awkward when you need to treate the files and folders in your 'traditional' mindset. 
- the cloud storage charges with its own rates, roughly based on the amount of storage and the inward/outward stream amout.

## Pricing
The story about pricing is trivial! This reminds me of the main reason why I usually prefer Apple's products rather than google's. 
> IT'S SHOULD BE SIMPLE!!!
When I was checking the pricing rate of these google cloud services, it's like, suppose I want to buy a new iPhone and go to their store and ask: "I'd like to buy an iPhone, which one should I choose?" Think that if the guy tells me "well that depends on what kind of processor, storage, RAMs, camera you want. Here is the pricing list and help yourself ..." 
**See? this would drive me crazy. And this is how you feel when you are checking google cloud pricing**. 
The real way apple store would respond is: "Here are three new iPhones with basic price of \$749, \$999 and \$1099, you can add more storage with extra price ..."

My dear google, why don't you make the pricing a little bit straight forward? Wouldn't a rough approximation for every service rate in one page as a pricing overview be much better???

A little digressed from the subject. Here I give a rough pricing list for most the services mentioned above.

| Service | Rate per Hour | Rate per Day | Rate per Month |
| :-: | --: | --: | --: |
| CPU VM |  | ~$1.00 |  |
| GPU VM  | ~$1.00 | - | - |
| Persistent Disk | - | - | ~$2.00 per 100G |
| Cloud Storage | - | - | ~$2.00 per 100G |

I hope in this way you can have a general idea of the pricing of these google cloud services.

## Commands
In the following I'll continue to list some commonly use commands with google cloud's daily-based usage.

### Compute Engine Basic
- Set/Unset zone:
```bash
# Set zone
gcloud config set compute/zone <zone-name>
# Unset zone
gcloud config unset compute/zone <zone-name>
```

- List all the VM instances:
```bash
gcloud compute instances list
```

- Start a VM instance:
```bash
gcloud compute instances start <instance-name> [--zone=<zone-name>]
```

- Terminate a VM instance:
```bash
gcloud compute instances stop <instance-name> [--zone=<zone-name>]
```

- SSH login to VM instance:
```bash
gcloud compute ssh [<user-name>@]<instance-name> [--zone=<zone-name>]
```

- Transfer files from/to the VM instance:
```bash
# From local to VM instance
gcloud compute scp [--recurse] <file-dir> <user-name>@<instance-name>:<dest-dir>
# From VM instance to local
gcloud compute scp [--recurse] [<user-name>@]<instance-name>:<file-dir> <dest-dir>
```

### Mount/Unmount Persistent Disk
[Adding or Resizing Persistent Disks](https://cloud.google.com/compute/docs/disks/add-persistent-disk)
- Create a persistent disk:
```sh
gcloud compute disks create <disk-name> --size <disk-size> --type <disk-type>
```
where:
    - <disk-name> is the name of the new disk.
    - <disk-size> is the size of the new disk in GB.
    - <disk-type> is the type of persistent disk. Either `pd-standard` or `pd-ssd`.

- Attach a disk to any running or stopped instance
```sh
gcloud compute instances attach-disk <instance-name> --disk <disk-name>
```

- In an instance, list disks attached to the current instance:
```sh
sudo lsblk
```

- Format the disk: (usually, the `DEVICE_ID` is `sdb`)
```sh
sudo mkfs.ext4 -m 0 -F -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/<DEVICE_ID>
```

- Mount the disk to the instance:
```sh
mount -o discard, defaults /dev/<DEVICE_ID> <mnt_dir>
```
where
    - the `DEVICE_ID` is often `sdb`
    - `mnt_dir` is the directory where you want the disk to be mounted, usually you create such folder before this `mount` command.

- Grant write access to the device for all users:
```sh
chmod a+w <mnt_dir>
```

- Add the persistent disk to `etc/fstab` file so t hat the device automatically mounts again when the instance restarts.

    1. Create a backup of your current
    ```sh
    sudo cp /etc/fstab /etc/fstab.backup
    ```
    
    2. Use the blkid command to find the UUID for the persistent disk. The system generates this UUID when you format the disk. Use UUIDs to mount persistent disks because UUIDs do not change when you move disks between systems.
    3. Create a backup of your current `/etc/fstab` file.
    ```sh
    $ sudo blkid /dev/[DEVICE_ID] /dev/[DEVICE_ID]: UUID="[UUID_VALUE]" TYPE="ext4"
    ```
    where:
        - `[DEVICE_ID]` is the device ID of the persistent disk that you want to automatically mount. If you created a partition table on the disk, specify the partition that you want to mount.
        - `[UUID_VALUE]` is the UUID of the persistent disk that you must include in the `/etc/fstab` file.
    4. Open the `/etc/fstab` file in a text editor and create an entry that includes the UUID. For example:
    ```sh
    UUID=[UUID_VALUE] /mnt/disks/[MNT_DIR] ext4 discard,defaults,[NOFAIL_OPTION] 0 2
    ```
    where:
        - `[UUID_VALUE]` is the UUID of the persistent disk that you must include in the `/etc/fstab` file.
        - `[MNT_DIR]` is the directory where you mounted your persistent disk.
        - `[NOFAIL_OPTION]` is a variable that specifies what the operating system should do if it cannot mount the persistent disk at boot time. To allow the system to continue booting even when it cannot mount the persistent disk, specify this option. For most distributions, specify the nofail option. For Ubuntu 12.04 or Ubuntu 14.04, specify the nobootwait option.
    
### Cloud Storage Operation Tools
[Connecting to Cloud Storage buckets](https://cloud.google.com/compute/docs/disks/gcs-buckets)
[Quickstart: Using the gsutil Tool](https://cloud.google.com/storage/docs/quickstart-gsutil#create)

1. Create a bucket by using `gsutil mb` command:
```sh
gsutil mb -l us-east1 gs://my-awesome-bucket/
```
note that the bucket name should be globally-unique.

2. Copy local file to the bucket using `gsutil cp` command:
```sh
gsutil [-m] cp [-r] <file-name> gs://my-awesome-bucket
```
**Remember**: using `-m` flag to activate multi-thread will increase the transfer speed dramatically.

3. Download file from bucket using `gsutil cp` command:
```sh
gsutil [-m] cp [-r] gs://my-awesome-bucket/<file-name> <local-dir>
```

4. Delete an object using `gsutil rm`:
```sh
# Delete file
gsutil rm gs://my-awesome-bucket/<file-name>
# Delete folder
gsutil rm -r gs://my-awesome-bucket/<folder-name>
```

5. Remove the bucket
```sh
gsutil rm -r gs://my-awesome-bucket
```

 
<br>***KF*** 
