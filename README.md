# systemC
systemC projects

Steps to setup systemC on your Mac OS 10.14 

1. Install Apple's "Command Line Tools"

	You have two options: install Xcode (a big download), or just the command line tools (a much smaller download). 
	If your goal is simply building SystemC applications at the command line, then I recommend the latter.

	Install Apple's "Command Line Tools" by launching Terminal, entering

	$ xcode-select --install
	then clicking Install. After that, you'll have make, clang and more available at the command line. 

2. Build and install Accellera's SystemC implementation

	Download the latest release from the Accellera Downloads page (annoyingly, you'll have to provide a few personal details) and extract the contents of the .zip file.

	I like to keep a copy of the SystemC source code available, because it can be useful for debugging or understanding how something works. Therefore, I move the extracted folder (systemc-2.3.1) in		to ~/Work/Other. That's where I keep source code for third party libraries. However, you can put it wherever you like.

	Open Terminal, change into the extracted folder (systemc-2.3.1), and execute:

	$ mkdir build
	$ cd build
	$ export CXX=clang++
	$ ../configure --with-arch-suffix=
	$ make install
	The --with-arch-suffix= option prevents a -macosx64 suffix being add to the lib folder name, allowing your build scripts to be simpler.

	After that process, the salient include and lib folders should be available within the systemc-2.3.1 folder.


3. 	Build "Hello World" systemC application.

	copy the below code in a hello_world.cpp file.

	// All systemc modules should include systemc.h header file
	
	#include "systemc.h"
	// Hello_world is module name
	SC_MODULE (hello_world) {
	  SC_CTOR (hello_world) {
	    // Nothing in constructor 
	  }
	  void say_hello() {
	    //Print "Hello World" to the console.
	    cout << "Hello World.\n";
	  }
	};

	// sc_main in top level function like in C++ main
	int sc_main(int argc, char* argv[]) {
	  hello_world hello("HELLO");
	  // Print the hello world
	  hello.say_hello();
	  return(0);
	}


	To compile and run above systemc "hello world" program.

	export SYSTEMC_HOME=path/to/systemc-2.3.1
	g++ hello_world.cpp -o hello_world -L $SYSTEMC_HOME/lib  -I $SYSTEMC_HOME/include/ -l systemc 		
	if no error compiling:
        ./hello_world
