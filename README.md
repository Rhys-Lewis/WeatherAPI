# WeatherAPI Reporting

## Overview
This sample was put together to demonstrate the flow from raw data to a usable product. In this example Microsoft's suite of data and analysis tools are utilised to extract, transform, load, model and present externally sourced data.

## Environment
The project was put together in my home lab consisting of:

 - A development PC on which Visual Studio 2019 was loaded with the required plugins for SS(IAR)S development.
 - A secondary PC acting as the 'remote host' for the various server instances and databases.
 - A virtual machine running as the domain controller and DNS to link the network.

### Environment diagram
![image](https://user-images.githubusercontent.com/101239122/224204403-f4a553bb-9ccc-43f3-b1a2-b42b9a6bf80c.png)

## Data storage (SQL Server 2017)
WeatherAPI.com provides an API with endpoints for current and future weather data. In this example, the API called for the latest weather information for Darwin. This is done via a job scheduled every 15 minutes on the remote host which is running SQL Server 17.

The weather data is then divided across one Fact and three Dimension tables, resulting in a database normalised to the third form. 

### Entity Relationship Diagram
![image](https://user-images.githubusercontent.com/101239122/224204099-d4e05c5f-89f6-41b6-92ff-871c6e0efeeb.png)

## ETL (SSIS)
The ETL process is done using an SSIS package. The package makes a call to the API, parses the returned JSON and saves the output to the database tables. Data destined to the fact table is run through lookup steps to replace WeatherCondition and Location values with the appropriate foreign keys. If an entry in the WeatherCondition and Location tables cannot be found then one is made.

### Control and Data flow
![image](https://user-images.githubusercontent.com/101239122/224196503-69d28711-65bd-4d5b-af5f-6ed0533e396a.png)

#### Getting the latest data
![image](https://user-images.githubusercontent.com/101239122/224196565-81fdee81-2012-48a4-b768-02d6cc08dcf3.png)

#### Saving the data to the database
![image](https://user-images.githubusercontent.com/101239122/224196794-e3d49a31-c09e-4dd8-848c-cbdfc5299af1.png)

## Data modeling (SSAS)
The weather data is taken from the databases and put through a SSAS tabular model for consumption by Power BI. The tabular model is run in memory due to the relatively small amount of data. 

## Presentation (SSRS + Power BI)
Power BI Report Server provides housing for reports, datasets and KPIs.
![image](https://user-images.githubusercontent.com/101239122/224196414-3d30d98b-45bb-4ee6-86b5-b8a0d3183133.png)

#### KPI
The KPI feature is used to show the latest weather temperature without having to open and wait for additional reports.

#### PowerBI report
The WeatherObsevation report contains additional information about the current weather and displays historical weather observation data.
![image](https://user-images.githubusercontent.com/101239122/224196635-2fe941e2-50b2-4d1b-bdc4-ab3ec64c5ba8.png)

#### Paginated report
The WeatherAPI Raw Data report provides a way to extrac the original list of values collected from the API.
