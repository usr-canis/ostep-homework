
### 1. Run `process-run.py` with the following flags: `-l 5:100,5:100`
   - **What should the CPU utilization be (e.g., the percent of time the CPU is in use)? Why do you know this? Use the `-c` and `-p` flags to see if you were right.**
     - **Answer:** 100% CPU utilization, since there’s no I/O process. Total time is 10 seconds, as there are 5 instructions in each of the 2 processes.

### 2. Now run with these flags: `./process-run.py -l 4:100,1:0`
   - **These flags specify one process with 4 instructions (all to use the CPU), and one that simply issues an I/O and waits for it to be done. How long does it take to complete both processes? Use `-c` and `-p` to find out if you were right.**
     - **Answer:** `-l 4:100,1:0 →` 10 seconds.

### 3. Switch the order of the processes: `-l 1:0,4:100`
   - **What happens now? Does switching the order matter? Why? (As always, use `-c` and `-p` to see if you were right)**
     - **Answer:** `-l 1:0,4:100 →` 6 seconds. Yes, the order does matter since an I/O request is made by `1:0`, allowing process `4:100` to get CPU while `1:0` waits for I/O completion.

### 4. Run with flags: `-l 1:0,4:100 -c -S SWITCH ON END`
   - **With the `SWITCH ON END` flag, what happens when you run the following two processes, one doing I/O and the other doing CPU work?**
     - **Answer:** With `SWITCH_ON_END`, the CPU waits for I/O completion and only switches when the process execution is complete.

### 5. Run with flags: `-l 1:0,4:100 -c -S SWITCH ON IO`
   - **What happens now? Use `-c` and `-p` to confirm that you are right.**
     - **Answer:** With `SWITCH_ON_IO`, the CPU switches to the other process when one is waiting for I/O, improving CPU utilization.

### 6. Run with flags: `-l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -I IO RUN LATER -c -p`
   - **With `-I IO RUN LATER`, when an I/O completes, the process that issued it is not necessarily run right away. What happens when you run this combination of processes? Are system resources being effectively utilized?**
     - **Answer:** With `-I IO_RUN_LATER`, the CPU isn’t utilized efficiently, as it keeps running the current process even after an I/O completes.

### 7. Run the same processes, but with `-I IO RUN IMMEDIATE` set
   - **How does this behavior differ? Why might running a process that just completed an I/O again be a good idea?**
     - **Answer:** `-I IO_RUN_IMMEDIATE` executes the process right after an I/O completes, enabling the CPU to handle other processes in parallel, improving overall efficiency.

### 8. Run with some randomly generated processes: `-s 1 -l 3:50,3:50` or `-s 2 -l 3:50,3:50` or `-s 3 -l 3:50,3:50`
   - **What happens when you use the flag `-I IO RUN IMMEDIATE` vs. `-I IO RUN LATER`? What happens when you use `-S SWITCH ON IO` vs. `-S SWITCH ON END`?**
     - **Answer:** `IO_RUN_IMMEDIATE` takes less time, as it immediately executes processes after I/O completion. `SWITCH_ON_IO` also takes less time, as it allows process switching during I/O wait, improving CPU utilization.
