# EMsoftGDL
This repository contains stand-alone IDL routines that can be executed in a GDL Docker image; GDL is an open-source package that is virtually compatible with IDL, apart from a few routines.  Below are instructions for obtaining the GDL Docker image, and for building the image on Mac OS X.  



### Instructions for obtaining the GDL Docker image 

* If you just want to run IDL programs in the GDL environment, then you do not need to build the docker image at all; you can just pull it from the Docker archive.

* If you haven't already done so, download and install the [Docker app](https://www.docker.com/products/docker-desktop/); start the app and set the memory requirement as mentioned above.

* Pull the Docker image as follows:

```
docker pull marcdegraef/gdl
```


### Instructions for building GDL in a Docker image with X11 support on Mac OS X

* If you haven't already done so, download and install the [Docker app](https://www.docker.com/products/docker-desktop/). Before you build this Docker image, make sure that you allocate sufficient memory in the Docker app settings; 48Gb of Memory seems to be what's needed (on Mac M1, running OS Monterey; if you get strange compilation errors during the GDL build, then that's probably because insufficient memory was allocated within the Docker app).

* After cloning this repository, navigate to the Docker folder and build the Docker image using the following command (depending on your hardware, this can take several minutes):

```
docker-compose up --build gdl
```

* If this is the first time you are running this image, do the following steps:

1. start XQuartz from the Applications/Utilities folder (install it first if you don't have it, from [XQuartz-2.8.1](https://github.com/XQuartz/XQuartz/releases/download/XQuartz-2.8.1/XQuartz-2.8.1.dmg)); in the Preferences window, select Security and turn on "Allow connections from network clients".
2. to verify that things are properly configured, run *"netstat -an | grep -F 6000"* in an X11 terminal window; this should list a couple of ports that XQuartz is listening to  (Docker will connect on port 6000).
3. restart XQuartz.
4. in a regular terminal window (using the Terminal.app in the Utilities folder), execute the following command:

```
xhost +localhost
```

* at the top level of this respository, create a symbolic link to the folder that contains your EMsoft data (master patterns, EBSD patterns, etc.); to make those files appear within the Docker image, the linked folder **must** have the name *"EMsoftData"*:

```
ln -s /full-path-to-your-EMsoft-data-folder EMsoftData
```

* in an X11 terminal window, navigate to the Docker folder in the repository and run the following Docker command to start the image:

```
docker-compose run -e DISPLAY=host.docker.internal:0 gdl
```

This will start the docker image; you will be logged in as *gdluser*, and there will be three folders:

1. Resources: this contains a few files used by the IDL scripts;
2. data: this is the folder that is linked to your EMsoft data folder;
3. pro: this contains all the IDL .pro routines.

At the command prompt, type *"gdl"* to start the GDL program; to verify that things work properly, execute *"plot,findgen(100)"*; a window should show up with the corresponding plot.

To execute an IDL program (currently only the *efit* program is available), cd into the *pro* folder using *cd,'pro'* on the GDL command line, then compile the program *".r efit"*, and execute it with *"efit"*.  There will be a number of Gtk-CRITICAL warnings; we are looking to how to fix these.  They do not appear to affect the actual execution of the program.

