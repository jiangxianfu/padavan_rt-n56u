###
How To Independently Build Firmware For The Routers ASUS RT-N14U/N56U/N65U/N11P/AC51U/AC54U/AC1200HP/N56UB1

---
Instructions for independently build firmware for your router from source in Ubuntu 16.04 LTS i386.

Install to your PC the Oracle VirtualBox.

Download Ubuntu 16.04 LTS i386 (for example, from this mirror).

Create new virtual machine and install Ubuntu from step 2 to it. VirtualBox may use a ISO-image as drive in virtual machine.

warning.png If you plan to use Ubuntu with the GUI, you must dedicate at least 1.5Gb of RAM for the virtual machine. When you select a smaller size of RAM you may receive errors when you create the image of the firmware.

Start your virtual machine and open Terminal on it.

Install git:

sudo apt-get update 
sudo apt-get install git
Go to directory /opt and run command for create the local copy of repository:

cd /opt
sudo git clone https://bitbucket.org/padavan/rt-n56u.git
This copies all the source code, creates a local git-repository. Directory /opt/rt-n56u will be the root of the git-repository.

Read the document /opt/rt-n56u/readme.eng.txt and install all the required packages that are listed in it:

sudo apt-get update
sudo apt-get install autoconf automake autopoint bison build-essential flex gawk gettext git gperf libtool pkg-config zlib1g-dev libgmp3-dev libmpc-dev libmpfr-dev texinfo python-docutils
mc (midnight commander) do not need to build the firmware, but it will help you navigate through directories, copy or edit files.

Go to directory with toolchain sources (cross-compiler and tools for building) and build it:

cd /opt/rt-n56u/toolchain-mipsel
sudo ./clean_sources
sudo ./build_toolchain
The result will be collected the target of toolchain /opt/rt-n56u/toolchain-mipsel/toolchain-3.4.x

If you plan to build the firmware with the kernel 3.0, you must build the appropriate version of tolchain:

cd /opt/rt-n56u/toolchain-mipsel
sudo ./clean_sources
sudo ./build_toolchain_3.0.x
The result will be collected the target of toolchain /opt/rt-n56u/toolchain-mipsel/toolchain-3.0.x

In the future, you will need these commands only if the toolchain will be updated.

Now go to directory with sources:

cd /opt/rt-n56u/trunk
and edit file /opt/rt-n56u/trunk/.config to fit your needs.

Edit path to toolchain (if you need it):

CONFIG_TOOLCHAIN_DIR=/opt/rt-n56u/toolchain-mipsel
To build the firmware, for example, for router RT-N65U uncomment (remove the simbol #) the line:

CONFIG_FIRMWARE_PRODUCT_ID="RT-N65U"
and comment the line:

#CONFIG_FIRMWARE_PRODUCT_ID="RT-N56U"
Save the file after edit.

Clear source tree (every time before a new build)

sudo ./clear_tree
Build the firmware:

sudo ./build_firmware
Created custom firmware file will be in the directory ./path_to_your_dir/rt-n56u/trunk/images. If you want to save the firmware that you created earlier - copy it to another location, because the command clear_tree overwrites the directory images.

When repository updated a local source tree must be updated with command:

sudo git pull
If you made ​​any changes to the local repository, when you upgrade tree some files could not be copied. In this case, you must give the command:

sudo git stash
sudo git pull
If toolchain sources (cross-compiler and tools for building) is changed you must re-build it:

cd /opt/rt-n56u/toolchain-mipsel
sudo ./clean_sources  
sudo ./clean_toolchain  
sudo ./build_toolchain