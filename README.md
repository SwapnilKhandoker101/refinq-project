# Vegetation Analysis Project Using NDVI Satellite Data

## Overview

Welcome to the Vegetation Analysis Project, a small problem designed to evaluate
your expertise in processing, analyzing, and interpreting remote sensing data. This
project focuses on the Normalized Difference Vegetation Index (NDVI) data derived from
GeoTIFF files sourced from the Copernicus Sentinel Hub. You will explore NDVI data
snapshots of Naturpark Karwendel to assess environmental changes.

## Data

You are provided with GeoTIFF files containing NDVI (see section `Understanding NDVI`)
data for Naturpark Karwendel at different times. These files are located in the `data`
folder of this repository. You can use the `rasterio` library to read the GeoTIFF files
and work with the NDVI data, but you are free to use any other libraries you prefer. The
files contain not only the NDVI data, but also the geographical information (e.g.
coordinates, projection, etc.) of the area.

## Project Goal

We have a fictional client operating a manufacturing plant in Scharnitz near Naturpark
Karwendel. Through their operations, they use a significant amount of water and emit
non-GHG emissions. Therefore, it is necessary to assess the integrity (health) of the
bordering protected area, Naturpark Karwendel.

The primary goal of this project is to answer the following question:

_"Is the vegetation in Naturpark Karwendel changing (declining or growing) over time?"_

This is a broad question, and you can approach it in several ways, and there is no
definite right or wrong answer. We are more interested in your thought process and
approach, than the actual result. Also we love some clean code and good documentation!
We have to read it after all ;)

## Getting Started

1. **Fork and Setup:** Fork this repository to begin your project.
2. **Analyze:** Dive into the data using your chosen analytical tools and methodologies.
3. **Document:** Share your thought process, analysis, and conclusions in the README
   under `Documentation` and/or in code comments.
4. **Code and Tools:** Place your scripts, notebooks, and any other utilities in
   the `src` directory.
5. **Submit:** After completing your analysis, update your fork and email us the link to
   your repository (markus@refinq.com).

## Questions?

If you have any questions or require further clarification about the project or the
dataset, please feel free to reach out at any time!

## Understanding NDVI

The normalized difference vegetation index is a simple, but effective index for
quantifying green vegetation. It is a measure of the state of vegetation health based on
how plants reflect light at certain wavelengths. The value range of the NDVI is -1 to 1.
Negative values of NDVI (values approaching -1) correspond to water. Values close to
zero (-0.1to 0.1) generally correspond to barren areas of rock, sand, or snow. Low,
positive values represent shrub and grassland (approximately 0.2 to 0.4), while high
values indicate temperate and tropical rainforests (values approaching 1).
More info on NDVI can be
found [here](https://custom-scripts.sentinel-hub.com/sentinel-2/ndvi/)
and [here](https://eos.com/ndvi/).

## Documentation

_This section is for you to detail your project's journey. Describe your approach,
thoughts, findings, and any challenges you encountered. Don't hesitate to also describe
things that didn't work. The more you share, the better we can understand your thought
process and approach. Also use comments in your code to explain why you are doing
something. You may also include an outlook and propose innovative ideas for enhancing
vegetation change detection, such as incorporating machine learning models to predict
future trends or suggesting the integration of additional datasets
(e.g., soil moisture or temperature data)._

## Vegetation Analysis of NaturPark Karwendel

### Introduction

In this project, I analyzed the vegetation changes in NaturPark Karwendel which is a
protected area near Scharnitz.The primary objective is to detemine whether the 
vegetation in this area is declining or growing over time. The analysis uses NDVI
(Noormalized Difference Vegetation Index) data extracted from satellite images
(Copernicus Sentinel Hub) to assess vegetation health.

###  Data Sources

The notebook utilizes satellite images in GeoTIFF format. NDVI data is extracted from images.
NDVI is a standard measure used in remote sensing to estimate vegetation health and coverage.
Generally, it is calculated using NIR(near Infrared Red) channel and red channel. Initially,
I thought the data is taken by sentinel 2 satellites and it consists of 13 bands where band8 
represent NIR and band4 represent Red according to [this](https://custom-scripts.sentinel-hub.com/sentinel-2/ndvi/).
Later, after inspecting the image I found that the ndvi data is given as 2D array which represent one channel,
and then assume its the ndvi data and I donot need to write code for calculating it since its already given.
As mention in the project description other information such as geographical information (e.g.
coordinates, projection, etc.) of the area is also given.

By calculating the longitude and latitude, I found out that the image 1 and image 2 are of 
different places of the park whereas image 2 and image 3 is of the same place.I will
explain how I arrived at this point later.

## Methodology

### Initial Setup:

I have used jupyter notebook for my analysis and I have used the following libraries:

rasterio==1.3.9
torch
torchvision
numpy
rasterio
matplotlib
plotly
seaborn

To see few visualiztion , run the notebook in local machine then the plots which are 
done with plotly will be visible in notebook

### Step01:
Initially I tried to gain some initial intuition about the data by plotting the tiff images
with the help of  this function : `def display_tiff_images(directory)`.
My looking into the images, the initial idea I gained is that vegetation was declining
since green regions was decreasing and the light-brown region was increasing and in 
last image which was taken in 2024-05-15, it seems there was a great decline of vegetation
and barren land increases significantly.

### Step02: 
Then I inspect the image data by reading them using the function `read_ndvi_from_file(file_path)`.
I then also extracted the ndvi data , affine and geo-information data from the images using the function
`extract_info(directory)`

#### Affine data:

Affine data which I extracted includes the data for affine transformation.The affine transformation
typically includes information on the scale, rotation and translation of the image.Here, the 
affine transformation is in the format of a 3 X 3 matrix and commonly structured as:
| a, b, c |
| d, e, f |
| 0, 0, 1 |

(a, b, c) are the coefficients affecting the x-coordinates (longitude).
(d, e, f) are the coefficients affecting the y-coordinates (latitude).
(c, f) is the top-left corner of the image in geographic coordinates.

Using this affine transformation I calculated the geographic coordinates(longitude and latitude) for each pixel using
the following formula:

lon = a * col + b * row + c
lat = d * col + e * row + f

I achieved this with the function `get_geographic_coordinates(ndvi_data, affine)` 

Before doing that, I also checked the structure of the ndvi_data and I figured out that it is
2 dimensional numpy array with a shape of (1102,2500)

### Step03:

After extracting all those data I compared the places of images within the park with the help of 
longitude and latitude and I figured out that Image01 and Image02 is of different place within the park since
whereas the image02 and image03 is of the same place of the park since the difference of their 
longitude and latitude is 0

### Step04:

Then tried to look into the statistics of the ndvi values by finding mean,median,standard deviation
and min-max

The following is the statistics that I got for ndvi value of each image:
"
Statistics for Image 1:
Mean NDVI: 0.3374541401863098
Median NDVI: 0.0
Standard Deviation: 0.44071006774902344
Minimum NDVI: 0.0, Maximum NDVI: 1.0


Statistics for Image 2:
Mean NDVI: 0.3221842348575592
Median NDVI: 0.0
Standard Deviation: 0.4391603171825409
Minimum NDVI: 0.0, Maximum NDVI: 1.0


Statistics for Image 3:
Mean NDVI: 0.5459474325180054
Median NDVI: 0.6399999856948853
Standard Deviation: 0.4148198664188385
Minimum NDVI: 0.0, Maximum NDVI: 1.0

"

### Step05:
Then I plot mean NDVI of each images with time. I found that from 2019 to 2023 the ndvi value
decreases but from 2023 to 2024 it increases sharply.

I doubted this behaviour of the data and later found that in 2019 and in 2023, the data
was taken in mid of the year(June) mostly it remain summer(higher temperature during that time of the year) 
during that time whereas in 2024 the data was taken during February(lower temperature).

Weather might affect the amount of light reflected to satellite and might have affected 
the value of band frequency or wavelength of the near-infrared band and typically during 
February,it remains Winter and thus there is a low reflection of red channel compared to 
NIR channel, and thus it yields high NDVI value.Even from images it can be said that the 
amount of barren land in last image is more than rest two.So I think data collection 
method is not consistent.

### Step06:

I tried to categorize the ndvi value into Water, barren-land, shrub/grassland and forest
based on the range of values.

I basically used the following range:

if ndvi_value < -0.1: 

        "Water"

elif -0.1 <= ndvi_value <= 0.1:

        "Barren Land"

elif 0.1 < ndvi_value <= 0.4:


         "Shrub/Grassland"
else:

         "Forest"


I then plotted histogram based on this value for each of the images. I calculated the 
percentage of each categories present in an image using the function `calculate_pixel_percentages(ndvi_data)`

Even from this histogram plot it was clear that amount of barren land was higher in 
image1 and image2 and it was increasing from image1 to image2 whereas in image3 it decreases
significantly. Even though image2 and image3 is of the same place of the park but the decrease
of barren land and increase of green land is abnormal so I think data collection is inconsistent.

### Step07:

#### KNN classifier:

Next I tried to run knn on the ndvi data where the nearing value in ndvi array would 
represent a certain classes and from which we could have gain some idea of how the 
ndvi value is spread of the classes which which classes is dominant in the images.

Here with classes I mean the category based on the ndvi values such as :
(taken from the project github page):
The value range of the NDVI is -1 to 1. Negative values of NDVI (values approaching -1) correspond to water. Values close to zero (-0.1to 0.1) generally correspond to barren areas of rock, sand, or snow. Low, positive values represent shrub and grassland (approximately 0.2 to 0.4), while high values indicate temperate and tropical rainforests (values approaching 1).

It was taking a long time to process the data so I had to stop the kernel.

### Step08:

Since knn didnot work I visualize the distribution of ndvi values in each of the image.
Even from this distribution the following things are quite prominent:

Image 01:
More values are near 0 which indicates high proportion of barren land compared to green 
areas.

Image 02:
Compared to image 01 , more values are near zero which indicates increase of barren 
land and also less points around 1 which indicates decreases in vegetation.

Image 03:
Compared to image 01 and image02 , the frequency of value near zero increases 
but freequency of value near 1 increases which indicates barren land decreases 
and green area increases.


### Step09:

Next , I tried to visualize the distribution of each category with interactive plot in 
each of the images and from there also it was prominent that in first two images 
barren land is more and green area is less whereas in image 3 barren land decreases
sharply and green area increases significantly.

## Results

My goal of this analysis was to answer the following question:

"Is the vegetation in Naturpark Karwendel changing (declining or growing) over time?"

From my analysis, I can say that last image seems to bring out some anomaly as the image itself 
does not speak the same as its data and comparing with the first 2 images, the sudden rise of 
ndvi value seems anomalous and the third image is also not taken in the same time gap and in 
same time of the year as the other two. 
Since I am provided with only 3 images, it would be too early to judge it.With more
images, it would have been easier to assess.

Now , if we ignore the third image, then the vegetation in Naturpark Karwendel is 
declining over time, and it might happen because of the large amount of water usage which 
decreasing the water level from surrounding area and making it harder for green area to grow.
Also, emission of non GHG gases might affect the ndvi value.

## Challenges

1. Dataset was too small
2. Data is not collected at regular monthly interval which makes is 
   producing some anomalous result
3. Due to large ndvi arrays Knn didnot works so might need to adapt some new techique
   to make it work

## Improvement 

1. Need to collect some significant amount images so that the size of dataset increases
2. Along with the images, the measurement of water absorption and non-GHG gase emission could 
   have provided 
3. WIth sufficiently large data we could have use some supervised Machine learning techniques
   and predict the affect of use of too much water and emission of Non-GHG gases 
   on vegetation. An example of a machine learning model to do the task 
   could be Random Forest which is a powerful ensemble learning method and is good for 
   regression and classification task.
   
   For running using such machine learning model we would need a dataset that includes
   NDVI values as target value and water absorption, non-GHG gases emissions,land surface 
   temperature, soil moisture as featues.

   A high level approach on how to implement such machine learning techniques:
   1. Data Collection: Gather NDVI data along with corresponding environmental variables
   2. Data Preprocessing: Clean the data and engineer relevant features.
   3. Feature Selection: Determine which features are most predictive of NDVI values.
   4. Model Training: Split the data into training and test sets, and train the model.
   5. Model Evaluation: Evaluate the model's performance using appropriate metrics 
   (e.g., R-squared, RMSE for regression, accuracy for classification).
   6. Model Tuning: Optimize the model by tuning hyperparameters.
   7. Prediction: Use the trained model to predict NDVI based on environmental variables.
   8. Interpretation: Analyze the model results to understand the impact of each variable 
   on NDVI.

   We can also try out other machine learning model and can select the best models.
   Also it is also possible to some deep learning here. Using transfer learning and 
   pytorch pretrained weights for models, we can detect the change of pattern for green
   area and utilize this information to gain furhter insights. In this case we could use 
   come deep learning model such ImageNet, AlexNet etc. 

   We need to keep one thing in mind that the accuracy and usefulness of the model will 
   greatly depend on input data so we might need to handle data preprocessing carefully.
4. We can deploy our model into clouds such aws, azure and can also create interactive
   web-apps to help kpi report for the client and can also include a dashboard which will 
   continouly give update to the client about the vegetation growth rate of naturpark.


























