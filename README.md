# Climate Variable Data & Visualization

Data source: [Monthly aggregated Water Vapor MODIS MCD19A2 (1 km): Long-term data (2000-2022)](https://doi.org/10.5281/zenodo.8193024)

```python
pip install GDAL
```

```python
import numpy as np
from osgeo import gdal
import matplotlib.pyplot as plt
import seaborn as sns

fig = plt.figure(figsize = (14, 3))
i = 0
for month in [5, 6]:
    dataset = gdal.Open(r'wv_mcd19a2v061.seasconv.m.m0{}_p50_1km_s_20000101_20221231_go_epsg.4326_v20230619.tif'.format(month))
    mat = np.array(dataset.GetRasterBand(1).ReadAsArray()[4500 : 8000, 6600 : 13100]).astype(float)
    mat[mat == -1] = np.nan
    ax = fig.add_subplot(1, 2, i + 1)
    ax = sns.heatmap(mat,  cmap = "Spectral_r", vmin = 0, vmax = 4500,
                     cbar_kws = {"shrink": 0.5, 'label': r'Water vapor ($10^{-3}$cm)'})
    plt.axis('off')
    if month == 5:
        plt.title('May (2000-2022)')
    elif month == 6:
        plt.title('June (2000-2022)')
    i += 1
plt.show()
```

```python
import numpy as np
from osgeo import gdal
import matplotlib.pyplot as plt
import seaborn as sns

dataset = gdal.Open(r'wv_mcd19a2v061.seasconv.m.m06_p50_1km_s_20000101_20221231_go_epsg.4326_v20230619.tif')
mat = np.array(dataset.GetRasterBand(1).ReadAsArray()[4500 : 8000, 6600 : 13100]).astype(float)
mat[mat == -1] = np.nan
fig = plt.figure(figsize = (6.5, 3))
ax = sns.heatmap(mat,  cmap = "Spectral_r", vmin = 0, vmax = 4500,
                  cbar_kws = {"shrink": 0.5, 'label': r'Water vapor ($10^{-3}$cm)'})
plt.axis('off')
plt.title('June (2000-2022)')
plt.show()
```

```python
import numpy as np
from osgeo import gdal

for i in ['01', '02', '03', '04', '05', '06',
          '07', '08', '09', '10', '11', '12']:
    dataset = gdal.Open(r'wv_mcd19a2v061.seasconv.m.m{}_p50_1km_s_20000101_20221231_go_epsg.4326_v20230619.tif'.format(i))
    mat = np.array(dataset.GetRasterBand(1).ReadAsArray()[4500 : 8000, 6600 : 13100]).astype(float)
    mat[mat == -1] = np.nan
    np.savez_compressed('water_vapor_month_{}.npz'.format(i), mat)
```

```python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
fig = plt.figure(figsize = (14, 3))
i = 0
for month in [5, 6]:
    mat = np.load('water_vapor_month_0{}.npz'.format(month))['arr_0']
    ax = fig.add_subplot(1, 2, i + 1)
    ax = sns.heatmap(mat,  cmap = "Spectral_r", vmin = 0, vmax = 4500,
                     cbar_kws = {"shrink": 0.5, 'label': r'Water vapor ($10^{-3}$cm)'})
    plt.axis('off')
    if month == 5:
        plt.title('May (2000-2022)')
    elif month == 6:
        plt.title('June (2000-2022)')
    i += 1
plt.show()
```

