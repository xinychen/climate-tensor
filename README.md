# Short Climate Variable Forecasting

Data source: [Monthly aggregated Water Vapor MODIS MCD19A2 (1 km): Long-term data (2000-2022)](https://doi.org/10.5281/zenodo.8193024)

```python
pip install GDAL
```

```python
from osgeo import gdal
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np

dataset = gdal.Open(r'wv_mcd19a2v061.seasconv.m.m01_p50_1km_s_20000101_20221231_go_epsg.4326_v20230619.tif')

band = dataset.GetRasterBand(1)

b = np.array(band.ReadAsArray()[4500 : 8000,
                                6600 : 13100]).astype(float)

b[b == -1] = np.nan

f = plt.figure(figsize = (6.5, 3))
# plt.imshow(b1)
sns.heatmap(b1, cmap = "Spectral_r", vmin = 0, vmax = 3000)

plt.axis('off')
plt.savefig('water_vapor_na.png')
plt.show()
```
