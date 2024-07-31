title: Case Studies - GHFP Research Institute

#GHFP Research Institute

## The Client

[GHFP Research Institute](https://ghfp.org/)

## The Problem

In collaboration with [The Centre for Trust, Peace and International Relations](https://pureportal.coventry.ac.uk/en/organisations/centre-for-trust-peace-and-social-relations-2) at the University of Coventry, the GHFP Research Institute had developed *the Better Place Index* a metric of quality of life in different countries. They wished to create an interactive map which would allow users to explore how this metric and its key contributing factors varied from country to country.

## The Approach

Geopandas was used to combine the [Natural Earth Countries Shapefile](https://www.naturalearthdata.com/downloads/50m-cultural-vectors/50m-admin-0-countries-2/) with a spreadsheet of the Better Place Index and its contributing factors. The resulting GeoDataFrame was then used in CartoFrames to produce an [interactive map of the Better Place Index](https://www.thebetterplaceindex.report/map) on which

* Countries are coloured according to the Better Place Index
* Hovering the mouse over a country displays the Better Place Index for that country, and its best and worst contributing factors
* Countries may be selected by ranges of the Better Place Index, or by the best or worst contributing factor.

## Technology Used

* [Geopandas](https://geopandas.org/)
* [CartoFrames](https://carto.com/)

by [Dr Peter Bleackley]({% link index.md %})

[Case Studies]({% link Portfolio/index.md %})
