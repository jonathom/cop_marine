# Group Challenge
Alba Vilanova Cortezón · Jannis Fröhlking · Jonathan Bahlmann

## STEP 1: Set up the Google Drive Environment
*Try to increase notebook speed by saving files to my Google Drive*


```python
from google.colab import drive
drive.mount('/content/drive')
```

    Mounted at /content/drive



```python
# Leave that out to save files in running time
import os
os.chdir("/content/drive/MyDrive")
```

## STEP 2: Collect Data

### AIS
Dates

* 01.01.2020
* 01.04.2020
* 01.07.2020
* 01.10.2020


```python
# Define regional domain
# Between Rio de Janeiro and Lisboa
# bbox = ((-9, 22), (-46, 38))
# Alba: It does not work: I don't know how to consider west (negative) coordinates! 
# We should figure that out or change it

# Baltic sea
bbox = ((10, 54),(30, 65))
```


```python
# Download
import requests
import zipfile
import os

# Set file path
urls = ['https://coast.noaa.gov/htdata/CMSP/AISDataHandler/2020/AIS_2020_01_01.zip',
'https://coast.noaa.gov/htdata/CMSP/AISDataHandler/2020/AIS_2020_04_01.zip',
'https://coast.noaa.gov/htdata/CMSP/AISDataHandler/2020/AIS_2020_07_01.zip',
'https://coast.noaa.gov/htdata/CMSP/AISDataHandler/2020/AIS_2020_10_01.zip']

for url in urls:
  r = requests.get(url)
  filename = url.split('/')[-1]
  with open(filename,'wb') as output_file:
      output_file.write(r.content)
  # Unzip
  try:
      with zipfile.ZipFile(filename) as z:
          z.extractall()
          print("Extracted all")
          os.remove(filename)
  except:
      print("Invalid file")
print("Download completed!")
```

    Extracted all
    Extracted all
    Extracted all
    Extracted all
    Download completed!



```python
# Read csv
import pandas as pd
ais_data = pd.DataFrame()
for url in urls:
  filename = url.split('/')[-1]
  filename = filename.replace("zip","csv")
  data = pd.read_csv(filename)
  ais_data = ais_data.append(data)
print("Before preprocessing...")
ais_data
```

    Before preprocessing...





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>MMSI</th>
      <th>BaseDateTime</th>
      <th>LAT</th>
      <th>LON</th>
      <th>SOG</th>
      <th>COG</th>
      <th>Heading</th>
      <th>VesselName</th>
      <th>IMO</th>
      <th>CallSign</th>
      <th>VesselType</th>
      <th>Status</th>
      <th>Length</th>
      <th>Width</th>
      <th>Draft</th>
      <th>Cargo</th>
      <th>TranscieverClass</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>367149340</td>
      <td>2020-01-01T00:00:00</td>
      <td>29.96476</td>
      <td>-90.02724</td>
      <td>1.3</td>
      <td>10.0</td>
      <td>16.0</td>
      <td>SYDNEE TAYLOR</td>
      <td>NaN</td>
      <td>WDD4807</td>
      <td>31.0</td>
      <td>0.0</td>
      <td>26.0</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>31.0</td>
      <td>B</td>
    </tr>
    <tr>
      <th>1</th>
      <td>367687520</td>
      <td>2020-01-01T00:00:00</td>
      <td>30.20558</td>
      <td>-91.03578</td>
      <td>10.7</td>
      <td>124.9</td>
      <td>130.0</td>
      <td>CHIPPEWA</td>
      <td>NaN</td>
      <td>WDI3361</td>
      <td>31.0</td>
      <td>0.0</td>
      <td>22.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
    </tr>
    <tr>
      <th>2</th>
      <td>367368170</td>
      <td>2020-01-01T00:00:03</td>
      <td>47.53785</td>
      <td>-122.32833</td>
      <td>0.6</td>
      <td>46.5</td>
      <td>141.0</td>
      <td>SONJA H</td>
      <td>NaN</td>
      <td>WDE5536</td>
      <td>31.0</td>
      <td>0.0</td>
      <td>18.0</td>
      <td>6.0</td>
      <td>NaN</td>
      <td>32.0</td>
      <td>B</td>
    </tr>
    <tr>
      <th>3</th>
      <td>367007980</td>
      <td>2020-01-01T00:00:05</td>
      <td>37.95154</td>
      <td>-121.32682</td>
      <td>0.0</td>
      <td>-49.6</td>
      <td>511.0</td>
      <td>ANGIE M BRUSCO</td>
      <td>IMO5111359</td>
      <td>WDC3446</td>
      <td>31.0</td>
      <td>0.0</td>
      <td>28.0</td>
      <td>7.0</td>
      <td>3.4</td>
      <td>NaN</td>
      <td>B</td>
    </tr>
    <tr>
      <th>4</th>
      <td>367538940</td>
      <td>2020-01-01T00:00:05</td>
      <td>30.00258</td>
      <td>-93.22608</td>
      <td>3.1</td>
      <td>168.0</td>
      <td>511.0</td>
      <td>RITA ANN</td>
      <td>NaN</td>
      <td>WDG4670</td>
      <td>31.0</td>
      <td>0.0</td>
      <td>21.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7803747</th>
      <td>636019842</td>
      <td>2020-10-01T23:59:56</td>
      <td>29.92496</td>
      <td>-89.94634</td>
      <td>0.0</td>
      <td>-145.2</td>
      <td>268.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
    </tr>
    <tr>
      <th>7803748</th>
      <td>338314788</td>
      <td>2020-10-01T23:59:57</td>
      <td>39.31591</td>
      <td>-76.40135</td>
      <td>0.1</td>
      <td>-49.6</td>
      <td>511.0</td>
      <td>DREAMTIME III</td>
      <td>IMO0000000</td>
      <td>NaN</td>
      <td>37.0</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
    </tr>
    <tr>
      <th>7803749</th>
      <td>368070720</td>
      <td>2020-10-01T23:59:59</td>
      <td>30.04614</td>
      <td>-90.66375</td>
      <td>0.0</td>
      <td>-99.2</td>
      <td>511.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
    </tr>
    <tr>
      <th>7803750</th>
      <td>368090840</td>
      <td>2020-10-01T23:59:59</td>
      <td>29.90862</td>
      <td>-90.10211</td>
      <td>2.9</td>
      <td>96.5</td>
      <td>179.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>A</td>
    </tr>
    <tr>
      <th>7803751</th>
      <td>368078690</td>
      <td>2020-10-01T23:59:59</td>
      <td>30.27919</td>
      <td>-91.20450</td>
      <td>5.3</td>
      <td>-126.7</td>
      <td>286.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>B</td>
    </tr>
  </tbody>
</table>
<p>30502798 rows × 17 columns</p>
</div>



### CMEMS
Downloading the data for the 4 days (in stage I). [DATASETS](https://resources.marine.copernicus.eu/?option=com_csw&task=results)


```python
import ftplib # import libraries
import os
from getpass import getpass

con = ftplib.FTP('nrt.cmems-du.eu') # connect to CMEMS FTP
print(con.getwelcome())

"""
name = getpass('Enter name: ') # get login from input
pwd = getpass('Enter pwd: ')
"""

name = 'avilanovacortez'
pwd = 'CMEMS-data-2021'
```

    220 Welcome to CMEMS NRT FTP service



```python
wav_url = '/Core/GLOBAL_ANALYSIS_FORECAST_WAV_001_027/global-analysis-forecast-wav-001-027/2020/' # url for WAVE products - 3-hourly, one-day-forecast
phy_url = '/Core/GLOBAL_ANALYSIS_FORECAST_PHY_001_024/global-analysis-forecast-phy-001-024/2020/' # url for PHYSICAL products

def print_collection(url): # define function for FTP exploration
  try:
    # login
    con.login(name, pwd)
    # navigate to a wave forecast product, this month
    con.cwd(url)
    # retrieve as list
    con.retrlines('LIST') 
    
  except ftplib.all_errors as e:
    print('FTP error:', e)

print_collection(wav_url + '07') # print content of url
```

    -rw-rw-r--    1 ftp      ftp      633801925 Jul 02  2020 mfwamglocep_2020070100_R20200702.nc
    -rw-rw-r--    1 ftp      ftp      631627132 Jul 03  2020 mfwamglocep_2020070200_R20200703.nc
    -rw-rw-r--    1 ftp      ftp      634470499 Jul 04  2020 mfwamglocep_2020070300_R20200704.nc
    -rw-rw-r--    1 ftp      ftp      634911437 Jul 05  2020 mfwamglocep_2020070400_R20200705.nc
    -rw-rw-r--    1 ftp      ftp      633233443 Jul 06  2020 mfwamglocep_2020070500_R20200706.nc
    -rw-rw-r--    1 ftp      ftp      634290111 Jul 07  2020 mfwamglocep_2020070600_R20200707.nc
    -rw-rw-r--    1 ftp      ftp      634594020 Jul 08  2020 mfwamglocep_2020070700_R20200708.nc
    -rw-rw-r--    1 ftp      ftp      635713877 Jul 09  2020 mfwamglocep_2020070800_R20200709.nc
    -rw-rw-r--    1 ftp      ftp      636855533 Jul 10  2020 mfwamglocep_2020070900_R20200710.nc
    -rw-rw-r--    1 ftp      ftp      639037924 Jul 11  2020 mfwamglocep_2020071000_R20200711.nc
    -rw-rw-r--    1 ftp      ftp      635226220 Jul 12  2020 mfwamglocep_2020071100_R20200712.nc
    -rw-rw-r--    1 ftp      ftp      633358568 Jul 13  2020 mfwamglocep_2020071200_R20200713.nc
    -rw-rw-r--    1 ftp      ftp      631530511 Jul 14  2020 mfwamglocep_2020071300_R20200714.nc
    -rw-rw-r--    1 ftp      ftp      629912123 Jul 15  2020 mfwamglocep_2020071400_R20200715.nc
    -rw-rw-r--    1 ftp      ftp      628150083 Jul 16  2020 mfwamglocep_2020071500_R20200716.nc
    -rw-rw-r--    1 ftp      ftp      629491286 Jul 17  2020 mfwamglocep_2020071600_R20200717.nc
    -rw-rw-r--    1 ftp      ftp      630929866 Jul 18  2020 mfwamglocep_2020071700_R20200718.nc
    -rw-rw-r--    1 ftp      ftp      630973689 Jul 19  2020 mfwamglocep_2020071800_R20200719.nc
    -rw-rw-r--    1 ftp      ftp      630963059 Jul 20  2020 mfwamglocep_2020071900_R20200720.nc
    -rw-rw-r--    1 ftp      ftp      634016423 Jul 21  2020 mfwamglocep_2020072000_R20200721.nc
    -rw-rw-r--    1 ftp      ftp      636248163 Jul 22  2020 mfwamglocep_2020072100_R20200722.nc
    -rw-rw-r--    1 ftp      ftp      637710739 Jul 23  2020 mfwamglocep_2020072200_R20200723.nc
    -rw-rw-r--    1 ftp      ftp      639017052 Jul 24  2020 mfwamglocep_2020072300_R20200724.nc
    -rw-rw-r--    1 ftp      ftp      638780989 Jul 25  2020 mfwamglocep_2020072400_R20200725.nc
    -rw-rw-r--    1 ftp      ftp      636179439 Jul 26  2020 mfwamglocep_2020072500_R20200726.nc
    -rw-rw-r--    1 ftp      ftp      636169984 Jul 27  2020 mfwamglocep_2020072600_R20200727.nc
    -rw-rw-r--    1 ftp      ftp      634458296 Jul 28  2020 mfwamglocep_2020072700_R20200728.nc
    -rw-rw-r--    1 ftp      ftp      633303458 Jul 29  2020 mfwamglocep_2020072800_R20200729.nc
    -rw-rw-r--    1 ftp      ftp      633012641 Jul 30  2020 mfwamglocep_2020072900_R20200730.nc
    -rw-rw-r--    1 ftp      ftp      633876588 Jul 31  2020 mfwamglocep_2020073000_R20200731.nc
    -rw-rw-r--    1 ftp      ftp      633677279 Aug 01  2020 mfwamglocep_2020073100_R20200801.nc



```python
wav_url = '/Core/GLOBAL_ANALYSIS_FORECAST_WAV_001_027/global-analysis-forecast-wav-001-027/2020/' # url for WAVE products - 3-hourly, one-day-forecast
phy_url = '/Core/GLOBAL_ANALYSIS_FORECAST_PHY_001_024/global-analysis-forecast-phy-001-024/2020/'

wav_jan = "mfwamglocep_2020010100_R20200102.nc"
wav_apr = "mfwamglocep_2020040100_R20200402.nc"
wav_jul = "mfwamglocep_2020070100_R20200702.nc"
wav_oct = "mfwamglocep_2020100100_R20201002.nc"

phy_jan = "mercatorpsy4v3r1_gl12_mean_20200101_R20200115.nc"
phy_apr = "mercatorpsy4v3r1_gl12_mean_20200401_R20200415.nc"
phy_jul = "mercatorpsy4v3r1_gl12_mean_20200701_R20200715.nc"
phy_oct = "mercatorpsy4v3r1_gl12_mean_20201001_R20201014.nc"

def download_ftp(url, prod_name): # function to download CMEMS FTP
  # do check if file exists, taken from source at the top
  if os.path.isfile(prod_name):
    print("There is already a local copy of {}".format(prod_name))
  else:
    try:
      # login as before
      con.login(name, pwd)
      # navigate to desired folder seen above
      con.cwd(url)
      with open(prod_name, 'wb') as fp:
        con.retrbinary('RETR {}'.format(prod_name), fp.write)
            
    except ftplib.all_errors as e:
      print('FTP error:', e)

download_ftp(wav_url + '01', wav_jan)
download_ftp(wav_url + '04', wav_apr)
download_ftp(wav_url + '07', wav_jul)
download_ftp(wav_url + '10', wav_oct)

download_ftp(phy_url + '01', phy_jan)
download_ftp(phy_url + '04', phy_apr)
download_ftp(phy_url + '07', phy_jul)
download_ftp(phy_url + '10', phy_oct)
```

## STEP 3: Merge and preprocess
For CMEMS Data:
* OK Reduce CMEMS data size by using bbox
* Normalize data and remove its outliers

For AIS Data:
* OK Reduce AIS data size by using bbox
* OK Remove ships with Status =! 0 or Status =! 8
* OK Remove ships with SOG < 5 or SOG > 102.2
* OK Remove ships with latitude > 91 and longitude > 181
* OK Remove ships with heading > 361
* WRONG Calculate Gross Tonnage
* Estimate data base times
* Normalize data (SOG) and remove its outliers

In general:
* Merge AIS data with CMEMS data
* Remove unneccessary columns (show only estimated time, latitude, longitude, heading, SOG, COG and gross tonage)


```python
import xarray as xr
import numpy as np

def get_closest(array, value):
    return np.abs(array - value).argmin()

ds_wav_jan = xr.open_dataset(wav_jan)
ds_wav_apr = xr.open_dataset(wav_apr)
ds_wav_jul = xr.open_dataset(wav_jul)
#ds_wav_oct = xr.open_dataset(wav_oct)

ds_wav_all = [ds_wav_jan, ds_wav_apr, ds_wav_jul]
#ds_wav_all = [ds_wav_jan, ds_wav_apr, ds_wav_jul, ds_wav_oct]
datasets_wav = []

for ds_month in ds_wav_all:
  
  lon_min = get_closest(ds_month.longitude.data, bbox[0][0])
  lon_max = get_closest(ds_month.longitude.data, bbox[1][0])
  lat_min = get_closest(ds_month.latitude.data, bbox[0][1])
  lat_max = get_closest(ds_month.latitude.data, bbox[1][1])

  ds_wav_month_reg = ds_month.isel(time = 0, longitude = slice(lon_min, lon_max), latitude = slice(lat_min, lat_max))
  datasets_wav.append(ds_wav_month_reg)

ds_wav = xr.concat(datasets_wav, dim = 'ds_month')
ds_wav
```




<div><svg style="position: absolute; width: 0; height: 0; overflow: hidden">
<defs>
<symbol id="icon-database" viewBox="0 0 32 32">
<path d="M16 0c-8.837 0-16 2.239-16 5v4c0 2.761 7.163 5 16 5s16-2.239 16-5v-4c0-2.761-7.163-5-16-5z"></path>
<path d="M16 17c-8.837 0-16-2.239-16-5v6c0 2.761 7.163 5 16 5s16-2.239 16-5v-6c0 2.761-7.163 5-16 5z"></path>
<path d="M16 26c-8.837 0-16-2.239-16-5v6c0 2.761 7.163 5 16 5s16-2.239 16-5v-6c0 2.761-7.163 5-16 5z"></path>
</symbol>
<symbol id="icon-file-text2" viewBox="0 0 32 32">
<path d="M28.681 7.159c-0.694-0.947-1.662-2.053-2.724-3.116s-2.169-2.030-3.116-2.724c-1.612-1.182-2.393-1.319-2.841-1.319h-15.5c-1.378 0-2.5 1.121-2.5 2.5v27c0 1.378 1.122 2.5 2.5 2.5h23c1.378 0 2.5-1.122 2.5-2.5v-19.5c0-0.448-0.137-1.23-1.319-2.841zM24.543 5.457c0.959 0.959 1.712 1.825 2.268 2.543h-4.811v-4.811c0.718 0.556 1.584 1.309 2.543 2.268zM28 29.5c0 0.271-0.229 0.5-0.5 0.5h-23c-0.271 0-0.5-0.229-0.5-0.5v-27c0-0.271 0.229-0.5 0.5-0.5 0 0 15.499-0 15.5 0v7c0 0.552 0.448 1 1 1h7v19.5z"></path>
<path d="M23 26h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
<path d="M23 22h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
<path d="M23 18h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
</symbol>
</defs>
</svg>
<style>/* CSS stylesheet for displaying xarray objects in jupyterlab.
 *
 */

:root {
  --xr-font-color0: var(--jp-content-font-color0, rgba(0, 0, 0, 1));
  --xr-font-color2: var(--jp-content-font-color2, rgba(0, 0, 0, 0.54));
  --xr-font-color3: var(--jp-content-font-color3, rgba(0, 0, 0, 0.38));
  --xr-border-color: var(--jp-border-color2, #e0e0e0);
  --xr-disabled-color: var(--jp-layout-color3, #bdbdbd);
  --xr-background-color: var(--jp-layout-color0, white);
  --xr-background-color-row-even: var(--jp-layout-color1, white);
  --xr-background-color-row-odd: var(--jp-layout-color2, #eeeeee);
}

html[theme=dark],
body.vscode-dark {
  --xr-font-color0: rgba(255, 255, 255, 1);
  --xr-font-color2: rgba(255, 255, 255, 0.54);
  --xr-font-color3: rgba(255, 255, 255, 0.38);
  --xr-border-color: #1F1F1F;
  --xr-disabled-color: #515151;
  --xr-background-color: #111111;
  --xr-background-color-row-even: #111111;
  --xr-background-color-row-odd: #313131;
}

.xr-wrap {
  display: block;
  min-width: 300px;
  max-width: 700px;
}

.xr-text-repr-fallback {
  /* fallback to plain text repr when CSS is not injected (untrusted notebook) */
  display: none;
}

.xr-header {
  padding-top: 6px;
  padding-bottom: 6px;
  margin-bottom: 4px;
  border-bottom: solid 1px var(--xr-border-color);
}

.xr-header > div,
.xr-header > ul {
  display: inline;
  margin-top: 0;
  margin-bottom: 0;
}

.xr-obj-type,
.xr-array-name {
  margin-left: 2px;
  margin-right: 10px;
}

.xr-obj-type {
  color: var(--xr-font-color2);
}

.xr-sections {
  padding-left: 0 !important;
  display: grid;
  grid-template-columns: 150px auto auto 1fr 20px 20px;
}

.xr-section-item {
  display: contents;
}

.xr-section-item input {
  display: none;
}

.xr-section-item input + label {
  color: var(--xr-disabled-color);
}

.xr-section-item input:enabled + label {
  cursor: pointer;
  color: var(--xr-font-color2);
}

.xr-section-item input:enabled + label:hover {
  color: var(--xr-font-color0);
}

.xr-section-summary {
  grid-column: 1;
  color: var(--xr-font-color2);
  font-weight: 500;
}

.xr-section-summary > span {
  display: inline-block;
  padding-left: 0.5em;
}

.xr-section-summary-in:disabled + label {
  color: var(--xr-font-color2);
}

.xr-section-summary-in + label:before {
  display: inline-block;
  content: '►';
  font-size: 11px;
  width: 15px;
  text-align: center;
}

.xr-section-summary-in:disabled + label:before {
  color: var(--xr-disabled-color);
}

.xr-section-summary-in:checked + label:before {
  content: '▼';
}

.xr-section-summary-in:checked + label > span {
  display: none;
}

.xr-section-summary,
.xr-section-inline-details {
  padding-top: 4px;
  padding-bottom: 4px;
}

.xr-section-inline-details {
  grid-column: 2 / -1;
}

.xr-section-details {
  display: none;
  grid-column: 1 / -1;
  margin-bottom: 5px;
}

.xr-section-summary-in:checked ~ .xr-section-details {
  display: contents;
}

.xr-array-wrap {
  grid-column: 1 / -1;
  display: grid;
  grid-template-columns: 20px auto;
}

.xr-array-wrap > label {
  grid-column: 1;
  vertical-align: top;
}

.xr-preview {
  color: var(--xr-font-color3);
}

.xr-array-preview,
.xr-array-data {
  padding: 0 5px !important;
  grid-column: 2;
}

.xr-array-data,
.xr-array-in:checked ~ .xr-array-preview {
  display: none;
}

.xr-array-in:checked ~ .xr-array-data,
.xr-array-preview {
  display: inline-block;
}

.xr-dim-list {
  display: inline-block !important;
  list-style: none;
  padding: 0 !important;
  margin: 0;
}

.xr-dim-list li {
  display: inline-block;
  padding: 0;
  margin: 0;
}

.xr-dim-list:before {
  content: '(';
}

.xr-dim-list:after {
  content: ')';
}

.xr-dim-list li:not(:last-child):after {
  content: ',';
  padding-right: 5px;
}

.xr-has-index {
  font-weight: bold;
}

.xr-var-list,
.xr-var-item {
  display: contents;
}

.xr-var-item > div,
.xr-var-item label,
.xr-var-item > .xr-var-name span {
  background-color: var(--xr-background-color-row-even);
  margin-bottom: 0;
}

.xr-var-item > .xr-var-name:hover span {
  padding-right: 5px;
}

.xr-var-list > li:nth-child(odd) > div,
.xr-var-list > li:nth-child(odd) > label,
.xr-var-list > li:nth-child(odd) > .xr-var-name span {
  background-color: var(--xr-background-color-row-odd);
}

.xr-var-name {
  grid-column: 1;
}

.xr-var-dims {
  grid-column: 2;
}

.xr-var-dtype {
  grid-column: 3;
  text-align: right;
  color: var(--xr-font-color2);
}

.xr-var-preview {
  grid-column: 4;
}

.xr-var-name,
.xr-var-dims,
.xr-var-dtype,
.xr-preview,
.xr-attrs dt {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  padding-right: 10px;
}

.xr-var-name:hover,
.xr-var-dims:hover,
.xr-var-dtype:hover,
.xr-attrs dt:hover {
  overflow: visible;
  width: auto;
  z-index: 1;
}

.xr-var-attrs,
.xr-var-data {
  display: none;
  background-color: var(--xr-background-color) !important;
  padding-bottom: 5px !important;
}

.xr-var-attrs-in:checked ~ .xr-var-attrs,
.xr-var-data-in:checked ~ .xr-var-data {
  display: block;
}

.xr-var-data > table {
  float: right;
}

.xr-var-name span,
.xr-var-data,
.xr-attrs {
  padding-left: 25px !important;
}

.xr-attrs,
.xr-var-attrs,
.xr-var-data {
  grid-column: 1 / -1;
}

dl.xr-attrs {
  padding: 0;
  margin: 0;
  display: grid;
  grid-template-columns: 125px auto;
}

.xr-attrs dt,
.xr-attrs dd {
  padding: 0;
  margin: 0;
  float: left;
  padding-right: 10px;
  width: auto;
}

.xr-attrs dt {
  font-weight: normal;
  grid-column: 1;
}

.xr-attrs dt:hover span {
  display: inline-block;
  background: var(--xr-background-color);
  padding-right: 10px;
}

.xr-attrs dd {
  grid-column: 2;
  white-space: pre-wrap;
  word-break: break-all;
}

.xr-icon-database,
.xr-icon-file-text2 {
  display: inline-block;
  vertical-align: middle;
  width: 1em;
  height: 1.5em !important;
  stroke-width: 0;
  stroke: currentColor;
  fill: currentColor;
}
</style><pre class='xr-text-repr-fallback'>&lt;xarray.Dataset&gt;
Dimensions:    (ds_month: 3, latitude: 132, longitude: 240)
Coordinates:
  * longitude  (longitude) float64 10.0 10.08 10.17 10.25 ... 29.75 29.83 29.92
  * latitude   (latitude) float64 54.0 54.08 54.17 54.25 ... 64.75 64.83 64.92
    time       (ds_month) datetime64[ns] 2020-01-01T03:00:00 ... 2020-07-01T0...
Dimensions without coordinates: ds_month
Data variables: (12/17)
    VHM0       (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    VMDR_WW    (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    VHM0_WW    (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    VMDR_SW1   (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    VTM01_SW1  (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    VMDR_SW2   (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    ...         ...
    VTPK       (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    VSDX       (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    VSDY       (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    VPED       (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    VTM02      (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    VTM01_WW   (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
Attributes: (12/27)
    Conventions:                   CF-1.6
    time_coverage_start:           20200101-03:00:00
    time_coverage_end:             20200102-00:00:00
    date_created:                  20200102-06:22:00
    product_type:                  hindcast
    product:                       GLOBAL_ANALYSIS_FORECAST_WAV_001_027
    ...                            ...
    geospatial_lon_step:           0.08332825
    geospatial_lon_units:          degree
    geospatial_lat_min:            -80.0
    geospatial_lat_max:            90.0
    geospatial_lat_step:           0.08333588
    geospatial_lat_units:          degree</pre><div class='xr-wrap' hidden><div class='xr-header'><div class='xr-obj-type'>xarray.Dataset</div></div><ul class='xr-sections'><li class='xr-section-item'><input id='section-66e95564-9ea9-4203-84e1-f5fe41c7b831' class='xr-section-summary-in' type='checkbox' disabled ><label for='section-66e95564-9ea9-4203-84e1-f5fe41c7b831' class='xr-section-summary'  title='Expand/collapse section'>Dimensions:</label><div class='xr-section-inline-details'><ul class='xr-dim-list'><li><span>ds_month</span>: 3</li><li><span class='xr-has-index'>latitude</span>: 132</li><li><span class='xr-has-index'>longitude</span>: 240</li></ul></div><div class='xr-section-details'></div></li><li class='xr-section-item'><input id='section-ce7c74d1-a129-4693-983f-0fb7c723ee4b' class='xr-section-summary-in' type='checkbox'  checked><label for='section-ce7c74d1-a129-4693-983f-0fb7c723ee4b' class='xr-section-summary' >Coordinates: <span>(3)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><ul class='xr-var-list'><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>longitude</span></div><div class='xr-var-dims'>(longitude)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>10.0 10.08 10.17 ... 29.83 29.92</div><input id='attrs-41d90b14-b5d3-4a9c-b7e9-d5ab8b656449' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-41d90b14-b5d3-4a9c-b7e9-d5ab8b656449' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-a3217492-c203-4f62-a3af-902c16e9729f' class='xr-var-data-in' type='checkbox'><label for='data-a3217492-c203-4f62-a3af-902c16e9729f' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>standard_name :</span></dt><dd>longitude</dd><dt><span>long_name :</span></dt><dd>longitude coordinate</dd><dt><span>units :</span></dt><dd>degrees_east</dd><dt><span>axis :</span></dt><dd>X</dd><dt><span>step :</span></dt><dd>0.08332825</dd></dl></div><div class='xr-var-data'><pre>array([10.      , 10.083333, 10.166667, ..., 29.75    , 29.833333, 29.916667])</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>latitude</span></div><div class='xr-var-dims'>(latitude)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>54.0 54.08 54.17 ... 64.83 64.92</div><input id='attrs-1af5c976-e98b-4921-8a35-4919146dd688' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-1af5c976-e98b-4921-8a35-4919146dd688' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-5fc7670d-8f8b-4ec7-9d69-ae6b8d8e0294' class='xr-var-data-in' type='checkbox'><label for='data-5fc7670d-8f8b-4ec7-9d69-ae6b8d8e0294' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>standard_name :</span></dt><dd>latitude</dd><dt><span>long_name :</span></dt><dd>latitude coordinate</dd><dt><span>units :</span></dt><dd>degrees_north</dd><dt><span>axis :</span></dt><dd>Y</dd><dt><span>step :</span></dt><dd>0.08333588</dd></dl></div><div class='xr-var-data'><pre>array([54.      , 54.083333, 54.166667, 54.25    , 54.333333, 54.416667,
       54.5     , 54.583333, 54.666667, 54.75    , 54.833333, 54.916667,
       55.      , 55.083333, 55.166667, 55.25    , 55.333333, 55.416667,
       55.5     , 55.583333, 55.666667, 55.75    , 55.833333, 55.916667,
       56.      , 56.083333, 56.166667, 56.25    , 56.333333, 56.416667,
       56.5     , 56.583333, 56.666667, 56.75    , 56.833333, 56.916667,
       57.      , 57.083333, 57.166667, 57.25    , 57.333333, 57.416667,
       57.5     , 57.583333, 57.666667, 57.75    , 57.833333, 57.916667,
       58.      , 58.083333, 58.166667, 58.25    , 58.333333, 58.416667,
       58.5     , 58.583333, 58.666667, 58.75    , 58.833333, 58.916667,
       59.      , 59.083333, 59.166667, 59.25    , 59.333333, 59.416667,
       59.5     , 59.583333, 59.666667, 59.75    , 59.833333, 59.916667,
       60.      , 60.083333, 60.166667, 60.25    , 60.333333, 60.416667,
       60.5     , 60.583333, 60.666667, 60.75    , 60.833333, 60.916667,
       61.      , 61.083333, 61.166667, 61.25    , 61.333333, 61.416667,
       61.5     , 61.583333, 61.666667, 61.75    , 61.833333, 61.916667,
       62.      , 62.083333, 62.166667, 62.25    , 62.333333, 62.416667,
       62.5     , 62.583333, 62.666667, 62.75    , 62.833333, 62.916667,
       63.      , 63.083333, 63.166667, 63.25    , 63.333333, 63.416667,
       63.5     , 63.583333, 63.666667, 63.75    , 63.833333, 63.916667,
       64.      , 64.083333, 64.166667, 64.25    , 64.333333, 64.416667,
       64.5     , 64.583333, 64.666667, 64.75    , 64.833333, 64.916667])</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>time</span></div><div class='xr-var-dims'>(ds_month)</div><div class='xr-var-dtype'>datetime64[ns]</div><div class='xr-var-preview xr-preview'>2020-01-01T03:00:00 ... 2020-07-...</div><input id='attrs-3d5d080a-b2c2-474e-800a-9b0141e550b9' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-3d5d080a-b2c2-474e-800a-9b0141e550b9' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-a47b6bce-18dd-46a7-8f05-460828915795' class='xr-var-data-in' type='checkbox'><label for='data-a47b6bce-18dd-46a7-8f05-460828915795' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>standard_name :</span></dt><dd>time</dd><dt><span>long_name :</span></dt><dd>time</dd><dt><span>axis :</span></dt><dd>T</dd><dt><span>step :</span></dt><dd>3</dd></dl></div><div class='xr-var-data'><pre>array([&#x27;2020-01-01T03:00:00.000000000&#x27;, &#x27;2020-04-01T03:00:00.000000000&#x27;,
       &#x27;2020-07-01T03:00:00.000000000&#x27;], dtype=&#x27;datetime64[ns]&#x27;)</pre></div></li></ul></div></li><li class='xr-section-item'><input id='section-41cf364c-208e-4ea9-9536-865ae4464cca' class='xr-section-summary-in' type='checkbox'  ><label for='section-41cf364c-208e-4ea9-9536-865ae4464cca' class='xr-section-summary' >Data variables: <span>(17)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><ul class='xr-var-list'><li class='xr-var-item'><div class='xr-var-name'><span>VHM0</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-d62323f0-3ccd-4e8f-9a53-b691adde6ff8' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-d62323f0-3ccd-4e8f-9a53-b691adde6ff8' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-e3255dc3-1c9a-4970-9f10-198b9fcc50a0' class='xr-var-data-in' type='checkbox'><label for='data-e3255dc3-1c9a-4970-9f10-198b9fcc50a0' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Spectral significant wave height (Hm0)</dd><dt><span>units :</span></dt><dd>m</dd><dt><span>standard_name :</span></dt><dd>sea_surface_wave_significant_height</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>100</dd></dl></div><div class='xr-var-data'><pre>array([[[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [5.48     , 5.47     , 5.44     , ...,       nan,       nan,
               nan],
        [5.45     , 5.44     , 5.4      , ...,       nan,       nan,
               nan],
        [5.4      , 5.39     , 5.3599997, ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
...
        [4.5899997, 4.5899997, 4.5699997, ...,       nan,       nan,
               nan],
        [4.5899997, 4.6      , 4.5699997, ...,       nan,       nan,
               nan],
        [4.6      , 4.61     , 4.6      , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [2.54     , 2.52     , 2.48     , ...,       nan,       nan,
               nan],
        [2.54     , 2.53     , 2.5      , ...,       nan,       nan,
               nan],
        [2.54     , 2.53     , 2.51     , ...,       nan,       nan,
               nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VMDR_WW</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-e1d81acf-5984-42f8-903d-c89fcff4bff8' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-e1d81acf-5984-42f8-903d-c89fcff4bff8' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-e45b99d7-d3f7-4e84-b61c-44e1eff5df11' class='xr-var-data-in' type='checkbox'><label for='data-e45b99d7-d3f7-4e84-b61c-44e1eff5df11' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Mean wind wave direction from</dd><dt><span>units :</span></dt><dd>degree</dd><dt><span>standard_name :</span></dt><dd>sea_surface_wind_wave_from_direction</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>101</dd></dl></div><div class='xr-var-data'><pre>array([[[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [254.53   , 254.98999, 255.32   , ...,       nan,       nan,
               nan],
        [254.23   , 254.64   , 254.92   , ...,       nan,       nan,
               nan],
        [254.07   , 254.39   , 254.73   , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
...
        [269.68   , 272.91998, 276.91   , ...,       nan,       nan,
               nan],
        [273.58   , 276.7    , 280.32   , ...,       nan,       nan,
               nan],
        [276.1    , 279.68   , 283.09   , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [339.27   , 339.37   , 339.91   , ...,       nan,       nan,
               nan],
        [339.49   , 339.83002, 340.14   , ...,       nan,       nan,
               nan],
        [339.78   , 340.84   , 341.11   , ...,       nan,       nan,
               nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VHM0_WW</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-4001edc8-eb44-485c-9a4f-7045a91e3fb4' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-4001edc8-eb44-485c-9a4f-7045a91e3fb4' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-8c841ecd-86a2-415f-8a88-c160ba32195d' class='xr-var-data-in' type='checkbox'><label for='data-8c841ecd-86a2-415f-8a88-c160ba32195d' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Spectral significant wind wave height</dd><dt><span>units :</span></dt><dd>m</dd><dt><span>standard_name :</span></dt><dd>sea_surface_wind_wave_significant_height</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>102</dd></dl></div><div class='xr-var-data'><pre>array([[[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [5.22      , 5.21      , 5.19      , ...,        nan,
                nan,        nan],
        [5.16      , 5.16      , 5.12      , ...,        nan,
                nan,        nan],
        [5.08      , 5.08      , 5.06      , ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
...
        [0.35      , 0.34      , 0.34      , ...,        nan,
                nan,        nan],
        [0.32999998, 0.32      , 0.32      , ...,        nan,
                nan,        nan],
        [0.31      , 0.29999998, 0.29999998, ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [1.12      , 1.15      , 1.2099999 , ...,        nan,
                nan,        nan],
        [1.16      , 1.1999999 , 1.24      , ...,        nan,
                nan,        nan],
        [1.2099999 , 1.25      , 1.3       , ...,        nan,
                nan,        nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VMDR_SW1</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-3d620559-e2ed-4efa-a0fe-d376d9fa4e74' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-3d620559-e2ed-4efa-a0fe-d376d9fa4e74' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-d1ce9989-d285-43fb-87a8-f2dd9c6ceccc' class='xr-var-data-in' type='checkbox'><label for='data-d1ce9989-d285-43fb-87a8-f2dd9c6ceccc' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Mean primary swell wave direction from</dd><dt><span>units :</span></dt><dd>degree</dd><dt><span>standard_name :</span></dt><dd>sea_surface_primary_swell_wave_from_direction</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>107</dd></dl></div><div class='xr-var-data'><pre>array([[[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [317.82   , 312.97   , 312.07   , ...,       nan,       nan,
               nan],
        [320.14   , 314.11   , 312.43   , ...,       nan,       nan,
               nan],
        [318.84   , 314.32   , 312.65   , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
...
        [314.56   , 312.76   , 311.28   , ...,       nan,       nan,
               nan],
        [311.26   , 309.57   , 309.24   , ...,       nan,       nan,
               nan],
        [309.21   , 309.22   , 309.2    , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [358.39   , 357.72998, 356.86   , ...,       nan,       nan,
               nan],
        [359.28   , 358.56   , 357.84998, ...,       nan,       nan,
               nan],
        [299.94   , 349.26   , 358.7    , ...,       nan,       nan,
               nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VTM01_SW1</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-5bc8775f-aa22-4a92-bb49-cc3c2b5e1985' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-5bc8775f-aa22-4a92-bb49-cc3c2b5e1985' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-0e11e48b-f72c-4178-99ae-001d8e2d8bb4' class='xr-var-data-in' type='checkbox'><label for='data-0e11e48b-f72c-4178-99ae-001d8e2d8bb4' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Spectral moments (0,1) primary swell wave period</dd><dt><span>units :</span></dt><dd>s</dd><dt><span>standard_name :</span></dt><dd>sea_surface_primary_swell_wave_mean_period</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>226</dd></dl></div><div class='xr-var-data'><pre>array([[[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [8.38     , 8.84     , 8.94     , ...,       nan,       nan,
               nan],
        [8.17     , 8.73     , 8.91     , ...,       nan,       nan,
               nan],
        [8.429999 , 8.78     , 8.9      , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
...
        [9.07     , 9.04     , 9.01     , ...,       nan,       nan,
               nan],
        [8.98     , 8.95     , 8.94     , ...,       nan,       nan,
               nan],
        [8.92     , 8.92     , 8.929999 , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [7.1099997, 7.1299996, 7.17     , ...,       nan,       nan,
               nan],
        [7.1099997, 7.12     , 7.16     , ...,       nan,       nan,
               nan],
        [7.0899997, 7.12     , 7.16     , ...,       nan,       nan,
               nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VMDR_SW2</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-95886b37-bc79-46db-bdbf-e75ae6ece4c5' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-95886b37-bc79-46db-bdbf-e75ae6ece4c5' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-83004b97-3ec0-43ab-a00f-b9574f39ffb6' class='xr-var-data-in' type='checkbox'><label for='data-83004b97-3ec0-43ab-a00f-b9574f39ffb6' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Mean secondary swell wave direction from</dd><dt><span>units :</span></dt><dd>degree</dd><dt><span>standard_name :</span></dt><dd>sea_surface_secondary_swell_wave_from_direction</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>109</dd></dl></div><div class='xr-var-data'><pre>array([[[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [296.11   , 296.18   , 296.36   , ...,       nan,       nan,
               nan],
        [296.14   , 296.24   , 296.41998, ...,       nan,       nan,
               nan],
        [285.5    , 291.46   , 295.24   , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
...
        [230.15   , 215.14   , 200.77   , ...,       nan,       nan,
               nan],
        [197.99   , 183.     , 180.     , ...,       nan,       nan,
               nan],
        [180.     , 180.     , 180.     , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [281.97   , 287.65   , 288.76   , ...,       nan,       nan,
               nan],
        [287.51   , 288.53   , 288.75   , ...,       nan,       nan,
               nan],
        [288.72998, 288.66998, 288.89   , ...,       nan,       nan,
               nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VTM01_SW2</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-097baee6-e30d-46cb-91e9-22edef28079d' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-097baee6-e30d-46cb-91e9-22edef28079d' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-d407f51f-90d0-42a9-95f3-5380a431fe01' class='xr-var-data-in' type='checkbox'><label for='data-d407f51f-90d0-42a9-95f3-5380a431fe01' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Spectral moments (0,1) secondary swell wave period</dd><dt><span>units :</span></dt><dd>s</dd><dt><span>standard_name :</span></dt><dd>sea_surface_secondary_swell_wave_mean_period</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>227</dd></dl></div><div class='xr-var-data'><pre>array([[[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [11.55     , 11.559999 , 11.57     , ...,        nan,
                nan,        nan],
        [11.559999 , 11.559999 , 11.57     , ...,        nan,
                nan,        nan],
        [11.559999 , 11.57     , 11.58     , ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
...
        [ 5.5699997,  4.04     ,  2.76     , ...,        nan,
                nan,        nan],
        [ 2.6      ,  1.13     ,  0.84     , ...,        nan,
                nan,        nan],
        [ 0.84     ,  0.84     ,  0.84     , ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [ 4.98     ,  4.6      ,  4.54     , ...,        nan,
                nan,        nan],
        [ 4.37     ,  4.49     ,  4.58     , ...,        nan,
                nan,        nan],
        [ 4.5099998,  4.5099998,  4.6      , ...,        nan,
                nan,        nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VMDR</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-859c6d1b-66cc-4326-81e4-6980ad83ad89' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-859c6d1b-66cc-4326-81e4-6980ad83ad89' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-47fddf2f-5d7a-4c88-a7ad-4c2daf9d2f18' class='xr-var-data-in' type='checkbox'><label for='data-47fddf2f-5d7a-4c88-a7ad-4c2daf9d2f18' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Mean wave direction from (Mdir)</dd><dt><span>units :</span></dt><dd>degree</dd><dt><span>standard_name :</span></dt><dd>sea_surface_wave_from_direction</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>200</dd></dl></div><div class='xr-var-data'><pre>array([[[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [259.     , 259.62   , 260.04   , ...,       nan,       nan,
               nan],
        [258.72998, 259.33002, 259.75   , ...,       nan,       nan,
               nan],
        [258.54   , 259.09   , 259.62   , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
...
        [309.18   , 309.11   , 309.03998, ...,       nan,       nan,
               nan],
        [309.12   , 309.09   , 309.09998, ...,       nan,       nan,
               nan],
        [309.07   , 309.09998, 309.09998, ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [353.64   , 352.77   , 351.71   , ...,       nan,       nan,
               nan],
        [353.95   , 353.18   , 352.22998, ...,       nan,       nan,
               nan],
        [354.22   , 353.52   , 352.72   , ...,       nan,       nan,
               nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VTM10</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-3f246209-acf6-4bcd-b264-645aac4ed7f7' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-3f246209-acf6-4bcd-b264-645aac4ed7f7' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-35978ed3-79c1-43dd-844c-9848afb6a083' class='xr-var-data-in' type='checkbox'><label for='data-35978ed3-79c1-43dd-844c-9848afb6a083' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Spectral moments (-1,0) wave period (Tm-10)</dd><dt><span>units :</span></dt><dd>s</dd><dt><span>standard_name :</span></dt><dd>sea_surface_wave_mean_period_from_variance_spectral_density_inverse_frequency_moment</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>201</dd></dl></div><div class='xr-var-data'><pre>array([[[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [ 9.469999 ,  9.44     ,  9.4      , ...,        nan,
                nan,        nan],
        [ 9.46     ,  9.429999 ,  9.389999 , ...,        nan,
                nan,        nan],
        [ 9.429999 ,  9.41     ,  9.38     , ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
...
        [10.639999 , 10.65     , 10.65     , ...,        nan,
                nan,        nan],
        [10.63     , 10.639999 , 10.639999 , ...,        nan,
                nan,        nan],
        [10.62     , 10.62     , 10.63     , ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [ 7.69     ,  7.66     ,  7.62     , ...,        nan,
                nan,        nan],
        [ 7.6499996,  7.6299996,  7.5899997, ...,        nan,
                nan,        nan],
        [ 7.6099997,  7.58     ,  7.5499997, ...,        nan,
                nan,        nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VHM0_SW1</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-62ec9d91-a7f9-4631-82f8-7e1103b617de' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-62ec9d91-a7f9-4631-82f8-7e1103b617de' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-1e998427-a83f-4064-a02d-dc870ba91c09' class='xr-var-data-in' type='checkbox'><label for='data-1e998427-a83f-4064-a02d-dc870ba91c09' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Spectral significant primary swell wave height</dd><dt><span>units :</span></dt><dd>m</dd><dt><span>standard_name :</span></dt><dd>sea_surface_primary_swell_wave_significant_height</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>202</dd></dl></div><div class='xr-var-data'><pre>array([[[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [0.87      , 0.66999996, 0.64      , ...,        nan,
                nan,        nan],
        [0.96999997, 0.71999997, 0.65      , ...,        nan,
                nan,        nan],
        [0.96999997, 0.76      , 0.66999996, ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
...
        [4.36      , 4.44      , 4.47      , ...,        nan,
                nan,        nan],
        [4.5       , 4.5699997 , 4.56      , ...,        nan,
                nan,        nan],
        [4.5899997 , 4.6       , 4.5899997 , ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [2.25      , 2.21      , 2.1399999 , ...,        nan,
                nan,        nan],
        [2.23      , 2.2       , 2.1299999 , ...,        nan,
                nan,        nan],
        [2.21      , 2.1699998 , 2.12      , ...,        nan,
                nan,        nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VHM0_SW2</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-5a598cc2-92c9-4826-ba07-c3af0da4247e' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-5a598cc2-92c9-4826-ba07-c3af0da4247e' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-9c68561c-a88d-40e6-ac32-8480d3f1432b' class='xr-var-data-in' type='checkbox'><label for='data-9c68561c-a88d-40e6-ac32-8480d3f1432b' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Spectral significant secondary swell wave height</dd><dt><span>units :</span></dt><dd>m</dd><dt><span>standard_name :</span></dt><dd>sea_surface_secondary_swell_wave_significant_height</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>203</dd></dl></div><div class='xr-var-data'><pre>array([[[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [0.55      , 0.55      , 0.55      , ...,        nan,
                nan,        nan],
        [0.53      , 0.53      , 0.53      , ...,        nan,
                nan,        nan],
        [0.5       , 0.51      , 0.51      , ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
...
        [1.13      , 0.76      , 0.45999998, ...,        nan,
                nan,        nan],
        [0.42      , 0.07      , 0.        , ...,        nan,
                nan,        nan],
        [0.        , 0.        , 0.        , ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [0.24      , 0.22999999, 0.24      , ...,        nan,
                nan,        nan],
        [0.25      , 0.24      , 0.24      , ...,        nan,
                nan,        nan],
        [0.22999999, 0.22999999, 0.24      , ...,        nan,
                nan,        nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VTPK</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-6f88c713-f549-41c9-9f41-b94d05b9ea1e' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-6f88c713-f549-41c9-9f41-b94d05b9ea1e' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-37e99172-469d-40ca-95a1-1be46ed214ee' class='xr-var-data-in' type='checkbox'><label for='data-37e99172-469d-40ca-95a1-1be46ed214ee' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Wave period at spectral peak / peak period (Tp)</dd><dt><span>units :</span></dt><dd>s</dd><dt><span>standard_name :</span></dt><dd>sea_surface_wave_period_at_variance_spectral_density_maximum</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>204</dd></dl></div><div class='xr-var-data'><pre>array([[[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [10.78     , 10.76     , 10.73     , ...,        nan,
                nan,        nan],
        [10.7699995, 10.76     , 10.73     , ...,        nan,
                nan,        nan],
        [10.76     , 10.74     , 10.719999 , ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
...
        [12.13     , 12.139999 , 12.139999 , ...,        nan,
                nan,        nan],
        [12.12     , 12.13     , 12.139999 , ...,        nan,
                nan,        nan],
        [12.12     , 12.12     , 12.13     , ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [ 8.96     ,  8.95     ,  8.92     , ...,        nan,
                nan,        nan],
        [ 8.929999 ,  8.91     ,  8.889999 , ...,        nan,
                nan,        nan],
        [ 8.889999 ,  8.87     ,  8.86     , ...,        nan,
                nan,        nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VSDX</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-f8d37f7a-7f5d-416d-b1fc-927d3e201aba' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f8d37f7a-7f5d-416d-b1fc-927d3e201aba' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-75e1e21c-38fa-4833-9fad-b1c74f5cd712' class='xr-var-data-in' type='checkbox'><label for='data-75e1e21c-38fa-4833-9fad-b1c74f5cd712' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Stokes drift U</dd><dt><span>units :</span></dt><dd>m s-1</dd><dt><span>standard_name :</span></dt><dd>sea_surface_wave_stokes_drift_x_velocity</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>215</dd></dl></div><div class='xr-var-data'><pre>array([[[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [0.25      , 0.25      , 0.25      , ...,        nan,
                nan,        nan],
        [0.25      , 0.25      , 0.25      , ...,        nan,
                nan,        nan],
        [0.24      , 0.24      , 0.24      , ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
...
        [0.09999999, 0.09999999, 0.09999999, ...,        nan,
                nan,        nan],
        [0.09999999, 0.09999999, 0.09999999, ...,        nan,
                nan,        nan],
        [0.09999999, 0.09999999, 0.09999999, ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [0.03      , 0.03      , 0.04      , ...,        nan,
                nan,        nan],
        [0.03      , 0.03      , 0.04      , ...,        nan,
                nan,        nan],
        [0.03      , 0.03      , 0.04      , ...,        nan,
                nan,        nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VSDY</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-0bdf7d7b-e106-43e1-b597-cb9ec0145ceb' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-0bdf7d7b-e106-43e1-b597-cb9ec0145ceb' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-c6ae28ae-f0bb-4c51-ba26-06344994c6ae' class='xr-var-data-in' type='checkbox'><label for='data-c6ae28ae-f0bb-4c51-ba26-06344994c6ae' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Stokes drift V</dd><dt><span>units :</span></dt><dd>m s-1</dd><dt><span>standard_name :</span></dt><dd>sea_surface_wave_stokes_drift_y_velocity</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>216</dd></dl></div><div class='xr-var-data'><pre>array([[[  nan,   nan,   nan, ...,   nan,   nan,   nan],
        [  nan,   nan,   nan, ...,   nan,   nan,   nan],
        [  nan,   nan,   nan, ...,   nan,   nan,   nan],
        ...,
        [ 0.07,  0.07,  0.07, ...,   nan,   nan,   nan],
        [ 0.07,  0.07,  0.07, ...,   nan,   nan,   nan],
        [ 0.06,  0.06,  0.06, ...,   nan,   nan,   nan]],

       [[  nan,   nan,   nan, ...,   nan,   nan,   nan],
        [  nan,   nan,   nan, ...,   nan,   nan,   nan],
        [  nan,   nan,   nan, ...,   nan,   nan,   nan],
        ...,
        [-0.07, -0.07, -0.07, ...,   nan,   nan,   nan],
        [-0.07, -0.07, -0.08, ...,   nan,   nan,   nan],
        [-0.08, -0.08, -0.08, ...,   nan,   nan,   nan]],

       [[  nan,   nan,   nan, ...,   nan,   nan,   nan],
        [  nan,   nan,   nan, ...,   nan,   nan,   nan],
        [  nan,   nan,   nan, ...,   nan,   nan,   nan],
        ...,
        [-0.12, -0.12, -0.12, ...,   nan,   nan,   nan],
        [-0.12, -0.12, -0.13, ...,   nan,   nan,   nan],
        [-0.12, -0.13, -0.13, ...,   nan,   nan,   nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VPED</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-bc4cc15d-ae2c-4edb-9e20-0ad376f400a0' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-bc4cc15d-ae2c-4edb-9e20-0ad376f400a0' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-3a2abc81-523e-4fc8-acf4-241cc5e0041f' class='xr-var-data-in' type='checkbox'><label for='data-3a2abc81-523e-4fc8-acf4-241cc5e0041f' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Wave principal direction at spectral peak</dd><dt><span>units :</span></dt><dd>degree</dd><dt><span>standard_name :</span></dt><dd>sea_surface_wave_from_direction_at_variance_spectral_density_maximum</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>[]</dd></dl></div><div class='xr-var-data'><pre>array([[[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [262.5     , 262.5     , 262.5     , ...,        nan,
                nan,        nan],
        [262.5     , 262.5     , 262.5     , ...,        nan,
                nan,        nan],
        [262.5     , 262.5     , 262.5     , ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
...
        [307.5     , 307.5     , 307.5     , ...,        nan,
                nan,        nan],
        [307.5     , 307.5     , 307.5     , ...,        nan,
                nan,        nan],
        [307.5     , 307.5     , 307.5     , ...,        nan,
                nan,        nan]],

       [[       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        [       nan,        nan,        nan, ...,        nan,
                nan,        nan],
        ...,
        [  7.5     ,  34.33    , 137.83    , ...,        nan,
                nan,        nan],
        [  7.5     ,  14.639999,  81.4     , ...,        nan,
                nan,        nan],
        [  7.5     ,   7.5     ,   7.5     , ...,        nan,
                nan,        nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VTM02</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-b14d7ef5-fa3b-4e8c-9384-24116ba08dcd' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-b14d7ef5-fa3b-4e8c-9384-24116ba08dcd' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-47daa24b-ffb4-4ef7-a4ea-f45d67907d58' class='xr-var-data-in' type='checkbox'><label for='data-47daa24b-ffb4-4ef7-a4ea-f45d67907d58' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Spectral moments (0,2) wave period (Tm02)</dd><dt><span>units :</span></dt><dd>s</dd><dt><span>standard_name :</span></dt><dd>sea_surface_wave_mean_period_from_variance_spectral_density_second_frequency_moment</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>221</dd></dl></div><div class='xr-var-data'><pre>array([[[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [7.83     , 7.81     , 7.7599998, ...,       nan,       nan,
               nan],
        [7.8199997, 7.7999997, 7.75     , ...,       nan,       nan,
               nan],
        [7.7999997, 7.7799997, 7.75     , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
...
        [8.63     , 8.639999 , 8.639999 , ...,       nan,       nan,
               nan],
        [8.63     , 8.65     , 8.639999 , ...,       nan,       nan,
               nan],
        [8.63     , 8.65     , 8.65     , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [6.08     , 6.0499997, 6.       , ...,       nan,       nan,
               nan],
        [6.06     , 6.0299997, 5.99     , ...,       nan,       nan,
               nan],
        [6.04     , 6.02     , 5.98     , ...,       nan,       nan,
               nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>VTM01_WW</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-9d58d0ed-86f1-4ab0-a5fa-93346e1b702f' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-9d58d0ed-86f1-4ab0-a5fa-93346e1b702f' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-915fa8a1-2912-471a-aa11-b795d9e2f554' class='xr-var-data-in' type='checkbox'><label for='data-915fa8a1-2912-471a-aa11-b795d9e2f554' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Spectral moments (0,1) wind wave period</dd><dt><span>units :</span></dt><dd>s</dd><dt><span>standard_name :</span></dt><dd>sea_surface_wind_wave_mean_period</dd><dt><span>cell_methods :</span></dt><dd>time:point area:mean</dd><dt><span>type_of_analysis :</span></dt><dd>spectral analysis</dd><dt><span>WMO_code :</span></dt><dd>223</dd></dl></div><div class='xr-var-data'><pre>array([[[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [8.5      , 8.47     , 8.42     , ...,       nan,       nan,
               nan],
        [8.48     , 8.45     , 8.4      , ...,       nan,       nan,
               nan],
        [8.44     , 8.42     , 8.389999 , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
...
        [2.51     , 2.46     , 2.45     , ...,       nan,       nan,
               nan],
        [2.46     , 2.4199998, 2.4099998, ...,       nan,       nan,
               nan],
        [2.3999999, 2.36     , 2.35     , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [4.13     , 4.19     , 4.2799997, ...,       nan,       nan,
               nan],
        [4.22     , 4.2799997, 4.35     , ...,       nan,       nan,
               nan],
        [4.3199997, 4.4      , 4.48     , ...,       nan,       nan,
               nan]]], dtype=float32)</pre></div></li></ul></div></li><li class='xr-section-item'><input id='section-3025d323-a092-4ee5-bac2-a5e93c9125de' class='xr-section-summary-in' type='checkbox'  ><label for='section-3025d323-a092-4ee5-bac2-a5e93c9125de' class='xr-section-summary' >Attributes: <span>(27)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><dl class='xr-attrs'><dt><span>Conventions :</span></dt><dd>CF-1.6</dd><dt><span>time_coverage_start :</span></dt><dd>20200101-03:00:00</dd><dt><span>time_coverage_end :</span></dt><dd>20200102-00:00:00</dd><dt><span>date_created :</span></dt><dd>20200102-06:22:00</dd><dt><span>product_type :</span></dt><dd>hindcast</dd><dt><span>product :</span></dt><dd>GLOBAL_ANALYSIS_FORECAST_WAV_001_027</dd><dt><span>product_ref_date :</span></dt><dd>20200101-00:00:00</dd><dt><span>product_range :</span></dt><dd>D-1</dd><dt><span>product_user_manual :</span></dt><dd>http://marine.copernicus.eu/documents/PUM/CMEMS-GLO-PUM-001-027.pdf</dd><dt><span>quality_information_document :</span></dt><dd> http://marine.copernicus.eu/documents/QUID/CMEMS-GLO-QUID-001-027.</dd><dt><span>dataset :</span></dt><dd>global-analysis-forecast-wav-001-027</dd><dt><span>title :</span></dt><dd>Mean fields from global wave model MFWAM of Meteo-France with ECMWF forcing</dd><dt><span>institution :</span></dt><dd>METEO-FRANCE</dd><dt><span>references :</span></dt><dd>http://marine.copernicus.eu</dd><dt><span>credit :</span></dt><dd>E.U. Copernicus Marine Service Information (CMEMS)</dd><dt><span>licence :</span></dt><dd>http://marine.copernicus.eu/services-portfolio/service-commitments-and</dd><dt><span>contact :</span></dt><dd>servicedesk.cmems@mercator-ocean.eu</dd><dt><span>producer :</span></dt><dd>CMEMS - Global Monitoring and Forecasting Centre</dd><dt><span>area :</span></dt><dd>GLO</dd><dt><span>geospatial_lon_min :</span></dt><dd>-180.0</dd><dt><span>geospatial_lon_max :</span></dt><dd>179.9167</dd><dt><span>geospatial_lon_step :</span></dt><dd>0.08332825</dd><dt><span>geospatial_lon_units :</span></dt><dd>degree</dd><dt><span>geospatial_lat_min :</span></dt><dd>-80.0</dd><dt><span>geospatial_lat_max :</span></dt><dd>90.0</dd><dt><span>geospatial_lat_step :</span></dt><dd>0.08333588</dd><dt><span>geospatial_lat_units :</span></dt><dd>degree</dd></dl></div></li></ul></div></div>




```python
ds_phy_jan = xr.open_dataset(phy_jan)
ds_phy_apr = xr.open_dataset(phy_apr)
ds_phy_jul = xr.open_dataset(phy_jul)
#ds_phy_oct = xr.open_dataset(phy_oct)

ds_phy_all = [ds_phy_jan, ds_phy_apr, ds_phy_jul]
#ds_phy_all = [ds_phy_jan, ds_phy_apr, ds_phy_jul, ds_phy_oct]
datasets_phy = []

for ds_month in ds_phy_all:
  
  lon_min = get_closest(ds_month.longitude.data, bbox[0][0])
  lon_max = get_closest(ds_month.longitude.data, bbox[1][0])
  lat_min = get_closest(ds_month.latitude.data, bbox[0][1])
  lat_max = get_closest(ds_month.latitude.data, bbox[1][1])

  ds_phy_month_reg = ds_month.isel(time = 0, longitude = slice(lon_min, lon_max), latitude = slice(lat_min, lat_max))
  datasets_phy.append(ds_phy_month_reg)

ds_phy = xr.concat(datasets_phy, dim = 'ds_month')
ds_phy
```




<div><svg style="position: absolute; width: 0; height: 0; overflow: hidden">
<defs>
<symbol id="icon-database" viewBox="0 0 32 32">
<path d="M16 0c-8.837 0-16 2.239-16 5v4c0 2.761 7.163 5 16 5s16-2.239 16-5v-4c0-2.761-7.163-5-16-5z"></path>
<path d="M16 17c-8.837 0-16-2.239-16-5v6c0 2.761 7.163 5 16 5s16-2.239 16-5v-6c0 2.761-7.163 5-16 5z"></path>
<path d="M16 26c-8.837 0-16-2.239-16-5v6c0 2.761 7.163 5 16 5s16-2.239 16-5v-6c0 2.761-7.163 5-16 5z"></path>
</symbol>
<symbol id="icon-file-text2" viewBox="0 0 32 32">
<path d="M28.681 7.159c-0.694-0.947-1.662-2.053-2.724-3.116s-2.169-2.030-3.116-2.724c-1.612-1.182-2.393-1.319-2.841-1.319h-15.5c-1.378 0-2.5 1.121-2.5 2.5v27c0 1.378 1.122 2.5 2.5 2.5h23c1.378 0 2.5-1.122 2.5-2.5v-19.5c0-0.448-0.137-1.23-1.319-2.841zM24.543 5.457c0.959 0.959 1.712 1.825 2.268 2.543h-4.811v-4.811c0.718 0.556 1.584 1.309 2.543 2.268zM28 29.5c0 0.271-0.229 0.5-0.5 0.5h-23c-0.271 0-0.5-0.229-0.5-0.5v-27c0-0.271 0.229-0.5 0.5-0.5 0 0 15.499-0 15.5 0v7c0 0.552 0.448 1 1 1h7v19.5z"></path>
<path d="M23 26h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
<path d="M23 22h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
<path d="M23 18h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
</symbol>
</defs>
</svg>
<style>/* CSS stylesheet for displaying xarray objects in jupyterlab.
 *
 */

:root {
  --xr-font-color0: var(--jp-content-font-color0, rgba(0, 0, 0, 1));
  --xr-font-color2: var(--jp-content-font-color2, rgba(0, 0, 0, 0.54));
  --xr-font-color3: var(--jp-content-font-color3, rgba(0, 0, 0, 0.38));
  --xr-border-color: var(--jp-border-color2, #e0e0e0);
  --xr-disabled-color: var(--jp-layout-color3, #bdbdbd);
  --xr-background-color: var(--jp-layout-color0, white);
  --xr-background-color-row-even: var(--jp-layout-color1, white);
  --xr-background-color-row-odd: var(--jp-layout-color2, #eeeeee);
}

html[theme=dark],
body.vscode-dark {
  --xr-font-color0: rgba(255, 255, 255, 1);
  --xr-font-color2: rgba(255, 255, 255, 0.54);
  --xr-font-color3: rgba(255, 255, 255, 0.38);
  --xr-border-color: #1F1F1F;
  --xr-disabled-color: #515151;
  --xr-background-color: #111111;
  --xr-background-color-row-even: #111111;
  --xr-background-color-row-odd: #313131;
}

.xr-wrap {
  display: block;
  min-width: 300px;
  max-width: 700px;
}

.xr-text-repr-fallback {
  /* fallback to plain text repr when CSS is not injected (untrusted notebook) */
  display: none;
}

.xr-header {
  padding-top: 6px;
  padding-bottom: 6px;
  margin-bottom: 4px;
  border-bottom: solid 1px var(--xr-border-color);
}

.xr-header > div,
.xr-header > ul {
  display: inline;
  margin-top: 0;
  margin-bottom: 0;
}

.xr-obj-type,
.xr-array-name {
  margin-left: 2px;
  margin-right: 10px;
}

.xr-obj-type {
  color: var(--xr-font-color2);
}

.xr-sections {
  padding-left: 0 !important;
  display: grid;
  grid-template-columns: 150px auto auto 1fr 20px 20px;
}

.xr-section-item {
  display: contents;
}

.xr-section-item input {
  display: none;
}

.xr-section-item input + label {
  color: var(--xr-disabled-color);
}

.xr-section-item input:enabled + label {
  cursor: pointer;
  color: var(--xr-font-color2);
}

.xr-section-item input:enabled + label:hover {
  color: var(--xr-font-color0);
}

.xr-section-summary {
  grid-column: 1;
  color: var(--xr-font-color2);
  font-weight: 500;
}

.xr-section-summary > span {
  display: inline-block;
  padding-left: 0.5em;
}

.xr-section-summary-in:disabled + label {
  color: var(--xr-font-color2);
}

.xr-section-summary-in + label:before {
  display: inline-block;
  content: '►';
  font-size: 11px;
  width: 15px;
  text-align: center;
}

.xr-section-summary-in:disabled + label:before {
  color: var(--xr-disabled-color);
}

.xr-section-summary-in:checked + label:before {
  content: '▼';
}

.xr-section-summary-in:checked + label > span {
  display: none;
}

.xr-section-summary,
.xr-section-inline-details {
  padding-top: 4px;
  padding-bottom: 4px;
}

.xr-section-inline-details {
  grid-column: 2 / -1;
}

.xr-section-details {
  display: none;
  grid-column: 1 / -1;
  margin-bottom: 5px;
}

.xr-section-summary-in:checked ~ .xr-section-details {
  display: contents;
}

.xr-array-wrap {
  grid-column: 1 / -1;
  display: grid;
  grid-template-columns: 20px auto;
}

.xr-array-wrap > label {
  grid-column: 1;
  vertical-align: top;
}

.xr-preview {
  color: var(--xr-font-color3);
}

.xr-array-preview,
.xr-array-data {
  padding: 0 5px !important;
  grid-column: 2;
}

.xr-array-data,
.xr-array-in:checked ~ .xr-array-preview {
  display: none;
}

.xr-array-in:checked ~ .xr-array-data,
.xr-array-preview {
  display: inline-block;
}

.xr-dim-list {
  display: inline-block !important;
  list-style: none;
  padding: 0 !important;
  margin: 0;
}

.xr-dim-list li {
  display: inline-block;
  padding: 0;
  margin: 0;
}

.xr-dim-list:before {
  content: '(';
}

.xr-dim-list:after {
  content: ')';
}

.xr-dim-list li:not(:last-child):after {
  content: ',';
  padding-right: 5px;
}

.xr-has-index {
  font-weight: bold;
}

.xr-var-list,
.xr-var-item {
  display: contents;
}

.xr-var-item > div,
.xr-var-item label,
.xr-var-item > .xr-var-name span {
  background-color: var(--xr-background-color-row-even);
  margin-bottom: 0;
}

.xr-var-item > .xr-var-name:hover span {
  padding-right: 5px;
}

.xr-var-list > li:nth-child(odd) > div,
.xr-var-list > li:nth-child(odd) > label,
.xr-var-list > li:nth-child(odd) > .xr-var-name span {
  background-color: var(--xr-background-color-row-odd);
}

.xr-var-name {
  grid-column: 1;
}

.xr-var-dims {
  grid-column: 2;
}

.xr-var-dtype {
  grid-column: 3;
  text-align: right;
  color: var(--xr-font-color2);
}

.xr-var-preview {
  grid-column: 4;
}

.xr-var-name,
.xr-var-dims,
.xr-var-dtype,
.xr-preview,
.xr-attrs dt {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  padding-right: 10px;
}

.xr-var-name:hover,
.xr-var-dims:hover,
.xr-var-dtype:hover,
.xr-attrs dt:hover {
  overflow: visible;
  width: auto;
  z-index: 1;
}

.xr-var-attrs,
.xr-var-data {
  display: none;
  background-color: var(--xr-background-color) !important;
  padding-bottom: 5px !important;
}

.xr-var-attrs-in:checked ~ .xr-var-attrs,
.xr-var-data-in:checked ~ .xr-var-data {
  display: block;
}

.xr-var-data > table {
  float: right;
}

.xr-var-name span,
.xr-var-data,
.xr-attrs {
  padding-left: 25px !important;
}

.xr-attrs,
.xr-var-attrs,
.xr-var-data {
  grid-column: 1 / -1;
}

dl.xr-attrs {
  padding: 0;
  margin: 0;
  display: grid;
  grid-template-columns: 125px auto;
}

.xr-attrs dt,
.xr-attrs dd {
  padding: 0;
  margin: 0;
  float: left;
  padding-right: 10px;
  width: auto;
}

.xr-attrs dt {
  font-weight: normal;
  grid-column: 1;
}

.xr-attrs dt:hover span {
  display: inline-block;
  background: var(--xr-background-color);
  padding-right: 10px;
}

.xr-attrs dd {
  grid-column: 2;
  white-space: pre-wrap;
  word-break: break-all;
}

.xr-icon-database,
.xr-icon-file-text2 {
  display: inline-block;
  vertical-align: middle;
  width: 1em;
  height: 1.5em !important;
  stroke-width: 0;
  stroke: currentColor;
  fill: currentColor;
}
</style><pre class='xr-text-repr-fallback'>&lt;xarray.Dataset&gt;
Dimensions:    (depth: 50, ds_month: 3, latitude: 132, longitude: 240)
Coordinates:
  * longitude  (longitude) float32 10.0 10.08 10.17 10.25 ... 29.75 29.83 29.92
  * latitude   (latitude) float32 54.0 54.08 54.17 54.25 ... 64.75 64.83 64.92
  * depth      (depth) float32 0.494 1.541 2.646 ... 5.275e+03 5.728e+03
    time       (ds_month) datetime64[ns] 2020-01-01T12:00:00 ... 2020-07-01T1...
Dimensions without coordinates: ds_month
Data variables:
    mlotst     (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    zos        (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    bottomT    (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    sithick    (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    siconc     (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    usi        (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    vsi        (ds_month, latitude, longitude) float32 nan nan nan ... nan nan
    thetao     (ds_month, depth, latitude, longitude) float32 nan nan ... nan
    so         (ds_month, depth, latitude, longitude) float32 nan nan ... nan
    uo         (ds_month, depth, latitude, longitude) float32 nan nan ... nan
    vo         (ds_month, depth, latitude, longitude) float32 nan nan ... nan
Attributes: (12/24)
    title:              daily mean fields from Global Ocean Physics Analysis ...
    easting:            longitude
    northing:           latitude
    history:            2020/01/12 21:22:06 MERCATOR OCEAN Netcdf creation
    source:             MERCATOR PSY4V3R1
    institution:        MERCATOR OCEAN
    ...                 ...
    longitude_min:      -180.0
    longitude_max:      179.91667
    latitude_min:       -80.0
    latitude_max:       90.0
    z_min:              0.494025
    z_max:              5727.917</pre><div class='xr-wrap' hidden><div class='xr-header'><div class='xr-obj-type'>xarray.Dataset</div></div><ul class='xr-sections'><li class='xr-section-item'><input id='section-c3f7cbed-67b5-4774-a55f-39d9ec9d64de' class='xr-section-summary-in' type='checkbox' disabled ><label for='section-c3f7cbed-67b5-4774-a55f-39d9ec9d64de' class='xr-section-summary'  title='Expand/collapse section'>Dimensions:</label><div class='xr-section-inline-details'><ul class='xr-dim-list'><li><span class='xr-has-index'>depth</span>: 50</li><li><span>ds_month</span>: 3</li><li><span class='xr-has-index'>latitude</span>: 132</li><li><span class='xr-has-index'>longitude</span>: 240</li></ul></div><div class='xr-section-details'></div></li><li class='xr-section-item'><input id='section-d8cf8e22-c1b5-4698-be49-c8d16a472ab0' class='xr-section-summary-in' type='checkbox'  checked><label for='section-d8cf8e22-c1b5-4698-be49-c8d16a472ab0' class='xr-section-summary' >Coordinates: <span>(4)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><ul class='xr-var-list'><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>longitude</span></div><div class='xr-var-dims'>(longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>10.0 10.08 10.17 ... 29.83 29.92</div><input id='attrs-11e0bae1-00d3-45c5-9261-cb7a495589d4' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-11e0bae1-00d3-45c5-9261-cb7a495589d4' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-c7f25e0a-88a3-4097-8718-85c339436e79' class='xr-var-data-in' type='checkbox'><label for='data-c7f25e0a-88a3-4097-8718-85c339436e79' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>valid_min :</span></dt><dd>-180.0</dd><dt><span>valid_max :</span></dt><dd>179.91667</dd><dt><span>step :</span></dt><dd>0.08332825</dd><dt><span>units :</span></dt><dd>degrees_east</dd><dt><span>unit_long :</span></dt><dd>Degrees East</dd><dt><span>long_name :</span></dt><dd>Longitude</dd><dt><span>standard_name :</span></dt><dd>longitude</dd><dt><span>axis :</span></dt><dd>X</dd></dl></div><div class='xr-var-data'><pre>array([10.      , 10.083333, 10.166667, ..., 29.75    , 29.833334, 29.916666],
      dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>latitude</span></div><div class='xr-var-dims'>(latitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>54.0 54.08 54.17 ... 64.83 64.92</div><input id='attrs-18d05892-57e1-46ba-8b56-a654c6d9618c' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-18d05892-57e1-46ba-8b56-a654c6d9618c' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-d5712eb2-afee-4927-a02d-ce2779c16a81' class='xr-var-data-in' type='checkbox'><label for='data-d5712eb2-afee-4927-a02d-ce2779c16a81' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>valid_min :</span></dt><dd>-80.0</dd><dt><span>valid_max :</span></dt><dd>90.0</dd><dt><span>step :</span></dt><dd>0.08333588</dd><dt><span>units :</span></dt><dd>degrees_north</dd><dt><span>unit_long :</span></dt><dd>Degrees North</dd><dt><span>long_name :</span></dt><dd>Latitude</dd><dt><span>standard_name :</span></dt><dd>latitude</dd><dt><span>axis :</span></dt><dd>Y</dd></dl></div><div class='xr-var-data'><pre>array([54.      , 54.083332, 54.166668, 54.25    , 54.333332, 54.416668,
       54.5     , 54.583332, 54.666668, 54.75    , 54.833332, 54.916668,
       55.      , 55.083332, 55.166668, 55.25    , 55.333332, 55.416668,
       55.5     , 55.583332, 55.666668, 55.75    , 55.833332, 55.916668,
       56.      , 56.083332, 56.166668, 56.25    , 56.333332, 56.416668,
       56.5     , 56.583332, 56.666668, 56.75    , 56.833332, 56.916668,
       57.      , 57.083332, 57.166668, 57.25    , 57.333332, 57.416668,
       57.5     , 57.583332, 57.666668, 57.75    , 57.833332, 57.916668,
       58.      , 58.083332, 58.166668, 58.25    , 58.333332, 58.416668,
       58.5     , 58.583332, 58.666668, 58.75    , 58.833332, 58.916668,
       59.      , 59.083332, 59.166668, 59.25    , 59.333332, 59.416668,
       59.5     , 59.583332, 59.666668, 59.75    , 59.833332, 59.916668,
       60.      , 60.083332, 60.166668, 60.25    , 60.333332, 60.416668,
       60.5     , 60.583332, 60.666668, 60.75    , 60.833332, 60.916668,
       61.      , 61.083332, 61.166668, 61.25    , 61.333332, 61.416668,
       61.5     , 61.583332, 61.666668, 61.75    , 61.833332, 61.916668,
       62.      , 62.083332, 62.166668, 62.25    , 62.333332, 62.416668,
       62.5     , 62.583332, 62.666668, 62.75    , 62.833332, 62.916668,
       63.      , 63.083332, 63.166668, 63.25    , 63.333332, 63.416668,
       63.5     , 63.583332, 63.666668, 63.75    , 63.833332, 63.916668,
       64.      , 64.083336, 64.166664, 64.25    , 64.333336, 64.416664,
       64.5     , 64.583336, 64.666664, 64.75    , 64.833336, 64.916664],
      dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>depth</span></div><div class='xr-var-dims'>(depth)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>0.494 1.541 ... 5.275e+03 5.728e+03</div><input id='attrs-a01aeeb3-fcd3-41c9-b8a4-b7947608cd17' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-a01aeeb3-fcd3-41c9-b8a4-b7947608cd17' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-79540ac1-eb68-495e-90f3-f15400f737e5' class='xr-var-data-in' type='checkbox'><label for='data-79540ac1-eb68-495e-90f3-f15400f737e5' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>valid_min :</span></dt><dd>0.494025</dd><dt><span>valid_max :</span></dt><dd>5727.917</dd><dt><span>units :</span></dt><dd>m</dd><dt><span>positive :</span></dt><dd>down</dd><dt><span>unit_long :</span></dt><dd>Meters</dd><dt><span>long_name :</span></dt><dd>Depth</dd><dt><span>standard_name :</span></dt><dd>depth</dd><dt><span>axis :</span></dt><dd>Z</dd></dl></div><div class='xr-var-data'><pre>array([4.940250e-01, 1.541375e+00, 2.645669e+00, 3.819495e+00, 5.078224e+00,
       6.440614e+00, 7.929560e+00, 9.572997e+00, 1.140500e+01, 1.346714e+01,
       1.581007e+01, 1.849556e+01, 2.159882e+01, 2.521141e+01, 2.944473e+01,
       3.443415e+01, 4.034405e+01, 4.737369e+01, 5.576429e+01, 6.580727e+01,
       7.785385e+01, 9.232607e+01, 1.097293e+02, 1.306660e+02, 1.558507e+02,
       1.861256e+02, 2.224752e+02, 2.660403e+02, 3.181274e+02, 3.802130e+02,
       4.539377e+02, 5.410889e+02, 6.435668e+02, 7.633331e+02, 9.023393e+02,
       1.062440e+03, 1.245291e+03, 1.452251e+03, 1.684284e+03, 1.941893e+03,
       2.225078e+03, 2.533336e+03, 2.865703e+03, 3.220820e+03, 3.597032e+03,
       3.992484e+03, 4.405224e+03, 4.833291e+03, 5.274784e+03, 5.727917e+03],
      dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>time</span></div><div class='xr-var-dims'>(ds_month)</div><div class='xr-var-dtype'>datetime64[ns]</div><div class='xr-var-preview xr-preview'>2020-01-01T12:00:00 ... 2020-07-...</div><input id='attrs-558dddab-73c0-455d-ba8a-3e041af41edb' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-558dddab-73c0-455d-ba8a-3e041af41edb' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-21970ce3-e200-48b6-a82a-b1382bb09515' class='xr-var-data-in' type='checkbox'><label for='data-21970ce3-e200-48b6-a82a-b1382bb09515' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Time (hours since 1950-01-01)</dd><dt><span>standard_name :</span></dt><dd>time</dd><dt><span>valid_min :</span></dt><dd>613620.0</dd><dt><span>valid_max :</span></dt><dd>613620.0</dd><dt><span>axis :</span></dt><dd>T</dd></dl></div><div class='xr-var-data'><pre>array([&#x27;2020-01-01T12:00:00.000000000&#x27;, &#x27;2020-04-01T12:00:00.000000000&#x27;,
       &#x27;2020-07-01T12:00:00.000000000&#x27;], dtype=&#x27;datetime64[ns]&#x27;)</pre></div></li></ul></div></li><li class='xr-section-item'><input id='section-0ce9c50f-1d2a-408c-ad7e-0ea84da09d55' class='xr-section-summary-in' type='checkbox'  checked><label for='section-0ce9c50f-1d2a-408c-ad7e-0ea84da09d55' class='xr-section-summary' >Data variables: <span>(11)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><ul class='xr-var-list'><li class='xr-var-item'><div class='xr-var-name'><span>mlotst</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-45437100-e19e-4eb5-a3ca-5b21d0f26e41' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-45437100-e19e-4eb5-a3ca-5b21d0f26e41' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-bf41ad36-cbbc-49fb-9c82-0da30efb2e03' class='xr-var-data-in' type='checkbox'><label for='data-bf41ad36-cbbc-49fb-9c82-0da30efb2e03' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Density ocean mixed layer thickness</dd><dt><span>standard_name :</span></dt><dd>ocean_mixed_layer_thickness_defined_by_sigma_theta</dd><dt><span>units :</span></dt><dd>m</dd><dt><span>unit_long :</span></dt><dd>Meters</dd><dt><span>valid_min :</span></dt><dd>1</dd><dt><span>valid_max :</span></dt><dd>7517</dd><dt><span>cell_methods :</span></dt><dd>area: mean</dd></dl></div><div class='xr-var-data'><pre>array([[[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [70.955536, 55.085915, 47.914062, ...,       nan,       nan,
               nan],
        [59.66369 , 50.66073 , 50.20295 , ...,       nan,       nan,
               nan],
        [58.595543, 63.020725, 53.559986, ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
...
        [17.090366, 19.07407 , 24.109625, ...,       nan,       nan,
               nan],
        [18.768885, 20.447403, 28.07703 , ...,       nan,       nan,
               nan],
        [20.142218, 20.142218, 24.41481 , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [10.528886, 10.528886, 10.528886, ...,       nan,       nan,
               nan],
        [10.528886, 10.528886, 10.528886, ...,       nan,       nan,
               nan],
        [10.528886, 10.528886, 10.528886, ...,       nan,       nan,
               nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>zos</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-f81d15eb-5ad6-46e3-a5b9-60e8ae2afd9e' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-f81d15eb-5ad6-46e3-a5b9-60e8ae2afd9e' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-2d34d743-9f74-4563-9a81-39a40744ddb0' class='xr-var-data-in' type='checkbox'><label for='data-2d34d743-9f74-4563-9a81-39a40744ddb0' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Sea surface height</dd><dt><span>standard_name :</span></dt><dd>sea_surface_height_above_geoid</dd><dt><span>units :</span></dt><dd>m</dd><dt><span>unit_long :</span></dt><dd>Meters</dd><dt><span>valid_min :</span></dt><dd>-6293</dd><dt><span>valid_max :</span></dt><dd>5327</dd><dt><span>cell_methods :</span></dt><dd>area: mean</dd></dl></div><div class='xr-var-data'><pre>array([[[        nan,         nan,         nan, ...,         nan,
                 nan,         nan],
        [        nan,         nan,         nan, ...,         nan,
                 nan,         nan],
        [        nan,         nan,         nan, ...,         nan,
                 nan,         nan],
        ...,
        [-0.32441175, -0.28138065, -0.23407696, ...,         nan,
                 nan,         nan],
        [-0.3238014 , -0.2722251 , -0.22888882, ...,         nan,
                 nan,         nan],
        [-0.3109836 , -0.27954954, -0.23255104, ...,         nan,
                 nan,         nan]],

       [[        nan,         nan,         nan, ...,         nan,
                 nan,         nan],
        [        nan,         nan,         nan, ...,         nan,
                 nan,         nan],
        [        nan,         nan,         nan, ...,         nan,
                 nan,         nan],
...
        [-0.49501023, -0.47395244, -0.45258948, ...,         nan,
                 nan,         nan],
        [-0.4977569 , -0.46662802, -0.44526505, ...,         nan,
                 nan,         nan],
        [-0.50630206, -0.48158208, -0.4522843 , ...,         nan,
                 nan,         nan]],

       [[        nan,         nan,         nan, ...,         nan,
                 nan,         nan],
        [        nan,         nan,         nan, ...,         nan,
                 nan,         nan],
        [        nan,         nan,         nan, ...,         nan,
                 nan,         nan],
        ...,
        [-0.516068  , -0.51454204, -0.5108799 , ...,         nan,
                 nan,         nan],
        [-0.51240575, -0.50904876, -0.50447094, ...,         nan,
                 nan,         nan],
        [-0.5087435 , -0.5059969 , -0.50111395, ...,         nan,
                 nan,         nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>bottomT</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-a858cbfe-450e-4526-b5f8-db2d635351f2' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-a858cbfe-450e-4526-b5f8-db2d635351f2' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-1881215a-8e25-46ab-972a-6f5a533cf530' class='xr-var-data-in' type='checkbox'><label for='data-1881215a-8e25-46ab-972a-6f5a533cf530' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Sea floor potential temperature</dd><dt><span>standard_name :</span></dt><dd>sea_water_potential_temperature_at_sea_floor</dd><dt><span>units :</span></dt><dd>degrees_C</dd><dt><span>unit_long :</span></dt><dd>Degrees Celsius</dd><dt><span>valid_min :</span></dt><dd>-32697</dd><dt><span>valid_max :</span></dt><dd>18215</dd><dt><span>cell_methods :</span></dt><dd>area: mean</dd></dl></div><div class='xr-var-data'><pre>array([[[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [8.108249 , 8.212256 , 8.342631 , ...,       nan,       nan,
               nan],
        [8.188818 , 8.251808 , 8.325785 , ...,       nan,       nan,
               nan],
        [8.224708 , 8.307474 , 8.333842 , ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
...
        [7.4029055, 7.426344 , 7.4951935, ...,       nan,       nan,
               nan],
        [7.4168215, 7.575762 , 7.7530136, ...,       nan,       nan,
               nan],
        [7.539872 , 7.705405 , 7.8189335, ...,       nan,       nan,
               nan]],

       [[      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        [      nan,       nan,       nan, ...,       nan,       nan,
               nan],
        ...,
        [8.257668 , 8.31846  , 8.373394 , ...,       nan,       nan,
               nan],
        [8.325052 , 8.33311  , 8.35142  , ...,       nan,       nan,
               nan],
        [8.312601 , 8.32725  , 8.342631 , ...,       nan,       nan,
               nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>sithick</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-45bce354-8825-4d5a-99fb-08322ebdec56' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-45bce354-8825-4d5a-99fb-08322ebdec56' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-852d0a67-6e12-49da-bd19-069bf4cbe7ba' class='xr-var-data-in' type='checkbox'><label for='data-852d0a67-6e12-49da-bd19-069bf4cbe7ba' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Sea ice thickness</dd><dt><span>standard_name :</span></dt><dd>sea_ice_thickness</dd><dt><span>units :</span></dt><dd>m</dd><dt><span>unit_long :</span></dt><dd>Meters</dd><dt><span>valid_min :</span></dt><dd>1</dd><dt><span>valid_max :</span></dt><dd>5701</dd><dt><span>cell_methods :</span></dt><dd>area: mean where sea_ice</dd></dl></div><div class='xr-var-data'><pre>array([[[nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        ...,
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan]],

       [[nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        ...,
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan]],

       [[nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        ...,
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>siconc</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-851df0ce-683a-401b-9a23-c349fef6832f' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-851df0ce-683a-401b-9a23-c349fef6832f' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-66b5ad7b-170b-4a9a-bfa8-aa69fc1cab5d' class='xr-var-data-in' type='checkbox'><label for='data-66b5ad7b-170b-4a9a-bfa8-aa69fc1cab5d' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Ice concentration</dd><dt><span>standard_name :</span></dt><dd>sea_ice_area_fraction</dd><dt><span>units :</span></dt><dd>1</dd><dt><span>unit_long :</span></dt><dd>Fraction</dd><dt><span>valid_min :</span></dt><dd>1</dd><dt><span>valid_max :</span></dt><dd>27775</dd><dt><span>cell_methods :</span></dt><dd>area: mean where sea_ice</dd></dl></div><div class='xr-var-data'><pre>array([[[nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        ...,
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan]],

       [[nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        ...,
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan]],

       [[nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        ...,
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>usi</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-8c7cdde0-356b-4562-ad10-8f952d7c0b13' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-8c7cdde0-356b-4562-ad10-8f952d7c0b13' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-13fd6bd7-1f69-4eaf-94aa-e0750dd73a38' class='xr-var-data-in' type='checkbox'><label for='data-13fd6bd7-1f69-4eaf-94aa-e0750dd73a38' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Sea ice eastward velocity</dd><dt><span>standard_name :</span></dt><dd>eastward_sea_ice_velocity</dd><dt><span>units :</span></dt><dd>m s-1</dd><dt><span>unit_long :</span></dt><dd>Meters per second</dd><dt><span>valid_min :</span></dt><dd>-29827</dd><dt><span>valid_max :</span></dt><dd>32664</dd><dt><span>cell_methods :</span></dt><dd>area: mean where sea_ice</dd></dl></div><div class='xr-var-data'><pre>array([[[nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        ...,
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan]],

       [[nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        ...,
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan]],

       [[nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        ...,
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>vsi</span></div><div class='xr-var-dims'>(ds_month, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-8d1c726e-708d-4011-b7c7-01d996692196' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-8d1c726e-708d-4011-b7c7-01d996692196' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-aee2c730-aea9-4ec5-be4d-68251c76a458' class='xr-var-data-in' type='checkbox'><label for='data-aee2c730-aea9-4ec5-be4d-68251c76a458' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Sea ice northward velocity</dd><dt><span>standard_name :</span></dt><dd>northward_sea_ice_velocity</dd><dt><span>units :</span></dt><dd>m s-1</dd><dt><span>unit_long :</span></dt><dd>Meters per second</dd><dt><span>valid_min :</span></dt><dd>-32763</dd><dt><span>valid_max :</span></dt><dd>27001</dd><dt><span>cell_methods :</span></dt><dd>area: mean where sea_ice</dd></dl></div><div class='xr-var-data'><pre>array([[[nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        ...,
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan]],

       [[nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        ...,
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan]],

       [[nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        ...,
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan],
        [nan, nan, nan, ..., nan, nan, nan]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>thetao</span></div><div class='xr-var-dims'>(ds_month, depth, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-99601cbd-a9de-4223-a091-4bd2ea84d4e8' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-99601cbd-a9de-4223-a091-4bd2ea84d4e8' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-32ffd206-4bd5-489d-bcec-e50b72f372cb' class='xr-var-data-in' type='checkbox'><label for='data-32ffd206-4bd5-489d-bcec-e50b72f372cb' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Temperature</dd><dt><span>standard_name :</span></dt><dd>sea_water_potential_temperature</dd><dt><span>units :</span></dt><dd>degrees_C</dd><dt><span>unit_long :</span></dt><dd>Degrees Celsius</dd><dt><span>valid_min :</span></dt><dd>-32689</dd><dt><span>valid_max :</span></dt><dd>18195</dd><dt><span>cell_methods :</span></dt><dd>area: mean</dd></dl></div><div class='xr-var-data'><pre>array([[[[       nan,        nan,        nan, ...,        nan,
                 nan,        nan],
         [       nan,        nan,        nan, ...,        nan,
                 nan,        nan],
         [       nan,        nan,        nan, ...,        nan,
                 nan,        nan],
         ...,
         [ 7.9317303,  8.091403 ,  8.149999 , ...,        nan,
                 nan,        nan],
         [ 7.8811913,  8.0936   ,  8.082614 , ...,        nan,
                 nan,        nan],
         [ 7.9060946,  7.9961853,  8.117039 , ...,        nan,
                 nan,        nan]],

        [[       nan,        nan,        nan, ...,        nan,
                 nan,        nan],
         [       nan,        nan,        nan, ...,        nan,
                 nan,        nan],
         [       nan,        nan,        nan, ...,        nan,
                 nan,        nan],
...
         [       nan,        nan,        nan, ...,        nan,
                 nan,        nan],
         [       nan,        nan,        nan, ...,        nan,
                 nan,        nan],
         [       nan,        nan,        nan, ...,        nan,
                 nan,        nan]],

        [[       nan,        nan,        nan, ...,        nan,
                 nan,        nan],
         [       nan,        nan,        nan, ...,        nan,
                 nan,        nan],
         [       nan,        nan,        nan, ...,        nan,
                 nan,        nan],
         ...,
         [       nan,        nan,        nan, ...,        nan,
                 nan,        nan],
         [       nan,        nan,        nan, ...,        nan,
                 nan,        nan],
         [       nan,        nan,        nan, ...,        nan,
                 nan,        nan]]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>so</span></div><div class='xr-var-dims'>(ds_month, depth, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-cdd4ba68-d116-4452-b217-42286e18f38e' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-cdd4ba68-d116-4452-b217-42286e18f38e' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-b5a00155-e87a-4b8e-a91a-a7a3a75b8849' class='xr-var-data-in' type='checkbox'><label for='data-b5a00155-e87a-4b8e-a91a-a7a3a75b8849' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Salinity</dd><dt><span>standard_name :</span></dt><dd>sea_water_salinity</dd><dt><span>units :</span></dt><dd>1e-3</dd><dt><span>unit_long :</span></dt><dd>Practical Salinity Unit</dd><dt><span>valid_min :</span></dt><dd>2</dd><dt><span>valid_max :</span></dt><dd>28643</dd><dt><span>cell_methods :</span></dt><dd>area: mean</dd></dl></div><div class='xr-var-data'><pre>array([[[[      nan,       nan,       nan, ...,       nan,       nan,
                nan],
         [      nan,       nan,       nan, ...,       nan,       nan,
                nan],
         [      nan,       nan,       nan, ...,       nan,       nan,
                nan],
         ...,
         [34.756004, 34.54695 , 34.30738 , ...,       nan,       nan,
                nan],
         [34.725487, 34.46608 , 34.257027, ...,       nan,       nan,
                nan],
         [34.646137, 34.557632, 34.325695, ...,       nan,       nan,
                nan]],

        [[      nan,       nan,       nan, ...,       nan,       nan,
                nan],
         [      nan,       nan,       nan, ...,       nan,       nan,
                nan],
         [      nan,       nan,       nan, ...,       nan,       nan,
                nan],
...
         [      nan,       nan,       nan, ...,       nan,       nan,
                nan],
         [      nan,       nan,       nan, ...,       nan,       nan,
                nan],
         [      nan,       nan,       nan, ...,       nan,       nan,
                nan]],

        [[      nan,       nan,       nan, ...,       nan,       nan,
                nan],
         [      nan,       nan,       nan, ...,       nan,       nan,
                nan],
         [      nan,       nan,       nan, ...,       nan,       nan,
                nan],
         ...,
         [      nan,       nan,       nan, ...,       nan,       nan,
                nan],
         [      nan,       nan,       nan, ...,       nan,       nan,
                nan],
         [      nan,       nan,       nan, ...,       nan,       nan,
                nan]]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>uo</span></div><div class='xr-var-dims'>(ds_month, depth, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-d6351807-17ab-4229-949f-d32276426c9e' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-d6351807-17ab-4229-949f-d32276426c9e' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-5086e000-c675-4659-a108-0ced4daf6b36' class='xr-var-data-in' type='checkbox'><label for='data-5086e000-c675-4659-a108-0ced4daf6b36' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Eastward velocity</dd><dt><span>standard_name :</span></dt><dd>eastward_sea_water_velocity</dd><dt><span>units :</span></dt><dd>m s-1</dd><dt><span>unit_long :</span></dt><dd>Meters per second</dd><dt><span>valid_min :</span></dt><dd>-2810</dd><dt><span>valid_max :</span></dt><dd>3613</dd><dt><span>cell_methods :</span></dt><dd>area: mean</dd></dl></div><div class='xr-var-data'><pre>array([[[[        nan,         nan,         nan, ...,         nan,
                  nan,         nan],
         [        nan,         nan,         nan, ...,         nan,
                  nan,         nan],
         [        nan,         nan,         nan, ...,         nan,
                  nan,         nan],
         ...,
         [ 0.00671407, -0.08056886, -0.06714072, ...,         nan,
                  nan,         nan],
         [ 0.03845332,  0.08117923,  0.11963256, ...,         nan,
                  nan,         nan],
         [ 0.05493332,  0.12573627,  0.21057771, ...,         nan,
                  nan,         nan]],

        [[        nan,         nan,         nan, ...,         nan,
                  nan,         nan],
         [        nan,         nan,         nan, ...,         nan,
                  nan,         nan],
         [        nan,         nan,         nan, ...,         nan,
                  nan,         nan],
...
         [        nan,         nan,         nan, ...,         nan,
                  nan,         nan],
         [        nan,         nan,         nan, ...,         nan,
                  nan,         nan],
         [        nan,         nan,         nan, ...,         nan,
                  nan,         nan]],

        [[        nan,         nan,         nan, ...,         nan,
                  nan,         nan],
         [        nan,         nan,         nan, ...,         nan,
                  nan,         nan],
         [        nan,         nan,         nan, ...,         nan,
                  nan,         nan],
         ...,
         [        nan,         nan,         nan, ...,         nan,
                  nan,         nan],
         [        nan,         nan,         nan, ...,         nan,
                  nan,         nan],
         [        nan,         nan,         nan, ...,         nan,
                  nan,         nan]]]], dtype=float32)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>vo</span></div><div class='xr-var-dims'>(ds_month, depth, latitude, longitude)</div><div class='xr-var-dtype'>float32</div><div class='xr-var-preview xr-preview'>nan nan nan nan ... nan nan nan nan</div><input id='attrs-65014987-bb7c-4bf9-ac41-bde974de0539' class='xr-var-attrs-in' type='checkbox' ><label for='attrs-65014987-bb7c-4bf9-ac41-bde974de0539' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-3459f9d6-8199-44db-ab38-360baa14f0f2' class='xr-var-data-in' type='checkbox'><label for='data-3459f9d6-8199-44db-ab38-360baa14f0f2' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'><dt><span>long_name :</span></dt><dd>Northward velocity</dd><dt><span>standard_name :</span></dt><dd>northward_sea_water_velocity</dd><dt><span>units :</span></dt><dd>m s-1</dd><dt><span>unit_long :</span></dt><dd>Meters per second</dd><dt><span>valid_min :</span></dt><dd>-3441</dd><dt><span>valid_max :</span></dt><dd>3105</dd><dt><span>cell_methods :</span></dt><dd>area: mean</dd></dl></div><div class='xr-var-data'><pre>array([[[[           nan,            nan,            nan, ...,
                     nan,            nan,            nan],
         [           nan,            nan,            nan, ...,
                     nan,            nan,            nan],
         [           nan,            nan,            nan, ...,
                     nan,            nan,            nan],
         ...,
         [ 5.9999388e-01,  7.6906645e-01,  8.4536272e-01, ...,
                     nan,            nan,            nan],
         [ 6.1952573e-01,  8.2033753e-01,  8.6123234e-01, ...,
                     nan,            nan,            nan],
         [ 4.5899838e-01,  6.1830503e-01,  7.1535385e-01, ...,
                     nan,            nan,            nan]],

        [[           nan,            nan,            nan, ...,
                     nan,            nan,            nan],
         [           nan,            nan,            nan, ...,
                     nan,            nan,            nan],
         [           nan,            nan,            nan, ...,
                     nan,            nan,            nan],
...
                     nan,            nan,            nan],
         [           nan,            nan,            nan, ...,
                     nan,            nan,            nan],
         [           nan,            nan,            nan, ...,
                     nan,            nan,            nan]],

        [[           nan,            nan,            nan, ...,
                     nan,            nan,            nan],
         [           nan,            nan,            nan, ...,
                     nan,            nan,            nan],
         [           nan,            nan,            nan, ...,
                     nan,            nan,            nan],
         ...,
         [           nan,            nan,            nan, ...,
                     nan,            nan,            nan],
         [           nan,            nan,            nan, ...,
                     nan,            nan,            nan],
         [           nan,            nan,            nan, ...,
                     nan,            nan,            nan]]]],
      dtype=float32)</pre></div></li></ul></div></li><li class='xr-section-item'><input id='section-0cb8d2e7-ff15-44ff-9403-148a29d6d8fe' class='xr-section-summary-in' type='checkbox'  ><label for='section-0cb8d2e7-ff15-44ff-9403-148a29d6d8fe' class='xr-section-summary' >Attributes: <span>(24)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><dl class='xr-attrs'><dt><span>title :</span></dt><dd>daily mean fields from Global Ocean Physics Analysis and Forecast updated Daily</dd><dt><span>easting :</span></dt><dd>longitude</dd><dt><span>northing :</span></dt><dd>latitude</dd><dt><span>history :</span></dt><dd>2020/01/12 21:22:06 MERCATOR OCEAN Netcdf creation</dd><dt><span>source :</span></dt><dd>MERCATOR PSY4V3R1</dd><dt><span>institution :</span></dt><dd>MERCATOR OCEAN</dd><dt><span>references :</span></dt><dd>http://www.mercator-ocean.fr</dd><dt><span>comment :</span></dt><dd>CMEMS product</dd><dt><span>Conventions :</span></dt><dd>CF-1.4</dd><dt><span>domain_name :</span></dt><dd>GL12</dd><dt><span>field_type :</span></dt><dd>mean</dd><dt><span>field_date :</span></dt><dd>2020-01-01 00:00:00</dd><dt><span>field_julian_date :</span></dt><dd>25567.0</dd><dt><span>julian_day_unit :</span></dt><dd>days since 1950-01-01 00:00:00</dd><dt><span>forecast_range :</span></dt><dd>0-day_forecast</dd><dt><span>forecast_type :</span></dt><dd>hindcast</dd><dt><span>bulletin_date :</span></dt><dd>2020-01-15 00:00:00</dd><dt><span>bulletin_type :</span></dt><dd>operational</dd><dt><span>longitude_min :</span></dt><dd>-180.0</dd><dt><span>longitude_max :</span></dt><dd>179.91667</dd><dt><span>latitude_min :</span></dt><dd>-80.0</dd><dt><span>latitude_max :</span></dt><dd>90.0</dd><dt><span>z_min :</span></dt><dd>0.494025</dd><dt><span>z_max :</span></dt><dd>5727.917</dd></dl></div></li></ul></div></div>




```python
# Remove ships considering region
ais_data = ais_data[(ais_data['LON'] > bbox[0][0]) & (ais_data['LON'] < bbox[1][0])].dropna()
ais_data = ais_data[(ais_data['LAT'] > bbox[0][1]) & (ais_data['LAT'] < bbox[1][1])].dropna()

# Remove ships with status =! 0 and status =! 8
ais_data = ais_data[(ais_data['Status'] == 0) | (ais_data['Status'] == 8)].dropna()

# Remove ships with SOG < 5 or SOG > 102.2
ais_data = ais_data[(ais_data['SOG'] > 5) & (ais_data['SOG'] < 102.2)].dropna()

# Remove ships with heading > 361
ais_data = ais_data[(ais_data['Heading'] < 361)].dropna()

# Calculate tonnage (Length * Breadth * Depth * S) - WE DON'T HAVE THE DEPTH
# According to https://cdn.shopify.com/s/files/1/1021/8837/files/Tonnage_Guide_1_-_Simplified_Measurement.pdf?1513
ais_data['GrossTonnage'] = 0.67 * ais_data['Length'] * ais_data['Width']

ais_data = ais_data.reset_index(drop=True)

ais_data
```


```python
# Merge AIS and CMEMS data

study_data = ais_data

study_data['VHM0'] = 0.0
study_data['VMDR'] = 0.0
study_data['Temperature'] = 0.0
study_data['Salinity'] = 0.0

def extract_to_ais():
  for index, row in study_data.iterrows():

      lat = round(row['LAT'], 1)
      lon = round(row['LON'], 1)
      time = str(row['BaseDateTime'])

      VHM0 = ds_wav.VHM0.sel(time = time, longitude = lon, latitude = lat, method = 'nearest')
      VMDR = ds_wav.VMDR.sel(time = time, longitude = lon, latitude = lat, method = 'nearest')
      
      thetao = ds_phy.thetao.sel(longitude = lon, latitude = lat, method = 'nearest').isel(depth = 0)
      salin = ds_phy.so.sel(longitude = lon, latitude = lat, method = 'nearest').isel(depth = 0)

      study_data.at[index, 'VHM0'] = VHM0
      study_data.at[index, 'VMDR'] = VMDR
      study_data.at[index, 'Temperature'] = thetao
      study_data.at[index, 'Salinity'] = salin
      
extract_to_ais()

study_data
```

#Notes from meetings

**02 - 06 - 2021**

Space time cubes
- 52N: sfTracks space time cube

simple solution:
- least cost path as in simple_routing.ipynb

- start simple

**09 - 06 - 2021**

easy retrieval for cmems data: harvester.maridata EnvDataAPI github

regarding modelling:
- filter for ship types
- select a few specific ship
- 2000 records is too small
- have specific lengths and widths of ships
- Exclude Speed over ground below 7 knots
- standardization & normalization is recommended

regarding routing:
- Example Group C: Genetic algorithm
- multi-objective optimization: pymoo - NSGA-II
