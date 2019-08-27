# Pittsburg Metro 21: Fire Risk Analysis Template

Edit:  This is an modified template of the original open-source Pittsburg Fire Risk model (also called [FireGuru](https://www.youtube.com/watch?v=j5AZngew2bg) and also called Firebird 2.0).  The Pittsburg model was by [Dr. Michael Madio](http://michaelmadaio.com/) and his team for Pittsburg fire department.   This fork is to serve as an educational template for the Atlytics event to help Altanta metrocounties predict fire risk for *new types of fires like residental fires*.  However, Pittsburg's Fire Risk model was designed to predict commerical fire risk, and Atlytics data is complete different for residental fires.  Still, the fire risk model was being modified for predicting residental fire risk as of Feb 05 2019.  Furthermore its based on the original firebird that was also a commerical fire risk model. As consequence, it should only serve as a starting point for predicting any other noncommerical type of fire risk. **Please give citation and credit to the original creators** (listed as authors below) not this fork. 

# Fire Risk Analysis

This is a set of scripts for a machine learning pipeline to predict structure fire risk and inform fire inspection prioritization decisions. A full technical report can be found [here](http://michaelmadaio.com/Metro21_FireRisk_FinalReport.pdf).  For more details on the most recent changes please (see Singh Walia, Bhavkaran & Madaio, et al. (2018). A Dynamic Pipeline for Spatio-Temporal Fire Risk Prediction. 764-773. 10.1145/3219819.3219913)

## Residental Risk Model
  The residental risk model the team developed during "A Dynamic Pipeline for Spatio-Temporal Fire Risk Prediction" and "A Longitudinal Evaluation of a Deployed Predictive Model of Fire Risk" in 2018-2019 can be found here.

## Run_Model.sh
Runs all three python scripts listed below in succession.

## getdata.py

Scrapes [WPRDC](https://wprdc.org) for:
* City of Pittsburgh property data ("pittdata.csv") 
* City of Pittsburgh parcel data ("parcels.csv")
* Permits, Licenses, and Inspections data ("pli.csv") (updated every 12 hours)

## riskmodel.py

Runs the risk prediction model, using:
* the three datasets from WPRDC
* Fire Incident data from PBF (public, aggregated version available at [WPRDC](https://data.wprdc.org/dataset/fire-incidents-in-city-of-pittsburgh). However, please note that due to privacy concerns, the most detailed fire incident data that the model is trained on are not publicly accessible, but the aggregated version of the incident data is available, at the block-level, instead of the address-level. At the moment, this script is not able to run on the aggregated, block-level data. (Edit: This anonymization was a slight issue so the data provide is mostly a simulation

## merger.py

Takes the output of the risk model, and merges each property's risk score with the rest of the property data in pittdata and parcels, sending the output to the Burgh's Eye View directory for map and dashboard visualization (on a private instance developed for Bureau of Fire inspectors; public version of BEV available [here](https://pittsburghpa.shinyapps.io/BurghsEyeView/?_inputs_&basemap_select=%22OpenStreetMap.Mapnik%22&circumstances_select=null&crash_select=null&dept_select=null&dow_select=null&filter_select=%22%22&fire_desc_select=null&funcarea_select=null&heatVision=0&hier=null&map_bounds=%7B%22north%22%3A40.6035267998859%2C%22east%22%3A-79.5238494873047%2C%22south%22%3A40.290001686076%2C%22west%22%3A-80.4027557373047%7D&map_center=%7B%22lng%22%3A-79.9629625321102%2C%22lat%22%3A40.4467468302211%7D&map_zoom=11&navTab=%22Points%22&offense_select=null&origin_select=null&report_select=%22311%20Requests%22&req.type=null&result_select=null&search=%22%22&status_type=null&times=%5B0%2C24%5D&toggle311=true&toggleArrests=true&toggleBlotter=true&toggleCitations=true&toggleCproj=true&toggleCrashes=false&toggleFires=true&toggleViolations=true&violation_select=null))

## Fire Risk Dashboard: ui.R and server.R

Takes the output of the risk scores, merged with property data, and visualizes them in an R Shiny dashboard, for inspectors and fire chiefs to view property risk levels, by property type, neighborhood, and fire district.

## requirements.txt

All of the packages you'll need to install for the scripts to run.

## Extra Folders
### datasets
* Useful data dictionaries can be found in the dictionary subfolder.  Of important note though is the *Fire Code Dictionary* (called data_dictionary_incident_type_fire_codes.xls).  It contains the national standard fire codes.
Though its *strongly* encourage to use APIs, the data can be obtained from the following locations:
* [Fire Inspection dataset](https://data.wprdc.org/dataset/fire-incidents-in-city-of-pittsburgh): ~20,000 fires and 5 years.  See riskmodel.py below for more details about issues with the data.  Due to these issues the data is can be considered a simulated or rough approximation.
* [Pittsburg Commerical Inspection and Permit data](https://data.wprdc.org/dataset/pittsburgh-pli-violations-report)(updated daily):  This is the data that we can to compare the commerical fires too as technical report notes.  *For residental fire risk we will not have fire inspections, so we will probably use the property data and other sources (like smoke alarm locations).  Still its a starting point.*
* [Pittsburg Properties](https://data.wprdc.org/dataset/property-assessments)(updated monthly):  Allegheny County Property Assessments. The map can be dashboard found [here](http://tools.wprdc.org/property-dashboard/).  The old source is linked below.
* [Pittsburg Parcels](https://data.wprdc.org/dataset/parcel-centroids-in-allegheny-county-with-geographic-identifiers)(updated yearly): Allegheny Parcel Centroids for the city of Pittsburg.  The old source is linked below.
*Residental Datasets*: The following data sources were used for residental simulation deeply buried in the code.
* [Unpaid Tax Information](https://data.wprdc.org/dataset/allegheny-county-tax-liens-filed-and-satisfied): The Pittsburg Unpaid Tax information.  Useful for which regions have finacial difficulties related to property.  Its related to vacancy, but a better measure might be vacany reports from the police.
* occupancy.csv: The Census occupancy information by blockgroup (using the Census API)
* income.csv: The Census median housing income information by blockgroup.
As a curtousy, copies of these datasources have been zipped to the datasets folder.
### articles
 This is a collection of various research articles people might find useful.
### image and log
  The output of the models can be found here.

### Authors: 
* Michael Madaio
* Geoffrey Arnold
* Bhavkaran Singh
* Qianyi Hu
* Nathan Kuo
* Jason Batts

