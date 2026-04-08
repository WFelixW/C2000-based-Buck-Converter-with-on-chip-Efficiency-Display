The programming was carried out in Code Composer Studio (CCS) with device
configuration and peripheral setup performed using the SysConfig tool integrated within CCS.
The implementation includes EPWM generation, I2C communication setup, INA219 sensor inter-
facing, OLED data display, and efficiency calculation.

SysConfig Setup
>EPWM Configuration
1. An empty project was first created for the TMS320F280049C LaunchPad under CCS. The target
configuration was verified to ensure the correct device and connection were selected. Figure 1.1
shows the workspace setup with the Buck_Efficiency.c source file.

2. The build environment was verified for proper compiler and device support. Before debugging,
the Test Connection option was used under the target configuration menu to ensure communica-
tion between the LaunchPad and CCS through the onboard XDS110 debugger.

3.The SysConfig tool was used to graphically configure peripheral modules such as EPWM and I2C.
This eliminated manual register setup and ensured correct pin assignments and synchronization
between modules.

4. The EPWM module was configured to generate a 150 kHz PWM signal with a duty cycle of
approximately 31 %. The counter mode was set to up-down for symmetric PWM generation, min-
imizing harmonic distortion. Figure 1.2 and Figure 1.3 show the SysConfig settings for EPWM.
Liao et al. (2018)
|
|
|---->The key EPWM parameters used were:
    • EPWM Period: 333 (corresponding to 150 kHz at 100 MHz clock)
    • Counter Mode: Up–Down count mode   
    • Compare Value (CMPA): 166 (31% duty)
    • Clock Divider: /1
    • Load Mode: Shadow Register for smooth updates

>I2C Configuration
1. The I2C peripheral (I2CA) was enabled in controller mode to communicate with two INA219
sensors and an OLED display. The I2C bit rate was set to 100 kbit s−1, and GPIO pins 35 (SDA)
and 37 (SCL) were mapped in SysConfig as shown in Figure 1.4.

The device operates as an I2C master transmitting and receiving data from multiple slave ad-
dresses:
  • 0x40 – Input INA219 sensor
  • 0x41 – Output INA219 sensor
  • 0x3C – OLED display module

>Code Implementation
1. After peripheral configuration, the main control program was written in C to perform sensor ini-
tialization, data acquisition, efficiency calculation, and OLED display updates. The source file
Buck_Efficiency.c contains modular functions for I2C read/write operations, INA219 data
handling, and OLED printing routines. Figure 1.5 shows the main program loop handling mea-
surement and display logic.

The core program workflow is summarized as follows:
1. Initialize peripherals and communication interfaces (EPWM, I2C, GPIO).
2. Configure and calibrate both INA219 sensors for input and output measurements.
3. Initialize the OLED display and print a startup message.
4. Continuously read input/output voltage and current values.
5. Compute converter efficiency:
η = VoutIout
    --------  X 100%.
     VinIin

6. Display real-time readings and calculated efficiency on the OLED.

>Debugging and Execution
After successful build compilation, the program was loaded into the LaunchPad using the De-
bug mode in CCS. Before flashing, the Test Connection feature verified stable communication
through the onboard XDS110 debugger. Once flashed, real-time debugging allowed monitoring
of variables, register values, and peripheral activity in the Expressions window.

>PWM Signal Verification
After uploading the program, the generated PWM signal was verified using a digital storage oscil-
loscope (DSO) at the gate driver input pin. A clean, stable PWM waveform of 150 kHz frequency
and 31 % duty cycle was observed, confirming the correct EPWM configuration. This PWM out-
put served as the control signal for the IR2104 gate driver in the buck converter.

>Final System Operation
Upon successful code execution, the system continuously displayed:
• Input voltage and current (Vin, Iin)
• Output voltage and current (Vout, Iout)
• Calculated efficiency (η)
on the OLED display at an update rate of approximately 2 Hz. The readings were cross-
checked with multimeter and DSO measurements for validation, confirming correct operation of
both software and hardware modules.


  
