# LimeSDR

# Kernel/Machine Setup

Follow the instruction provided [here] (https://gitlab.eurecom.fr/oai/openairinterface5g/wikis/OpenAirKernelMainSetup) to setup you machine, in particular the power / cpu freq management. This will resolve, in most of the cases, the realtime issues when running OAI and the RF driver.

Install the required Ubuntu packages for Lime
Note: the following steps will be shortly integrated with build_oai script

Install from packages

The drivers PPA for Ubuntu has a recent build of LimeSuite:

    sudo add-apt-repository -y ppa:myriadrf/drivers
    sudo apt-get update
    sudo apt-get install limesuite liblimesuite liblimesuite-dev limesuite-udev limesuite-images
    sudo apt-get install soapysdr soapysdr-module-lms7

For more information, have a look at Lime Suite, git clone https://github.com/myriadrf/LimeSuite, and read the following file: LimeSuite/docs/lms7suite_compilation_guide.pdf

# Install from source

The instructions are provided here[here
](https://open-cells.com/index.php/2017/05/10/limesdr-installation/) by [open-cells](https://open-cells.com), and are added here for completeness.

## Install the required packages

    sudo apt-get install cmake g++ libpython-dev python-numpy swig git libsqlite3-dev libi2c-dev libusb-1.0-0-dev libwxgtk3.0-dev freeglut3-dev

## Install the SoapySDR

    git clone https://github.com/pothosware/SoapySDR.git
    cd SoapySDR
    git pull origin master
    mkdir build && cd build
    cmake ..
    make -j4
    sudo make install
    sudo ldconfig

## Install LimeSDR

    git clone https://github.com/myriadrf/LimeSuite.git
    cd LimeSuite
    mkdir build && cd build
    cmake ..
    make -j4
    sudo make install
    sudo ldconfig
    cd ../udev-rules/
    sudo ./install.sh
    # Download board firmware
    sudo LimeUtil --update
