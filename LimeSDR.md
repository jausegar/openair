# LimeSDR

This tutorial is for LimeSDR Mini. If you use LimeSDR USB, click [here](https://github.com/myriadrf/LimeSDR-USB). In this [link](https://github.com/myriadrf/trx-lms7002m) you can find the ini files for LimeSDR USB and LimeSDR Mini.

# Kernel/Machine Setup

Follow the instruction provided [here](https://gitlab.eurecom.fr/oai/openairinterface5g/wikis/OpenAirKernelMainSetup) to setup you machine, in particular the power / cpu freq management. This will resolve, in most of the cases, the realtime issues when running OAI and the RF driver.

Install the required Ubuntu packages for LimeSDR Mini.
Note: the following steps will be shortly integrated with build_oai script

# Install from packages

The drivers PPA for Ubuntu has a recent build of LimeSuite:

    sudo add-apt-repository -y ppa:myriadrf/drivers
    sudo apt-get update
    sudo apt-get install limesuite liblimesuite-dev limesuite-udev limesuite-images
    sudo apt-get install soapysdr soapysdr-module-lms7
    sudo apt-get install soapysdr-module-audio soapysdr-module-uhd uhd-soapysdr soapysdr-module-rtlsdr
    sudo apt install gr-gsm gqrx-sdr

For more information, have a look at Lime Suite, git clone https://github.com/myriadrf/LimeSuite, and read the following file: LimeSuite/docs/lms7suite_compilation_guide.pdf

# Install from source

The instructions are provided [here
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

# Find devices and testing

Sometimes when it's plugged in, LimeUtil --find reports

    $ LimeUtil --find
        * [LimeSDR Mini, media=USB 3.0, module=FT601, addr=24607:1027, serial=1D3AEC399FFA28]

but sometimes it reports

    $ LimeUtil --find
        * [LimeSDR Mini, media=USB 2.0, module=FT601, addr=24607:1027, serial=1D3AEC399FFA28]

And I have no idea why it might switch because each port I'm plugging it into should be USB 3.0 capable.

Run uhd_find_devices to make sure that your LimeSDR is found by the uhd-soapy interface:

    $ uhd_find_devices 
    [INFO] [UHD] linux; GNU C++ version 8.3.0; Boost_106700; UHD_3.13.1.0-3build1
    --------------------------------------------------
    -- UHD Device 0
    --------------------------------------------------
    Device Address:
        serial: 
        default_input: False
        default_input: True
        default_output: False
        default_output: True
        device_id: 0
        device_id: 6
        driver: audio
        label: default
        label: hw:HDA Intel PCH,0
        type: soapy

    --------------------------------------------------
    -- UHD Device 1
    --------------------------------------------------
    Device Address:
        serial: 1D3AEC399FFA28
        addr: 24607:1027
        driver: lime
        label: LimeSDR Mini [USB 3.0] 1D3AEC399FFA28
        media: USB 3.0
        module: FT601
        name: LimeSDR Mini
        type: soapy

Make sure your LimeSDR has the latest firmware:

    $ LimeUtil --find
        * [LimeSDR Mini, media=USB 3.0, module=FT601, addr=24607:1027, serial=1D3AEC399FFA28]

    $ LimeUtil --update
        Connected to [LimeSDR Mini [USB 3.0] 1D3AEC399FFA28]
        Existing gateware is same as update (1.30)

        Programming update complete!
        
Note: It is better to have just one device.

# Calibration

In order to calibrate the VCO in the LimeSDR, we will use LimeSuiteGUI. We will connect the device selecting Options/ConnectionSettings. After this, we will do a full calibration in the calibration tag, by pressing 'Calibrate all'. Finally, we have to disconnect the device.


