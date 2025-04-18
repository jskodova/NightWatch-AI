# NightWatch-AI

Final project for the Building AI course

## Summary

NightWatch-AI is an intelligent system designed to monitor and analyze light pollution using a combination of satellite imagery, ground-based sensors, and artificial intelligence. The system aims to identify and visualize light pollution hotspots across both urban and rural areas. NightWatch-AI provides valuable insights for urban planners and researchers to make decisions for ecological protection and sustainable urban development.

## Background

Light pollution, caused by excessive artificial lighting, is a growing environmental issue. Despite its broad implications, it remains an overlooked aspect of environmental monitoring. It disrupts ecosystems, affects human health, obscures our view of the night sky, and contributes to energy waste. This project aims to address that gap by offering a scalable, data-driven solution that automatically analyzes and maps light pollution.

<img src="https://i.redd.it/0pcgu4ha31u61.jpg" width="400">

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


## Challenges

While the project provides insights into where the problem areas are located, it does not directly find solutions.
Satellite images can include noise, such as clouds, moonlight strength or seasonal changes.
If higher resolution data is used in the future, it might raise privacy concerns.

## What next?

In future versions, the project could be expanded into a public web platform that allows users to explore light pollution trends over time and across regions. Additional AI models could be incorporated to predict future pollution based on urban development trends or seasonal variations. It would be interesting to collaborate with city councils to support real-world implementation. One idea could be to develop smart lighting systems autonomously adjust intensity based on AI recommendations.

## Code example

```
import numpy as np
import cv2
import os
import tensorflow as tf
from tensorflow.keras import layers, models
import matplotlib.pyplot as plt

# Loading satellite images
def load_images_from_folder(folder, image_size=(128, 128)):
    images = []
    for filename in os.listdir(folder):
        img = cv2.imread(os.path.join(folder, filename))
        if img is not None:
            img = cv2.resize(img, image_size)
            img = img / 255.0  # Normalize
            images.append(img)
    return np.array(images)

images = load_images_from_folder('./satellite_data')

# Filtering of clouds/moonlight
def filter_cloudy_images(images, cloud_threshold=0.4):
    # Simulate cloud detection using average brightness
    filtered = []
    for img in images:
        brightness = np.mean(img)
        if brightness < cloud_threshold:  # assume cloudy/moonlit images are brighter
            filtered.append(img)
    return np.array(filtered)

filtered_images = filter_cloudy_images(images)

# Classification of light pollution
def create_model():
    model = models.Sequential([
        layers.Input(shape=(128, 128, 3)),
        layers.Conv2D(32, (3, 3), activation='relu'),
        layers.MaxPooling2D(2, 2),
        layers.Conv2D(64, (3, 3), activation='relu'),
        layers.MaxPooling2D(2, 2),
        layers.Flatten(),
        layers.Dense(64, activation='relu'),
        layers.Dense(3, activation='softmax')  # Classes: Low, Medium, High
    ])
    model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', metrics=['accuracy'])
    return model

labels = np.random.randint(0, 3, size=(len(filtered_images),))  # 0 = Low, 1 = Medium, 2 = High

model = create_model()
model.fit(filtered_images, labels, epochs=5, batch_size=8)

predictions = model.predict(filtered_images)
predicted_classes = np.argmax(predictions, axis=1)

coords = [(60.1695 + i * 0.01, 24.9354 + i * 0.01) for i in range(len(predicted_classes))]
print(f"Location ({lat:.4f}, {lon:.4f}): Light Pollution = {['Low', 'Medium', 'High'][predicted_classes[i]]}")
```
