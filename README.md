# NightWatch-AI

Final project for the Building AI course

## Summary

NightWatch-AI is an intelligent system designed to monitor and analyze light pollution using a combination of satellite imagery, ground-based sensors, and artificial intelligence. The system aims to identify and visualize light pollution hotspots across both urban and rural areas. NightWatch-AI provides valuable insights for urban planners and researchers to make decisions for ecological protection and sustainable urban development.

## Background

Light pollution, caused by excessive artificial lighting, is a growing environmental issue. Despite its broad implications, it remains an overlooked aspect of environmental monitoring. It disrupts ecosystems, affects human health, obscures our view of the night sky, and contributes to energy waste. This project aims to address that gap by offering a scalable, data-driven solution that automatically analyzes and maps light pollution.

The lack of comprehensive, high-quality data makes it hard to monitor light pollution. Current methods, such as satellite observations and manual ground measurements, are too infrequent and sparse to combat the problem.

This project is motivated by a personal interest in star watching and environmental protection. 

## Who can use this project

NightWatch-AI has a wide range of potential users:

* City planners identifying areas where lighting can be reduced

* Researchers tracking light pollution over time

* Ecologists monitoring high-risk areas for conservation efforts

* Citizens that want to engage in better lighting practices

## Data sources and AI methods

Data Acquisition:

A network of ground sensors will be set up in strategic locations, such as urban centers, suburbs, and ecologically sensitive areas. Each unit will contain DSLRs or specialized low-light cameras to periodically capture hemispheric sky images, sky quality meters to measure sky brightness, and GPS and timestamping devices to provide location and time-logging for the data.

Publicly available satellite data (e.g., VIIRS DNB) will be used to provide more context and aid in interpolating the ground data.

Other metadata, such as weatherdata, astrological data, population density and streetlight density will be added in order to find correlations and reduce false positives.

AI Usage:

NightWatch-AI processes and analyzes night-time satellite images using CNN models. The images will first be classified to filter unusable images that feature high cloud cover or high level of moonlight. The remaining images will be labeled according to light intensity, enabling the models to learn spatial patterns associated with different levels of light pollution and estimate sky brightness. These classifications are then mapped onto geographic overlays to identify light pollution hotspots. The data pipeline includes preprocessing steps such as normalization, resizing, and cloud coverage filtering to ensure accuracy. Future work might include adding object detection from ground data, such as streetlights or different light sources.


![Cat](https://upload.wikimedia.org/wikipedia/commons/5/5e/Sleeping_cat_on_her_back.jpg)

<img src="https://upload.wikimedia.org/wikipedia/commons/5/5e/Sleeping_cat_on_her_back.jpg" width="300">



## Challenges

While the project provides insights into where the problem areas are located, it does not directly find solutions.
Satellite images can include noise, such as clouds, moonlight strength or seasonal changes.
If higher resolution data is used in the future, it might raise privacy concerns.

## What next?

In future versions, the project could be expanded into a public web platform that allows users to explore light pollution trends over time and across regions. Additional AI models could be incorporated to predict future pollution based on urban development trends or seasonal variations. It would be interesting to collaborate with city councils to support real-world implementation. One idea could be to develop smart lighting systems autonomously adjust intensity based on AI recommendations.

