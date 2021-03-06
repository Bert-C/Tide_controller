Tide_controller
===============

Arduino code and associated files for building an aquarium tide height controller. This assumes that you 
have downloaded and installed the latest version of the Arduino software (1.0.1 or newer).

Installation:

To operate the tide controller rack, install the following:

1. Copy the RTClib folder into your Arduino file folder inside a folder called 'libraries'. If there is not already a libraries directory, create it now.

2. Copy the folder with the tide library for your site (i.e. TidelibSanDiegoSanDiegoBay) into Arduino/libraries/

3. Copy the Tide_controller_v2_3 folder (or most recent version) into your Arduino folder where 
your other sketches are normally stored. 

4. Open the Tide_controller sketch in the Arduino IDE, and make sure the correct tide site
library is referenced in the Initial Setup section, near line 57, with a line like:

	\#include "TidelibSanDiegoSanDiegoBay.h"

That line should contain the name of the library for your local site that you copied into Arduino/libraries/.

5. If you want to change the tide range in which the tide rack travels, you can change the
value for the upperPos on or around line 64, which looks like:
	float upperPos = 5.0;
That line represents the upper limit at which the tide rack will stop moving upward, even
if the predicted tide goes higher. The units of upperPos are feet (not meters or inches). 

Plug the Arduino in to the computer using a USB cable. Upload the program to the Arduino. 
Open the serial monitor to view the output. See the http://arduino.cc site for help with 
these steps. 

------------------------------
If the real time clock attached to the Arduino is not yet set, you need to set it one time
using the RealTimeClock_reset sketch found in the folder of the same name. Upload that
to the Arduino, and it should automatically reset the real time clock. Make sure your
computer's clock is set to local Standard Time, not Daylight Savings time (which runs Mar-Nov
in most places). The tide prediction routine relies on the time being set to local 
standard time for your site, otherwise you won't get the current tide height out. After running
the RealTimeClock_reset sketch, before unplugging the Arduino, immediately upload a different 
sketch (such as the Tide_controller sketch) to the Arduino so that the clock doesn't try to reset 
itself repeatedly when the Arduino restarts.

-------------------------------
If there is no folder containing a tide prediction library for your desired site, it
will be necessary to generate a library using the R scripts found in the 
Generate_new_site_libraries directory. The harmonic data for NOAA sites are all in
the Harmonics_20120302.Rdata file. With these data, you can generate a new library
by running the R script tide_harmonics_library_generator.R. Inside that file, you must
enter a name for the site you're interested in on the line
stationID = 'Monterey Harbor'
Change the value inside the quote marks to match your site, save the file, and run the
script in R. It will create a new directory with the necessary library files inside that
can be copied into the Arduino/libraries/ folder. Find the name for your site by looking 
through the XTide website http://www.flaterco.com/xtide/locations.html 
XTide only produces harmonics for 635 reference tide stations (all others are approximated 
from these tide stations), so you need to find the nearest station listed as "Ref" on that 
page, rather than "Sub" stations.

The other R scripts in the Generate_new_site_libraries directory could be used to make an
updated Rdata file when XTide updates the original harmonics database and after you
convert it to a text file using the libtcd library from the XTide site (see the notes inside
the R scripts). 
