# Bar Chart Race

Make animated bar and line chart races in Python with matplotlib or plotly.

Original Repo (without icons) : [https://github.com/dexplo/bar_chart_race](https://github.com/dexplo/bar_chart_race)

Andres Berejnoi's Repo (with
icons) : [https://github.com/andresberejnoi/bar_chart_race](https://github.com/andresberejnoi/bar_chart_race)

---

## Top Computer Science Schools 2000 - 2020

![img](video.gif)

---

## Installation

First Create a New Virtual Environment & Activate It:

```
pip install conda
conda env create --name envname --file=environments.yml
conda activate envname
```

Install `bar_chart_race` using `pip`:

```
pip install git+https://github.com/nangkyeong/bar_char_race_icons.git@main
```

You also need to install
FFmpeg: [https://github.com/BtbN/FFmpeg-Builds/releases/](https://github.com/BtbN/FFmpeg-Builds/releases/)

This is my current version of FFmpeg:
```
ffmpeg version 5.1.2 Copyright (c) 2000-2022 the FFmpeg developers
  built with Apple clang version 14.0.0 (clang-1400.0.29.202)
  libavutil      57. 28.100 / 57. 28.100
  libavcodec     59. 37.100 / 59. 37.100
  libavformat    59. 27.100 / 59. 27.100
  libavdevice    59.  7.100 / 59.  7.100
  libavfilter     8. 44.100 /  8. 44.100
  libswscale      6.  7.100 /  6.  7.100
  libswresample   4.  7.100 /  4.  7.100
  libpostproc    56.  6.100 / 56.  6.100
```

---

## Usage

Create a file and use the `bar_chart_race` library as shown below:

```python
import mchart as bcr
import pandas as pd

# Read data
selected_countries = ['BRA','CAN','CHN','DNK','FRA','DEU','KOR','MEX','NOR','ESP','SWE','GBR','USA']
df = pd.read_csv("data/API_NY.GDS.TOTL.ZS_DS2_en_csv_v2_4772577.csv",skiprows=2,header=1)

# Transform data from wide in year 
pivot_cols  = [col_ for col_ in df.columns if col_ not in ['Country Name', 'Country Code', 'Indicator Name', 'Indicator Code', 'Unnamed: 66']]
df_unpivot = pd.melt(df,id_vars = "Country Code" , value_vars = pivot_cols) 
df_unpivot["code"] = df_unpivot["Country Code"]
df_unpivot["date"] = df_unpivot["variable"]
df_unpivot = df_unpivot.drop(["variable","Country Code"],axis = 1)
df_unpivot = df_unpivot[df_unpivot["code"].isin(selected_countries)]

# filter data to equalizea 
df_norm = df_unpivot[df_unpivot["date"]>"1969"]

# wide data for countries
df_graph = df_norm.pivot(index='date',columns='code',values='value').tail(5)

# FILL NAs
# df_graph.fillna(0.0, inplace=True)
df_graph.interpolate(axis = 0, inplace=True)

bcr.bar_chart_race(df=df_graph,

    # name of the video file
    filename="video.mp4",
    # specify location of image folder
    img_label_folder="bar_image_labels",
    fixed_max = True, 
    # change the Figure properties
    fig_kwargs={
        'figsize': (26, 15),
        'dpi': 120,
        'facecolor': '#F8FAFF'
    },

    # orientation of the bar: h or v
    orientation="h",

    # sort the bar for each period
    sort="desc",

    # number of bars to display in each frame
    n_bars=10,

    # to fix the maximum value of the axis
    # fixed_max=True,

    # smoothness of the animation
    steps_per_period=10,

    # time period in ms for each row
    period_length=1500,

    # custom set of colors
    colors=[
        '#6ECBCE', '#FF2243', '#FFC33D', '#CE9673', '#FFA0FF', '#6501E5', '#F79522', '#699AF8', '#34718E', '#00DBCD',
        '#00A3FF', '#F8A737', '#56BD5B', '#D40CE5', '#6936F9', '#FF317B', '#0000F3', '#FFA0A0', '#31FF83', '#0556F3'
    ],

    # title and its styles
    title={'label': 'Programming Language Popularity 1990 - 2020',
           'size': 52,
           'weight': 'bold',
           'pad': 40
           },

    # adjust the position and style of the period label
    period_label={'x': .95, 'y': .15,
                  'ha': 'right',
                  'va': 'center',
                  'size': 72,
                  'weight': 'semibold'
                  },

    # style the bar label text
    bar_label_font={'size': 27},

    # style the labels in x and y axis
    tick_label_font={'size': 27},

    # adjust the style of bar
    # alpha is opacity of bar
    # ls - width of edge
    bar_kwargs={'alpha': .99, 'lw': 0},

    # adjust the bar label format
    bar_texttemplate='{x:.2f}',

    # adjust the period label format
)
```