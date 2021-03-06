
This tutorial will explain how to install the Pandora framework using `CMAKE`.

### Requirements

You will need a PC with GNU/Linux installed . The tutorial will explain how to install Pandora on Ubuntu(16.04 or later), but any other distribution should work (probably with additional effort related to looking for the correct packages and versions).

### Dependences of Pandora

To compile and install Pandora we need some libraries and programs to do so. This is a summary of what we need. How to install everything step by step and will be explained after this part:

    ```
    GDAL
    HDF5
    CMAKE
    SCONS
    ```

* Libraries:
	
	```
	mpich
	python3 
	python3-dev
	```
	
* Boost packages:
	
	```
	libboost-python-dev 
	libboost-filesystem-dev 
	libboost-system-dev 
	libboost-timer-dev
	```

### Dependences of Cassandra

This are the dependences of Cassandra, a GUI to analyse Pandora's outputs

* `SCONS`
* Libraries:
	
	```
	libtinyxml-dev 
	libdevil-dev 
	freeglut3-dev 
	libqwt-dev 
	libqt4-dev 
	libqt4-opengl-dev 
	libgdal1-dev 
	build-essential 
	```

* Boost packages

	```
	libboost-random-dev 
	libboost-test-dev 
	libboost-timer-dev 
	libboost-chrono-dev
	```
	
## Compile and Install Pandora

- First of all, open a console terminal and upgrade your system to the last updates

```bash
 sudo apt-get update
```

- Compiling and installing `GDAL`
	1. Download and decompress it using the following commands:

	```bash
	wget http://download.osgeo.org/gdal/1.10.1/gdal-1.10.1.tar.gz
	tar xvfz gdal-1.10.1.tar.gz
	```

	2. Configure the library

	```bash
	cd gdal-1.10.1/
	CXXFLAGS="-fpermissive" ./configure --prefix=/opt/gdal-1.10.1 --with-pcraster=internal --with-png=internal --with-libtiff=internal --with-geotiff=internal --with-jpeg=internal --with-gif=internal --with-netcdf=no --enable-debug
	```

	3. Compile and install 

	```bash
	make
	sudo make install
	```

	4. Add `gdal` to your libraries path:

	```bash
	export GDAL_ROOT=/opt/gdal-1.10.1
	export LD_LIBRARY_PATH=${GDAL_ROOT}/lib/:${LD_LIBRARY_PATH}
    ```
    if you want to avoid to type those command each time you can add thos line to your `.bashrc`:
    ```bash
	echo ""  >> ~/.bashrc
	echo "export GDAL_ROOT=/opt/gdal-1.10.1" >> ~/.bashrc	
	echo "export LD_LIBRARY_PATH=\${GDAL_ROOT}/lib/:\${LD_LIBRARY_PATH}" >> ~/.bashrc
	```
	
- Compiling and installing `HDF5`
	1. `HDF5` needs the following library to be installed

	```bash
	sudo apt-get install mpich
	```
	2. Download and decompress `HDF5` source 

	```bash
	wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.8/hdf5-1.8.19/src/hdf5-1.8.19.tar.gz	
	tar xvfz hdf5-1.8.19.tar.gz
	```

	3. Configure the library 

	```bash
	cd hdf5-1.8.19/
	./configure --prefix=/opt/hdf5-1.8.19 --enable-fortran --enable-parallel --enable-debug=all	
	```

	4. Compile and install it

	```bash
	make
	sudo make install
	```

	5. Add `hdf5` to your `$LD_LIBRARY_PATH`.

	```bash
	export HDF5_ROOT=/opt/hdf5-1.8.19/
	export LD_LIBRARY_PATH=${HDF5_ROOT}/lib/:${LD_LIBRARY_PATH}
	echo ""  >> ~/.bashrc
	echo "export HDF5_ROOT=/opt/hdf5-1.8.19/"  >> ~/.bashrc
	echo "export LD_LIBRARY_PATH=\${HDF5_ROOT}/lib/:\${LD_LIBRARY_PATH}" >> ~/.bashrc
	```

Before compiling Pandora we need to install some more libraries

- Python
	* We use Python 3, the libraries we need are the following ones:

	```bash
	sudo apt-get install python3 python3-dev
	```

- Boost Packages
	* You can install the boost packages that we need using the following command:

	```bash
	 sudo apt-get install libboost-python-dev libboost-filesystem-dev libboost-system-dev libboost-timer-dev
	```

- Compile Pandora
Now that we have all the dependencies installed let's go and compile Pandora herself
	
	1. Download the source of the project. 
        Using git: 

	```bash
	git clone https://github.com/QuimLaz/pandora.git 
	```
    or downloading a zip of the source: [here](https://github.com/xrubio/pandora/archive/master.zip)

    ```bash
    wget https://github.com/xrubio/pandora/archive/master.zip
    unzip master.zip
    ```


	2. Then change to the following directory:
	
	```bash
	cd pandora #or pandora-master if you download the zipfile
	```

	3. We will know use `cmake` to generate the makefiles, if you don't have it already install it with:

	```bash
	sudo apt-get install cmake
	```
	
	4. Then we have to make a build folder to build the makefiles

	```bash
	mkdir build
	cd build
	```
	
	5. After that we need to configure, compile and install Pandora

	```bash
	cmake -DCMAKE_INSTALL_PREFIX=/opt/pandora ../
	make
	sudo make install
	```

    If you want to compile also the example given with pandor you will have to turn on the option with cmake:
	```bash
	cmake -Dexample=ON -DCMAKE_INSTALL_PREFIX=/opt/pandora ../
	make
	sudo make install
	```
	
	6. And finally export Pandora's libraries

	```bash
	export PANDORAPATH=/opt/pandora/
	export PATH=${PANDORAPATH}/bin:${PATH}
	export PYTHONPATH=${PANDORAPATH}/bin:${PYTHONPATH}
	export LD_LIBRARY_PATH=${PANDORAPATH}/lib:${LD_LIBRARY_PATH}
	echo ""  >> ~/.bashrc
	echo "export PANDORAPATH=/opt/pandora/"  >> ~/.bashrc
	echo "export PATH=${PANDORAPATH}/bin:${PATH}" >> ~/.bashrc
	echo "export PYTHONPATH=${PANDORAPATH}/bin:${PYTHONPATH}" >> ~/.bashrc
	echo "LD_LIBRARY_PATH=${PANDORAPATH}/lib:${LD_LIBRARY_PATH}" >> ~/.bashrc
	```
	
	* You might want to check if `LD_LIBRARY_PATH` is updated correctly using this:
	
	```bash
	echo $LD_LIBRARY_PATH
	```
	
	If the output isn't the following one, execute the commands in point 7. Skip 7 otherwise
	
	Expected output: 
	```bash
	/opt/pandora//lib:/opt/hdf5-1.8.19//lib:/opt/gdal-1.10.1/lib/:
	```
	
	7. If the `LD_LIBRARY_PATH` isn't correctly updated you can do it manually using this:
	
	```bash
	echo LD_LIBRARY_PATH -= :/opt/pandora/path
	echo LD_LIBRARY_PATH += :/opt/pandora/lib/
	```

## Compile and Install Cassandra (GUI)

Now you could use Pandora just fine. But we offer you a GUI to make the interpretation of your models in a easier way. To do so go to Pandora's main directory

1. The first you need to do is install `scons`. We'll use it to compile the models:
		
	```bash
	sudo apt-get install scons
	```
	
2. To compile Cassandra more libraries are needed. You can install them all using the following command:
	
	```bash
	sudo apt-get install libtinyxml-dev libdevil-dev freeglut3-dev libqwt-dev libqt4-dev libqt4-opengl-dev libgdal1-dev build-essential libboost-random-dev libboost-test-dev libboost-timer-dev libboost-chrono-dev
	```
	
3. Then you just need to go to the Cassandra directory and compile her using the Makefile there

	```bash
	cd pandora/cassandra/
	make
	```
	
4. Finally an executable file will be generated in the `pandora/bin` directory. To execute Cassandra do as follows

	```bash
	cd ../bin/
	./cassandra
	```

        
[Next - Get Started with pyPandora](01_getting_started_pyPandora.md)
Or [Next - Get Started with C++](02_getting_started_pandora.md)
