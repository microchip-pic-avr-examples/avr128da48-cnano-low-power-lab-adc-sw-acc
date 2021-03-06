[![MCHP](images/microchip.png)](https://www.microchip.com)

# AVR DA family training - Low Power Using ADC Software Accumulation

This repository provides an MPLAB X IDE project with an MCC (MPLAB Code Configurator) generated code example for accumulating 128 conversion with the ADC in a software implementation.

## Related Documentation
More details and code examples on the AVR128DA48 can be found at the following links:
- [Low-Power Modes Using Curiosity Nano - AVR DA Training Manual](http://www.microchip.com/DS40002243A.pdf)
- [AVR128DA48 Product Page](https://www.microchip.com/wwwproducts/en/AVR128DA28)
- [AVR128DA48 Code Examples on GitHub](https://github.com/microchip-pic-avr-examples?q=avr128da48)
- [AVR128DA48 Project Examples in START](https://start.atmel.com/#examples/AVR128DA48CuriosityNano)

## Software Used
- MPLAB® X IDE 5.40 or newer [(microchip.com/mplab/mplab-x-ide)](http://www.microchip.com/mplab/mplab-x-ide)
- MPLAB® XC8 2.20 or newer [(microchip.com/mplab/compilers)](http://www.microchip.com/mplab/compilers)
- MPLAB® Code Configurator (MCC) 3.95.0 or newer [(microchip.com/mplab/mplab-code-configurator)](https://www.microchip.com/mplab/mplab-code-configurator)
- AVR-Dx_DFP 1.1.40 or newer Device Pack
- 8-bit AVR MCUs Lib version 2.3.0

## Hardware Used
- AVR128DA48 Curiosity Nano [(DM164151)](https://www.microchip.com/Developmenttools/ProductDetails/DM164151)

## Setup
The AVR128DA48 Curiosity Nano Development Board is used as test platform.
<br><img src="images/AVR128DA48_CNANO_instructions.PNG" width="500">

The following MCC configurations must be made for this project:

 - System Module
    1. Internal Oscillator (24 MHz)
    2. Prescaler disabled
    3. WDT disabled

<br><img src="images/system_module.png" width="500">


- VREF module
	1. ADC Voltage reference: Internal 2.048V reference (this enables the temperature sensor)
	
<br><img src="images/vref.png" width="500">

- ADC 
	1. 12-bit mode
	2. No accumulation
	3. Sample Length: 31
	
<br><img src="images/ADC.png" width="500">
	

## Demo Code 

The source code for this project can be downloaded from the current page by clicking the "Download" button, or if you want to make your own project, please pay attention to the following steps:
 - After making the MCC settings, press the "Generate" button, and this will generate the required .c and .h files.
 - Then edit the resulting code by adding the following code snippets.
    
	In the "main.c" file, replace the code with the following: 

    ```
    #include "mcc_generated_files/mcc.h"

    uint32_t accumulator = 0;

    int main(void)
    {
        SYSTEM_Initialize();
        while (1){
            accumulator = 0;
            for(uint8_t i = 0; i<128; i++)
            {
                ADC0_StartConversion(ADC_MUXPOS_TEMPSENSE_gc);
                while(!ADC0_IsConversionDone())
                {
                    ;
                }
                accumulator += ADC0_GetConversionResult();
            }
        }
    }
    ```


## Operation

1. Connect the AVR128DA48 Curiosity Nano Development Board to PC using the USB cable.
2. Build the firmware and load the generated hex file into MCU.


## Demo:

After the code has been compiled and loaded onto the device, the microcontroller will start a conversion on the temperature channel. When the conversion is done, the results is added to the accumulator variable and another conversion is started until 128 conversions have been completed.

## Summary
This example represents one of the codes for the AVR DA low power lab manual.