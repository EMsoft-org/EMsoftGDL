# EMsoftGDL
This repository contains stand-alone IDL routines that can be executed in a GDL Docker image; GDL is an open-source package that is virtually compatible with IDL, apart from a few routines.  Below are instructions for running GDL in a Docker image on Mac OS X.  Before you build this Docker image, make sure that you allocate sufficient memory in the Docker app settings; 48Gb of Memory seems to be what's needed (on Mac M1, running OS Monterey; if you get strange compilation errors during the GDL build, then that's probably because insufficient memory is allocated within the Docker app).


### Instructions for running GDL in a Docker image with X11 support on Mac OS X

* After cloning this repository, navigate to the Docker folder and edit the **Docker-compose.yml** file; near the bottom of the file, replace the string **"/full-path-to-your-EMsoft-data-folder"** with the actual full path. Save the file.

* in the Docker folder, build the Docker image using the following command:

```
docker-compose up --build
```

* If this is the first time you are running this image, do the following steps:

1. start XQuartz from the Applications/Utilities folder; in the Preferences window, select Security and turn on "Allow connections from network clients".
2. to verify that things are properly configured, run *"netstat -an | grep -F 6000"* in an X11 terminal window; this should list a couple of ports that XQuartz is listening to  (Docker will connect on port 6000).
3. restart XQuartz
4. in a regular terminal window (using the Terminal.app in the Utilities folder), execute the following command:

```
xhost +localhost
```
* in an X11 terminal window, run the following Docker command to start the image:

```
docker-compose run -e DISPLAY=host.docker.internal:0 gdl
```

This will start the docker image; at the command prompt, type *"gdl"* to start the GDL program; to verify that things work properly, execute *"plot,findgen(100)"*; a window should show up with the corresponding plot.

To execute an IDL program (currently only the *efit* program is available), cd into the *pro* folder using *cd,'pro'* on the GDL command line, then compile the program *".r efit"*, and execute it with *"efit"*.  

