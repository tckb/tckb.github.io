---
title:  Potential Issues with Chandrayaan2 TMC2 Dataset / Shapefiles
description : |
 The blog post focuses on the issues that I've found in the TMC2 dataset of Chandrayanan2 
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

This is a revised version of the document I originally submitted to [ISRO's Ahmadabad SAC](https://www.sac.gov.in/Vyom/index). The purpose is to understand and hopefully resolve the issues observed in the shapefiles provided by ISRO for public dissemination. 

I strongly urge ISRO to thoroughly review the observations I've outlined below. The datasets currently available on the PRADAAN portal appear to contain errors, rendering them unfit for any substantive analysis. I am prepared to supply additional information as required, or to rectify any errors on my end.

Despite my efforts in reaching out through various channels, I have not received any response from ISRO Ahmedabad SAC to address these concerns. Hence, I am extending this inquiry to the wider scientific community and interested parties who may be able to assist.

![redacted version of communication](/img/isro-redacted-email.png)

## Table of Contents
- [Table of Contents](#table-of-contents)
- [Primary Objectives of using the Dataset](#primary-objectives-of-using-the-dataset)
  - [Search for points of interests within the full catalogue using Shapefiles](#search-for-points-of-interests-within-the-full-catalogue-using-shapefiles)
  - [Building 3D-Topographic Models](#building-3d-topographic-models)
- [Potential Issues](#potential-issues)
  - [Issue#1: Erroneous Stray Data Observed Outside the Normal Image Data Strip](#issue1-erroneous-stray-data-observed-outside-the-normal-image-data-strip)
  - [Issue#2: Location Inaccuracy between DTM and Orthographic Images](#issue2-location-inaccuracy-between-dtm-and-orthographic-images)
  - [Issue#3: Mismatched Dimensions of DTM/Ortho and Shape Images](#issue3-mismatched-dimensions-of-dtmortho-and-shape-images)
  - [Possible Explanation & Disappointment](#possible-explanation--disappointment)
- [Demonstrative Dataset](#demonstrative-dataset)
  - [Derived Products / Level 2](#derived-products--level-2)
  - [Calibrated Products / Level 1](#calibrated-products--level-1)
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
- [Special Thanks](#special-thanks)

---

## Primary Objectives of using the Dataset

### Search for points of interests within the full catalogue using Shapefiles

One of the important objectives is to explore the desired set of points of interest within the TMC2 dataset.

**For example:**

- Theophilus Crater: Lat: 11.4°S, Long: 26.4°E

At the moment, the most straightforward way is to search based on shapefiles. Spatial databases such as [Spatialite](https://www.gaia-gis.it/fossil/libspatialite/index) and [PostGIS](https://postgis.net) facilitate this. Other mechanisms include "Search based on `Corrected Coordinates`" available via the ISSDC portal.

A potential usecase could be that a portal  can be developed where the POIs can be searched within the dataset, for eg: [Soma Unified Portal](https://x.com/this_is_tckb/status/1706384218152968215/photo/1)

---

### Building 3D-Topographic Models

Furthermore, both DEM/DTM and Orthographic dataset help in building a 3d topographic model of the POIs enabling better understanding of the sites. _For this to work correctly, the DEM and Orthographic data need to have a good agreement matching both shape and dimensions within a minor margin of errors._

<img width="600" alt="image" src="https://github.com/tckb/ch2-tmc2-dataset-issues/assets/939542/c14fdcf9-780d-460b-ab88-50d82890fedc">

![3d-topography.png](/img/3d-topography.png)

source: [topograhy view of Chang'e 6 Landing site](https://twitter.com/this_is_tckb/status/1708991962748015064)


---
## Potential Issues

### Issue#1: Erroneous Stray Data Observed Outside the Normal Image Data Strip

The default assumption is the data is going to come in a strip and the shapefiles indicate the boundaries of these strips. But in multiple cases it is observed that the images have erroneous "extra" data outside and far away from the strip which is causing shapefiles to extend far beyond the actual coverage. This makes searching and using the shapefiles impossible and renders shapefiles useless. You can observe this in the image the green shape polygon seems to be “stretched” outside the image. Note the extra junk data at the edge, I have indicated this error data in red arrows. 

![isro-extra-junk.gif](/img/dtm_stray_data.gif)
![isro-extra-junk.png](/img/isro-extra-junk.png)


Is the stray data outside the normal image strip expected or a result of erroneous production during the derivation? I believe this stray data is the reason the shape is stretched out beyond the normal image strip and causing these issues.

### Issue#2: Location Inaccuracy between DTM and Orthographic Images

![isro-extra-junk.gif](/img/dtm_oth_mismatch.gif)
![isro-displated-layers](/img/isro-displaced-layers.png)

It is my understanding that the DEM/DTM and Ortho needs= to perfectly align to achieve a 3d reconstruction of a site and minor inaccuracy is still acceptable. However, in the above picture, you see there is a clear and notable mis-alignment (QGIS> go 136670.736, 874537.345). This would not result in an accurate 3d reconstruction of the site.  This makes both of these files not useful at all. I have seen many in this condition. 

What is also observable is that the mis-alignment is both in longitude and latitude. Could this be also a related issue during the derivation?

### Issue#3: Mismatched Dimensions of DTM/Ortho and Shape Images

While the mismatched of shape file and derived products  can be explained due to Issue#1,  the DTM and Ortho image also do not match. In this product, the derived product differs by more than 1.5KM . This difference was observed in multiple products but it is inconsistent.

![isro-extra-junk.gif](/img/dtm_oth_misalign.gif)
![isro-mismatched-shapes](/img/isro-mismatched-shapes.png)

### Possible Explanation & Disappointment

These issues (#1,#2, #3) tells me that there could be a potential bug during the derivation of certain data data products as these inconsistencies are not observed in all the data products.

Due to the lack of communication from ISRO, I can arrive to this conclusion that the current state of datasets provided can not be used for _any meaningful analysis_ unless the bugs are fixed while generating derived products or shape files updated. My primary objectives seemed unresolved with the data provided as it is. 


## Demonstrative Dataset

To illustrate the data discrepanceis in the TMC2 dataset, I'm using the following dataset as a demonstrator - you can use this to replicate the issuse.

**Shapefile:** `PRODUCT_ID_ch2_tmc_ndn_20220109T1022082458_d_dtm_d32`

### Derived Products / Level 2

- **DTM:** `ch2_tmc_ndn_20220109T1022082458_d_dtm_d32`
- **Ortho:** `ch2_tmc_ndn_20220109T1022082458_d_oth_d32`

### Calibrated Products / Level 1

- **Fore:** `ch2_tmc_ncf_20220109T1022082458_d_img_d32`
- **Nadir:** `ch2_tmc_ncn_20220109T1022082458_d_img_d32`

> **Note:** The "aft" sensor data from the ISSDC portal was not available at the time of drafting this document.

It is also observed that the CRS of shape files and that of derived products are in Polar Stereographic [CRS References]. For illustrations, QGIS is used with these CRS.

> **Note:** use PRADDAN webportal to download the dataset.  

---

## Appendices

### Some Background about the data

If you are completely new to #Chandrayaan mission and TMC2 dataset you can read it more  here: [Topographic Mapping Using TMC-2 of Chandrayaan-2](https://www.isro.gov.in/Topographic%20Mapping.html)

### So whats a DTM/DEM again?

A Digital Elevation Model (DEM) is a 3D representation of elevation data for terrain or objects, applicable not just to Earth but also to moons, asteroids and other planetary bodies. It is a representation of elevation data of the underlying  terrain or overlaying objects. This is commonly used in Geographic information systems (GIS) to study the geography and, topographic analysis of the terrain.

![dem_india.png](/img/dem_india.png)


source: [??](???)

![dem_moon.png](/img/dem_moon.png)

source: [NASA LROC](https://onlinemaps.surveyofindia.gov.in/Product_Specification.aspx)

For all intensions and purposes, they usually look like a black and white images where the brighness values depict the relative height values. More information and explanation of DEM: [What is a digital elevation model (DEM)?](https://www.usgs.gov/faqs/what-digital-elevation-model-dem#:~:text=A%20Digital%20Elevation%20Model%20(DEM)%20is%20a%20representation%20of%20the,derived%20primarily%20from%20topographic%20maps)

### So whats a Shapefile then?

A shapefile is a data format used in Geographic Information Systems (GIS) for storing geospatial information. It contains both geometry and attribute data, often used for mapping and analyzing geographic features like points, lines, and polygons. Shapefiles commonly consist of a set of files with extensions like `.shp` for geometry, `.shx` for index data, and `.dbf` for attribute data. These files work together to accurately represent and store spatial data for various applications, ranging from urban planning to environmental studies.

A shapefile stores nontopological geometry and attribute information for the spatial features in a data set. The geometry for a feature is stored as a shape comprising a set of vector coordinates. Because shapefiles do not have the processing overhead of a topological data structure, they have advantages over other data sources such as faster drawing speed and edit ability. Shapefiles handle single features that overlap or that are noncontiguous. They also typically require less disk space and are easier to read and write.
Shapefiles can support point, line, and area features. Read more [here](https://www.esri.com/content/dam/esrisites/sitecore-archive/Files/Pdfs/library/whitepapers/pdfs/shapefile.pdf)

![shape_india.png](/img/shape_india.png)

source: [Survey of India](https://onlinemaps.surveyofindia.gov.in/Product_Specification.aspx)

### Where do I download this and other mission dataset?

![pradaan](/img/pradaan.png)


source: [Handbook of Chandrayan 2 Payloads Data & Science](https://pradan.issdc.gov.in)

[Indian Space Science Data Centre](http://issdc.gov.in) (ISSDC) is the prime data centre of the ISRO for all the science, lunar and interplanetary missions of ISRO. ISRO Science Data Archive (ISDA) is the central repository for all scientific and engineering data acquired by different ISRO’s planetary missions. ISDA makes the planetary data sets accessible to the world-wide scientific community.

The datasets are hosted using  Policy Based Data Retrieval, Analytics, Dissemination and Notification System ([PRADAN](https://pradan.issdc.gov.in)) and is accompanied with the meta information of the data in PDS4 standard as per International Planetary Data Alliance (IPDA). PDS4 is a format used primarily by NASA to store and distribute solar, lunar and planetary imagery data. GDAL provides read-write access to PDS4 formatted imagery data. Some more information about this standard and [processing levels of the data: pdf](https://pds.nasa.gov/datastandards/documents/ProcessingLevelsMapping_220202b.pdf)

#### What softwares do I need to replicate the issues?

I primarily work on [QGIS](http://qgis.org/) and to replicate the issues here, this should be enough for replicating and is only needed for all PDS4 datasets. For 3d rendering of the topography,  I used [Qgis2threejs](https://plugins.qgis.org/plugins/Qgis2threejs)  plugin and [Blender](https://blender.org/). In the limited context here, this is optional and is not neccessary.

### CRS References

#### Shapefile CRS

`$ ogrinfo -al -so PRODUCT_ID_ch2_tmc_ndn_20220109T1022082458_d_dtm_d32.gpkg`

```yaml
INFO: Open of `PRODUCT_ID_ch2_tmc_ndn_20220109T1022082458_d_dtm_d32.gpkg'
using driver `GPKG' successful.
Layer name: PRODUCT_ID_ch2_tmc_ndn_20220109T1022082458_d_dtm_d32
Geometry: Multi Polygon
Feature Count: 1
Extent: (-40285.300000, -32505.300000) - (906585.000000, 155464.000000)
Layer SRS WKT:
PROJCRS["POLAR_STEREOGRAPHIC_MOON",
    BASEGEOGCRS["GCS_MOON",
        DATUM["D_MOON",
            ELLIPSOID["MOON",1737400,0,
                LENGTHUNIT["metre",1,
                    ID["EPSG",9001]]]],
        PRIMEM["Reference_Meridian",0,
            ANGLEUNIT["Degree",0.0174532925199433]]],
    CONVERSION["unnamed",
        METHOD["Polar Stereographic (variant B)",
            ID["EPSG",9829]],
        PARAMETER["Latitude of standard parallel",-90,
            ANGLEUNIT["Degree",0.0174532925199433],
            ID["EPSG",8832]],
        PARAMETER["Longitude of origin",0,
            ANGLEUNIT["Degree",0.0174532925199433],
            ID["EPSG",8833]],
        PARAMETER["False easting",0,
            LENGTHUNIT["metre",1],
            ID["EPSG",8806]],
        PARAMETER["False northing",0,
            LENGTHUNIT["metre",1],
            ID["EPSG",8807]]],
    CS[Cartesian,2],
        AXIS["easting",north,
            ORDER[1],
            LENGTHUNIT["metre",1,
                ID["EPSG",9001]]],
        AXIS["northing",north,
            ORDER[2],
            LENGTHUNIT["metre",1,
                ID["EPSG",9001]]]]
Data axis to CRS axis mapping: 1,2
```

#### CRS Orthofile

`$  gdalinfo  ch2_tmc_ndn_20220109T1022082458_d_dtm_d32/data/derived/20220109/ch2_tmc_ndn_20220109T1022082458_d_dtm_d32.tif`

```yaml
Driver: GTiff/GeoTIFF
Files: ch2_tmc_ndn_20220109T1022082458_d_dtm_d32/data/derived/20220109/ch2_tmc_ndn_20220109T1022082458_d_dtm_d32.tif
Size is 94650, 18754
Coordinate System is:

PROJCRS["Polar StereoGraphic",
    BASEGEOGCRS["Polar StereoGraphic",
        DATUM["Moon_Spheroid",
            ELLIPSOID["Moon Spheroid",1737400,0,
                LENGTHUNIT["metre",1,
                    ID["EPSG",9001]]]],
        PRIMEM["Reference_Meridian",0,
            ANGLEUNIT["degree",0.0174532925199433,
                ID["EPSG",9122]]]],
    CONVERSION["Polar Stereographic (variant A)",
        METHOD["Polar Stereographic (variant A)",
            ID["EPSG",9810]],
        PARAMETER["Latitude of natural origin",-90,
            ANGLEUNIT["degree",0.0174532925199433],
            ID["EPSG",8801]],
        PARAMETER["Longitude of natural origin",0,
            ANGLEUNIT["degree",0.0174532925199433],
            ID["EPSG",8802]],
        PARAMETER["Scale factor at natural origin",1,
            SCALEUNIT["unity",1],
            ID["EPSG",8805]],
        PARAMETER["False easting",0,
            LENGTHUNIT["metre",1],
            ID["EPSG",8806]],
        PARAMETER["False northing",0,
            LENGTHUNIT["metre",1],
            ID["EPSG",8807]]],
    CS[Cartesian,2],
        AXIS["(E)",north,
            MERIDIAN[90,
                ANGLEUNIT["degree",0.0174532925199433,
                    ID["EPSG",9122]]],
            ORDER[1],
            LENGTHUNIT["metre",1]],
        AXIS["(N)",north,
            MERIDIAN[0,
                ANGLEUNIT["degree",0.0174532925199433,
                    ID["EPSG",9122]]],
            ORDER[2],
            LENGTHUNIT["metre",1]]]
Data axis to CRS axis mapping: 1,2
```

#### CRS DTM File

`$gdalinfo  ch2_tmc_ndn_20220109T1022082458_d_dtm_d32/data/derived/20220109/ch2_tmc_ndn_20220109T1022082458_d_dtm_d32.tif`

```yaml
Driver: GTiff/GeoTIFF
Files: ch2_tmc_ndn_20220109T1022082458_d_dtm_d32/data/derived/20220109/ch2_tmc_ndn_20220109T1022082458_d_dtm_d32.tif
Size is 94650, 18754
Coordinate System is:
PROJCRS["Polar StereoGraphic",
    BASEGEOGCRS["Polar StereoGraphic",
        DATUM["Moon_Spheroid",
            ELLIPSOID["Moon Spheroid",1737400,0,
                LENGTHUNIT["metre",1,
                    ID["EPSG",9001]]]],
        PRIMEM["Reference_Meridian",0,
            ANGLEUNIT["degree",0.0174532925199433,
                ID["EPSG",9122]]]],
    CONVERSION["Polar Stereographic (variant A)",
        METHOD["Polar Stereographic (variant A)",
            ID["EPSG",9810]],
        PARAMETER["Latitude of natural origin",-90,
            ANGLEUNIT["degree",0.0174532925199433],
            ID["EPSG",8801]],
        PARAMETER["Longitude of natural origin",0,
            ANGLEUNIT["degree",0.0174532925199433],
            ID["EPSG",8802]],
        PARAMETER["Scale factor at natural origin",1,
            SCALEUNIT["unity",1],
            ID["EPSG",8805]],
        PARAMETER["False easting",0,
            LENGTHUNIT["metre",1],
            ID["EPSG",8806]],
        PARAMETER["False northing",0,
            LENGTHUNIT["metre",1],
            ID["EPSG",8807]]],
    CS[Cartesian,2],
        AXIS["(E)",north,
            MERIDIAN[90,
                ANGLEUNIT["degree",0.0174532925199433,
                    ID["EPSG",9122]]],
            ORDER[1],
            LENGTHUNIT["metre",1]],
        AXIS["(N)",north,
            MERIDIAN[0,
                ANGLEUNIT["degree",0.0174532925199433,
                    ID["EPSG",9122]]],
            ORDER[2],
            LENGTHUNIT["metre",1]]]
```
---

## Special Thanks
