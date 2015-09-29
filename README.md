# unicorn-scroller

A scrolling information display using a Rasperry Pi and a Pimoroni Unicorn Hat.  
By Andrew Wedgbury <wedge@sconemad.com>  

## Setup

To start with, clone this git repo!

Install the unicornhat Python module from Pimoroni following the [instructions here] (https://github.com/pimoroni/unicorn-hat).

unicorn-scroller.py uses the [PIL](http://www.pythonware.com/products/pil/) module.   
The astro message script uses the [pyephem](http://rhodesmill.org/pyephem/) module.   
You can install these via pip with:

    sudo pip install PIL
    sudo pip install pyephem

You also need a font to use when displaying text. It's difficult to find a font that looks good at 8 pixels high - The best I've found is Minecraftia-Regular, which you can download from http://www.dafont.com/minecraftia.font
Unzip the font into the same directory as the script. You can also try out some different fonts - there are plenty available for free online. Unzip your chosen font into the same directory and modify the "font" and "font-offset" variables near the top of the main program.

You'll find various other variables defined at the top of unicorn-scroller.py, which you can use to set the display orientation, brightness, background colour and scroll speed.

## Runing

To run from the command line:

    sudo python unicorn-scroller.py

To run on system boot, add the following to /etc/rc.local
(before the "exit 0" line at the end):

    (cd /home/pi/unicorn-scroller && python unicorn-scroller.py)&

Being sure to change /home/pi/unicorn-scroller to the actual directory where
you checked out the files if different.

## Messages

The program scrolls messages generated by scripts located in the messages subdirectory.
Each script outputs its message in a simple format, consisting of text and the names of image files located in the images subdirectory.

The following message scripts are available:
(The example images shown below were generated by saving the PIL Image object as a PNG file, and scaling up by 2x, and use the Minecraftia-Regular font)

### time

Example: ![time](doc/time2x.png)

Displays the current time.  
Make sure you have your timezone set correctly!

### date

Example: ![time](doc/date2x.png)

Displays the current date.

### weather

Example: ![time](doc/weather2x.png)

Displays the current weather conditions plus a 3 day forecast.  
This uses [BBC weather feeds](https://support.bbc.co.uk/platform/feeds/WeatherFeeds.htm), you will need to edit the script and add your location ID, the previous link should explain how to get this.

### astro

Example: ![time](doc/astro2x.png)

Displays some astronimical information calculated using pyephem - moon phase, sunrise and sunset times.  
You will need to set the 'loc' variable near the top of this script to describe your location for this to be accurate.

### fortune

Example: ![time](doc/fortune2x.png)

Displays a short fortune cookie message generated by the unix [fortune](http://linux.die.net/man/6/fortune) program.


## Message frequency

The order and frequency of the messages to be displayed is determined by a set of [symbolic links](https://en.wikipedia.org/wiki/Symbolic_link) in the messages subdirectory. The program looks for links named in the form of two numbers separated by a dash, which are interpreted as a denominator, **d** and remainder **r** in the following formuala:

    i % d == r
    
Where **i** is an integer which starts at 1 and is incremented for every loop, and **%** is the modulus operator.

The default configuration is the following set of links:

    01-00 -> time
    16-00 -> date
    16-04 -> weather
    16-08 -> date
    16-12 -> astro
    99-00 -> fortune

So the *time* message is shown for every loop, *date* is shown every 8 loops, and *weather* and *astro* on every 16 loops. The *fortune* message is shown less frequently, on every 99 loops. You can change these links as required,  by removing (using the rm command) and adding (using the ln -s command). For example, to show the *fortune* message every 50 loops:

    cd messages
    rm 99-00
    ln -s fortune 50-00
    
You can of course  modify the message scripts or add new ones as required!
