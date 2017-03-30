---
layout: post
title: "Install and Configure MySQL Connector/C++ on Ubuntu 16.04"
categories: blog
tags: [tech]
date: 2017-03-29T19:19:55 EST
---



When I started to install MySQL Connector/C++ on Ubuntu 16.04, I first followed the tutorial on MySQL official [Developer Guide](https://dev.mysql.com/doc/connector-cpp/en/ "MySQL Connector/C++ Developer Guide"). While it did not work well. So, I made some changes when I followed the tutorial for installation and configurations. Hope my personal experience can provide a bit help when you face the same situation. My major steps are list below: 

1: Make sure your system has satisfied the installation prerequisites;

2: Install MySQL Connector/C++. In this step, I found out that only the source code includes cakelist.txt and cppconn folder. I also download the MySQL Connector/C++ from github:

```bash
	git clone https://github.com/mysql/mysql-connector-cpp
```

3: Then I unpack the compressed file and copy&past cppconn to the folder where I stored the MySQL Connector/C++ downloaded from github.

4: Install Boost C++ libraries, Boost 1.56.0 or higher version. I installed Boost 1.58.0.

```bash
	sudo apt-get install libboost-all-dev
```

5: Change location to the top-level directory of the source distribution: 

```bash
	cd /path/to/mysql-connector-cpp
```

6: Tell the build system where the Boost files are located by defining the BOOST_ROOT option when you invoke CMake:

```bash
	locate /boost/version.hpp
    
	cmake . -DBOOST_ROOT=/path/to/boost_1_58_0
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

> `/usr/local/lib`; `/usr/lib`

13: In the same panel, navigate to **`Libraries`**. I choose the Dynamic Library, so that `libmysqlcppconn.so` is chosen for

>  **Add Library** (NOT Add Library File): (dynamic) `/usr/lib/libmysqlcppconn.so`, display`"mysqlcppconn"`
   
   **Note**: Make sure the libmysqlcppconn.so is included in the added librart directories.