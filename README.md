# Classification of Celestial Objects with Machine Learning Algorithms
<p align="center">
<img width="800" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/b94bae56-0d1d-4322-8fe2-70bf0b87cad6"></p>

Spiral galaxy LEDA 2046648 as captured by NASA’s JWST[^JWST]
<br><br><br>

## SUMMARY

Humans have been looking to the sky with wonder and curiosity since our species originated. In the modern era, sophisticated telescopes allow us to analyze celestial objects that exist billions of light-years away from Earth. By examining astronomical sky survey data, we can understand more about the universe, including what characteristics different classes of celestial objects may share. To that end, machine learning and data science techniques can help us to parse sophisticated datasets where many measurements are taken for a single observation. In this project, a <i>k</i>-Nearest Neighbors (<i>k</i>-NN) model and a Decision Tree model are applied to labeled Sloan Digital Sky Survey (SDSS) data in RapidMiner Studio to determine which algorithm is most appropriate and accurate for identifying three types of celestial objects within an astronomical dataset. Contrary to what might be assumed, findings indicate that the simpler algorithm, the Decision Tree, was able to categorize this dataset better and more accurately than the <i>k</i>-NN model.


## INTRODUCTION

The Sloan Digital Sky Survey (SDSS) is one of the most comprehensive and widely cited astronomical surveys in history. Sponsored by the Alfred P. Sloan Foundation with support from the National Science Foundation, the Simons Foundation, and the Heising-Simons Foundation, SDSS endeavors to help humanity achieve a deeper understanding of the structure and origins of the universe[^2]. 

A partnership with the Carnegie Observatories, the current phase of SDSS, SDSS-V, employs the Sloan Foundation Telescope at Apache Point Observatory (New Mexico, USA) and the Irénée du Pont Telescope at Las Campanas Observatory (Atacama, Chile) to capture over a third of the night sky in both hemispheres[^2] to provide open-use, comprehensive astronomical data to the public for the purposes of studying stars, galaxies, black holes, and more. 

The purpose of this project is to determine whether a machine learning classification algorithm can correctly classify objects within astronomical data, and further, to understand which classification algorithm is best for doing so. Considering the nature and size of the dataset, two classification models have been selected as best options for determining the classes of celestial objects: 

1.	<i>k</i>-Nearest Neighbors (<i>k</i>-NN) is a supervised classification algorithm that groups each individual observations within novel data based on its proximity to labeled data points, as though the labeled data were a reference table to check against[^3]. In a dataset with a high number of attributes per observation, this is a complex and time-consuming task. 

2.	A Decision Tree is a supervised classification algorithm which works by memorizing training data and then making classification decisions within novel data in the form of a flowchart, where each decision node occurs at its the highest possible level of homogeneity in the dataset[^3]. This results in a “tree” which groups the data by the largest groups at the top and branches off smaller groups progressively until all the data is categorized. Where a <i>k</i>-Nearest Neighbors model creates a mathematical relationship between observations, a Decision Tree merely generalizes relationships within the dataset[^3].


## DATA AND METHODS

The dataset used in this project to classify observed celestial objects as a star, a galaxy, or a quasi-stellar object (QSO) is labeled data derived from the most recent survey by the SDSS-V, Data Release 18, and contains 100,000 observations characterized by 42 attributes and 1 classification, or type of celestial object[^5]. These data are compiled using optical, ultraviolet, and infrared observations, and measurements to understand redshift and photometric banding of each celestial object are calculated using spectroscopy[^4]. 

Several attributes in the dataset are identifiers or reference columns to track when and where observations occurred. Measurement attributes include Petrosian radii, Petrosian fluxes, Petrosian half-light radii, Point Spread Function (PSF) magnitude, and the axis ratio of exponential fits of objects observed in the ultraviolet, green, red, infrared, and near-infrared photometric bands[^5]. These attributes help to characterize the observed size, brightness, color, and shape of celestial objects[^5]. Finally, the redshift attribute describes the distance and motion of celestial objects in relation to Earth[^6]. 

### <i>k</i>-Nearest Neighbors

To begin the process of training and implementing the <i>k</i>-NN model, the dataset was first imported into RapidMiner Studio and prepared by applying the Set Role operator to designate “class” as the dataset label, indicating to RapidMiner that this was the desired attribute for which to create a prediction. In general, the dataset was very clean and complete, so did not require extensive preparation prior to analysis.

The data was then split into a training dataset (70% of total rows) and an unlabeled dataset (30% of total rows), and each batch fed to a weighted <i>5</i>-NN model (the default setting of <i>k</i> = 5) separately. Next, RapidMiner ignored the existing class column in the unlabeled dataset to create its own <i>k</i>-NN prediction as to whether each celestial object fell into the star, galaxy, or QSO class based on each data point’s proximity to labeled data.

Optimizing the dataset for a second run was necessary to improve the model. As the dataset is quite detailed, this included optimizing the model run time and prediction accuracy by using the Select Attributes operator to remove extraneous reference attributes from the dataset. These included the object ID, spec object ID, run number, rerun number, camera column, field number, fiber ID, modified Julian date, and plate number. 

### Decision Tree

To prepare the dataset for processing with a Decision Tree model it was imported into RapidMiner Studio, where extraneous reference attributes were removed using the Select Attributes operator and the class attribute was designated as the label with the Set Role operator. Again, no extensive preparation of the data was necessary as it was already clean and complete.

The Decision Tree parameters were adjusted to a maximal depth of 20 with a minimal leaf size of 10. Pruning and pre-pruning were both applied to the model.


## RESULTS

The <i>k</i>-NN model using a weighted <i>k</i> of 5 produced a correct prediction of celestial object class with 93.56% accuracy. 

<p align="center">
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/3f1818e0-a1be-428d-baf5-fa795c0ecfc4"></p><br>

Lowering the <i>k</i> value to 3 did not make much difference to the accuracy of the model (93.90% accuracy). However, even optimized, the model was cumbersome to run with quite a long duration of execution. At 100,000 observations and 33 attributes, the SDSS-V dataset is of moderate size and complexity. Running larger, more detailed astronomical datasets through a <i>k</i>-NN model for the purposes of classification would require significant time and resources.

<p align="center">
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/2b1b43c3-bb89-4d04-a06c-b330c468c3eb"></p><br>

The Decision Tree model produced a correct prediction of celestial object class with 98.64% accuracy. 
<p align="center">
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/9bc6e01f-e30e-4ce2-a0cf-1ece411b0511"></p><br>

The model was not only able to categorize the data much more naturally with higher accuracy than the <i>k</i>-NN model, but additionally provided much deeper insight as to which astronomic measurements could broadly be used to define groups of celestial bodies. After two runs of this model with minor adjustments to tree depth and leaf size, the Decision Tree was able to produce accurate predictions using only redshift measurements. This makes a great deal of logical sense: in astronomy, stars, galaxies, and quasi-stellar objects are easily stratified by their distances from Earth, and this would be reflected in the measurement of each object’s redshift. 
<p align="center">
<img width="400" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/97a7811e-55c7-4ac6-9b88-497c588aaf43"></p>

In conclusion, the Decision Tree model was found to be more appropriate for this astronomical survey dataset than the <i>k</i>-NN model due to its shorter run time and the simplicity of its categorization logic. Additionally, the Decision Tree produced a 5% higher percentage of accurate classification predictions than the <i>k</i>-NN algorithm.


## APPENDIX

### RapidMiner Studio Processes
#### <i>k</i>-Nearest Neighbors

Design Page:<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/119ee7db-5c44-43bc-a6ba-620a9586fd6e">

Exclusion attributes:<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/85175c28-c543-43fc-998f-c4d354bb2478">

Set class attribute as label:<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/507084e8-998f-4db7-ba63-27709b7794c8">

Partition data for training and application datasets:<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/90b2a79c-88c1-40d7-8239-f7c03e5711da">

Assign <i>k</i> value, weights, and measure types (see Parameters box):<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/c421c180-ac07-4f33-98ab-c08d0004f4c7">

#### Decision Tree

Design Pages:<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/4854b574-9cdf-4a83-9104-c169d4c37028">
<br><br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/1f9bd4c2-db7e-4a71-928d-bddcb7893c8a">

Exclusion attributes:<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/d1c195b8-9e5b-4bc5-8add-15c3fbf9b926">

Set class attribute as label:<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/7077f996-8aaf-466f-8721-2c92a367407b">


### Data Output
#### <i>k</i>-Nearest Neighbors

Correct Predictions:<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/6c0a8fa5-e92c-4bdf-8338-f375dbaebfdd">

Wrong Predictions:<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/22ad38e5-b2ba-4b08-8028-948508078d5e">

Performance:<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/a58b5f67-cbef-46a7-86ad-6c78db39db86">

#### Decision Tree

Correct Predictions:<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/2e4ab26c-2e84-4c7d-a836-8b6141e84bb9">

Wrong Predictions:<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/1e3f0298-35ce-4a95-acdc-8c48efeb396c">

Performance:<br>
<img width="700" alt="image" src="https://github.com/morgan-turner/SDSS_analysis/assets/92185928/102632e4-b0e9-4a40-ac77-36a06cb33466">


## WORKS CITED

[^JWST]: ESA/Webb, et al. “A Spiral Amongst Thousands.” NASA’s James Webb Space Telescope Photostream, 31 Jan. 2023, [https://www.flickr.com/photos/nasawebbtelescope/52660766241/in/album-72177720305127361/]. Accessed 3 Aug. 2023. 
[^2]: <i>Sloan Digital Sky Survey: Alfred P. Sloan Foundation</i>, sloan.org/programs/research/sloan-digital-sky-survey. Accessed 3 Aug. 2023. 
[^3]: Kotu, Vijay, and Bala Deshpande. “Ch 4: Classification.” <i>Data Science: Concepts and Practice</i>, Morgan Kaufmann, Cambridge, 2019.
[^4]: Sloan Digital Sky Survey. “Data Release 18.” <i>Sloan Digital Sky Survey</i>, 26 July 2023, www.sdss.org/dr18/.
[^5]: R, Farid. “Sloan Digital Sky Survey - DR18.” Kaggle, 29 July 2023, [www.kaggle.com/datasets/diraf0/sloan-digital-sky-survey-dr18].
[^6]: “Redshift.” <i>Las Cumbres Observatory</i>, [lco.global/spacebook/light/redshift/]. Accessed 6 Aug. 2023. 

