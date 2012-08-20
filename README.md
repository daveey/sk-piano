sk-piano
========

Arduino code for controlling piano and lights

Simulator Setup
---------------

In order to run the simulator you need a few things set up:
* ruby + ruby gems
* ruby gem: eventmachine (install: sudo gem install eventmachine)
* ruby gem: em-websocket (install: sudo gem install em-websocket)
* gcc/g++ (on mac, this means xcode + command line tools)

To run it, run simulator/websockets_server.rb from the simulator directory, then
browse to simulator/sim.html in Chrome.  (You probably need Chrome, and a recent
version, due to the simulator's use of web sockets)  Every time you reload the
page it will re-run beaglebone/piano (and should kill that process when you
browse away from the page).

Using the simulator
-------------------

* Right now, the whole strip is shown as one long line.
* Keys are Z, X, ..., >, ? for C3 through E4; Q, W, ..., [, ] for F4 through C6,
  with keys above for sharps (e.g. "S" is C#3)
* If the keys seem weird I might've (umm, accidentally?) checked it in to use
  dvorak key bindings; fix sim.js to use "key_mapping_reverse_qwerty" instead of
  the dvorak one.
* ENTER is A0 (mostly for switching visualizers using MasterVisualizer)

Compiling
---------

* You should be able to just run "make" in the beaglebone/ subdir.
* "make -j && ../simulator/websockets_server.rb" is your friend.

(Very Rough) Getting Started Guide
----------------------------------

* main.cpp has a piano() method where visualizers are instantiated, modify this
  to change which visualizers are active.
* MasterVisualizer holds a bunch of other visualizers and cycles through them
  when A0 (the leftmost key on the keyboard, ENTER in the simulator) is pressed.
* The most basic visualizers just respond to onKeyDown/onKeyUp events as well as
  a "onPassFinished" event which gets called once per piano read / render cycle.
  See SimpleVisualizer for an example of how to code these.
* There's some light infrastructure for "Particle"-based visualizers which
  operate generally by spawning new particles sometimes and those particles
  get "aged" (where they can update any of their state, including position /
  color in response to time) and eventually the particles decide they have died.
  See SimpleParticleVisualizer for an example of this.
* Even though the websockets_server.rb script will run a new instance of the
  piano executable every time a page is loaded, don't forget you still have to
  compile your code when you change something :)

Upcoming
--------

* I'm going to (soon) make a CompositeVisualizer class that runs multiple
  visualizers at the same time, most usefully with each of them only operating
  on different subsets of the strip.  So you can have DaveeyVisualizer running
  right above the keyboard while other things happen elsewhere.

Useful Utility Classes
----------------------

* Check out Colors.h for useful things dealing with colors.
* Check out Util.h for some easy utils for timestamps and logging.

