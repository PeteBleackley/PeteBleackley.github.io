---
layout: default
title: Case Studies -  Amey Strategic Consulting
---

# Amey Strategic Consulting


## The Client

[Amey Strategic Consulting](https://www.amey.co.uk/amey-consulting/services/strategic-consulting/)

## The Problem

As part of a major data science project on behalf of [Highways England](https://highwaysengland.co.uk/), Amey wished to create an automatic diagnostic system that would detect faults in traffic flow sensors on the strategic road network. As well as enabling timely and efficient maintenance, this would prevent delays to journeys caused by incorrectly set signals, which are estimated to cost the economy £7.5 million per year.

## The Approach

From a shapefile containing the geometry of the Strategic Road Network, the topology of the network was calculated and groups of sensors assigned to links, which are sections of carriageway between two junctions. Over a link, traffic flow readings should be approximately consistent at a given time. Anomaly detection can then be used to find the sensor whose readings are most different from the rest. This should vary randomly, but if the same sensor is inconsistent with the rest for a few minutes at a time, it can be assumed to be faulty.

After testing this approach on one link, a simple dashboard was created to demonstrate the results and work began on scaling to the full network.

## Technology Used

* [Geopandas](https://geopandas.org/)
* [Scikit-learn](https://scikit-learn.org/stable/) (Isolation Forests)
* [Jupyter Lab](https://jupyter.org/)
* [PySpark](https://spark.apache.org/docs/latest/api/python/)

by [Dr Peter Bleackley]({% link index.md %})

[Case Studies]({% link Portfolio/index.md %})
