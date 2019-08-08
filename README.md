# Geocoding Workshop

# The Big Concepts

## What is Geocoding?

Geocoding is the process of translating postal addresses into geographic coordinates.

## The Road Network
We do the process of geocoding in our brains every time we go to a new address.  Let's imaging your friend moved to a new house and you're on your way to their house warming party.  Their address is 150 Main Street.  If you're familiar with the town and you're not using a turn-by-turn direction program, you'd probably go to Main Street and start looking at addresses.  Once you found the 100 block (the addresses are something like 110, 125, etc.), you would probably guess that house number 150 is about in the middle of the block and on the side of the street with the even numbered houses.

Why does this work? Because most cities have an organized system of addresses.  Postal codes are typically made up of a string of increasingly broad information - Building Number, Street, City, State, Postal Code, Country - for example the City of Davis' city hall address is "23 Russell Boulevard, Davis, CA 95616, USA" and the city hall in Sydney, Australia, is "483 George St, Sydney NSW 2000, Australia".

Buildings are arranged in numerical order along a street and each block typically starts a new number at the front of the building number.  So all the 100 addresses are on one block, the 200s are on the next block, etc.  Further, usually the odd numbers are on one side of the street and the even numbers are on the other.

# Software
There are many tools to help you geocode addresses.  A typical geocoding tool consists of software that compares your addresses to the road network or known address locations.  You may need to provide the road network data, depending on the software you choose.


There are some important things to consider when choosing a tool:

1. **Budget** How much will it cost to use the software.  Some tools are free.  Others are free for a certain number of addresses and then charge if you exceed that number. Others have a subscription, yearly, or one-time fee.
1. **Data Required** Does the software require you to provide a road network or does the software come with its own network? If you need to provide one, is the data available for the part of the world you are working in?
1. **Geographic Restrictions** Different tools specialize in different regions of the world.  For example, Geocodio's online tool is excellent for the US, but doesn't have the data necessary for geocoding for international addresses.
1. **Platform** What platform does the tool work on? It could be implemented in R, Python, or SQL. Maybe it runs inside a graphical tool like QGIS. Maybe you have to write a script to work with an API. Consider what skills you have or want to learn and if the tool is a good fit for your situation.
1. **Local Processing or Third Party**  Geocoding can be run on your local computer.  To do this, you typically need geocoding software and to download road network data with address ranges included.  Third party tools don't require you to download road network data because they send your addresses to a cloud service for processing. Addresses by their nature identify the location of people so we should be careful of sending non-public addresses to a third party.  Online geocoders are all third party products.  **Personally Identifiable Information (PII) or HIPAA Data should never be sent to third party geocoder for processing unless that tool is certified to be HIPAA-compliant.**

## Some Software Options

There are many, many options for geocoder tools.  I'll list the ones I'm familiar with here, but this is not an exhaustive list.

### Local Geocoders

| Software | Region | Cost | Tutorial | Notes |
|---|---|---|---|---|
|[MMQGIS Plugin for QGIS - Geocode from Street Layer tool](http://michaelminn.com/linux/mmqgis/) | Anywhere | Free & Open Source | Scroll down | You need to download the road network data to use with the tool |
|PostGIS Tiger Geocoder | US | Free & Open Source | [Building a PostGIS Geocoder](https://experimentalcraft.wordpress.com/2017/11/01/how-to-make-a-postgis-tiger-geocoder-in-less-than-5-days/) | Uses US Census data |
|PostGIS with OpenAddresses | Dependent on OpenAdresses data coverage | Free & Open Source | [Loading OpenAddresses Data into PostGIS](https://experimentalcraft.wordpress.com/2018/10/19/loading-open-addresses-data-into-postgis/) | |
|[Pelias Geocoder](https://github.com/pelias/pelias) Installed Locally |

### Third Party Geocoders

| Software | Region | Cost | Tutorial | Notes |
|---|---|---|---|---|
| [Geocodio](https://www.geocod.io) | US | 2,500 free lookups per day; $0.50 per 1,000 after that | |  |
| [Geocodio+HIPAA](https://www.geocod.io/healthcare/) | US | Monthly or Yearly Subscription | | HIPAA-compliant |
| OpenStreetMap Nominatim API | | | | Use the API with your favorite programming tools |
| Nominatim Locator Filter Plugin for QGIS | | | | Uses the OpenStreetMap Nominatim API to process data |
| Pelias Geocoding Plugin for QGIS | | | | Uses the Pelias geocoder for processing data |
| [R package tmaptools's geocode_OSM()](https://cran.r-project.org/web/packages/tmaptools/index.html) | Worldwide | Free & Open Source | | Sends data to the OpenStreetMap Nominatim API |
|[Pelias Geocoder](https://github.com/pelias/pelias)|
| ArcGIS Geocoder | | Software + Extensions + Credits | | Using any ESRI web service costs credits. Geocoding is particularly credit-intensive so UC Davis affiliates should consult with esrisupport@ucdavis.edu before using this service |

# Hands-On Geocoding

We will be working with QGIS 3 (this was written and tested on 3.8, but 3.4 or 3.6 should also work) and using the MMQGIS Plugin to geocode from a street file that we download - so we'll be geocoding locally.

## Get the Software

Download & install [QGIS 3](https://qgis.org) - the download page will have install files for Mac, Windows, & Linux.

## Organizing Your Address Data

The biggest mistake I see when people try to geocode data is trying to use messy data.  Geocoders are very particular about how an address is formatted and what information you include.  Most geocoders require each address to be separated into columns for each piece of information (check your chosen tool's documentation to find out how it specifically needs you to format your data) - so separate columns for Street Address (number + street name), City, State, Postal Code, Country.  

You should **not include** things like 

1. Apartment number, suite, or half addresses.  The geocoder usually doesn't know what to do with these and it will fail.  Remember that the goal is usually to estimate a location along a block face, not to match an exact address (unless you are using OpenAddresses).  "150 Main Street" is probably in the middle of the 100 block.  "150 Main Street suite A" will fail because the geocoder can't find a street called "Main Street suite A".
2. Non-standard street abbreviations. St., Blvd., Dr. are all fine.  Shortening "Paseo" to PSO will fail. (True story - I've seen this one before.)

***The MMQGIS Geocode from Street Layer tool requires that the street address be separated into two columns*** - one for the street number and another for the street name.

### You Try:

This list of restaurants and addresses in downtown Davis came directly from their listing in Google Maps and is pretty typical of geocoding datasets.  You can get your own copy in the [data folder](/data). How would you clean this dataset & prepare it for use with the MMQGIS plugin?

|Restaurant|Street|City|State|Zip|
|---|---|---|---|---|
|Ali Baba|220 3rd St|Davis|CA|95616
|Burgers & Brew|403 3rd St|Davis|CA|95616|
|Davis Noodle City|129 E St #1D|Davis|CA|95616|
|Davis' Pinata Mexican Grill|305 1st St|Davis|CA|95616|
|Three Ladies Cafe|130 G St Suite A|Davis|CA|95616|

## Download the Road Network Data

Because we want to use a local geocoder, we need to download a road network dataset to match our addresses to.  If we were using an online cloud service, the service would provide the road network for us.

Not just any road shapefile will work for a geocoder.  Specifically, it needs to have attributes for the address ranges (from 100 to 199, for example).

A good source of current and recent road data for the US is the US Census TIGER files, specifically the Edges files.  We will download the edges file for California's Yolo County.  The Census' FTP site is organized with FIPS codes indicating the state and county with a numeric code.  We can look up the FIPS codes at the [County FIPS Codes page of the USDA's NRCS](https://www.nrcs.usda.gov/wps/portal/nrcs/detail/ca/home/?cid=nrcs143_013697) among other places and see the **Yolo County's FIPS code is 06113** - 06 at the front tells us it's California and 113 refers to Yolo County.

Now we'll navigate through the Census' FTP site to download the Yolo County Edges file (be patient, it can be slow):

1. Go to the [US Census' FTP Site](ftp://ftp2.census.gov/geo/tiger)
1. Click on the *TIGER2018/* folder - note that you can select the folder for the year you want to work with.
1. Click on the *EDGES/* folder
1. Click on the *tl_2018_06113_edges.zip* file and download it to a folder on your computer (that you have permissions for and can find again).  Note that the *06113* part of the file name tells us we're downloading the edges for Yolo county.
