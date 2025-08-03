---
layout: post
title: Remote Sensing used to monitor the effect of droughts on water levels in the water reservoirs in the region of Andalucia in Spain (2023)
description: "This project investigates the effects of prolonged droughts on water reservoirs in Andalucia, Spain, using remote sensing data. By analyzing satellite imagery from Sentinel-2, it tracks changes in water surface area and vegetation health over the past five years. The goal is to identify trends in water scarcity and environmental degradation and provide actionable insights to help prevent a looming water crisis in the region." 
skills: 
- EO Browser
- Remote Sensing
- Manual Classification
main-image: /AndalusiaFront.png
---
## Project Documentation
<div style="display: flex; flex-wrap: wrap; gap: 12px; margin-bottom: 20px;">

  <a href="/assets/Earth_Observation_Project_Jasper_Welgemoed.pdf" target="_blank" style="
    background-color: #007bff;
    color: black;
    padding: 8px 16px;
    border-radius: 6px;
    text-decoration: none;
    font-weight: bold;
    font-family: sans-serif;">
    📄 Project Report
  </a>

</div>

## Aim of the project

The aim of this project is to **analyze the impact of droughts on the water levels in reservoirs across the Andalucia region in Spain over the past five years** using **remote sensing techniques**. Ultimately, I **informed and urged the Andalucian government** to take **immediate and effective action** to combat the ongoing drought and **prevent a full-blown water crisis** in the region. 

## Method
- **Satellite Data Selection**: Sentinel-2 L2A imagery was chosen for its high spatial (10–20 m) and temporal (5-day) resolution.
- **Water Surface Analysis**: The surface area of the five largest reservoirs (Iznajar, La Breña II, Guadalcacín, Andévalo, and Negratín) was measured monthly using visual spectrum data and polygon mapping in EO Browser.
- **Vegetation Health Monitoring**: NDVI (Normalized Difference Vegetation Index) was calculated from Near Infrared (Band 8) and Red (Band 4) wavelengths to assess vegetation stress linked to drought.
- **Temporal Resolution**: Data was collected monthly, totaling 60 data points per reservoir and NDVI value across five years to observe trends and seasonal variation.
- **Trend Analysis**: Linear regression was used to estimate yearly trends in reservoir surface area and NDVI values, enabling predictions about future water availability and vegetation health.

<div style="display: flex; gap: 10px; justify-content: center; align-items: flex-start;">

  <figure>
  <img width="971" height="263" alt="image" src="https://github.com/user-attachments/assets/b3b0ce54-746d-4e53-90a6-f6a62fe0c8e5" />
  <figcaption>Figure 1: Manual Annotation of Water Area 1 Year Time Difference  </figcaption>
  </figure>  

</div>

## Results

The analysis revealed a significant negative trend in both water surface area and vegetation health across Andalucia over the past five years:

- **Reservoir Surface Area**: All five major reservoirs showed a consistent decline in surface area. The average loss was approximately **-1.79 km² per year**. If this trend continues, the largest reservoir (Iznajar) could be depleted within **4 years**.
- **NDVI (Vegetation Health)**: NDVI values decreased from **0.3527 in 2018 to 0.2911 in 2023**, with an average yearly decline of **-0.012**. This indicates a steady deterioration in vegetation health, likely due to reduced rainfall and water restrictions at that time.
- **Artificial Watering Impact**: A sharper NDVI drop was observed after 2021, coinciding with government restrictions on artificial watering, highlighting the increasing effect of natural drought conditions.
- **Water Supply Estimation**: Based on population needs and current reservoir levels, the region may face severe water shortages within the next **4 years** without intervention.

I closed of the project by writing an advise to the governor of Andalucia. 
<div style="display: flex; gap: 10px; justify-content: center; align-items: flex-start;">


  <figure>
  <img width="667" height="270" alt="image" src="https://github.com/user-attachments/assets/e4373107-6b70-4618-8ce8-cf5b4d011ce7" />
  <figcaption>Figure 2: Vegetation Quality Andalucia (darker green is better quality)  </figcaption>
  </figure>
  
  <figure>
  <img width="445" height="239" alt="image" src="https://github.com/user-attachments/assets/32732970-0284-4959-b89c-80c295f7de1c" />
  <figcaption>Figure 3: Water Declination Trend in one Andalucian Water Reserve  </figcaption>
  </figure>
  

</div>


