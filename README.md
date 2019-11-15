# Quantify FRET ratio from Timelapse experiments

Step-by-step instruction to quantify the FRET ratio from timelapse imaging experiments that measure the donor (D) and sensitized emission(SE)
Note: this procedure generates lineplots with FRET ratio changes over time - if you want images of the FRET ratio, the alignment of the channels is crucial. Therefore, to obtain FRET maps, image alignment should be performed and verified.

There are two option discussed here. Option 1 uses two channel data; data of the donor (w1: excite donor, detect donor) and data of the sensitized emission (w2: excite donor, detect acceptor).
Option 2 uses additional data, which is directly excited acceptor (w3: excite acceptor, detect acceptor). Next to this data, correction factors beta and gamma are needed (we verified that alpha and delta are nearly zero). For background and an explanation of the correction factors see: [Rheenen et al 2004](https://doi.org/10.1016/S0006-3495(04)74307-6)

### General preparations
#### Only measure gray values
* Set the measurements to only measure mean gray values: _Analyze > Set Measurements..._ select _Mean gray value_
#### Set export options for csv
* Set the options for input/ouput: _Edit > Options > Input/Output..._
* Set the "File extension for tables" to .csv
* In the "Results Table Options" make sure that "Save column headers" and "Save row numbers" are selected

# Option 1: uncorrected (Sensitized Emission/Donor) ratio

### Preparations

* Download the ImageJ macro file "Subtract_Measured_Background.ijm" (available in this repository)
* Install it as a plugin: _Plugins > Install Plugin..._

### Analysis

#### Open the image sequences
* If the images are individually saved (as in the example data) use: _File > Import > Image Sequence..._
* If the images are saved as a single file, use the right file import option
* Open both the image data from the donor and FRET channel (w1 and w2 in the example data).

#### Define and subtract background
* Use an ROI to define an area with background fluorescence in the image
* Use _Subtract Measured Background_ to remove the average value of the ROI from the image (this action will be performed on each image in the stack)

#### Calculate the ratio image
* Use _Process > Image Calculator..._
* Set 'Operation' to 'divide', Image1 is the sensitized emission data and Image2 is the donor intensity data. Make sure to check the boxes 'Create new window' and '32-bit (float) result'

#### Select and analyze cells
* Activate the ROI manager: _Analyze > Tools > ROI Manager..._
* Draw an ROI in the result window to select a cell (try to exclude background), add to ROI manager ("Add [t]")
* Repeat the selection of cells with ROIs until all cells are marked
* Save the set of ROIs (_More... > Save..._)
* Select all ROIs in the ROI manager
* Measure in all ROIs the mean gray value: _More > Multi Measure_ (select "Measure all slices"" and "One row per slice")
* Save the table with results (csv format).

#### Plot the result
* Open the csv file with results from the quantification in [PlotTwist](https://huygens.science.uva.nl/PlotTwist/) by using the "Upload" option
* Apply normalization in PlotTwist - the default is 'Fold change over baseline (I/I0)'
* Note: PlotTwist has the option to exclude data from user selected cells

### Sample data and expected results
* Raw sample data is available in this [folder](https://github.com/JoachimGoedhart/Quantify-FRET-ratio/tree/master/Example-data_raw)
* The graph of the example data generated with PlotTwist looks like this:


![alt text](https://github.com/JoachimGoedhart/Quantify-FRET-ratio/blob/master/Example-data_processed/PlotTwist-results.png "Output")

# Option 2: corrected (Sensitized Emission/Donor) ratio

### Preparations
* Determine the correction factors correction factors beta and gamma (and verify that alpha and delta are nearly zero). For background and an explanation of the correction factors see: [Rheenen et al 2004](https://doi.org/10.1016/S0006-3495(04)74307-6)

#### Analysis
* Open the timeseries data from each channel (w1,w2,w3)
* First, define a ROI that corresponds to background
* Next, define and add ROIs for each cell (For the example data set the ROIs are available in 'Galphai_ROI.csv.zip' - these ROIs can be shown in ImageJ by dropping the file on the application bar))
* Measure intensities by using the multimeasure option for each of the three channels (w1,w2,w3). The data is available [here](https://github.com/JoachimGoedhart/Quantify-FRET-ratio/tree/master/Example-data_processed) for each of the channels as a csv file.
* Copy the data into the right tabs of the enclosed excel file; data_analysis_Galphai.xlsx
* The result is a corrected FRET ratio in the last tab; 'NORM FRET(BT corrected)'
* Note: the correction factors beta and gamma can be adjusted in the tab that is used to correct the sensitized emission: 'YFP-BG-CFPbleed-YFPexc'


### Sample data and expected results
* Raw sample data is available in this [folder](https://github.com/JoachimGoedhart/Quantify-FRET-ratio/tree/master/Example-data_raw)
* Extracted intensity data together with the ROIs and the results of the measurement is available in this [folder](https://github.com/JoachimGoedhart/Quantify-FRET-ratio/tree/master/Example-data_processed)
* The excel file that can be used to correct the data with the beta and gamma factor is available
* The plot is directly shown in the excel file. Alternatively the processed data can be exported as csv and visualized with PlotTwist 

