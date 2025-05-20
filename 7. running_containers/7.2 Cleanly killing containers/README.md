# PROBLEM
You want to cleanly terminate a container.

# SOLUTION
Use docker stop rather than docker kill to cleanly terminate the container.

# STEP
The kill program works by sending a TERM (a.k.a. signal value 15) signal to the
process specified, unless directed otherwise. This signal indicates to the program that
it should terminate, but it doesn’t force the program. Most programs will perform
some kind of cleanup when this signal is handled, but the program can do what it
likes—including ignoring the signal.

# DISCUSSION
Although we recommend docker stop for everyday use, docker kill has some additional configurability that allows you to choose the signal sent to the container via the
--signal argument. As discussed, the default is KILL, but you can also send TERM or
one of the less common Unix signals.