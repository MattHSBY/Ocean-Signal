A feedback polynomial is a mathematical expression that represents how the bits in the shift register interact to produce the next bit in the sequence. It is written as: 

$$P(x) = x^n + c_{k1}x^{k1} + c_{k2}x^{k2} + ... + c_1x+1$$
where $c_i$ is either 0 or 1, and the positions where $c_i = 1$ are called taps.
For example, if we have a 4-bit LFSR with taps at position 4 and 1, its feedback polynomial is:
$$P(X) = x^4 + x + 1$$

The [[T-018]] specification explains that the polynomial $$G(x) = x^{23} + x^{18} + 1 $$ is used, which is primitive under the [[Galois Field]] GF(2)