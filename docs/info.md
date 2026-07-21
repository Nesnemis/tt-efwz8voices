
## How it works
Vibe coded.
A single input clock is divided down into eight square-wave voices that form a
justly intoned minor chord. A ripple divide-by-2 spine plus two small synchronous
counters (divide-by-3 and divide-by-5) derive outputs at CLK/64, /48, /40, /32,
/24, /20, /16 and /12. Because every pitch is an exact integer division of the
same master clock, the intervals are pure ratios (the 10:12:15 subharmonic minor
triad, with octaves): the chord is beat-free in a way equal-tempered instruments
cannot be. Each voice is ANDed with one input pin and driven to its own dedicated
output pin, so all mixing happens off-chip and each voice can be leveled
independently. The counters self-correct from any power-on state within two
clocks, so rst_n is unused. Total: 14 flip-flops and 12 gates.

## How to test

Clock the design at 10 kHz (any rate from about 1 kHz to 150 kHz is musical;
pitch scales linearly, root = CLK/48, so CLK = 48 × the root frequency you want —
e.g. 7,911 Hz gives an E minor chord, 10,560 Hz gives A minor). Drive inputs high
with the DIP switches or buttons: ui_in[0] gates the lowest voice (CLK/64) and
ui_in[7] the highest (CLK/12), each appearing on the matching uo_out pin.
ui_in[1] alone sounds the root; ui_in[1..3] together sound the root triad. With
all inputs low, every output must be silent. Below ~1 kHz the voices become
audible click patterns in 3:4:5:6:8:10:12:16 polyrhythm — intended behavior, not
a fault. No reset sequence is needed.

## External hardware

Amplified speaker or audio amplifier, plus a simple passive mixer: 
one ~10 kΩ resistor from each uo_out pin into a common summing node, 
optionally 10 nF from the node to ground (~14 kHz corner)to soften the 
extreme treble — into an audio amplifier or powered speaker via a
volume pot. For a bare 8 Ω speaker, use a small amplifier module (PAM8302/LM386
class); a single NPN transistor works only with 47–100 Ω in series with the
speaker. A piezo element directly on any uo_out pin works for single-voice
testing, and the TT Audio Pmod (onboard piezo / line out on uo_out[7]) will play
the highest voice with no wiring at all. Optionally, momentary buttons to 3.3 V
with 10 kΩ pull-downs on ui_in turn the DIP-switch drone into a keyboard.
