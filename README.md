# MIR-Cetacean-Calls

Cetacean Call Examiner
Introduction

In the Salish Sea, a biologically rich environment, many species of cetaceans (whales and dolphins) are at risk due to climate change and human activities. In particular, the Southern Resident Killer Whales (SRKW), a specific ecotype restricted to the Salish Sea, has been reduced to less than 80 total members in recent decades. As a response to ongoing threats, the government of Canada has issued restrictions on how close vessels can approach different species of cetaceans in Canadian watersSRKW2. The strictest restriction is for SRKW which vessels must stay at least 1000m from and other orcas in their range cannot be approached within 200m. Vessels cannot approach other cetaceans within 100mregulations. While these restrictions effective, they can be very hard for boaters to follow in practice due to the fact that cetaceans can travel significant distances underwater, and accurately gauging distance while in the water can be very difficult, even for experienced boaters.

One suggestion for improving approach distance compliance that has gained popularity in recent years is use hydrophones, underwater microphones, to detect and locate nearby cetaceans. Because of the distinctness of their vocalizations, trained individuals can identify cetacean calls, down to the individual pod of orcas, with a high degree of accuracy, which when detected using a hyrdophone array, as seen in Figure 1, can be used to pinpoint the origin of specific calls. Unfortunately, because of the size of the range, and the large number of hydrophones that need to be constantly listening in order to achieve accurate detection, using human annotators for real time detection is infeasible. A solution to this problem that has been proposed and experimented with in recent years is to use machine learning to identify and locate the origin of cetacean calls.

Figure 1: Hydrophone array deployed by Ocean Networks Canada

Barriers to Implementation and Adoption

While the use of machine learning to identify and locate the origin of cetacean calls in real time has been a popular idea in recent years, it has yet to have any real-world implementations. There are a few main reasons for this.

Firstly, the ocean, particularly in areas with high levels of vessel traffic where detection is most important, is noisy. There are high levels of variable background noise from vessels, other animals, and the natural movement of the water, among other things. This can frequently make calls difficult to distinguish or identify, even to a trained ear. The kinds of noise encountered also varies significantly from location to location.

Another major barrier is that while the Salish Sea has a large number or permanent hyrdophones, they have ownership spread across different governments and organizations, using different technologies. This can create major ownership and compatibility issues that any practical system would need to contend with.

The ownership hurdles also impact the next issue which is speed and efficiency. Any real-time detection model would need to be both fast and able to be run across the data from many hyrdophones that are recording 24/7 for both identification and location triangulation. Additionally speed is also impacted by the fact that many of the real-time hydrophones currently running, at least in Canada, have to have their outputs reviewed and approved by the Department of Defense to ensure that classified military information is not accidentally released.

Finally, there are a large number of cetacean species that can be found in the Salish Sea, all of which have a wide variety of vocalizations that they make. Because different species have different restrictions and regulations surrounding them, any classifier would need to be able to distinguish between a variety of calls with a high degree of accuracy. This adds a layer of complication beyond a simple binary call/no-call classification system for practical use.

Project Overview

The project described in this report is the Cetacean Call Examiner. It is a simple program that can take audio recordings and classify whether or not they contain cetacean calls, specifically southern resident killer whales or humpback whales, using machine learning algorithms. The program will also include multiple functions that are intended to improve the audibility and clarity of the calls via de-noising for listeners, particularly for those without prior knowledge or training. The algorithms will be tested both on the raw data as well as the de-noised data to see how the classification accuracy is affected. Overall, the purpose of the project is to serve as an introduction to bio-acoustic data.

Project Goals

This section outlines the specific goals of the project at different levels. These goals were defined before any significant work on the project began.

Basic
Find a suitable cetacean call dataset that can be used for machine learning.
Train a classification model on the calls to classify clips with killer whale calls, humpback whale calls, and no cetacean calls.
Implement spectral gating to de-noise audio samples.
Expected
Implement spectral subtraction to de-noise audio samples.
Implement a human speech de-noising package on the bio-acoustic audio samples.
Train the classification model on de-noised versions of the calls to see if classification accuracy improves in comparison to the baseline.
Extended
Create an algorithm that can identify and mark the beginning and ends of cetacean calls within an audio clip.
Expand the classifier to include data from other labeled datasets.
Achieved

All the goals in the basic and expected levels were completed in the scope of the project. However, the goals in the extended level were not completed. This is largely due to time constraints. This project involved training multiple Convolutional Neural Networks with a sizable dataset, which I did not have prior experience with, resulting in a significant amount of trail and error. Each time a model was trained it took between 20 minutes to an hour and a half. Three different CNN models were trained, one as the baseline on the raw spectrograms, one on the spectrograms with spectral subtraction, and one on spectrograms with Per-Channel Energy Normalization. This meant there was ultimately not enough time to complete the extended goals.

De-Noising Algorithms

Background noise is a significant problem in bio-acoustics, particularly in cases where sounds are recorded in non controlled settings. Many of the most popular methods for de-noising sound samples either for human classification or machine learning have been taken from the fields of human speech detection and music information retrieval. Three popular methods were explored in the scope of this project. For demonstration purposes all three algorithms were applied to the same SRKW vocalization sample shown an unaltered spectrogram in Figure 2.

Figure 2: Spectrogram of an unaltered Southern Resident Killer Whale vocalization

Spectral Gating

The first de-noising algorithm explored was spectral gating. In short, the way that spectral gating works is by taking the mean amplitude value for each sampled frequency in an audio clip. Then those mean values are used to create a gate that bands in that frequency must surpass by a defined threshold or be filtered out. This algorithm for noise reduction is known to work well where noise is relatively quiet in comparison to the main signal, but can create choppy audio when the noise is loud, or inhabits a different frequency range than the desired sound.

Spectral gating was able to make the calls appear more visually distinct when looking at the spectrogram, particularly when a high threshold value was used, as can be seen in Figure 3. However, it lead to audio clips that sounded very choppy, by changing the mostly consistent background noise to a background noise that rapidly started and stopped across the different frequency bands. This had the opposite of the intended effect by sometimes making it harder to discern some of the vocalizations for human listeners. When a threshold value was used that was high enough to remove most of the background noise all together, many of the more subtle aspects of the vocalizations were also removed.

Ultimately, no model was trained using the samples de-noised using spectral gating. The primary reason for this was an inability to find an suitable threshold value that was effective across a selected subset of audio samples.

Figure 3: Spectrogram of a Southern Resident Killer Whale vocalization with spectral gating at a threshold of 1.5 applied

Spectral Subtraction

The next de-noising algorithm experimented with is known as spectral subtraction. It works under the assumption that background noise is mostly constant. A audio snippet is taken from the audio sample from a time that is just noise with no sounds of interest. This snippet is then used to calculate an average noise spectrum that can then be subtracted from the rest of the audio sample, zeroing out any negative amplitude values. This algorithm is known to work best in cases where the noise is constant, and where there is a portion of the audio sample that is only noise.

In this case spectral subtraction was effective at removing the consistent background noise while leaving some of the more infrequent noises, both the vocalization and other background noise. This is the expected behaviour for this algorithm. It was able to improve he distinctness of the vocalizations both through audio, for the human ear, and visually via the spectrogram, as can be seen in Figure 4.

Figure 4: Spectrogram of a Southern Resident Killer Whale vocalization with spectral subtraction applied to it

Per-Channel Energy Normalization

The final algorithm explored, Per-Channel Energy Normalization (PCEN),is primarily used for machine learning, in particular spectrogram based models, and is not strictly speaking a noise-reduction algorithm but can serve the same purpose as one for this project. PCEN is popular for use both in human speech as well as bio-acoustics. PCEN works by taking a running average of the spectrogram frequency amplitudes. Each time-frequency point is then divided by that running average and then non-linearly compressed, typically with a logarithm. PCEN is able to increase the contrast of non-noise frequency-time points in a spectrogram and is known to be particularly effective for sounds that are relatively faint, making it a popular choice in bio-acoustics.

PCEN is distinct from the other two de-noising methods explored in this project because it is used to process spectrograms rather than audio, and is designed with machine readability, like for deep learning, rather than for improving the human listening experience.

For the vocalizations it was tested on, PCEN was effective at removing the background noise from the samples, however it also had the tendency to over-normalize the actual vocalizations. The vocalizations were mostly left in the resulting spectrogram but often appeared very faint, as can be seen in Figure 5. This may be an issue that can be tuned with better parameter selection but was not figured out in this project.

Figure 5: Spectrogram of a Southern Resident Killer Whale vocalization with Per-Channel Energy Normalization Applied to it

Cetacean Call Data Set

The primary data set used in this project was recorded from a single cabled hydrophone located off the coast of San Juan Island in Washington state. The dataset is comprised of 3 second sound samples that were labelled as being either SRKW, humpback songs, or negative (containing typical, non-vocalization sounds for the area) by trained acousticians accustomed to bio-acoustic datadataset:1. A clear example of each of these classes can be seen in Figure 6. For this project a subset of 2100 total audio samples were used, with an equal distribution between the three classes. Of these selected samples, a total of 100 from each class were set aside to be used for testing.

Figure 6: Representative spectrograms of the three audio classes examined, in the order Southern Resident Killer Whale, Humpback Whale, and Negative on the bottom.

Machine Learning Exploration

For this project a Convolutional Neural Network (CNN) was used to create the classification model. The model was trained using spectrograms of the 3 second audio clips. The use of spectrograms for bio-acoustic classification is common due to the prevalence and development of computer vision models in recent years. This however, leads to the downside of requiring that the raw audio data goes through some processing to generate the spectrograms, increasing the computational load of the models.

The different CNN's were set up using the same architecture so that the efficacy of the different de-noising algorithms could be compared. All featured two 2D convolutional layers with batch normalization and maxpooling to extract stable features which then have global average pooling applied to condense those features. This is then followed by a dense classifier with dropout to help prevent over-fitting, a common issue with neural networks. The model was given up to 25 epochs to train, with early stopping implemented with a patience value of 5.

Raw Data Model

The first model was trained using the raw data, with preprocessing restricted to converting the audio signals into spectrograms. This was the baseline to see how the de-noising algorithms performed and to explore the basics of how bio-acoustic classification works.

This baseline classifier was able to achieve a validation accuracy of 76.7%. It was able to classify the humpback calls with the most accuracy, correctly classifying 93 out of 100 samples. However the classifier struggled more with the SRKW samples of which only 58 were classified correctly. This is to be expected for a couple of reasons. First of all, there are several different kinds of SRKW vocalizations which are distinct from each other, making their classification more complex. Also some of the SRKW vocalizations, particularly the clicks, are faint in volume and very brief, which makes it more likely to get lost in the aggregation portions of the model or mistaken as noise.

Figure 7: Confusion matrix for the model trained using the unaltered spectrograms

Spectral Subtraction Model

The CNN model, with the same architecture as the raw model, was retrained using spectrograms that had spectral subtraction applied to their audio. While doing a cursory exploration of a random subset of different vocalization samples, spectral subtraction was able to effectively clarify many of the vocalizations for human listening.

The model trained on the de-noised data was able to achieve a validation accuracy of 93.0%. Surprisingly, it was able to classify SRKW vocalizations with the highest accuracy with 97 out of 100 classified correctly followed by humpbacks at 95 correct classifications.

This was achieved with a relatively naive application of spectral subtraction where the first 10% of each audio sample was assumed to be noise, regardless of which section of the sample was actually noise or vocalization. Most samples only had vocalizations, or sounds of interest for the negative samples, for a small portion of the overall sample. This also provides a potential explanation for why SRKW vocalizations were classified the most accurately. Many of those vocalizations were very short making it more likely that the first 10% of the audio is actually just noise and not vocalizations, making spectral subtraction more effective.

A potentially more effective application of this de-noising algorithm could be one where the start and end times for each vocalization were estimated so that the noise estimate used in the subtraction could be chosen from a portion of the audio that is just noise.

Figure 8: Confusion matrix for the model trained using the spectrograms with spectral subtraction applied to them

PCEN Model

The final model was trained using data that had the PCEN algorithm applied to the audio sample spectrogram. It was able to achieve a validation accuracy of 63.7%. Interestingly, this model preformed worse than the baseline model using the raw data. In particular, the model was able to classify the negative samples relatively accurately, with 94 of the samples classifeid correctly. The SRKW and humpback samples were correctly classified with 49 and 48 samples respectively classified correctly out of 100. The overwhelming majority of the incorrect classifications were mistakenly classified as negative by the model. The model trained on PCEN spectrograms also took longer to train in comparison to the other two models, needing 10 epochs to achieve a validation accuracy over 50%.

One possible explanation for why the model was less effective is that some of the PCEN de-noised samples had the vocalizations normalized out. When manually inspecting the samples it was noticed that some of the humpback sounds had the vocalizations removed with the noise, as they overlapped in frequency. It was also noticed that some of the SRKW clicking vocalizations, which appear very small on the unaltered spectrogram were missed in the normalization process.

Figure 9: Confusion matrix for the model trained using the spectrograms with Per-Channel Energy Normalization Applied to them

Project Deliverables

The source code for the project is available on GitHub (https://github.com/KCCalder/MIR-Cetacean-Calls) which is run through Google Colab, along with the subset of the cetacean call dataset used in this project available in a zip file. The raw data, including the audio samples that were not used in this project, are available on Zenodo (https://zenodo.org/records/15390884).

Conclusions

Ultimately, out of the de-noising algorithms explored in the scope of this project, spectral subtraction was found to be the most effective at preparing the bio-acoustic audio samples for both deep learning and human consumption. The generalization ability of spectral subtraction in real-world settings remains unknown because all the samples used in this project were from a single hydrophone. In order to test the generalizability, data samples from more hydrophones from different locations should be included in the model, significantly increasing the overall number of data samples.

The parameter tuning was discovered to be an important aspect of the de-noising process that unfortunately was not well explored in the scope of this project. PCEN was not particularly effective in this project but that may be more reflective of poorly tuned parameters rather than the overall efficacy of the algorithm. If I was to do this project again I would dedicate time to hyper-parameter optimization of the de-noising algorithms in order to get a more accurate examination of which are most effective for this use case.
