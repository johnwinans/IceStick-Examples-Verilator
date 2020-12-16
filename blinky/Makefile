
PROJ      = blinky
BUILD     = ./build
DEVICE    = 1k
FOOTPRINT = tq144

FILES = top.v

.PHONY: all clean burn

all: $(BUILD)/$(PROJ).bin

$(BUILD)/$(PROJ).bin: top.v pinmap.pcf
	mkdir -p $(BUILD)
	yosys -p "synth_ice40 -top top -blif $(BUILD)/$(PROJ).blif" $(FILES)
	arachne-pnr -d $(DEVICE) -P $(FOOTPRINT) -o $(BUILD)/$(PROJ).asc -p pinmap.pcf $(BUILD)/$(PROJ).blif
	icepack $(BUILD)/$(PROJ).asc $(BUILD)/$(PROJ).bin

burn: $(BUILD)/$(PROJ).bin
	iceprog $(BUILD)/$(PROJ).bin

clean:
	rm -rf build
