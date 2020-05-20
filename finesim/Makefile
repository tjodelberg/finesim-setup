#Written by Trevor Odelberg 2020

#Only change here:

target := YOUR_NAME_HERE

#No need to change below:

all: append run_fs clean organize

append: 
	perl -i -pe 's/PSF=2/PSF = 2\n+    finesim_output=psf\n+      finesim_mode=promd\n+    finesim_mcbrief=0\n\n/g;' $(target).ckt
run_fs: 
	finesim -np 7 $(target).ckt
	echo SIM END
clean:
	rm -r ./run/*
organize:
	grep -ir --include $(target).log warning > $(target).warning.log
	mv $(target).log ./run
	mv $(target).ic  ./run
	mv $(target).valog ./run
	mv $(target).pvadir ./run
	mv *.log* ./run

viva:
	viva -datadir $(target)_psf/ &
wv:
	wv $(target).tr0 &

