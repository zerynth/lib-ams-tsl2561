.. module:: tsl2561

**************
TSL2561 Module
**************

This module contains the driver for AMS TSL2561 Luminosity sensor integrating both infrared and full-spectrum photodiodes.
The TSL2561 is capable of direct I2C communication and is able to conduct specific light ranges from 0.1 - 40k+ Lux easily. Additionally, it contains two integrating analog-to-digital converters (ADC) that convert currents from the two photodiodes, simultaneously (`datasheet <http://ams.com/eng/content/download/250094/975485/file/TSL2560-61_DS000110_2-00.pdf>`_).
    
.. class:: TSL2561(i2cdrv, addr=0x49, clk=400000)

    Creates an intance of a new TSL2561.

    :param i2cdrv: I2C Bus used '( I2C0, ... )'
    :param addr: Slave address, default 0x49
    :param clk: Clock speed, default 400kHz

    Example: ::

        from ams.tsl2561 import tsl2561

        ...

        tsl = tsl2561.TSL2561(I2C0, addr=tsl2561.TSL_I2C_ADDRESS["LOW"])
        tsl.start()
        tsl.init()
        lux = tsl.get_lux()

    .. note:: This sensor has a dedicaded "Address" pin that allows to select 1 of 3 available address as shown in the table below.

======================= =========== ========================================
ADDR SEL Terminal Level I2C Address Zerynth Define 
======================= =========== ========================================
    GND                    0x29     addr = tsl2561.TSL_I2C_ADDRESS["LOW"]      
    Float                  0x39     addr = tsl2561.TSL_I2C_ADDRESS["NORMAL"]   
    Vdd                    0x49     addr = tsl2561.TSL_I2C_ADDRESS["HIGH"]    
======================= =========== ========================================

    
.. method:: init(gain=1, timing=0, pack=1)

        Initialize the TSL2561 setting the gain, timing and kind of package.

        :param gain: set the gain of the sensor (values allowed: 0 for gain=1x and 1 for gain=16x), default 1
        :param timing: set the integration time value (from 0 to 2 - 0 for 13 ms, 1 for 101 ms, 2 for 402 ms), default 0
        :param pack: set the kind of package for the correct lux calculation (values allowed: 0 for CS package and 1 for T,FN,CL package), default 1

        
.. method:: get_raw_fullspectrum()

        Retrieves the current raw value read on channel0 (full-spectrum photodiode).

        Returns raw_fs

        
.. method:: get_raw_infrared()

        Retrieves the current raw value read on channel1 (infrared photodiode).

        Returns raw_ir

        
.. method:: get_raw_visible()

        Retrieves the difference between the current raw value read on channel0 and raw value on channel1 (visible spectrum).

        Returns raw_vis = (raw_fs - raw_ir)

        
.. method:: get_lux()

        Converts the raw sensor values to the standard SI lux equivalent (according to the sensor settings - gain, timing and kind of package).

        Returns lux value or 0 if the sensor is saturated and the values are unreliable.

        
