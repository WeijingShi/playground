# Simulate openmp code in gem5 by m5thread 

Openmp is so far not supported in gem5 se, so we can either 1. use fully system (fs) simulation or 2. link openmp code to m5thread as a workaround.

The second option using m5thread seems quite straight forward. 

Download m5thread here: https://github.com/gem5/m5threads/blob/master/

Build the library:

    $cd ~/m5thread
    $make
    
Above, we assume the host system is using the same ISA as the simulation machine here. If they're different, modify the makefile. A good reference is ~/m5thread/test/Makefile  

Let's try the example, compile the code:

    $cd ~/m5thread/test/
    $g++ -g -o3 -fopenmp -c -o test_omp.o test_omp.cpp
    
Statically link to the library.

    $g++ -static -o test_omp test_omp.o -lgomp ~/myOMP/libpthread.a -lgomp

Simulate with an example X86-isa gem5 build (if you have not build one, check the gem5 website)

    $cd ~/gem5-gpu/gem5/
    $build/X86_VI_hammer_GPU/gem5.opt ../gem5-gpu/configs/se_fusion.py -n 8 -c /home/weijing/gem5-gpu/m5threads/tests/test_omp.cpp -o "8 8"

Note, we change the number of codes to 8, so that we can have 8 threads. 
