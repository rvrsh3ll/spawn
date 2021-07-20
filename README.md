## SPAWN - Cobalt Strike BOF
Beacon Object File (BOF) that spawns an arbitrary process from beacons memory. Supports Parent Process ID (PPID) spoofing & blocking non-MS signed DLLs from loading into the processes memory (some EDR DLLs).

### Why did I build this?
##### 1. To learn more about Cobalt Strike BOFs
##### 2. I want flexibility in choosing my sacraficial processes. 
  + Spawning the same process for every fork-and-run seems like bad/predictable OPSEC to me.
  + There are probably methods for this out there or built into CS already. Either way, I wanted to build my own.
##### 3. I have allot of cool BOF ideas that I want to build on this.

### Proof of Concept Demo Screenshot 
  ![](/images/Bof-SpawnSuspendedProcess.png)
+ Spawning new suspended processes with the BOF

### Compile with x64 MinGW:
```bash
x86_64-w64-mingw32-gcc -c spawnSuspendedProcess.c -o spawnSuspendedProcess.o
```
### Run from Cobalt Strike Beacon Console
```bash
beacon> inline-execute /Path/To/spawnSuspendedProcess.o <EXE FileName>
```


### To Do List
+ ~Agressor script for better end user experience~
![](/images/cna.png)
+ ~PPID spoofing for better parent-child process relation OPSEC~
![](/images/ppidspoof.png)
  - Here we can see our `cmd.exe` process being spawned with the PPID as `OneDrive.exe`
+ ~implement Cobalt Strike `blockdll` functionality to prevent non-MS signed DLLs from loading into the spawned processes memory~
![](/images/blockdll.png)
  - We see the parent-child process relationship, and that our spawned process has been created with the `Signatures restricted (Microsoft only)`
  - The `Signatures restricted (Microsoft only)` makes it so DLL's not signed by Microsoft cannot be loaded into our spawned process
+ ~~Do not crash the beacon process when the PE file does not exist~~
![](/images/noCrashUpdate.png)
  - No longer crashes on process creation failure!
+ Return the PID to the Cobalt Strike console when the new process is spawned
+ Build out different methods of remote process injection
+ Build out different methods of remote process patching
  + NTDLL.DLL remote process Unhooking
  + ETW remote process Patching/Bypass
  + AMSI remote process Patching/Bypass
  + CLR Loading & .Net assembly injection


### Credits / References
##### PPID Spoofing & blockDll functionality
+ Credit/shoutout to: Adam Chester @_xpn_ + @SEKTOR7net + Raphael Mudge
+ Thank you for the amazing work that you've contributed. I would not be able to publish this without your blogs, videos, and awesome content!
+ Main References for PPID Spoofing & blockdll
  - https://blog.xpnsec.com/protecting-your-malware/
  - https://blog.cobaltstrike.com/2021/01/13/pushing-back-on-userland-hooks-with-cobalt-strike/
  - https://institute.sektor7.net/ (Courses)
##### Raphael Mudge - Beacon Object Files - Luser Demo
+ https://www.youtube.com/watch?v=gfYswA_Ronw
##### Cobalt Strike - Beacon Object Files
+ https://www.cobaltstrike.com/help-beacon-object-files
##### BOF Code References
###### anthemtotheego/InlineExecute-Assembly
+ https://github.com/anthemtotheego/InlineExecute-Assembly/blob/main/inlineExecuteAssembly/inlineExecute-Assembly.cna
###### ajpc500/BOFs
+ https://github.com/ajpc500/BOFs/blob/395c66a9353bb19853913aebcb3af143dd8f1d36/ETW/etw.c
###### trustedsec/CS-Situational-Awareness-BOF
+ https://github.com/trustedsec/CS-Situational-Awareness-BOF
##### Sektor7 Malware Dev Essentials course - learned how to do the early bird injection technique
+ https://institute.sektor7.net/red-team-operator-malware-development-essentials