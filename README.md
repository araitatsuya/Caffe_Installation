# Caffe_Installation
Mac OS Sierra. v10.12.5.
Python. v2.7.13.
Homebrew Package Manager.

1
http://d.hatena.ne.jp/shu223/touch/20160105/1451952796
```{r, engine='bash', code_block_name}
    $ brew install --fresh -vd snappy leveldb gflags glog szip lmdb
    $ brew tap homebrew/science
    $ brew install hdf5 opencv
    $ brew install --build-from-source --with-python --fresh -vd protobuf
    $ brew install --build-from-source --fresh -vd boost boost-python
    $ brew install openblas
```
It worked. 
  
Or 1 They tweaked opencv.

http://installing-caffe-the-right-way.wikidot.com/start

We will need to edit the OpenCV installation file a bit.
```{r, engine='bash', code_block_name}
  $ brew edit opencv
```
replace the following lines -
```{r, engine='bash', code_block_name}
  args « "-DPYTHON#{py_ver}_LIBRARY=#{py_lib}/libpython2.7.#{dylib}"
  args « "-DPYTHON#{py_ver}_INCLUDE_DIR=#{py_prefix}/include/python2.7"
  with -
  args « "-DPYTHON_LIBRARY=#{py_prefix}/lib/libpython2.7.dylib"
  args « "-DPYTHON_INCLUDE_DIR=#{py_prefix}/include/python2.7"
```
Install snappy, leveldb, gflags, glog, szip, lmdb and opencv.
```{r, engine='bash', code_block_name}
  $ brew install —fresh -vd snappy leveldb gflags glog szip lmdb homebrew/science/opencv
```
Install protobuf.
```{r, engine='bash', code_block_name}
  $ brew install —build-from-source —with-python —fresh -vd protobuf
```
Install boost libraries for python.
```{r, engine='bash', code_block_name}
  $ brew install —build-from-source —fresh -vd boost boost-python
```

2 Download Caffe
```{r, engine='bash', code_block_name}
  $ git clone https://github.com/BVLC/caffe.git
```

3 Edit the Makefile.config (Very Important!)
```{r, engine='bash', code_block_name}
  $ cd caffe
  $ cp Makefile.config.example Makefile.config
```
The following two lines are very important. 
```{r, engine='bash', code_block_name}
    PYTHON_INCLUDE := /usr/include/python2.7 \
    ...
    PYTHON_LIB := /usr/local/Cellar/python/2.7.9/Frameworks/Python.framework/Versions/2.7/lib/
    ...
```
If the paths are not correct, compiler will be upset as Python.h and/or numpy/arrayobject.h will be missing. 
Even if it is seemingly compiled, python will not import caffe. (e.g. Segmentation fault: 11)

4 Compile
```{r, engine='bash', code_block_name}
$ make clean
$ make all -j4
$ make test -j4
$ make runtest
```
5 Build PyCaffe
```{r, engine='bash', code_block_name}
$ cd python/
$ for li in $(cat requirements.txt); do sudo pip install $li; done 
```
```{r, engine='bash', code_block_name}
$ cd ../
$ make pycaffe
$ make distribute
```
5 Path for caffe
```{r, engine='bash', code_block_name}
$ vi ~/.bash_profile
```
```
export PYTHONPATH=<caffe-home>/python:$PYTHONPATH
```
```{r, engine='bash', code_block_name}
$ source ~/.bashrc
```
6 Hope this works
```{r, engine='bash', code_block_name}
$ python 
>>> import caffe
```
If you see the following error message, you'd better revisit #3. 
```{r, engine='bash', code_block_name}
>>> Segmentation fault: 11
```

Cheers. 

UT Southwestern Medical Center
Tatsuya Arai
06/03/2016






