<header><h1><center>Discovering Manchester </center></h1>

<h4>
<center>
IBM Capstone Project - Exploring the best areas for a young professional to move to in Greater Manchester
</center>
</h4>
</header>

## Table of Contents

* [Introduction](#Introduction)
* [Data](#Data)
* [Methods](#Methods)
    * [Scaling](#Scaling)
    * [Parameter Selection](#Parameter-Selection)
    * [Spatial Matrix](#Spatial-Matrix)
* [Results](#Results)
* [Discussion](#Discussion)
* [Conclusion](#Conclusion)
* [References](#References)

## Introduction
Greater Manchester is the second most populous urban zone in the
UK<sup>[1](https://en.wikipedia.org/wiki/Demography_of_Greater_Manchester#Urban_and_metropolitan_area) </sup>
with a booming economy that saw businesses grow 58% from
2014-2018<sup>[2](https://www.manchestereveningnews.co.uk/business/business-news/business-booming-manchester-ons-data-15263089)
</sup>. A region with a proud history of working-class roots that has always had a reputation of
being a little rough around the edges, has seen a massive cash injection since the turn of the
century resulting in a city oozing culture and a unique personality. Renowned for its
impressive food, theatre, sports, music and nightlife scenes along with the stunning surrounding
areas such as the peak and lake districts; there really is something for everyone in Manchester.

This project clusters the wards of Greater Manchester based on their characteristics, namely
the amount of green space in the ward, as well as the number and type of nearby venues. Once clustered,
data-based decision can then be used to assess the desirability of wards for different types of people,
from families to young professionals, working class to affluent people and so on. This is assessed in the
case of a young professional seeking a lively area with a bustling food and nightlife scene, preferably with
nearby green space, moving to Manchester for work.

Two clustering methods were used for analysis; K Means clustering and spatially constrained
hierarchical agglomerative clustering. Results showed that the features used in the modelling were not organised in a
particularly clustered manner and results were sub-optimal. Nonetheless, by trying different model parameters and weight
matrices, similar wards were clustered and insights were still able to be gained on the characteristics of certain areas.

For the case of the young professional, suburban wards such as Chorlton, Didsbury West and
Didsbury East, as well as central wards Deansgate, Piccadilly and Hulme were shown have
suitable characteristics for their preferences.

## Data

Two data sources were identified; OS Data Hub<sup> [3](https://osdatahub.os.uk/) </sup> and
Foursquare <sup>[4](https://developer.foursquare.com/) </sup>.

Shape files acquired from OS Data Hub contained the shapes (polygons) of all wards and
all 'green space' in the UK. The 215 Greater Manchester wards' polygons were extracted, as
were the green space areas which were within the wards (Figure 1).

<center>
<figure>
<img src="https://github.com/stuartclothier/BattleOfTheNeighbourhoods/blob/main/Project%20Files/Manchester/Images/Manchester_Greenspace.PNG" style="width:100px;height:350px;">
<figcaption>Figure 1: Greenspace (green) shown within Greater Manchester ward boundaries
(orange). Plotted using Folium with Stamen Toner tiles. Data Source: OS Data Hub.</figcaption>
</figure>
</center>

Foursquare's explore endpoint was accessed to find venues within 1km of the centre of the
wards, with a limit of 100 venues. A choropleth map of the total venues per ward can be seen
below in Figure 2.

<center>
<figure>
<img src="https://github.com/stuartclothier/BattleOfTheNeighbourhoods/blob/main/Project%20Files/Manchester/Images/Manchester_Venues_Choropleth.PNG" style="width:500px;height:350px;">
<figcaption>Figure 2: Choropleth of total venue within Greater Manchester ward boundaries.
Plotted using Folium with Open Street Map tiles. Data Source: Foursquare.</figcaption>
</figure>
</center>

The choropleth shows that, as one may expect, the central areas of Manchester have the highest
density of venues. Conversely, most outer wards contain very low density of nearby venues although
some pockets of moderate venue density are seen. This is most notable around the inner southern wards
of Chorlton and Didsbury, as well as Davyhulme East, home of Greater Manchester largest shopping complex,
the Intu Trafford Centre. There is also a slight increase in venue density around towns of Bolton, Wigan,
Bury, Rochdale and Stockport.
For the purpose of clustering wards seven features were selected. The percentage of ward considered
Green space ('Greenspace(pct)') and the number of nearby venues found with Foursquare in the categories:
'Arts & Entertainment', 'Food', 'Nightlife Spot', 'Outdoors & Recreation', 'Shop & Service' and 'Travel
& Transport'.

## Methods

### Scaling
Due to the distributions of the features differing significantly scaling of the data was imperative.
Two feature sets were created for modelling. A non-transformed min-max scaled feature set, which retained the shape of
the original distributions. A log transformed and min-max scaled set was also created, minimising the
prevalence of outliers.

<center>
<figure>
<img src="https://github.com/stuartclothier/BattleOfTheNeighbourhoods/blob/main/Project%20Files/Manchester/Images/count_distributions.PNG">
<figcaption>Figure 3: Venue count data boxplots; Left to Right: Unscaled, Log transformed and min-max
scaled; Min-max scaled </figcaption>
</figure>
</center>

### Parameter Selection
Two methods for clustering Greater Manchester wards were used: K Means and spatially constrained
hierarchical agglomerative clustering. Both methods were applied on both the untransformed feature set and log
transformed feature set.

K Means simply uses the euclidean distance for the similarity of the feature values and any spatial coherence is ignored.
For the number of clusters for K Means the elbow method and silhouette coefficients were considered,
using the Yellowbrick package[link please].

<center>
<figure>
<img src="https://github.com/stuartclothier/BattleOfTheNeighbourhoods/blob/main/Project%20Files/Manchester/Images/Clustering_Outputs/Elbow_Method_nontransfrom.PNG" style="width:400px;height:250px;">
<img src="https://github.com/stuartclothier/BattleOfTheNeighbourhoods/blob/main/Project%20Files/Manchester/Images/Clustering_Outputs/Elbow_Method_logtransfrom.PNG" style="width:400px;height:250px;">
<figcaption>Figure 4: Elbow method based on distortion score at k values 2 to 10; left: untransformed
features, right: log transformed features. Suggested k value based on point with maximum curvature.
</figcaption>
</figure>
</center>

The Yellowbrick function suggests a k value of 4 for both sets of features, as this is the point with maximum curvature. However,
as seen in figure 4, the curve doesn't exhibit a clear 'elbow' and, as such, this k selection is not
clearly superior to other values. The silhouettes for 3, 4 and 5 cluster solution for both feature sets were also considered.

The higher the value of the silhouette coefficient or score, the better each sample 'fits' to its cluster and
'does not fit' to the other clusters (i.e. similarity within the cluster/against dissimilarity to other clusters).
Silhouette coefficients take values between -1 and 1. For a sample with a score of 1, it would indicate optimal
clustering. A value of 0 indicates that observation lies exactly between clusters, while a negative values indicates
it is likely in the wrong cluster[link please].
Ideally, silhouettes should be of similar sizes or width, with the majority of samples fitting consistently well
(i.e. the majority of points around the average silhouette score). Although a metric of the overall fit of the model,
average silhouette scores can be somewhat misleading. It is possible a few samples may be placed in clusters
with more dissimilarity than similarity, yet the model will still yield a relatively high mean score.
The high mean score will be a result of a considerably larger, well fitting cluster.

<center>
<figure>
<img src="https://github.com/stuartclothier/BattleOfTheNeighbourhoods/blob/main/Project%20Files/Manchester/Images/Clustering_Outputs/silhouette_nontransfrom.PNG" style="width:800px;height:250px;">
<img src="https://github.com/stuartclothier/BattleOfTheNeighbourhoods/blob/main/Project%20Files/Manchester/Images/Clustering_Outputs/silhouette_logtransfrom.PNG" style="width:800px;height:250px;">
<figcaption>Figure 5: Silhouette plots for 3 to 5 K Means clustering; left: untransformed features,
right: log transformed features.</figcaption>
</figure>
</center>

This is evident in the silhouette plots above, particularly for the untransformed data. A large section of samples
(e.g. cluster 0 in the 3 cluster solution for) are responsible for inflating the average coefficient value. Although the 3 cluster solution gives
the largest mean silhouette value for both sets, the distribution of samples across the clusters is far from ideal.
Similarly for the untransformed 3 cluster solution, around half of the
samples in cluster 2 have silhouette scores below 0, indicting this cluster has many outliers. 4 and 5 cluster
solutions offer similar issues, indicating the features selected are not organised in a clustered manner.

Taking the log of the venue counts was shown to produce more visually pleasing silhoutte plots. It must be noted however that
the mean silhouette scores now obtained are around one third lower than before. Some 'uniqueness' of the data appears
to have been lost. Although outlier prevalance is significantly reduced there is a tradeoff in the overall 'power' in the
data.

All things considered, a 4 cluster solution was selected for the untransformed feature set, and a 5 cluster
solution for the log transformed data.

### Spatial Matrix

The spatially constrained method includes a connectivity matrix which uses geospatial weighting to group nearby wards.
This restricts which wards can be clustered together; wards that are not 'connected' cannot be clustered together.
Several parameters for the spatial matrix were considered including a 'Queen' method, which restricts clustering to only
allow neighbouring wards. This was considered too restrictive and instead a distance band of 4.6 km was applied. This
value was chosen to allow wards that don't touch to still be clustered while minimising the distance without creating
islands (i.e. wards with no connections). This gives the clustering mild spatial coherence, increasing the likelihood
of nearby wards being clustered, creating 'regions'.

4 and 5 cluster solutions, for the untransformed and log transformed feature sets respectively, were produced for comparison to K Means.

## Results

As mentioned in the methodology section, the wards were clustered four times. 4 cluster solutions of the un-transformed
features and 5 cluster solutions of the log transformed features, using both K Means and spatially constrained hierarchical agglomerative clustering.

<center>
<figure>
<img src="https://github.com/stuartclothier/BattleOfTheNeighbourhoods/blob/main/Project%20Files/Manchester/Images/Clustering_Outputs/KMeans_4_nontransfrom.PNG" style="width:500px;height:350px;">
<img src="https://github.com/stuartclothier/BattleOfTheNeighbourhoods/blob/main/Project%20Files/Manchester/Images/Clustering_Outputs/HAC_4_nontransfrom.PNG" style="width:500px;height:350px;">
<img src="https://github.com/stuartclothier/BattleOfTheNeighbourhoods/blob/main/Project%20Files/Manchester/Images/Clustering_Outputs/KMeans_5_logtransfrom.PNG" style="width:500px;height:350px;">
<img src="https://github.com/stuartclothier/BattleOfTheNeighbourhoods/blob/main/Project%20Files/Manchester/Images/Clustering_Outputs/HAC_5_logtransfrom.PNG" style="width:500px;height:350px;">
<figcaption>Figure 6: Clustering of Greater Manchester wards; top left (a): non-transformed K Means, top right (b): non-transformed Spatially Constrained Hierarchical
Agglomerative Clustering, bottom left (c): log-transformed K Means, bottom right (d): log-transformed Spatially Constrained Hierarchical Agglomerative Clustering
Plotted using Folium with Open Street Map tiles.</figcaption>
</figure>
</center>

The spatially constrained clustering methods have produced results with slightly more spatial coherence. That is, this
clustering appears to have more geographical dependence than the seemingly random K Means clustering. Nonetheless,
despite some visually interesting results, gaining information from the above visualisations alone is very difficult.
Viewing the mean values of cluster features allows an insight on which wards might be suitable for our young professional.

<center>
<table align="center">
<tr><td></td><td>0</td><td>1</td><td>2</td><td>3</td></tr>
<tr><td>Arts & Entertainment</td><td>1.789</td><td>0.344</td><td>0.213</td><td>3.833</td></tr>
<tr><td>Food</td><td>13.842</td><td>3.25</td><td>2.011</td><td>35.667</td></tr>
<tr><td>Nightlife Spot</td><td>3.789</td><td>1.302</td><td>0.915</td><td>16.833</td></tr>
<tr><td>Outdoors & Recreation</td><td>3.684</td><td>1.135</td><td>1.351</td><td>7.0</td></tr>
<tr><td>Shop & Service</td><td>9.053</td><td>3.823</td><td>2.713</td><td>10.667</td></tr>
<tr><td>Travel & Transport</td><td>3.263</td><td>0.906</td><td>0.723</td><td>4.167</td></tr>
<tr><td>Greenspace(pct)</td><td>0.106</td><td>0.061</td><td>0.149</td><td>0.101</td></tr>
<tablecaption>Table 1: Mean values for spatially constrained hierarchical agglomerative clustering of non-transformed features
</tablecaption>
</table>
</center>

The mean values of the non-transformed spatially constrained clusters gives the best set of wards which fit the needs of
the young professional best. From table 1, cluster 3 is identified as a potential list of suitable wards, given the
very high venue counts. This cluster can also be geographical visualised in figure 6(b) (green).
The original values of the features for these wards can also be viewed.

<center>
<table>
<tr><td></td><td>NAME</td><td>cluster</td><td>Arts & Entertainment</td><td>Food</td><td>Nightlife Spot</td><td>Outdoors & Recreation</td><td>Shop & Service</td><td>Travel & Transport</td><td>Greenspace(pct)</td></tr>
<tr><td>46</td><td>Didsbury West</td><td>3</td><td>0</td><td>41</td><td>18</td><td>7</td><td>12</td><td>5</td><td>0.211</td></tr>
<tr><td>47</td><td>Chorlton</td><td>3</td><td>0</td><td>29</td><td>21</td><td>8</td><td>13</td><td>1</td><td>0.041</td></tr>
<tr><td>53</td><td>Hulme</td><td>3</td><td>10</td><td>24</td><td>13</td><td>1</td><td>4</td><td>2</td><td>0.061</td></tr>
<tr><td>64</td><td>Deansgate</td><td>3</td><td>7</td><td>46</td><td>17</td><td>9</td><td>14</td><td>6</td><td>0.007</td></tr>
<tr><td>66</td><td>Piccadilly</td><td>3</td><td>3</td><td>44</td><td>20</td><td>11</td><td>13</td><td>8</td><td>0.014</td></tr>
<tr><td>68</td><td>Didsbury East</td><td>3</td><td>3</td><td>30</td><td>12</td><td>6</td><td>8</td><td>3</td><td>0.274</td></tr>
<tablecaption>Table 2: Feature values for cluster 3 wards using spatially constrained hierarchical agglomerative clustering of non-transformed feature values.
</tablecaption>
</table>
</center>

From the wards listed in table 2, Didsbury West and East contain significantly higher green space; another preferred
characteristic, suggesting these as ideal wards.

## Discussion

Clustering Greater Manchester's wards based on venue counts and greenspace area (percentage of total area) proved to be
difficult, as the features selected are not organised in a particularly clustered manner. Fine-tuning of parameters can
lead to significantly different results. However, running several clustering algorithms a shortlist of suggested wards
can be produced. 6 wards which featured prominently in most favourable clusters were those shown in cluster 3 of the
untransformed Spatially Weighted Hierarchical Agglomerative Clustering (Table 2).

Considering the superior green space percentage, the Disdbury wards could be considered most optimal for
the young professional. Additionally, both Didsbury wards and nearby Chorlton are further from the centre than the
other three wards in this cluster. This may equate to less expensive housing/renting costs than the central wards.

Although the Didsbury area appears to be an ideal choice for a young professional seeking an area with high restaurant
and bar density, with decent green space, it must be noted that several limitations to this solution exist.

Possibly the most notable issue with the approach taken in this project is the lumping of feature values into
irregularly shaped ward boundaries, effectively acting as bins. Neighbouring wards can be seen to have considerably
different feature values with no consideration given to the actual proximity of amenities. For example, Sedgley is
around 7% greenspace despite bordering onto the massive Heaton Park while neighbour Higher Blackley, which contains
the park, has about 55% greenspace. It is entirely feasible to live closer to Heaton Park while still living in Sedgley,
than it is for some living in Higher Blackley.

Furthermore, some wards extend well beyond the 1 km search radius used for venue counts, while some are well short.
Given the 100 venue search limit these results are also limited in higher venue density areas.

The features selected also only characterise the wards to a certain degree. For a better picture it would be beneficial
to add many more discriminating features, such as housing/living costs, crime rate, school access/ratings,
access to healthcare and so on. Increasing the features would also increase the usefulness of using a clustering
algorithm. Given the 7 current features, it would likely be more efficient to sort the features and simply find which
wards tend to rate highest consistently.

One distinct strength of the ward-based approach (or any boundary approach, e.g, state or counties etc.)
however, is the ability to use census, survey or local government statistics. In the UK, censuses are taken every 10
years and at the time of writing this report a census has actually taken place.
Unfortunately, these results will not be published in time for analysis. Additionally, the use of clustering
algorithms on these statistics may be somewhat superfluous and insights could easily be obtained from more
simple methods such as choropleth maps, like shown in figure 2.

Methods for analysing proximity to precise amenity locations may be more useful than putting feature values in broad,
irregularly shaped bins, for example heatmaps.

## Conclusion

The clustering methods to venue count and greenspace percentage has proven sub-optimal. Several limitations exists such
as the limited features inputted and the irregular shape of the ward boundaries. Choropleths for ward-specific
statistics may provide clearer insights, while heatmaps might produce better geographical solutions for proximity to
amenities. The case of a young professional seeking an area with a high density of restaurants, bars and access to
greenspace, yielded a selection of 6 preferred wards (cluster 3, Untransformed Spatially Weighted Hierarchical
Agglomerative Clustering). Disbury West was suggested as an ideal area to concentrate a search for a home around.

## References

1. [Demography_of_Greater_Manchester, Wikipedia, accessed 17/03/21](https://en.wikipedia.org/wiki/Demography_of_Greater_Manchester#Urban_and_metropolitan_area)
2. [Business Booming Manchester, Manchester Evening News, accessed 17/03/21](https://www.manchestereveningnews.co.uk/business/business-news/business-booming-manchester-ons-data-15263089)
3. [OS Data Hub](https://osdatahub.os.uk/)
4. [Foursquare Developer Documentation](https://developer.foursquare.com/)

