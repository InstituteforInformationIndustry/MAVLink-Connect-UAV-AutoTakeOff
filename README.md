MAVLink Connect UAV AutoTakeOff

Pre-Install Software

1.	System Update
	$ sudo apt-get update
	$ sudo apt-get upgrade

2.	Install Screen
	sudo apt-get install screen
	
3.	Install OpenCV
	sudo apt-get install build-essential
	sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
	sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
	sudo apt-get -y install libopencv-dev build-essential cmake git libgtk2.0-dev pkg-config python-dev python-numpy libdc1394-22 		libdc1394-22-dev libjpeg-dev libpng12-dev libtiff4-dev libjasper-dev libavcodec-dev libavformat-dev libswscale-dev libxine-dev 		libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libv4l-dev libtbb-dev libqt4-dev libfaac-dev libmp3lame-dev libopencore-		amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev x264 v4l-utils unzip
	Goto opencv Homepage
	http://opencv.org/
	Downloading opencv.2.4.13
	https://codeload.github.com/opencv/opencv/zip/2.4.13
	unZip(filePath)
	cd opencv-2.4.13(filePath)
	mkdir build
	cd build
	cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D WITH_V4L=ON -D WITH_QT=ON -D 			WITH_OPENGL=ON ..
	/* 
		if cmake error，Please use this command
		cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..
	*/
	make -j $(nproc)
	sudo make install
	sudo /bin/bash -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf'
	sudo ldconfig
	/* 
		Verification opencv Install status
		pkg-config opencv --cflags
			=> -I/usr/local/include/opencv -I/usr/local/include  
	 	pkg-config opencv --libs
	 		=> -L/usr/local/lib -lopencv_calib3d -lopencv_contrib -lopencv_core -lopencv_features2d -lopencv_flann -					lopencv_gpu -lopencv_highgui -lopencv_imgproc -lopencv_legacy -lopencv_ml -lopencv_nonfree -						lopencv_objdetect -lopencv_ocl -lopencv_photo -lopencv_stitching -lopencv_superres -lopencv_ts -					lopencv_video -lopencv_videostab -ltbb -lXext -lX11 -lICE -lSM -lGL -lGLU -lrt -lpthread -lm -ldl 
	*/

4.	Set AutoLogin
	sudo echo "autologin-user = odroid" >> /usr/share/lightdm/lightdm.conf.d/60-lightdm-gtk-greeter.conf


This code is mainly to allow flight control version support PIX4 take off automatically through raspberry pi3 and perform pre-set tasks
	Server Setting
	1.63 RS_DEVICE_APM "/dev/ttyUSB0"  //Setting COMPort
	2.64 BAUDRATE_APM  57600           //Setting 
	3.65 NET_HOST	  "27.105.106.52" //Server IP 

	Mav Mode

			0:"STABILIZE"

			1:"Acro"

			2:"AltHold"

			3:"AUTO"
			
			4:"Guided"

			5:"Loiter"

			6:"RTL"

			7:"Circle"

			9:"Land"

			10"OF_Loiter"

			11:"Drift"

			13 "Sport"

	Receive DATA

		sprintf(buff,"16111002,254,%s,1,%d,%d,%d,%.6f,%.6f,%.4f,%.4f,%d,%d,%d", mav_Mode(custom_mode), (HeartBeat>10?1:0), 			sys_status.battery_remaining, mission_current.seq, lon, lat, alt, (double)gps_raw_int.vel*100, fix_type, 				gps_raw_int.satellites_visible, latitude);
		//ID, Air_ID, Mode, flight_C, Sys_Status, battery, current_WP, lon, lat, alt, speed, gps_fix, gps_sv

	How to use 

		1.Goto APM_MAVLink.c's Path to make
		2.Open a terminal，and go to APM_MAVLink.c's Path
		 $ sudo ./a.out
		3.Open another terminal and waiting 5 second 
		 $ sudo screen /dev/ttyUSB0 57600

