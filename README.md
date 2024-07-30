# Sri Lankan maps for understanding environment and urban growth
by @yudhanjaya

This repository contains the results of our work with satellite imagery. What we are specifically interested here is in understanding how Sri Lanka's environment has changed over the years 2017 to 2024, specifically with regard to loss of greenery and the growth of built-up areas and urban spaces. This operates broadly in the space known as land use and land cover classification. While the government of Sri Lankar does do and use and cover work, the closest publicly available map is from 2018 as of the time of writing and therefore it is quite difficult to observe a range of years up to today. 

In order to be practically useful and usable in a journalistic setting, LULC work must be taken out of a pure machine learning and data science context, and be rendered as images that anyone can open. We've uploaded said imagery here:

1) Gdrive link: https://drive.google.com/drive/folders/1ehnyt1YYxJW-wsyqOUrjFpzLRVw2tNzt?usp=drive_link
2) Mega link: https://mega.nz/folder/b3g3lTxb#sKY1D4u75PQ4fO41d_WQVg


These cloud storage drives contain: 


1) Satellite imagery from Sentinel 2 for each year between 2017 and 2024. The data in question uses multiple months, specifically three seasons, for all except for 2024, in order to provide a cloudless snapshot of the country.
![image](https://github.com/user-attachments/assets/2f48ed8c-daad-4cc3-b6e7-7080c07af755)

2) High contrast re-renders of the same satellite imagery. This makes terrain features features far more visible than the ordinary render.
![image](https://github.com/user-attachments/assets/467ac359-2164-4b08-88a7-83dea456e254)

3) A single colour version of the same image, where each pixel of the high-contrast re-render has been processed and its luminosity assigned a value between white and deep blue. We refer to this as bluescale. What it essentially shows is greenery and lack thereof with the darker colours being more untouched greenery and the lighter colours being a loss of greenery; it has the effect of making roads, human settlements, cleared land and other features stand out in brilliant white.
![image](https://github.com/user-attachments/assets/c1398d63-9b3e-48f2-8a39-c03d00d56550)

4) For easier reference, land use, land classification map for each year derived using a pix2pix network (a conditional generative adversarial network) trained on the Urban Development Authority's 2018 land use map. While not particularly precise, it is much smaller in file size, easier to refer to due to its gridded nature and forms a useful artifact in examining differences across the years. 
![image](https://github.com/user-attachments/assets/336d132d-de3d-4766-be7b-b73774d60afb)


Due to the very large nature of file sizes involved in this, the imagery available on the cloud storage drives are JPGs at 13214x21888 pixels. The PNG editions of the same files (double the resolution) total 27 gigabytes; the TIFF versions, which are the original format of the satellite imagery, are even larger. 

This repository also contains the notebook that was used for 4). Other scripts used for this exercise have been standardized and uploaded into the team-watchdog/geodog repo, since they are useful units of functionality for future work. 

# Data acquisition

Data used for this project comes from the Sentinel-2 satellites from the European Space Agency. These satellites operate at a 10 square meter resolution for their RGB bands; that is, a single pixel is 10 meters in reality. We used the RGB bands; they are the most intuitive, and we needed to keep processing and data our heads low (early experiments with NDVI, SWIR and near-infrared bands led us to gargantuan data sizes for each single year and comparatively increased processing times). 

While originally we obtained imagery directly from Sentinel Hub using their Python libraries, we found it more convenient to use Sepal.io, which in turn uses Google Earth Engine and Google Cloud to set up repeatable workflows that standardize the question of imagery and help us work around disruptions such as electricity or internet outages. 

To minimize cloud cover and artefacts, we use data from three seasons of each year up to the 31st of December. The exception is 2024, where, as of the time of writing, it is not yet December, and therefore, we used two seasons of data up to the 29th of July. Imagery used for this pull had to satisfy the criteria of having less than 20% cloud cover; then these are put together into an optical mosaic that gives us a composite image for the year. The general settings for the data pull are here (also available as LK_Optical_S2.json in this repo) and can be replicated on Sepal*. 

```
{
   "id":"5ac6742e-819a-4e7e-96af-26c6af252c0a",
   "projectId":"7a41c873-8df1-4f39-9b12-2b4552ca511f",
   "type":"MOSAIC",
   "placeholder":"Optical_mosaic_2024-06-24_15-14-29",
   "model":{
      "dates":{
         "targetDate":"2024-07-29",
         "seasonStart":"2024-01-01",
         "seasonEnd":"2025-01-01",
         "yearsBefore":1,
         "yearsAfter":0
      },
      "sources":{
         "dataSets":{
            "SENTINEL_2":[
               "SENTINEL_2"
            ]
         },
         "cloudPercentageThreshold":20
      },
      "sceneSelectionOptions":{
         "type":"ALL",
         "targetDateWeight":0
      },
      "compositeOptions":{
         "corrections":[
            "SR",
            "BRDF",
            "CALIBRATE"
         ],
         "filters":[
            
         ],
         "cloudDetection":[
            "QA",
            "CLOUD_SCORE"
         ],
         "cloudMasking":"MODERATE",
         "cloudBuffer":0,
         "snowMasking":"ON",
         "compose":"MEDOID"
      },
      "scenes":null,
      "aoi":{
         "type":"EE_TABLE",
         "id":"users/wiell/SepalResources/gaul",
         "keyColumn":"id",
         "key":231,
         "level":"COUNTRY",
         "buffer":1
      }
   },
   "layers":{
      "areas":{
         "left":{
            "id":"e38d8c84-6d63-4b31-8810-f6c8fa4da1f3",
            "imageLayer":{
               "sourceId":"google-satellite"
            },
            "featureLayers":[
               {
                  "sourceId":"aoi",
                  "disabled":true
               },
               {
                  "sourceId":"labels",
                  "disabled":true
               }
            ]
         },
         "right":{
            "id":"default-layer",
            "imageLayer":{
               "sourceId":"this-recipe",
               "layerConfig":{
                  "panSharpen":false,
                  "visParams":{
                     "type":"rgb",
                     "bands":[
                        "red",
                        "green",
                        "blue"
                     ],
                     "min":[
                        300,
                        100,
                        0
                     ],
                     "max":[
                        2500,
                        2500,
                        2300
                     ],
                     "gamma":[
                        1.3,
                        1.3,
                        1.3
                     ],
                     "inverted":[
                        false,
                        false,
                        false
                     ]
                  }
               }
            },
            "featureLayers":[
               {
                  "sourceId":"aoi"
               },
               {
                  "sourceId":"labels",
                  "disabled":true
               },
               {
                  "sourceId":"values",
                  "disabled":true
               }
            ]
         }
      },
      "mode":"grid"
   },
   "title":"LK_Optical_s2"
}
```
* note that Sepal requires some setting up and interlinking of Google Earth Engine and Google Cloud.

Imagery obtained this way is tiled into multiple tiffs that are connected together by a single .VRT file. We used QGIS to stitch everything together into a single TIF file for each year. 


# Postprocessing: highcontrast and bluespace

First, we converted the image to a PNG. At these sizes, conversion using conventional graphical editors gets challenging, so we wrote a small utility that processes an image in tiles: tiff_to_png.py (see geodog repository).

To generate the high contrast imagery, we converted the image into a three-channel (RGB) image, arranged it into two layers, and applied the multiply operation to combine the layers - ie: multiplied the pixel values of the upper layer with those of the layer below it and then divided the result by 255. 

To generate the bluescale imagery, we wrote a utility that uses the same tile-based approach as above to calculate the luminance values of each pixel within a tile and map them onto an axis ranging from dark blue ( 0, 0, 50 ) to white. This is blueconverter.py (see geodog repository). This utility also allows us to rescale and adjust contrast as required. We used a contrast setting of 125% and a scale setting of 50% for the final results. 

# Building the Pix2Pix model

## Training data

Finding training data for this kind of work is difficult. Usually, in land cover and land classification work, there exists shapefiles that are the results of surveys done by governments; these form the basis for training and machine learning model. In our case, we had a single image to go on:

![image](https://github.com/user-attachments/assets/aa696000-c911-4304-8d28-b8e0d4312c3d)



