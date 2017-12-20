# MAVLink Connect UAV AutoTakeOff

# 預先安裝軟體

## 1.__系統更新__
	$ sudo apt-get update
	$ sudo apt-get upgrade

***

## 2.__安裝 Screen__
	$ sudo apt-get install screen
	
## 3.__安裝 OpenCV__
	$ sudo apt-get install build-essential

	$ sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev

	$ sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev

	$ sudo apt-get -y install libopencv-dev build-essential cmake git libgtk2.0-dev pkg-config python-dev python-numpy libdc1394-22 libdc1394-22-dev libjpeg-dev libpng12-dev libtiff4-dev libjasper-dev libavcodec-dev libavformat-dev libswscale-dev libxine-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libv4l-dev libtbb-dev libqt4-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev x264 v4l-utils unzip

	前往 opencv官網下載

	Information: http://opencv.org/

	下載 opencv.2.4.13

	$ https://codeload.github.com/opencv/opencv/zip/2.4.13

	解壓縮(檔案路徑)

	$ cd opencv-2.4.13(檔案路徑)
	$ mkdir build
	$ cd build
	$ cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D WITH_V4L=ON -D WITH_QT=ON -D WITH_OPENGL=ON ..
	/* 
		如果上面 cmake 指令建立時發生錯誤，請改用下列 cmake 指令
		cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..
	*/
	$ make -j $(nproc)
	$ sudo make install
	$ sudo /bin/bash -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf'
	$ sudo ldconfig
	/* 
		驗證 opencv 是否安裝成功
		pkg-config opencv --cflags
			=> -I/usr/local/include/opencv -I/usr/local/include  
	 	pkg-config opencv --libs
	 		=> -L/usr/local/lib -lopencv_calib3d -lopencv_contrib -lopencv_core -lopencv_features2d -lopencv_flann -lopencv_gpu -lopencv_highgui -lopencv_imgproc -lopencv_legacy -lopencv_ml -lopencv_nonfree -lopencv_objdetect -lopencv_ocl -lopencv_photo -lopencv_stitching -lopencv_superres -lopencv_ts -lopencv_video -lopencv_videostab -ltbb -lXext -lX11 -lICE -lSM -lGL -lGLU -lrt -lpthread -lm -ldl 
	*/

## 4.__設定自動登入__
	$ sudo echo "autologin-user = odroid" >> /usr/share/lightdm/lightdm.conf.d/60-lightdm-gtk-greeter.conf


	這支成程式碼主要是用在已連結pixhawk飛控版的無人機,可透過網際網路傳遞任務包及飛行,降落,旋停等無人機指令
		
		__伺服器設定__
			1.63 RS_DEVICE_APM "/dev/ttyUSB0"  //設定Comport
			2.64 BAUDRATE_APM  57600           //設定port 
			3.65 NET_HOST	  "27.105.106.52" //伺服器 IP 

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

		__接收資料格式__

			sprintf(buff,"16111002,254,%s,1,%d,%d,%d,%.6f,%.6f,%.4f,%.4f,%d,%d,%d", mav_Mode(custom_mode), (HeartBeat>10?1:0), sys_status.battery_remaining, mission_current.seq, lon, lat, alt, (double)gps_raw_int.vel*100, fix_type, gps_raw_int.satellites_visible, latitude);
		    //ID, Air_ID, Mode, flight_C, Sys_Status, battery, current_WP, lon, lat, alt, speed, gps_fix, gps_sv

		__如何使用__

			1.前往 APM_MAVLink.c's 檔案路徑下執行make指令
			2.開啟終端機,前往APM_MAVLink.c's檔案路徑下執行
				$ sudo ./a.out
			3.開啟另一個終端機視窗並且等待五秒鐘後執行 
				$ sudo screen /dev/ttyUSB0 57600

