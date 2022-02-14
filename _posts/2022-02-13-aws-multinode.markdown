---
layout: post
title:  "Multi-Node Distributed Neural Network Training on AWS"
date:   2022-02-13 00:21:00 -0400
categories: jekyll update
---
note: the following tutorial will use an example with two p3.16 instance (16 V100 GPUs on 2 nodes), but it is easy to generalize to more instances (if AWS's use limit and capacity availability agrees)

0. Launch multiple instances at once in AWS management console
	* click ```Launch Instances``` button
	* change the number of instances to 2 in ```3. Config Instances```
1. Allow all traffic (by changing inbound/outbound rules) in the security group with all instances
2. ssh to instances (in different terminals)
	* run ```ssh -i {PRIVATE KEY} {INSTANCE NAME}@{PUBLIC IPv4 ADDRESS}```
3. Config network interface for every node
	* run ```ifconfig``` to find the name (e.g. ens3)
	* run ```export NCCL_SOCKET_IFNAME=ens3``` to set the environment variable
4. Add multi-node support in python script
	* in ```init_process_group```, we should provide:
		* url: one can use the private IPv4 address of node 0 and the port name, e.g. ```tcp://172.31.22.234:23456```
			* this is one of the initialization method (other option e.g. shared file system, but need more configurations)
		* node index (e.g. 0 and 1 with two nodes)
		* number of processors per node: to calculate the global rank of a GPU
		* world size: total number of GPUs to be used in all nodes
		* global rank: calculated from the above parameters and local rank of a GPU
	* note: local rank of a GPU will be automatically set by ```pytorch.distributed.launch``` (see how to use it in the next step)
5. Use ```pytorch.distributed.launch``` to run training script in distributed setting
	* run ```python -m torch.distributed.launch --nproc_per_node=8 --nnodes=2 {PYTHON_FILE_NAME} {ARGS OF PYTHON SCRIPT}```
6. Go fast girl!

# Reference
* [pt-distributed-tutorial by Nathan Inkawhich](https://github.com/inkawhich/pt-distributed-tutorial/blob/master/pytorch-aws-distributed-tutorial.py)
