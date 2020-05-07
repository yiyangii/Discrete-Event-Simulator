# Discrete-Event-Simulator
Readme.txt

The purpose of this assignment was to demonstrate a simulation similar to Giorgio's Discreet Event Simulator. The program is to synthesize "Events", or pseudo-processes that are to enter the system and hop in line to take its turn using the CPU. The system also contains two disks, for I/O purposes. Regarding the flow of the program, the events first enter the line for the CPU and await their turn. When they are next in line, they are removed and begins occupying the processor.
	
After this occurs, a probability factor determines whether the event needs to do disk I/O, or exit the system. Providing there is more work to be done, the event joins the shortest disk line and awaits disk usage permission. Similarly to the CPU, when an event is up next and the disk is ready, it is popped out of line and begins to occupy. The amount of time that a process spends in each component, as well as many other factors such as the probability factor, the start and end time, and the time between new jobs are all inputted through a text file named "input.txt", which gets opened and the values get extracted in the early stages of the program. 
	
The data gathered from that text file is then outputted to a log file ("logs.txt"), as well as every significant event that occurs during the running of the program. Events such as "Event 2 entered the CPU", or "Event 28 is finished using disk 2" are written to the log as they occur. Rather than time being independent, this program moves through time by calculating the time of the next events, adding them to a priority queue that sorts them by time, and sets the global time as the events are processed one-by-one. 
	
As far as the implementation goes, I chose to implement using C, and to use a structure for all of the configuration data, a queue structure for each of the three components, and a min-heap priority queue for the event queue. Each individual event is its own object containing the time, job number, and typecode, that can be translated into the jobs status using a function named type_to_string(). Many of the functions included in this program handle the queue and priority queue data types, but ones are also included to generate random numbers between two intervals, determine if an event will occur given a probability factor, and functions to process the devices (CPU, disks) as jobs move in and out of them. 

The main loop of the program states that, while the event queue is not empty and the simulation hasn't ended, pull the next event out of the list and determine its type. If it is a job for the CPU, then I call a function to manage the CPU. If the job is for the disks instead, I call a similar function for the disks. These functions are able to handle both arrivals and departures, by using a switch statement for the job's operation code in the function. For arrivals, the basic process is to create a new event for the next job when a job arrives at the CPU, and send the current job to the FIFO queue for the CPU. For disks, I simply sent the job to the FIFO queue for the disk with the shortest line. If said function was called for the departure of an event from a disk, it sets the respective device to idle (implemented by a global integer 1 for running, 0 for idle), changes the job's status to return to the CPU queue, and logs such an event. 

After these functions are called and their respective work has been done, I had to ensure that, if any devices are idling with jobs to be served, the jobs were being popped off the respective queue and beginning to use the device. If this is the case, a new event needed to be created for the termination of the job at said device, and written to the log file. The log file contains the initial configuration data (start time, stop time, probability, etc), as well as a statement for each time that a device is added to or removed from a device queue, or when the device begins service at a particular device. 

The process of writing these events to a log is done by a function that requires a string buffer and uses that buffer to print to a previously opened file, the log file. This helps to assist the monitoring of the devices, and ensuring that the program is working as intended. At the beginning of runtime, only two events are created, the arrival of the first job and the event that signals the termination of the event. New jobs are created every time an event joins the CPU queue, and each arrival will signal a corresponding finish event. 
	
This job was tested by first repeatedly running the program, testing different configuration values. After it is apparent that the program was stable, I began implementing statistics for each device within the program, that creates a table for max and average values. 
