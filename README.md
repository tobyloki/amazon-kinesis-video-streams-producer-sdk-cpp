# Instructions

- https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/
- https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/blob/master/docs/linux.md
- (old version) https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/tree/f7bea929f15dba649858882ea746a9f0739eebab

`
1. Install packages
sudo apt-get install cmake m4 git build-essential
sudo apt-get install libssl-dev libcurl4-openssl-dev liblog4cplus-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-base-apps gstreamer1.0-plugins-bad gstreamer1.0-plugins-good gstreamer1.0-plugins-ugly gstreamer1.0-tools\

2. Clone repo and build
git clone --recursive https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp.git
mkdir -p amazon-kinesis-video-streams-producer-sdk-cpp/build
cd amazon-kinesis-video-streams-producer-sdk-cpp/build
cmake .. -DBUILD_GSTREAMER_PLUGIN=ON
cd ..
make

3. Edit ~/.bashrc
cd ~/kinesisProducer/amazon-kinesis-video-streams-producer-sdk-cpp
export GST_PLUGIN_PATH=`pwd`/build
export LD_LIBRARY_PATH=`pwd`/open-source/local/lib
export AWS_ACCESS_KEY_ID=<your access key>
export AWS_SECRET_ACCESS_KEY=<your secret key>

4. Run streaming demo
gst-launch-1.0 -v v4l2src device=/dev/video0 ! videoconvert ! video/x-raw,format=I420,width=640,height=480,framerate=30/1 ! x264enc  bframes=0 key-int-max=45 bitrate=500 tune=zerolatency ! video/x-h264,stream-format=avc,alignment=au ! kvssink stream-name=<your stream name> storage-size=128
`
