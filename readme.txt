The contents of this repository:

- binder/
	- environment.yml
	- install_instructions.txt

- dummy_data
	- 210528_travel_matrix.csv
	- dummy_activity_data.csv
	- dummy_scenarios_data.csv
	- dummy_clinic_data.csv
	- dummy_clinic_locations.csv
	- dummy_clinic_label_ocations.csv

- output_data/
	- master_df_dummydata.csv

- shapefiles/
	- GB_Postcodes/
	- county_boundaries/
	- Cornwall postcodes.csv
	- cornwall_postcode_sectors.csv
	- cornwall_postcode_areas.csv

- check_data_across_input_files.ipynb
- 210609_gm_with_results_dummydata.ipynb
- input_file_requirements.txt
- readme.txt [this file]

--------------------------------------------------------------------------------

See install_instructions.txt for information to install and use the environment (called "geopandas") so that you have the necessary software to run the code.

--------------------------------------------------------------------------------

There are 2 jupyter notebooks that are doing the sequence of work:

File 1. "check_data_across_input_files.ipynb"

This one reads in the input files and checks that all of the data is present across all of the files. For example, are all of the postcode sectors in the activity file in the travel matrix file.

This code showed the need to find out the full list of postcode sectors in Cornwall. 

This code also extracts the postcode sectors from the file "Cornwall postcodes.csv" 
(downloaded from https://www.doogal.co.uk/AdministrativeAreas.php?district=E06000052)

Finds 117 unique postcode sectors in Cornwall.

There are 14 postcdoe sectors included in the full list and not in the Travel matrix (these need to be included in the Travel Matrix to future proof the model):
['TR13 3', 'PL27 9', 'TR18 9', 'TR15 9', 'TR26 9', 'PL12 9', 'TR27 9', 'EX23 3', 'TR11 9', 'PL13 9', 'PL17 0', 'TR7 9', 'PL15 0', 'PL31 9']

There are the 4 postcode sectors included in both the the activity data and Travel matrix that are not in the full list (these are the Isles of Scilly and will be treated differently in the model):
['TR25 0', 'TR23 0', 'TR22 0', 'TR24 0']

--------------------------------------------------------------------------------

File 2. "210609_gm_with_results_dummydata.ipynb"

This is the geographical model that requires 6 input files. Please refer to "input_file_requirements.txt" to see what coloumn names are required for each file.

This code uses 6 dummy input files (travel, activity, scenarios, clinic data, clinic locations, clinic label locations) to create a single DataFrame (master_df) with the following columns:
    pc_sector
    treatment_function
    total_new_adms
    total_followup_adms
    scenario_title
    pc_followup_adms_inperson
    pc_new_adms_inperson
    new_adms_inperson
    followup_adms_inperson
    new_adms_virtual
    followup_adms_virtual
    min_distance
    nearest_clinic
    clinic_label

So for each postcode sector we have the information about the number of admissions that are travelling to their nearest clinic under each scenario, and for each treatment function. These admissions are divided into new admissions, and followup admissions.

The scenarios explore changing the proportion of admissions (by treatment function) that are to travel and attend their appointment in person. The remainder of the admissions have virtual appointments.

This notebook will use the data in the DataFrame (master_df) to calculate some example performance metrics, in a range of display formats (table, stacked barchart, maps). To date these include:

* total km travelled & co2 emitted (per scenario). Format: Table and bar chart
  (also broken down by new and followup admissions, and by treatment function. Format: Table and bar chart)
* co2 emitted (per scenario) by postcode. Format: Map
* number of admissions (new and followup) seen by each clinic (virtually and in person). Format: Table and bar chart

External files used for this code:
* postcode sector shapefile from https://datashare.ed.ac.uk/handle/10283/2597 downloaded GB_Postcodes.zip (180.8Mb)
* county shapefile from https://data.gov.uk/dataset/11302ddc-65bc-4a8f-96a9-af5c456e442c/counties-and-unitary-authorities-december-2016-full-clipped-boundaries-in-england-and-wales

--------------------------------------------------------------------------------

Note: In all of these calculates, need to update and use the correct km to CO2 factor
