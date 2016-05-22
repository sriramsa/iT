# Designing iT 
How all this can come together

## Image
iT Image will be a standard RPi distribution + media content + iT packages and customizations. This will be burnt to a Micro SD card and will distributed with the kit. 

### Image Content
The image will contain the following:
1. Educational Content
..1. Audio/Video Content
..2. Source Code Exercises
2. iT Packages
..1. Admin UI package
..2. Media downloader
..3. iT Exercises
..4. iT SSH
..5. iT Connect
..6. Azure Linux CLI
3. Customizations
..1. Azure Repo added to pull down packages
..2. iT admin user

### Servicing the image
Once the image is released, how do we service the content and packages in the kit. 
#### Educational Content
The content will be stored in a directory structure. We can have a daemon that can periodically check for updates, if any updates to Audio/Video or Source code is available, we can download them to the RPi with user consent.
(or)
We can provide a button in Admin UI where the student can "Check for updates". This may be a preferable option.

#### iT Packages and RPi packages
We can auto-update packages automatically when connected. Or instruct the student to do so periodically.

## Components
* Admin UI
* Tablet Connect
* Azure Connect
* Media
* iT Code 
* iT Terminal

### Admin UI:
There are a few web based control panels for Linux; webmin etc., But they are too feature rich to use without modifications. It might be easier to just build some small UI for adding user, reset tablet connection etc.,
    - Package: iT-admin.0.1.0.deb
    - depends: apache

### Tablet Connect:
Pi acts as an AP to let the tablet connect to it. 
**HostAPD** can be used to make RPi be an AP. Not all wifi adapters would work, though. The wifi usb adapter I have on my Pi doesn't have the necessary support. I'm going to buy a couple of wifi usb adapters and see if one of them will work. 
    - Package: iT-ap-connect.0.1.0.deb
    - depends: hostapd, openssl

### iT Media
Media files are stored in a pre-known directory structure exposed over a web server that a standard Web UI can load and present. A package that creates the directory structure and scripts to download/update the existing media will be built.
    - For first iteration/demo we can have something small browsing files on Pi over the tablet. 
        - Later this can be a Win10 app or a web app.
    - Package: iT-media.0.1.0.deb

### iT Code
Code is similar to media files and is stored in the workspace directory for Codiad.
    - Code editing on an Web IDE and saves on the Pi. 
    - For IDE to edit code, this looks promising: http://codiad.com/
    - Run the code in the terminal. 
    - We can have pre-configured projects that students can modify and run.
    - Also have online projects that they can download
    - Package: iT-code.0.1.0.deb

### iT Terminal
How do we connect from the tablet to the terminal. We can use putty or a browser based terminal. If the tablet isn't based on x86, we may not be able to use Putty. There are a few browser based terminals that are easy to setup. 
**shellinabox**, is more stable and has deb package for Pi. Pretty straight forward to use. 
    - Package: iT-terminal.0.1.0.deb

### Connecting to Azure
Azure Linux cli will be installed on the box.
