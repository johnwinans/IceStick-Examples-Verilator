# IceStick-Examples

This project is a simple application for testing the programming of 
an IceStick on a Linux platform.  It has been verified on a 
PI-4B running Rasbian.

# Install prerequisites

    sudo apt install build-essential clang bison flex libreadline-dev \
                    gawk tcl-dev libffi-dev git mercurial graphviz   \
                    xdot pkg-config python python3 libftdi-dev


# Download the tools

    git clone https://github.com/cliffordwolf/icestorm.git
    git clone https://github.com/cseed/arachne-pnr.git
    git clone https://github.com/cliffordwolf/yosys.git


# Build and install everything (this will litter /usr/local)

    PROCS=3

    cd icestorm
    make -j$PROCS
    sudo make install
    cd ..

    cd arachne-pnr
    make -j$PROCS
    sudo make install
    cd ..

    cd yosys
    make -j$PROCS
    sudo make install
    cd ..


# Install verilator if you want to run Verlog simulation tests

	sudo apt install verilator gtkwave

# Copy this repo to your PI, build, and run blinky

Plug the IceStick into a USB port and then run the following from this cloned repo:

	git clone https://github.com/johnwinans/IceStick-Examples.git
	cd IceStick-Examples/blinky
	make
	make burn

# To run verilator to simulate blinky and view a waveform of the counter

	cd IceStick-Examples/blinky
	make Vtop
	obj_dir/Vtop -t
	gtkwave wave.vcd

In gtkwave, open the 'TOP' object in the tree, click on 'TOP->top', select 'clk'
and 'counter[24:0]' in the signals box below, and click the Append button at the bottom left.
At this polint, you can zoom and scroll around the waveform on the right to see the clock
signal ticking and the counter advancing on the rising/positive edge of the 'clk' signal.
