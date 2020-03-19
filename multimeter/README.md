# Multimeter details :

Thanks to USB Energetic spy’s project at Polytech Grenoble, we measured the consumption of
our system during 1 hour. Datas was exported in an Excel file for analysis. Here take look at the current chart directly pull from the battery :   

![Air Quality Station 2020](https://raw.githubusercontent.com/airqualitystation/hardware/master/images/current_chart.png)
<p align="center">
  <i>The current of the system as a function of time </i>
</p>

We wanted do the same and monitoring in real time the project current and send the consumption by LoRa. In addition we wanted to have a rough idea of ​​the state of the battery in order to make the upkeep easy.  

## Hardware requirements
* [Analog digital converter ADS1115](https://www.adafruit.com/product/1085)  

For the LoRa board project we use, there is no implementation of ADC driver on RIOT OS. But also usually ADC are only 12 bits converter on STM32. This ADS115 has 16 bits and it even includes a programmable gain amplifier, up to x16, to help boost up smaller single/differential signals to the full range.  

The higher gain allows us to measure an input range of +/-0.256V with 16 bit resolution it's just great for our purpose to monitoring small voltage drops for the current through the resistor.  

**See the source code to manage the ADS1115 in the examples folder of the firmware repository.**  

## Voltage level as indicator of system autonomy
The cheapest and easiest way to check the battery state is to use the theorical level indicator of a lipo 3.7 V battery. here the table :  

![Air Quality Station 2020](https://raw.githubusercontent.com/airqualitystation/hardware/master/images/voltage_level_indicator.png)
<p align="center">
  <i> Voltage level indicator (lipo 3.7V) </i>
</p>


* You just need to connect the MPPT positif input with the ADS1115 channel of your choice.

## Voltage and Current measurement in real time 

The current is measure thanks the Ohm law through a resitor and the differential measurement with two channels of the analog digital converter.  
We put an 1 Ohm precision resistor in serial between the positif baterry output and the MPPT positif input.  

We know thanks all datasheets sensors and board that the current is approximatly under the 200 mA. Therefore we can't selected a better precision resistor because the voltage drop would have been too low for the input channel of the ADS1115 (higher gain +/-0.256 V).  
With the resolution of 16 bits ADC that offer the ADS1115, we can precisely measure at the tenth of a micrometer.  

**It works ! Great** But the resistor bring a voltage drop that is not fit to the power supply because the MPPT stop run his boost mode if the battery is empty (voltage level indicator). So the supply could be stop whereas the battery state is not down.  

### Solution

To avoid the issue mentioned above, we use a Pmos Transistor as a programable switch by the LoRa board. So users can choice when they want take a picture of the current.  

The Pmos need the lowest Drain-Source resistor as possible to limit the drop voltage but it also need to be operated from 0V to 3.3V according by the GPIO output voltage.  

by searching on the component sellers website we find one wich fit well design [Pmos NDP6020P](https://www.onsemi.com/products/discretes-drivers/mosfets/ndp6020p)  


We have give you a _**Ltspice**_ simulation with a the NDP6020P spice model , if you want check. (see the coresponding folder)  

```
if Gate=0v then Pmos is closed
if Gate=3.3v then Pmos is open
```
 
### Here some Ltspice pictures : 


![Air Quality Station 2020](https://raw.githubusercontent.com/airqualitystation/hardware/master/images/Pmos_closed.png)
<p align="center">
  <i> Pmos closed (only 7.5 mV drop)</i>
</p>


![Air Quality Station 2020](https://raw.githubusercontent.com/airqualitystation/hardware/master/images/Pmos_open.png)
<p align="center">
  <i> Pmos open (All the current in the 1 Ohm resistor)</i>
</p>
