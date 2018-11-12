##  Objective:
The objective of this project is to demonstrate how to use the Capacitive Voltage Divider (CVD) feature of the ADCC module that can be found on many of Microchip’s newer 8-Bit PIC Microcontroller devices.  This project will show how to get started with the ADCC module and make CVD measurements to detect a touch on a conductive sensor.  The PIC16F19176 was used to emulate a simple eight key touch piano. [Click HERE to see a video of the CVD Touch Piano in action!](https://vimeo.com/270141774)

## Board Connection & Setup
The development board used for this example was the Curiosity HPC Development Board. The click board used to generate an audible tone for the touch piano keyboard response was the MikroElektronika Buzz Click board.  The hardware used for the USB to EUSART serial conversion was the Microchip MCP2200 breakout module.  There are eight external conductive sensors that are tied to eight separate analog channels on the PIC microcontroller.  The PPS settings can be found at the end of this description.  Note:  In this example, conductive paint was used to create the conductive sensor that the ADCC module will perform CVD measurements on.  Any other conductive material can be used to create a sensor for each of the piano keys, although minor changes may need to be made to the source code.

### Curiosity HPC Board Setup:
![](https://static.transim.com/img/52018/b15a5ab756554e8193b38f01855bef45-l5qs9.jpg){width=auto height=auto}

## ADCC Module Setup / Configuration:
The ADCC module was used to perform CVD measurements on each of the analog channels in order to determine when any of the piano keys (conductive sensors) are touched.  The module was configured to operate in burst average mode so that multiple CVD measurements could be sequentially performed and combined, so that later on an average CVD measurement value could be computed to help filter out any discrepancies from individual readings.  The ADCC module was configured to perform 32 reads, adding each result to the previous reading.  The computation feature of this ADC module was  utilized in this example to compute a final average CVD measurement value by dividing the final accumulated ADC value by 32 (this is done by shifting the final value right by 5).  The CVD was configured to have a precharge count of 10 us, with no additional sample and hold capacitance used.  The screenshots below show the configuration settings used for these modules in MCC.

![](https://static.transim.com/img/52018/843e7bf8756049e7958a0328ae3e97df-cm77v.png){width=auto height=auto}

![](https://static.transim.com/img/52018/68614fd87af04abd87c7d085396cea00-p6pmx.png){width=auto height=auto}

## Driving the MikroElektronika Buzz Click Board:
The MikroElektronika Buzz Click board that was used in this example is driven using a PWM signal, with the tone and volume dependent on frequency and duty cycle.  The PWM4 and TIMER2 modules were both added to this project using MCC so that we could drive the Buzz Click board to produce audible tones for each of the keys pressed.  The screenshots below show the configuration settings in MCC that were used for both the PWM4 and TMR2 modules.  The PWM module was initialized to have a dutycycle of 0%.  Wen the ADCC module performs a CVD measurement and detects that one of the piano keys have been touched, software will jump into a routine that sets the PWM duty cycle and TMR period (PR Value) to represent the note of the key that was touched.

### Table 1: MikroElektronika Buzz Click PWM Period / Frequency Settings:
![](https://static.transim.com/img/52018/92eac4ec53b840e3970cfb89e60a12c2-663fg.png){width=auto height=auto}

### PWM4 Module Configuration:
![](https://static.transim.com/img/52018/df72c4dc0ecc4386b29023f3dfe96a9f-rfqxm.png){width=auto height=auto}

### TMR2 Module Configuration:
![](https://static.transim.com/img/52018/6f9ed46b06924dfe90468b68e7e6fc5b-fgxld.png){width=auto height=auto}

## EUSART Module Setup / Configuration:
The EUSART module was used in this example to display a message that described which piano key was pressed by the user in real time via serial port to PC.  The EUSART2 module was added into this example using MCC.  It was configured in Asynchronous mode to operate at a Baud Rate of 9600 with transmission enabled.  In addition to these settings, the "Redirect STDIO to USART" software setting was enabled.  The screenshot below shows the configuration settings used for the EUSART2 module in this application.

![](https://static.transim.com/img/52018/d76f1348eb0a4a8aaad1c2b1ed288382-j4qxf.png){width=auto height=auto}

### Application Code:
The first part of this application is to continually perform CVD measurements on all of the piano keys. Refer back to the PPS settings for more information about which analog channels each of the piano keys are tied to.  After a CVD measurement is performed on each of the analog channels and stored locally, the next step in this application is to use some logic to determine if any of the piano keys were pressed.  The CVD measurement from the piano key significantly changes when it is touched.  Whenever a piano key touch has been detected, software will then jump into a routine that handles the feedback portion of this application.  This routine adjusts the duty cycle of the PWM and the period of the TMR in order to play an audible tone that represents the note that was played.

### ADCC Conversion / CVD Measurement Code Snippet:
![](https://static.transim.com/img/52018/cb7e50e0dc5b47fcbbfabea8d597a73f-2hmbn.png){width=auto height=auto}
### Piano Key Pressed Audio Routine Code Snipper:
![](https://static.transim.com/img/52018/aaedf7180a3149419d4fdef60c50ae76-4wxk4.png){width=auto height=auto}

In addition to the audio feedback when a piano key press is detected from the CVD measurements, the EUSART module is also implemented in this example so that we can send a message back to the PC that tells the user when a piano key has been pressed, and which note was pressed.  The screenshot below shows the EUSART messages that are sent when a piano key is pressed.

![](https://static.transim.com/img/52018/5227d8ee869c4966a982925805cc8632-qdbfx.png){width=auto height=auto}

## MCC Pin Manager View - PPS Setup:
The following screenshot shows the PPS settings that were configured in MCC using the Pin Manager View:
![](https://static.transim.com/img/52018/5a644baa9bb64abaa69fbe878eef4bb7-rbzz5.png){width=auto height=auto}

## Conclusion:
For more information about the PIC16F19176, or the ADCC and CVD functionality, please refer to the device datasheet linked at the bottom of this example.  In addition to the datasheet, refer to the ADCC Technical Brief and the CVD Technical brief for more information.  Comment below with any questions!`
