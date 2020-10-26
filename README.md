# Instructions

### Github versions of repo
- (Oct 26 2020 - this version) https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/tree/017ab505f0724a0e385e3a1298d48bf92406f13b
- (Oct 26 2020 - this version) https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/blob/017ab505f0724a0e385e3a1298d48bf92406f13b/docs/linux.md
- (April 5 2018 - old version referenced by articles) https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/tree/f7bea929f15dba649858882ea746a9f0739eebab

### Kinesis Video Streams & Rekognition (must be in us-west-2 region)
- https://aws.amazon.com/blogs/machine-learning/easily-perform-facial-analysis-on-live-feeds-by-creating-a-serverless-video-analytics-environment-with-amazon-rekognition-video-and-amazon-kinesis-video-streams/

### Setup for this version
###### Install packages
```
sudo apt-get install cmake m4 git build-essential
sudo apt-get install libssl-dev libcurl4-openssl-dev liblog4cplus-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-base-apps gstreamer1.0-plugins-bad gstreamer1.0-plugins-good gstreamer1.0-plugins-ugly gstreamer1.0-tools
```

###### Clone repo and build
```
git clone --recursive https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp.git
git reset --hard 017ab505f0724a0e385e3a1298d48bf92406f13b
mkdir -p amazon-kinesis-video-streams-producer-sdk-cpp/build
cd amazon-kinesis-video-streams-producer-sdk-cpp/build
cmake .. -DBUILD_GSTREAMER_PLUGIN=ON
cd ..
make
```

###### Edit ~/.bashrc
```
cd ~/kinesisProducer/amazon-kinesis-video-streams-producer-sdk-cpp
export GST_PLUGIN_PATH=`pwd`/build
export LD_LIBRARY_PATH=`pwd`/open-source/local/lib
export AWS_ACCESS_KEY_ID=<your access key>
export AWS_SECRET_ACCESS_KEY=<your secret key>
cd ~
```

###### Run streaming demo
```
gst-launch-1.0 -v v4l2src device=/dev/video0 ! videoconvert ! video/x-raw,format=I420,width=640,height=480,framerate=30/1 ! x264enc  bframes=0 key-int-max=45 bitrate=500 tune=zerolatency ! video/x-h264,stream-format=avc,alignment=au ! kvssink stream-name=<your stream name> storage-size=128
```

### Setup for the old version
###### Install packages
```
sudo apt-get install git
sudo apt-get install cmake
sudo apt-get install libtool
sudo apt-get install libtool-bin
sudo apt-get install automake
sudo apt-get install g++
sudo apt-get install curl
sudo apt-get install pkg-config
sudo apt-get install flex
sudo apt-get install bison
sudo apt-get install openjdk-8-jdk
```

###### Edit ~/.bashrc
```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/
export LD_LIBRARY_PATH=/home/pc/amazon-kinesis-video-streams-producer-sdk-cpp/kinesis-video-native-build/downloads/local/lib
export AWS_ACCESS_KEY_ID=<your access key>
export AWS_SECRET_ACCESS_KEY=<your secret key>
```

###### Clone repo and build
```
git clone https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp.git
git reset --hard f7bea929f15dba649858882ea746a9f0739eebab
cd amazon-kinesis-video-streams-producer-sdk-cpp/kinesis-video-native-build
chmod +x install-script
./install-script
```

###### Copy Amazon Trust Certificate (https://www.amazontrust.com/repository/SFSRootCAG2.pem) into /etc/ssl/cert.pem
```
sudo cp ~/cert.pem /etc/ssl/cert.pem
```

###### Run streaming demo (inside of kinesis-video-native-build directory)
```
./kinesis_video_gstreamer_sample_app <your stream name>
```
