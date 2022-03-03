---
layout: post
title:  "Get Started with SciNet Mist"
date:   2022-03-02 00:21:00 -0400
categories: jekyll update
---
This is a tutorial for those who are new to Mist, a GPU cluster in the SciNet supercomputer center.

0. Register a [Compute Canada Database (CCDB)](https://ccdb.computecanada.ca/) account
	* there are multiple roles for an account, for student researcher to be added to a group, need a sponsor code
	* it takes a few hour for the account to be activated by consortium
1. Request access to Niagara and Mist on [Compute Canada](https://ccdb.computecanada.ca/services/opt_in?)
	* takes hours, will be notified by email
2. Set up ssh public/private key:
	* generate key via ```ssh-keygen -t $TYPE -f $KEY_NAME```, where ```$TYPE``` could be any of these preferred public key algorithm: rsa, dsa, ecdsa, ed25519
	* copy public key (i.e the file with extension .pub) to Compute Canada's webpage, select MyAccount -> Manage SSH Keys
	* give the key a name, then click add key
3. ssh to Mist
	* ```ssh -i $USERNAME -Y $USERNAME@mist.scinet.utoronto.ca```, where ```$USERNAME``` is that of the CCDB account registered in step 1
4. Load/Install software modules
	* load anaconda: ```module load anaconda3```
	* create a virtual environment: ```conda create -n $ENV_NAME python=$PYTHON_VERSION```
	* install any requirements in IBM Open-CE Conda Channel:
		* e.g. PyTorch, CUDAToolkit: ```conda install -c /scinet/mist/ibm/open-ce pytorch=1.10.2 cudatoolkit=11.2```
5. (Heads up) Large dataset like ImageNet can exhaust disk quota in personal directory under ```/home($HOME)```, it should be downloaded and stored in personal directory under  ```/scratch($SCRATCH)```
6. (Optional, but recommended) Request a debugjob via ```debugjob --clean -g $NUM_GPUS``` and test the code on a small scale experiment first. This command gives an interactive session equivalent to 1 full hour compute

# Reference
* [Compute Canada Documentation](https://docs.computecanada.ca/wiki/Compute_Canada_Documentation)
* [Getting Access to SciNet Systems and Services](https://www.scinethpc.ca/getting-a-scinet-account/)
* [Keygen (SSH Academy)](https://www.ssh.com/academy/ssh/keygen)
* [Mist Documentation](https://docs.scinet.utoronto.ca/index.php/Mist)
