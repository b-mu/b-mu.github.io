# mac os 10.15 + bootcamp(win 10) + nvidia eGPU

0. back up mac

1. bootcamp win 10
* download [win 10 iso](https://www.microsoft.com/en-ca/software-download/) (~5.7G)
* launch bootcamp, select a partition size, install windows ([Apple's instruction](https://support.apple.com/en-ca/HT201468))
* system will reboot in windows

2. nvidia drivers
* plugin gpu and turn on gpu power supply (for msi RTX2080 super, needs all of 2 x 8 pins)
* install [geforce experience] (https://www.nvidia.com/en-us/geforce/geforce-experience/)
* check drivers update in geforce experience
* if you need nvidia control panel but it is missing, try: [standard driver instead of DCH drivers](https://www.youtube.com/watch?v=Ytnv8XJ_hV4) by using [advance drivers search](https://www.nvidia.com/Download/Find.aspx)

3. anaconda
* install [anaconda](https://www.anaconda.com/products/individual)
  * tick add anaconda to Path variable
  * or do it manually by start -> type "env var" -> click "edit the system environment variables" -> click "environment variables" -> click "Path" under User variables and click edit -> add "C:\Users\[user_name]\anaconda3; C:\Users\[user_name]\anaconda3\Scripts;"
* create env ```conda create --name env_gpu```
* install tensorfow-gpu 1 ```conda install tensorflow-gpu=1.15``` (this will automatically install cuda 10.0 and cudnn 7.6, [compatible](https://www.tensorflow.org/install/source) with tensorflow 1.15)
* check if gpu is visible: [several methods](https://stackoverflow.com/questions/38009682/how-to-tell-if-tensorflow-is-using-gpu-acceleration-from-inside-python-shell/38019608), alternatively, run the command: ```nvidia-smi```

4. git-bash
* install [git](https://git-scm.com/downloads)
* if anaconda is successfully added to Path variable, then the command "conda activate env_gpu" should be recognized and the environment should be activated

up to now, python scripts should be able to run in this environment. on the other hand, since it is in a bash shell, .sh scripts should also work.
