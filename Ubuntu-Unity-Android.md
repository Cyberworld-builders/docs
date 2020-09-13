# Ubuntu Unity Android

Setting up a development environment for Unity AR Foundation for Android on Ubuntu

## Ubuntu
I'm using Ubuntu 18 because Ubuntu 20 was giving me trouble.
https://releases.ubuntu.com/18.04/

### Creating a persistent live USB for Ubuntu
https://www.howtogeek.com/howto/14912/create-a-persistent-bootable-ubuntu-usb-flash-drive/


Make sure you partition this thing correctly. I got it backwards and gave it most of the disc for system files and not enough for storage. I think the left side is storage and the right side is system. Either that or the left side is for system and storage and the rest of it is left over. Or some other third thing. Like I could still access it anyway I just don't know what it's called.

__Note:__ It may be a good idea to go back and revisit this. I'm pretty sure I could have just used the partition "usbdata". I may have done it correctly in the beginning and then gotten confused during the Unity install.

### Boot from the USB

*Acer Predator Helios 300 uses f2 to enter boot menu.*

Go ahead and install and sign in to some apps that we will use later to make this all much easier.
- google chrome *makes it easier to slack thing so you don't have to type as much - you can also pull up this document on github once i push it*

## Android Studio

https://linuxize.com/post/how-to-install-android-studio-on-ubuntu-18-04/

`sudo apt update`

`sudo apt install openjdk-8-jdk` *unable to locate package*

https://stackoverflow.com/questions/32942023/ubuntu-openjdk-8-unable-to-locate-package

`sudo add-apt-repository ppa:openjdk-r/ppa`

`sudo apt-get update`

`sudo apt-get install openjdk-8-jdk` *still unable to locate package*

`update-java-alternatives -l` *not found, but can be installed with...*

`sudo apt install java-common`

### Try installing Java straight up.
https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04

`sudo apt update`

`java -version` *not installed. great, let's do it*

#### Java:
`sudo apt install default-jre -y`

`java -version` *we're now running java 11. may need to replace with 8, but let's try this first*

#### Java Development Kit:

`sudo apt install default-jdk`

I think we could technically stop there for what we're doing but I'm going to follow through with the whole blog so this doesn't get sloppy.

#### Oracle JDK
Find the versio of the Oracle install script you need from https://launchpad.net/~linuxuprising/+archive/ubuntu/java/+packages. Find the one that matches your jdk version and os. `javac -version` will give you the jdk version. OS will be Bionic Beaver for this scenario.

Go to the downloads page https://www.oracle.com/java/technologies/javase-downloads.html and find and download the right script. Download the file. You'll need to authenticate using an Oracle account to download it. If you don't have one create one.

`sudo apt install software-properties-common -y`

`sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EA8CACC073C3DB2A`

`sudo add-apt-repository ppa:linuxuprising/java` *press enter*

`sudo mkdir -p /var/cache/oracle-jdk11-installer-local/`

`sudo cp ~/Downloads/jdk-11.0.7_linux-x64_bin.tar.gz /var/cache/oracle-jdk11-installer-local/`

*the file i downloaded was actually a .deb file so you can just run it as an installer through the software app instead of doing all this. i think this is meant for being done on a server*

`sudo nano /etc/environment`

add this line: `JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"`

`source /etc/environment`

`echo $JAVA_HOME`

### Meanwhile... back in the Android Studio install

`sudo snap install android-studio --classic`

Now open up android studio and follow the wizard. I chose everything standard.

#### Debugging to a Physical Device

Previously, I've had issues with debugging to a device on some workstations. It had something to do with the ADB bridge connection.

Here's some basic things I did once I got Android Studio installed and opened.
- create a basic project
- click on build to build the project. don't even make any changes.
- connect the phone with a usb *debugging was already enabled on my phone from previous attempts this will need to be enabled on newer android phones if it's not already*
- click troubleshoot connections
- click re-scan devices
- make sure you're watching your phone because it will ask for permission to debug from the development machine
- you may have to run it again after that.
- at this point it should have opened up your app on the phone.

Now you know Android Studio works and can be used to send builds to a physical device. After all that, you're barely going to use all this for any real development. Most of our work will be done in Unity and then configured to launch Android Studio just to send builds to the phone. It is crucial, however.   

## Unity
Now let's Unity the fuck out of this hooker.

Going to try to follow this video:
https://www.youtube.com/watch?v=yauNR4uFWko

Go here:
https://beta.unity3d.com/download/dad990bf2728/public_download.html

The first link, "Linux Download Assistant", will trigger a download of a script we need.

Give the script execute permission. `chmod +x /home/ubuntu/Downloads/UnitySetup-2018.2.7f1`

Install some packages `sudo apt install libgtk2.0-0 libsoup2.4-1 libarchive13 libpng16-16 libgconf-2-4 lib32stdc++6 libcanberra-gtk-module` *unable to locate package libgconf-2-4*

Upon further inspection of the libgconf-2-4 package on cannonical, it's only available from the "universe" repository. So before you run that command, add universe and update the cache.

`sudo add-apt-repository universe`

`sudo apt update`

Now you can run the whole thing without errors.

`sudo apt install libgtk2.0-0 libsoup2.4-1 libarchive13 libpng16-16 libgconf-2-4 lib32stdc++6 libcanberra-gtk-module`
Run the installer.

`/home/ubuntu/Downloads/UnitySetup-2018.2.7f1`

The video only adds the components Unity and Documentation. I added Android Build Support as well.

Select location "~/Downloads"
