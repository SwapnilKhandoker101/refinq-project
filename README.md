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
We have to read it after all.

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
something._
