# ==== Environment Variables ====
#     vvvv DO NOT CHANGE vvvv
XCELIUM_BIN                := $(shell {(ls /afs/cs/es/tools/cadence/Xcelium/20.09.005/tools/bin/xrun) || (ls /afs/wsi/es/tools/cadence/Xcelium/20.09.005/tools/bin/xrun)} 2>/dev/null)
XCELIUM_ELAB_FLAGS         := -elaborate -v93 -sv -exit -access +rw -timescale '1ps/1ps'
XCELIUM_RUN_FLAGS          := -access +rw -R -snapshot run -v93 -timescale '1ps/1ps'
SIMVISION_BIN              := $(shell {(ls /afs/cs/es/tools/cadence/Xcelium/20.09.005/tools/bin/simvision) || (ls /afs/wsi/es/tools/cadence/Xcelium/20.09.005/tools/bin/simvision)} 2>/dev/null)
GENUS_BIN                  := $(shell {(ls /afs/cs/es/tools/cadence/GENUS/20.10.000/bin/genus) || (ls /afs/wsi/es/tools/cadence/GENUS/20.10.000/bin/genus)} 2>/dev/null)
GENUS_FLAGS                := -common_ui
SYN_LIB_FILE               := $(shell {(ls /afs/cs/es/pdks/nangate/NanGate_45nm_2010.12/Front_End/Liberty/CCS/NangateOpenCellLibrary_typical_ccs.lib) || (ls /afs/wsi/es/pdks/nangate/NanGate_45nm_2010.12/Front_End/Liberty/CCS/NangateOpenCellLibrary_typical_ccs.lib)} 2>/dev/null)
POST_SYN_GATE_FILE         := 
POST_SYN_GATE_FILE         := $(shell {(ls /afs/wsi/es/pdks/nangate/NanGate_45nm_2010.12/Front_End/Verilog/NangateOpenCellLibrary.v) || (ls /afs/wsi/es/pdks/nangate/NanGate_45nm_2010.12/Front_End/Verilog/NangateOpenCellLibrary.v)} 2>/dev/null)
LM_LICENSE_FILE            := $(shell ((dig +noall +answer menelaos.informatik.uni-tuebingen.de | wc -c)>0) && echo "1701@menelaos.informatik.uni-tuebingen.de" || echo "1701@menelaos.cs.uni-tuebingen.de")
export W3264_NO_HOST_CHECK := 1
export LM_LICENSE_FILE     := $(LM_LICENSE_FILE)
.PHONY: help elaborate sdc xconf post_xconf env clean
#     ^^^^ DO NOT CHANGE ^^^^

# ==== Design Files ====
#     vvvv CHANGE vvvv
# List source files here (not the testbench!).
VHDL_SRC     := TODO
# Put testbench file here!
VHDL_TB_SRC  := TODO
# Top module name Synthesis.
VHDL_SYN_TOP := TODO
# Top module name Simulation (Testbench).
VHDL_TB_TOP  := TODO
# Name of the VCD file which is generated.
VCD_FN       := TODO
#     ^^^^ CHANGE ^^^^

help:
	@echo " ___  ___  ___  ___   _  _       _                ® "
	@echo "| __|/ __|| __|/ __| | || | ___ | | _ __  ___  _ _  "
	@echo "| _| \__ \| _| \__ \ | __ |/ -_)| || '_ \/ -_)| '_| "
	@echo "|___||___/|___||___/ |_||_|\___||_|| .__/\___||_|   "
	@echo "                                   |_|              "
	@echo "This scripts is part of the ESES lecture offered at "
	@echo "the University of Tuebingen. The script is provided "
	@echo "'AS IS', without warranty of any kind. Students are "
	@echo "allowed to modify the scripts to their needs but    "
	@echo "have to ensure correct functionality themselves.    "
	@echo "NOTE: MODIFIED SCRIPTS MIGHT LEAD TO WRONG BEHAVIOR!"
	@echo "                                                    "
	@echo "Help:                                               "
	@echo "=====                                               "
	@echo "      make help           Display this help message."
	@echo "                                                    "
	@echo "      ======= Pre-Synthesis Simulation =======      "
	@echo "                                                    "
	@echo "      make elaborate      Elaborate the given design"
	@echo "                          files and testbench.      "
	@echo "                                                    "
	@echo "      make xconf          Create Xcelium tcl command"
	@echo "                          file.                     "
	@echo "                                                    "
	@echo "      make sim            Elaborate and Simulate the"
	@echo "                          the given design and test-"
	@echo "                          bench files.              "
	@echo "                                                    "
	@echo "      make sim_gui        Elaborate and Simulate the"
	@echo "                          the given design and test-"
	@echo "                          bench files with GUI.     "
	@echo "                                                    "
	@echo "              ======= Synthesis =======             "
	@echo "                                                    "
	@echo "      make sdc            Create simple synthesis   "
	@echo "                          constraint file.          "
	@echo "                                                    "
	@echo "      make gconf          Create Genus tcl command  "
	@echo "                          file.                     "
	@echo "                                                    "
	@echo "      make synthesis      Synthesis the given design"
	@echo "                          files.                    "
	@echo "                                                    "
	@echo "      ======= Post-Synthesis Simulation =======     "
	@echo "                                                    "
	@echo "      make post_xconf     Create Xcelium post-syn   "
	@echo "                          tcl command file.         "
	@echo "                                                    "
	@echo "      make post_sim       Post-Synthesis simulation "
	@echo "                          of the synthesized design."
	@echo "                                                    "
	@echo "      make post_sim_gui   Post-Synthesis simulation "
	@echo "                          of the synthesized design."
	@echo "                          with GUI."
	@echo "                                                    "
	@echo "                 ======= Misc =======               "
	@echo "                                                    "
	@echo "      make env            Create a file containing  "
	@echo "                          environment variables.    "
	@echo "                          Compatible with bash, zsh!"
	@echo "                                                    "
	@echo "      make clean          Delete all files generated"
	@echo "                          by other targets.         "

elaborate: 
	@echo "-- Elaborate"
	$(XCELIUM_BIN) $(XCELIUM_ELAB_FLAGS) -top $(VHDL_TB_TOP) $(VHDL_SRC) $(VHDL_TB_SRC)

xconf:
	@echo "-- TCL Command File for Xcelium"
	@echo "database -open vcddb -into $(VCD_FN) -vcd -default" >  xconf.tcl
	@echo "probe -create -vcd -all -depth all -database vcddb" >> xconf.tcl

sim: elaborate xconf
	@echo "-- Simulation"
	@echo "run"  >> xconf.tcl
	@echo "exit" >> xconf.tcl
	$(XCELIUM_BIN) $(XCELIUM_RUN_FLAGS) -input xconf.tcl

sim_gui: elaborate xconf
	@echo "-- Simulation with GUI"
	@PATH=$(SIMVISION_BIN):$(PATH)
	$(XCELIUM_BIN) $(XCELIUM_RUN_FLAGS) -input xconf.tcl -gui

sdc:
	@mkdir -p synthesis
	@echo "-- SDC Constraint Files for GENUS"
	@echo "create_clock -name CLK -period $(PERIOD)"    >  ./synthesis/constraints.sdc
	@echo "set_input_delay -clock CLK 0 [get_ports *]"  >> ./synthesis/constraints.sdc
	@echo "set_output_delay -clock CLK 0 [get_ports *]" >> ./synthesis/constraints.sdc

gconf: sdc
	@echo "-- TCL Command File for GENUS"
	@echo "read_libs $(SYN_LIB_FILE)"                                     >  ./synthesis/gconf.tcl
	@echo "read_sdc ./synthesis/constraints.sdc"                          >> ./synthesis/gconf.tcl
	@echo "read_hdl -language vhdl $(VHDL_SRC)"                           >> ./synthesis/gconf.tcl
	@echo "elaborate $(VHDL_SYN_TOP)"                                     >> ./synthesis/gconf.tcl
	@echo "syn_generic"                                                   >> ./synthesis/gconf.tcl
	@echo "syn_map"                                                       >> ./synthesis/gconf.tcl
	@echo "syn_opt"                                                       >> ./synthesis/gconf.tcl
	@echo "report_gates  > ./synthesis/report_gate.rpt"                   >> ./synthesis/gconf.tcl
	@echo "report_area   > ./synthesis/report_area.rpt"                   >> ./synthesis/gconf.tcl
	@echo "report_timing > ./synthesis/report_timing.rpt"                 >> ./synthesis/gconf.tcl
	@echo "report_timing -unconstrained > ./synthesis/report_utiming.rpt" >> ./synthesis/gconf.tcl
	@echo "write_hdl $(VHDL_SYN_TOP) > ./synthesis/syn_$(VHDL_SYN_TOP).v" >> ./synthesis/gconf.tcl
	@echo "exit"                                                          >> ./synthesis/gconf.tcl 
	
synthesis: gconf
	@echo "-- Synthesis"
	$(GENUS_BIN) $(GENUS_FLAGS) -files ./synthesis/gconf.tcl -log ./synthesis/genus
	@rm -rf ./fv ./synthesis/fv


post_elaborate: synthesis
	@echo "-- Pre-Synthesis Elaborate"
	$(XCELIUM_BIN) $(XCELIUM_ELAB_FLAGS) -top $(VHDL_TB_TOP) $(POST_SYN_GATE_FILE) ./synthesis/syn_$(VHDL_SYN_TOP).v $(VHDL_TB_SRC)

post_xconf:
	@echo "-- TCL Command File for Xcelium Post-Synthesis"
	@echo "database -open vcddb -into post_$(VCD_FN) -vcd -default" >  post_xconf.tcl
	@echo "probe -create -vcd -all -depth all -database vcddb"      >> post_xconf.tcl

post_sim: post_elaborate post_xconf
	@echo "-- Simulation Post-Synthesis with GUI"
	@echo "run"  >> post_xconf.tcl
	@echo "exit" >> post_xconf.tcl
	$(XCELIUM_BIN) $(XCELIUM_RUN_FLAGS) -input post_xconf.tcl

post_sim_gui: post_elaborate post_xconf
	@echo "-- Simulation Post-Synthesis with GUI"
	$(XCELIUM_BIN) $(XCELIUM_RUN_FLAGS) -input post_xconf.tcl -gui

env:
	@echo "-- Environment File"
	@echo "export LM_LICENSE_FILE=$(LM_LICENSE_FILE)"        >  env.sh
	@echo "export PATH=$$(dirname $(SIMVISION_BIN)):$$PATH)" >> env.sh
	@echo "export W3264_NO_HOST_CHECK=1"                     >> env.sh

clean:
	@echo "-- Clean"
	@rm -rf *.vcd \
            *.histroy \
            xcelium.d \
            xrun.* \
            *.key \
            env.sh \
            xconf.tcl \
            post_xconf.tcl \
            syn_*.v \
            *.rpt \
            constraints.sdcs \
            gconf.tcl \
            genus.* \
            .*.tstamp \
            fv \
            synthesis \
            *.shm \
            .simvision \
            *.diag

