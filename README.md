# Livestock Emissions Inventory: Enteric Fermentation in the Twin Cities (MN) 

This project calculates approximate methane emissions (measured in metric tons of CO2 equivalent) from enteric fermentation in the Twin Cities 7 county region. Its purpose is to contribute to the Metropolitan Council Research division's growing Greenhouse Gas Inventory. Cattle and other mammalian livestock are a significant source of methane, which has the potential to exacerbate global climate change. This inventory is being put together in order to provide public data on these greenhouse gases -- where they are coming from, and how much. 

## Code
All code is done in R.

## Method

Emissions (mt CO2e) = Animal Population x EF (*Emissions Factor*) x (1/1000) x GWP (*Global Warming Potential*)

The methodology is taken from the 2013 ICLEI protocol, specifically from Appendix G: Agricultural Livestock  Emission Activities and Sources (U.S. Community Protocol for Accounting and Reporting of Greenhouse Gas Emissions), summarized below: 

- *Step 1: Obtain data on animal populations*
- *Step 2: Choose appropriate emissions factor*
- *Step 3: Estimate methane emissions*
- *Step 4: Convert to metric tons CO2e*
- *Step 5: Sum all methane emissions from different livestock*

Emissions factors are the multiplier associated with each head of livestock (each animal). Emissions factors have been sourced from the Inventory of U.S. Greenhouse Gas Emissions and Sinks: 1990-2018 from the EPA.
*(Specifically, Annexes to the Inventory of U.S. GHG Emissions and Sinks, Annex 3.10,  Table A-176: Emission Factors for Cattle by Animal Type and State, for 2017 (kg CH4/head/year) and Table A-179: Emission Factors for Other Livestock (kg CH4/head/year) https://www.epa.gov/sites/production/files/2020-04/documents/us-ghg-inventory-2020-annexes.pdf)*

# Walkthrough 
The .rmd is annotated, but additional information may be helpful. 

## 1: Data
- 1.1: Data for the livestock totals are sourced from the USDA NASS dataset, but 7 county specific data used here are compiled by Metro Council. Data were collected at the end of December 2017. The emissions factor table was made from the Minnesota regional emissions factors from the EPA, sourced above. This table was compiled by myself and is stored with Metro Council. A field has been made for compatibility with the USDA dataset. Change paths for these files if replicating this process! 

- 1.2: Merging the emissions factor table with the livestock dataset for ease of use. 

- 1.3: The number of cattle calves is not available through the USDA NASS dataset;  therefore, this had to be estimated. 
These chunks roughly estimate the number of calves given a ratio of calves to all cattle statewide for the appropriate time period (NASS Census data were collected end of December 2017, NASS survey data were collected beginning of January 2018). The first R chunk is not necessary for use, but rather illustrates where the ratio is coming from. 
The second chunk uses the ratio value to add rows of approximate calf populations per county using the form *(calf inventory estimate = all cattle * ratio)*. 

- 1.4: This step simply appends the calf rows to the overall livestock dataset. 

## 2: Analysis: Calculating Emissions
- 2.1: This step sets up the emissions calculation. The first chunk sets some key values (GWP of methane, kg CH4 to metric tons CO2e conversion factor). The second chunk calculates emissions for each inventory group using the emissions factors, population counts, and key values. 

- 2.2: Emissions from all cattle are estimated using the assumption that all cattle emissions are found by adding the emissions from bulls, dairy cows, beef cows, and calves. Bulls are isolated using the assumption that the inventory item "ALL CATTLE EXCL COWS" minus the estimated number of calves represents bulls. Mutating a new column with these sums proved buggy and so a very manual approach was taken, adding items row by row. The final chunks merge these results to the main table and recode the livestock type categories to reflect all cattle. 
- 2.3: Emissions from all equine animals are estimated using a similar method to the cattle, except the sums are much simpler. The final chunks merge these results to the main table and recode the livestock type categoreis to reflect all equine.
- 2.4: Emissions from all livetock are estimated using the all cattle and all equine estimates plus goat, hog, and sheep inventories. This was also done in a similar method to the other totals. The final chunks merge these results to the main table and recode livestock type and commodity fields to reflect all livestock. 

# Credits
Thanks to Mauricio Leon and Kristen Peterson of Met Council and Ryan Cowen of the USDA for their contributions. 
