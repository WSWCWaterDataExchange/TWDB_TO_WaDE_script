# Map the TWDB headers to WaDE excel headers

ORGANIZATION_ID  
REPORT_ID  
REPORT_UNIT_ID  
BENEFICIAL_USE_ID  
SUMMARY_SEQ  
ROW_SEQ  
AMOUNT  
CONSUMPTIVE_INDICATOR  
METHOD_ID  
START_DATE  
END_DATE



### First add some code to check in the type of the input file 

If the excel file name contains "Basin" then AreaType="Basin"

elif the excel file name contains "County" then AreaType="County"

else print 'The file name is not supported'

We will use this "AreaType" below to look up the identifier of the basin or county inside each file


### Read the excel file County_Basin_IDs.xlsx and its two sheets: County_FIPS, BasinTCEQID
### so basically I want to print a new column to the result sheet for the identifier of each "name" in the input data
In the County_FIPS sheet, store the two columns "COUNTYNAME" and "COUNTYFP" together which will use them in a look up function

The look up works like this:
We have a column called County in the input excel files, for each row, we need to look up the COUNTYFP for it from the above pair.
Where 
County=COUNTYNAME  
Get COUNTYFP

In the BasinTCEQID sheet, store the two columns "BasinName" and "BasinID" together which will use them in a look up function

The look up works like this:
We have a column called "Basin" in the input excel files, for each row, we need to look up the Name for it from the above pair.
where Name=BasinName  
Get BasinID


### add a new column called "ReportingUnitID" which takes the value of the  COUNTYFP or the BasinID
## Add a new sheet to the same exported file
call it: SUMMARY_USE

first print these headers to it

ORGANIZATION_ID,REPORT_ID,REPORTING_UNIT_ID,BENEFICIAL_USE_ID,SUMMARY_SEQ,FRESH_SALINE_IND,SOURCE_TYPE,POWER_GENERATED,POPULATION_SERVED,WFS_FEATURE_REF

ORGANIZATION_ID='TWDB'  
REPORT_ID=fill in the year value  
REPORTING_UNIT_ID=fill in the County or Basin ID value  
BENEFICIAL_USE_ID='6' # we need to look it up later  # yes it would take the value from LU_SEQ_NO

SUMMARY_SEQ='1'  
FRESH_SALINE_IND='1'  
SOURCE_TYPE='1'  
POWER_GENERATED=''  

if attribute contains "Municipal" then  
POPULATION_SERVED=Population   

else  
POPULATION_SERVED=""  

WFS_FEATURE_REF=""  


## the number of rows in the oputput in this sheet are the same like the input file (23) or 400 rows depending on the source file.
# Add a new sheet called REPORTING_UNIT  

ORGANIZATION_ID,REPORT_ID,REPORTING_UNIT_ID,REPORTING_UNIT_NAME,REPORTING_UNIT_TYPE,STATE,COUNTY_FIPS,HUC   


WHERE   

ORGANIZATION_ID='TWDB'     
REPORT_ID=fill in the year value      
REPORTING_UNIT_ID=fill in the County or Basin ID value      
REPORTING_UNIT_NAME= fill in the name  
REPORTING_UNIT_TYPE=fill in the ReportUnitType   

STATE='55'  

if REPORTING_UNIT_TYPE=="County"  
Then COUNTY_FIPS= REPORTING_UNIT_ID  
else COUNTY_FIPS= ""

HUC=""  



## the # of rows here is similair to the source file (23 or 400) depending on the file type  
## Add a new sheet called REPORT  
add the headers to it
ORGANIZATION_ID,REPORT_ID,REPORTING_DATE,REPORTING_YEAR,REPORT_NAME,REPORT_LINK,YEAR_TYPE


WHERE   

ORGANIZATION_ID='TWDB'       
REPORT_ID=fill in the year value from the folder name e.g., "2015"    
REPORTING_DATE= get the date of today like this format 8/15/2018  

REPORT_NAME= get the excel file name  

REPORT_LINK="http://www.twdb.texas.gov/waterplanning/waterusesurvey/estimates/index.asp"
 

YEAR_TYPE="Calendar year"


## the # of rows here is like the number of excel files (each excel file has one row stored)
# Read the excel file County_Basin_IDs.xlsx 

Get the new sheet called: LU_BENEFICIAL_USE  

In the LU_BENEFICIAL_USE sheet, store the two columns "Attribute" and "LU_SEQ_NO"  
which will use them in a look up function  

The look up works like this:  
We have a column called Attribute in the input excel files, for each row, we need to look up the "LU_SEQ_NO" for it from the above pair.   

Where    
Attribute==Attribute    
Get LU_SEQ_NO   

then set the BENEFICIAL_USE_ID=LU_SEQ_NO  

then use this value in S_USE_AMOUNT table  


## Update these columns to the S_USE_AMOUNT sheet 

ORGANIZATION_ID,REPORT_ID,REPORTING_UNIT_ID,BENEFICIAL_USE_ID,SUMMARY_SEQ,ROW_SEQ,AMOUNT,CONSUMPTIVE_INDICATOR,METHOD_ID,START_DATE,END_DATE

WHERE 

ORGANIZATION_ID='TWDB'    
REPORT_ID=fill in the year value    
REPORTING_UNIT_ID=fill in the County or Basin ID value    

BENEFICIAL_USE_ID= use the look up SEQ_NO value   
SUMMARY_SEQ='1'    
ROW_SEQ='1'  
AMOUNT=Value

CONSUMPTIVE_INDICATOR="2"  
METHOD_ID="1"  
START_DATE="01/01"  
END_DATE="12/31"  


# add another sheet called "S_USE_IRRIGATION"

first print these headers to the sheet  

ORGANIZATION_ID,REPORT_ID,REPORTING_UNIT_ID,BENEFICIAL_USE_ID,SUMMARY_SEQ,IRRIGATION_SEQ, IRRIGATION_METHOD, ACRES_IRRIGATED,CROP_TYPE  

this sheet uses the same data for the first five columns like the first sheet called "S_USE_AMOUNT" only if the next condition is met  
 
if attribute contains "Irrigation" then  

ORGANIZATION_ID='TWDB'  
REPORT_ID=fill in the year value  
REPORTING_UNIT_ID=fill in the County or Basin ID value  
BENEFICIAL_USE_ID= use the look it SEQ_NO
SUMMARY_SEQ='1'  


IRRIGATION_SEQ='1'  
IRRIGATION_METHOD=""  
ACRES_IRRIGATED=''  
CROP_TYPE=""  
