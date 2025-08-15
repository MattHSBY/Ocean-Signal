Quadrature phase shift keying (QPSK) is a type of [[Phase shift keying]]. QPSK is generally more useful than [[Binary phase shift keying|BPSK]] for instance, because it uses 4 points on a constellation diagram, equispaced around a circle. With 4 phases, QPSK can encode two bits per symbol. ![[Pasted image 20250226101754.png]]
A common problem is that when flipping both bits, (going straight across the circle to the opposite corner), the amplitude will become too low, as it goes near the origin. You can avoid this by using Offset QPSK.

## OQPSK
Offset quadrature phase shift keying (OQPSK) is an implementation of QPSK which avoids the origin while performing QPSK. In OQPSK you only switch 1 bit at a time, in order to keep the amplitude high enough. 


