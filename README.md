# Install ROS noetic

If you want to use ROS on a Raspberry Pi or a NUC, you will need to follow this steps in order to be able to upload code on it.


## Download Ubuntu

Firstly, to use ROS (Robot Operating System) you need to have a Linux system with Ubuntu. Each version of ROS can run on only one version of Ubuntu.

For running ROS noetic, you need to download Ubuntu 20.04 from the [official website of Ubuntu](https://releases.ubuntu.com/20.04/).

You can choose the image you want.

![alt text](https://github.com/Clerbout-Francois/Install_ROS_noetic/blob/main/Ubuntu_website.png?raw=true)

_Figure 1 : Ubuntu website for the installation of the 20.04 version._

You need a USB key in order to flash it when the download of the image is finished (/!\ be sure to have enough space on your USB key, 4GO is sufficient).

## Install ROS

To install ROS you will only need a terminal. Open a terminal either on the Ubuntu desktop or via ssh (recommended) for a Raspberry.

### Configure Ubuntu repositories

You need to allow restricted, universe, and multiverse repositories. It exists 2 solutions depending if you want to use the Ubuntu Desktop or a terminal.

If you want to use the Ubuntu Desktop, open “System Settings…” > “Software & Updates” > “Ubuntu Software”. Click on the 3 check-boxes for universe, restricted and multiverse.

If you want to use the terminal, open a terminal and you can directly execute those commands in a terminal :

```
sudo add-apt-repository universe
sudo add-apt-repository restricted
sudo add-apt-repository multiverse
```

### Setup sources

Execute this command in the terminal :

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

### Setup the keys

```
sudo apt install curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

Now, simply update the sources to find new packages with the following command in the terminal :

```
sudo apt update
```

### Install ROS core packages

On the [Wiki](http://wiki.ros.org/noetic/Installation/Ubuntu) installation guide you are now given a list of choices for which ROS packages to install first.

You have three choices, it depends of your storage. I will detail them, give you the command and specify which choice is the most appropriate for a NUC or a Raspberry.

**Desktop-Full Install (Recommended for NUC) :*** ROS, rqt, rviz, robot-generic libraries, 2D/3D simulators and 2D/3D perception.

```
sudo apt install ros-noetic-desktop-full
```

**Desktop Install (Recommended for Raspberry Pi4) :** ROS, rqt, rviz, and robot-generic libraries.

```
sudo apt install ros-noetic-desktop
```

**ROS-Base (Bare Bones) (Recommended for Raspberry Pi3) :** ROS package, build, and communication libraries. No GUI tools. 

```
sudo apt install ros-noetic-ros-base
```

**Individual Package :** You can also install a specific ROS package (replace underscores with dashes of the package name): 

```
sudo apt install ros-noetic-PACKAGE_YOU_WANT_TO_ADD
```

...for example : 

```
sudo apt install ros-noetic-slam-gmapping

```

To find available packages, use this command :

```
apt search ros-noetic
```

The installation will be quite long. Several hundreds packages will be installed so it's time to have a break and enjoy a coffee ! :nerd_face:


### Environment setup

It's convenient if the ROS environment variables are automatically added to your bash session every time a new shell (terminal) is launched instead of doing ``` source /opt/ros/noetic/setup.bash ``` every time you want to run ros. So use this command :

```
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

_If you have more than one ROS distribution installed, ~/.bashrc must only source the setup.bash for the version you are currently using._

If you just want to change the environment of your current shell, instead of the above command you can type : 

```
source /opt/ros/noetic/setup.bash
```

If you use zsh instead of bash you need to run the following commands to set up your shell:

```
echo "source /opt/ros/noetic/setup.zsh" >> ~/.zshrc
source ~/.zshrc
```

/!\ If your environnemnt isn't sourced, it happens if you try to start roscore without your environment correctly setup, you can get this kind of error message : 

```
$ roscore

Command 'roscore' not found, but can be installed with:

sudo apt install python-roslaunch
```

### Initialize rosdep

Before you can use many ROS tools, you will need to initialize rosdep. rosdep enables you to easily install system dependencies for source you want to compile and is required to run some core components in ROS. If you have not yet installed rosdep, do so as follows.

```
sudo apt install python3-rosdep
```

With the following, you can initialize rosdep.
```
sudo rosdep init 
rosdep update
```

### Create a working directory

The last step is to create your working directory. This is mandatory as ROS will only work in your catkin_ws.

```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws
catkin_make
```

```catkin_make``` allows you to root of your workspace and build any package found in ```~/catkin_ws/src```, just finish by sourcing devel directory :

```
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
```


You can now launch the command ``` roscore ``` in a terminal. Good coding !!

### Tests

Now, to test your installation, please proceed to the [ROS Tutorials](http://wiki.ros.org/ROS/Tutorials).

And you can find the original tutorial for ROS noetic [here](http://wiki.ros.org/noetic/Installation/Ubuntu).
