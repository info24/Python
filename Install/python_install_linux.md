Linux上python的源码安装：
（多版本安装指定安装目录：
./configure --prefix=/usr/local/python3
 make
 make install
 mv /usr/bin/python /usr/bin/python_old   # 移除老版本
 ln -s /usr/local/python3/bin/python3 /usr/bin/python # 建立软连接
 ）
[简介：如果直接./configure make make install, 加密码模块不能被正确安装，而此模块依赖于openssl，故系统必须得有相应版本的openssl。]

1.open ssl的安装：
varssl=`which openssl`
	if [ "$varssl" != "/usr/local/bin/openssl" ]
	then
		nohup ln -s  /usr/bin/openssl /usr/local/bin/openssl >> $THELOGFILE
	fi
	#ldd 查看程序依赖库— Linux Tools Quick Tutorial - Read the Docs
	#iotop -b -n 10 > iotop.txt
	libssl=`find /usr/local/lib64/ -name "libssl.so"|head -1`
	if [ "$libssl" == "" ]
	then	
		PRINT_LOG "INFO" "no libssl.so in /usr/local/lib64/ ,creating symbolic link /usr/local/lib64/libssl.so"
		ln -s  /usr/lib64/libssl.so /usr/local/lib64/libssl.so >> $THELOGFILE
	else
		PRINT_LOG "INFO" "libssl.so exsit in /usr/local/lib64/,creating symbolic link /usr/lib64/libssl.so"
		ln -s  /usr/local/lib64/libssl.so /usr/lib64/libssl.so >> $THELOGFILE
	fi
	libcrypto=`find /usr/local/lib64/ -name "libcrypto.so"|head -1`
	if [ "$libcrypto" == "" ]
	then
		PRINT_LOG "INFO" "no libcrypto.so in /usr/local/lib64/"
		ln -s  /usr/lib64/libcrypto.so /usr/local/lib64/libcrypto.so >> $THELOGFILE
	else
		PRINT_LOG "INFO" "libcrypto.so exsit in /usr/local/lib64/"
		ln -s  /usr/local/lib64/libcrypto.so /usr/lib64/libcrypto.so >> $THELOGFILE
	fi
