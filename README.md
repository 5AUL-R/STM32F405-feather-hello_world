# STM32F405-feather-hello_world
Example of how to program the STM324F05 feather express board to blink an LED and print to serial with STM32CubeIDE on Ubuntu  
This is just an example from loosely follow this [guide](https://www.digikey.co.nz/en/maker/projects/making-a-temperature-logger-with-the-adafruit-feather-stm32f405-express/11ea860d54074a19bb75cb6425e6d0b0) from Shawn Hymel on how to use the board  
The directory "feather-setup" should be placed in your STM32CubeIDE workspace for this to work
# Quick points
## Setup/configure the board in IDE
Configure I/O  
This is the bare setup for the feather board  
- External crystal  

Here's what I added for this project:  
- serial comms
- swd (for future use)
- I2C 
- GPIO for `RED_LED`
- 
Then configure the clock:
- Set the Input Frequency to 12 (MHz)
- Click to change the PLL Source Mux input to HSE
- Click to change the System Clock Mux input to PLLCLK
- Change the HCLK number to 168 (MHz)

## Add the source code 
- see files
## Build and compile
the source builds in it's current state so make sure it also builds on your machine too.  
You need to get binaries from the compilation to burn to the board:  
- Right click teh project file
- Select `Properties`
- Go to `C/C++ Build > Settings > Tool Settings > MCU Post build outputs`
- Enable **Convert to binary file (-O binary)**  

Build the project

## Burn the executable
Naviagate to your workspace directory, then go into the `Debug` folder  
Open terminal here (or navigate to this directory, ethier works)  
run the following:  
`dfu-util -a 0 -s 0x08000000:leave -D feather-setup.bin`  
This should work

## See what it's saying on terminal  
Minicom is good for this (on ubuntu)  
https://help.ubuntu.com/community/Minicom  
install it of course:  
`sudo apt install minicom`  
Then:  
`dmesg | grep tty`  
On my device it comes up as `/dev/ttyACM0`  
So;  
`sudo minicom -s` TO configure it  
Hit A, and write the address to reflect what you just found with grep above.  
exit that, exit again (but not 'exit minicom')  
Bang! you should be getting a lovely message from the terminal!

