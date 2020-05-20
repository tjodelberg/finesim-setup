==============================================================================================
==============================================================================================
Finesim Flow by Trevor Odelberg. Last Updated: April 2020.
==============================================================================================
==============================================================================================

Finesim is a fast transient simulator by Synopsys. It is run from the command line.
It can be much faster than spectre sims as it allows mutiple core checkout. 

All you have to do is save your netlist in hspice format to this directory and run the Makefile.

==========
IMPORTANT: If you are going to check out all the cores, be nice and coordinate with others to make sure
no one is using your server. 
==========

==============================================================================================
DIRECTORY STRUCTURE
==============================================================================================
/finesim		 -> Home
/finesim/run		 -> Run logs
/finesim/.cshrc_setup_fs -> Load Enviroment
/finesim/README 	 -> Instructions

==============================================================================================
GUIDE
==============================================================================================

=======================
How to setup netlist:
=======================
1. Open ADE and switch simulator to hspiceD.
2. Set up your test and export hspiceD netlist. Make sure sim runs correctly in ADE using hspiceD before exporting (set small timescale to test).

 	Tips:
	-Double check all analogLib cells you are using have an hspiceD view (some PDKs dont provide)
	-Double check models are hspice
	-Only transient sim supported as far as I know
	-Some blocks don't export correctly for hspice depending on PDK (such as Delay block in TSMC 28)
        -hspiceD does not let you save nets that are in internal cells. So plan testbenches accordingly.

3. Make and Save the netlist into this directory (/finesim)  as  "NAME.ckt" , where NAME can be your choice.

=======================
How to run finesim:
=======================
1. Source the .cshrc_fs_setup to load the correct setup. Only certain versions of Finesim work. Do this once.
2. Edit the Makefile to point to your .ckt name. Then type "make" to run.

	make ->
		-Edits the netlist to add finesim run settings
		-Runs finesim using 7 core checkout
		-Cleans run directory
		-Organizes output files and creates a warnings.log file

Wave files "psf" and "tr0" will be created if successfull. "warnigs.log" contains any runtime warnings copied to one file. 

=======================
3. To view waves
=======================
	-make viva -> opens wave in viva (cadence viewer). Uses psf format.
	-make wv   -> opens waves in wave view (custom-explorer). Uses tr0 format


==============================================================================================
TIPS AND NOTES:
==============================================================================================

        1. Each PDK has quirks when it comes to exporting hspice netlist accuratly. This can take sometime to debug.
 
	2. Accuracy: For long ADC sims on high resultion ADCs (+10bits), I have noticed the normal finesim simulator
	is not accurate enough (degrades SNDR). In most cases the accuracy is fine however. You can change the 
        finesim_mode in Makefile to increase resultion with a speed tradeoff. 
       
        For my test sim (10 cycles of 10 bit ADC), here are the speed times for each setting.

        #Fastest
       
        -proxd		11 sec
	-promd 		12 sec
        -prohd		21 sec
	-spicexd	31 sec
        -spicemd	31 sec
        -spicehd	73 sec

        #Accurate
   
        I usually use promd and only use spicemd if high accuracy is required.
       
	3. Core Checkout: You can see how many cores are in your CPU by running cat /proc/cpuinfo. Do not check out
	more than you have or sims will actually run slower. I have also noticed that decreasing core count can lead
        to faster sims. For example, for Calhoun (8 Cores),  it actually runs slightly faster at 7 core checkout.

 	4. Addtional resources can be found in /usr/caen/finesim-2017.03-SP1-1/



