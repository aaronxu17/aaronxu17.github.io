---
layout: post
title: "Install and Configure MySQL Connector/C++ on Ubuntu 16.04"
categories: blog
tags: [tech]
date: 2017-03-29T19:19:55 EST
---



When I started to install MySQL Connector/C++ on Ubuntu 16.04, I first followed the tutorial on MySQL official [Developer Guide](https://dev.mysql.com/doc/connector-cpp/en/ "MySQL Connector/C++ Developer Guide"). While it did not work well. So, I made some changes when I followed the tutorial for installation and configurations. Hope my personal experience can provide a bit help when you face the same situation. My major steps are list below: 

1: Make sure your system has satisfied the installation prerequisites;

2: Install MySQL Connector/C++. In this step, I download one copy [mysql-connector-c++-1.1.8-linux-ubuntu16.04-x86-64bit.tar.gz](https://dev.mysql.com/downloads/file/?id=467847) (since I need its two folders `/include` and `/lib`, where `/include` has `/cppconn`, and `/lib` includes `libmysqlcppconn.so`,`libmysqlcppconn-static.a`, etc.) from <https://dev.mysql.com/downloads/connector/cpp/> and merge it with the copy which is from github:

```bash
	git clone https://github.com/mysql/mysql-connector-cpp
```

3: I merge `/include` and `/lib` to `/mysql-connector-cpp` since only in `mysql-connector-cpp` I have cmakefile.

4: Install Boost C++ libraries, Boost 1.56.0 or higher version. I installed Boost 1.58.0.

```bash
	sudo apt-get install libboost-all-dev
```

5: Change location to the top-level directory of the source distribution: 

```bash
	cd /path/to/mysql-connector-cpp
```

6: Tell the build system where the Boost files are located (in my case, I find it in `/usr/include/boost`) by defining the BOOST_ROOT option when you invoke CMake:

```bash
	locate -i /boost/version.hpp
    
	cmake . -DBOOST_ROOT=/path/to/boost directory
```

7: Run CMake to build a Makefile

```bash
	cmake . 
```

8: Use make to build Connector/C++. First make sure you have a clean build, then build the connector:

```bash
	make clean
    
	make
``` 
   
9: Make sure you can see the output message "Linking CXX executable statement" or "Linking CXX executable try"

10: Install the header and library files:

```bash
	make install (may use sudo)
```

11: Building Connector/C++ Linux Applications with NetBeans, Select **`File`**, **`Project Properties`** from the main menu. In the **`Categories`**: tree view panel, navigate to **`Build`**, **`C++ Compiler`**. In the **`General panel`**, select **`Include Directories`**. Add

> `/usr/include/boost`; `/usr/local/mysql/connector-c++2.0/include`; `/path/to/mysql-connector-c++-1.1.8/driver`
   
   **NOTE**: It seems that the above directorie can be not included, still can build successfully.

12: In the same panel, navigate to **`Linker`**->**`Additional Library Directories`**. Add

> `/usr/local/lib`

13: In the same panel, navigate to **`Libraries`**. I choose the Dynamic Library, so that `libmysqlcppconn.so` is chosen for

>  **Add Library** (NOT Add Library File): (dynamic) `/usr/local/lib/libmysqlcppconn.so`, display`"mysqlcppconn"`
   
   **Note**: Make sure the libmysqlcppconn.so and libmysqlcppconn.so.7 are included in the added library directories. In my case, I copy these two files from `/mysql-connector-cpp/lib/` directory.