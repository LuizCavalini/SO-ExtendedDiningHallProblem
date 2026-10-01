# Extended Dining Hall Problem

Concurrent simulation of the Extended Dining Hall problem in C, using
POSIX threads with mutexes and condition variables. Operating Systems
coursework.

## The problem

Students arrive at a dining hall with a limited number of seats. The hall
only opens for a new group once the previous one has finished, so arriving
students must wait while a meal is in progress. The challenge is
coordinating arrival, seating, eating and leaving so that no student is
served out of turn, no one waits forever, and the shared state stays
consistent while every student runs on its own thread.

## Approach

Each student is a thread. Access to the shared hall state is protected by
a mutex, and threads block on condition variables instead of spinning —
one for students waiting to enter, another for the group currently eating.
A thread signals the relevant condition whenever it changes state that
others are waiting on.

Using condition variables rather than busy-waiting is the point of the
exercise: a blocked thread consumes no CPU and is woken only when the
condition it depends on may actually have changed. Each wait is wrapped in
a loop that re-checks the predicate, since a wakeup does not guarantee the
condition holds.

## Files

| File | Description |
|---|---|
| `dining_hall.c` | Full simulation — thread creation, synchronisation and state machine |
| `makefile` | Build script |

## Building and running

```bash
make
./dining_hall
```

Requires a POSIX environment with pthreads (Linux, macOS, or WSL).
Compiled with `-pthread`.

## Stack

C · POSIX threads · mutexes · condition variables
