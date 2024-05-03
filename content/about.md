Hello, and welcome to my teeny-tiny corner of the web!\
I have created this website to write about things I find interesting and as a note to my future self. \
I love Computers but I also like reading books, watching movies, traveling places among many other things.\
\
Over the years I have collected notes on various Computer Science related fields and other topics which can be browsed [here](../notes/). \
It's a never ending list. Hopefully, someday I would be able to read most if not all of it.


### Contact

For questions and comments, please contact me at kumarravi1165@gmail.com.

5FC*#%#5Crhcaf

QBUS

How to spawn a JVM per core running a single threaded APP
leave some CPU cores for OS stuff and interrupts
network I/O happens on separate threads
threads don't wait for more work but busy sping instead.

In most cases market data comes with FIX protocol.


Collection < User > users = … ; // Fetch. 
try
(
    ExecutorService es = Executors. newVirtualThreadPerTaskExecutor() ;
)
{
    for ( User : user ) 
    { 
        es.submit( … some task object — `Runnable` or `Callable` … ) ;
    }
}
// Flow-of-control stops here (blocks) until all submitted tasks complete/fail. 