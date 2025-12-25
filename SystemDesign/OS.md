### Q: What are the four reasons (necessary conditions) for deadlock in an OS?

**A:** The four necessary conditions (Coffman conditions) are:

1. **Mutual exclusion** — at least one resource is non-shareable (only one process can use it at a time).
2. **Hold and wait** — a process holds resources while waiting to acquire more.
3. **No preemption** — resources can’t be forcibly taken; they’re released only by the holding process.
4. **Circular wait** — a cycle exists where each process waits for a resource held by the next process.
