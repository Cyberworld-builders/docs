# Ubuntu Unity Android

Setting up a development environment for Unity AR Foundation for Android on Ubuntu

## Ubuntu
I'm using Ubuntu 18 because Ubuntu 20 was giving me trouble. *__Update:__ It also appears to work fine on Ubuntu 20 as well.*

https://releases.ubuntu.com/18.04/

### Creating a persistent live USB for Ubuntu
https://www.howtogeek.com/howto/14912/create-a-persistent-bootable-ubuntu-usb-flash-drive/

Make sure you give it at least a good 10 gigs for the system. Unity will take up 6 or so, depending on what all components you select.

### Boot from the USB

*Acer Predator Helios 300 uses f2 to enter boot menu.*

Go ahead and install and sign in to some apps that we will use later to make this all much easier.
- google chrome *makes it easier to slack thing so you don't have to type as much - you can also pull up this document on github once i push it*

## Android Studio

https://linuxize.com/post/how-to-install-android-studio-on-ubuntu-18-04/

`sudo apt update`

`sudo apt install openjdk-8-jdk` *unable to locate package*

__Note:__ You can now skip all the way down to "Install Java Straight-up" *I'm almost positive this is correct.*

__To do:__ Go back with a fresh os installation and attempt to duplicate this process without any of these unnecessary steps. I think you can skip all of this but you may have to add that openjkd repo and maybe install that java-common package but I'm not 100% sure.

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

__Note:__ I'm going to try skipping this whole Oracle JDK step. **

__To Do:__ Verify that we need to do any of this Oracle JDK stuff. I'm almost certain it is not needed.

Find the version of the Oracle install script you need from https://launchpad.net/~linuxuprising/+archive/ubuntu/java/+packages. Find the one that matches your jdk version and os. `javac -version` will give you the jdk version. OS will be Bionic Beaver for this scenario.

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


#### Conjecture on Whether or not to Install Android Studio.

__TLDR:__ Android Studio and the Command Line Tools installation of the SDK are both fine ways to set up Android development for Unity. However, installation and configuration of Android Studio is quick, straight-forward, well-documented and well-supported.

Some developers in the community recommend installing just the SDK without installing Android Studio at all. While this can be done using the command line tools and the reasoning for it is valid, It's so much easier to just install Android Studio and let it guide you through the configuration of the Android SDK. In fact, I think I actually ran into some limitations where the command line tools support tends to stay notably behind support for Studio, meaning depending on when you try to set this up it may be broken and the docs you follow will mislead you. My opinion is that Google obviously favors the use of Studio and it's not really a tremendous overhead if you have a decent machine.

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

Add the universe repository for libgconf-2-4 package.

`sudo add-apt-repository universe`

Update the cache

`sudo apt update`

Now you can run the whole thing without errors.

`sudo apt install libgtk2.0-0 libsoup2.4-1 libarchive13 libpng16-16 libgconf-2-4 lib32stdc++6 libcanberra-gtk-module`
Run the installer.

`/home/ubuntu/Downloads/UnitySetup-2018.2.7f1`

The video only adds the components Unity and Documentation. I added Android Build Support as well.

Select location "Downloads" for both downloads and installation.

When I created the persistent live ubuntu usb stick, I used a 32 gig usb 3.0 and gave it 25% for system and the rest for storage. This means that our system space is not sufficient and we have to use the data partition. If your stick is large enough, go for it. I chose a smaller size usb stick for cost and speed because mine is a 3.0 and anything over 32 gigs gets pretty pricey. And I knew I would be able to access the storage space for this very purpose. I guess technically I could have done it 50/50 and it would have been enough. Mainly I'm rambling now because the unity installation is taking a long time and it's still not done.

`cd ~`
`sudo mv ~/Downloads/Unity-2018.2.7f1 /opt/Unity3D`
`sudo ln -s /opt/Unity3D/Editor/Unity /usr/bin/unity`
`unity` *Or find it in the GUI and click on it*

## Connecting Unity to Android SDK

From the Unity app...

Create a new project. (Or open and existing one.)

Give it a unique name in player settings.

### Configure Android SDK

`Edit -> Preferences`

Select the External Tools tab.

Under Android, beside SDK, click the browse button.

Mine was in `~/Android/Sdk`. Seems like it found it for me almost.

You will also need to specify the JDK as well. Mine was at `/usr/lib/jvm/java-1.8.0-openjdk-amd64/`. Don't worry about it for now because later on in the process, a future step should detect jdk and suggest it for you. If not, you can come back here and troubleshoot.

### Configure Player Settings

`Edit -> Project Settings -> Player`

Edit the company name at least, then product name if this is a real project.

Expand "Other Settings"

Under "Identification", make the Package Name reflect the company and project name. __Note:__ It must be unique.

Open up build settings.

### Configure Build Settings

`File -> Build Settings` *There's a handy shortcut to open player settings if you need to make quick edits to that.*

Under Platform, select Android.

Make sure your phone is still connected and authenticated with android studio.

Beside run device, select your device in the drop down.

Click "Build and Run"

Create a name for your build or overwrite the existing one. Probably need to follow the unique naming convention from player settings but I'm not really sure.

Now wait for it to build and run. It will connect to the Android Sdk that was installed for you when you set up Android Studio. Once it's done, wake your phone up and find the app with the Unity logo (unless you stopped to actually work on your app and updated the logo. Then it should have your app's logo. Otherwise, since this document is just for setting up development and has nothing to do with building an actual app, you should have a basic app with a unity logo that launches an empty world where you can't even pan the camera).
