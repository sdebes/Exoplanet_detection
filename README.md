# Exoplanet detection

This repo contains the 7.5 ECTS project I did on the detection of Exoplanets using machine learning and the Doppler method.

In this project, I determined whether the star HC34411 was orbited by an exoplanet using the Doppler method, the Lomb-Scargle periodogram, and the machine learning clustering algorithm DBSCAN.

# Intro
We often think of the center of our solar system to be the center of the Sun, a point that all the planets orbit. In actuality, all the planets as well as the Sun orbit the collective center of mass of all objects in the solar system. When we detect a distant star we can then determine if any exoplanets are displacing the common center of mass, causing the star to orbit.

# The Doppler Shift
The Doppler shift occurs when wave sources are moving away or towards the observer. The shift either reduces or increases the length of the wave. When the source is moving towards the observer, the wavelengths are bunched up in front of the source, making the wavelengths shorter. With soundwaves, the shift causes the sirens of the passing ambulance to pitch up then down. A passing light source causes the light to become more blue then red.

# The Data
Light from the star HD34411 was recorded using a spectrograph over a period of approximately 400 days. The spectrograph can take in light and reveal the intensity of the wavelengths present in the light. We expect the light sources in the star to produce light of a few specific, constant wavelengths. The emission of light from e.g. the hydrogen atom only has a few allowed energies or wavelengths.

Each wavelength with a high intensity constitutes a peak on the wavelength axis in the data. If we record these wavelengths over a longer period, we would expect the peaks to remain constant, unless the star is in orbit, which would Doppler shift the wavelengths.

# Data Transformation
If the light had been Doppler shifted, we would expect the data to be identical, but with a shift in wavelength in all peaks. So between two measurements, we ask, "How many nanometers is the shift?", which also reveals the difference in radial velocity of the star between the two measurements. The radial velocity of the star was found for each measurement by minimizing the chi-square of the 58x58 matrix containing the pairwise radial velocity difference.

For each peak, a 16-dimensional vector of summary statistics was created, containing the mean, median, std, the error on these, and the top 5 most prominent frequencies and their corresponding intensities.

# Clustering
Each of these 16D vectors was then transformed to 2D points using Principal Component Analysis (PCA), which were clustered using the DBSCAN algorithm. The algorithm was first used to classify signal from noise. It created two clusters where cluster 1 was tightly centered around Origo on the two PCA axes, while cluster 2 was spread out far from Origo.
Cluster 1 being close to origo indicates that it varies little, as we expect from a constant signal, while cluster 2 being spread out means that it varied a lot, which we would expect from noise.

The cleaned signal was then again clustered into 10 potential sources of light. For each of the clusters of peaks, the mean radial velocity was calculated. The radial velocity of the star was then calculated as the average of the radial velocities weighted with the uncertainties of the value of the radial velocities in each cluster.

# Result
Finally, the radial velocity of the star was calculated to be $-0.123\pm0.004$ m/s indicating no presence of an exoplanet. The uncertainty is unreasonably small but is derived from uncertainties inherited in the data, and I did not have the time to figure out the source of the error.
