
PROJ = minimal
TOP = top

V_FILES = $(TOP).v

DEVICE = 1k
FOOTPRINT = tq144

I_DIR = ./ice_dir
V_DIR = ./obj_dir


.PHONY: all sim bin clean burn world timings

all: sim bin

sim: $(V_DIR)/V$(TOP)
bin: $(I_DIR)/$(PROJ).bin

$(I_DIR)/$(PROJ).asc: $(V_FILES) pinmap.pcf
	mkdir -p $(I_DIR)
	yosys -p "synth_ice40 -top $(TOP) -blif $(I_DIR)/$(PROJ).blif" $(V_FILES)
	arachne-pnr -d $(DEVICE) -P $(FOOTPRINT) -o $(I_DIR)/$(PROJ).asc -p pinmap.pcf $(I_DIR)/$(PROJ).blif

$(I_DIR)/$(PROJ).bin: $(I_DIR)/$(PROJ).asc
	icepack $(I_DIR)/$(PROJ).asc $(I_DIR)/$(PROJ).bin

burn: $(I_DIR)/$(PROJ).bin
	iceprog $(I_DIR)/$(PROJ).bin


timings: $(I_DIR)/$(PROJ).asc
	icetime -tmd hx$(DEVICE)  $(I_DIR)/$(PROJ).asc


$(V_DIR)/V$(TOP): sim_$(TOP).cpp $(V_FILES)
	verilator -Wall --Mdir $(V_DIR) --cc -CFLAGS "-g" --trace $(TOP).v --exe sim_$(TOP).cpp
	make -j -C $(V_DIR) -f V$(TOP).mk V$(TOP)


clean:
	rm -rf $(I_DIR) $(V_DIR)

world: clean all
