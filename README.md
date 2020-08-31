# KSP-Multitasker
An importable KSP script for defining and running triggering concurrent asynchronous tasks. 

A user friendly way to create multiple asynchronous processes in KSP. A 'task' behaves somewhat like a function, except it will run asynchronously from the rest of the code in the parent callback. 

With a regular KSP function (whether 'called' or inline), the script will jump to the start of the function definition, run the code in the function, and then jump back to the place the function was called, and continue running the code in that callback. This means that if the function contains a 'wait', the parent callback will also be affected by the wait.

When a 'task' is called, the code inside the task will start running, but the parent callback will carry on running, even if there is a 'wait' inside the task.

## Usage

Using the multitasker is simple - add the following line to your init callback:
multitasker.init()

To define a task, use the following syntax anywhere outside a callback:

	define_task(name, task_id)
		// code goes here
	end_define

The 'task_id' must be a unique number, starting from 0 and incrementing.

To call the task, simply use the task name as if it were a function:

	on note
		task_name() 
	end on

You can also pass arguments to the task. To do so, use this alternative task definition syntax:

	define_task_with_args(name, task_id, arg1, arg2, arg3)
		// code goes here
	end_define

Then, simply include the arguments when calling the task:

	on note
		task_name(30, EVENT_NOTE, some_variable)
	end on

The number of arguments must be exactly 3, and only integers can be passed through them. If you need less than 3, you can fill in the unused arguments with throwaway names.


Any variables you use inside a task definition should be polyphonic. This ensures they are kept 'thread safe', and won't be overwritten by any other concurrent tasks or callbacks.

It is recommended that you use the latest version of the SublimeKSP compiler, and make sure 'Optimize Compiled Code' is enabled.


## Caveats
You are advised not to use the custom event par 'EVENT_PAR_0' for any other note events, but if you must, make sure the value is always less than 20000000.

You can't call tasks from within other tasks. You also can't call tasks from the release callback.

Since the multitasker uses the release callback internally, you will need to use this pseudo-callback if you need to handle releases in your main script. 

	on_release

	end_on
