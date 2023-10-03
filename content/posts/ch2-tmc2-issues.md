---
title:  Potential Issues with Chandrayaan2 TMC2 Dataset / Shapefiles
description : |
  In this blog post, I try to explain the issues I have and some possible explanation of the cause. I also try give to some background knowledge to understand this blogpost.
type:
- posts
- post
tags: 
- isro
- gis
- tmc2
keywords :  
 - 'chandrayaan'
 - 'isro'
---
_This blog post is a part of [#knowyourISRO](https://twitter.com/search?q=%23KnowYourISRO&src=hashtag_click) series_

**Disclaimer** 

This is a revised version of the document I originally submitted to [ISRO's Ahmadabad SAC](https://www.sac.gov.in/Vyom/index). The purpose is to understand and hopefully resolve the issues observed in the shapefiles provided by ISRO for public dissemination. Despite reaching out through various channels, I have not received any response from ISRO Ahmedabad SAC to address these concerns. Hence, I am extending this inquiry to the wider scientific community and interested parties who may be able to assist.

<img width="915" alt="image" src="/img/272233364-84d06373-1530-4e30-b3dc-efb6837eb195.png">

## Table of Contents
- [Disclaimer](#disclaimer)
- [Table of Contents](#table-of-contents)
- [Issues](#potential-issues)
    - [Issue#1: Superfluous Stray Data Observed Outside the Normal Image Data Strip](#issue1-superfluous-stray-data-observed-outside-the-normal-image-data-strip)
    - [Issue#2: Location Inaccuracy between DTM and Orthographic Images](#issue2-location-inaccuracy-between-dtm-and-orthographic-images)
    - [Issue#3: Mismatched Dimensions of DTM/Ortho and Shape Images](#issue3-mismatched-dimensions-of-dtmortho-and-shape-images)
- [Possible Explanation](#possible-explanation)
- [Primary Objectives of using the Dataset](#primary-objectives-of-using-the-dataset)
    - [Identify Point of Interests / POI within the Dataset](#identify-point-of-interests--poi-within-the-dataset)
    - [Make 3D-Topographic Models](#make-3d-topographic-models)
- [Demonstrative Dataset](#demonstrative-dataset)
    - [Derived Products / Level 2](#derived-products--level-2)
    - [Calibrated Products / Level 1](#calibrated-products--level-1)
- [Special Thanks](#special-thanks)
- [Appendices](#appendices)
    - [Some Background about the data](#some-background-about-the-data)
    - [So whats a DTM/DEM again?](#so-whats-a-dtmdem-again)
    - [So whats a Shapefile then?](#so-whats-a-shapefile-then)
    - [Where do I download this and other mission dataset?](#where-do-i-download-this-and-other-mission-dataset)
        - [What softwares do I need to replicate the issues?](#what-softwares-do-i-need-to-replicate-the-issues)
    - [CRS References](#crs-references)
      - [Shapefile CRS](#shapefile-crs)
      - [CRS Orthofile](#crs-orthofile)
      - [CRS DTM File](#crs-dtm-file)

