# cfs-rcc-demo

This repository contains code and instructions to build the RTEMS Kernel Image (RKI) and core Flight System (cFS) for the LEON3 and LEON4 using the RCC 1.3.1 toolchain and BSPs provided by Gaisler.

The RCC 1.3.1 toolchain provides the GCC Sparc/LEON cross compiler and RTEMS 5.1 compiled for all of the Gaisler Sparc/LEON targets such as LEON3 and LEON4 (GR740)

The RCC 1.3.1 toolchain for linux is available here:
https://www.gaisler.com/index.php/downloads/compilers

The specific compiler I used is here:
https://www.gaisler.com/anonftp/rcc/bin/linux/sparc-rtems-5-gcc-10.2.0-1.3.1-linux.txz

## Toolchain installation:
I install the RCC toolchain in a Tools directory in my linux home directory:

```
$HOME/Tools/rcc-1.3.1-gcc
```

For a shared workstation, it often makes sense to install the toolchain in the /opt directory:
```
/opt/rcc-1.3.1-gcc
```

After you install the toolchain, make sure you set the path to the bin file:
For my setup, I put the following in my ~/.profile file (Ubuntu Linux):

```
# set RCC 1.3.1 PATH
if [ -d "$HOME/Tools/rcc-1.3.1-gcc/bin" ] ; then
    PATH="$HOME/Tools/rcc-1.3.1-gcc/bin:$PATH"
fi
```

There are two places in this repository that care about where the tools are installed:
1. RTEMS Kernel Image (rki2) directory. The file is rki2/build/rtems-paths.mak
2. cFS directory - The toolchain file(s) for the LEON3 and LEON4(GR740)
Depending on where you install your tools, you may need to adjust these files. 


## Building the RKI2 (without cFS):
To start, you can build the RKI2 project and run it on a LEON target such as a simulator or the GR740 board.
1. In the rki2 directory check the build/rtems-paths.mak and adjust the toolchain location if needed
2. In the rki2/build/leon-rcc directory, edit the Makefile and select the Board Support Package you want to build for:

```
##
## Select your BSP here 
##
# BSP ?= gr740_smp
BSP ?= leon3
```

To build for the gr740, uncomment the gr740_smp line and put a comment marker in front of the leon3 line.
To build for the leon3, leave it as is above.

Once you have the compiler installed, the path set to your compiler in your linux .profile, and the BSP selected, just type:

```
make
```

The RKI project will be compiled and it will result in rki.elf, which is an executable that can be loaded to the target and run.

When you run this project, you will see the following on the UART:
RTEMS Kernel Image Booting

```
*** RTEMS Info ***
5.0.0.cba7f0485d9c8a37a889586b188070f365106065-modified


 BSP Ticks Per Second = 100
*** End RTEMS info ***

Populating Root file system from TAR file.
Setting up filesystems.
rki_init_network: Calling system_init.
Initializing network
Can't set default route: Network is unreachable
Initializing network DONE

Destination     Gateway/Mask/Hw    Flags     Refs     Use Expire Interface
127.0.0.1       127.0.0.1          UH          0        0      0 lo0

************ INTERFACE STATISTICS ************
***** lo0 *****
Address:127.0.0.1       Net mask:255.0.0.0       
Flags: Up Loopback Running Multicast
Send queue limit:50   length:0    Dropped:0       



Starting the FTP Server.
syslog: ftpd: FTP daemon started (1 session max)
Initializing Local Commands.
Adding Target specific commands
Running /shell-init.

1: mkdir ram
2: mkrfs /dev/rda
rtems-rfs: format: buffer open failed: 6: No such device or address
error: format of /dev/rda failed: No such device or address
3: mount -t rfs /dev/rda /ram
error: No such device or address
4: hello
    ____  ______________  ________ 
   / __ \/_  __/ ____/  \/  / ___/ 
  / /_/ / / / / __/ / /\_/ /\__ \  
 / _, _/ / / / /___/ /  / /___/ /  
/_/ \_| /_/ /_____/_/  /_//____/   

Hello RTEMS!
Create your own command here!
Starting shell....


RTEMS Shell on /dev/console. Use 'help' to list commands.
shell0 [/] # 
```


You can explore the commands available with the "help" command on the RTEMS shell.


