# STM32F405-feather-hello_world
Example of how to program the STM324F05 feather express board to blink an LED and print to serial with STM32CubeIDE on Ubuntu  
This is just an example from loosely following this [guide](https://www.digikey.co.nz/en/maker/projects/making-a-temperature-logger-with-the-adafruit-feather-stm32f405-express/11ea860d54074a19bb75cb6425e6d0b0) from Shawn Hymel on how to use the board.  
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
The source builds in it's current state so make sure it also builds on your machine too.  
You need to get binaries from the compilation to burn to the board:  
- Right click teh project file
- Select `Properties`
- Go to `C/C++ Build > Settings > Tool Settings > MCU Post build outputs`
- Enable **Convert to binary file (-O binary)**  

Build the project

## Burn the executable
Ther are many ways to burn an executable to the board. Following the guide, we will use [DFU Utilities](), this as an open source tool avaialable for ubuntu users.  
before using dfu-util to burn the binary file however, the STM32 needs to be manually entered into bootloader mode. To do this, bull the B0 pin low (connect it to GND) and press the enter button.  

Naviagate to your workspace directory, then go into the `Debug` folder  
Open terminal here (or navigate to this directory, either works)  
run the following:  
`dfu-util -a 0 -s 0x08000000:leave -D <BINARY_NAME>.bin`
Becuase our binary is called `feather-setup.bin`, the command we will run is:  
`dfu-util -a 0 -s 0x08000000:leave -D feather-setup.bin`
Sometimes you will get an error saying there isn't a device to burn to.  
Try running: `dfu-util` If there is a bootloader mode STM32 you should get 3 entries. If not just pull B0 pin low and hit the reset button firmly a few times, this will eventually work.

## See what it's saying on terminal  
Minicom is good for this (on ubuntu).  
https://help.ubuntu.com/community/Minicom  
install it of course:  
`sudo apt install minicom`  
Then:  
`dmesg | grep tty`  
This is running the *Diagnostic Message* command  - which shows us a list of active/running device drivers on our machine. Then the `grep tty` filters the output of just `dmesg` for only entries containing 'tty'. This is a feature of how device drivers work in Linux operating systems. If you;re curious about this check out some resources [here]() and [here]()   
  
Anyway! On my device it comes up as `/dev/ttyACM0`  
    
Enter `sudo minicom -s` To configure minicom. We want to tell it which serial port we want to look at, the baud rate, flow control (is it hardware or software?), how many data and stop bits in a stream, etc.  
  
Once that's all configured, hit A, and write the address to reflect what you just found with grep above.  
exit that, exit again (but not 'exit minicom')  
Bang! you should be getting a lovely message from the terminal!  
  
## Some Notes and Points I wish I knew:
- The Ardunio framework for the STM32F405 feather board currently lacks capability for the board to *enumerate* a serial port with a linux machine (as for Windows, I'm not sure - I haven't tried).  
Essentially, this means you can't get your STM32 to print to a serial port in a way where your machine *can actually see it*, your machine simpy won't see the device. Running `dmesg | grep tty` won't return anything.

