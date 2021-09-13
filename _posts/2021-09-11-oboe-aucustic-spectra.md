---
layout: post
title: "Oboe Acoustic Spectra Analysis"
author: Nathaniel S. Keplinger
categories: projects
date: 2021-09-11
usemathjax: true
---

# Introduction

This project began when Gillian Lopez, an oboist of the Trinity University music department approached the Trinity University Physics and Astronomy Department with a proposal to study whether a musician’s perceived acoustic differences in oboes are identifiable in their acoustic spectra. It is understood in the oboist community that there is a trade off between dollar cost and acoustic quality when selecting oboes. In general, plastic bodied oboes are introduced to beginner players as a low cost option to begin their music education. Although plastic is cheaper, many oboists can tell the difference in lower quality of sound compared to wooden models.

This project aimed to understand the degree to which these material differences affect the timbre, or ”tone color”, of an oboe. Additionally, this project aimed to understand if one could distinguish on oboe from another by analysing the acoustic spectra. Understanding the degree to which one can distinguish between the oboe timbre has implication the types of oboes music teachers ought suggest to their students.

A note played by a pitched instrument, like the oboe, is primarily characterized by the fundamental frequency of the note played. The fundamental frequency of a note played by an instrument is lowest frequency and helps define the musical tone or pitch of the note. The full acoustic spectrum of a sound wave is also comprised of integer multiples of the fundamental frequency which are thought to make up the timbre of the note played. Through mathematical analysis, we can study the differences in acoustic spectrum between different oboes playing a series of different notes.

For this project we studied the sounds produced by six mystery oboes of unknown, make, model, and material. These mystery oboes played a series of four known notes recorded at a sample rate of 44100 Hz. Analyzing audio recordings with minimal information about the oboes themselves allowed us to test the extent to which we could distinguish between oboes solely using their acoustic spectra. An example of the information that was given to us is shown in figure 1. The musician, for all six oboes, played a B-flat, F, D, and A notes which correspond to fundamental frequencies of 234, 294, 349, and 440 Hz respectively.

# Acoustic Feature Extraction and Analysis

The primary mathematical tool used to investigate the timbre of an instrument is the discrete Fourier transform (DFT). The DFT is mathematical transformation that takes a discrete signal represented in time and decomposes it into temporal frequency space. The DFT and the inverse DFT are shown in the equations (1) and (2). 
$$
\tag{1}
g_k = \sum_{n=0}^{N-1} f_n e^{-i \omega_k t_n} 
$$

\begin{equation}
\tag{2}
    f_k = \frac{1}{N} \sum_{n=0}^{N-1} g_k e^{i \omega_k t_n} 
\end{equation}

Where the subscripts n and k represent the functions g and f in their respective spaces. ω is the angular frequency and t is time. 
<img src="{{ site.baseurl }}/projectAssets/Oboe1.png">
*Figure 1: An example of the original audio file provided for analysis. Each peak represents a note played from which we can window a one second sample.*  

There are limitations to amount of information that can be extracted using a DFT. The sample rate of the sampler of a discrete signal limits the highest measurable frequency of a discrete signal. This highest measurable frequency is called the Nyquist frequency and it defined in equation (3), where $$f_{Nyquist}$$ is the Nyquist frequency and $$f_S$$ is the sample rate.

\begin{equation}
\tag{3}
f_{Nyquist} = \frac{f_S}{2}
\end{equation}

Given a set of audio files, a known sample rate, and knowing what note was played, we can glean some information from the acoustic spectra Oboes. Before doing so it is important to understand how best to normalize the information revealed by the DFT. For the sake of consistency across different oboe models, we normalized the all harmonics used for analysis to the strength of the fundamental frequency.


After normalization, we employed some statistical calculations to both classify and characterize the acoustic spectra of audio signals produced by unknown oboes. All analysis however stems from the amplitudes and frequencies of the harmonics found by implementing the DFT.

# Fourier Transforming Oboe Audio Signals

The entirety of the spectrum analysis was done with MATLAB code. In MATLAB we windowed each audio recording to extract one second snippets from each oboe playing each note. The one second clips were extracted from the middle of the signal to capture the sound waves in its steady state and avoid differences in the spectrum caused any transient phenomena. From there we wrote a function to take the DFT of each of the windowed signal. We wrote a separate function, to locate the frequencies of the measured harmonics, calculate their complex amplitudes and record the location of each harmonic. A Fourier transformed B-flat note is shown in figure 2.

![Figure 2](/projectAssets/TwoSidedBflat.png)
*Figure 2: The Fourier transform of the a B-flat note played by oboe one. This is an absolute value plot of the DFT coefficients plotted against the frequency.*

An attribute of the acoustic spectra that became immediately apparent was that the strongest frequency component of the signal was not the fundamental frequency as we might of expected. Instead, across all oboes and notes, the strongest harmonics were around the fifth harmonic. In figure 2 it is clear that there are two groups of peaks that mirror each other. It turns out these higher frequencies are part of the spectrum but do not significantly contribute to the to audible sound of the instrument.

# Digitally Reconstructing the Oboe Sounds

One of the first tests we conducted was to see if we would could take the information found in DFT and digitally reconstruct the notes played by each oboe. In doing so we could qualitatively test the extent to which the bulk information is contained in peaks of the acoustic spectrum. Equation (4) shows the signal $$S_{reconstructed}$$ reconstructed with $$N$$ harmonics over a period $$T$$. Where $$Z$$ is the complex amplitude of the peaks at each harmonics, $$Z^*$$ is the complex conjugate of $$Z$$, $$f_n$$ is the frequency at index $$n$$, and $$t$$ is the time.

\begin{equation}
\tag{4}
S_{reconstructed} = \sum_{k=1}^{T}\sum_{n=1}^{N}Z_ne^{2\pi i * f_n * t_k} + Z^{*}_n e^{-2\pi i * f_n * t_k}
\end{equation}

We used equation (4) and 40 harmonics to reproduce and listen to a one second note. The waveform is shown in figure 3. Upon playing the original audio files and comparing it to the reproduction it was difficult to tell the difference with our untrained ears. Our oboist also found it difficult to tell the difference between the two audio signals. This test qualitatively revealed number of harmonics necessary to reproduce a sound with any psychoacoustical difference.

![Figure 3](/projectAssets/Oboe1Reproduction.png)
*Figure 3: A reproduction a one second long B-flat note played by oboe one. The reconstruction was done with a sum of complex exponentials and just 40 harmonics.*

# Power and Convergence

We calculated the total power present in each note played by each due to all the harmonics present in our sampled signal. We calculated the power, $$P$$, using equation (5), where $$Z$$ again is a vector containing the complex amplitudes of all the harmonics and N is the total number of harmonics. Upon plotting this equation, as shown in figure figure 4, we can see that the total energy that each harmonic contributes to the sound is essentially negligible after the tenth harmonic. After the tenth harmonic, the total power converges to some constant at each subsequent higher frequency harmonic.

\begin{equation}
\tag{5}
P = \sum_{n=1}^{N} |{Z_n}|^2
\end{equation}

or any subsequent analysis using the data acquired by the DFT, this test provides validation that any number of harmonics above ten is sufficient for analysis. In terms of the amount of energy in the system, the benchmark of 50 harmonics used in majority of our analysis is more than adequate.

![Figure 4](/projectAssets/Convergence.png)
*Figure 4:  The total power in a one second long B-flat note played by oboe one for a range of harmonics. After around the tenth harmonic the total power added by each additional harmonic is insignificant.*

# Spectral Centroid and Spread

Another analysis technique we explored was the spectral centroid and spread off all oboes and notes. The spectral centroid, defined in equation (6), is the mean frequency across the harmonics of the sample. The spread, equation (7), is the standard deviation. Figure 5 shows the harmonic spectral centroid and spread for all combinations of mystery oboes and notes. These data initially seemed promising in terms of its usefulness distinguishing between oboes based off of their acoustic spectra. Although there seemed to be oboes that had similar centroids and spreads, these plots as a method of classification proved to be a dead end. One could speculate whether a larger spread could indicate greater ”depth” of sound or "richness” in tone but these are adjectives that are difficult to quantify. The centroid calculations in the end were too ambiguous to clearly distinguish different oboes from each other.


\begin{equation}
\tag{6}
Centroid = \frac{\sum_{n=1}^{N} |{Z_n}| * f_n}{\sum_{n=1}^{N} |{Z_n}|}
\end{equation}

\begin{equation}
\tag{7}
Spread = \frac{\sqrt{\sum_{n=1}^{N} (f_n - Centroid)^2|{Z_n}|}}{\sum_{n=1}^{N} |{Z_n}|}
\end{equation}

![Figure 5](/projectAssets/HarmonicCentroid.png)
*Figure 5: The harmonic centroid and spread for all six oboes playing each note.*

# Euclidean Distance Analysis

Our attempts to best distinguish between oboes based on their acoustic spectra found the most success with the implementation of the Euclidean distance equation (8).

\begin{equation}
\tag{8}
d(p,q) = \sqrt{\sum{n=1}^{N-1}(|p_n|-|q_n|)^2}
\end{equation}

This formula takes any two vectors of the same length, like p and q in (8), and calculates $$d$$, their overall distance. The smaller d is, the more similar, on aggregate, the two vectors are. This metric allowed us to identify the most conclusive trend upon comparing two different oboes. Figure 6 shows the comparisons across all six oboes playing each note.

![Figure 6](/projectAssets/FullEuclidianDistance.png)
*Figure 6: The Euclidean distance between 15 possible combination of two oboes and notes played.*

The most obvious result that figure 6 reveals is the similarity between oboes four and five. Across all notes, oboes four and five were consistently the most similar based on the Euclidean distance metric. The analysis of the B-flat note provided further evidence suggesting that oboes four and five might be of similar build or material. As described by Gillian, playing a B-flat note requires the entirety of the oboes to play the note. All holes above the ”bell” or bottom section of the oboes are closed when playing this low note. That is to say that analysis of the B-flat Euclidean distances show which two oboes are most similar across the entirety of the oboes body. Figure 7 shows the Euclidean distance data between all oboes playing the B-flat note. The plot shows the distance between oboes four and five being significantly smaller than the others.

![Figure 7](/projectAssets/BflatStemPlot.png)
*Figure 7: The Euclidean distance between 15 combinations of oboes playing the B-flat note.*


Analysis of the oboes playing the A note reveals a separate trend. As seen in figure 8, all of the oboes when compared to oboe number two produce a distinctly larger Euclidean distance than if it were compared to a different oboe. It is important to note that when an oboist plays the A note only the holes in the upper joint are closed. At this point, even if we do not know the model or material the oboe, we can conclude that perhaps the difference has something to do with the upper joint.

As for the other oboes, our immediate Euclidean distance analysis did not reveal any trends that allowed us to distinguish between oboes on this metric. Perhaps the information that produces some auditory differences between oboes is lost when we are taking an aggregate metric like the Euclidean distance. The nuance of the sound contained in the autistic spectrum could on average be similar yet contain distinct differences at different harmonics. For that reason we were not able to use Euclidean distance information to the same usefulness in distinguishing between oboes, one, three, and six.

![Figure 8](/projectAssets/AStemPlot.png)
*Figure 8: The Euclidean distance between 15 combinations of oboes playing the A note.*

# Conclusion
The analysis of the acoustic spectra of each of these oboe allowed us to identity two oboes of possibly the same model and material. This analysis also allowed us to identify a possible difference in the upper joint of oboe two. Granted, while we were doing this analysis we still did not know much about the model and material of each oboes. Upon reconvening with Gillian to learn the make, model, and material of each mystery oboe we learned the following about each oboe:

- Oboe 1 - Model A Fossati brand all wood grenadilla oboe
- Oboe 2 - Grenadilla Model A Oboe with a plastic resin upper joint
- Oboe 3 - Model A oboe made of cocobolo wood
- Oboe 4 - FX3 grenadilla oboe with middleweight resin finials
- Oboe 5- FX3 grenadilla oboe with heavy resin finials
- Oboe 6 - FX3 grenadilla oboe with wood finials. The top finial is lined with aluminum and the bottom finial has a ring of aluminum around the outside

The Euclidean distance calculations allowed us to correctly identify that oboes four and five are the most similar oboes. Oboes four and five are oboes of the same make, model, and material with the only difference being the finial pieces attached to the end of the instrument . The Euclidean distance calculations for the A note in particular also revealed a possible difference in material or model in oboe two. Oboe two is the only instrument in our data set with. a plastic resin upper joint. This joint accounts for the distinctly different, according the Euclidean distance metric, A note produced by oboe two.

Going forward, additional analysis is needed to further understand how the acoustic spectra differ between oboes one, three, and six. Analysis in the mel-frequency space could reveal which harmonics are most dominant as perceived by human ears.

# Acknowledgements

I would like to thank Dr. Nirav Mehta for advising me throughout the course of my research experience. I would also like to thank Gillian Lopez for introducing the physics department to this project and providing her musical insight. Lastly, I would like to thank the Trinity University Communications department for allowing us to use there recording equipment to the oboe sounds.