# Bio-Optical Models for Water Quality Analysis  


This repository has the implementation and tests of benchmarked bio-optic models that evaluates some water quality indexes by analyzing satellite images.
  

## Table of contents  

* [General Info](#general-info)
* [Getting Started](#getting-started)
* [Usage](#usage)
* [Testing](#testing)
* [Contributing](#contributing)
* [Links](#links)
* [Authors and Contributors](#authors-and-contributors)
* [License](#license)



## General Info  

In order to analyze the water quality of reservoirs and lakes by remote sensing methods, such as satellites images, bio-optical models were used. Those models are mathematical and statistical algorithms which can be used to predict different water quality indexes by analyzing the water-leaving radiance measured at different bands of electromagnetic spectrum by sensors
onboard satellites.

According to the literature there are different approaches used in bio-optical modeling
since simple models, based on empirical and semi-empirical relations, until most complexes models based on radiative transfer theory.

Currently, this project implements a library of empirical and semi-empirical models which can be
used to predict the concentration of chlorophyll-a, total suspended solids, water transparency, turbidity, phycocyanin and the detection of macrophytes in aquatic environments.

All the original equations are adapted in order to be applied in images collected by MSI (Multispectral Instrument) sensor onboard [Sentinel-2A and Sentinel-2B platforms](https://earth.esa.int/web/sentinel/user-guides/sentinel-2-msi).

  
  
## Getting Started  

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.
  
  
### Technologies

To execute this project, you'll need the following technologies:
  
- 32- or 64-bit computer
- [Python 3](https://www.python.org/downloads/)


### Setup

This repository can be used as a complementary library for a main project and its modules can be used whenever they are necessary. 
All dependencies are listed in requirements.txt.

To install dependencies use the following command:  
  
```python3 -m pip install -r requirements.txt```

You can install dependencies directly in your machine or in a virtual environment of your choice, such as [VirtualEnv](https://virtualenv.pypa.io/en/latest/) or [Conda](https://docs.conda.io/en/latest/).


## Usage

First, import the required packages and desired methods:


```python
import rasterio as rio
import numpy

from models import total_suspended_solids_turbidity as turbidity
``` 


Set and open the respective satellite images required to analyze the indexes:


```python
B04_20m = rio.open("tests/assets/20m/B4_20m_20181224.tif").read()

reflectance_659nm_wavelength = B04_20m
```


Open the band image file and choose one or more methods to analyze the desired index: 


```python
meta = rio.open("tests/assets/20m/B4_20m_20181224.tif").meta
meta.update(driver="GTiff")
meta.update(dtype=rio.float32)

miller_mckee_2004 = turbidity.miller_mckee_2004(reflectance_659nm_wavelength)
```


Create and save the new image generated by the respectives bands of the chosen method:


```python
with rio.open("miller_mckee_2004.tif", "w", **meta) as dist:
    dist.write(miller_mckee_2004.astype(rio.float32))
```

The output is a **.tif** file containing the reservoir image processed by the method:

![Reservoir](https://i.imgur.com/gOnaIAn.png)

## Testing

This repository implementations can be tested by running **pytest** command in **src** folder.  
  
```python3 -m pytest```
  

## Contributing


Contributions are always welcome!
To fix a bug or enhance an existing module, follow these steps:

- Fork the repo
- Create a new branch (```git checkout -b improve-feature```)
- Make the appropriate changes in the files
- Add changes to reflect the changes made
- Commit your changes (```git commit -am 'Improve feature'```)
- Push to the branch (```git push origin improve-feature```)
- Create a Pull Request


While contributing, remember to add tests to the new developed methods.



## Links 

- [A Comprehensive Review on Water Quality Parameters Estimation Using Remote Sensing Techniques](https://www.researchgate.net/publication/306240486_A_Comprehensive_Review_on_Water_Quality_Parameters_Estimation_Using_Remote_Sensing_Techniques)
- [Bio-optical Modeling and Remote Sensing of Inland Waters](https://www.sciencedirect.com/book/9780128046449/bio-optical-modeling-and-remote-sensing-of-inland-waters)


## References

### Chlorophyll-a

ALLAN, M.G, HICKS, B.J., BRABYN, L. (2007). Remote sensing of the Rotorua lakes for water quality. CBER Contract Report No. 51, client report prepared for Environment Bay of Plenty. Hamilton, New Zealand: Centre for Biodiversity and Ecology Research, Department of Biological Sciences, School of Science and Engineering, The University of Waikato.

CHAVULA, G.; BREZONIK, P.; THENKABAIL, P.; JOHNSON, T.; BAUER, M. Estimating chlorophyll concentration in Lake Malawi from MODIS satellite imagery. Physics and Chemistry of the Earth, Parts A/B/C, [s. l.], v. 34, n. 13–16, p. 755–760, 2009.

DALL’OLMO, G.; GITELSON, A. A.; RUNDQUIST, D. C. Towards a unified approach for remote estimation of chlorophyll-a in both terrestrial vegetation and turbid productive waters. Geophysical Research Letters, [s. l.], v. 30, n. 18, 2003.

GITELSON, A. A.; SCHALLES, J. F. & HLADIK, C. M. Remote chlorophyll-a retrieval in turbid, productive estuaries: Chesapeake Bay case study, Remote Sensing of Environment, v. 109, p. 464 – 472, 2007.

GORDON, H. & MOREL, A. Remote Assessment of Ocean Color for Interpretation of Satellite Visible Imagery: A Review. Lecture Notes on Coastal and Estuarine Studies, v. 4, Springer Verlag, New York, 114 p. 1983.

GONS, H. J. Optical Teledetection of Chlorophyllain Turbid Inland Waters. Environmental Science & Technology, [s. l.], v. 33, n. 7, p. 1127–1132, 1999.

GOWER, J.; KING, S.; BORSTAD, G.; BROWN, L. Detection of intense plankton blooms using the 709 nm band of the MERIS imaging spectrometer. International Journal of Remote Sensing, [s. l.], v. 26, n. 9, p. 2005–2012, 2005.

LE, C.; LI, Y.; ZHA, Y.; SUN, D.; HUANG, C.; LU, H. A four-band semi-analytical model for estimating chlorophyll a in highly turbid lakes: The case of Taihu Lake, China. Remote Sensing of Environment, [s. l.], v. 113, n. 6, p. 1175–1182, 2009.

MISHRA, S.; MISHRA, D. R. A novel model for remote estimation of chlorophyll-a concentration in turbid productive waters. Remote Sensing of Environment, v. 117, p. 394 - 406, 2012.

RODRIGUES, T; ALCÂNTARA, E; WATANABE, F; ROTTA, LUIZ; IMAI, N; CURTARELLI, M & BARBOSA, C. Comparação entre Métodos Empíricos para estimativa da concentração de Clorofila-a em Reservatórios em Cascata (Rio Tietê, São Paulo), Revista Brasileira de Cartografia, v. 68, p. 181-192, 2016.


### Cyanobacteria

DASH, P., WALKER, N.D., MISHRA, D.R., HU, C., PINCKNEY, J.L., D’SA, E.J., (2011). Estimation of cyanobacterial pigments in a freshwater lake using OCM satellite data. Remote Sens. Environ. 115 (12), 3409-3423.

SIMIS, S.G.H., PETERS, S.W.M., GONS, H.J., (2005). Remote sensing of the cyanobacterial pigment phycocyanin in turbid inland water. Limnol. Oceanogr. 50, 237-245.

WOZNIAK, M., BRADTKE, K.M., DARECKI, M., KREZEL, A., (2016). Empirical model for phycocyanin concentration estimation as an indicator of cyanobacterial bloom in the optically complex coastal waters of the Baltic Sea. Remote Sens. 8 (3), 212-234.


### Macrophytes

HUETE, A. A comparison of vegetation indices over a global set of TM images for EOS-MODIS. Remote Sensing of Environment, [s. l.], v. 59, n. 3, p. 440–451, 1997. 

TUCKER, C. J. Red and photographic infrared linear combinations for monitoring vegetation. Remote Sensing of Environment, [s. l.], v. 8, n. 2, p. 127–150, 1979.

VILLA, P.; LAINI, A.; BRESCIANI, M.; BOLPAGNI, R. A remote sensing approach to monitor the conservation status of lacustrine Phragmites australis beds. Wetlands Ecology and Management, [s. l.], v. 21, n. 6, p. 399–416, 2013.

VILLA, P.; MOUSIVAND, A.; BRESCIANI, M. Aquatic vegetation indices assessment through radiative transfer modeling and linear mixture simulation. International Journal of Applied Earth Observation and Geoinformation, [s. l.], v. 30, p. 113–127, 2014. 


### Total Suspended Solids and Turbidity

DOXARAN, D.; FROIDEFOND, J.-M.; CASTAING, P. Remote-sensing reflectance of turbid sediment-dominated waters Reduction of sediment type variations and changing illumination conditions effects by use of reflectance ratios. Applied Optics, [s. l.], v. 42, n. 15, p. 2623, 2003.

DOXARAN, D.; FROIDEFOND, J.-M.; CASTAING, P.; BABIN, M. Dynamics of the turbidity maximum zone in a macrotidal estuary (the Gironde, France): Observations from field and MODIS satellite data. Estuarine, Coastal and Shelf Science, [s. l.], v. 81, n. 3, p. 321–332, 2009.

LIU, C. D., HE, B. Y., LI, M. T., REN, X. X.  (2006). Quantitative modeling of suspended sediment in middle Changjiang river from MODIS. Chinese Geographical Science, v. 16, pp. 79–82.

MILLER, R. L.; MCKEE, B. A. Using MODIS Terra 250 m imagery to map concentrations of total suspended matter in coastal waters. Remote Sensing of Environment, [s. l.], v. 93, n. 1–2, p. 259–266, 2004. 

TANG, S.; LAROUCHE, P.; NIEMI, A.; MICHEL, C. Regional algorithms for remote-sensing estimates of total suspended matter in the Beaufort Sea. International Journal of Remote Sensing, [s. l.], v. 34, n. 19, p. 6562–6576, 2013.

TARRANT, P. E.; AMACHER, J. A.; NEUER, S. Assessing the potential of Medium-Resolution Imaging Spectrometer (MERIS) and Moderate-Resolution Imaging Spectroradiometer (MODIS) data for monitoring total suspended matter in small and intermediate sized lakes and reservoirs. Water Resources Research, [s. l.], v. 46, n. 9, 2010.

ZHANG, Y.; LIN, S.; LIU, J.; QIAN, X.; GE, Y. Time-series MODIS Image-based Retrieval and Distribution Analysis of Total Suspended Matter Concentrations in Lake Taihu (China). International Journal of Environmental Research and Public Health, [s. l.], v. 7, n. 9, p. 3545–3560, 2010.


### Water Transparency

GIARDINO, C. et al. (2001). Detecting chlorophyll, Secchi disk depth and surface temperature in a sub-alpine lake using Landsat imagery. The Science of Total Environment, v. 268, pp. 19-29.

GUIMARÃES, V. S. et al. (2016). Desenvolvimento de modelo empírico para determinação de transparência de Secchi na Lagoa da Conceição – SC, a partir de imagens multiespectrais do sensor Operational Land Imager (OLI) -Landsat-8. Anais do XXI Simpósio Brasileiro de Recursos Hídricos.

HÄRMÄ, P. et al. (2001). Detecting chlorophyll, Secchi disk depth and surface temperature in a sub-alpine lake using Landsat imagery. The Science of Total Environment, v. 268, pp. 107-121.



## Authors & Contributors

Developed by CERTI Foundation.

This research was supported by FOZ DO CHAPECÓ ENERGIA S.A research and technological development program, 

through the PD-02949-2405/2019 project, regulated by Brazilian Electricity Regulatory Agency (ANEEL).



## License

This repository is licensed under the terms of the BSD-style license.
