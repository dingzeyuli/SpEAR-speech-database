# SpEAR Database: Technical Description

## Recording Equipment:
  

R0DE NT1 - Large Diaphragm Condenser Microphone.  
Event Electronics - 20/20p Powered Monitor System.  
Event Electronics - Gina 20-Bit Multitrack Digital Audio Recorder.  
Applied Research and Technology - Professional Tube Mic Preamp  

## Playback and Recording Format:

Playback and recordings were done at 48KHz by upsampling from the original sampling rate.  
All Recordings were subsequently downsampled to 16kHz, and stored as 16 bit wav files.  
Sampling conversion was performed by a polyphase implementation in MATLAB using the "resample" function.

## Explanation of the Data:

### _Noisy Speech Recordings_ 

The speech waveform is played through monitor A, and the noise waveform is simultaneously played through monitor B. The noisy speech is recorded through the microphone. All D/A and A/D converters are clocked synchronously.  
A clean reference to the speech is obtained by playing the speech file through monitor A in the absence of noise. This clean reference recording is used as the "signal" in signal-to-noise ratio calculations.  
Successive recordings of the clean speech differ slightly due to minor variations in the room channel. If the variability between successive recordings is viewed as a form of noise on the clean reference, the SNR (or Signal to Error Ratio) of the reference can be determined to be approximately 40dB. This can be taken as a measure of the accuracy of the clean room reference.  
However, because this variability error has a negligable effect on the signal-to-noise ratio of the noisy speech, it is simply omitted from all SNR calculations. That is, the SNR of the clean reference is assumed to be infinite (no noise).  


|  Variable Name  |  Notation  |  Description  |  SNR Calculation  |
| --- | --- | --- | --- |
|   Speech Wavfile   |  s  |  16 bit data waveform  |   Not defined  |
|    Noise Wavfile   |   n  |  16 bit data waveform   |   Not defined  |
|    Clean Speech Recording   |   r(s)  |  recording of speech wavfile played through room. |   Inf. dB   |
|   Noisy Speech Recording   |   r(s,n)   |   simultaneous recording of speech and noise wavfiles played through room. |  10 * log10 * var[r(s)] / var[r(s,n) - r(s)]
|   Enhanced Speech	 |   x  |   result of passing r(s,n) through enhancement algorithm	 |  10 * log10 * var[r(s)] / var[x - r(s)]
		

### _Lombard Speech_

A live speaker talks in the room while noise is simultaneously played through monitor B. The noisy speech is recorded through the microphone. The D/A and A/D converters are clocked synchronously.  
To obtain a clean reference to the speech, the noise file is played through monitor B in the absence of speech. This noise recording is then subtracted from the noisy speech recording to provide the clean reference.  
This clean reference will have some residual noise in it due to the minor variability in the channel. The SNR of this residual noise can be computed from segments of the clean reference where no speech is present. This was found to be approximately 20-30dB. This noise level is not negligible. However, if the residual noise is assumed to be independent of the speech and noise signals, then the SNR calculations can be easily adjusted to account for it.  



| Variable Name | Notation | Description | SNR Calculation |
|--- |--- |--- |--- |
|Noise Wavfile|n|16 bit data waveform|Not defined|
|Noise Recording|r(n)|recording of noise wavfile played through room.|-Inf. dB|
|Noisy Speech Recording|r(sL,n)|simultaneous recording of live speech and noise file including room acoustics.| 10 * log10 * (var[cL]-var[ecL] )/ (var[r(s,n)-cL]-var[ecL])
|Cleaned Speech Reference|cL = r(sL,n) - r(n)|noisy lombard speech recording minus noise recording.| 10 * log10 * (var[cL]-var[ecL]) / var[ecL]
|Enhanced Speech|x|result of passing r(sL,n) through enhancement algorithm | 10 * log10 * (var[cL]-var[ecL]) / (var[x-cL]-var[ecL])
|Residual Noise|ecL|Segment of cL where no speech is present.|Not defined|



**Monaural Recordings**  
One speech waveform is played through monitor A, with a second speech file simultaneously played through monitor B. The noisy speech is recorded through the microphone. All D/A and A/D converters are clocked synchronously.

Clean references are obtained by individually playing the first speech file through monitor A, and the second speech file through monitor B.

As with the Noisy Speech Recordings, there is a negligible amount of residual noise in clean reference recordings, due to channel variability. This residual noise is negligible, and is ignored in the SNR calculations.

| Variable Name | Notation | Description | SNR Calculation |
|--- |--- |--- |--- |
|1st Speech Wavfile|s1|16 bit data waveform|Not defined|
|2nd Speech Wavfile|s2|16 bit data waveform|Not defined|
|1st Clean Speech Recording|r(s1)|recording of speech wavfile played through room.|Inf. dB|
|2nd Clean Speech Recording|r(s2)|recording of speech wavfile played through room.|-Inf. dB|
|Monaural Combined Recording|r(s1,s2)|simultaneous recording of two speech wavfiles played through room.|10 * log10 * var[r(s1)]/ (var[r(s1,s2)-r(s1)])
|Enhanced Speech|xi|result of passing r(s1,s2) through enhancement algorithm|10 * log10 * var[r(si)]/ (var[xi-r(si)])



### _Original Speech Files:_

TIMIT Wav files - 16 bit, 16KHz.

"Vega" - Vocal by Susan Vega - 16 bit, 44.1KHz.

### _Original Noise Files:_

**F-16 noise** acquired by recording samples from 1/2" B&K; condenser microphone onto digital audio tape (DAT). The noise was recorded at the co-pilot's seat in a two-seat F-16, traveling at a speed of 500 knots, and an altitude of 300-600 feet. The sound level during the recording process was 103 dBA.It was found that the flight condition had only a minor effect on the noise. The reproduced noise can therefore be considered to be representative. sampling rate: 19.98 KHz. A/D: 16 bit. pre-filter: anti-aliasing filter. pre-emphasis: none. filter: none. Source: Examples from NOISEX Database.

**Factory Noise** acquired by recording samples from 1/2" B&K; condenser microphone onto digital audio tape (DAT). This noise was recorded in a car production hall. sampling rate: 19.98 KHz. A/D: 16 bit. pre-filter: anti-aliasing filter. pre-emphasis: none. filter: none. Source: Examples from NOISEX Database.

**Pink Noise 1** acquired by sampling high-quality analog noise generator (Wandel & Goltermann). Exhibits equal energy per 1/3 octave. sampling rate: 19.98 KHz. A/D: 16 bit. Source: Examples from NOISEX Database.

**Pink Noise 2** (combined with "Vega" file). Computer generated using _CoolEdit_ .

**Volvo 340 Noise** acquired by recording samples from 1/2" B&K; condenser microphone onto digital audio tape (DAT). This recording was made at 120 km/h, in 4th gear, on an asphalt road, in rainy conditions. sampling rate: 19.98 KHz. A/D: 16 bit. pre-filter: anti-aliasing filter. pre-emphasis: none. filter: none. Source: Examples from NOISEX Database.

**White Noise** acquired by sampling high-quality analog noise generator (Wandel & Goltermann). Exhibits equal energy per Hz. bandwidth. sampling rate: 19.98 KHz. A/D: 16 bit. Source: Examples from NOISEX Database.

**Bursting Noise** computer generated using a white Gaussian random number generator.