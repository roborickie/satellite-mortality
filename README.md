# satellite-mortality
EDIT ML 2022 team 4 project

Satellite Mortality
By: Sayan Bhattacharya, Ananya Pamal, Christal Wang, and An Le


Data Collection

5,175 images were obtained from counties across the United States using the Google Static Maps application programming interface (API), and we used publicly available mortality data from the Institute for Health Metrics and Evaluation


Training the model

We had initially planned to use The Satellite Imagery Multiscale Rapid Detection with Windowed Networks (SIMRDWN) codebase, which builds on the YOLT object detection system, to train our model. However, we soon switched to PyTorch Lightning, an open source machine learning framework, as it was possible to be just as efficient while requiring much less code. SIMRDWN also required a lot of initial setup and was complex to understand, whereas PyTorch Lightning was much easier to get started with. 


Mortality Data

We used publicly available mortality data from the Institute for Health Metrics and Evaluation. This database had the mortality rate of each county in the US, sorted by underlying cause of death.


Satellite Images

Images were taken using post offices as a “point of interest” in each county. Post offices were chosen because they are normally located in the center of a town, usually near its most densely populated area. However, it is true that post offices may not always give an accurate representation of the rest of the county (for example, they are no longer as widely used as they once were). 

First, we randomly chose three post offices per county to obtain images from. We gathered post office location data from Harvard Dataverse’s historical and spatial dataset of post offices in the United States. This dataset included each post office’s coordinates, as well as the county and state in which they were located. We cross referenced the counties that were in our mortality dataset and our satellite image dataset to create an intersection dictionary that only contained valid counties from both sets (totaling to around 1430 counties). 

Using the coordinates we had stored, we could begin to download a maximum of three images per county (as some counties contained fewer than three post offices). The zoom level for each image was set to 17, and each image had a dimension of 400 pixels by 400 pixels in order to ensure that specific features within each image could be easily identified.


Classifying Images

We then classified the images categorically. First, we created a graph of the mortality data, and found that the mortality data was skewed right. 

Based on this, we chose a high mortality threshold mortality value of 538.4612 per 100,000; if a county’s mortality rate was over this 90th percentile, it was considered a county with a “high” mortality. Otherwise, the image was classified as “low”. We then split all the counties in our data into either a “training” group or a “validation” group. Once all the images had been classified, it was time to start training the model.


PyTorch Lightning

Ultimately, we used PyTorch Lightning, a deep learning framework, to train our model. First, we created a data module, with separate training and validation data sets. We built our task using ResNet 34, as it usually has a high validation accuracy. We then created the trainer with 11 epochs to prevent overfitting our data. Next, we trained our data using the “testing” data set. 


Results 

Our goal for the end of the summer was to develop a machine learning model that is capable of predicting the mortality rate of a county based on its satellite image. We slightly modified our model to instead categorize counties as having either high or low mortality rates based on visual markers in their images. When we tested the efficiency of our model on the validation data set, however, we found that the model was only 50% accurate (almost random). We are still working to better our model for our project and obtain better results, but we are hoping to reach our end goal soon.


Discussion

In this study, we aimed to design and implement a machine learning algorithm that could ascertain a county’s mortality rate using satellite imagery. Using IHME mortality data and images obtained using the Google Static Maps API, we were able to utilize PyTorch Lightning and produce results.

There were some limitations in our study. For example, our mortality data only assessed the years between 1980-2019, and our images were taken after that time period. We cannot confirm that the areas photographed had not changed during this time. We only drew images from three post offices, chosen at random, in each county, and we recognize that the features in these images may not be the most accurate representation of the rest of the county. For example, post offices are often located in the center of a town, often near common areas such as parks and town squares. Green spaces such as these have been correlated with a slew of positive health benefits, and the recurrence of these features in the images we analyzed may have skewed our results.
Furthermore, our model could not actually predict the crude mortality rate, but only categorize a county as having a “high” or “low” mortality rate. We were not able to pinpoint specific image characteristics that corresponded to a certain mortality rate.

Future studies could build on our work by obtaining more pictures per county (for example, 5 pictures per POI) in order to have more image features to examine and to produce more accurate results, as well as choosing different POIs and analyzing how the results vary. The number of counties from which images were pulled could be varied as well, along with the selection criteria for inclusion in the study (our study looked to survey as many counties as possible, while future work could sort the counties by population, number of large cities, etc). Finally, the algorithm developed in this study can be used to correlate other socioeconomic/demographic factors, such as race and sex, with image characteristiscs.

Like any other project or experiment, there are variables that are not in our complete control. For instance, we discovered from a few articles that the Census introduced a new Disclosure Avoidance System (DAS). Initially, its purpose was to use differential privacy to increase protection. Differential privacy is a system for publicly sharing information about a dataset by describing the patterns of groups within the dataset while withholding information about individuals in the dataset. Historically, due to Trump’s presidency and administration in 2020, there wasn’t a citizenship question on the Census form, which caused a lot of doubt, especially within immigrant Latinx families. Many articles state that Latinx families are doubtful of how the data will be used, because of “a hostile environment toward immigrants propagated by this administration”. This is only one example. The 2020 miscounts of multiple colored groups were a result of this. In 2020, there was an overcount for the white and Asian populations, but an undercount for black, native american, and Hispanic populations, which can raise some implications. 
