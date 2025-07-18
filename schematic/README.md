# Schematic description

The description of this scheme begins with the power supply circuit. The power supply circuit receives a voltage of 9V from a battery and is stabilized at 5V. This voltage is agreed upon by the components and cannot affect the integrated circuits on the board.

<br>To 
build this circuit, a 7805 type voltage stabilizer is used. Considering the planned consumption of the circuit, a 7805 in a TO220 package is used, which can withstand a current of up to 1A. To avoid incorrect connection of the power supply, a 1N4001 diode (or another diode from the 1N4002, 1N4003, ..., 1N4007 series) is 
installed at the input of the stabilizer. Capacitors C1 (100nF) and C2 (100µF) filter the input voltage, and C3 (100nF) and C4 (100µF) clean the output voltage. Electrolytic capacitors eliminate low-frequency components, while ceramic ones eliminate 
high-frequency components. A green LED indicates the presence of 5V output voltage, and the current through LED is limited by resistor R1 of the order of hundreds of ohms.

<br>The clock signal is used to simulate the actual clocks for cars entering and leaving the parking lot. As you can see, these are taken to a pin strip. This happens because from the pin strip these signals must 
reach the external Arduino board. Arduino, by programming it, will give a signal through that strip that will reach the counter and it will decrease or add to the 
respective number. This clock signal is connected to the down and up pins of the counter, which have an important role in counting the free parking spaces. 
Power on reset with Schmitt Trigger is used to ensure a correct and stable reset to the counters at startup, to avoid improper operation or initial errors. When the system is turned on, the input voltage 
increases gradually. When this voltage exceeds the upper level of the Schmitt trigger, its output switches to its opposite logic state (for example, from 1 to 0). The signal is passed through its opposite logic state twice through the CD40106BE and 74HC14, then it is fed to the "LOAD" signal which must be at logic 0.

<br>The counting part is handled by two successive 74HC192 decade counters. In order for the counting of parking spaces to start from the maximum number that these counters support, they will be initialized with 9. Therefore, the displayed number is 99. This is possible by initializing the A, B, C, D pins with the number 9 in binary, that is, 1001. In order for the initialization to be done for the logical 1 number, the pin will be added to 5V, and for the logical 0, the pin will be added to GND. The initialization is done on both counters. The sequence of these should make it possible to connect the BO and CO pins of the next counter to the down and up pins. Let's not forget that all connections to 5V or GND will go through a 680Ω resistor to be sure that the integrator pins will not be affected in case of a design error. Through the 
pins QA, QB, QC, QD are the outputs of the counters that will be taken to the input of the decoder. Apart from the Power on Reset, I have also added a reset button that connects to the counter and can provide a reset without removing the circuit from the 
power supply. but was added only for this safety on reset.<br>

The display of the number given by the counter is decoded by the CD4543BE. 
It receives a signal from the counter and passes on the signal through the outputs from A to F to the 7-segment display. Another 16-pin header was added, because it is not wanted to physically put it on the board. This number is intended to be put on the 3D design, 
because I want it to be more legible for people to read. In order to make the display possible, a 680Ω resistor was added to each signal coming from the CD4543BE to the 7-segment display to limit the current passing through these LEDs and not to damage anything.
