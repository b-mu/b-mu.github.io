---
layout: post
title:  "Download ImageNet-1k Dataset on AWS"
date:   2022-02-12 00:21:00 -0400
categories: jekyll update
---

0. Launch a t2.large instance
1. Create a 1000G GP2 volume on AWS (has to be in the same zone with the instance)
2. Attach the volume created in step 1 to the instance
3. Format the volume and mount in the system
	* run ```lsblk``` to check the device name of the volume created in step 1
		* e.g. ```/dev/xvdf```
	* format the volume: ```sudo mkfs -t ext4 $DEVICE_NAME```
	* mount the volume:
		* ```sudo mkdir imagenet```
		* ```sudo mount $DEVICE_NAME imagenet``` where ```DEVICE_NAME``` should be replaced by that found above
4. Sign up on [image-net.org](https://image-net.org/index.php) (if one does not have an account)
5. Download [ImageNet Large Scale Visual Recognition Challenge 2012 (ILSVRC2012)
](https://image-net.org/challenges/LSVRC/2012/2012-downloads.php)
	* Training data
		* right click Training images (Task 1 & 2) on ImageNet's website and copy link address
		* run ```sudo nohup wget $LINK``` where ```$Link``` should be replaced by the copied link address from website (takes hours!)
		* extract: ```tar -xf ILSVRC2012_img_train.tar```
		* create directories ```find . -name "*.tar" | while read NAME ; do mkdir -p "${NAME%.tar}"; tar -xvf "${NAME}" -C "${NAME%.tar}"; rm -f "${NAME}"; done```
	* Validation data
		* right click Validation images (all tasks) on ImageNet's website and copy link address
		* run ```sudo nohup wget $LINK``` where ```$Link``` should be replaced by the copied link address from website
		* download [```build_validation_tree.sh```](https://github.com/juliensimon/aws/blob/master/mxnet/imagenet/build_validation_tree.sh)
		* create directories by running ```sh build_validation_tree.sh```
	* note:
		* both training and validation dataset should have 1000 directories in total, from n01440764 to n15075141
		* make sure only directories with training images are in the "train" and "validation" directories, otherwise, the target can be wrong at training
6. Create a snapshot from the volume that contains the dataset in AWS EC2 management console
7. Terminate the instance used to download the dataset. Launch a new instance for training. Attach the volume to the new instance for training and mount the volume as in step 3.
8. Load the dataset to the training program with ```torchvision.datasets.ImageFolder```
9. Start training!

# Reference
* [```[Hands-on]``` Fast Training ImageNet on on-demand EC2 GPU instances with Horovod](https://housekdk.gitbook.io/ml/ml/cv/imagenet-horovod)
* [Learning Machine Learning on the cheap: Persistent AWS Spot Instances](https://blog.slavv.com/learning-machine-learning-on-the-cheap-persistent-aws-spot-instances-668e7294b6d8)
