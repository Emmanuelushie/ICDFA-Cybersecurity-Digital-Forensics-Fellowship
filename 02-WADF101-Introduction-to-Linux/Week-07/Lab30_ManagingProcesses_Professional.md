# Lab 30: Managing Processes

## Student Information

| Field | Details |
|---|---|
| Student Name | Emmanuel Adie Ushie |
| Student ID | C11/26/ICDF/17173 |
| Course | WADF-2026-M02: Introduction to Linux |
| Instructor | Mr. Udam Akume Gabriel |
| Week | Week 07 |
| Lab | Lab 30: Managing Processes |
| Date of Submission | 06/08/2026 |


---

## Executive Summary

This report documents Lab 30 of Week 07. The lab covered process discovery, shell job control, suspension and resumption, graceful and forced termination, resource monitoring, scheduling priority, and parent-child process relationships.

Disposable `sleep` processes were used to practise background execution and signals safely. The activities distinguished shell jobs from system process identifiers and demonstrated why graceful termination should normally be attempted before forced termination.

---

## Objectives

- List session and system-wide processes.
- Start and inspect background jobs.
- Move jobs between foreground and background execution.
- Suspend and resume a process.
- Apply `SIGTERM` and `SIGKILL` appropriately.
- Monitor live resource consumption.
- Start a process with reduced scheduling priority.
- Display process ancestry as a tree.

---

## Tools and Environment

- **`ps` and `ps aux`:** Displayed process snapshots.
- **`jobs`, `fg`, and `bg`:** Managed jobs created by the current shell.
- **`kill`:** Sent signals to jobs or process IDs.
- **`top`:** Displayed live resource and process information.
- **`nice`:** Started a process with an adjusted niceness value.
- **`pstree`:** Displayed parent-child process relationships.

---

## Workspace and Evidence Storage

```text
/media/sf_ICDFA/Week07/Screenshots/
/media/sf_ICDFA/Week07/Notes/
```

---

## Methodology

### 30A. Listing Processes

```bash
ps
ps aux
```

`ps` displayed processes associated with the current terminal context. `ps aux` produced a broader BSD-style system snapshot containing users, PIDs, CPU and memory percentages, start information, and command lines.

**Screenshot Filename:** `Fig03_CLI_ProcessListing_Ps_PsAux.png`

<img width="1366" height="662" alt="Fig03_CLI_ProcessListing_Ps_PsAux png" src="https://github.com/user-attachments/assets/85d991a7-5a6a-4b9e-9d43-3ea2b26d35e4" />


---

### 30B. Background Jobs, Suspension, and Resumption

```bash
sleep 300 &
jobs
fg %1
```

After `fg %1`, `Ctrl+Z` was pressed interactively to send a terminal stop signal and suspend the foreground job. The job was then resumed in the background:

```bash
bg %1
jobs
```

Job specifications such as `%1` are interpreted by the current shell and are not the same as system PIDs.

**Screenshot Filename:** `Fig04_CLI_BackgroundJobsAndSuspend_SleepAmp_Jobs_CtrlZ.png`

<img width="1366" height="662" alt="Fig04_CLI_BackgroundJobsAndSuspend_SleepAmp_Jobs_CtrlZ png" src="https://github.com/user-attachments/assets/0b7886fa-230b-4747-a110-a28ce564eeec" />


---

### 30C. Terminating Processes

```bash
kill %1
```

Without an explicit signal, `kill` normally sends `SIGTERM`, allowing the target process an opportunity to terminate cleanly. A new disposable process was used if forced termination needed to be demonstrated:

```bash
sleep 300 &
jobs
kill -9 %1
```

`-9` sends `SIGKILL`, which cannot be handled or ignored. It should be reserved for a process that does not respond to a normal termination request.

**Screenshot Filename:** `Fig05_CLI_JobControlAndKillSignals_Bg_Kill_KillNine.png`

<img width="1366" height="662" alt="Fig05_CLI_JobControlAndKillSignals_Bg_Kill_KillNine png" src="https://github.com/user-attachments/assets/c8358403-d946-4939-8ba3-b309b8736c07" />


---

### 30D. Monitoring, Priority, and Process Trees

```bash
top
nice -n 10 sleep 300 &
pstree
```

`top` displayed a continuously updating view of CPU, memory, load, and processes and was exited with `q`. `nice -n 10` started a process with a higher niceness value and therefore generally lower CPU scheduling priority. `pstree` displayed process ancestry.

> Niceness influences CPU scheduling but does not guarantee a fixed percentage of CPU time.

**Screenshot Filename:** `Fig06_CLI_TopMonitorNiceAndProcessTree_Top_Nice_Pstree.png`

<img width="1366" height="662" alt="Fig06_CLI_TopMonitorNiceAndProcessTree_Top_Nice_Pstree png" src="https://github.com/user-attachments/assets/4a95caab-a884-4b02-b58e-117b4bcd9eb4" />


---

## Results and Findings

The lab demonstrated the distinction between a static process snapshot and continuous monitoring. It also showed that shell job control applies only to jobs managed by the current interactive shell, while signals can be directed to system PIDs when permissions allow.

`SIGTERM` is preferable for routine shutdown because a process may perform cleanup. `SIGKILL` forces immediate kernel termination, which can leave incomplete writes or temporary state. Its use should therefore be deliberate and documented.

---

## Challenges and Solutions

| Challenge | Cause | Resolution |
|---|---|---|
| A job number was no longer valid after termination. | Shell job numbers refer only to currently tracked jobs. | A new disposable `sleep` process was started and verified with `jobs` before the next signal test. |
| `top` occupied the terminal. | `top` is interactive and updates continuously. | `q` was pressed to return to the shell. |

---

## Screenshot Reference

| Figure | Filename | Section |
|---|---|---|
| Fig 3 | `Fig03_CLI_ProcessListing_Ps_PsAux.png` | 30A |
| Fig 4 | `Fig04_CLI_BackgroundJobsAndSuspend_SleepAmp_Jobs_CtrlZ.png` | 30B |
| Fig 5 | `Fig05_CLI_JobControlAndKillSignals_Bg_Kill_KillNine.png` | 30C |
| Fig 6 | `Fig06_CLI_TopMonitorNiceAndProcessTree_Top_Nice_Pstree.png` | 30D |

---

## Recommendations

- Verify the target job or PID immediately before sending a signal.
- Attempt `SIGTERM` before `SIGKILL` unless an emergency requires immediate termination.
- Record process details before termination when preserving incident evidence.
- Use `top` for live behaviour and `ps` for reproducible snapshots.
- Remove or terminate disposable training processes after the exercise.

---

## Conclusion

Lab 30 established a practical process-management workflow covering discovery, job control, monitoring, priority, and termination. These capabilities support Linux operations and incident response, where analysts must identify and control processes without affecting unrelated workloads.

---

## Safety and Evidence Handling

Only disposable `sleep` processes were controlled. No critical service or unrelated user process was intentionally suspended or terminated.

---

## References

1. ICDFA. (2026). *WADF-2026-M02 Week 7 Practical Labs: Labs 29–32, Standard Text Streams, Processes, Archives and File Permissions.* International Cybersecurity and Digital Forensics Academy.
2. Linux man-pages Project. (n.d.). *Linux manual pages.* https://man7.org/linux/man-pages/
3. GNU Project. (n.d.). *GNU tar Manual.* https://www.gnu.org/software/tar/manual/
