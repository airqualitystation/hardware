# To do list :

Because of government decisions following the COVID-19 health crisis we did not have time to electrically characterize our project (no equipment)

Here some interesting ideas of improvement for the next team :  

* Thanks our homemade multimeter, it could be interesting to carry out testbench's for the impact of the LoRa code on the consumption. (Spreading factor, duty cycle ...). And see the famous "Low power comunication"


* Study the MPPT energy efficiency to set a power consumption analysis more correct.  

* Find a chip reference which provide you the battery autonomy as output (I2C or UART) in relation to the temperature, the current and the voltage. This kind of component is currently used in smartphone and is more complexe than our system. If you do that, you'll need to implement a driver libray to manage the sensor with RIOT according the communication way between the board and the device (as we have done with the PMS7003 driver though an UART interface)
