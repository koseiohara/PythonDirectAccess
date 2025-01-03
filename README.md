# Read Direct Access Files in Python

Developer : Kosei Ohara

## Environment
Python 3.7.7


## About this program
Direct Access in Fortran makes it easier to read binary files.
However, as far as I know, there is no easy and useful function in Python that works like Fortran's Direct Access for reading binary files.
This function allows us to seamlessly read no header binary data in Python.
It takes the record number, skips unnecessary data, and returns data of the specified length.


## filein
In this class, file is opened when this is called

### \_\_init\_\_
Initialize the class.  
You need to declare filein with arguments below

#### filename
Hidden (Cannot be accessed after declaration).  
Name of input file.  
string.  

#### shape
Hidden (Cannot be accessed after declaration).  
Shape of output array.  
List, tuple, and ndarray were checked.

#### recl
Hidden (Cannot be accessed after declaration).  
Data size of output in byte.  
recl=kind*(number of elements of output)  

#### rec
NOT hidden (Can be accessed even after declaration)  
Record you want to read at the first step  
After the first step, this parameter increases (recstep) every call of fread()

#### kind
Hidden (Cannot be accessed after declaration).  
kind-parameter  
4 for single precision real, 8 for double precision real

#### endian
Hidden (Cannot be accessed after declaration).  
Endian type of the binary file.  
'little' and 'big' are available. You can also write in capital letters like 'LITTLE' or 'LiTtLe'

#### recstep
Hidden (Cannot be accessed after declaration).  
Number of skipped records per call.  
In n-th call of fread, rec+recstep*(n-1) th recored is read.

### \_\_del\_\_
If class vanishes without closing the file, fclose() is called implicitly
    
### fclose
close the file

### fread
main function.  
no arguments.  
read_direct() is executed in this function.  
filein.\_\_recstep is added to filein.rec  
If one of the data element is NaN, warning is printed (you can comment-out warning lines).

## Usage
To read a single precision no headder binary, this class will be called like below:
```Python
from filein import filein

---

file_pointer = filein(filename=ifile     , \
                      shape   =[nz,ny,nx], \
                      recl    =4*nx*ny*nz, \
                      rec     =1         , \
                      kind    =4         , \
                      endian  ='LITTLE'  , \
                      recstep =1           )
mean = numpy.zeros([nz,ny,nx])
work_reader = numpy.empty([nz,ny,nx])

for t in range(nt):
    work_reader[:,:,:] = file_pointer.fread()
    mean[:,:,:] = mean[:,:,:] + work_reader[:,:,:]

mean[:,:,:] = mean[:,:,:] / float(nt)
```
where nx, ny, and nz are number of grids in x, y, and z direction, respectively.
mean is the time average of each grid.


