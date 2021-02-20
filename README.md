# CNN-LSTMs for regional hydrological modelling

This repository contains the code used in the study:

Anderson, Sam and Valentina Radic.  "Evaluation and interpretation of convolutional-recurrent neural networks for regional hydrological modelling" (Submitted 2021).

The code in this repository can reproduce all figures and findings in the study.  All data used is publicly accessable.  This repository contains the following files:

* main.ipynb: Defines functions, loads preprocessed data, builds/trains CNN-LSTM model, evaluates performance, interprets model learning, creates figures
* preprocessing.ipynb: Loads raw data (temperature, precipitation, streamflow, and basin outlines) and preprocesses into format used in main.ipynb
* figure_study_region.ipynb: Creates Figure 1 (study region).
* era5_download_P_075grid.py: Connects to ERA5 API and downloads raw precipitation data
* era5_download_T2m_075grid.pi: Connects to ERA5 API and downloads raw temperature data

Practically, main.ipynb runs best on a GPU to train the models much faster.  It is set up to run in Google Colab, with files organized/saved in Google Drive as outlined in the notebook.  The other files can be run locally.
