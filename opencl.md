# OpenCL #

## With NVidia Graphic Card ##

The first hudle to take is to get approval for the login for the NVidia Open CL developer program. It took two weeks.

### Install the driver ###

	sudo /sbin/init 3
	chmod u+x devdriver_3.1_linux_64_258.19_opencl1.1.run
	./devdriver_3.1_linux_64_258.19_opencl1.1.run
	sudo /sbin/init 5

### Install the SDK ###

	sh gpucomputingsdk_1_1_beta_linux.run

### Try ###

	cd ~/NVIDIA_GPU_Computing_SDK/OpenCL
	make

I ran into problems 

	./../../../x86_64-suse-linux/bin/ld: cannot find -lGLU 

I just started up yast and install the freeglut-devel package
