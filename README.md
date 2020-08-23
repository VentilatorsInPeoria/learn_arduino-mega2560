# learn_arduino-mega2560
Workspace to learn about Arduino Mega 2560 (r3)
## Useful Links
- https://blog.arduino.cc/2020/03/13/arduino-cli-an-introduction/
	- https://arduino.github.io/arduino-cli/latest/installation/
	- https://arduino.github.io/arduino-cli/latest/getting-started/
- https://blog.arduino.cc/2019/11/14/arduino-on-github-actions/

## Getting Set Up:
	git clone https://github.com/jameslewellyn/learn_arduino-mega2560.git
	cd learn_arduino-mega2560/
	git submodule update --init --recursive
	git config submodule.arduino-cli.ignore all
	cd arduino-cli/
	./install.sh
	cd ../
	export PATH="$(readlink -f ./arduino-cli/bin):${PATH}"

## Initializing a Project:
	mkdir my-project
	cd my-project/
	git init
	arduino-cli config init
	arduino-cli sketch new MyFirstSketch
	arduino-cli core update-index
	# Connect board through USB
	arduino-cli board list
	# Port         Type              Board Name                FQBN             Core
	# /dev/ttyACM0 Serial Port (USB) Arduino Mega or Mega 2560 arduino:avr:mega arduino:avr
	arduino-cli core install arduino:avr
	# ...
	# ...
	# arduino:avr@1.8.3 installed
	arduino-cli core list
	# ID          Installed Latest Name
	# arduino:avr 1.8.3     1.8.3  Arduino AVR Boards

## Compiling and Running a Project
Edit ./MyFirstSketch/MyFirstSketch.ino to contain the following:

	void setup() {
		pinMode(LED_BUILTIN, OUTPUT);
	}

	void loop() {
		digitalWrite(LED_BUILTIN, HIGH);
		delay(1000);
		digitalWrite(LED_BUILTIN, LOW);
		delay(1000);
	}

Now run the following:

	arduino-cli compile --fqbn arduino:avr:mega MyFirstSketch
	# Sketch uses 1536 bytes (0%) of program storage space. Maximum is 253952 bytes.
	# Global variables use 9 bytes (0%) of dynamic memory, leaving 8183 bytes for local variables. 	Maximum is 8192 bytes.
	arduino-cli upload -p /dev/ttyACM0 --fqbn arduino:avr:mega MyFirstSketch
	# You should now see the LED blinking at 1 second on, 1 second off intervals

## Behind the Scenes Commands Used:
	# Used to create this guide's repo:
	git submodule add -b master -f https://github.com/arduino/arduino-cli.git
	# Needed to access serial ports without root permissions:
	sudo adduser "$USER" dialout
	newgrp dialout
