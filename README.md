[![Build Status](https://travis-ci.org/OpenEnergyPlatform/edatamodel.svg?branch=develop)](https://travis-ci.org/OpenEnergyPlatform/oedatamodel)

<a href="http://oep.iks.cs.ovgu.de/"><img align="right" width="200" height="200" src="https://avatars2.githubusercontent.com/u/37101913?s=400&u=9b593cfdb6048a05ea6e72d333169a65e7c922be&v=4" alt="OpenEnergyPlatform"></a>

# Open Energy Family - Datamodel
A common open energy data model (oedatamodel) and datapackage format for energy and scenario data.

# Introduction
The oedatamodel is provided as a template data model as Entity Relationship Modell(ERM) and is designed
for the usage on the open energy platform but it can be used in common relational database systems. 
Additional we include a datapackage for every release. 

Existing approaches and ideas such as the [IAMC data format](https://github.com/IAMconsortium/pyam#data-model) or [Do-a-thon: Towards a common data standard 
for integrated assessment and energy systems modelling](https://forum.openmod-initiative.org/t/do-a-thon-towards-a-common-data-standard-for-integrated-assessment-and-energy-systems-modelling/1774/5) were adopted in the development process.

The latest version can be found in the folder [oedatamodel/latest](https://github.com/OpenEnergyPlatform/oedatamodel/tree/develop/oedatamodel/latest). 
The version available there offers the user 2 ERM's. The ERM "OEDataModel-normalization.pdf" shows the data model 
that can be implemented on a database (e.g. postgresql). Here tables, relations, column names and 
data types are provided. The ERM "OEDataModel-concrete.pdf" is provided additionally and is designed 
to simplify the editing of the data by a user. It also provides tables and relations as well as column 
names and datatypes, but the tables are in a less normalized format. We recommend this v~~~~ersion of the 
data model for the implementation in e.g. csv tables because the advantage of the more human usable format 
is not optimal for the technical usage in a database. 

In addition, the raw files (file format: .er) from which the PDF respectively the ERM is generated are
also provided for each data model.

# Oedatamodel - Datapackage

We publish a data package for each release. The data package contains the file oedatamodel_datapackage.json 
with example and template content. This includes CSV files representing the data model. The JSON 
file contains the [oemetadata format](https://github.com/OpenEnergyPlatform/oemetadata), which is used to store/provide metadata for open data on the OEP. 
This includes:

- A general description of the data
- List of contributions 
- Licence information 
- Information about sources like records or model frameworks with license information for each source
- The datamodel with metadata for each field
- Table relations and key attributes to provide the information outside a database
- The metadata string itself can be validated using the [Open Metadata Integration (omi)](https://github.com/OpenEnergyPlatform/omi) tool
 
Oemetadata provides a [detailed description](https://github.com/OpenEnergyPlatform/oemetadata/blob/develop/metadata/latest/metadata_key_description.md) with examples for each key in the metadata string.

# Oedatamodel - Usage

## OEDataModel variations

- OEDataModel-parameter
    - Main usage as CSV files
    - Optimized for user-friendly documentation of parameters for energy systems modelling
    - Tool that maps the parameter model to the concrete and/or normalization model will be provided. Please be aware about new features.

- OEDataModel-concrete
    - Main usage as CSV files
    - Tool that maps the concrete model to the normalization model will be provided. Please be aware about new features.

- OEDataModel-normalization
    - Main usage database (realtional database like postgreSQL)
    - Optimized to store data in a relational data model
    - Normalization for practical data relationships and reduced/no redundant fields to avoid redundant data

## Fields that are not present or empty in your data

When transferring data from your own data format to the oedatamodel, it may happen that fields cannot be filled properly. In this case we recommend not to leave the field empty and to insert the value "unkown" into the field.

## Delimiter and decimal sepperators

If you transfer data into the oedatamodel format make sure to use the delimiter ";" in e.g. .csv files for delimiting values. Decimal numbers are separated by a "." .

Example:
| **series**         | 
|--------------------|
| [1423.55706450302; 1566.42140196079]|

## Apply a Datapackage to the Database

[Guide to publish data on the OEP](https://github.com/OpenEnergyPlatform/tutorial/blob/develop/upload/OEP_Research_Data_Publishing_Guidebook.ipynb)

In short:
- Create a OEP compatible [Datapackage](https://github.com/OpenEnergyPlatform/oedatamodel/tree/develop/oedatamodel/latest/v100/datapackage/OEDataModel-concrete-datapackage)
- Start a OEP-Review process
- Start the Upload-Process 

[Guide to upload data to the OEP](https://github.com/OpenEnergyPlatform/tutorial/blob/develop/upload/OEP_Upload_Process_Data_and_Metadata_oem2orm.ipynb)

In short:
- Use your Datapackage files to:
    - Create Tables from the OEDataModel-Datapackage.json file usign [oem2orm](https://pypi.org/project/oem2orm/)
    - Apply OEMetadata to the table
    - Upload your data to the tables from the .csv files in OEDataModel format using Pandas with the oedialect

## Description and examples

### OEDataModel-parameter

### Scalar description

| **Field**      | **Datatype** | **Description**                                                                                                                                                                                             |
|----------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id             | integer      | A primary key is a field or set of fields that uniquely identifies each row in the table. It's recorded as a list of strings, since it is possible to define the primary key as made up of several columns. |
| region         | text         | It describes the geographical scope of the dataset.                                                                                                                                                         |
| year           | integer      | It describes the time frame of the dataset.                                                                                                                                                                 |
| type           | text         | Is used distinguish different process types in a dataset.                                                                                                                                                   |
| parameter1     | float array  | First column for parameter input (unlimited additional parameter columns can be added)                                                                                                                      |
| parameter2     | float array  | Second Column for parameter input (unlimited additional parameter columns can be added)                                                                                                                     |
| bandwidth_type | json         | It describes the bandwidths type of the values in the parameter columns.                                                                                                                                    |
| version        | text         | It describes the version of the values in the parameter columns.                                                                                                                                            |
| method         | json         | It describes the procedure for obtaining the value, in case it does not originate from a single source.                                                                                                     |
| source         | json         | Human readable title of the source, e.g. document title or organisation name. The source must relate to a source provided in the oemetadata (datapackage) file.                                             |
| comment        | json         | Free text comment on what's been done.                                                                                                                                                                      |

For more information see: scalar [datapackage](https://github.com/sedos-project/oedatamodel/blob/main/oedatamodel-parameter/datamodel_scalars.json)

### Example table:

| id  | region  | year | type     | generating_capacity_for_one_unit_MW | average_annual_full_load_hours | forced_outage   | planned_outage      | technical_lifetime_years | construction_time_years | space_requirement_1000m2_MW | primary_regulation | secondary_regulation | nominal_investment | equipment_share_of_investment | installation_share_of_investment | grid_connection_share_of_investment | fixed_O_M           | variable_O_M  | rotor_diameter | hub_height    | specific_power | average_capacity_factor | average_availability | specific_area_coverage | bandwidth_type                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | version                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | method                                            | source    | comment                         |
|-----|---------|------|----------|-------------------------------------|--------------------------------|------------------|---------------------|--------------------------|-------------------------|-----------------------------|--------------------|----------------------|--------------------|-------------------------------|----------------------------------|-------------------------------------|---------------------|---------------|----------------|---------------|----------------|-------------------------|----------------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|-----------|---------------------------------|
| 1   | Europe  | 2015 | onshore  | [8]                                 | 4400                           | 0.04             | 0.003               | 25                       | 3                       | 185                         |                    |                      | 2.86               | 1.11                          | 1.35                             | 0.4                                 | 57300               | 4.3           | 164            | 103           | 379            | 50                      | 0.96                 | 5.4                    | {'generating_capacity_for_one_unit_MW':'2S','average_annual_full_load_hours':'discrete','forced_outage':'discrete','planned_outage':'discrete','technical_lifetime_years':'discrete','construction_time_years':'discrete','space_requirement_1000m2_MW':'discrete','primary_regulation':'discrete','secondary_regulation':'discrete','nominal_investment':'discrete','equipment_share_of_investment':'discrete','installation_share_of_investment':'discrete','grid_connection_share_of_investment':'discrete','fixed_O_M':'discrete','variable_O_M':'discrete','rotor_diameter':'discrete','hub_height':'discrete','specific_power':'discrete','average_capacity_factor':'discrete','average_availability':'discrete','specific_area_coverage':'discrete'}                                                                      | {'generating_capacity_for_one_unit_MW':'v1','average_annual_full_load_hours':'v1','forced_outage':'v1','planned_outage':'v1','technical_lifetime_years':'v1','construction_time_years':'v1','space_requirement_1000m2_MW':'v1','primary_regulation':'v1','secondary_regulation':'v1','nominal_investment':'v1','equipment_share_of_investment':'v1','installation_share_of_investment':'v1','grid_connection_share_of_investment':'v1','fixed_O_M':'v1','variable_O_M':'v1','rotor_diameter':'v1','hub_height':'v1','specific_power':'v1','average_capacity_factor':'v1','average_availability':'v1','specific_area_coverage':'v1'} | {'generating_capacity_for_one_unit_MW':'average'} | DEA, DEA2 | 03b Coal to wood chips n. boile |
| 2   | Europe  | 2020 | onshore  | [8,10,12]                           | [4200,4500,5000]                    | [0.01,0.03,0.05] | [0.001,0.003,0.005] | [20,27,35]               | [1.5,2.5,4]             | [180,220,240]               |                    |                      | [1.92,2.13,2.23]   | [0.71,0.79,0.83]              | [0.87,0.96,1.01]                 | [0.34,0.38,0.4]                     | [36053,40059,42062] | [2.7,3,3.1]   | [165,190,280]  | [100,115,160] | [332,353,366]  | [46,51,57]              | [0.99,0.97,0.95]     | [5.6,4.5,4.2]          | {'generating_capacity_for_one_unit_MW':'uncertainty','average_annual_full_load_hours':'uncertainty','forced_outage':'uncertainty','planned_outage':'uncertainty','technical_lifetime_years':'uncertainty','construction_time_years':'uncertainty','space_requirement_1000m2_MW':'uncertainty','primary_regulation':'uncertainty','secondary_regulation':'uncertainty','nominal_investment':'uncertainty','equipment_share_of_investment':'uncertainty','installation_share_of_investment':'uncertainty','grid_connection_share_of_investment':'uncertainty','fixed_O_M':'uncertainty','variable_O_M':'uncertainty','rotor_diameter':'uncertainty','hub_height':'uncertainty','specific_power':'uncertainty','average_capacity_factor':'uncertainty','average_availability':'uncertainty','specific_area_coverage':'uncertainty'} | {'generating_capacity_for_one_unit_MW':'v1','average_annual_full_load_hours':'v1','forced_outage':'v1','planned_outage':'v1','technical_lifetime_years':'v1','construction_time_years':'v1','space_requirement_1000m2_MW':'v1','primary_regulation':'v1','secondary_regulation':'v1','nominal_investment':'v1','equipment_share_of_investment':'v1','installation_share_of_investment':'v1','grid_connection_share_of_investment':'v1','fixed_O_M':'v1','variable_O_M':'v1','rotor_diameter':'v1','hub_height':'v1','specific_power':'v1','average_capacity_factor':'v1','average_availability':'v1','specific_area_coverage':'v1'} |                                                   | DEA       |                                 |
| 3   | Europe  | 2030 | onshore  | [15]                                | 4650                                | 0.03             | 0.003               | 30                       | 2.5                     | 220                         |                    |                      | 1.93               | 0.71                          | 0.87                             | 0.36                                | 36053               | 2.7           | 235            | 135           | 346            | 53                      | 0.97                 | 4.5                    | {'generating_capacity_for_one_unit_MW':'discrete','average_annual_full_load_hours':'discrete','forced_outage':'discrete','planned_outage':'discrete','technical_lifetime_years':'discrete','construction_time_years':'discrete','space_requirement_1000m2_MW':'discrete','primary_regulation':'discrete','secondary_regulation':'discrete','nominal_investment':'discrete','equipment_share_of_investment':'discrete','installation_share_of_investment':'discrete','grid_connection_share_of_investment':'discrete','fixed_O_M':'discrete','variable_O_M':'discrete','rotor_diameter':'discrete','hub_height':'discrete','specific_power':'discrete','average_capacity_factor':'discrete','average_availability':'discrete','specific_area_coverage':'discrete'}                                                                | {'generating_capacity_for_one_unit_MW':'v1','average_annual_full_load_hours':'v1','forced_outage':'v1','planned_outage':'v1','technical_lifetime_years':'v1','construction_time_years':'v1','space_requirement_1000m2_MW':'v1','primary_regulation':'v1','secondary_regulation':'v1','nominal_investment':'v1','equipment_share_of_investment':'v1','installation_share_of_investment':'v1','grid_connection_share_of_investment':'v1','fixed_O_M':'v1','variable_O_M':'v1','rotor_diameter':'v1','hub_height':'v1','specific_power':'v1','average_capacity_factor':'v1','average_availability':'v1','specific_area_coverage':'v1'} |                                                   | DEA       |                                 |
| 4   | Europe  | 2040 | onshore  | 18                                  | 4700                                | 0.025            | 0.003               | 30                       | 2.5                     | 220                         |                    |                      | 1.81               | 0.65                          | 0.8                              | 0.36                                | 33169               | 2.5           | 260            | 150           | 339            | 54                      | 0.97                 | 4.5                    | {'generating_capacity_for_one_unit_MW':'discrete','average_annual_full_load_hours':'discrete','forced_outage':'discrete','planned_outage':'discrete','technical_lifetime_years':'discrete','construction_time_years':'discrete','space_requirement_1000m2_MW':'discrete','primary_regulation':'discrete','secondary_regulation':'discrete','nominal_investment':'discrete','equipment_share_of_investment':'discrete','installation_share_of_investment':'discrete','grid_connection_share_of_investment':'discrete','fixed_O_M':'discrete','variable_O_M':'discrete','rotor_diameter':'discrete','hub_height':'discrete','specific_power':'discrete','average_capacity_factor':'discrete','average_availability':'discrete','specific_area_coverage':'discrete'}                                                                | {'generating_capacity_for_one_unit_MW':'v1','average_annual_full_load_hours':'v1','forced_outage':'v1','planned_outage':'v1','technical_lifetime_years':'v1','construction_time_years':'v1','space_requirement_1000m2_MW':'v1','primary_regulation':'v1','secondary_regulation':'v1','nominal_investment':'v1','equipment_share_of_investment':'v1','installation_share_of_investment':'v1','grid_connection_share_of_investment':'v1','fixed_O_M':'v1','variable_O_M':'v1','rotor_diameter':'v1','hub_height':'v1','specific_power':'v1','average_capacity_factor':'v1','average_availability':'v1','specific_area_coverage':'v1'} |                                                   | DEA       |                                 |
| 5   | Europe  | 2050 | onshore  | [10,20,40,30]                       | [4500,4900,5500]                    | [0.01,0.02,0.05] | [0.001,0.003,0.005] | [20,30,35]               | [1.5,2,4]               | [180,220,240]               |                    |                      | [1.42,1.78,1.95]   | [0.51,0.64,0.7]               | [0.62,0.78,0.86]                 | [0.29,0.36,0.4]                     | [25958,32448,35692] | [1.9,2.4,2.6] | [190,280,380]  | [115,160,220] | [310,325,340]  | [46,56,63]              | [0.99,0.98,0.95]     | [5.6,4.5,4.2]          | {'generating_capacity_for_one_unit_MW':'uncertainty','average_annual_full_load_hours':'uncertainty','forced_outage':'uncertainty','planned_outage':'uncertainty','technical_lifetime_years':'uncertainty','construction_time_years':'uncertainty','space_requirement_1000m2_MW':'uncertainty','primary_regulation':'uncertainty','secondary_regulation':'uncertainty','nominal_investment':'uncertainty','equipment_share_of_investment':'uncertainty','installation_share_of_investment':'uncertainty','grid_connection_share_of_investment':'uncertainty','fixed_O_M':'uncertainty','variable_O_M':'uncertainty','rotor_diameter':'uncertainty','hub_height':'uncertainty','specific_power':'uncertainty','average_capacity_factor':'uncertainty','average_availability':'uncertainty','specific_area_coverage':'uncertainty'} | {'generating_capacity_for_one_unit_MW':'v1','average_annual_full_load_hours':'v1','forced_outage':'v1','planned_outage':'v1','technical_lifetime_years':'v1','construction_time_years':'v1','space_requirement_1000m2_MW':'v1','primary_regulation':'v1','secondary_regulation':'v1','nominal_investment':'v1','equipment_share_of_investment':'v1','installation_share_of_investment':'v1','grid_connection_share_of_investment':'v1','fixed_O_M':'v1','variable_O_M':'v1','rotor_diameter':'v1','hub_height':'v1','specific_power':'v1','average_capacity_factor':'v1','average_availability':'v1','specific_area_coverage':'v1'} |                                                   | DEA       |                                 |
| 6   | Europe  | 2030 | offshore | [15]                                | 4650                                | 0.<br/>03             | 0.003               | 30                       | 2.5                     | 220                         |                    |                      | 1.93               | 0.71                          | 0.87                             | 0.36                                | 36053               | 2.7           | 235            | 135           | 346            | 53                      | 0.97                 | 4.5                    | {'generating_capacity_for_one_unit_MW':'discrete','average_annual_full_load_hours':'discrete','forced_outage':'discrete','planned_outage':'discrete','technical_lifetime_years':'discrete','construction_time_years':'discrete','space_requirement_1000m2_MW':'discrete','primary_regulation':'discrete','secondary_regulation':'discrete','nominal_investment':'discrete','equipment_share_of_investment':'discrete','installation_share_of_investment':'discrete','grid_connection_share_of_investment':'discrete','fixed_O_M':'discrete','variable_O_M':'discrete','rotor_diameter':'discrete','hub_height':'discrete','specific_power':'discrete','average_capacity_factor':'discrete','average_availability':'discrete','specific_area_coverage':'discrete'}                                                                | {'generating_capacity_for_one_unit_MW':'v1','average_annual_full_load_hours':'v1','forced_outage':'v1','planned_outage':'v1','technical_lifetime_years':'v1','construction_time_years':'v1','space_requirement_1000m2_MW':'v1','primary_regulation':'v1','secondary_regulation':'v1','nominal_investment':'v1','equipment_share_of_investment':'v1','installation_share_of_investment':'v1','grid_connection_share_of_investment':'v1','fixed_O_M':'v1','variable_O_M':'v1','rotor_diameter':'v1','hub_height':'v1','specific_power':'v1','average_capacity_factor':'v1','average_availability':'v1','specific_area_coverage':'v1'} |                                                   | DEA       |                                 |
| 7   | Europe  | 2040 | offshore | 18                                  | 4700                                <br/>| 0.025            | 0.003               | 30                       | 2.5                     | 220                         |                    |                      | 1.81               | 0.65                          | 0.8                              | 0.36                                | 33169               | 2.5           | 260            | 150           | 339            | 54                      | 0.97                 | 4.5                    | {'generating_capacity_for_one_unit_MW':'discrete','average_annual_full_load_hours':'discrete','forced_outage':'discrete','planned_outage':'discrete','technical_lifetime_years':'discrete','construction_time_years':'discrete','space_requirement_1000m2_MW':'discrete','primary_regulation':'discrete','secondary_regulation':'discrete','nominal_investment':'discrete','equipment_share_of_investment':'discrete','installation_share_of_investment':'discrete','grid_connection_share_of_investment':'discrete','fixed_O_M':'discrete','variable_O_M':'discrete','rotor_diameter':'discrete','hub_height':'discrete','specific_power':'discrete','average_capacity_factor':'discrete','average_availability':'discrete','specific_area_coverage':'discrete'}                                                                | {'generating_capacity_for_one_unit_MW':'v1','average_annual_full_load_hours':'v1','forced_outage':'v1','planned_outage':'v1','technical_lifetime_years':'v1','construction_time_years':'v1','space_requirement_1000m2_MW':'v1','primary_regulation':'v1','secondary_regulation':'v1','nominal_investment':'v1','equipment_share_of_investment':'v1','installation_share_of_investment':'v1','grid_connection_share_of_investment':'v1','fixed_O_M':'v1','variable_O_M':'v1','rotor_diameter':'v1','hub_height':'v1','specific_power':'v1','average_capacity_factor':'v1','average_availability':'v1','specific_area_coverage':'v1'} |                                                   | DEA       |                                 |
| 8   | Europe  | 2050 | offshore | [10,20,40,30]                       | [4500,4900,5500]                    <br/>| [0.01,0.02,0.05] | [0.001,0.003,0.005] | [20,30,35]               | [1.5,2,4]               | [180,220,240]               |                    |                      | [1.42,1.78,1.95]   | [0.51,0.64,0.7]               | [0.62,0.78,0.86]                 | [0.29,0.36,0.4]                     | [25958,32448,35692] | [1.9,2.4,2.6] | [190,280,380]  | [115,160,220] | [310,325,340]  | [46,56,63]              | [0.99,0.98,0.95]     | [5.6,4.5,4.2]          | {'generating_capacity_for_one_unit_MW':'uncertainty','average_annual_full_load_hours':'uncertainty','forced_outage':'uncertainty','planned_outage':'uncertainty','technical_lifetime_years':'uncertainty','construction_time_years':'uncertainty','space_requirement_1000m2_MW':'uncertainty','primary_regulation':'uncertainty','secondary_regulation':'uncertainty','nominal_investment':'uncertainty','equipment_share_of_investment':'uncertainty','installation_share_of_investment':'uncertainty','grid_connection_share_of_investment':'uncertainty','fixed_O_M':'uncertainty','variable_O_M':'uncertainty','rotor_diameter':'uncertainty','hub_height':'uncertainty','specific_power':'uncertainty','average_capacity_factor':'uncertainty','average_availability':'uncertainty','specific_area_coverage':'uncertainty'} | {'generating_capacity_for_one_unit_MW':'v1','average_annual_full_load_hours':'v1','forced_outage':'v1','planned_outage':'v1','technical_lifetime_years':'v1','construction_time_years':'v1','space_requirement_1000m2_MW':'v1','primary_regulation':'v1','secondary_regulation':'v1','nominal_investment':'v1','equipment_share_of_investment':'v1','installation_share_of_investment':'v1','grid_connection_share_of_investment':'v1','fixed_O_M':'v1','variable_O_M':'v1','rotor_diameter':'v1','hub_height':'v1','specific_power':'v1','average_capacity_factor':'v1','average_availability':'v1','specific_area_coverage':'v1'} |                                                   | DEA       |                                 |


### Timeseries description

| **Field**            | **Datatype**                                                            | **Description**                                                                                                                                                                                             |
|----------------------|-------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                   | integer                                                                 | A primary key is a field or set of fields that uniquely identifies each row in the table. It's recorded as a list of strings, since it is possible to define the primary key as made up of several columns. |
| region               | text                                                                    | It describes the geographical scope of the dataset.                                                                                                                                                         |
| type                 | text                                                                    | Is used distinguish different process types in a dataset.                                                                                                                                                   |
| timeindex start      | [timestamp](https://www.postgresql.org/docs/9.5/datatype-datetime.html) | Both date and time, with time zone.                                                                                                                                                                         |
| timeindex stop       | [timestamp](https://www.postgresql.org/docs/9.5/datatype-datetime.html) | Both date and time, with time zone.                                                                                                                                                                         |
| timeindex resolution | [interval](https://www.postgresql.org/docs/9.5/datatype-datetime.html)  | The time span between individual points of information in a time series.                                                                                                                                    |
| series1              | float array                                                             | First column for series input (unlimited additional series columns can be added)                                                                                                                            |
| series2              | float array                                                             | Second Column for series input (unlimited additional series columns can be added)                                                                                                                           |
| version              | text                                                                    | It describes the version of the values in the parameter columns.                                                                                                                                            |
| method               | json                                                                    | It describes the procedure for obtaining the value, in case it does not originate from a single source.                                                                                                     |
| source               | json                                                                    | Human readable title of the source, e.g. document title or organisation name. The source must relate to a source provided in the oemetadata (datapackage) file.                                             |
| comment              | json                                                                    | Free text comment on what's been done.                                                                                                                                                                      | 

For more information see: timeseries [datapackage](https://github.com/sedos-project/oedatamodel/blob/main/oedatamodel-parameter/datamodel_timeseries.json)

### Example table:

| id  | region | type    | timeindex_resolution | timeindex_start           | timeindex_stop            | wind_speed_10m                                                                                                                                                                                                                                | wind_speed_100m                                                                                                                                                                                                                             | version                                           | method | source                                                                           | comment |
|-----|--------|---------|---------------------|---------------------------|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|--------|----------------------------------------------------------------------------------|---------|
| 1   | Europe | onshore | 1h                  | 2016-01-01 00:00:00+01:00 | 2016-12-31 23:00:00+01:00 | [1,1597.0,1519.64,1471.85,1451.37,1445.4,1456.9,1525.83,1658.16,1863.97,2051.03,2218.37,2280.12,2157.11,2022.07,88,2931.02,2805.22,2754.16,2764.23,2624.13,2596.24,2575.67,2628.21,2587.71,2414.04,2209.3,1986.03,1805.76,1699.03,1651.18,16] | [1597.0,1519.64,1471.85,1451.37,1445.4,1456.9,1525.83,1658.16,1863.97,2051.03,2218.37,2280.12,2157.11,2022.07,88,2931.02,2805.22,2754.16,2764.23,2624.13,2596.24,2575.67,2628.21,2587.71,2414.04,2209.3,1986.03,1805.76,1699.03,1651.18,16] | {“wind_speed_10m”: “v1”, “wind_speed_100m”: “v1”} |        | {“wind_speed_10m”: “[EAS2018, ISA2019]”, “wind_speed_100m”:”RENEWABLENINJA2019”} |         |
| 2   | Europe | onshore | 1h                  | 2016-01-01 00:00:00+01:00 | 2016-12-31 23:00:00+01:00 | [2,1597.0,1519.64,1471.85,1451.37,1445.4,1456.9,1525.83,1658.16,1863.97,2051.03,2218.37,2280.12,2157.11,2022.07,88,2931.02,2805.22,2754.16,2764.23,2624.13,2596.24,2575.67,2628.21,2587.71,2414.04,2209.3,1986.03,1805.76,1699.03,1651.18,16] |                                                                                                                                                                                                                                             | {“wind_speed_10m”: “v2”}                          |        | {“wind_speed_10m”: “EAS2018”, “wind_speed_100m”:”RENEWABLENINJA2019”}            |         |
| 3   | Europe | onshore | 1h                  | 2020-01-01 00:00:00+01:00 | 2020-12-31 23:00:00+01:00 | [3,1597.0,1519.64,1471.85,1451.37,1445.4,1456.9,1525.83,1658.16,1863.97,2051.03,2218.37,2280.12,2157.11,2022.07,88,2931.02,2805.22,2754.16,2764.23,2624.13,2596.24,2575.67,2628.21,2587.71,2414.04,2209.3,1986.03,1805.76,1699.03,1651.18,16] |                                                                                                                                                                                                                                             | {“wind_speed_10m”: “v3”}                          |        | {“wind_speed_10m”: “EAS2018”, “wind_speed_100m”:”RENEWABLENINJA2019”}            |         |
| 4   | Europe | offhore | 1h                  | 2016-01-01 00:00:00+01:00 | 2016-12-31 23:00:00+01:00 | [1,1597.0,1519.64,1471.85,1451.37,1445.4,1456.9,1525.83,1658.16,1863.97,2051.03,2218.37,2280.12,2157.11,2022.07,88,2931.02,2805.22,2754.16,2764.23,2624.13,2596.24,2575.67,2628.21,2587.71,2414.04,2209.3,1986.03,1805.76,1699.03,1651.18,16] | [1597.0,1519.64,1471.85,1451.37,1445.4,1456.9,1525.83,1658.16,1863.97,2051.03,2218.37,2280.12,2157.11,2022.07,88,2931.02,2805.22,2754.16,2764.23,2624.13,2596.24,2575.67,2628.21,2587.71,2414.04,2209.3,1986.03,1805.76,1699.03,1651.18,16] | {“wind_speed_10m”: “v1”, “wind_speed_100m”: “v1”} |        | {“wind_speed_10m”: “[EAS2018, ISA2019]”, “wind_speed_100m”:”RENEWABLENINJA2019”} |         |
| 5   | Europe | offhore | 1h                  | 2016-01-01 00:00:00+01:00 | 2016-12-31 23:00:00+01:00 | [2,1597.0,1519.64,1471.85,1451.37,1445.4,1456.9,1525.83,1658.16,1863.97,2051.03,2218.37,2280.12,2157.11,2022.07,88,2931.02,2805.22,2754.16,2764.23,2624.13,2596.24,2575.67,2628.21,2587.71,2414.04,2209.3,1986.03,1805.76,1699.03,1651.18,16] |                                                                                                                                                                                                                                             | {“wind_speed_10m”: “v2”}                          |        | {“wind_speed_10m”: “EAS2018”, “wind_speed_100m”:”RENEWABLENINJA2019”}            |         |
| 6   | Europe | offhore | 1h                  | 2020-01-01 00:00:00+01:00 | 2020-12-31 23:00:00+01:00 | [3,1597.0,1519.64,1471.85,1451.37,1445.4,1456.9,1525.83,1658.16,1863.97,2051.03,2218.37,2280.12,2157.11,2022.07,88,2931.02,2805.22,2754.16,2764.23,2624.13,2596.24,2575.67,2628.21,2587.71,2414.04,2209.3,1986.03,1805.76,1699.03,1651.18,16] |                                                                                                                                                                                                                                             | {“wind_speed_10m”: “v3”}                          |        | {“wind_speed_10m”: “EAS2018”, “wind_speed_100m”:”RENEWABLENINJA2019”}            |         |



### Bandwidth types and cell methods

[cfconventions](https://cfconventions.org/Data/cf-conventions/cf-conventions-1.9/cf-conventions.pdf#page=143) for cell methods are used and extended, for the columns <u>bandwidth_type</u> and <u>method</u>.

| cell_methods           | Description                                                                                                                                                                         | Source      |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|
| point                  | The data values are representative of points in space or time (instantaneous). This is the default method for a quantity that is intensive with respect to the specified dimension. | NetCDF_v1.9 |
| sum                    | The data values are representative of a sum or accumulation over the cell. This is the default method for a quantity that is extensive with respect to the specified dimension.     | NetCDF_v1.9 |
| maximum                | Maximum                                                                                                                                                                             | NetCDF_v1.9 |
| maximum_absolute_value | Maximum absolute value                                                                                                                                                              | NetCDF_v1.9 |
| median                 | Median                                                                                                                                                                              | NetCDF_v1.9 |
| mid_range              | Average of maximum and minimum                                                                                                                                                      | NetCDF_v1.9 |
| minimum                | Minimum                                                                                                                                                                             | NetCDF_v1.9 |
| minimum_absolute_value | Minimum absolute value                                                                                                                                                              | NetCDF_v1.9 |
| mean                   | Mean (average value)                                                                                                                                                                | NetCDF_v1.9 |
| mean_absolute_value    | Mean absolute value                                                                                                                                                                 | NetCDF_v1.9 |
| mean_of_upper_decile   | Mean of the upper group of  data values defined by the upper tenth of their distribution                                                                                            | NetCDF_v1.9 |
| mode                   | Mode (most common value)                                                                                                                                                            | NetCDF_v1.9 |
| range                  | Absolute difference between maximum and minimum                                                                                                                                     | NetCDF_v1.9 |
| root_mean_square       | Root mean square (RMS)                                                                                                                                                              | NetCDF_v1.9 |
| standard_deviation     | Standard deviation                                                                                                                                                                  | NetCDF_v1.9 |
| sum_of_squares         | Sum of squares                                                                                                                                                                      | NetCDF_v1.9 |
| variance               | Variance                                                                                                                                                                            | NetCDF_v1.9 |
| values                 | The bandwidth data should be interpreted as list of single values                                                                                                                   | SEDOS       |



### OEDataModel-concrete/normalization

The following examples are intended to provide a simple example table as well as a detailed descriptoion on each field/column. For completeness we also link to the datapacke examples which are already provided as file. 

**Since we offer two data model variants with almost identical field names, we provide a description that applies to both variants.**

Origin data model: [OEDataModel-concrete](https://github.com/OpenEnergyPlatform/oedatamodel/blob/develop/oedatamodel/latest/v100/OEDataModel-concrete.pdf)

### Scenario description

| **Field**         | **Datatype** | **Description**                                                                                                                                                                                               |
|-------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   scenario id     | integer      | A primary key is a field or set of fields that uniquely identifies each row in the table. It's recorded as a list of strings, since it is possible to define the primary key as made up of several columns.   |
|   scenario        | text         | Name of the scenario.                                                                                                                                                                                         |
|   region          | json         | It describes the geographical scope of the dataset.                                                                                                                                                           |
|   year            | integer      | It describes the time frame of the dataset.                                                                                                                                                                   |
|   source          | text         | Human readable title of the source, e.g. document title or organisation name. The source must relate to a source provided in the oemetadata (datapackage) file.                                               |
|   comment         | text         | Free text comment on what's been done.                                                                                                                                                                        |

### Example table:

| **scenario id** (PK) | **scenario** | **region**  | **year**  | **source** | **comment** |
|----------------------|--------------|-------------|-----------|------------|-------------|
| 1                    | ToDo         | ['BB']      | 2020      | path       | ToDo        |
| 2                    | ToDo         | ['DE']      | ToDo      | ToDo       | ToDo        |
| 3                    | ToDo         | ['North']   | ToDo      | ToDo       | ToDo        |
| ...                  | ...          | ...         | ...       | ...        | ...         |


### Scalar description

Todo's: 
* add bandwidth, version columns and explanations
* make value column of type: float array

| **Field**              |  **Datatype** | **Description**            |
|------------------------|---------------|----------------------------|
|   scalar id            |      integer      | A primary key is a field or set of fields that uniquely identifies each row in the table. It's recorded as a list of strings, since it is possible to define the primary key as made up of several columns.                           |
|   scenario id          |      integer      | A foreign key is a field that refers to a primary key column in another table.                           |
|   region               |      json     | It describes the area name in which a scalar operates.                           |
|   input energy vector  |      text     | It describes any type of energy or energy carrier (e.g. electricity, heat, solar radiation, natural gas, ...) that enters a technology.                           |
|   output energy vector |      text     | It describes any type of energy or energy carrier (e.g. electricity, heat, hydrogen, LNG, CO2, ...) that exits a technology.                           |
|   parameter name       |      text     | It describes a considered property of an element in the energy system. It can be technology-related or technology-independent. It can refer to technological, economic or environmental characteristics.                           |
|   technology           |      text     | It describes an element of the modelled energy system that processes an energy vector. A technology can be real (e.g. specific type of power plant) as well as abstracted as an aggregation of energy processes or a virtual process.                           |
|   technology type      |      text     | Is used to specify the technology field. The specification can be technological, or freely user-defined, based on the requirements of the model.                           |
|   value                |      decimal  | Indicates the numerical value of a scalar.                           |
|   unit                 |      text     | Indicates the measuring unit of a value.                           |
|   tags                 |      json     | Is used to further describe a scalar.                           |
|   method               |      json     | It describes the procedure for obtaining the value, in case it does not originate from a single source.                           |
|   source               |      text     | Human readable title of the source, e.g. document title or organisation name. The source must relate to a source provided in the oemetadata (datapackage) file.                           |
|   comment              |      text     | Free text comment on what's been done.                           |

### Example table:

| **scalar id** (PK) | **scenario id** (FK) | **region** | **input energy vector**   | **output energy vector** | **parameter name** | **technology** | **technology type** |**value** | **unit** | **tags** | **method** | **source** | **comment** |
|-----------|--------------|------------|----------------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|
| 1   | 1 | ['DE']    | solar radiation | electricity     |    variable costs |    photovoltaics |      utility |      0.00 |      €/MWh |      Assumption |      ToDo |      ToDo |      ToDo |
| 2   | 1 | ['DE']      | lignite | co2     |    output ratio |    generator |      ToDo |      0.40 |      t/MWh |      ToDo |      ToDo |      ToDo |      ToDO |
| 3   | 2 | ['DE']      | heavyoil | co2     |    output ratio |    generator |      ToDo |      0.29 |      t/MWh |      ToDo |      ToDo |      ToDo |      ToDo |
| ...       | ...          | ...        | ...            | ...      |      ... |      ... |      ... |      ... |      ... |      ... |      ... |      ... |      ... |


### Timeseries description

| **Field**              |  **Datatype** | **Description**            |
|------------------------|---------------|----------------------------|
|   timeseries id        |     integer       | A primary key is a field or set of fields that uniquely identifies each row in the table. It's recorded as a list of strings, since it is possible to define the primary key as made up of several columns.                           |
|   scenario id          |     integer       | A foreign key is a field that refers to a primary key column in another table.                           |
|   region               |     json      | It describes the area name in which a timeseries operates.                           |
|   input energy vector  |     text      | It describes any type of energy or energy carrier (e.g. electricity, heat, solar radiation, natural gas, ...) that enters a technology.                           |
|   output energy vector |     text      | It describes any type of energy or energy carrier (e.g. electricity, heat, hydrogen, LNG, CO2, ...) that exits a technology.                           |
|   parameter name       |     text      | It describes a considered property of an element in the energy system. It can be technology-related or technology-independent. It can refer to technological, economic or environmental characteristics.                           |
|   technology           |     text      | It describes an element of the modelled energy system that processes an energy vector. A technology can be real (e.g. specific type of power plant) as well as abstracted as an aggregation of energy processes or a virtual process.                           |
|   technology type      |     text      | Is used to specify the technology field. The specification can be technological, or freely user-defined, based on the requirements of the model.                           |
|   timeindex start      |     [timestamp](https://www.postgresql.org/docs/9.5/datatype-datetime.html)          | Both date and time, with time zone.                           |
|   timeindex stop       |     timestamp | Both date and time, with time zone.                           |
|   timeindex resolution |     [interval](https://www.postgresql.org/docs/9.5/datatype-datetime.html)          | The time span between individual points of information in a time series.                           |
|   series               |     [decimal] | Series of values, from start to stop with a step size of stepvalues.                           |
|   unit                 |     text      | Indicates the measuring unit of a value.                           |
|   tags                 |     json      | Is used to further describe a timeseries.                           |
|   method               |     json      | It describes the procedure for obtaining the value, in case it does not originate from a single source.                           |
|   source               |     text      | Human readable title of the source, e.g. document title or organisation name. The source must relate to a source provided in the oemetadata (datapackage) file.                           |
|   comment              |     text      | Free text comment on what's been done.                           |

### Example table:

| **timeseries id** (PK) | **scenario id** (FK) | **region** | **input energy vector**   | **output energy vector** | **parameter name** | **technology** | **technology type** |**timeindex start** | **timeindex stop** | **timeindex resolution** | **series** | **unit** | **tags** | **method** | **source** | **comment** |
|-----------|--------------|------------|----------------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|----------|
| 1   | 1 | ['DE']      | solar radiation     |    electricity |    capacity factor |      photovoltaics |      rooftop |      2016-09-30 16:00:00+01:00 |      2016-09-30 17:00:00+01:00 |      1 |      [0.014; 0] |      ToDo |      ToDo |      NUTS 2 aggregated to NUTS 1 and weighted per area |      ToDo |      ToDo |
| 2   | 1 | ['BB']      | air     |    electricity |    capacity factor |      wind turbine |      onshore |      2016-02-07 08:00:00+01:00 |      2016-02-07 09:00:00+01:00 |      1 |      [0.21546274939004; 0.140089694955441] |      ToDo |      ToDo |      NUTS 2 aggregated to NUTS 1 and weighted per area |      ToDo |      ToDo |
| 3   | 2 | TODO      | TODO     |    TODO |    TODO |      ... |      ... |      ... |      ... |      ... |      ... |      ... |      ... |      ... |      ... |      ... |
| ...       | ...          | ...        | ...            | ...      |      ... |      ... |      ... |      ... |      ... |      ... |      ... |      ... |      ... |      ... |      ... |      ... |


- Example  Datapackage

    - Example JSON: [OEDataModel-concrete-datapackage](https://github.com/OpenEnergyPlatform/oedatamodel/blob/develop/oedatamodel/latest/v100/datapackage/OEDataModel-concrete-datapackage/OEDataModel-concrete-datapackage.json)

    - Example CSV: 
        - [concrete-datapackage_scalar](https://github.com/OpenEnergyPlatform/oedatamodel/blob/develop/oedatamodel/latest/v100/datapackage/OEDataModel-concrete-datapackage/OEDataModel-concrete-datapackage_scalar.csv)
        - [OEDataModel-concrete-datapackage_scenario](https://github.com/OpenEnergyPlatform/oedatamodel/blob/develop/oedatamodel/latest/v100/datapackage/OEDataModel-concrete-datapackage/OEDataModel-concrete-datapackage_scenario.csv)
        - [OEDataModel-concrete-datapackage_timeseries](https://github.com/OpenEnergyPlatform/oedatamodel/blob/develop/oedatamodel/latest/v100/datapackage/OEDataModel-concrete-datapackage/OEDataModel-concrete-datapackage_timeseries.csv)

# Edit the Entity Relationship Modell

For the generation of an ERM we use this [erm tool](https://github.com/BurntSushi/erd). The [er or erd](https://github.com/BurntSushi/erd#the-er-file-format) file format offers a simple syntax and 
can be created and saved using a standard text editor.  

For the generation of the ERM e.g. in .pdf format the installation of the erm tool is necessary. For 
detailed instructions, please see the [package description](https://github.com/BurntSushi/erd#installation).   

After successful installation, a terminal/CMD must be opened and the console command (Windows: 'cd path')
must be used to navigate to the folder where the .er/.erd file is stored. To execute the tools, the command 
is then used in the Terminal/CMD to generate the ERM: 

`erd -i oedatamodel.er -o oedatamodel.pdf`

