# CNN-LSTMs for regional hydrological modelling

This repository contains the code used in the study:

Anderson, Sam and Valentina Radic.  "Evaluation and interpretation of convolutional-recurrent neural networks for regional hydrological modelling" (Submitted 2021).

The code in this repository can reproduce all figures and findings in the study.  All data used is publicly accessable and details to download data are given below.  This repository contains the following files:

* main_publish.ipynb: Defines functions, loads preprocessed data, builds/trains CNN-LSTM model, evaluates performance, interprets model learning, creates figures
* preprocessing.ipynb: Loads raw data (temperature, precipitation, streamflow, and basin outlines) and preprocesses into format used in main.ipynb
* figure_study_region.ipynb: Creates Figure 1 (study region).
* era5_download_P_075grid.py: Connects to ERA5 API and downloads raw precipitation data
* era5_download_T2m_075grid.pi: Connects to ERA5 API and downloads raw temperature data


___
# How to run code

Practically, main_publish.ipynb runs best on a GPU to train the models much faster.  It is set up to run in Google Colab.  Google Colab does not access locally saved files; rather, it can access those in Github and Google Drive.  So, main.ipynb can be run on Colab via Github, and all outputs/required data can be saved/organized in Google Drive as outlined in the notebook.  The other files can be run locally.  Here we give instructions to replicate the results in Google Colab.

1. Download ERA5 data:

    * Locally, run era5_download_P_075grid.py and era5_download_T2M_075grid.py; save output files (ERA5_T_1979_2015_6hourly_075_grid_AB_BC.nc and ERA5_P_1979_2015_6hourly_075_grid_AB_BC.nc) locally in cnn_lstm_era/Data/ERA5/

2. Download streamflow data:

    * See [here](https://wateroffice.ec.gc.ca/mainmenu/tools_and_downloads_index_e.html) for instructions to download available data for all active and naturalized stream gauge stations in Alberta and BC.   ABActNatFlowAll.csv and BCActNatFlowAll.csv list the stations which should be downloaded.  Save streamflow data in ./Data/Flow/

3. Download basin outline data:

    * From [here](https://open.canada.ca/data/en/dataset/0c121878-ac23-46f5-95df-eb9960753375), download the folder WSC_Basins.gdb.  A direct download link and other information can be found [here](https://wiki.usask.ca/pages/viewpage.action?pageId=1766228079#HowtoloadandreprojectWaterSurveyofCanada%22WSC_Basins%22GeodatabaseinQGIS-Dataavailability)
Save this folder as ./Data/WSC_Basins.gdb/

4. Download provincial border shapefiles:

	* From [Statistic Canada](https://open.canada.ca/data/en/dataset/a883eb14-0c0e-45c4-b8c4-b54c4a819edb), download the "Provinces/Territories Cartographic Boundary File - 2016 Census" shapefile (SHP).  Note: This data is not necessary for the analysis, but it used for making maps.

5. Preprocess the raw ERA5, streamflow, and basin outline data using preprocessing.ipynb

6. Upload preprocessed files from Step 5 to Google Drive in folder ./data/.  Upload shapefiles from Step 4 to Google Drive in folder ./data/province_borders/ (e.g. ./data/province_borders/lpr_000b16a_e.shp)

7. Run main_publish.ipynb in Colab.

___
# File organization

Local organization:  

* cnn_lstm_era/  
  * main_publish.ipynb  
  * preprocessing.ipynb  
  * figure_study_region.ipynb  
  * era5_download_P_075grid.py  
  * era5_download_T2M_075grid.py  
  * Data/  
  	* ERA5/  
		* ERA5_T_1979_2015_6hourly_075_grid_AB_BC.nc  
		* ERA5_P_1979_2015_6hourly_075_grid_AB_BC.nc  
	* Flow/
		* AB/  
			* ABActNatFlowAll.csv  
			* 05AA004_Daily_Flow_ts.csv  
			* ...  
			* 11AA026_Daily_Flow_ts.csv  
		* BC/  
		  	* BCActNatFlowAll.csv  
		  	* 07EA004_Daily_Flow_ts.csv  
		  	* ...
		  	* 10DA001_Daily_flow_ts.csv  
	* WSC_Basins.gdb/
		* ...

Google Drive organization (for Colab access)  

* My Drive/  
	* Colab Notebooks/  
	* cnn_lstm_era/   
		* models/  
		* output/  
		* heat_maps/ 
		* data/
			* province_borders/  
