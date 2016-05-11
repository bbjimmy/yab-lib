# spinner.yab
A barber pole spinner for yab

the following are the exported sub programs:

**new_spinner(hz,vt,ht,width, name$, view$)**

This defines the barber pole and gives it a name. This must be called before any other sub programs in this library can be executed for this spinner.
 
*note:*

new_spinner() does not show the spinner, it just reserves the name and co-ordinates for the spinner. hz and vt difine the top left corner of the spinner.

* hz = horizontal position, pixels from the left of the view.

* vt = virtival position, pixels from the top of the view.

* ht = height in pixels or the spinner.

* width = width in pixels of the spinner.

* name$ = the name of the spinner

* view$ = the view to place the spinner on.

**spinner_set(lr,name$)**

This sets the spin of the spinner. set lr to true to have the spinner spin to the left or down. false for right or up.


**spinner_colors(r,g,b,r1,g1,b1)**

This over-rides the color of the spinner.

* r, g, b Sets the foorground color

* r1,g1,b1 Sets the background color.

The defaults are:
r=0 g=80 b=225 r1=245 g1=245 b1=245 


**display_spinner(name$)**

Turns on the spinner display for spinner named name$

*note:*

The barber pole will not be displayed, just a blank spinner. One must spin the spinner to dieplay the barber pole.


**spinall()**

Spins all of your spinners.

**spin(name$)**

Only spin the spinner named name$

**hide_spinner(name$)**

Removes the spinner from the view.


## usage

using the spinner can take two files:


testspinner.yab:

	#!yab
	//mimetype "application/x-vnd.testspinner"

	dir$=attribute get$ "",""
	dir$=dir$+"/"
	
	
	import spinner
	// set DEBUG = 1 to print out all messages on the console
	DEBUG = 0
	
	OpenWindow()
	
	
	// Main Message Loop
	dim msg$(1)
	while(not leavingLoop)
		nCommands = token(message$, msg$(), "|")
	
		for everyCommand = 1 to nCommands
			if(DEBUG and msg$(everyCommand)<>"") print msg$(everyCommand)
	
			switch(msg$(everyCommand))
				case "_QuitRequested"
				case "MainWindow:_QuitRequested"
					leavingLoop = true
					break
				case "start"
					my_cool_process()
					break
				case "_Scripting:done"
					spin=0
					hide_spinner("MySpinner")
					OPTION SET "start", "Enabled", true
					draw flush "MyView"
					break
				case "_Scripting:start"	
					window set "MainWindow","Activate"
					OPTION SET "start", "Enabled", false
					draw text 5,30, "MyCoolProcess","MyView"				
					display_spinner("MySpinner")
					spin=true
					break
				default
				
					
			end switch
				
	
		next everyCommand
		if spin spinall()
	
	wend
	CloseWindow()
	end
	
	sub OpenWindow()
		window open 100,100 to 300,300, "MainWindow", "Test Spinner"
		button 65,10 to 135,40, "start", "start", "MainWindow"
		WINDOW SET "MainWindow", "MoveTo", 300,300
		Window Set "MainWindow", "Flags"	,"Not-Resizable Not-Zoomable"
		VIEW 50,50 TO 150,150, "MyView", "MainWindow"
		draw set "bgcolor" ,227,227,227, "MyView"
		new_spinner(10,70,20,80,"MySpinner", "MyView")
		return
	end sub
	
	
	// Close down the main window
	sub CloseWindow()
		window close "MainWindow"
		return
	end sub
	
	sub my_cool_process()
	system(dir$+"MyCoolProcess &")
	end sub

MyCoolProcess.yab:

	#!yab
	// mimetype "application/x-vnd.MyCoolProcess"
	// set DEBUG = 1 to print out all messages on the console
	DEBUG = 0
	openwindow()
	arived = message send "application/x-vnd.testspinner", "start"
	// Main Message Loop
	leavingLoop = false
	dim msg$(1)
	while(not leavingLoop)
		nCommands = token(message$, msg$(), "|")
	
		for everyCommand = 1 to nCommands
			if(DEBUG and msg$(everyCommand)<>"") print msg$(everyCommand)
	
			switch(msg$(everyCommand))
				case "_QuitRequested"
				case "hidden_window:_QuitRequested"
					leavingLoop = true
					break
				default
					
			end switch
			mcp()
	
		next everyCommand
	
	wend
	closewindow()
	
	
	end
	sub closewindow()
	window close "hidden_window"
	end sub
	
	sub openwindow()
	
	WINDOW OPEN -100,-100 TO -90,-90, "hidden_window", ""
	end sub
	
	sub mcp() // This represents the process that required the spinner.
	
	for x=1 to 500
	sleep .01
	next
	
	arived = message send "application/x-vnd.testspinner", "done"
	
	
	
	leavingLoop = true
	end sub

Set the application flag on "MyCoolProcess" to "Background app" with the FileType Tracker add-on so that the second program is not shown in the DeskBar.