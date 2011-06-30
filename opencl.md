# OpenCL #

> OpenCL is an open industry standard for programming a heterogeneous collection of CPUs, GPUs and other discrete computing devices organized into a single platform.	It is more than a language. OpenCL is a framework for parallel programming and includes a language, API, libraries and a runtime system to support software development. Using OpenCL, for example, a programmer can write general purpose programs that execute on GPUs without the need to map their algorithms onto a 3D graphics API such as OpenGL or DirectX.

[p. 21][#Khronos:2011].

> Harnessing the Computational power of GPUs enables high performance/watt and performance/

[p. 6][#AMD:2010].

## Concepts ##

A **host** is connected to one or more OpenCL **devices** (collection of one or more *compute units* (arguably cores)). A system can offer multiple devices. Pick the best device for your algorithm. To execute code on these devices the developer writes kernels in OpenCL (a sort of C) and a host program to control the environment (in C or C++).

Think of the **kernel** as the entry points to the device program. They are the only functions that can be called from the host.

When writing a kernel you will have to think about the execution model. The code you write will be executed by many (many, many) different threads with each thread executing the same code but handling different data (**SIMD**: Single instruction, multiple data).

Every time a kernel is launched lots of **work items** are launched. A work item is the smallest execution entity. Each work-item has an ID, which is accessible from the kernel, and which is used to distinguish the data to be processed by each work-item.

You can define local **work groups** to allow communication and cooperation between work-items (they share memory). Each has their own ID and using events can be used organized and synchronize the work items,.

Work groups are organized through the *ND-Range*, an abstract organization layer.

In order to get work done on your device(s) you have to do **Explicit Memory management**. You must move data from host to global, from global to local and back again.

Work is than submitted to a **Command Queue**. Each device must have queue.

## Paradigms ##

OpenCL isn't hard. The API is easy enough. The hardware is there and it will only get more powerful over time. The hard part is rewriting algorithms to work in parallel on data. That also means rewiring your brain.

Writing code in a SIMD (Single instruction, multiple data) way can be quite challenging. You will often find yourself thinking more deeply about how to handle your data on a byte level.

OpenCL seems to handle data very goody that comes in blocks. Writing and reading from the device costs time, doing so in blocks will save you some.

## Setup ##

The first hurdle to take is to [get approval](https://registration.nvidia.com/Cuda.aspx) for the login for the NVidia Open CL developer program. (It took several weeks). Once approved you can get access to early developer tools (like the 1.1 implementation of the SDK and more current drivers).

### Unix ###

Install the driver

	sudo /sbin/init 3
	chmod u+x devdriver_3.1_linux_64_258.19_opencl1.1.run
	./devdriver_3.1_linux_64_258.19_opencl1.1.run
	sudo /sbin/init 5

Install the SDK

	sh gpucomputingsdk_1_1_beta_linux.run

Try it out

	cd ~/NVIDIA_GPU_Computing_SDK/OpenCL
	make

Basic `Makefile`:

	CXX = g++
	DIR=/home/john.doe/NVIDIA_GPU_Computing_SDK/OpenCL/common
	#----------------------------------------------------------------
	CXXFLAGS += -I$(DIR)/inc

	#----------------------------------------------------------------
	LIBS = -L$(DIR)/lib/
	LIBS += -lOpenCL

	#---------------------------------------------------------------

	OBJS = main.o

	#----------------------------------------------------------------
	TARGET = compression

	all:	$(TARGET)
	#----------------------------------------------------------------
	$(TARGET):	main.o
		$(CXX) -o $(TARGET) $(OBJS) $(LIBS)
	#----------------------------------------------------------------
	main.o: main.cpp global.h
		$(CXX) $(CXXFLAGS) -c main.cpp -o main.o
	#----------------------------------------------------------------
	clean:
		rm -f $(OBJS) $(TARGET)

## OS X ##

OS X > 10.6.x comes preinstalled with OpenCL.

Basic `Makefile`:

	CXX = g++
	DIR=/Developer/GPU\ Computing/OpenCL/common
	#----------------------------------------------------------------
	CXXFLAGS += -I$(DIR)/inc

	#----------------------------------------------------------------
	LIBS = -L$(DIR)/lib/
	LIBS += -framework OpenCL

	#---------------------------------------------------------------

	OBJS = main.o

	#----------------------------------------------------------------
	TARGET = compression

	all:	$(TARGET)
	#----------------------------------------------------------------
	$(TARGET):	main.o
		$(CXX) -o $(TARGET) $(OBJS) $(LIBS)
	#----------------------------------------------------------------
	main.o: main.cpp global.h
		$(CXX) $(CXXFLAGS) -c main.cpp -o main.o
	#----------------------------------------------------------------
	clean:
		rm -f $(OBJS) $(TARGET)

## Usage ##

### Preparing the environment ###

**Get platforms**

	cl_int	clGetPlatformIDs (cl_uint num_entries, cl_platform_id *platforms, cl_uint *num_platforms)

- `num_entries` is the number of cl_platform_id entries that can be added to `platforms`. If `platforms`
	is not `NULL`, the `num_entries` must be greater than zero.
- `platforms` returns a list of OpenCL platforms found
- `num_platforms` returns the number of OpenCL platforms available. If `num_platforms` is `NULL`, this argument is ignored.

Example code

	cl_uint num_entries = 1;
	cl_platform_id *cpPlatform;
	err = clGetPlatformIDs(num_entries, cpPlatform, NULL);

**Get the device**

	clGetDeviceIDs (cl_platform_id platform, cl_device_type device_type, cl_uint num_entries, cl_device_id *devices, cl_uint *num_devices)

- `platform` refers to the platform ID returned by `clGetPlatformIDs` or can be `NULL`. If platform is `NULL`, the behavior is implementation-defined.
- `device_type` is a bitfield that identifies the type of OpenCL device.
	- `CL_DEVICE_TYPE_CPU` An OpenCL device that is the host processor. The host processor runs the OpenCL implementations and is a single or multi-core CPU.
	- `CL_DEVICE_TYPE_GPU` An OpenCL device that is a GPU. By this we mean that the device can also be used to accelerate a 3D API such as OpenGL or DirectX.
	- `CL_DEVICE_TYPE_ACCELERATOR` Dedicated OpenCL accelerators (for example the IBM CELL Blade). These devices communicate with the host processor using a peripheral interconnect such as PCIe.
	- `CL_DEVICE_TYPE_DEFAULT` The default OpenCL device in the system.
	- `CL_DEVICE_TYPE_ALL` All OpenCL devices available in the system.

Example code

	cl_uint num_devices_returned;
	cl_device_id devices[2];
	err = clGetDeviceIDs(NULL, CL_DEVICE_TYPE_GPU, 1, &devices[0], &num_devices_returned);
	err = clGetDeviceIDs(NULL, CL_DEVICE_TYPE_CPU, 1, &devices[1], &num_devices_returned);

**Create a context**

	cl_context clCreateContext (const cl_context_properties *properties, cl_uint num_devices, const cl_device_id *devices, void (CL_CALLBACK *pfn_notify)(const char *errinfo, const void *private_info, size_t cb, void *user_data), void *user_data, cl_int *errcode_ret)

- `properties` specifies a list of context property names and their corresponding values. Each property name is immediately followed by the corresponding desired value. The list is terminated with `0`. Currently (1.1. spec) only the `CL_CONTEXT_PLATFORM` property with possible values of `cl_platform_id` specifying the platform to use is supported.
- `num_devices` is the number of devices specified in the `devices` argument
- `devices` is a pointer to a list of unique devices returned by `clGetDeviceIDs` for a platform
- `pfn_notify` is a callback function to report information about errors. If `pfn_notify` is NULL, no callback function is registered.
- `user_data` will be passed as the `user_data` argument when `pfn_notify` is called. `user_data` can be `NULL`
- `errcode_ret` will return an appropriate error code. If `errcode_ret` is `NULL`, no error code is returned.

Sample

	cl_context context;
	context = clCreateContext(0, 2, devices, NULL, NULL, &err);

**Create command queue(s)**

	cl_command_queue	clCreateCommandQueue (cl_context context, cl_device_id device, cl_command_queue_properties properties, cl_int *errcode_ret)

- `context` must be a valid OpenCL context.
- `device` must be a device associated with the context.
- `properties` specifies a list of properties (see spec for details. One is for ordering command, the other is for enabling profiling) for the command-queue. The list is terminated with `0`.
- `errcode_ret` will return an appropriate error code. If `errcode_ret` is `NULL`, no error code is returned.

Sample

	cl_command_queue queue_gpu, queue_cpu;
	queue_gpu = clCreateCommandQueue(context, devices[0], 0, &err);
	queue_cpu = clCreateCommandQueue(context, devices[1], 0, &err);

### Allocating memory ###

To execute our kernels we first have to move our data from the host to the global memory of the device. You can create a buffer by using:

	cl_mem	clCreateBuffer (cl_context context, cl_mem_flags flags, size_t size, void *host_ptr, cl_int *errcode_ret)

- `context` is a valid OpenCL context used to create the buffer object.
- `flags` is a bit-field that is used to specify allocation and usage information
	- `CL_MEM_READ_WRITE` Default. The memory object will be read and written by a kernel
	- `CL_MEM_WRITE_ONLY` The memory object will be written but not read by a kernel.
	- `CL_MEM_READ_ONLY` The memory object is a read-only memory object when used inside a kernel.
	- `CL_MEM_COPY_HOST_PTR` Valid only if `host_ptr` is not `NULL`. If specified, it indicates that the application wants the OpenCL implementation to allocate memory for the memory object and copy the data from memory referenced by `host_ptr`.
	- For details on the other flags (`CL_MEM_USE_HOST_PTR`, `CL_MEM_ALLOC_HOST_PTR` consult the spec)
- `size` is the size in bytes of the buffer memory object to be allocated.
- `host_ptr` is a pointer to the buffer data that may already be allocated by the application. The size of the buffer that `host_ptr` points to must be >= size bytes.
- `errcode_ret` will return an appropriate error code.

The `clEnqueueReadBuffer` and `clEnqueueWriteBuffer` function can be used to enqueue commands to read from a buffer object to host memory or write to a buffer object from host memory. In the following example we just use `CL_MEM_COPY_HOST_PTR` and an appropriate `host_ptr` to copy the data.

Sample

	const int mem_size = sizeof(float)*size;
	// Allocates a buffer of size mem_size and copies mem_size bytes from src_a_h
	cl_mem src_a_d = clCreateBuffer(context, CL_MEM_READ_ONLY | CL_MEM_COPY_HOST_PTR, mem_size, src_a_h, &error);
	cl_mem src_b_d = clCreateBuffer(context, CL_MEM_READ_ONLY | CL_MEM_COPY_HOST_PTR, mem_size, src_b_h, &error);
	cl_mem res_d = clCreateBuffer(context, CL_MEM_WRITE_ONLY, mem_size, NULL, &error);

### Writing to a buffer ###

If you data is aligned in a away so that you can't copy it directly to the device with `clCreateBuffer` you can use `clEnqueueWriteBuffer` to write to the memory of the device.

	cli_int	clEnqueueWriteBuffer 	(
										cl_command_queue command_queue,
										cl_mem buffer,
										cl_bool blocking_write,
										size_t offset,
										size_t cb,
										const void *ptr,
										cl_uint num_events_in_wait_list,
										const cl_event *event_wait_list,
										cl_event *event
									)

- `command_queue` refers to the command-queue in which the write command will be queued. `command_queue` and `buffer` must be created with the same OpenCL context.
- `buffer` refers to a valid buffer object.
- If `blocking_write` is `CL_TRUE`, the OpenCL implementation copies the data referred to by `ptr` and enqueues the write operation in the command-queue. The memory pointed to by ptr can be reused by the application after the clEnqueueWriteBuffer call returns. If `blocking_write` is `CL_FALSE`, the OpenCL implementation will use `ptr` to perform a non-blocking write. As the write is non-blocking the implementation can return immediately. The memory pointed to by `ptr` cannot be reused by the application after the call returns.
- `offset` is the offset in bytes in the buffer object to write to.
- `cb` is the size in bytes of data being written.
- `ptr` is the pointer to buffer in host memory
- `event_wait_list` and `num_events_in_wait_list` specify events that need to complete before this particular command can be executed. For more information please look into the spec.

### Preparing the kernel ###

There are two ways of creating a kernel either from source (`clCreateProgramWithSource`) or from a binary (`clCreateProgramWithBinary`).

Creating the program from source

	cl_program 	clCreateProgramWithSource (cl_context context, cl_uint count, const char **strings, const size_t *lengths, cl_int *errcode_ret)

- `context` must be a valid OpenCL context.
- `strings` is an array of `count` pointers to optionally null-terminated character strings that make up the source code.
- `lengths` argument is an array with the number of chars in each string. If `lengths` is `NULL`, all strings in the strings argument are considered null-terminated
- `errcode_ret` will return an appropriate error code. If `errcode_ret` is `NULL`, no error code is returned.

Sample code

	cl_int clresult;
	const char* openclsource = loadProgamFile(KERNEL_PATH); // custom code
	size_t sourcesize[] = { strlen(openclsource) };
	cl_program program = clCreateProgramWithSource(curr_ctxt, 1, &openclsource, sourcesize, &clresult);
	if (clresult != CL_SUCCESS) printf("Compile error\n");

In practice it seems that `clCreateProgramWithSource` will never return an error.

To compile and build the program you use

	cl_int clBuildProgram (cl_program program, cl_uint num_devices, const cl_device_id *device_list, const char *options, void (CL_CALLBACK *pfn_notify)(cl_program program, void *user_data), void *user_data)

 - `program` is the program object
- `device_list` is a pointer to a list of devices associated with program.	If `device_list` is a `NULL` value, the program executable is built for all devices associated with program for which a source or binary has been loaded. If `device_list` is a non-`NULL` value, the program executable is built for devices specified in this list for which a source or binary has been loaded.
- `num_devices` is the number of devices listed in device_list
- `options` is a pointer to a null-terminated string of characters that describes the build options. The spec for details
- `pfn_notify` is a function pointer to a notification routine which will be called when the program executable has been built (successfully or unsuccessfully).
- `user_data` will be passed as an argument when `pfn_notify` is called. `user_data` can be `NULL`

Sample

	cl_int clresult = clBuildProgram(program, 0, NULL, NULL, NULL, NULL);
	if (clresult != CL_SUCCESS) printf("Compilation error\n");

Create a Kernel

	cl_kernel	clCreateKernel (cl_program program, const char *kernel_name, cl_int *errcode_ret)

 - `program` is a program object with a successfully built executable.
 - `kernel_name` is a function name in the program declared with the `__kernel` qualifier.
 - `errcode_ret` will return an appropriate error code. If `errcode_ret` is `NULL`, no error code is returned.

Sample

	cl_int clresult;
	clCreateKernel(program, "name_of_function", &clresult);

Use `clSetKernelArg` to set the argument value for a specific argument of a kernel.

	cl_int	clSetKernelArg (cl_kernel kernel, cl_uint arg_index, size_t arg_size, const void *arg_value)

- `kernel` is a valid kernel object.
- `arg_index` is the argument index.
- `arg_value` is a pointer to data that should be used as the argument value for argument specified by `arg_index`. The argument data pointed to by `arg_value` is copied and the `arg_value` pointer can therefore be reused by the application after `clSetKernelArg` returns
- `arg_size` specifies the size of the argument value.

### Launching the kernel ###

	cl_int clEnqueueNDRangeKernel (cl_command_queue command_queue, cl_kernel kernel, cl_uint work_dim, const size_t *global_work_offset, const size_t *global_work_size, const size_t *local_work_size, cl_uint num_events_in_wait_list, const cl_event *event_wait_list, cl_event *event)

- `command_queue` is a valid command-queue
- `kernel` is a valid kernel object. The OpenCL context associated with *kernel* and *command-queue* must be the same.
- `work_dim` is the number of dimensions. Must be greater than zero and less than or equal to `CL_DEVICE_MAX_WORK_ITEM_DIMENSIONS`


Sample

	// Enqueuing parameters
	// Note that we inform the size of the cl_mem object, not the size of the memory pointed by it
	error = clSetKernelArg(vector_add_k, 0, sizeof(cl_mem), &src_a_d);
	error |= clSetKernelArg(vector_add_k, 1, sizeof(cl_mem), &src_b_d);
	error |= clSetKernelArg(vector_add_k, 2, sizeof(cl_mem), &res_d);
	error |= clSetKernelArg(vector_add_k, 3, sizeof(size_t), &size);
	assert(error == CL_SUCCESS);

	// Launching kernel
	const size_t local_ws = 512;	// Number of work-items per work-group
	// shrRoundUp returns the smallest multiple of local_ws bigger than size
	const size_t global_ws = shrRoundUp(local_ws, size);	// Total number of work-items
	error = clEnqueueNDRangeKernel(queue, vector_add_k, 1, NULL, &global_ws, &local_ws, 0, NULL, NULL);
	assert(error == CL_SUCCESS);

### Reading results ###

Reading the buffer and getting the results back to the host

	cl_int	clEnqueueReadBuffer (	cl_command_queue command_queue,
	 								cl_mem buffer,
	 								cl_bool blocking_read,
									size_t offset,
									size_t cb,
									void *ptr,
									cl_uint num_events_in_wait_list,
									const cl_event *event_wait_list,
									cl_event *event)

- `command_queue` and `buffer` must be created with the same OpenCL context
- If `blocking_read` is `CL_TRUE`, clEnqueueReadBuffer is blocking and does not return until the buffer has been read and copied into `ptr`. If `CL_FALSE` clEnqueueReadBuffer is non blocking and returns. The contents of the buffer that `ptr` points to cannot be used until the read command has completed. `event` argument returns an event object which can be used to query the execution status of the read command.
- `offset` is the offset in bytes in the buffer object to write to
- `cb` is the size in bytes of data being read or written.
- `ptr` is the pointer to buffer in host memory where data is to be read
- `event_wait_list` and `num_events_in_wait_list` specify events that need to complete before this particular command can be executed. If `event_wait_list` is `NULL`, then this particular command does not wait on any event to complete. If `event_wait_list` is `NULL`, `num_events_in_wait_list` must be `0`.
- `event` returns an event object that identifies this particular read command. Can be `NULL`

Sample

	float* check = new float[size];
	clEnqueueReadBuffer(queue, res_d, CL_TRUE, 0, mem_size, check, 0, NULL, NULL);

### Cleaning up ###

Clean up behind yourself. Everything allocated with `clCreate...` must be destroyed with `clRelease...`

	// Cleaning up
	delete[] src_a_h;
	delete[] src_b_h;
	delete[] res_h;
	delete[] check;
	clReleaseKernel(vector_add_k);
	clReleaseCommandQueue(queue);
	clReleaseContext(context);
	clReleaseMemObject(src_a_d);
	clReleaseMemObject(src_b_d);
	clReleaseMemObject(res_d);

## FAQ/Problems ###

###	`./../../../x86_64-suse-linux/bin/ld: cannot find -lGLU ` ###

I just started up yast and installed the `freeglut-devel` package

### `"error: ‘uint’ does not name a type" types.h` ###

On OSX you have to include `types.h`

	#include <sys/types.h>

## Resources ##

- [Khronos, OpenCL 1.1 Specification](http://www.khronos.org/registry/cl/specs/opencl-1.1.pdf)
- [Khronos, OpenCL 1.1 C++ Bindings Specification](http://www.khronos.org/registry/cl/specs/opencl-cplusplus-1.1.pdf)
- [Khronos, OpenCL 1.1 Quick Reference](http://www.khronos.org/files/opencl-1-1-quick-reference-card.pdf)
- [Khronos, OpenCL 1.1 Overview](http://www.khronos.org/developers/library/overview/opencl_overview.pdf)

- [AMD, OpenCL: Introductory Tutorial](http://developer.amd.com/zones/openclzone/courses/pages/introductory-opencl-saahpc10.aspx)
- [Codeplex, Tutorial](http://opencl.codeplex.com/wikipage?title=OpenCL%20Tutorials%20-%201)

[#Khronos:2011]: [Khronos, OpenCL 1.1 Specification](http://www.khronos.org/registry/cl/specs/opencl-1.1.pdf), 2011.

[#AMD:2010]: [AMD, OpenCL: Introductory Tutorial, Course Presentation	](http://developer.amd.com/zones/OpenCLZone/courses/Documents/AMD_OpenCL_Tutorial_SAAHPC2010.pdf), 2010.