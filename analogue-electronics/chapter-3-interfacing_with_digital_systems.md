---
layout: page
title: "Chapter 3: Interfacing with Digital Systems"
parent: Analogue Electronics
nav_order: 4
---

# Chapter 3: Interfacing with Digital Systems

Interfacing a micro-controller with external circuitry can be a challenging task and frequently requires a signal to be either multiplied by a gain or offset by some value. This signal conditioning is completed on both micro-controller inputs and outputs.

In Chapter 1 we showed how a MOSFET H-bridge can be driven with a micro-controller by using two NPN BJT level adjusters. This simple addition to the H-bridge operates by multiplying the input signal from a micro-controller with some gain and shows how the output of a micro-controller can be conditioned.

Likewise, an input signal to a micro-controller must be adjusted such that its voltage varies between 0 V and 3.3 V. If this is not completed, the sampled signal from the micro-controller can be saturated resulting in a digital signal that is essentially useless.

This chapter will expand on two types of circuits that can be used to interface an external circuit with a micro-controller by discussing level adjusters and how to apply a gain to a signal.

## Level adjusters

A level adjuster is a type of Op-Amp circuit that is used to condition a signal to or from a micro-controller by:

- Adding an DC offset to the signal.
- Multiplying the signal by some gain value.

We can write the operation of this circuit quite simply. Consider an input signal $x$ and output (level adjusted) signal $y$, the operation of the level adjuster can then be written as:

$$y = m(x +c)$$

We leave the above equation in the above form since differential amplifiers are often used to construct level adjusters as the transfer function for a differential amplifier has the same form of the above equation (refer back to Chapter 2). Thus if we have an input signal $x$ that varies from $x_{min}$ to $x_{max}$ and an output signal $y$ must vary from $y_{min}$ to $y_{max}$, we shall use a differential amplifier circuit.

<div class="example" markdown="1">
#### **Example 3.1**
\\
Let's say a sensor output can vary between -12 V and +12 V.

Assume that the supplies of -12 V and +12 V are available.

From $y = m(x + c)$ follows:

$$0 = m\*(-12 + c), and$$

$$3.3 = m\*(+12 + c)$$

Therefore $c = 12$, thus $m = \frac{3.3}{24}$.

Therefore $y = \frac{3.3}{24}$(x + 12) = $\frac{3.3}{24}$\[x -- (-12)\].

The following circuit would be good:

<img src="./images/3.1.png" width="50%" alt="Level adjuster based on differential amplifier"/>
_Figure 3.1: Level adjuster based on differential amplifier_

</div>

> #### **Question 3.1:**
> Can 330 Ω and 2.4 kΩ also work?
> 
> Why are the chosen values better?
> 
> Can 3.87 kΩ and 28.15 kΩ also work?
> 
> Why are the chosen values better?

<div class="example" markdown="1">
#### **Example 3.2**
\\
Let's say a sensor output can vary between -10 V and +10 V.

Assume that supplies of -5 V and +5 V are available for level adjuster circuitry.

Design the level adjuster circuitry suitable for connecting the sensor output to the A/D of a micro.

Indicate the supply voltages to the op-amp(s).

Similar to what we've done in Example 3.1, the required solution is:

$$y = \frac{3.3}{20}(x + 10) = \frac{3.3}{20}(x - (-10))$$

A general differential amplifier as shown below would work. Make sure you can analyse and design it.

<img src="./images/3.2.png" width="50%" alt="Level adjuster based on a differential amplifier"/>
_Figure 3.2: Level adjuster based on a differential amplifier_

Applying the equation
$$V_{3} = V_{2}\frac{R3}{R1}\left( \frac{1 + \frac{R1}{R3}}{1 + \frac{R2}{R4}} \right) - V_{1}\frac{R3}{R1}$$

that was derived before for the general differential amplifier, yields
$$V_{3} = y,\ V_{2} = x,\ V_{1} = - 5:$$

$$y = x\frac{3.3k}{10k}\left( \frac{1 + \frac{10k}{3.3k}}{1 + \frac{23.3k}{3.3k}} \right) + 5\frac{3.3k}{10k}$$
$$= 0.33(0.5x + 5) = 0.165x --(-5)0.33$$

Test: $x = -10 V =\> y = 0\  V$ and $x = +10 V =\> y = 3.3\  V$

How were the resistors designed?

From the equations above follow: $\frac{R3}{R1} = 0.33$ and
$\frac{R3}{R1}\left( \frac{1 + \frac{R1}{R3}}{1 + \frac{R2}{R4}} \right) = 0.165$

Choose $R_{1} = 10\  k\Omega$, $\Rightarrow$ $R_{3} = 3.3\  k\Omega$

Therefore
$$\frac{3.3k}{10k}\left( \frac{1 + \frac{10k}{3.3k}}{1 + \frac{R2}{R4}} \right) = 0.165$$
Choose $R_{4} = 3.3\  k\Omega$, $\Rightarrow$ $R_{2} = 23.3 k\Omega = 20 k\Omega + 3.3\  k\Omega$
</div>

<div class="example" markdown="1">
#### **Example 3.3**
\\
Let's say a sensor output can vary between -12 V and +12 V.

Assume that the supplies of -12 V and +12 V are available, but different from example 1, -12 V from the sensor must provide 3.3 V, and +12 V must provide 0 V.

From $y = m(x + c)$ follows:

$$0 = m\*(+12 + c),$$

$$3.3 = m\*(-12 + c)$$

Therefore $c = -12$, thus $m = - \frac{3.3}{24}$.

Therefore $y = - \frac{3.3}{24}(x - 12) = \frac{3.3}{24}(12 - x) = - \frac{3.3}{24}x - \frac{3.3}{24}( - 12)$.

From the last two representations, it can be seen that it can be implemented with either a differential amplifier and the +12 V as one input, or an inverting adder and the -12 V as the one input:

<img src="./images/3.3.png" width="100%" alt="Level adjusters based on a differential amplifier (a) and an inverting adder (b)"/>
_Figure 3.3: Level adjusters based on a differential amplifier (a) and an inverting adder (b)_
</div>



{: .note }
You must be able to design similar level adjusters after changing the sensor voltage levels and available supplies in the examples above to anything realistic. Be careful of the supply voltages and always check whether all op-amps can deliver what it should with the given supplies.

## Gains and attenuations
Gains or attenuations (gain < 1.0) are often required and are simpler to implement than level adjusters:

-   If a gain of more than one is required, and it must be non-inverting, a non-inverting amplifier is suitable.

-   If an attenuation is required, and it must be non-inverting, two inverting amplifiers in series can be considered.

Comparators can also be used to effectively get required gains or attenuations. The supply voltages to the comparators will then determine the output levels.

The following circuit will change 0 V and 3.3 V levels to about 0 V and 12 V respectively. It will also produce the inverse of the first comparator's output. This is required if drive signal 1 and drive signal 2 are required to drive a BJT H-bridge in the mode where the one must be the inverse of the other.

<img src="./images/3.4.png" width="50%" alt="Level adjuster based on two comparators"/>
_Figure 3.4: Level adjuster based on two comparators_

The $10 \  k\Omega$ and $62 \  k\Omega$ were calculated to produce a reference voltage of about half the 3.3 V on one of the inputs of each of the two comparators.

The $1 \  k\Omega$ and $100 \  k\Omega$ were practically found to let the comparators always function correctly, because they prevent the inputs to the comparators to be too close to the 0 V of the comparators' supply voltage when the input signal is at 0 V.
