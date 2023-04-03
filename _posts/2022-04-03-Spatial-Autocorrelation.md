---
layout: distill
title: Spatial Randomness and Autocorrelation
description: An introduction to computing spatial Randomness and autocorrelation in R with examples
giscus_comments: true
categories: Tutorial
date: 2023-04-03

authors:
  - name: Kenan Li
    url: "https://www.slu.edu/public-health-social-justice/faculty/li-kenan.php"
    affiliations:
      name: Saint Louis University
  

bibliography: 2018-12-22-distill.bib

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: Background
    # if a section has subsections, you can add them as follows:
    subsections:
      - name: Definitions
      - name: Differences
      - name: Complete Spatial Randomness (CSR) measurements
      - name: Spatial autocorrelation measurements
  - name: Examples
    subsections:
      - name: Using K function
      - name: Using L function
      - name: Using G function
---

## Background

Spatial randomness and spatial autocorrelation are two concepts related to the distribution of data in space. They describe different aspects of spatial data and have different implications for the analysis of spatial patterns.

### Definitions

`Spatial randomness`: Spatial randomness refers to a situation where the location of events or features in space does not follow any discernible pattern. In other words, the events or features are randomly distributed across the study area, and their locations are not influenced by the locations of other events or features. Complete spatial randomness (CSR) is often used as a null hypothesis in spatial analyses, which assumes that the observed pattern is random and not influenced by any underlying spatial process.

`Spatial autocorrelation`: Spatial autocorrelation refers to the correlation between a variable at one location and the same variable at neighboring locations. It measures the degree to which nearby locations exhibit similar (or dissimilar) values. Positive spatial autocorrelation means that similar values are clustered together, while negative spatial autocorrelation means that dissimilar values are closer together. If there is no spatial autocorrelation, the values are randomly distributed across the study area.

### Differences

* Spatial randomness pertains to the absence of any discernible pattern in the distribution of events or features in space, while spatial autocorrelation measures the correlation between a variable at one location and the same variable at neighboring locations.

* Spatial randomness is often used as a null hypothesis in spatial analysis, while spatial autocorrelation is an inherent property of the data that can reveal underlying spatial processes or structures.

In summary, spatial randomness and spatial autocorrelation describe different aspects of spatial data. Spatial randomness relates to the distribution of events or features, while spatial autocorrelation quantifies the degree of similarity between values at nearby locations. These concepts are essential for understanding and analyzing spatial patterns in various types of spatial data.

### Complete Spatial Randomness (CSR) measurements

1. `Ripley's K function`: The K function is used to assess CSR in point patterns. It calculates the expected number of points within a given distance 'r' of an arbitrary point, divided by the overall point density. If the observed K function is close to the theoretical K function under CSR, the point pattern is consistent with spatial randomness.

2. `L function`: The L function is a derived function from Ripley's K function, which is used to assess Complete Spatial Randomness (CSR) in point pattern data. The L function is designed to provide a linearized visualization of the K function, making it easier to interpret the results.

3. `Nearest Neighbor Distance Distribution function (G function)`: The G function is another measure used to assess CSR in point patterns. It estimates the cumulative distribution function of the nearest neighbor distances in a point pattern. If the observed G function is close to the theoretical G function under CSR, the point pattern is consistent with spatial randomness.

### Spatial autocorrelation measurements

1. `Moran's I` Moran's I is a global measure of spatial autocorrelation for continuous or areal data. It measures the correlation between a variable at one location and the same variable at neighboring locations. Moran's I values range from -1 (negative spatial autocorrelation) to 1 (positive spatial autocorrelation), with values close to 0 indicating no spatial autocorrelation.

2. `Geary's C`: Geary's C is another global measure of spatial autocorrelation for continuous or areal data. It is based on the difference between values at neighboring locations. Geary's C values range from 0 (positive spatial autocorrelation) to 2 (negative spatial autocorrelation), with values close to 1 indicating no spatial autocorrelation.

3. `Local Indicators of Spatial Association (LISA)`: LISA is a set of local measures of spatial autocorrelation for continuous or areal data. It identifies local clusters of high or low values and spatial outliers. Common LISA statistics include Local Moran's I, Local Geary's C, and Local Getis-Ord G*.

4. `Semivariogram`: The semivariogram is a measure of spatial autocorrelation for continuous data that quantifies the similarity between pairs of data points as a function of the distance between them. It is used to model spatial dependence and is the basis for geostatistical techniques like kriging.

***

## Examples

### Using K Function and L Fuction

The K function is defined by:

$$K(r) = λ^{-1} * E[N(r)] \quad\quad (1)$$

where λ is the point density (total number of points divided by the study area), E[N(r)] is the expected number of points within distance 'r' from an arbitrary point, and r is the distance. 

When analyzing point patterns to test for CSR, we often assume an underlying Poisson process.The theoretical K function under CSR for a Poisson process is:

$$K(r) = \pi * r^2 \quad\quad (2)$$

* If the observed K function is close to the theoretical K function (Poisson process), the point pattern is consistent with CSR.

* If the observed K function lies above the theoretical K function, the point pattern exhibits clustering.

* If the observed K function lies below the theoretical K function, the point pattern exhibits regularity or dispersion.

By comparing the observed K function with the theoretical K function under the CSR assumption, you can assess whether the spatial distribution of points is consistent with a Poisson process or if there is evidence of clustering or regularity in the data.

When working with a sample of data points, the K function will not usually be known, and we will use the Edge-corrected estimation of K function (Ripley's K function):

$$K(r) = \left(1 / λ\right) * \sum_{i=1}^{n}\left[\sum_{j\neq i}(I(||u_i - u_j|| <= r) / w(||u_i - u_j||))\right] \quad\quad (3)$$

where:

* K(r) is the estimated K function at distance r,
* λ is the point density (total number of points divided by the study area),
* n is the total number of points in the point pattern,
* $u_i$ and $u_j$ are the spatial coordinates of points i and j, respectively,
* $$||u_i - u_j||$$ is the Euclidean distance between points i and j,
* I(·) is an indicator function that equals 1 if the condition inside the brackets is true and 0 otherwise,
* w(·) is an edge-correction function that accounts for the fact that points near the boundary of the study area have fewer neighbors.

The edge correction function is an essential component of Ripley's K function estimation. It is needed to account for the boundary effects that arise when analyzing point patterns within a finite study area. Points near the boundary of the study area have fewer neighbors than points located in the interior, which can lead to biased estimations of the K function. Note that there are different edge-correction methods available, such as the isotropic correction, which is the default method in Ripley's K function estimation. The isotropic correction is given by:

$$w(||u_i - u_j||) = 1/(1 - (π * r^2 / A)) \quad\quad (4)$$

The L function is derived from the K function and is used to provide a linearized interpretation of the results from the K function. The L function is defined as:

$$L(r) = \sqrt{\frac{K(r)}{\pi}} \quad\quad (5)$$

Given a Poisson process, we can substitute Equation 2 into the L function to get the L function under CSR:

$$L_{CSR}(r) = \sqrt{\frac{\pi r^2}{\pi}} = r \quad\quad (6)$$

To simplify the interpretation of the L function, we often plot the difference between the L function and the distance r:

$$L(r) - r \quad\quad (7)$$

The relationship between the K function and the L function can be summarized as follows:

* The L function is derived from the K function to provide a linearized interpretation of the spatial pattern analysis.

* The L function under CSR is equal to the distance r, which simplifies the interpretation of the results.

* By plotting the difference between the L function and the distance r, we can easily identify deviations from CSR, clustering, or dispersion.

To interpret the L(r) - r plot:

* If the L(r) - r plot is close to the horizontal axis (L(r) - r ≈ 0), it suggests Complete Spatial Randomness (CSR).

* If the L(r) - r plot lies above the horizontal axis (L(r) - r > 0), it indicates clustering.

* If the L(r) - r plot lies below the horizontal axis (L(r) - r < 0), it indicates regularity or dispersion.

In summary, the L function is derived from the K function to provide a more straightforward interpretation of spatial patterns in point data. By comparing the L function plot with the expected L function under CSR (L(r) = r), we can assess the degree of clustering or regularity in the data.

In the following example, we will use Ripley's K function to analyze the `bramblecanes` dataset from the `spatstat`library in R. First, Let us visulize the locations of the bramble canes being analysed.

**Figure 1: Bramble Cane Locations**

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/bramblecanes_locations.png" title="Bramble Cane Locations" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Next, we will extimate and plot the observed K function along with the theoretical K function under CSR

{% highlight r %}
K_hat <- Kest(bramblecanes)
plot(K_hat, main = "Observed K Function vs. Theoretical K Function (CSR)")
{% endhighlight %}

**Figure 2: Obeserved K Function vs Theoretical K function (CSR)**
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/bramblecanes_k_function.png" title="Bramble Cane K Functions" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Finally, we can compute and plot the L function:

{% highlight r %}
L_hat <- Lest(bramblecanes)
plot(L_hat, main = "L Function: L(r) - r")
{% endhighlight %}

**Figure 3: Obeserved L Function vs Theoretical L function (CSR)**
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/bramblecanes_l_function.png" title="Bramble Cane L Functions" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

***















