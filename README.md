Programmer : Kosei Ohara
Last Update : 2024/Apr/20


Test Environment
	gale (2024/Apr/20)
	Tested with output file of MIM.
	Not tested with big-endian data.


About this program
	Direct Access provided by Fortran makes it easier to read binary file, but as far as I know, there is 
	no usuful function to read binary file like that with Fortran.
	This function make us easy to read data seemlessly.
	The function takes the record number and skip unnecessary data, then return specified length of data.
    This is renewed version of ver1. class is newly used in this virsion.


Functions
    __init__
        Initialize the class.
        You need to declare filein with arguments below:
            filename
                hidden (you cannot access this variable after declaration)
                Name of input file
                string type
            shape
                hidden (you cannot access this variable after declaration)
                shape of output array. list, tuple, and ndarray are checked
            recl
                hidden (you cannot access this variable after declaration)
                data size of output in byte
                recl=kind*(number of element of output)
            rec
                NOT hidden (you can access this variable even after declaration)
                Record you want to read at the first step
                After the first step, this parameter increases (recstep) every call of fread()
            kind
                hidden (you cannot access this variable after declaration)
                kind-parameter
                4 for single precision real, 8 for double precision real
            endian
                hidden (you cannot access this variable after declaration)
                endian type of the binary file
                'little' and 'big' are available. You can also write in capital letters like 'LITTLE' or 'LiTtLe'
            recstep
                hidden (you cannot access this variable after declaration)
                Number of skipped records per call
                In n-th call of fread, rec+recstep*(n-1) th recored is read.
        In this class, file is opened when this is called

    __del__
        If class is vanished without closing the opened file, fclose() is called implicitly
    
    fclose
        close the file

    fread
        main function.
        no arguments
        Execute read_direct. recstep is added to rec
        If one of the data element is NaN, Warning is printed (you can comment-out warning lines)

	read_direct
		Users call only this routine.
        Programmer assumes fread() is to be used, but you can call this function directoly
        If you not fread but this, most arguments are not checked.
        Description of arguments is omitted

	get_kind
		sub function to return kind specifier.
		If argument 'kind' is 4, 'f' is returned by this function.
		If 8, 'd' is returned.
		Error for any other value.
		
	get_endian
		sub function to return endian specifier.
		If argument 'endian' is 'little', '<' is returned by this function.
		if 'big', '>' is returned.
		Error for any other value.

	__argcheck
        hidden (you cannot access this function)
		sub function to check the validity of arguments.


Notes
	In Python, memory order is not same as that in Fortran.
	If you want to read data outputted by
		write(unit,rec=record) array(1:nx,1:ny,1:nz)
	in Fortran, you need to declare array shape as array[0:nz,0:ny,0:nx] in Python

