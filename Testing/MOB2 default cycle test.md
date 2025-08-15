Testing by looking at the order of the flashes that appear when the [[MOB2]] is activated, we can link these to the LED indications which are listed in the manual.
Order of flashes: 
- green flash (x5) -- this is the startup sequence (not indication of any function)
- cyan flash -- GNSS search (it's actually a blue flash and then a green flash in quick succession, should happen once every 5 seconds)
- strobe white flash -- visual aid to location (should happen once every 2.5 seconds)
- amber flash (x3) -- receiver status indicator (transmitting without GNSS Fix, should happen every 5 seconds)
- strobe white flash
- cyan flash
- strobe white flash
- amber flash (x3)
- strobe white flash
- ...
As far as I can tell, everything so far is working as intended, as declared in the manual. It also makes sense for example that the strobe effect is happening more frequently than the amber flash x3 (2 strobes after the first amber before the next amber) because the strobe happens once every 2.5 seconds, whereas the amber flashes happen once every 5 seconds. 