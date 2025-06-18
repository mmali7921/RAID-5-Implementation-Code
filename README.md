



Details
-------
You will create a simulator program named raidsim.


Virtual disk array
Please use the functions defined in disk-array.h. The file test.c shows how to use the functions. The block size for the project is defined in disk.h.


RAID 5
Blocks should be striped in strip size units across N disks, reserving one disk for parity. For the first strip, disk 0 should be used for parity, for the second disk 1 etc. For stripe, parity returns to disk 0.
For example, with strip size 1 and 4 disks:




For writes less than a full stripe, you can use additive (read the old blocks and compute new parity) or subtractive (read the old block and old parity) parity. For writes to a full stripe, just write out the data and new parity.


Specification
-------------
### Command line parameters (these can come in any order):

• -level [5] specifies the RAID level 5

• -strip n specifies the number of blocks in a strip

• -disks n specifies the use of n disks. 

• -size n specifies the number of blocks per disk

• -trace filename specifies the name a file containing a trace of requests

• -verbose set the global variable "verbose" to 1 (defaults to 0)

All parameters are mandatory except "verbose". A sample command line:

```raidsim -level 5 -strip 5 -disks 10 -size 10000 -trace input.txt```


### Trace file format

The trace file contains a sequence of lines with one command per line. There will be exactly one space between each word in a command. The commands can be:

• READ LBA SIZE - read starting at block LBA for SIZE blocks. Prints out the first 4 byte value in each block, separated by spaces on a single line.

• WRITE LBA SIZE VALUE - write the 4-byte pattern VALUE repeatedly starting at block LBA for SIZE blocks.

• FAIL DISK - make disk number DISK fail, so it cannot be read or written any more. No output.

• RECOVER DISK - make disk number DISK recover as a clean, empty disk that can be read or written. No output.

• END - last command in trace.

For each RAID type, your simulator should try to complete as much of a command as possible: return as much data as possible on a read and write as much data as possible on a write.


Output
------
Your simulator should print the command line from the trace and then execute it, then print the desired output on the following line. After the END command, your simulator should call disk_array_print_stats() to print the statistics for the disks. On read errors, replace the value (which may be one of many in a set of blocks) with the word ERROR. Thus, a read may return a mix of values and an error. On write errors, print ERROR if any blocks of the write cannot be written.

For writes less than a full stripe, you can use additive (read the old blocks and compute new parity) or subtractive (read the old block and old parity) parity. For writes to a full stripe, just write out the data and new parity.


Tests
-----
To help with evaluating the implementation, tests are included.
