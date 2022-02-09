# What is the Time Series Explorer's intent and purpose?
This application creates standard time series graphs from compatible data sets
on a CKAN [data catalogue](http://data-demo.dpaw.wa.gov.au/).

Having a web app to access, analyse and visualise data has some benefits:

* it does one job (with a few user-supplied degrees of freedom) properly and 
according to some standards,
* it saves data analysts some leg work (by automating the boring bits) and
frees up their minds to think about the data (and not about R code),
* it provides graphs in a certain format and quality,
* it drives the clean-up and standardisation of data in the CKAN data catalogue
(because it requires data to be in a certain format to work), and
* it makes reporting transparent and reproducible by saving the work flow as R code.

# How can I use it?

* Choose a dataset, choose the CSV resource, and inspect / preview the data. You 
can delete the selected dataset and type a region or indicator to filter the list.
* Pick a numeric variable for the y axis, and a date variable for the x axis. The 
first date and numeric variables are pre-selected by default.
* View the graph in the "plot" tab, modify graphical parameters in the side panel 
as required.
* Download the graph as PDF, plus the R code to reproduce the graph as text.
* To save the data back to CKAN, paste your CKAN API key to unlock the upload of 
the PDF graph and R code.

How to get the API key: Logged in CKAN users can find their API key on their 
profile page (click on your user name). Contact the author of this application 
to get an account on the configured CKAN.

# How can I use the Time Series Explorer on my own data?

This application will only work with univariate (and optionally grouped) time series data.
Your data need to be on the [CKAN data catalogue](http://data-demo.dpaw.wa.gov.au/), 
conforming to a particular standard:

* The CKAN dataset must carry the tag `format_csv_ts` in order to show up in `Choose dataset`.
* The dataset title should include the asset (what the data is about, e.g. 
"Seagrass cover", "Finfish abundance", "Boat registrations") and the area with full name and 
abbreviated park designator ("Shark Bay MPA"). Backspace and type into the select box to filter!
* The data must be attached to the dataset as CSV resource in the format described below.
* If you want to upload figure and code directly, existing resources (one PDF, 
one TXT) to overwrite must already exist on that dataset.
* Data, figure and code should be the first CSV, PDF or TXT resources on the dataset. 
Ideally, Manage > resources > re-order resources so CSV, PDF and TXT are the first three resources.
* You must be authorised to update the data catalogue if you want to upload figure and script directly.

# What's the required format and structure for the CSV file?
For univariate time series, the CSV **must** have:

* one column "datetime", "Datetime" with any optinoal separator between date and 
time, containing an unquoted string of an ISO8601 date and time with or without timestamp 
(`2014-12-31T23:56:59+08:00` or `2014-12-31T23:56:59` assuming GMT+08), or 
* one column "date" or "Date" with an unquoted string of an ISO8601 date 
(preferred `2014-12-31` over `31-12-2014`),
* one column with the univariate dependent variable labelled alphanumerically (preferred "value")

Example:
```
date, value
2014-01-01, 12.234
2014-01-02, 13.2342
2014-01-03, 12.63
```

For grouped univariate time series, an additional column for the grouping factor 
must be given as quoted string.
The grouping variable name will not be used in the figure and can be simple.
Example:

```
date, value, group
2014-01-01, 14.1231, "Site A"
2014-01-01, 12.234, "Site B"
2014-01-02, 12.234, "Site A"
2014-01-02, 18.2223, "Site B"
2014-01-03, 12.62343, "Site A"
2014-01-03, 13.6323, "Site B"
```

Additionally, desired columns are:

* "latitude", decimal degrees of unprojected WGS84 latitude
* "longitude", decimal degrees of unprojected WGS84 longitude
* "altitude", meters of altitude over mean sea level (negative values for depth)

Any other columns can be included in the CSV as desired. 
Any strings can be used as factors to subset or group data by.
Any dates can be used for the x axis. 
Any numeric values can be plotted as y values. 
Multiple y data series are not supported - reshape the data into one value and 
one factor column instead.
