# Step by step to prepare Alves Lab's nano enviroment
# Update apt-get and linux utilities   
sudo apt-get update  
sudo apt-get upgrade  
sudo apt-get install curl  

# Install pip in Python3  
sudo apt-get install python3-pip  
sudo python3 -m pip install --upgrade pip  

# Install python libraries 
python3 -m pip install PyDrive2  
python3 -m pip install opencv-python  
python3 -m pip install numpy  

# Install Librealsense2  

git clone https://github.com/JetsonHacksNano/installLibrealsense  
cd installLibrealsense  
./installLibrealsense.sh  

# Download Camera Codes    
git clone https://github.com/plf-uga/capture_IPCAM.git  
git clone https://github.com/plf-uga/rs_capture.git  
git clone https://github.com/plf-uga/upload_py.git
git clone https://github.com/plf-uga/get_thermal_data.git

 **Secret client tokens for upload_py can be found at alveslabnanos@gmail.com**   
 Copy and paste all the files in the folder upload_py  

# Give Permissions for Programs to Run  
cd capture_rs  
chmod +x run.sh  
cd ..  
cd upload_py  
chmod +x upload.py  
chmod +x down_token.sh  

# Compile the rs_capture.cpp program   
Go to the rs_capture folder (cd rs_capture)  
g++ -std=c++11 rs_capture.cpp -lrealsense2 -o rs-shot.exe  

# Setting up remote access  
Create a gmail account for the nano  
Link the nano with Dataplicity  
Enter the email address in the following URL: https://www.dataplicity.com/  
Follow the website instructions

# Examples on how to shchedulle tasks on Linux environment   
crontab -e #Opens the crontab file  
SHELL=/bin/bash  
PATH = /bin:/sbin:/usr/bin:/usr/sbin  
45 02 * * * /home/alveslab/capture_cam1/run.sh   
46 02 * * * /home/alveslab/capture_cam2/run.sh  
45 13 * * * /home/alveslab/capture_cam1/run.sh  
46 13 * * * /home/alveslab/capture_cam2/run.sh  
45 23 * * * /home/alveslab/upload_py/down_token.sh  
05 00 * * * /usr/bin/python3.6 /home/alveslab/upload_py/upload.py >> /home/alveslab/upload.log 2>&1

# For Sub versions Only ***

Due to a shortage of Jetson Nanos, we are currently using sub-versions that already come with the OS pre-installed (with only 16 GB of hard memory, 13GB dedicated for the OS). 
The following pre-steps will be necessary.

## 1. Add new user
  sudo adduser <username>  
  usermod -aG sudo <username>

## 2. Mount an SD card permanently
   Run the following commands:

   2.1 Install nano command:
     sudo apt-get install nano  
   2.2 Identify the SD Card Device:  
     lsblk  
   2.3  Create a directory where you will mount the SD card  
     sudo mkdir -p /mnt/external  
   2.4  Edit /etc/fstab to Mount on Boot  
     sudo nano /etc/fstab  
   2.5  Edit /etc/fstab to Mount on Boot   
    assuming ext4 format: /dev/<name_of_card_on_system>    /mnt/external    ext4    defaults,exec    0    0  
    assuming FAT32 format: /dev/<name_of_card_on_system>    /mnt/external    vfat    defaults,exec,uid=1000,gid=1000    0    0  
    assuming exFAT format: /dev/sdb1    /mnt/<name_of_card_on_system   exfat    defaults,exec,uid=1000,gid=1000    0    0  

  You can check the name of the card using the command: lsblk -f  
  generally is something like mmcblk0p1 or sdb1  

  You will also need the card ID to edit the /etc/fstab. Run the following:   
  sudo blkid  

  2.6. After editing the /etc/fstab file, run the following:   
  sudo mount -a
  2.7 Give executable permission if needed:   
  sudo chmod -R +x /mnt/external  

  2.7. Confirm if the drive was mounted correctly:   
    mount | grep external  
    or  
    findmnt /mnt/sdcard  











