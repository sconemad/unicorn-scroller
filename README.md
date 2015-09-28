# unicorn-scroller

A scrolling information display using a Rasperry Pi and a Pimoroni Unicorn Hat.
By Andrew Wedgbury <wedge@sconemad.com>
http://www.sconemad.com

## Dependencies

unicorn-scroller.py uses the PIL module.
The astro message script uses the pyephem module.
You can install these via pip with:

   sudo pip install PIL
   sudo pip install pyephem

## Runing

To run from the command line:

   sudo python unicorn-scroller.py

To run on system boot, add the following to /etc/rc.local
(before the "exit 0" line at the end):

   (cd /home/pi/unicorn-scroller && python unicorn-scroller.py)&

Being sure to change /home/pi/unicorn-scroller to the actual directory where
you checked out the files if different.

