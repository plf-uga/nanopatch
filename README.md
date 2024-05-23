#Step by step to prepare Alves Lab's nano enviroment
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

# Install Librealsense2   #

git clone https://github.com/JetsonHacksNano/installLibrealsense
cd installLibrealsense
./installLibrealsense.sh

# Download Camera Codes  
git clone https://github.com/plf-uga/capture_IPCAM.git
git clone https://github.com/plf-uga/rs_capture.git
git clone https://github.com/plf-uga/upload_py.git

## Secret client tokens for upload_py can be found at alveslabnanos@gmail.com
## Copy and paste all the files in the folder upload_py

# Give Permissions for Programs to Run
cd capture_rs
chmode +x run.sh
cd ..
cd upload_py
chmod +x upload.py
chmod +x down_token.sh

# Compile the rs_capture.cpp program 
## Go to the rs_capture folder (cd rs_capture)
g++ -std=c++11 rs_capture.cpp -lrealsense2 -o rs-shot.exe

# Setting up remote access
## Create a gmail account for the nano
## Link the nano with Dataplicity
## Enter the email address in the following URL: https://www.dataplicity.com/
## Follow the website instructions



