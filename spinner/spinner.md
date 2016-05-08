# spinner.yab
A barber pole spinner for yab

the following are the exported sub programs:

**new_spinner(hz,vt,ht,width, name$, view$)**

This defines the barber pole and gives it a name. This must be called before any other sub programs in this library can be executed for this spinner.
 
*note:*
new_spinner() does not show the spinner, it just reserves the name and co-ordinates for the spinner.

* hz = horizontal position, ppixels from the left of the view.

* vt = virtival position, pixels from the top of the view.

* ht = height in pixels or the spinner.

* width = width in pixels of the spinner.

* name$ = the name of the spinner

* view$ = the view to place the spinner on.

**spinner_set(lr,name$)**

This sets the spin of the spinner. set lr to true to have the spinner spin to the left or up. false for right or down.


**spinner_colors(r,g,b,r1,g1,b1)**

This over-rides the color of the spinner.

* r, g, b Sets the foorground color

* r1,g1,b1 Sets the background color.

The defaults are:
r=0 g=80 b=225 r1=245 g1=245 b1=245 


**display_spinner(name$)**

Turns on the spinner display for spinner named name$

*note:* the barber pole will not be displayed, just a blank spinner. one must spin the spinner to dieplay the barber pole.


**spinall()**

Spins all tof your spinners.

**spin(name$)**

Only spin the spinner named name$

**hide_spinner(name$)**

Removes the spinner from the view.

