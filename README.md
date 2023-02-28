OpenGeoHub 2023 DataChallenge
================

## Abstract

The tick prevention app “tick” is a mobile application that provides
information on tick prevention and tick bite risk. The user is able to
enter tick bites in a diary and is reminded to check for possible
disease symptoms. Additionally, the user can enter the location of where
the tick bite took place on a map, thus helping to identify tick
hotspots. More information can be found
[here](https://www.zhaw.ch/en/lsfm/business-services/natural-resource-sciences/ticks/tick-app/?mdrv=www.zhaw.ch&cHash=a51a3a438004a34df53e9a544e4b070b).

In this manner, over 50k tick bites were collected over a period of
several years. This data can be used to identify tick hotspots and to
predict tick bite risk based on environmental factors. We have a list of
variables that experts think are important for predicting tick bite
risk. We want to use machine learning to find out which variables are
actually important and to predict tick bite risk for a given location.
The goal is to create model which outperforms [the existing expert based
model](https://map.geo.admin.ch/?lang=en&topic=ech&bgLayer=ch.swisstopo.pixelkarte-farbe&layers=ch.bag.zeckenstichmodell&E=2673880.30&N=1214206.44&zoom=8&layers_opacity=0.75).

## The App

The tick-app consists of a warning and an information section, showing
the correct procedure to follow after a tick bite. The warning function
displays the current tick-risk in an area. The dynamic warnings uses the
five-stage tick bite risk scale to indicate the tick risk during outdoor
activities. The information section shows you how to protect yourself
from ticks.

An entry in the tick diary allows the app to automatically remind the
user to check the location on their body of the tick bite for possible
disease symptoms. If there is suspicion of a tick-borne disease, users
are recommended to seek a doctor’s advice.

## Dependent variable: Tick data

### Temporal dimension

Every year, the number of users using the app continues to grow and thus
the number of recorded tick bites grows rapidly. Here are some figures
showing the data from 2015 to 2019. More recent data can be aquired
easily.

<!-- ![](images/unnamed-chunk-37-1.png){#fig-yearly} -->

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td style="text-align: center;"><div width="50.0%"
data-layout-align="center">
<figure>
<img src="images/unnamed-chunk-38-1.png" id="fig-yearly2"
data-ref-parent="fig-temporal" data-fig.extended="false" alt="(a)" />
<figcaption aria-hidden="true">(a)</figcaption>
</figure>
</div></td>
<td style="text-align: center;"><div width="50.0%"
data-layout-align="center">
<figure>
<img src="images/unnamed-chunk-39-1.png" id="fig-timeofday"
data-ref-parent="fig-temporal" data-fig.extended="false" alt="(b)" />
<figcaption aria-hidden="true">(b)</figcaption>
</figure>
</div></td>
</tr>
</tbody>
</table>

Figure 1: The first tick bites were recorded in early 2015 and the
latest beginning of 2020, more recent data is available. Most Tick bites
are recorded May to July, with the peak being in June. Most tick bites
are recorded between 20h and 22h. This is probably the instance when
tick bites are noticed (e.g. in the shower).

</div>

### Spatial accuracy

When specifying the location where the tick bite occured, the user has
the possibility to zoom into the map very closely (see [Figure 2
(a)](#fig-zoom)). The higher the zoom level the user chooses, the
smaller the radius of the red circle. This radius can be used as a proxy
of the accuracy of the users knowledge of where the tick bite occured.
The distribution of the accuracies are approximately log normally
distributed, see [Figure 2 (b)](#fig-accuracy). Different versions of
the app have different default values for accuracy. These default values
are the peaks visible in [Figure 2 (b)](#fig-accuracy).

<div>

<table>
<colgroup>
<col style="width: 30%" />
<col style="width: 70%" />
</colgroup>
<tbody>
<tr class="odd">
<td style="text-align: center;"><div width="30.0%"
data-layout-align="center">
<figure>
<img src="images/spatial_accuracy.jpg" id="fig-zoom"
data-ref-parent="fig-spat" data-fig.extended="false" alt="(a)" />
<figcaption aria-hidden="true">(a)</figcaption>
</figure>
</div></td>
<td style="text-align: center;"><div width="70.0%"
data-layout-align="center">
<figure>
<img src="images/unnamed-chunk-26-1.png" id="fig-accuracy"
data-ref-parent="fig-spat" data-fig.extended="false" alt="(b)" />
<figcaption aria-hidden="true">(b)</figcaption>
</figure>
</div></td>
</tr>
</tbody>
</table>

Figure 2: Accuracy of the tick bite locations can be inferred from the
size of the radius the user provided when recording the tick bite.

</div>

### Risk, hazard and exposure

The risk of getting bitten by a tick can be formalized as follows:

$$\text{Risk} = \text{Hazard} \times \text{Exposure}$$

This means that the risk is higher for areas not only where the *Hazard*
is higher (i.e. more ticks), but also where *Exposure* is higher,
i.e. more people. In other words, we must correct our data for
population before making any predictions about the occurance of ticks,
as [Figure 3](#fig-spatial) shows. If you are not familiar with swiss
geography, the hotspots of tick bites are clearly around major cities
like Zurich, Bern etc.

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td style="text-align: center;"><div width="50.0%"
data-layout-align="center">
<figure>
<img src="images/unnamed-chunk-14-1.png" id="fig-spatial-raw"
data-ref-parent="fig-spatial" data-fig.extended="false"
alt="(a) Raw data points" />
<figcaption aria-hidden="true">(a) Raw data points</figcaption>
</figure>
</div></td>
<td style="text-align: center;"><div width="50.0%"
data-layout-align="center">
<figure>
<img src="images/unnamed-chunk-15-2.png" id="fig-spatial-kde"
data-ref-parent="fig-spatial" data-fig.extended="false"
alt="(b) Point density (5km kernel filter)" />
<figcaption aria-hidden="true">(b) Point density (5km kernel
filter)</figcaption>
</figure>
</div></td>
</tr>
</tbody>
</table>

Figure 3: After filtering, the dataset consists of \> 30k tick bite
recordings. Naturally, we have most recordings around big cities

</div>

## Predictors: Geodata

Based on expert knowledge, we have selected and prepared a number of
possible datasets that can be used to predict tick occurance.

- [Land use
  statistics](https://map.geo.admin.ch/?lang=en&topic=ech&bgLayer=ch.swisstopo.pixelkarte-farbe&layers=ch.bfs.arealstatistik&layers_opacity=0.75&layers_timestamp=2018):
  A raster dataset with a 100m resolution with 72 basic categories
- [Population](https://map.geo.admin.ch/?lang=en&topic=ech&bgLayer=ch.swisstopo.pixelkarte-farbe&layers=ch.bfs.volkszaehlung-bevoelkerungsstatistik_einwohner&layers_timestamp=2021&E=2677260.22&N=1214005.67&zoom=10):
  A raster dataset with 100m resolution with population count
- [Vegetation
  Height](https://map.geo.admin.ch/?lang=en&topic=ech&bgLayer=ch.swisstopo.pixelkarte-farbe&layers=ch.bafu.landesforstinventar-vegetationshoehenmodell&E=2673314.45&N=1210711.46&zoom=5&layers_opacity=0.5):
  A raster dataset with 50cm resolution describing the height of the
  vegetation in meters.
- [Digital Elevation
  Model](https://map.geo.admin.ch/?lang=en&topic=ech&bgLayer=ch.swisstopo.pixelkarte-farbe&layers=ch.swisstopo.swissalti3d-reliefschattierung_monodirektional&E=2674016.86&N=1214686.09&zoom=11):
  A raster dataset with 50cm resolution describing elevation
- Temperature: Historic temperature values
- Percipitation: Historic percipitation values
