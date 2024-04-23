Introduction: Hydroligcal soil group (HSG) information based on STATSGO2 for entire CONUS and based on SSURGO for Florida.

CONUS (STATSGO2) ~ 100 x 100 m resolution: p:\11206085-onr-fhics\02_models\SFINCS_flooding\02_modelsetup\soil\wss_gsmsoil_US_1\HSG_CONUS.tif
Florida (SSURGO) ~ 10 x 10 m resolution: 

Hydrological soil groups (referred to HSG or HG) are converted from string values (A,B,C,D,A/D,B/D,C/D, <null>) to numbers 1-8 to be able to merge different areas following this conversion:

def reclass(fld):
   if fld == "A":
      value = 1
   elif fld == "B":
      value  = 6
   elif fld == "C":
      value  = 5
   elif fld == "D":
      value  = 3
   elif fld == "A/D":
      value  = 7
   elif fld == "B/D":
      value  = 2
   elif fld == "C/D":
      value  = 8
   else:
      value  = 4

   return value

Technical background: How to obtain such data

(1) Raw data for SSURGO (per state) or STATSGO2 (per state or entire CONUS) downloaded from: https://nrcs.app.box.com/v/soils/folder/148414960239?page=2
(2) Also possible to download per area (< 100 ha), or other ways explained here https://www.nrcs.usda.gov/wps/portal/nrcs/detail/soils/survey/geo/?cid=nrcs142p2_053629

(3) To process STATSGO2 data, ArcMap is needed (can be requested at Deltares ICT)
(4) Furthermore, the Soil Data Viewer should be downloaded https://www.nrcs.usda.gov/wps/portal/nrcs/detail/soils/survey/geo/?cid=nrcseprd337066
(5) Make sure that the Soil Data Viewer is added to ArcMap (eventually go to C:\Program Files (x86)\USDA\Soil Data Viewer 6.2, double click the file named “SDVArcMapAddin.esriAddin”)

(6) Within the STATSGO2 Data-folder, there should be a microsoft access soil database (e.g. soildb_US_2003.mdb). To be able to use this database with the Soil Data Viewer, you must open it first (only once) with Microsoft Access and import the Tabular data.
(7) Furthermore, spatial data (shapefile of the area) should be loaded into Arcmap 
(8) Then you should be able to run the Soil Data Viewer and load a Map Layer Source and a Database
	- For me, the entire CONUS area was too large to process with the Soil Data Viewer at once, so I had to create multiple smaller shapefiles (select desired area ad then export data)
	- I ended up by dividing the CONUS area into 5 smaller parts
	- If you did not open the database-file with Microsoft Access before, some error will pop-up about incomplete lines in the database when loading database in Soil Data Viewer

(9) Once Map Layer Source and Database are loaded into Soil Data Viewer, you can select one of the attribute folders (e.g. Hydrologic Soil Group). If you then push the Map button, the conversion should start
	- If the area is too large, an error will occur like 'Query is too complex'
(10) Repeat these steps for the different areas defined

(11) Now you should have several new layers in ArcMap (e.g. 5 times Hydrologic Soil Group)
(12) We would like to merge these individual shapefiles into 1 shapefile, however merging based on strings (Hydrlogic Soild Groups A,B,C,D brings diffiutlties), so we convert those into numbers:
	- Open Attribute Table
	- Add Field
	- Open Field Calculator
	- Select Python, select Show Codeblock, and paste the definition (python function) from above into the Pre-Logic Script code
	- Then this function should be called in the lower field by: reclass( !SdvOutput1.txt.HydrolGrp! )  , make sure that the expression between !! is the variable to be used in the function

(13) Open the Merge toolbox and load the different shapefles to be merged
	- Define an output dataset: e.g. merge.shp
	- In the Field Map (optional) field, you should define which variable you would like to have in the merged shapefile
	- You could create a new field, and then add input fields (e.g. 5 different fields of the hydrological soil groups), and merge rules (e.g. minimum)

(14) After merging, the shapefile with polygons can be converted into a raster using polygon_to_raster (define which variable you want to keep and the desired resolution)
	- Make sure that the toolbox-license that is necessary is selected under Customize - Extensions within ArcMap

(15) Export data from raster into tif or other desired output.


For the SSURGO data, is similar procure can be executed, which is explained in the User Guide which should be included in your download, but an example can be found here:
p:\11206085-onr-fhics\02_models\SFINCS_flooding\02_modelsetup\soil\gSSURGO_FL\gSSURGO_UserGuide_July2020.pdf

Contact:
If something does not work the way it should work, don't hesitate to contact me: roel.degoede@deltares.nl


 

  