# satellite2024

This repository contains the results of our work with satellite imagery. What we are specifically interested here is in understanding how Sri Lanka's environment has changed over the years 2017 to 2024, specifically with regard to loss of greenery and the growth of built-up areas and urban spaces. This operates broadly in the space known as land use and land cover classification. 

In order to be practically useful and usable in a journalistic setting, LULC work must be taken out of a pure machine learning and statistics and data science context, and be rendered as images that anyone can open. Therefore, these cloud storage drives contain: 


1) Satellite imagery from Sentinel 2 for each year between 2017 and 2024. The data in question uses multiple months, specifically three seasons, for all except for 2024, in order to provide a cloudless snapshot of the country.
2) High contrast re-renders of the same satellite imagery, where we've duplicated the image, arranged it into two layers, and applied the multiply operation to combine the layers - ie: multiplied the pixel values of the upper layer with those of the layer below it and then divided the result by 255. This makes terrain features features far more visible than the ordinary render.
3) A single colour version of the same image, where each pixel has been processed and its luminosity assigned a value between white and deep blue. We refer to this as bluescale. What it essentially shows is greenery and lack thereof with the darker colours being more untouched greenery and the lighter colours being a loss of greenery; it has the effect of making roads, human settlements, cleared land and other features stand out in brilliant white.
4) For easier reference, land use, land classification map for each year derived using a pix2pix network (a conditional generative adversarial network) trained on the Urban Development Authority's 2018 land use map. While not particularly precise, it is much smaller in file size, easier to refer to due to its gridded nature and forms a useful artifact in examining differences across the years.

Gdrive link: https://drive.google.com/drive/folders/1ehnyt1YYxJW-wsyqOUrjFpzLRVw2tNzt?usp=drive_link

Mega link: https://mega.nz/folder/b3g3lTxb#sKY1D4u75PQ4fO41d_WQVg

Due to the very large nature of file sizes involved in this, the imagery available on the cloud storage drives are high resolution JPGs. The PNG editions of the same files total 27 gigabytes and the TIFF versions which are the original format of the satellite imagery are even larger. 
