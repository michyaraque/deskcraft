# Long work

## Progress

- Count real units. For a load, the bytes actually read against the bytes this load will read; for a job, the steps done against the steps planned. Remember how many bytes the last load of the same thing read and use it as the expected total next time.
- Show the fraction, the current item and, once the rate is stable for a few seconds, the time left. A time left that keeps rising means the work is stalling; treat it as a bug to investigate, not a display problem.
- The UI polls a small shared state (a mutex held for an instant, or atomics) every 30 to 100 ms and redraws only when it changed.

## Cancel

- One abort flag per kind of work, set by the UI, checked by the worker at every natural step: each file read, each chunk, each frame, each request.
- On abort the worker frees what it allocated (GPU memory, buffers, temp files) and reports `cancelled`, which the UI shows as information, not as an error.
- Reset the flag when the next job starts, not when the old one ends, so a late check cannot cancel the new job.
- Esc and the Cancel button do the same thing.

## Threads and locks

- The UI thread only draws and reacts. Network, disk, decoding, rendering and hashing run on workers.
- Never take the same lock twice in one statement or call chain. `*state.lock() = (state.lock().0 + 1, x)` deadlocks on the second lock: the window freezes, the OS shows "not responding", nothing is logged. Take the guard once, then read and write through it.
- Never hold a lock while doing I/O or waiting for another thread.
- A panic on a worker becomes an error message to the UI and a log line with a backtrace; the worker loop survives it.

## Loading large files

- Read with large sequential reads (4 to 16 MB), a few at a time, in the order the data will be used. Page-fault-driven reading of a memory map runs at a fraction of the disk speed on a cold cache, and far worse while another thread is also faulting elsewhere.
- Start a read-ahead on a background thread that pulls the next ranges into the OS cache while the loader processes the current ones; it stops on the abort flag.
- Read straight into the final buffer (pinned or GPU-bound memory) instead of mapping and copying, so the data is not twice in the process.
- Pick memory strategies from free memory at run time with a margin for the OS and the file cache. Locking a third of the RAM on a laptop that also caches the same file makes the OS swap; streaming from the cache instead loads in seconds.
- Measure what matters: time to first result, not only time to load. A lazy load that moves the disk read into the first step is fine if the first step says so.

## Measuring

- Measure on the slowest machine you support and with a cold cache. On Windows, a copy made with unbuffered I/O (`robocopy /J`) is not in the cache; read that copy for each cold run. On Linux, `echo 3 > /proc/sys/vm/drop_caches` as root; on macOS, `sudo purge`.
- Report per phase (each component of a load) with time and read speed, behind an environment variable or a debug setting, so a user can send the numbers.
- Write down the machine next to every number in the README.

## Files that change on disk

- Lists backed by folders (plugins, templates, presets) are refreshed by a watcher: OS notifications, or a cheap poll every 2 s of names, sizes and dates. Fire once a snapshot is stable across two looks, so a 13 GB copy refreshes the list once, when it ends.
- A poll of a few hundred files costs tens of milliseconds on a worker thread; it never touches the UI thread and only sends a message when something changed.

## Hardware with generations

- When the newest instructions or drivers leave old hardware out, ship a second build flavour for it (an older toolkit, a lower compute target, a different runtime) and name it plainly: `App-1.2.0-legacy-setup.exe`.
- Each build knows its own minimum at compile time and gates features with a message that names what is needed.
- Ship the runtime libraries the build needs next to the executable, load them lazily so the window opens even when they are missing, and say exactly what is missing when it is.
