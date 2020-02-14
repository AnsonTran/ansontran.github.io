# Bootstrapping
A small program, BIOS (Basic Input Output System) is stored in non-volatile
memory in hardware. The BIOS knows how to access simple hardware devices i.e.
disk, keyboard, display.

When starting up, this program:
1. Checks RAM, keyboard, basic devices are installed and funtioning correctly
2. Scans buses to detect attached devices and configure new ones
3. Determine boot device (goes down the boot order list or user selects)
4. Read first sectore from boot device and executes it (bootloader)
5. Bootloader reads partition table, finds active partition, reads second
   bootloader
6. Secondary bootloader reads OS into memory and executes it

## OS Startup
Machine starts in system mode so kernel code can execute immediately
1. Initialize internal data structures
2. Create first process
3. Switch mode to user and start running first process
4. Wait for something to happen (OS entirely driven by external events)

## Process Creation
![process-creation](./pictures/process-creation.png)
1. Compile program code into object file with ASM instructions
2. Link file with shared/not shared libraries
3. Get an executable

```c
while (1) {
	char *cmd = read_command();
	int child_pid = fork();
	if (child_pid == 0) {
		exec(cmd); //cmd=executable name(reboot)
	} else {
		wait(child_pid);
	}
}
```
exec() does the following:
1. Stops current process
2. Loads program into process' address space
3. Initializes hardware context and args for new program
4. Places PCB onto the ready queue
*does not create a new process*
