# Regionalized BioModelos: Habitat availability multitemporal models of birds in Colombia 

BioModelos is a collaborative online system to map species distributions (Velásquez-Tibata et al, 2019). This repository stores functions to construct habitat availability models of Species (HAM) that are trained and projected multitemporally, from databases gathered, managed and curated by The Alexander Von Humboldt Institute and a network of experts in order to be evaluated and refined. This models use GEE computational capacity, and petabytes-size catalog of satellite information, reducing the modelling time, thus speeding up processing allowing simultaneous modelling for many species, projected in multitude of temporal scenarios.

The functions follows the next sequence (*Puede revisar el flujo de trabajo de este porjecto en la carpeta de imagenes, en el archivo llamado workflow.png*): 
* First, it needs to standardize the date format
* Second, it creates the annual MODIS mosaics, from where the models are going to be projected.
* Third, it creates a training dataset
* Fourth, it execute 4 different classification algorithms, to produce 4 different binary (presence/absence) maps
* Fifth, it execute 3 different classification algorithms, to produce 3 different probability maps 
* Sixth, it shows a general visualization of the binary maps
* Seventh, it creates a valuation data set, to perform different accuracy assessments on each binary map
* Eight, it performs the accuracy assessment resulting in a table
* Ninth, it allows the exportation of either the binary or probabilistic maps into the personal Google Drive
* Tenth, it calculates the total area for each species, in each year by each algorithm
* Eleventh, its a Google App that allows to visualize the products.

Current state: first version.

## Prerequisites

### Google Earth Engine

these codes were developed in Google Earth Engine (GEE), therefore for its use. the user must have an authorized GEE user.

GEE is free only for educational and public research. 

This link allows you to learn about GEE: https://earthengine.google.com/
in the top-rigth corner you can find the "Sing Up" button, where you must register (using a gmail account) and explain how you intend to use GEE. After a revision of you application by the Google team, you will receive an email either authorizing or denying your GEE account (it can be as well an email asking to further explain your intended use of GEE)

Once you have an authorized GEE user, you can access the GEE API using this link https://code.earthengine.google.com/


you can access some guides on how to use GEE, with this link: https://developers.google.com/earth-engine/guides/getstarted


### Assets

The scripts are written in a way that require a specific file structure. Also you must be careful with the names of the files in the scripts (the scripts have the files names used during the developing stage). You can use the same file names and structure in order to modify the minimum amount of the scripts, or you can change the names and structure, but must remember to modify the file paths in the scripts 

**This is the example of the files structure and names used in the sripts**

![](images/assets.png)

In the previous example we created a folder in the assets with the name '*biomodelos_anuales*'
Then we created seven (7) diferent folders in the assets with the names:

- '*binario*'
- '*dates*'
- '*inputs*' (this is the only folder where you are going to put the data, as explained below, the other ones are going to be filled as the scripts are executed)
- '*modis*'
- '*proba*'
- '*training*'
- '*validacion*'

After that, every folder must be moved inside the '*biomodelos_anuales*' one. And that how you can create the files structure

### Data

Before you start using the scripts, you must have previously prepared and uploaded you data to your GEE Assets.

The flowing data is the one we used to develop the scripts and it is not public data. However you can find public data and/or use your own. Not all the files here exposed are necessary but if you are not going to use a specific file, must remember to modify you scripts:

**Inputs**

![](images/inputs.png)

1. alturas_ayerbe: is a table containing 3 different columns: **max** (contains the maximum altitude the species can survive in meters), **min** (contains the minimum altitude the species can survive in meters) and **species** (contains the scientific name the species). *This file is not necessarily required, it is used to filter the altidinaly the observation data*

2. areas_ave: is a FeatureCollection containing the shapes of the estimated distribution area of each species, each shape must have the attribute **lista_aves** with the scientific name of each species. *This file is required, it is used to create the absence data, and limit the projections* 
This feature collection is a product derived, in part, from the maps of BirdLife International (BirdLife International and Handbook of the Birds of the World (2020) Bird species distribution maps of the world. Version 2020.1. Available at http://datazone.birdlife.org/species/requestdis. BirdLife International), and local distribution maps, this data its not available to the public.

3. usuario: is the FeatureCollection containing the observation data (points), each point must have at least these 2 attributes **acceptedNameUsage** (contains the scientific name the species) and **fecha** (contains the month when the observation was made its format must be YYYY-MM). *This file is required, it is used to create the multitemporal models, and evaluate the accuracy*

4. neotropico: is a shape of a single vector containing a general terrestrial area that encapsulates all the observation data of all the species. *This file is required, it is used to create the annual mosaics where the models are going to be projected*


## How to run

- You must run sequentially the scripts in numerical order. To run a script you must click the run button (in the GEE API) as shown bellow

![](images/run.png)

- Each script has its instructions and explanations (in Spanish and English) within them as comment lines. 
- You must wait until each task of each script is completely finished before running the next one. To execute a task you must find the task tab (in the GEE API) click it, then click run as shown bellow

![](images/task.png)

- There are scripts that are not fundamental to the overall work, so if you prefer you can skip them, those are:
  - 1.Dates_records: this script can be skipped if you data dates are already in format (YYYY-MM) **as text**. (However this script is quite basic and specific, so it may not be enough, sufficient, or functional to every date format. The important thing to remember is that you need your dates as text in the format YYYY-MM).
  - 6.Visualization: this script is merely a visualization tool, it can be completely skipped. it is used to do a preliminary visual check of the models.
  - 9.Export: this script just exports the models to Google Drive, if you don't want and/or need the rasters in your Drive, don't use it. However if you don't export then to your drive you wont be able to use them outside of GEE 

1. (**OPTIONAL IF YOUR DATES ARE IN THE REQUIRED FORMAT**) copy "*1. Dates_records*" script, and paste it in your code editor in GEE.
You must check that all the file paths are correct, and that the attributes required in each function is present in your data.

2. run the code for each species, and execute the task in the 'task' tab, these files would have the corrected dates and would be saved in the 'dates' folder in your assets.

3. copy "*2.Mosaics*" script, and paste it in your code editor in GEE.

4. run the code for each date that you want to project your models, it can be a month or a year. this value must be changed in the variable year in the line 225. Then execute the task in the 'task' tab, these files would have the MODIS mosaics for each date that yo want to project the models on, and would be saved in the 'modis' folder in your assets.

5. copy '*3.SampleTraining*' script, and paste it in your code editor in GEE.

6. run the code for each species, and execute the task in the 'task' tab, these files would have the training dataset for each species. These data is going to be multitemporally trained, and would be saved in the 'training' folder in your assets.

7. copy '*4.Models*' script, and paste it in your code editor in GEE.

8. run the code for each species, and execute the task in the 'task' tab, these files would have the binary classification, of the 4 algorithms, for the last 21 years, for each species. These rasters are going to be  saved in the 'binario' folder in your assets.

9. copy '*5.Probabilistic*' script, and paste it in your code editor in GEE.

10. run the code for each species, and execute the task in the 'task' tab, these files would have the probabilistic classification, of the 3 algorithms, for the last 21 years, for each species. These rasters are going to be  saved in the 'proba' folder in your assets.

11. this is an optional step, It´s function is simply to give a preliminary visualization of the models, to check how they are coming out. To do so, copy '6.Visualization' script, and paste it in your code editor in GEE. Then just tun it with the species you want to visualize 

12. copy '*7. SampleModels*' script, and paste it in your code editor in GEE.

13. run the code for each species, and execute the task in the 'task' tab, these files would have the training data binary classification, of the 4 algorithms (as a feature collection), for the last 21 years, for each species. These feature collections are going to be  saved in the 'validacion' folder in your assets. and would be used to assess the accuracy of the models.

14. copy '*8.Accuracy*' script, and paste it in your code editor in GEE.

15. run the code once an it should execute it for every species in the list. However if there is an error, you might divide the whole list (if it is too long), into sub-list and run the script in batches, and execute the task in the 'task' tab, these file would have the accuracy measurements for all species and save it in the assets.

16. copy '*9.Export*' script, and paste it in your code editor in GEE.

17. run the code once and it should execute it for every species in the list. Its important to notice, that you must be careful with the direction of the images (models), with the names of the bands you want to export, and the names yo want to use to rename them.
additionally the export function is tricky, nested, and deals with objects from the user and server side. in case present problems you can always export the models with the simple export function, however the purpose of this script is to optimize the exportation.

also you must click every task individually, but there are scripts online (external to GEE) that can help you to click all the task automatically (look for 'Batch execute GEE Export task' from Dongdong Kong)

18.copy '*10.area*' script, and paste it in your code editor in GEE.

19. run the code once and it should execute it for every species in the list and you'll have a feature collection with every area estimate.

## APP

finally there is an online APP developed for easy visualization, you can access it at https://biomodelos-iavh.users.earthengine.app/view/biomodelos

the code behind the app its not required to execute.

However if you want to explore it, there is a copy with comments (in Spanish because our developer team communicates mainly in Spanish), along the code there are comments indicating some images (from the folder 'images' in the folder images in this github), those images would help understand the arquitecture of the APP.

look for '*11.APP*' script

## Authors and contact

* **Gabriel Alejandro Perilla Suarez, [contacto institucional](mailto:gperilla@humboldt.org.co), [contacto personal](mailto:thealejandroperilla@gmail.com)** 

* **Elkin Alexi Noguera Urbano, [contacto institucional](mailto:enoguera@humboldt.org.co), [contacto personal](mailto:elkalexno@gmail.com)**

* **Luis Hernando Romero Jiménez, [contacto institucional](mailto:lromero@humboldt.org.co), [contacto personal](mailto:lhromeroj@gmail.com)**

* **María Helena Olaya, [contacto institucional](mailto:molaya@humboldt.org.co ), [contacto personal](mailto:olaya42@gmail.com)**

* **Carlos Jair Muñoz Rodriguez, [contacto institucional](mailto:cmunoz@humboldt.org.co), [contacto personal](mailto:cmunozbiol@gmail.com)**

* **Cristian Alexander Cruz Rodriguez, [contacto institucional](mailto:ccruz@humboldt.org.co), [contacto personal](cruzrodriguezcristian@gmail.com)** 

* **Héctor Manuel Arango Martínez, [contacto institucional](mailto:harango@humboldt.org.co), [contacto personal](hma9327@gmail.com)**



## Acknowledgment

EoData science

BirdLife International and Handbook of the Birds of the World (2020) Bird species distribution maps of the world. Version 2020.1. Available at http://datazone.birdlife.org/species/requestdis.

## License

This project is licensed under the MIT License - see the [License.md](License.md) file for details
