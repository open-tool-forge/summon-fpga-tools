# Summon-FPGA-TOOLS

This repository contains a simple but flexible script that builds a open-source FPGA tools. These tools consist from:

* Yosys
* nextpnr-ice40
* nextpnr-ecp5
* arachne-pnr
* icestorm
* prj-trellis
* icarus-verilog

More tools will be added over time. The current candidates are:

* verilator
* gtkwave

This script is intended to not require super user privileges and installs all the tools into `${HOME}/sft` directory. Obviously you will need some rights to install the dependencies provided by your operating system.

As many of the tools don't have official release tarballs, we are currently building the newest `master` releases of all the tools except Yosys. Yosys is currently the only tool that has official release tarballs, and we stick to them for now. If you want to build Yosys from the bleeding cutting edge run the build script as follows: `./summon-fpga-tools.sh YOSYS_GIT=master`

## Dependencies

You will need to install the following dependencies to be able to run this script.

### Debian/Ubuntu

```
sudo apt install build-essential clang bison flex libreadline-dev gawk \
                 tcl-dev libffi-dev git mercurial graphviz xdot pkg-config \
                 python python3 libftdi-dev qt5-default libqt5opengl5-dev \
                 python3-dev libboost-all-dev git
```

### Mac OS

XCode with command line tools.

```
brew install cmake python boost boost-python3 qt5 git libftdi bison gperf
```

## To compile the open source FPGA tools:

* `git clone https://github.com/esden/summon-fpga-tools.git`
 or
* `wget https://github.com/esden/summon-fpga-tools/zipball/master; unzip master`
* `cd summon-fpga-tools`
* `./summon-fpga-tools.sh`
* `export PATH=~/sft/bin`
* Profit

## Command line options

You can suffix the script call with the following variable parameters:

### `PREFIX=`

By default the installation prefix is `${HOME}/sft` you can change it to `/usr` or `/usr/local` then the binaries will be installed into `${HOME}/sft/bin`, `/usr/bin` or `/usr/local/bin` respectively.

### `SUDO=`

By default this variable is empty. If you need root rights for the install
step you may set this variable to `sudo`.

```
$ ./summon-fpga-tools.sh SUDO=sudo
```

This will prefix all make install steps with the sudo command asking for
your root password.

### `QUIET=`

By default set to 0. To decrease console output (may increase compile speed
in some cases) you can set this variable to 1.

### `CPUS=`

Overrides the autodetection of CPU cores on the host machine. This option
is translated into the `-j$CPUS+1` option to the make command when running
the script.

## Example:

```
$ ./summon-fpga-tools.sh CPUS=5
```

This will run the script with 5 CPUs on your host machine resulting in calling all make commands with `-j6`.

## Troubleshooting

**I am running iceprog and the programmer is not being detected**

* Check if the device is being detected by the kernel with 'lsusb' it will either show up as a Future Electronics device or the name of the programmer vendor.
* If the device is being detected by the kernel you might not have permissions to access the device. If you run `sudo iceprog ...` and the device is decected you can give yourself permissions by creating a udev file at: `/etc/udev/rules.d/53-lattice-ftdi.rules` and adding the following line in that file:
```
ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6010", MODE="0660", GROUP="plugdev", TAG+="uaccess"
```
After adding that file you need to at least replug the programmer or even reload the udev rules.

## Questions:

If you have any questions please contact us on gitter. Please contribute suggestions in for of issues and pull requests! :D
