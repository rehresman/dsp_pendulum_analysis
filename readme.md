# Analysis and Modeling of Pendulum Timing

The purpose of this analysis is to model pendulum metronome timing with a function simple enough that it can be implemented into any synthesizer, DAW, drum machine, or other device with **minimal performance implications**. The final model should maintain a steady tempo, i.e. if the clock starts at 120bpm, it should still be at about 120bpm, regardless of how long the clock has been running.  This will be the primary departure from the physical model of a pendulum metronome.


## Definition of the Resulting Sequence

From my analysis, I was able to define the following sequence. Let $\beta_n$ represent the duration of beat $n$, and $\epsilon_n$ represent a random sequence with a standard normal distribution. Let $c$ represent a centering constant. Then the equation of these results is as follows:

$$
\beta_{n} = bpm\cdot\big[\sigma_n\left(s + t\tau_n\right) + r\rho_n + e\epsilon_n + c\big]
$$
where
$$
\sigma_n = (-1)^{n+1} \qquad\text{(swing)}
$$
$$
\tau_n = 1+ \max\left(0, \frac{|1-k_n|}{3}\right) \qquad\text{(triangle pulse)}
$$
$$
\rho_n = \frac{\phi_n + 1 \mod 24}{23} \qquad\text{(ramp)}
$$
$$
k_n = \phi_n + 4 \mod 24
$$ 
$$
\phi_{n+1} = \phi_n + 1 \mod 24.
$$

![Plot of Model](./Model%20To%20Be%20Scaled%20By%20BPM.png)

# Discussion of Results

This model is simple enough to be implemented at very low computing cost.  It neglects to account for the linear drag real pedulum metronomes have, although estimates for coefficients of those parameters are also provided in the analysis.  It also appears to slightly under-estimate the periodic variance in tempo every 24 beats. The adjusted r-squared of the the clean model (without noise) was 0.769, so it accounts for 76.9% of the variance in the original data.

The final amount of noise added was the only factor not derived procedurally; instead a maximum noise amount was derived and the selected noise was chosen to be less.  This is due to the fact that this model does not account for all physical factors, but rather the most statistically significant of those tested.

Other techniques I attempted were Fourier Series Regression (ineffective at low-orders), modeling the amplitude spike with a flat pulse wave,  modeling amplitude with Fourier series (also ineffective at low-orders), and polynomial regression.

Further improvements could be made to this model by increasing the number of recordings used in analysis, including a larger data set of similar tempos and different tempos.  In the current model the effects of the different factors increase linearly with bpm.  This may or may not be true and requires further analysis.

Upon hearing the recording, I determined that the output of the model does "feel" like a pendulum metronome and will function as a suitable replacement.  I also listened to the clean model, which feels very good too.  It's less hypnotic than the model with noise, but has more groove, and is more engaging. I will be experimenting with them both in future projects.

Note: the audio files used for analysis are not included in this repo since they are too large for github (>100MB).