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
1. **Local Processing or Third Party**  Geocoding can be run on your local computer.  To do this, you typically need geocoding software and to download road network data with address ranges included.  Third party tools don't require you to download road network data because they send your addresses to a cloud service for processing. Addresses by their nature identify the location of people so we should be careful of sending non-public addresses to a third party.  Online geocoders are all third party products.  **Personally Identifiable Information (PII) or HIPAA Data should never be sent to third party geocoder for processing unless that tool is certified to be HIPAA-compliant.**

## Some Software Options

### Local Geocoders

| Software | Region | Cost | Tutorial | Notes |
|---|---|---|---|---|
|PostGIS Tiger Geocoder | US | Free | [Building a PostGIS Geocoder](https://experimentalcraft.wordpress.com/2017/11/01/how-to-make-a-postgis-tiger-geocoder-in-less-than-5-days/) | Uses US Census data |
|PostGIS with Open Addresses |
|Pelias Geocoder Installed Locally |

### Third Party Geocoders

| Software | Region | Cost | Tutorial | Notes |
|---|---|---|---|---|
| [Geocodio](https://www.geocod.io) | US | 2,500 free lookups per day; $0.50 per 1,000 after that | |  |
| [Geocodio+HIPAA](https://www.geocod.io/healthcare/) | US | Monthly or Yearly Subscription | | HIPAA-compliant |
| QGIS Plugins | | | | |
| ArcGIS Geocoder | | Software + Extensions + Credits | | Using any ESRI web service costs credits. Geocoding is particularly credit-intensive so UC Davis affiliates should consult with esrisupport@ucdavis.edu before using this service |


# Organizing Your Data
