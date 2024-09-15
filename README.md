# Web Synth
You can check out the full version here [https://web-vst.vercel.app/](https://web-vst.vercel.app/)
###### This project is a minimalist synthesizer designed to teach fundamental audio design principles, built with Svelte and web audio api. Most modern synthesizer plugins are extremely complicated and can be overwhelming to approach for newcomers, so my hope is that learning tools such as this one can provide a stepping stone to keep them engaged and motivated.
###### In the future I plan to add a few more features including tooltips and non-polyphonic keys. Check out below how you can build it yourself or go to the site to try it out. Additionally, I've added some simple instructions to help with playing it, at least until I add the instructions onto the site itself.
---
### How to Build It Yourself (Current Version)
1. **Clone the Flask Repository**\
    $ `git clone https://github.com/bls0053/web-vst.git`

2. **Install React Dependencies**\
    $ `npm install`

3. **Run the Development Server**\
    $ `npm run dev`
---
### How to Play

#### `ADSR`
Adsr is an envelope responsible for the notes base behavior (volume) when played. Different evelopes can control any aspect of the sound, but this one controls these four parameters. Displayed in brackets is the range of values allowed.
1. **Attack** [0 - 5.00] - How long the note takes to reach its peak volume.
2. **Decay** [0 - 5.00] - How long it takes to reach the sustain volume.
3. **Sustain** [0 - 1.00] - How loud the held note is after attack and decay.
4. **Release** [0 - 5.00] - How long the note takes to stop playing on release.

Ex - If you wanted a "plucky" sound, you could set the adsr to these values [ Attack = 0 , Decay = .1, Sustain = 0, Release = .25 ]. With these settings, it would play quickly and decay quickly, giving you a fast burst of sound, that fades withn a short release of a quarter second. Alternatively, If you wanted a nice smooth ambient sound, you could set the adsr to these values [ Attack = 1.5 , Decay = 2, Sustain = .5, Release = 2.25 ]. The result would be a slow ramp up for 1.5 seconds, then a decrease in sound over 2 seconds to half volume, then on letting go it would take 2.25 seconds to stop. 

#### `Waves`
The synth offers four different waves - in order left to right, you can choose `sine`, `square`, `triangle`, or `sawtooth`.

#### `Record`
The synth also allows you to record a single loop track to play over. The controls are fairly simple - for the buttons from left to right under the `Record` section:
1. Record - press this to start a recording, it gives you a 4 click countdown, then you play whatever you like until the recording duration is finished.
2. Stop - press this to stop the current recording prematurely.
3. Play - press this to replay your recorded loop.
4. Pause - press this to pause your currently playing loop.

There are a few settings here you can use to customize your recording:
1. Tempo - beats per minute of your recording.
2. Beats - amount of beats per measure.
3. Measures - amount of measures to record.
4. Gain - volume of played back audio.

Ex - if you wanted a fast loop in 5/4 time, you could set tempo to 180, bars to 5, and then set the measures to however long you'd like it to record.

#### `Filters`
There are four filters you can apply - in order left to right, `lowpass`, `bandpass`, `highpass`, or `notch`.
1. lowpass - allows low frequencies through and reduces the strength of high frequencies.
2. bandpass - allows frequencies that are within a certain band, and rejects others.
3. highpass - allows high frequencies through and reduces the strength of low frequencies.
4. notch - cuts out a band of frequencies and allows all others.

There are a few settings here you can use to modify these filters:
1. frequency ([20 - 2000] hz) - this is the frequency at which the filter is applied.
2. bandwidth ([.0001 - 30] hz) - this is the "width" of the filter when applied.
3. mix [0 - 100] - how much of the filtered sound is heard vs unfiltered (dry / wet).

Ex. A lowpass filter with a small bandwidth would sharply cuttoff all frequencies except very low ones. Alternatively a large bandwidth on a highpass filter would have a gradual cuttoff of lower frequencies, and include more higher ones.

#### `Key Area Settings`
1. Volume - volume of played notes (volume set here will be the max volume of loop playbacks).
2. Octave - change in octaves, can go down 2 octaves and up 4.
3. Modes - toggles on note names and key bindings.
4. Pitch Bend - allows a shift of played note pitch within 2 semitones (1/6 octave) of the played note.






























