# Read Direct Access Files in Python

Developer : Kosei Ohara

## Environment
Python 3.7.7


## About this program
Direct Access in Fortran makes it easier to read binary file, but as far as I know, there is 
no usuful function to read binary file like that in Fortran.  
This function make us easy to read data seemlessly in Python.  
The function takes the record number and skip unnecessary data, then return specified length of data.


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
List, tuple, and ndarray are checked.

#### recl
Hidden (Cannot be accessed after declaration).  
Data size of output in byte.  
recl=kind*(number of element of output)  

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
If class is vanished without closing the opened file, fclose() is called implicitly
    
### fclose
close the file

### fread
main function.  
no arguments.  
read_direct() is executed in this function.  
filein.\_\_recstep is added to filein.rec  
If one of the data element is NaN, Warning is printed (you can comment-out warning lines).

## Notes
In Python, memory order is not same as that in Fortran.
If you want to read data outputted by
```Fortran
write(unit,rec=record) array(1:nx,1:ny,1:nz)
```
in Fortran, you need to declare array shape as array[0:nz,0:ny,0:nx] in Python

