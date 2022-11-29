# IP Project
<br/>

## Problem Statement

**Dataset:** 

[Prostate cANcer graDe Assessment (PANDA) Challenge](https://www.kaggle.com/competitions/prostate-cancer-grade-assessment/overview)

## Background

### Gleason Score & ISUP Grade

The grading process consists of finding and classifying cancer tissue into so-called Gleason patterns (3, 4, or 5) based on the architectural growth patterns of the tumour. After the biopsy is assigned a Gleason score, it is converted into an ISUP grade on a 1-5 scale.

> The Gleason grading system is the most important prognostic marker for PCa, and the ISUP grade has a crucial role when deciding how a patient should be treated.
> 

There is both a risk of missing cancers and a large risk of over-grading resulting in unnecessary treatment. However, **the system suffers from significant inter-observer variability** between pathologists, limiting its usefulness for individual patients. This **variability in ratings could lead to unnecessary treatment, or worse, missing a severe diagnosis**.

![Example of the Grading process for a tissue sample](IP%20Project%20d98e5c6aabef45ed89d949597a5641c0/Untitled.png)

Example of the Grading process for a tissue sample

## Proposed System

We can improve the accuracy of the process by automating and standardising the recognition of the Gleason Pattern.

1. RGB to GrayScale
2. Thresholding to convert to Binary Image
3. Crop Image - using Polynomial Boundary Representation Principle (to reduce
computation and time)
4. Closing to enhance Gleason pattern
5. Using Local processing to identify Euler's Number of patches
6. Create regions using Split And Merge & Euler’s Number
7. Identify Majority and Minority Regions
8. Output Segmented Tissue Sample and Maj & Min Euler’s Score

## Code

### **Link to Notebook: Prostate Cancer Grade Detection**

[Google Colaboratory](https://colab.research.google.com/drive/1GawEscP_l9_8OmZKiKjrpu6GvrMWF389#scrollTo=MZddV8usc6gm)

The code generates a segmented tissue sample with an Euler’s score that can be mapped to a Gleason score and ISUP Grade. Illustrated below is a sample output of the above image processing pipeline.

## Segmented Tissue Sample

![output.png](IP%20Project%20d98e5c6aabef45ed89d949597a5641c0/output.png)

## Further Analysis of Segmented Tissue Sample

Looking at the Scaled Euler's Score & Area of each Cluster/Segment:

```jsx
Cluster 1: -5.711280214861236
Cluster 2: -27.291666666666668
Cluster 3: 12.112813370473537
Cluster 4: -13.592105263157896
```

```jsx
Area of Region 1: 13962500
Area of Region 2: 1500000
Area of Region 3: 4487500
Area of Region 4: 4750000
```

The Pathologist can Identify the 2 distinct most prominent Gleason Patterns and map them to the corresponding **Gleason Score & ISUP Grade.**

Here we Identified **Region 1 - as the Majority Region** & **Region 2 - as the Minority Region**

We can also see that the Euler’s Score estimated for each region is correct as it follows the progression - **The lower the Euler’s Number the higher the Gleason Score** of the correct Gleason Score i.e. **-5.711 is mapped to Score 3** & -**27.292 is mapped to Score 4**

Therefore the **ISUP Grade** can be estimated accurately - **3+4 gives Grade 2**

## Conclusion

1. Using the proposed system we were able to **Enhance the Tissue Sample** and **Identify the Gleason Pattern**. This included performing operations like - RGB to Grayscale conversion, Boundary Detection, Thresholding, Closing, and so on. 
2. We **used Local Processing to implement Euler’s Number** in order to establish a consistent measure to map to the Gleason Scoring System.
3. We **used the  K-Means algorithm for Segmentation**, to Identify regions in the tissue sample. We used a custom parameter - the Euler’s Number, instead of the pixel values for Segmentation. We evaluated different values of K to get the most accurate - neither too generalized nor too many regions. We **identified K=5 as the best value**, (4+1 to account for the background).
4. On Further Analysis of the 4 region, the **Majority and Minority Gleason Patterns were
Identified**.
5. Then using a Scoring Index these **Patterns were mapped to correct Gleason Score**.
6. The **ISUP Grade for the Tissue Sample was Identified**.

## Challenges

1. Reading in a high resolution (25kX25k pixels) tiff file.
2. Reducing Time and Computationally Complexity without Down Sampling
3. Converting to Binary Image using the correct Thresholding Approach
4. Enhancing Gleason Pattern (Evaluating the performance of different Morphological
Operations)
5. Calculating Euler's Score for each Region
6. Implementing Segmentation using custom value - Euler's Number not the pixel
values
7. Identifying the correct K value for Segmentation

## Future Scope

1. Create a dataset using these segmented tissue samples with Gleason Score for each segment
2. Build an ML model on that dataset for Prostate Cancer Grade detection
