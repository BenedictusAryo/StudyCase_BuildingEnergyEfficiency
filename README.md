
# BUILDING ENERGY EFFICIENCY
### Benedict Aryo
### _As part of Study Case Assignment in Make.ai Bootcamp_

#### Dataset Links : https://archive.ics.uci.edu/ml/datasets/Energy+efficiency

### Source:

The dataset was created by Angeliki Xifara (angxifara '@' gmail.com, Civil/Structural Engineer) and was processed by Athanasios Tsanas (tsanasthanasis '@' gmail.com, Oxford Centre for Industrial and Applied Mathematics, University of Oxford, UK).


### Data Set Information:

We perform energy analysis using 12 different building shapes simulated in Ecotect. The buildings differ with respect to the glazing area, the glazing area distribution, and the orientation, amongst other parameters. We simulate various settings as functions of the afore-mentioned characteristics to obtain 768 building shapes. The dataset comprises 768 samples and 8 features, aiming to predict two real valued responses. It can also be used as a multi-class classification problem if the response is rounded to the nearest integer.


### Attribute Information:

The dataset contains eight attributes (or features, denoted by X1...X8) and two responses (or outcomes, denoted by y1 and y2).<br> The aim is to use the eight features to predict each of the two responses. 

#### Specifically: 
- X1	Relative Compactness 
- X2	Surface Area 
- X3	Wall Area 
- X4	Roof Area 
- X5	Overall Height 
- X6	Orientation 
- X7	Glazing Area 
- X8	Glazing Area Distribution 
- y1	Heating Load 
- y2	Cooling Load




```python
import pandas as pd
import pandas_profiling
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
sns.set()
```


```python
df=pd.read_excel('Dataset/ENB2012_data.xlsx')
print("Dataset size : ",df.shape)
df.head()
```

    Dataset size :  (768, 10)
    




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
      <th>X1</th>
      <th>X2</th>
      <th>X3</th>
      <th>X4</th>
      <th>X5</th>
      <th>X6</th>
      <th>X7</th>
      <th>X8</th>
      <th>Y1</th>
      <th>Y2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.98</td>
      <td>514.5</td>
      <td>294.0</td>
      <td>110.25</td>
      <td>7.0</td>
      <td>2</td>
      <td>0.0</td>
      <td>0</td>
      <td>15.55</td>
      <td>21.33</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.98</td>
      <td>514.5</td>
      <td>294.0</td>
      <td>110.25</td>
      <td>7.0</td>
      <td>3</td>
      <td>0.0</td>
      <td>0</td>
      <td>15.55</td>
      <td>21.33</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.98</td>
      <td>514.5</td>
      <td>294.0</td>
      <td>110.25</td>
      <td>7.0</td>
      <td>4</td>
      <td>0.0</td>
      <td>0</td>
      <td>15.55</td>
      <td>21.33</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.98</td>
      <td>514.5</td>
      <td>294.0</td>
      <td>110.25</td>
      <td>7.0</td>
      <td>5</td>
      <td>0.0</td>
      <td>0</td>
      <td>15.55</td>
      <td>21.33</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.90</td>
      <td>563.5</td>
      <td>318.5</td>
      <td>122.50</td>
      <td>7.0</td>
      <td>2</td>
      <td>0.0</td>
      <td>0</td>
      <td>20.84</td>
      <td>28.28</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.describe()
```




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
      <th>X1</th>
      <th>X2</th>
      <th>X3</th>
      <th>X4</th>
      <th>X5</th>
      <th>X6</th>
      <th>X7</th>
      <th>X8</th>
      <th>Y1</th>
      <th>Y2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>768.000000</td>
      <td>768.000000</td>
      <td>768.000000</td>
      <td>768.000000</td>
      <td>768.00000</td>
      <td>768.000000</td>
      <td>768.000000</td>
      <td>768.00000</td>
      <td>768.000000</td>
      <td>768.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.764167</td>
      <td>671.708333</td>
      <td>318.500000</td>
      <td>176.604167</td>
      <td>5.25000</td>
      <td>3.500000</td>
      <td>0.234375</td>
      <td>2.81250</td>
      <td>22.307195</td>
      <td>24.587760</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.105777</td>
      <td>88.086116</td>
      <td>43.626481</td>
      <td>45.165950</td>
      <td>1.75114</td>
      <td>1.118763</td>
      <td>0.133221</td>
      <td>1.55096</td>
      <td>10.090204</td>
      <td>9.513306</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.620000</td>
      <td>514.500000</td>
      <td>245.000000</td>
      <td>110.250000</td>
      <td>3.50000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>6.010000</td>
      <td>10.900000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.682500</td>
      <td>606.375000</td>
      <td>294.000000</td>
      <td>140.875000</td>
      <td>3.50000</td>
      <td>2.750000</td>
      <td>0.100000</td>
      <td>1.75000</td>
      <td>12.992500</td>
      <td>15.620000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.750000</td>
      <td>673.750000</td>
      <td>318.500000</td>
      <td>183.750000</td>
      <td>5.25000</td>
      <td>3.500000</td>
      <td>0.250000</td>
      <td>3.00000</td>
      <td>18.950000</td>
      <td>22.080000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.830000</td>
      <td>741.125000</td>
      <td>343.000000</td>
      <td>220.500000</td>
      <td>7.00000</td>
      <td>4.250000</td>
      <td>0.400000</td>
      <td>4.00000</td>
      <td>31.667500</td>
      <td>33.132500</td>
    </tr>
    <tr>
      <th>max</th>
      <td>0.980000</td>
      <td>808.500000</td>
      <td>416.500000</td>
      <td>220.500000</td>
      <td>7.00000</td>
      <td>5.000000</td>
      <td>0.400000</td>
      <td>5.00000</td>
      <td>43.100000</td>
      <td>48.030000</td>
    </tr>
  </tbody>
</table>
</div>




```python
profilereport=pandas_profiling.ProfileReport(df)
profilereport
```




<meta charset="UTF-8">

<style>

        .variablerow {
            border: 1px solid #e1e1e8;
            border-top: hidden;
            padding-top: 2em;
            padding-bottom: 2em;
            padding-left: 1em;
            padding-right: 1em;
        }

        .headerrow {
            border: 1px solid #e1e1e8;
            background-color: #f5f5f5;
            padding: 2em;
        }
        .namecol {
            margin-top: -1em;
            overflow-x: auto;
        }

        .dl-horizontal dt {
            text-align: left;
            padding-right: 1em;
            white-space: normal;
        }

        .dl-horizontal dd {
            margin-left: 0;
        }

        .ignore {
            opacity: 0.4;
        }

        .container.pandas-profiling {
            max-width:975px;
        }

        .col-md-12 {
            padding-left: 2em;
        }

        .indent {
            margin-left: 1em;
        }

        .center-img {
            margin-left: auto !important;
            margin-right: auto !important;
            display: block;
        }

        /* Table example_values */
            table.example_values {
                border: 0;
            }

            .example_values th {
                border: 0;
                padding: 0 ;
                color: #555;
                font-weight: 600;
            }

            .example_values tr, .example_values td{
                border: 0;
                padding: 0;
                color: #555;
            }

        /* STATS */
            table.stats {
                border: 0;
            }

            .stats th {
                border: 0;
                padding: 0 2em 0 0;
                color: #555;
                font-weight: 600;
            }

            .stats tr {
                border: 0;
            }

            .stats td{
                color: #555;
                padding: 1px;
                border: 0;
            }


        /* Sample table */
            table.sample {
                border: 0;
                margin-bottom: 2em;
                margin-left:1em;
            }
            .sample tr {
                border:0;
            }
            .sample td, .sample th{
                padding: 0.5em;
                white-space: nowrap;
                border: none;

            }

            .sample thead {
                border-top: 0;
                border-bottom: 2px solid #ddd;
            }

            .sample td {
                width:100%;
            }


        /* There is no good solution available to make the divs equal height and then center ... */
            .histogram {
                margin-top: 3em;
            }
        /* Freq table */

            table.freq {
                margin-bottom: 2em;
                border: 0;
            }
            table.freq th, table.freq tr, table.freq td {
                border: 0;
                padding: 0;
            }

            .freq thead {
                font-weight: 600;
                white-space: nowrap;
                overflow: hidden;
                text-overflow: ellipsis;

            }

            td.fillremaining{
                width:auto;
                max-width: none;
            }

            td.number, th.number {
                text-align:right ;
            }

        /* Freq mini */
            .freq.mini td{
                width: 50%;
                padding: 1px;
                font-size: 12px;

            }
            table.freq.mini {
                 width:100%;
            }
            .freq.mini th {
                overflow: hidden;
                text-overflow: ellipsis;
                white-space: nowrap;
                max-width: 5em;
                font-weight: 400;
                text-align:right;
                padding-right: 0.5em;
            }

            .missing {
                color: #a94442;
            }
            .alert, .alert > th, .alert > td {
                color: #a94442;
            }


        /* Bars in tables */
            .freq .bar{
                float: left;
                width: 0;
                height: 100%;
                line-height: 20px;
                color: #fff;
                text-align: center;
                background-color: #337ab7;
                border-radius: 3px;
                margin-right: 4px;
            }
            .other .bar {
                background-color: #999;
            }
            .missing .bar{
                background-color: #a94442;
            }
            .tooltip-inner {
                width: 100%;
                white-space: nowrap;
                text-align:left;
            }

            .extrapadding{
                padding: 2em;
            }

            .pp-anchor{

            }

</style>

<div class="container pandas-profiling">
    <div class="row headerrow highlight">
        <h1>Overview</h1>
    </div>
    <div class="row variablerow">
    <div class="col-md-6 namecol">
        <p class="h4">Dataset info</p>
        <table class="stats" style="margin-left: 1em;">
            <tbody>
            <tr>
                <th>Number of variables</th>
                <td>10 </td>
            </tr>
            <tr>
                <th>Number of observations</th>
                <td>768 </td>
            </tr>
            <tr>
                <th>Total Missing (%)</th>
                <td>0.0% </td>
            </tr>
            <tr>
                <th>Total size in memory</th>
                <td>60.1 KiB </td>
            </tr>
            <tr>
                <th>Average record size in memory</th>
                <td>80.1 B </td>
            </tr>
            </tbody>
        </table>
    </div>
    <div class="col-md-6 namecol">
        <p class="h4">Variables types</p>
        <table class="stats" style="margin-left: 1em;">
            <tbody>
            <tr>
                <th>Numeric</th>
                <td>8 </td>
            </tr>
            <tr>
                <th>Categorical</th>
                <td>0 </td>
            </tr>
            <tr>
                <th>Boolean</th>
                <td>1 </td>
            </tr>
            <tr>
                <th>Date</th>
                <td>0 </td>
            </tr>
            <tr>
                <th>Text (Unique)</th>
                <td>0 </td>
            </tr>
            <tr>
                <th>Rejected</th>
                <td>1 </td>
            </tr>
            <tr>
                <th>Unsupported</th>
                <td>0 </td>
            </tr>
            </tbody>
        </table>
    </div>
    <div class="col-md-12" style="padding-left: 1em;">
        
        <p class="h4">Warnings</p>
        <ul class="list-unstyled"><li><a href="#pp_var_X7"><code>X7</code></a> has 48 / 6.2% zeros <span class="label label-info">Zeros</span></li><li><a href="#pp_var_X8"><code>X8</code></a> has 48 / 6.2% zeros <span class="label label-info">Zeros</span></li><li><a href="#pp_var_Y2"><code>Y2</code></a> is highly correlated with <a href="#pp_var_Y1"><code>Y1</code></a> (ρ = 0.97586) <span class="label label-primary">Rejected</span></li> </ul>
    </div>
</div>
    <div class="row headerrow highlight">
        <h1>Variables</h1>
    </div>
    <div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_X1">X1<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>12</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>1.6%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (n)</th>
                    <td>0</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>

        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>0.76417</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>0.62</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>0.98</td>
                </tr>
                <tr class="ignore">
                    <th>Zeros (%)</th>
                    <td>0.0%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram2311462699485699952">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAAbVJREFUeJzt3DGOm1AUhtGLNS0swIJdZBHZU%2BrsKYvILkBeAFaKFDEpEqeZmT8ZS/MI0jklFrrP4n1CNK/btm2rxpZlqWmaWo/l4OZ5rnEcm858ajrtt77vq%2BrXHx6GYY8lcCDrutY0TX/2TUu7BNJ1XVVVDcPwLJAPn740WcPXzx/ffM9b1/bIDF533zctnZpPhAMRCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIdjkX63/Q4vytVmd8tdDiHLFH57wnbxAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgWCXU022bauqqnVdn/324/u31svhH7z0rP7mkWf50pz7tfu%2Baanbdpi6LEtN09R6LAc3z3ON49h05i6B3G63ulwu1fd9dV3XejwHs21bXa/XOp/PdTq1/SrYJRA4Ch/pEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQ/AedpU8KKJUg/AAAAAElFTkSuQmCC">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives2311462699485699952,#minihistogram2311462699485699952"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives2311462699485699952">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles2311462699485699952"
                                                  aria-controls="quantiles2311462699485699952" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram2311462699485699952" aria-controls="histogram2311462699485699952"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common2311462699485699952" aria-controls="common2311462699485699952"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme2311462699485699952" aria-controls="extreme2311462699485699952"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>

    </ul>

    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles2311462699485699952">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>0.62</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>0.62</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>0.6825</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>0.75</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>0.83</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>0.98</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>0.98</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>0.36</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>0.1475</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>0.10578</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.13842</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>-0.70657</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>0.76417</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>0.088194</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>0.49551</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>586.88</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>0.011189</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>6.1 KiB</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram2311462699485699952">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAIABJREFUeJzt3X10VOW5/vFrTMgEMBkEzBsEGlkgVChFQEikElEjKGLLsohABI8iFEQDWoTDQVKPJhQVaaGgcBBokYIuX0qrJxJU3lYEJEgFygGECFEIAU9IeE2APL8/PJmfYxJIwjOZneT7WWuvxX72nmfue4YhF3vv2XEZY4wAAABgzTWBLgAAAKC%2BIWABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGXBgS6gISgtLdWRI0cUFhYml8sV6HIAAGgQjDE6deqUYmJidM01tXtMiYBVC44cOaLY2NhAlwEAQIOUm5ur1q1b1%2BpzErBqQVhYmKTv3%2BDw8PAAVwMAQMNQVFSk2NhY78/h2kTAqgVlpwXDw8MJWAAA1LJAXJ7DRe4AAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIxf9lzH9ZiWEegS6q1tL/YPdAnVUpf%2BLtS11xYAqosjWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAsnodsDZs2KD77rtPMTExcrlcev/9973bLly4oGeffVZdunRR06ZNFRMTo4cfflhHjhzxmaOgoEDJycnyeDzyeDxKTk7WyZMna7sVAABQh9TrgHXmzBl17dpV8%2BbNK7ft7Nmz2r59u6ZPn67t27fr3Xff1b59%2BzRo0CCf/YYNG6YdO3YoIyNDGRkZ2rFjh5KTk2urBQAAUAcFB7oAfxowYIAGDBhQ4TaPx6PMzEyfsblz5%2BqWW27R4cOH1aZNG%2B3Zs0cZGRnavHmzevXqJUlatGiR4uPjtXfvXt14441%2B7wEAANQ99foIVnUVFhbK5XKpWbNmkqTPPvtMHo/HG64kqXfv3vJ4PMrKyqp0nuLiYhUVFfksAACg4SBg/Z/z589rypQpGjZsmMLDwyVJeXl5ioiIKLdvRESE8vLyKp0rPT3de82Wx%2BNRbGys3%2BoGAADOQ8DS9xe8Dx06VKWlpZo/f77PNpfLVW5/Y0yF42WmTp2qwsJC75Kbm2u9ZgAA4Fz1%2Bhqsqrhw4YKGDBminJwcffLJJ96jV5IUFRWlY8eOlXvM8ePHFRkZWemcbrdbbrfbL/UCAADna9BHsMrC1f79%2B7V27Vq1aNHCZ3t8fLwKCwu1detW79iWLVtUWFiohISE2i4XAADUEfX6CNbp06f11VdfeddzcnK0Y8cONW/eXDExMXrggQe0fft2/eMf/9ClS5e811U1b95cISEh6tSpk/r376/Ro0fr9ddflyQ9/vjjGjhwIN8gBAAAlarXAWvbtm26/fbbveuTJk2SJI0cOVKpqalavXq1JOnnP/%2B5z%2BM%2B/fRTJSYmSpLefPNNPfnkk0pKSpIkDRo0qML7agEAAJSp1wErMTFRxphKt19uW5nmzZtr%2BfLlNssCAAD1XIO%2BBgsAAMAfCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWFavA9aGDRt03333KSYmRi6XS%2B%2B//77PdmOMUlNTFRMTo8aNGysxMVG7d%2B/22aegoEDJycnyeDzyeDxKTk7WyZMna7MNAABQx9TrgHXmzBl17dpV8%2BbNq3D7rFmzNHv2bM2bN0%2Bff/65oqKidNddd%2BnUqVPefYYNG6YdO3YoIyNDGRkZ2rFjh5KTk2urBQAAUAcFB7oAfxowYIAGDBhQ4TZjjObMmaNp06Zp8ODBkqRly5YpMjJSK1as0JgxY7Rnzx5lZGRo8%2BbN6tWrlyRp0aJFio%2BP1969e3XjjTfWWi8AAKDuqNdHsC4nJydHeXl5SkpK8o653W717dtXWVlZkqTPPvtMHo/HG64kqXfv3vJ4PN59AAAAfqxeH8G6nLy8PElSZGSkz3hkZKQOHTrk3SciIqLcYyMiIryPr0hxcbGKi4u960VFRTZKBgAAdUSDPYJVxuVy%2BawbY3zGfry9on1%2BLD093XtRvMfjUWxsrL2CAQCA4zXYgBUVFSVJ5Y5E5efne49qRUVF6dixY%2BUee/z48XJHvn5o6tSpKiws9C65ubkWKwcAAE7XYANWXFycoqKilJmZ6R0rKSnR%2BvXrlZCQIEmKj49XYWGhtm7d6t1ny5YtKiws9O5TEbfbrfDwcJ8FAAA0HPX6GqzTp0/rq6%2B%2B8q7n5ORox44dat68udq0aaOUlBSlpaWpffv2at%2B%2BvdLS0tSkSRMNGzZMktSpUyf1799fo0eP1uuvvy5JevzxxzVw4EC%2BQQgAACpVrwPWtm3bdPvtt3vXJ02aJEkaOXKkli5dqsmTJ%2BvcuXMaN26cCgoK1KtXL61Zs0ZhYWHex7z55pt68sknvd82HDRoUKX31QIAAJDqecBKTEyUMabS7S6XS6mpqUpNTa10n%2BbNm2v58uV%2BqA4AANRXDfYaLAAAAH8hYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAsgYfsC5evKj/%2BI//UFxcnBo3bqwbbrhBzz//vEpLS737GGOUmpqqmJgYNW7cWImJidq9e3cAqwYAAE7m2IC1fPlynT9/3u/P8/vf/16vvfaa5s2bpz179mjWrFl66aWXNHfuXO8%2Bs2bN0uzZszVv3jx9/vnnioqK0l133aVTp075vT4AAFD3ODZgTZo0SVFRURozZoy2bt3qt%2Bf57LPPdP/99%2Bvee%2B/VT37yEz3wwANKSkrStm3bJH1/9GrOnDmaNm2aBg8erM6dO2vZsmU6e/asVqxY4be6AABA3eXYgHXkyBG98cYbOnr0qPr06aObbrpJr7zyio4fP271efr06aOPP/5Y%2B/btkyT985//1KZNm3TPPfdIknJycpSXl6ekpCTvY9xut/r27ausrCyrtQAAgPrBsQErODhYgwcP1urVq3X48GGNHDlSb7zxhlq3bq3Bgwfrgw8%2BkDHmqp/n2Wef1UMPPaSOHTuqUaNG6tatm1JSUvTQQw9JkvLy8iRJkZGRPo%2BLjIz0bvux4uJiFRUV%2BSwAAKDhcGzA%2BqGoqCjdcccdSkxMlMvl0rZt2zRs2DC1b99eGzduvKq5V61apeXLl2vFihXavn27li1bppdfflnLli3z2c/lcvmsG2PKjZVJT0%2BXx%2BPxLrGxsVdVIwAAqFscHbBOnDihOXPmqGvXrrr11luVn5%2Bv999/X4cOHdK3336rgQMH6uGHH76q5/jtb3%2BrKVOmaOjQoerSpYuSk5M1ceJEpaenS/o%2B3Ekqd7QqPz%2B/3FGtMlOnTlVhYaF3yc3NvaoaAQBA3eLYgPWrX/1KrVq10muvvabk5GTl5ubq7bffVv/%2B/eVyuXTttddq8uTJOnTo0FU9z9mzZ3XNNb4vQ1BQkPc2DXFxcYqKilJmZqZ3e0lJidavX6%2BEhIQK53S73QoPD/dZAABAwxEc6AIqEx4errVr1%2BoXv/hFpftER0dr//79V/U89913n1588UW1adNGN910k7744gvNnj1b//Zv/ybp%2B1ODKSkpSktLU/v27dW%2BfXulpaWpSZMmGjZs2FU9NwAAqJ8cG7B%2BfA1URVwul9q1a3dVzzN37lxNnz5d48aNU35%2BvmJiYjRmzBg999xz3n0mT56sc%2BfOady4cSooKFCvXr20Zs0ahYWFXdVzAwCA%2BsllbHwVzw8mTpyodu3a6YknnvAZ/9Of/qSDBw/qlVdeCVBl1VdUVCSPx6PCwkLrpwt7TMuwOh/%2Bv20v9g90CdVSl/4u1LXXFkDd5M%2Bfv1fi2Guw3n77bfXu3bvceHx8vFatWhWAigAAAKrGsQHrxIkTuu6668qNh4eH68SJEwGoCAAAoGocG7DatWunjz76qNz4Rx99pLi4uABUBAAAUDWOvcg9JSVFKSkp%2Bu6779SvXz9J0scff6xZs2bp5ZdfDnB1AAAAlXNswBo9erTOnz%2BvtLQ0zZgxQ5LUunVr/fGPf/TeQgEAAMCJHBuwJGnChAmaMGGCjh49qsaNG6tZs2aBLgkAAOCKHB2wykRHRwe6BAAAgCpz7EXux48f1yOPPKI2bdooNDRUISEhPgsAAIBTOfYI1qhRo3TgwAH99re/VXR0tFwuV6BLAgAAqBLHBqwNGzZow4YN6tatW6BLAQAAqBbHniJs3bo1R60AAECd5NiA9eqrr2rq1Kn65ptvAl0KAABAtTj2FGFycrJOnTqltm3bKjw8XI0aNfLZnp%2BfH6DKAAAALs%2BxAWvmzJmBLgEAAKBGHBuwHn300UCXAAAAUCOOvQZLkr7%2B%2BmulpqYqOTnZe0pwzZo12rNnT4ArAwAAqJxjA9bGjRt10003af369Xrrrbd0%2BvRpSdL27dv13HPPBbg6AACAyjk2YD377LNKTU3Vp59%2B6nPn9n79%2Bmnz5s0BrAwAAODyHBuwvvzySz3wwAPlxiMiInT8%2BPEAVAQAAFA1jg1YzZo1U15eXrnxHTt2qFWrVgGoCAAAoGocG7CGDh2qKVOm6Pjx4947um/ZskXPPPOMRowYEeDqAAAAKufYgJWWlqaoqChFR0fr9OnT%2BulPf6qEhAT17NlT06dPD3R5AAAAlXLsfbBCQkK0atUq7du3T9u3b1dpaaluvvlmdezYMdClAQAAXJZjA1aZDh06qEOHDoEuAwAAoMocG7Aef/zxy25fuHBhLVUCAABQPY4NWEePHvVZv3Dhgnbv3q1Tp07ptttuC1BVAAAAV%2BbYgPX3v/%2B93NjFixf1m9/8Rp06dQpARQAAAFXj2G8RViQ4OFjPPPOMXnrppUCXAgAAUKk6FbAk6eDBg7pw4UKgywAAAKiUY08RTp482WfdGKOjR49q9erVGj58eICqAgAAuDLHBqzPPvvMZ/2aa67R9ddfr5kzZ2r06NEBqgoAAODKHBuwNm7cGOgSAAAAasSxAQsItB7TMgJdQr3Fa4u6atuL/QNdQrXUpc9aXXttr8SxAatnz57eX/J8JVu3bvVzNQAAAFXn2IB1%2B%2B236/XXX1eHDh0UHx8vSdq8ebP27t2rMWPGyO12B7hCAACAijk2YJ08eVLjx49XWlqaz/i0adN07Ngx/dd//VeAKgMAALg8x94H66233tIjjzxSbnzUqFF6%2B%2B23A1ARAABA1Tg2YLndbmVlZZUbz8rKsn568Ntvv9WIESPUokULNWnSRD//%2Bc%2BVnZ3t3W6MUWpqqmJiYtS4cWMlJiZq9%2B7dVmsAAAD1h2NPET755JMaO3asvvjiC/Xu3VvS99dgLVq0SP/%2B7/9u7XkKCgp066236vbbb9d///d/KyIiQgcOHFCzZs28%2B8yaNUuzZ8/W0qVL1aFDB73wwgu66667tHfvXoWFhVmrBQAA1A%2BODVjTpk1TXFyc/vCHP%2BiNN96QJHXq1EmLFi3SsGHDrD3P73//e8XGxmrJkiXesZ/85CfePxtjNGfOHE2bNk2DBw%2BWJC1btkyRkZFasWKFxowZY60WAABQPzj2FKEkDRs2TFu2bFFRUZGKioq0ZcsWq%2BFKklavXq0ePXro17/%2BtSIiItStWzctWrTIuz0nJ0d5eXlKSkryjrndbvXt27fCU5iSVFxc7K25bAEAAA2HowNWUVGRli5dqueee04FBQWSpH/%2B8586evSotec4ePCgFixYoPbt2%2Bujjz7S2LFj9eSTT%2BrPf/6zJCkvL0%2BSFBkZ6fO4yMhI77YfS09Pl8fj8S6xsbHW6gUAAM7n2FOEu3bt0p133qkmTZooNzdXo0aN0nXXXae33npL33zzjZYtW2bleUpLS9WjRw/v7SC6deum3bt3a8GCBXr44Ye9%2B/34pqfGmEpvhDp16lRNmjTJu15UVETIAgCgAXHsEayJEydq2LBhOnDggEJDQ73j9957rzZs2GDteaKjo/XTn/7UZ6xTp046fPiwJCkqKkqSyh2tys/PL3dUq4zb7VZ4eLjPAgAAGg7HBqzPP/9c48aNK3eUqFWrVlZPEd56663au3evz9i%2BffvUtm1bSVJcXJyioqKUmZnp3V5SUqL169crISHBWh0AAKD%2BcOwpwpCQEJ0%2Bfbrc%2BP79%2B9WyZUtrzzNx4kQlJCQoLS1NQ4YM0datW7Vw4UItXLhQ0venBlNSUpSWlqb27durffv2SktLU5MmTaxfcA8AAOoHxwasQYMG6T//8z%2B1atUqSd8HnW%2B//VZTpkzx3i7Bhp49e%2Bq9997T1KlT9fzzzysuLk5z5szR8OHDvftMnjxZ586d07hx41RQUKBevXppzZo13AMLAABUyGWMMYEuoiKFhYXq37%2B/9u/fr5MnTyo2NlZHjhxRz549lZGRoWuvvTbQJVZZUVGRPB6PCgsLrV%2BP1WNahtX5AACV2/Zi/0CXUC116WeEP15bf/78vRLHHsHyeDzKyspSZmamtm/frtLSUt188826%2B%2B67K/32HgAAgBM4MmBduHBB99xzj%2BbPn6%2BkpCSfm3wCAAA4nSO/RdioUSN98cUXHKkCAAB1kiMDliSNGDHC5/cDAgAA1BWOPEVYZt68eVq7dq169Oihpk2b%2BmybNWtWgKoCAAC4PMcGrOzsbP3sZz%2BTJH355Zc%2B2zh1CAAAnMxxAevgwYOKi4vTxo0bA10KAABAjTjuGqz27dvr%2BPHj3vUHH3xQx44dC2BFAAAA1eO4gPXj%2B55%2B%2BOGHOnPmTICqAQAAqD7HBSwAAIC6znEBy%2BVylbuInYvaAQBAXeK4i9yNMRo1apTcbrck6fz58xo7dmy52zS8%2B%2B67gSgPAADgihwXsEaOHOmzPmLEiABVAgAAUDOOC1jcvR0AANR1jrsGCwAAoK4jYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUErB9IT0%2BXy%2BVSSkqKd6y4uFgTJkxQy5Yt1bRpUw0aNEjffPNNAKsEAABOR8D6P59//rkWLlyon/3sZz7jKSkpeu%2B997Ry5Upt2rRJp0%2Bf1sCBA3Xp0qUAVQoAAJyOgCXp9OnTGj58uBYtWqTrrrvOO15YWKjFixfrlVde0Z133qlu3bpp%2BfLl2rlzp9auXRvAigEAgJMRsCSNHz9e9957r%2B68806f8ezsbF24cEFJSUnesZiYGHXu3FlZWVm1XSYAAKgjggNdQKCtXLlS2dnZ2rZtW7lteXl5CgkJ8TmqJUmRkZHKy8urdM7i4mIVFxd714uKiuwVDAAAHK9BH8HKzc3VU089pTfffFOhoaFVfpwxRi6Xq9Lt6enp8ng83iU2NtZGuQAAoI5o0AErOztb%2Bfn56t69u4KDgxUcHKz169frj3/8o4KDgxUZGamSkhIVFBT4PC4/P1%2BRkZGVzjt16lQVFhZ6l9zcXH%2B3AgAAHKRBnyK84447tHPnTp%2BxRx55RB07dtSzzz6r2NhYNWrUSJmZmRoyZIgk6ejRo9q1a5dmzZpV6bxut1tut9uvtQMAAOdq0AErLCxMnTt39hlr2rSpWrRo4R1/9NFH9fTTT6tFixZq3ry5nnnmGXXp0qXcBfEAAABlGnTAqopXX31VwcHBGjJkiM6dO6c77rhDS5cuVVBQUKBLAwAADkXA%2BpF169b5rIeGhmru3LmaO3duYAoCAAB1ToO%2ByB0AAMAfCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsKzBB6z09HT17NlTYWFhioiI0C9/%2BUvt3bvXZ5/i4mJNmDBBLVu2VNOmTTVo0CB98803AaoYAAA4XYMPWOvXr9f48eO1efNmZWZm6uLFi0pKStKZM2e8%2B6SkpOi9997TypUrtWnTJp0%2BfVoDBw7UpUuXAlg5AABwquBAFxBoGRkZPutLlixRRESEsrOzddttt6mwsFCLFy/WX/7yF915552SpOXLlys2NlZr167V3XffHYiyAQCAgzX4I1g/VlhYKElq3ry5JCk7O1sXLlxQUlKSd5%2BYmBh17txZWVlZFc5RXFysoqIinwUAADQcBKwfMMZo0qRJ6tOnjzp37ixJysvLU0hIiK677jqffSMjI5WXl1fhPOnp6fJ4PN4lNjbW77UDAADnIGD9wBNPPKEvv/xSf/3rX6%2B4rzFGLperwm1Tp05VYWGhd8nNzbVdKgAAcDAC1v%2BZMGGCVq9erU8//VStW7f2jkdFRamkpEQFBQU%2B%2B%2Bfn5ysyMrLCudxut8LDw30WAADQcDT4gGWM0RNPPKF3331Xn3zyieLi4ny2d%2B/eXY0aNVJmZqZ37OjRo9q1a5cSEhJqu1wAAFAHNPhvEY4fP14rVqzQ3/72N4WFhXmvq/J4PGrcuLE8Ho8effRRPf3002rRooWaN2%2BuZ555Rl26dPF%2BqxAAAOCHGnzAWrBggSQpMTHRZ3zJkiUaNWqUJOnVV19VcHCwhgwZonPnzumOO%2B7Q0qVLFRQUVMvVAgCAuqDBByxjzBX3CQ0N1dy5czV37txaqAgAANR1Df4aLAAAANsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIBVRfPnz1dcXJxCQ0PVvXt3bdy4MdAlAQAAhyJgVcGqVauUkpKiadOm6YsvvtAvfvELDRgwQIcPHw50aQAAwIEIWFUwe/ZsPfroo3rsscfUqVMnzZkzR7GxsVqwYEGgSwMAAA4UHOgCnK6kpETZ2dmaMmWKz3hSUpKysrIqfExxcbGKi4u964WFhZKkoqIi6/VdKj5jfU4AQMX88e%2B4P9WlnxH%2BeG3L5jTGWJ/7SghYV3DixAldunRJkZGRPuORkZHKy8ur8DHp6en63e9%2BV248NjbWLzUCAGqH55VAV1B/%2BfO1PXXqlDwej/%2BeoAIErCpyuVw%2B68aYcmNlpk6dqkmTJnnXS0tL9b//%2B79q0aJFpY%2BpD4qKihQbG6vc3FyFh4cHupxa0dB6bmj9SvTcEHpuaP1KDadnY4xOnTqlmJiYWn9uAtYVtGzZUkFBQeWOVuXn55c7qlXG7XbL7Xb7jDVr1sxvNTpNeHh4vf7AVqSh9dzQ%2BpXouSFoaP1KDaPn2j5yVYaL3K8gJCRE3bt3V2Zmps94ZmamEhISAlQVAABwMo5gVcGkSZOUnJysHj16KD4%2BXgsXLtThw4c1duzYQJcGAAAcKCg1NTU10EU4XefOndWiRQulpaXp5Zdf1rlz5/SXv/xFXbt2DXRpjhMUFKTExEQFBzec7N7Qem5o/Ur03BA0tH6lhtlzbXKZQHx3EQAAoB7jGiwAAADLCFgAAACWEbAAAAAsI2ABAABYRsDCZc2fP19xcXEKDQ1V9%2B7dtXHjxsvuf/LkSY0fP17R0dEKDQ1Vp06d9OGHH3q3p6amyuVy%2BSxRUVH%2BbqPKqtNvYmJiuV5cLpfuvfde7z7GGKWmpiomJkaNGzdWYmKidu/eXRutVJntnkeNGlVue%2B/evWujlSqr7t/rOXPm6MYbb1Tjxo0VGxuriRMn6vz581c1Z22y3a/TP8dS9Xq%2BcOGCnn/%2BebVr106hoaHq2rWrMjIyrmrO2ma737rwHjueASqxcuVK06hRI7No0SLzr3/9yzz11FOmadOm5tChQxXuX1xcbHqfVjNMAAAHx0lEQVT06GHuueces2nTJvP111%2BbjRs3mh07dnj3mTFjhrnpppvM0aNHvUt%2Bfn5ttXRZ1e33u%2B%2B%2B8%2Blj165dJigoyCxZssS7z8yZM01YWJh55513zM6dO82DDz5ooqOjTVFRUS11dXn%2B6HnkyJGmf//%2BPvt99913tdTRlVW35%2BXLlxu3223efPNNk5OTYz766CMTHR1tUlJSajxnbfJHv07%2BHBtT/Z4nT55sYmJizAcffGAOHDhg5s%2Bfb0JDQ8327dtrPGdt8ke/Tn%2BP6wICFip1yy23mLFjx/qMdezY0UyZMqXC/RcsWGBuuOEGU1JSUumcM2bMMF27drVapy3V7ffHXn31VRMWFmZOnz5tjDGmtLTUREVFmZkzZ3r3OX/%2BvPF4POa1116zV/hVsN2zMd8HrPvvv99qnTZVt%2Bfx48ebfv36%2BYxNmjTJ9OnTp8Zz1iZ/9Ovkz7Ex1e85OjrazJs3z2fs/vvvN8OHD6/xnLXJH/06/T2uCzhFiAqVlJQoOztbSUlJPuNJSUnKysqq8DGrV69WfHy8xo8fr8jISHXu3FlpaWm6dOmSz3779%2B9XTEyM4uLiNHToUB08eNBvfVRVTfr9scWLF2vo0KFq2rSpJCknJ0d5eXk%2Bc7rdbvXt27fKc/qTP3ous27dOkVERKhDhw4aPXq08vPzrdV9NWrSc58%2BfZSdna2tW7dKkg4ePKgPP/zQe1rUxuvoL/7ot4wTP8dSzXouLi5WaGioz1jjxo21adOmGs9ZW/zRbxmnvsd1BbdvRYVOnDihS5culfuF1pGRkeV%2B8XWZgwcP6pNPPtHw4cP14Ycfav/%2B/Ro/frwuXryo5557TpLUq1cv/fnPf1aHDh107NgxvfDCC0pISNDu3bvVokULv/dVmZr0%2B0Nbt27Vrl27tHjxYu9Y2eMqmvPQoUMWqr46/uhZkgYMGKBf//rXatu2rXJycjR9%2BnT169dP2dnZ5X4Jem2rSc9Dhw7V8ePH1adPHxljdPHiRf3mN7/RlClTajxnbfFHv5JzP8dSzXq%2B%2B%2B67NXv2bN12221q166dPv74Y/3tb3/z/uewvr3HV%2BpXcvZ7XFcQsHBZLpfLZ90YU26sTGlpqSIiIrRw4UIFBQWpe/fuOnLkiF566SVvwBowYIB3/y5duig%2BPl7t2rXTsmXLNGnSJP81UkXV6feHFi9erM6dO%2BuWW26xNmdtsd3zgw8%2B6P1z586d1aNHD7Vt21YffPCBBg8ebKfoq1SdntetW6cXX3xR8%2BfPV69evfTVV1/pqaeeUnR0tKZPn16jOWub7X6d/jmWqtfzH/7wB40ePVodO3aUy%2BVSu3bt9Mgjj2jJkiU1nrO22e63LrzHTscpQlSoZcuWCgoKKvc/oPz8/HL/UyoTHR2tDh06KCgoyDvWqVMn5eXlqaSkpMLHNG3aVF26dNH%2B/fvtFV8DNem3zNmzZ7Vy5Uo99thjPuNl37ipyZy1wR89VyQ6Olpt27YN%2BHss1azn6dOnKzk5WY899pi6dOmiX/3qV0pLS1N6erpKS0uv6nX0N3/0WxGnfI6lmvV8/fXX6/3339eZM2d06NAh/c///I%2BuvfZaxcXF1XjO2uKPfivipPe4riBgoUIhISHq3r27MjMzfcYzMzOVkJBQ4WNuvfVWffXVVz7/CO/bt0/R0dEKCQmp8DHFxcXas2ePoqOj7RVfAzXpt8xbb72l4uJijRgxwmc8Li5OUVFRPnOWlJRo/fr1V5yzNvij54p89913ys3NDfh7LNWs57Nnz%2Bqaa3z/qQwKCpL5/ktCV/U6%2Bps/%2Bq2IUz7H0tX9vQ4NDVWrVq108eJFvfPOO7r//vuvek5/80e/FXHSe1xn1P519agryr76u3jxYvOvf/3LpKSkmKZNm5qvv/7aGGNMcnKyz7dUDh8%2BbK699lrzxBNPmL1795p//OMfJiIiwrzwwgvefZ5%2B%2Bmmzbt06c/DgQbN582YzcOBAExYW5p0zkKrbb5k%2BffqYBx98sMI5Z86caTwej3n33XfNzp07zUMPPeTI2zTY6vnUqVPm6aefNllZWSYnJ8d8%2BumnJj4%2B3rRq1arO9jxjxgwTFhZm/vrXv5qDBw%2BaNWvWmHbt2pkhQ4ZUec5A8ke/Tv4cG1P9njdv3mzeeecdc%2BDAAbNhwwbTr18/ExcXZwoKCqo8ZyD5o1%2Bnv8d1AQELl/WnP/3JtG3b1oSEhJibb77ZrF%2B/3rutb9%2B%2BZuTIkT77Z2VlmV69ehm3221uuOEG8%2BKLL5qLFy96t5fdB6pRo0YmJibGDB482Ozevbu22rmi6va7d%2B9eI8msWbOmwvlKS0vNjBkzTFRUlHG73ea2224zO3fu9GcL1Waz57Nnz5qkpCRz/fXXm0aNGpk2bdqYkSNHmsOHD/u7jWqpTs8XLlwwqamppl27diY0NNTExsaacePG%2BfwwutKcgWa7X6d/jo2pXs/r1q0znTp1Mm6327Ro0cIkJyebb7/9tlpzBprtfuvCe%2Bx0LmMqOeYLAACAGuEaLAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlv0/3RFnszaxZpgAAAAASUVORK5CYII%3D"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common2311462699485699952">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.9</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.79</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.71</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.66</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.98</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.64</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.86</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.76</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.74</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.82</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="other">
        <td class="fillremaining">Other values (2)</td>
        <td class="number">128</td>
        <td class="number">16.7%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme2311462699485699952">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.62</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.64</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.66</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.69</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.71</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.79</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.82</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.86</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.9</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.98</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_X2">X2<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>12</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>1.6%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (n)</th>
                    <td>0</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>

        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>671.71</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>514.5</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>808.5</td>
                </tr>
                <tr class="ignore">
                    <th>Zeros (%)</th>
                    <td>0.0%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram-9216223717743914426">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAAa9JREFUeJzt3DvO00AUhuHjiNZOH8W7YBHsiZqS/bAIdpEoC7BFQUGGAkLB5ZO4/OPf4nlKW9EZR/PKcjNDa61VZ9frteZ57j2WnbtcLnU%2Bn7vOfNF12lfjOFbVlweepmmLJbAjy7LUPM/f9k1PmwQyDENVVU3TJJDvvHz97rd/8/7NqydYyd/718/y2Dc9HbpPhB0RCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAINjkXK/mTs5T%2Bd/6zp%2BMNAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQbHKqSWutqqqWZfnh3qePH3ovh2fkZ3vice2xb3raJJB1Xauqap7nLcbzjB3f/vreuq51PB77LaaqhrZBlvf7vW63W43jWMMw9B7PzrTWal3XOp1OdTj0/SrYJBDYCx/pEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCASfARjfRkE4ongLAAAAAElFTkSuQmCC">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives-9216223717743914426,#minihistogram-9216223717743914426"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives-9216223717743914426">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles-9216223717743914426"
                                                  aria-controls="quantiles-9216223717743914426" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram-9216223717743914426" aria-controls="histogram-9216223717743914426"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common-9216223717743914426" aria-controls="common-9216223717743914426"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme-9216223717743914426" aria-controls="extreme-9216223717743914426"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>

    </ul>

    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles-9216223717743914426">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>514.5</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>514.5</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>606.38</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>673.75</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>741.12</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>808.5</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>808.5</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>294</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>134.75</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>88.086</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.13114</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>-1.0595</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>671.71</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>75.542</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>-0.12513</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>515870</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>7759.2</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>6.1 KiB</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram-9216223717743914426">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAIABJREFUeJzt3X9UVWW%2Bx/HPUfCIBifRAUTRyIWjiZmpVyEnMYs0zZnrasxfpNWYXn%2BFZv64TsV0J2icMu/ozdKptFHLWmNdZ5qLYpnawp%2BQpV6vWjKCCmJePGgqoDz3D8dzOwJK9cA5wPu11l7L8zzP2fu7nzaHT3vvs3EYY4wAAABgTSNfFwAAAFDfELAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwLIAXxfQEJSXl%2BvEiRMKDg6Ww%2BHwdTkAADQIxhidPXtWkZGRatSods8pEbBqwYkTJxQVFeXrMgAAaJDy8vLUtm3bWt0mAasWBAcHS7ryHzgkJMTH1QAA0DAUFxcrKirK83u4NhGwasHVy4IhISEELAAAapkvbs/hJncAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWMYfewaAG%2Bg5L93XJXwvu18Y6OsS4Cfq0rFb345bzmABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMvqdcDasmWLHnzwQUVGRsrhcOjDDz/09JWVlWn27Nnq2rWrmjdvrsjISD3yyCM6ceKE1zqKioqUlJQkl8sll8ulpKQknTlzprZ3BQAA1CH1OmB9%2B%2B236tatmxYvXlyh7/z588rOztYzzzyj7OxsrV27VocOHdLQoUO9xo0aNUp79uxRenq60tPTtWfPHiUlJdXWLgAAgDoowNcF1KRBgwZp0KBBlfa5XC5lZGR4tS1atEj/9E//pNzcXLVr104HDhxQenq6tm/frt69e0uSli1bpri4OB08eFA//elPa3wfAABA3VOvz2B9X263Ww6HQzfffLMkadu2bXK5XJ5wJUl9%2BvSRy%2BVSZmZmlespKSlRcXGx1wIAABoOAtY/XLx4UXPmzNGoUaMUEhIiSSooKFBYWFiFsWFhYSooKKhyXWlpaZ57tlwul6KiomqsbgAA4H8IWLpyw/uIESNUXl6uV1991avP4XBUGG%2BMqbT9qrlz58rtdnuWvLw86zUDAAD/Va/vwaqOsrIyDR8%2BXDk5Ofrkk088Z68kKSIiQidPnqzwnlOnTik8PLzKdTqdTjmdzhqpFwAA%2BL8GfQbrarg6fPiwNm7cqJYtW3r1x8XFye12a%2BfOnZ62HTt2yO12Kz4%2BvrbLBQAAdUS9PoN17tw5ffXVV57XOTk52rNnj0JDQxUZGamHHnpI2dnZ%2Butf/6rLly977qsKDQ1VkyZN1LlzZw0cOFDjx4/X66%2B/Lkl64oknNGTIEL5BCAAAqlSvA9bu3bvVv39/z%2BsZM2ZIksaOHauUlBStW7dOknTHHXd4vW/Tpk1KSEiQJK1atUrTpk1TYmKiJGno0KGVPlcLAADgqnodsBISEmSMqbL/en1XhYaGauXKlTbLAgAA9VyDvgcLAACgJhCwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCsXgesLVu26MEHH1RkZKQcDoc%2B/PBDr35jjFJSUhQZGamgoCAlJCRo//79XmOKioqUlJQkl8sll8ulpKQknTlzpjZ3AwAA1DH1OmB9%2B%2B236tatmxYvXlxp//z587VgwQItXrxYu3btUkREhO677z6dPXvWM2bUqFHas2eP0tPTlZ6erj179igpKam2dgEAANRBAb4uoCYNGjRIgwYNqrTPGKOFCxdq3rx5GjZsmCRpxYoVCg8P1%2BrVqzVhwgQdOHBA6enp2r59u3r37i1JWrZsmeLi4nTw4EH99Kc/rbV9AQAAdUe9PoN1PTk5OSooKFBiYqKnzel0ql%2B/fsrMzJQkbdu2TS6XyxOuJKlPnz5yuVyeMQAAANeq12ewrqegoECSFB4e7tUeHh6uo0ePesaEhYVVeG9YWJjn/ZUpKSlRSUmJ53VxcbGNkgEAQB3RYM9gXeVwOLxeG2O82q7tr2zMtdLS0jw3xbtcLkVFRdkrGAAA%2BL0GG7AiIiIkqcKZqMLCQs9ZrYiICJ08ebLCe0%2BdOlXhzNd3zZ07V26327Pk5eVZrBwAAPi7BhuwoqOjFRERoYyMDE9baWmpNm/erPj4eElSXFyc3G63du7c6RmzY8cOud1uz5jKOJ1OhYSEeC0AAKDhqNf3YJ07d05fffWV53VOTo727Nmj0NBQtWvXTsnJyUpNTVVMTIxiYmKUmpqqZs2aadSoUZKkzp07a%2BDAgRo/frxef/11SdITTzyhIUOG8A1CAABQpXodsHbv3q3%2B/ft7Xs%2BYMUOSNHbsWC1fvlyzZs3ShQsXNGnSJBUVFal3797asGGDgoODPe9ZtWqVpk2b5vm24dChQ6t8rhYAAIBUzwNWQkKCjDFV9jscDqWkpCglJaXKMaGhoVq5cmUNVAcAAOqrBnsPFgAAQE0hYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAsgYfsC5duqRf//rXio6OVlBQkG699VY9//zzKi8v94wxxiglJUWRkZEKCgpSQkKC9u/f78OqAQCAP/PbgLVy5UpdvHixxrfzu9/9Tq%2B99poWL16sAwcOaP78%2Bfr973%2BvRYsWecbMnz9fCxYs0OLFi7Vr1y5FRETovvvu09mzZ2u8PgAAUPf4bcCaMWOGIiIiNGHCBO3cubPGtrNt2zb9/Oc/1%2BDBg3XLLbfooYceUmJionbv3i3pytmrhQsXat68eRo2bJhiY2O1YsUKnT9/XqtXr66xugAAQN3ltwHrxIkTevPNN5Wfn6%2B%2BffuqS5cuevnll3Xq1Cmr2%2Bnbt68%2B/vhjHTp0SJL0xRdf6LPPPtMDDzwgScrJyVFBQYESExM973E6nerXr58yMzOt1gIAAOoHvw1YAQEBGjZsmNatW6fc3FyNHTtWb775ptq2bathw4bpo48%2BkjHmR29n9uzZGjlypDp16qTAwEB1795dycnJGjlypCSpoKBAkhQeHu71vvDwcE/ftUpKSlRcXOy1AACAhsNvA9Z3RUREaMCAAUpISJDD4dDu3bs1atQoxcTEaOvWrT9q3WvWrNHKlSu1evVqZWdna8WKFXrppZe0YsUKr3EOh8PrtTGmQttVaWlpcrlcniUqKupH1QgAAOoWvw5Y33zzjRYuXKhu3brprrvuUmFhoT788EMdPXpUx48f15AhQ/TII4/8qG08/fTTmjNnjkaMGKGuXbsqKSlJ06dPV1pamqQr4U5ShbNVhYWFFc5qXTV37ly53W7PkpeX96NqBAAAdYvfBqx//ud/Vps2bfTaa68pKSlJeXl5ev/99zVw4EA5HA7ddNNNmjVrlo4ePfqjtnP%2B/Hk1auQ9DY0bN/Y8piE6OloRERHKyMjw9JeWlmrz5s2Kj4%2BvdJ1Op1MhISFeCwAAaDgCfF1AVUJCQrRx40b97Gc/q3JM69atdfjw4R%2B1nQcffFAvvPCC2rVrpy5duujzzz/XggUL9Nhjj0m6cmkwOTlZqampiomJUUxMjFJTU9WsWTONGjXqR20bAADUT34bsK69B6oyDodDHTp0%2BFHbWbRokZ555hlNmjRJhYWFioyM1IQJE/Tss896xsyaNUsXLlzQpEmTVFRUpN69e2vDhg0KDg7%2BUdsGAAD1k8PY%2BCpeDZg%2Bfbo6dOigKVOmeLX/x3/8h44cOaKXX37ZR5V9f8XFxXK5XHK73VwuBOqgnvPSfV3C97L7hYG%2BLgF%2Boi4duzVx3Pry96/f3oP1/vvvq0%2BfPhXa4%2BLitGbNGh9UBAAAUD1%2BG7C%2B%2BeYbtWjRokJ7SEiIvvnmGx9UBAAAUD1%2BG7A6dOig9evXV2hfv369oqOjfVARAABA9fjtTe7JyclKTk7W6dOndc8990iSPv74Y82fP18vvfSSj6sDAAComt8GrPHjx%2BvixYtKTU3Vc889J0lq27at/vCHP3geoQAAAOCP/DZgSdLUqVM1depU5efnKygoSDfffLOvSwIAALghvw5YV7Vu3drXJQAAAFSb397kfurUKT366KNq166dmjZtqiZNmngtAAAA/spvz2CNGzdOX3/9tZ5%2B%2Bmm1bt1aDofD1yUBAABUi98GrC1btmjLli3q3r27r0sBAAD4Xvz2EmHbtm05awUAAOokvw1Yr7zyiubOnatjx475uhQAAIDvxW8vESYlJens2bNq3769QkJCFBgY6NVfWFjoo8oAAACuz28D1osvvujrEgAAAH4Qvw1Yjz/%2BuK9LAAAA%2BEH89h4sSfr73/%2BulJQUJSUleS4JbtiwQQcOHPBxZQAAAFXz24C1detWdenSRZs3b9Z7772nc%2BfOSZKys7P17LPP%2Brg6AACAqvltwJo9e7ZSUlK0adMmrye333PPPdq%2BfbsPKwMAALg%2Bvw1YX375pR566KEK7WFhYTp16pQPKgIAAKgevw1YN998swoKCiq079mzR23atPFBRQAAANXjtwFrxIgRmjNnjk6dOuV5ovuOHTs0c%2BZMjRkzxsfVAQAAVM1vA1ZqaqoiIiLUunVrnTt3Trfddpvi4%2BPVq1cvPfPMM74uDwAAoEp%2B%2BxysJk2aaM2aNTp06JCys7NVXl6uO%2B%2B8U506dfJ1aQAAANfltwHrqo4dO6pjx46%2BLgMAAKDa/DZgPfHEE9ftX7p0aS1VAgAA8P34bcDKz8/3el1WVqb9%2B/fr7Nmzuvvuu31UFQAAwI35bcD6y1/%2BUqHt0qVL%2Bpd/%2BRd17tzZBxUBAABUj99%2Bi7AyAQEBmjlzpn7/%2B9/7uhQAAIAq1amAJUlHjhxRWVmZr8sAAACokt9eIpw1a5bXa2OM8vPztW7dOo0ePdpHVQEAANyY3wasbdu2eb1u1KiRfvKTn%2BjFF1/U%2BPHjfVQVAADAjfltwNq6dauvSwAAAPhB/DZgoXp6zkv3dQkA/AyfC4Dv%2BW3A6tWrl%2BePPN/Izp07a7gaAACA6vPbgNW/f3%2B9/vrr6tixo%2BLi4iRJ27dv18GDBzVhwgQ5nU4fVwgAAFA5vw1YZ86c0eTJk5WamurVPm/ePJ08eVJ//OMffVQZAADA9fntc7Dee%2B89PfrooxXax40bp/fff98HFQEAAFSP3wYsp9OpzMzMCu2ZmZnWLw8eP35cY8aMUcuWLdWsWTPdcccdysrK8vQbY5SSkqLIyEgFBQUpISFB%2B/fvt1oDAACoP/z2EuG0adM0ceJEff755%2BrTp4%2BkK/dgLVu2TP/6r/9qbTtFRUW666671L9/f/3Xf/2XwsLC9PXXX%2Bvmm2/2jJk/f74WLFig5cuXq2PHjvrtb3%2Br%2B%2B67TwcPHlRwcLC1WgAAQP3gtwFr3rx5io6O1r//%2B7/rzTfflCR17txZy5Yt06hRo6xt53e/%2B52ioqL01ltvedpuueUWz7%2BNMVq4cKHmzZunYcOGSZJWrFih8PBwrV69WhMmTLBWCwAAqB/89hKhJI0aNUo7duxQcXGxiouLtWPHDqvhSpLWrVunnj176pe//KXCwsLUvXt3LVu2zNOfk5OjgoICJSYmetqcTqf69etX6SVMSSopKfHUfHUBAAANh18HrOLiYi1fvlzPPvusioqKJElffPGF8vPzrW3jyJEjWrJkiWJiYrR%2B/XpNnDhR06ZN09tvvy1JKigokCSFh4d7vS88PNzTd620tDS5XC7PEhUVZa1eAADg//z2EuG%2Bfft07733qlmzZsrLy9O4cePUokULvffeezp27JhWrFhhZTvl5eXq2bOn53EQ3bt31/79%2B7VkyRI98sgjnnHXPvTUGFPlg1Dnzp2rGTNmeF4XFxcTsgAAaED89gzW9OnTNWrUKH399ddq2rSpp33w4MHasmWLte20bt1at912m1db586dlZubK0mKiIiQpApnqwoLCyuc1brK6XQqJCTEawEAAA2H3wasXbt2adKkSRXOErVp08bqJcK77rpLBw8e9Go7dOiQ2rdvL0mKjo5WRESEMjIyPP2lpaXavHmz4uPjrdUBAADqD7%2B9RNikSROdO3euQvvhw4fVqlUra9uZPn264uPjlZqaquHDh2vnzp1aunSpli5dKunKpcHk5GSlpqYqJiZGMTExSk1NVbNmzazfcA8AAOoHvw1YQ4cO1b/9279pzZo1kq4EnePHj2vOnDmexyXY0KtXL33wwQeaO3eunn/%2BeUVHR2vhwoUaPXq0Z8ysWbN04cIFTZo0SUVFRerdu7c2bNjAM7AAAEClHMYY4%2BsiKuN2uzVw4EAdPnxYZ86cUVRUlE6cOKFevXopPT1dN910k69LrLbi4mK5XC653W7r92P1nJdudX0AAPjC7hcGWl9nTf7%2BvRG/PYPlcrmUmZmpjIwMZWdnq7y8XHfeeafuv//%2BKr%2B9BwAA4A/8MmCVlZXpgQce0KuvvqrExESvh3wCAAD4O7/8FmFgYKA%2B//xzzlQBAIA6yS8DliSNGTPG6%2B8DAgAA1BV%2BeYnwqsWLF2vjxo3q2bOnmjdv7tU3f/58H1UFAABwfX4bsLKysnT77bdLkr788kuvPi4dAgAAf%2BZ3AevIkSOKjo7W1q1bfV0KAADAD%2BJ392DFxMTo1KlTntcPP/ywTp486cOKAAAAvh%2B/C1jXPvf0b3/7m7799lsfVQMAAPD9%2BV3AAgAAqOv8LmA5HI4KN7FzUzsAAKhL/O4md2OMxo0bJ6fTKUm6ePGiJk6cWOExDWvXrvVFeQAAADfkdwFr7NixXq/HjBnjo0oAAAB%2BGL8LWDy9HQAA1HV%2Bdw8WAABAXUfAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhY35GWliaHw6Hk5GRPW0lJiaZOnapWrVqpefPmGjp0qI4dO%2BbDKgEAgL8jYP3Drl27tHTpUt1%2B%2B%2B1e7cnJyfrggw/07rvv6rPPPtO5c%2Bc0ZMgQXb582UeVAgAAf0fAknTu3DmNHj1ay5YtU4sWLTztbrdbb7zxhl5%2B%2BWXde%2B%2B96t69u1auXKm9e/dq48aNPqwYAAD4MwKWpMmTJ2vw4MG69957vdqzsrJUVlamxMRET1tkZKRiY2OVmZlZ22UCAIA6IsDXBfjau%2B%2B%2Bq6ysLO3evbtCX0FBgZo0aeJ1VkuSwsPDVVBQUOU6S0pKVFJS4nldXFxsr2AAAOD3GvQZrLy8PD355JNatWqVmjZtWu33GWPkcDiq7E9LS5PL5fIsUVFRNsoFAAB1RIMOWFlZWSosLFSPHj0UEBCggIAAbd68WX/4wx8UEBCg8PBwlZaWqqioyOt9hYWFCg8Pr3K9c%2BfOldvt9ix5eXk1vSsAAMCPNOhLhAMGDNDevXu92h599FF16tRJs2fPVlRUlAIDA5WRkaHhw4dLkvLz87Vv3z7Nnz%2B/yvU6nU45nc4arR0AAPivBh2wgoODFRsb69XWvHlztWzZ0tP%2B%2BOOP66mnnlLLli0VGhqqmTNnqmvXrhVuiAcAALiqQQes6njllVcUEBCg4cOH68KFCxowYICWL1%2Buxo0b%2B7o0AADgpxzGGOPrIuq74uJiuVwuud1uhYSEWF13z3npVtcHAIAv7H5hoPV11uTv3xtp0De5AwAA1AQCFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsa/ABKy0tTb169VJwcLDCwsL0i1/8QgcPHvQaU1JSoqlTp6pVq1Zq3ry5hg4dqmPHjvmoYgAA4O8afMDavHmzJk%2BerO3btysjI0OXLl1SYmKivv32W8%2BY5ORkffDBB3r33Xf12Wef6dy5cxoyZIguX77sw8oBAIC/CvB1Ab6Wnp7u9fqtt95SWFiYsrKydPfdd8vtduuNN97Qn/70J917772SpJUrVyoqKkobN27U/fff74uyAQCAH2vwZ7Cu5Xa7JUmhoaGSpKysLJWVlSkxMdEzJjIyUrGxscrMzKx0HSUlJSouLvZaAABAw0HA%2Bg5jjGbMmKG%2BffsqNjZWklRQUKAmTZqoRYsWXmPDw8NVUFBQ6XrS0tLkcrk8S1RUVI3XDgAA/AcB6zumTJmiL7/8Uu%2B8884Nxxpj5HA4Ku2bO3eu3G63Z8nLy7NdKgAA8GMErH%2BYOnWq1q1bp02bNqlt27ae9oiICJWWlqqoqMhrfGFhocLDwytdl9PpVEhIiNcCAAAajgYfsIwxmjJlitauXatPPvlE0dHRXv09evRQYGCgMjIyPG35%2Bfnat2%2Bf4uPja7tcAABQBzT4bxFOnjxZq1ev1n/%2B538qODjYc1%2BVy%2BVSUFCQXC6XHn/8cT311FNq2bKlQkNDNXPmTHXt2tXzrUIAAIDvavABa8mSJZKkhIQEr/a33npL48aNkyS98sorCggI0PDhw3XhwgUNGDBAy5cvV%2BPGjWu5WgAAUBc0%2BIBljLnhmKZNm2rRokVatGhRLVQEAADqugZ/DxYAAIBtBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAqqZXX31V0dHRatq0qXr06KGtW7f6uiQAAOCnCFjVsGbNGiUnJ2vevHn6/PPP9bOf/UyDBg1Sbm6ur0sDAAB%2BiIBVDQsWLNDjjz%2BuX/3qV%2BrcubMWLlyoqKgoLVmyxNelAQAAPxTg6wL8XWlpqbKysjRnzhyv9sTERGVmZlb6npKSEpWUlHheu91uSVJxcbH1%2Bi6XfGt9nQAA1Laa%2BB15dZ3GGOvrvhEC1g188803unz5ssLDw73aw8PDVVBQUOl70tLS9Jvf/KZCe1RUVI3UCABAXed6uebWffbsWblcrprbQCUIWNXkcDi8XhtjKrRdNXfuXM2YMcPz%2BsyZM2rfvr1yc3Nr/T9wXVNcXKyoqCjl5eUpJCTE1%2BX4Leapepin6mGeqod5qj5/mStjjM6ePavIyMha3zYB6wZatWqlxo0bVzhbVVhYWOGs1lVOp1NOp7NCu8vl4oeymkJCQpiramCeqod5qh7mqXqYp%2Brzh7ny1YkNbnK/gSZNmqhHjx7KyMjwas/IyFB8fLyPqgIAAP6MM1jVMGPGDCUlJalnz56Ki4vT0qVLlZubq4kTJ/q6NAAA4Icap6SkpPi6CH8XGxurli1bKjU1VS%2B99JIuXLigP/3pT%2BrWrVu119G4cWMlJCQoIIBMeyPMVfUwT9XDPFUP81Q9zFP1NfS5chhffHcRAACgHuMeLAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwPqBUlJS5HA4vJaIiAhPvzFGKSkpioyMVFBQkBISErR//36vdRQVFSkpKUkul0sul0tJSUk6c%2BZMbe9KjbrRPI0bN65Cf58%2BfbzWUVJSoqlTp6pVq1Zq3ry5hg4dqmPHjtX2rtSK48ePa8yYMWrZsqWaNWumO%2B64Q1lZWZ5%2BjqsrbjRPHFfSLbfcUmEOHA6HJk%2BeLKl6%2B5%2Bbm6sHH3xQzZs3V6tWrTRt2jSVlpb6YndqzI3mKSEhoULfiBEjvNbREH7mLl26pF//%2BteKjo5WUFCQbr31Vj3//PMqLy/3jOHz6RoGP8hzzz1nunTpYvLz8z1LYWGhp//FF180wcHB5s9//rPZu3evefjhh03r1q38eJuDAAAIkUlEQVRNcXGxZ8zAgQNNbGysyczMNJmZmSY2NtYMGTLEF7tTY240T2PHjjUDBw706j99%2BrTXOiZOnGjatGljMjIyTHZ2tunfv7/p1q2buXTpUm3vTo363//9X9O%2BfXszbtw4s2PHDpOTk2M2btxovvrqK88YjqvqzRPHlTGFhYVe%2B5%2BRkWEkmU2bNhljbrz/ly5dMrGxsaZ///4mOzvbZGRkmMjISDNlyhQf7pV9N5qnfv36mfHjx3uNOXPmjNc66vvPnDHG/Pa3vzUtW7Y0f/3rX01OTo55//33zU033WQWLlzoGcPnkzcC1g/03HPPmW7dulXaV15ebiIiIsyLL77oabt48aJxuVzmtddeM8YY89///d9Gktm%2BfbtnzLZt24wk8z//8z81W3wtut48GXPlF%2BHPf/7zKvvPnDljAgMDzbvvvutpO378uGnUqJFJT0%2B3WquvzZ492/Tt27fKfo6rK240T8ZwXFXmySefNB06dDDl5eXV2v%2B//e1vplGjRub48eOeMe%2B8845xOp3G7XbXev215bvzZMyVgPXkk09WOb4h/MwZY8zgwYPNY4895tU2bNgwM2bMGGMMn0%2BV4RLhj3D48GFFRkYqOjpaI0aM0JEjRyRJOTk5KigoUGJiomes0%2BlUv379lJmZKUnatm2bXC6Xevfu7RnTp08fuVwuz5j6oqp5uurTTz9VWFiYOnbsqPHjx6uwsNDTl5WVpbKyMq%2B5jIyMVGxsbL2bp3Xr1qlnz5765S9/qbCwMHXv3l3Lli3z9HNcXXGjebqK4%2Br/lZaWauXKlXrsscfkcDiqtf/btm1TbGys1x/Jvf/%2B%2B1VSUuJ1ObY%2BuXaerlq1apVatWqlLl26aObMmTp79qynryH8zElS37599fHHH%2BvQoUOSpC%2B%2B%2BEKfffaZHnjgAUl8PlWGgPUD9e7dW2%2B//bbWr1%2BvZcuWqaCgQPHx8Tp9%2BrTnD0Nf%2B8egw8PDPX0FBQUKCwursN6wsLAKf1i6LrvePEnSoEGDtGrVKn3yySd6%2BeWXtWvXLt1zzz0qKSmRdGWemjRpohYtWnit97tzWV8cOXJES5YsUUxMjNavX6%2BJEydq2rRpevvttyWJ4%2BofbjRPEsfVtT788EOdOXNG48aNk1S9/S8oKKhwrLVo0UJNmjSpl3MkVZwnSRo9erTeeecdffrpp3rmmWf05z//WcOGDfP0N4SfOUmaPXu2Ro4cqU6dOikwMFDdu3dXcnKyRo4cKYnPp8o0zOfXWzBo0CDPv7t27aq4uDh16NBBK1as8NxM%2B93/A5Ku3AD43bZr%2BysbU9ddb55mzJihhx9%2B2NMfGxurnj17qn379vroo4%2B8PsSuVd/mSZLKy8vVs2dPpaamSpK6d%2B%2Bu/fv3a8mSJXrkkUc84xr6cVWdeeK48vbGG29o0KBBXmejKtPQjqVrVTZP48eP9/w7NjZWMTEx6tmzp7Kzs3XnnXdKahjztGbNGq1cuVKrV69Wly5dtGfPHiUnJysyMlJjx471jGvon0/fxRksS5o3b66uXbvq8OHDnm/JXZvICwsLPek%2BIiJCJ0%2BerLCeU6dOVfg/gPrku/NUmdatW6t9%2B/ae/oiICJWWlqqoqMhr3Hfnsr5o3bq1brvtNq%2B2zp07Kzc3V5I4rv7hRvNU1Xsa6nF19OhRbdy4Ub/61a88bdXZ/4iIiArHWlFRkcrKyurdHEmVz1Nl7rzzTgUGBnodS/X9Z06Snn76ac2ZM0cjRoxQ165dlZSUpOnTpystLU0Sn0%2BVIWBZUlJSogMHDqh169aKjo5WRESEMjIyPP2lpaXavHmz4uPjJUlxcXFyu93auXOnZ8yOHTvkdrs9Y%2Bqj785TZU6fPq28vDxPf48ePRQYGOg1l/n5%2Bdq3b1%2B9m6e77rpLBw8e9Go7dOiQ2rdvL0kcV/9wo3mqTEM%2Brt566y2FhYVp8ODBnrbq7H9cXJz27dun/Px8z5gNGzbI6XSqR48etbcDtaSyearM/v37VVZW5jmWGsLPnCSdP39ejRp5R4bGjRt7HtPA51MlfHRzfZ331FNPmU8//dQcOXLEbN%2B%2B3QwZMsQEBwebv//978aYK19XdblcZu3atWbv3r1m5MiRlX5d9fbbbzfbtm0z27ZtM127dq13X1e93jydPXvWPPXUUyYzM9Pk5OSYTZs2mbi4ONOmTRuveZo4caJp27at2bhxo8nOzjb33HNPvfo6/VU7d%2B40AQEB5oUXXjCHDx82q1atMs2aNTMrV670jOG4uvE8cVz9v8uXL5t27dqZ2bNnV%2Bi70f5ffUzDgAEDTHZ2ttm4caNp27ZtvXtMgzFVz9NXX31lfvOb35hdu3aZnJwc89FHH5lOnTqZ7t27ex0n9f1nzpgr38xt06aN5zENa9euNa1atTKzZs3yjOHzyRsB6we6%2BnyPwMBAExkZaYYNG2b279/v6S8vLzfPPfeciYiIME6n09x9991m7969Xus4ffq0GT16tAkODjbBwcFm9OjRpqioqLZ3pUZdb57Onz9vEhMTzU9%2B8hMTGBho2rVrZ8aOHWtyc3O91nHhwgUzZcoUExoaaoKCgsyQIUMqjKkv/vKXv5jY2FjjdDpNp06dzNKlS736Oa6uuN48cVz9v/Xr1xtJ5uDBgxX6qrP/R48eNYMHDzZBQUEmNDTUTJkyxVy8eLG2yq81Vc1Tbm6uufvuu01oaKhp0qSJ6dChg5k2bVqFZ6o1hJ%2B54uJi8%2BSTT5p27dqZpk2bmltvvdXMmzfPlJSUeMbw%2BeTNYYwxvj6LBgAAUJ9wDxYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMv%2BD74Ok2%2Bth0RTAAAAAElFTkSuQmCC"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common-9216223717743914426">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">563.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">735.0</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">686.0</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">637.0</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">808.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">514.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">759.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">710.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">661.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">612.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="other">
        <td class="fillremaining">Other values (2)</td>
        <td class="number">128</td>
        <td class="number">16.7%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme-9216223717743914426">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">514.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">563.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">588.0</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">612.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">637.0</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">710.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">735.0</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">759.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">784.0</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">808.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_X3">X3<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>7</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>0.9%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (n)</th>
                    <td>0</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>

        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>318.5</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>245</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>416.5</td>
                </tr>
                <tr class="ignore">
                    <th>Zeros (%)</th>
                    <td>0.0%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram7550354334602978036">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAAbVJREFUeJzt28Gt00AUhtHr6G3tAiK7C4qgJ9b0RBF0YSsFOGLBgpgFhA3wi6CncQznLBNZdyzNJ8vSuNu2bavGlmWpaZpaj%2BXg5nmucRybznxpOu27vu%2Br6tsND8OwxxI4kHVda5qmH/umpV0C6bquqqqGYdgtkDfvPjx8zcf3b59uxv/kvm9aOjWfCAciEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQ7PI9CL/nG5Ln4gkCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBIKnO%2B7%2BN8e9eUyrI/X/wtF9TxAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEOxy3H3btqqqWtf1p/%2B%2BfP7Uejl/7FfrTVrdS4t1PTrjNefcf7vvm5a6bYepy7LUNE2tx3Jw8zzXOI5NZ%2B4SyO12q8vlUn3fV9d1rcdzMNu21fV6rfP5XKdT27eCXQKBo/CSDoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUDwFamQYkbxUcqdAAAAAElFTkSuQmCC">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives7550354334602978036,#minihistogram7550354334602978036"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives7550354334602978036">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles7550354334602978036"
                                                  aria-controls="quantiles7550354334602978036" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram7550354334602978036" aria-controls="histogram7550354334602978036"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common7550354334602978036" aria-controls="common7550354334602978036"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme7550354334602978036" aria-controls="extreme7550354334602978036"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>

    </ul>

    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles7550354334602978036">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>245</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>245</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>294</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>318.5</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>343</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>416.5</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>416.5</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>171.5</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>49</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>43.626</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.13697</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>0.11659</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>318.5</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>32.667</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>0.53342</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>244610</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>1903.3</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>6.1 KiB</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram7550354334602978036">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAIABJREFUeJzt3X9UVXW%2B//HXUeH4IziBCAcSjWlRt8TfOoo1BVkY%2BWM11kyO5tUyqkk0f00TWY3UJE2/ppm8Nf1Qs0nTWjfLuZmJVloLLQVJsa5XixQNQk0BTY%2Bon%2B8fjefrERCsD5zD4flYa6/F/nw%2BZ/P%2BsNn56rP3OTiMMUYAAACwppW/CwAAAAg2BCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWBbUASsnJ0f9%2B/dXWFiYoqOjdcMNN2jbtm0%2BYzwejyZNmqSoqCh16NBBI0aM0O7du33G7Nq1S8OHD1eHDh0UFRWlyZMn69ixY005FQAA0Iy08XcBjWnNmjWaOHGi%2Bvfvr%2BPHj2vmzJlKS0vTF198oQ4dOkiSpkyZon/9619avHixOnbsqOnTp2vYsGHKz89X69atdeLECQ0dOlSdOnXSJ598ov3792vcuHEyxujZZ59tUB0nT57Ut99%2Bq7CwMDkcjsacMgAA%2BDdjjKqqqhQXF6dWrZp4Tcm0IOXl5UaSWbNmjTHGmIMHD5qQkBCzePFi75g9e/aYVq1amRUrVhhjjFm%2BfLlp1aqV2bNnj3fM66%2B/bpxOp6moqGjQ9y0pKTGS2NjY2NjY2PywlZSUWEwTDRPUK1hnqqiokCRFRkZKkvLz81VdXa20tDTvmLi4OCUlJSkvL09DhgzRunXrlJSUpLi4OO%2BYIUOGyOPxKD8/X6mpqfV%2B37CwMElSSUmJwsPDbU4JAADUobKyUvHx8d5/h5tSiwlYxhhNmzZNV1xxhZKSkiRJZWVlCg0NVUREhM/YmJgYlZWVecfExMT49EdERCg0NNQ75kwej0cej8e7X1VVJUkKDw8nYAEA0MT88XhOUD/kfrrMzExt3rxZr7/%2Ber1jjTE%2BJ6O2E3PmmNPl5OTI5XJ5t/j4%2BJ9eOAAAaHZaRMCaNGmSli1bpg8//FCdO3f2trvdbh07dkwHDhzwGV9eXu5dtXK73TVWqg4cOKDq6uoaK1unZGVlqaKiwruVlJRYnhEAAAhkQR2wjDHKzMzUW2%2B9pQ8%2B%2BEAJCQk%2B/X379lVISIhyc3O9baWlpSoqKtKgQYMkScnJySoqKlJpaal3zMqVK%2BV0OtW3b99av6/T6fTeDuS2IAAALU9QP4M1ceJELVq0SO%2B8847CwsK8K1Eul0vt2rWTy%2BXShAkTNH36dHXs2FGRkZGaMWOGunfvrmuuuUaSlJaWpssuu0xjx47VE088oe%2B//14zZsxQRkYGwQkAANTKYYwx/i6isdT1jNT8%2BfM1fvx4SdLRo0f1hz/8QYsWLdKRI0c0ePBgPffccz7PTe3atUt33323PvjgA7Vr106jR4/Wk08%2BKafT2aA6Kisr5XK5VFFRQSgDAKCJ%2BPPf36AOWIGCgAUAQNPz57%2B/Qf0MFgAAgD8QsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMCyoP6gUeDn6Ddzhb9LOCcbH73O3yU0GD9bAMGOFSwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGBZUAestWvXavjw4YqLi5PD4dDbb7/t0%2B9wOGrdnnjiCe%2BYCy%2B8sEb/fffd19RTAQAAzUgbfxfQmA4fPqyePXvq1ltv1Y033lijv7S01Gf/vffe04QJE2qMffjhh5WRkeHdP%2B%2B88xqnYAAAEBSCOmClp6crPT29zn632%2B2z/8477yg1NVW/%2BMUvfNrDwsJqjAUAAKhLUN8iPBffffed3n33XU2YMKFG31/%2B8hd17NhRvXr10qOPPqpjx46d9Vgej0eVlZU%2BGwAAaDmCegXrXCxYsEBhYWEaOXKkT/s999yjPn36KCIiQp999pmysrJUXFysl19%2Buc5j5eTkKDs7u7FLBgAAAYqA9W/z5s3TmDFj1LZtW5/2qVOner/u0aOHIiIidNNNN3lXtWqTlZWladOmefcrKysVHx/fOIUDAICAQ8CS9PHHH2vbtm1asmRJvWMHDhwoSdqxY0edAcvpdMrpdFqtEQAANB88gyVp7ty56tu3r3r27Fnv2E2bNkmSYmNjG7ssAADQTAX1CtahQ4e0Y8cO735xcbEKCwsVGRmpLl26SPrx9t2bb76pp556qsbr161bp/Xr1ys1NVUul0sbNmzQ1KlTNWLECO/rAQAAzhTUAWvjxo1KTU317p96LmrcuHF65ZVXJEmLFy%2BWMUa/%2B93varze6XRqyZIlys7OlsfjUdeuXZWRkaF77723SeoHAADNU1AHrJSUFBljzjrmjjvu0B133FFrX58%2BfbR%2B/frGKA0AAAQxnsECAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZUEdsNauXavhw4crLi5ODodDb7/9tk//%2BPHj5XA4fLaBAwf6jPF4PJo0aZKioqLUoUMHjRgxQrt3727KaQAAgGYmqAPW4cOH1bNnT82ZM6fOMdddd51KS0u92/Lly336p0yZoqVLl2rx4sX65JNPdOjQIQ0bNkwnTpxo7PIBAEAz1cbfBTSm9PR0paenn3WM0%2BmU2%2B2uta%2BiokJz587VP//5T11zzTWSpNdee03x8fFatWqVhgwZYr1mAADQ/AX1ClZDfPTRR4qOjtbFF1%2BsjIwMlZeXe/vy8/NVXV2ttLQ0b1tcXJySkpKUl5fnj3IBAEAzENQrWPVJT0/Xb37zG3Xt2lXFxcV68MEHdfXVVys/P19Op1NlZWUKDQ1VRESEz%2BtiYmJUVlZW53E9Ho88Ho93v7KystHmAAAAAk%2BLDlg333yz9%2BukpCT169dPXbt21bvvvquRI0fW%2BTpjjBwOR539OTk5ys7OtlorAABoPlr8LcLTxcbGqmvXrtq%2Bfbskye1269ixYzpw4IDPuPLycsXExNR5nKysLFVUVHi3kpKSRq0bAAAEFgLWafbv36%2BSkhLFxsZKkvr27auQkBDl5uZ6x5SWlqqoqEiDBg2q8zhOp1Ph4eE%2BGwAAaDmC%2BhbhoUOHtGPHDu9%2BcXGxCgsLFRkZqcjISM2aNUs33nijYmNj9c033%2Bj%2B%2B%2B9XVFSUfv3rX0uSXC6XJkyYoOnTp6tjx46KjIzUjBkz1L17d%2B%2B7CgEAAM4U1AFr48aNSk1N9e5PmzZNkjRu3Dg9//zz2rJli1599VUdPHhQsbGxSk1N1ZIlSxQWFuZ9zV//%2Ble1adNGv/3tb3XkyBENHjxYr7zyilq3bt3k8wEAAM1DUAeslJQUGWPq7H///ffrPUbbtm317LPP6tlnn7VZGgAACGI8gwUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWtfF3AY1p7dq1euKJJ5Sfn6/S0lItXbpUN9xwgySpurpaDzzwgJYvX66vv/5aLpdL11xzjR577DHFxcV5j3HhhRdq586dPsf94x//qMcee6xJ5wLAf/rNXOHvEs7Jxkev83cJQIsX1CtYhw8fVs%2BePTVnzpwafT/88IMKCgr04IMPqqCgQG%2B99Zb%2B7//%2BTyNGjKgx9uGHH1Zpaal3e%2BCBB5qifAAA0EwF9QpWenq60tPTa%2B1zuVzKzc31aXv22Wf1y1/%2BUrt27VKXLl287WFhYXK73Y1aKwAACB5BvYJ1rioqKuRwOHT%2B%2Bef7tP/lL39Rx44d1atXLz366KM6duyYnyoEAADNQVCvYJ2Lo0eP6r777tPo0aMVHh7ubb/nnnvUp08fRURE6LPPPlNWVpaKi4v18ssv13ksj8cjj8fj3a%2BsrGzU2gEAQGAhYOnHB95HjRqlkydP6rnnnvPpmzp1qvfrHj16KCIiQjfddJN3Vas2OTk5ys7ObtSaAQBA4Grxtwirq6v129/%2BVsXFxcrNzfVZvarNwIEDJUk7duyoc0xWVpYqKiq8W0lJidWaAQBAYGvRK1inwtX27dv14Ycf1rkidbpNmzZJkmJjY%2Bsc43Q65XQ6rdUJAACal6AOWIcOHfJZaSouLlZhYaEiIyMVFxenm266SQUFBfqf//kfnThxQmVlZZKkyMhIhYaGat26dVq/fr1SU1Plcrm0YcMGTZ06VSNGjPB5lyEAAMDpAvYW4WuvvaajR4/%2BrGNs3LhRvXv3Vu/evSVJ06ZNU%2B/evfXQQw9p9%2B7dWrZsmXbv3q1evXopNjbWu%2BXl5Un6cSVqyZIlSklJ0WWXXaaHHnpIGRkZev3113/2/AAAQPAK2BWsadOmKTMzUzfffLMmTJigX/7yl%2Bd8jJSUFBlj6uw/W58k9enTR%2BvXrz/n7wsAAFq2gF3B%2BvbbbzVv3jyVlpbqiiuuULdu3fTUU09p7969/i4NAADgrAI2YLVp00YjR47UsmXLtGvXLo0bN07z5s1T586dNXLkSL377rv1rkABAAD4Q8AGrNO53W4NHjxYKSkpcjgc2rhxo0aPHq3ExER9/PHH/i4PAADAR0AHrH379umZZ55Rz549dfnll6u8vFxvv/22du7cqT179mjYsGH6z//8T3%2BXCQAA4CNgH3L/9a9/reXLlyshIUG33367xo0bp06dOnn7zzvvPN177736%2B9//7scqAQAAagrYgBUeHq5Vq1bpV7/6VZ1jYmNjtX379iasCgAAoH4BG7AWLFhQ7xiHw6GLLrqoCaoBAABouIB9Bmvq1KmaM2dOjfb/%2Bq//0vTp0/1QEQAAQMMEbMB68803vX9Y%2BXTJyclasmSJHyoCAABomIANWPv27VNERESN9vDwcO3bt88PFQEAADRMwAasiy66SO%2B//36N9vfff18JCQl%2BqAgAAKBhAvYh9ylTpmjKlCnav3%2B/rr76aknS6tWr9fjjj%2BvJJ5/0c3UAAAB1C9iAlZGRoaNHj2r27Nn605/%2BJEnq3Lmz/v73v%2Bu2227zc3UAAAB1C9iAJUmTJk3SpEmTVFpaqnbt2un888/3d0kAAAD1CuiAdUpsbKy/SwAAAGiwgH3Ife/evbr11lvVpUsXtW3bVqGhoT4bAABAoArYFazx48frq6%2B%2B0h/%2B8AfFxsbK4XD4uyQAAIAGCdiAtXbtWq1du1a9e/f2dykAAADnJGBvEXbu3JlVKwAA0CwFbMD661//qqysLO3evdvfpQAAAJyTgL1FOHbsWFVVValr164KDw9XSEiIT395ebmfKgMAADi7gA1Yjz32mL9LAAAA%2BEkCNmBNmDDB3yUAAAD8JAH7DJYkffPNN5o1a5bGjh3rvSW4cuVKffnll36uDAAAoG4BG7A%2B/vhjdevWTWvWrNEbb7yhQ4cOSZIKCgr00EMP%2Bbk6AACAugVswPrjH/%2BoWbNm6cMPP/T55Parr75a69ev92NlAAAAZxewAWvz5s266aabarRHR0dr7969fqgIAACgYQI2YJ1//vkqKyur0V5YWKgLLrjADxUBAAA0TMAGrFGjRum%2B%2B%2B7T3r17vZ/o/umnn2rGjBm65ZZb/FwdAABA3QI2YM2ePVtut1uxsbE6dOiQLrvsMg0aNEj9%2B/fXgw8%2B2KBjrF27VsOHD1dcXJwcDofefvttn35jjGbNmqW4uDi1a9dOKSkp2rp1q8%2BYAwcOaOzYsXK5XHK5XBo7dqwOHjxobZ4AACD4BGzACg0N1ZIlS/TFF19o0aJFmjdvnrZu3arXX39dbdo07OO7Dh8%2BrJ49e2rOnDm19j/%2B%2BON6%2BumnNWfOHG3YsEFut1vXXnutqqqqvGNGjx6twsJCrVixQitWrFBhYaHGjh1rZY4AACA4BewHjZ5y8cUX6%2BKLL/5Jr01PT1d6enqtfcYYPfPMM5o5c6ZGjhwpSVqwYIFiYmK0aNEi3Xnnnfryyy%2B1YsUKrV%2B/XgMGDJAkvfTSS0pOTta2bdt0ySWX/LRJAQCAoBawAeuOO%2B44a/%2BLL774s45fXFyssrIypaWleducTqeuuuoq5eXl6c4779S6devkcrm84UqSBg4cKJfLpby8vDoDlsfjkcfj8e5XVlb%2BrFoBAEDzErABq7S01Ge/urpaW7duVVVVla688sqfffxT71CMiYnxaY%2BJidHOnTu9Y6Kjo2u8Njo6utZ3OJ6Sk5Oj7Ozsn10jAABongI2YP3rX/%2Bq0Xb8%2BHH9/ve/16WXXmrt%2B5x6h%2BIpxhiftjP7axtzpqysLE2bNs27X1lZqfj4eAvVAgCA5iBgH3KvTZs2bTRjxgw98cQTP/tYbrdbkmqsRJWXl3tXtdxut7777rsar927d2%2BNla/TOZ1OhYeH%2B2wAAKDlaFYBS5K%2B/vprVVdX/%2BzjJCQkyO12Kzc319t27NgxrVmzRoMGDZIkJScnq6KiQp999pl3zKeffqqKigrvGAAAgDMF7C3Ce%2B%2B912ffGKPS0lItW7ZMY8aMadAxDh06pB07dnj3i4uLVVhYqMjISHXp0kVTpkzR7NmzlZiYqMTERM2ePVvt27fX6NGjJUmXXnqprrvuOmVkZOiFF16Q9OPD98OGDeMdhAAAoE4BG7DWrVvns9%2BqVSt16tRJjz32mDIyMhp0jI0bNyo1NdW7f%2Bq5qHHjxumVV17RvffeqyNHjujuu%2B/WgQMHNGDAAK1cuVJhYWHe1yxcuFCTJ0/2vttwxIgRdX6uFgAAgCQ5jDHG30UEu8rKSrlcLlVUVPA8VjPSb%2BYKf5dwTjY%2Bep2/S2iw5vazbW6a0%2B8C0Jj8%2Be9vs3sGCwAAINAF7C3C/v37n/WjEE53%2BkPoAAAA/hawASs1NVUvvPCCLr74YiUnJ0uS1q9fr23btunOO%2B%2BU0%2Bn0c4UAAAC1C9iAdfDgQU2cOFGzZ8/2aZ85c6a%2B%2B%2B47vfzyy36qDAAA4OwC9hmsN954Q7feemuN9vHjx%2BvNN9/0Q0UAAAANE7ABy%2Bl0Ki8vr0Z7Xl4etwcBAEBAC9hbhJMnT9Zdd92lTZs2aeDAgZJ%2BfAbrpZde0v333%2B/n6gAAAOoWsAFr5syZSkhI0N/%2B9jfNmzdP0o%2BfrP7SSy95P2kdAAAgEAVswJKk0aNHE6YAAECzE7DPYEk/fgLrK6%2B8ooceekgHDhyQJH3%2B%2BecqLS31c2UAAAB1C9gVrKKiIl1zzTVq3769SkpKNH78eEVEROiNN97Q7t27tWDBAn%2BXCAAAUKuAXcGaOnWqRo8era%2B%2B%2Bkpt27b1tg8dOlRr1671Y2UAAABnF7ArWBs2bNDzzz9f48/lXHDBBdwiBAAAAS1gV7BCQ0N16NChGu3bt29XVFSUHyoCAABomIANWCNGjNAjjzyi48ePS5IcDof27Nmj%2B%2B67TyNHjvRzdQAAAHUL2ID11FNP6dtvv5Xb7daRI0d09dVX6xe/%2BIXatm1b4%2B8TAgAABJKAfQbL5XIpLy9Pubm5Kigo0MmTJ9WnTx8NGTKkxnNZAAAAgSQgA1Z1dbWuv/56Pffcc0pLS1NaWpq/SwIAAGiwgLxFGBISok2bNrFSBQAAmqWADFiSdMstt2j%2B/Pn%2BLgMAAOCcBeQtwlPmzJmjVatWqV%2B/furQoYNP3%2BOPP%2B6nqgAAAM4uYANWfn6%2BevToIUnavHmzTx%2B3DgEAQCALuID19ddfKyEhQR9//LG/SwEAAPhJAu4ZrMTERO3du9e7f/PNN%2Bu7777zY0UAAADnJuACljHGZ3/58uU6fPiwn6oBAAA4dwEXsAAAAJq7gAtYDoejxkPsPNQOAACak4B7yN0Yo/Hjx8vpdEqSjh49qrvuuqvGxzS89dZb/igPAACgXgG3gjVu3DhFR0fL5XLJ5XLplltuUVxcnHf/1GbLhRde6F01O32bOHGiJCklJaVG36hRo6x9fwAAEHwCbgWrqT%2B9fcOGDTpx4oR3v6ioSNdee61%2B85vfeNsyMjL08MMPe/fbtWvXpDUCAIDmJeACVlPr1KmTz/5jjz2miy66SFdddZW3rX379nK73U1dGgAAaKYC7hahPx07dkyvvfaabrvtNp8H6xcuXKioqCh169ZNM2bMUFVVlR%2BrBAAAga7Fr2Cd7u2339bBgwc1fvx4b9uYMWOUkJAgt9utoqIiZWVl6fPPP1dubm6dx/F4PPJ4PN79ysrKxiwbAAAEGALWaebOnav09HTFxcV52zIyMrxfJyUlKTExUf369VNBQYH69OlT63FycnKUnZ3d6PUCAIDAxC3Cf9u5c6dWrVql22%2B//azj%2BvTpo5CQEG3fvr3OMVlZWaqoqPBuJSUltssFAAABjBWsf5s/f76io6M1dOjQs47bunWrqqurFRsbW%2BcYp9Pp/RwvAADQ8hCwJJ08eVLz58/XuHHj1KbN//%2BRfPXVV1q4cKGuv/56RUVF6YsvvtD06dPVu3dvXX755X6sGAAABDIClqRVq1Zp165duu2223zaQ0NDtXr1av3tb3/ToUOHFB8fr6FDh%2BpPf/qTWrdu7adqAQBAoCNgSUpLS5MxpkZ7fHy81qxZ44eKAABAc8ZD7gAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYxt8ibOb6zVzh7xIA4Cdrbv8N2/jodf4u4Zw0p59vc/vZ1ocVLAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAy1p8wJo1a5YcDofP5na7vf3GGM2aNUtxcXFq166dUlJStHXrVj9WDAAAAl2LD1iS1K1bN5WWlnq3LVu2ePsef/xxPf3005ozZ442bNggt9uta6%2B9VlVVVX6sGAAABDIClqQ2bdrI7XZ7t06dOkn6cfXqmWee0cyZMzVy5EglJSVpwYIF%2BuGHH7Ro0SI/Vw0AAAIVAUvS9u3bFRcXp4SEBI0aNUpff/21JKm4uFhlZWVKS0vzjnU6nbrqqquUl5fnr3IBAECAa%2BPvAvxtwIABevXVV3XxxRfru%2B%2B%2B05///GcNGjRIW7duVVlZmSQpJibG5zUxMTHauXNnncf0eDzyeDze/crKysYpHgAABKQWH7DS09O9X3fv3l3Jycm66KKLtGDBAg0cOFCS5HA4fF5jjKnRdrqcnBxlZ2c3TsEAACDgcYvwDB06dFD37t21fft277sJT61knVJeXl5jVet0WVlZqqio8G4lJSWNWjMAAAgsBKwzeDweffnll4qNjVVCQoLcbrdyc3O9/ceOHdOaNWs0aNCgOo/hdDoVHh7uswEAgJajxd8inDFjhoYPH64uXbqovLxcf/7zn1VZWalx48bJ4XBoypQpmj17thITE5WYmKjZs2erffv2Gj16tL9LBwAAAarFB6zdu3frd7/7nfbt26dOnTpp4MCBWr9%2Bvbp27SpJuvfee3XkyBHdfffdOnDggAYMGKCVK1cqLCzMz5UDAIBA1eID1uLFi8/a73A4NGvWLM2aNatpCgIAAM0ez2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZS0%2BYOXk5Kh///4KCwtTdHS0brjhBm3bts1nTEpKihwOh882atQoP1UMAAACXYsPWGvWrNHEiRO1fv165ebm6vjx40pLS9Phw4d9xmVkZKi0tNS7vfDCC36qGAAABLo2/i7A31asWOGzP3/%2BfEVHRys/P19XXnmlt719%2B/Zyu91NXR4AAGiGWvwK1pkqKiokSZGRkT7tCxcuVFRUlLp166YZM2aoqqqqzmN4PB5VVlb6bAAAoOVo8StYpzPGaNq0abriiiuUlJTkbR8zZowSEhLkdrtVVFSkrKwsff7558rNza31ODk5OcrOzm6qsgEAQIAhYJ0mMzNTmzdv1ieffOLTnpGR4f06KSlJiYmJ6tevnwoKCtSnT58ax8nKytK0adO8%2B5WVlYqPj2%2B8wgEAQEAhYP3bpEmTtGzZMq1du1adO3c%2B69g%2BffooJCRE27dvrzVgOZ1OOZ3OxioVAAAEuBYfsIwxmjRpkpYuXaqPPvpICQkJ9b5m69atqq6uVmxsbBNUCAAAmpsWH7AmTpyoRYsW6Z133lFYWJjKysokSS6XS%2B3atdNXX33cAz5iAAAMtklEQVSlhQsX6vrrr1dUVJS%2B%2BOILTZ8%2BXb1799bll1/u5%2BoBAEAgavHvInz%2B%2BedVUVGhlJQUxcbGerclS5ZIkkJDQ7V69WoNGTJEl1xyiSZPnqy0tDStWrVKrVu39nP1AAAgELX4FSxjzFn74%2BPjtWbNmiaqBgAABIMWv4IFAABgGwELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwGui5555TQkKC2rZtq759%2B%2Brjjz/2d0kAACBAEbAaYMmSJZoyZYpmzpypTZs26Ve/%2BpXS09O1a9cuf5cGAAACEAGrAZ5%2B%2BmlNmDBBt99%2Buy699FI988wzio%2BP1/PPP%2B/v0gAAQABq4%2B8CAt2xY8eUn5%2Bv%2B%2B67z6c9LS1NeXl5tb7G4/HI4/F49ysqKiRJlZWV1us74Tls/Zhonhrj96ux8HvbuPhdaDzN6WcrNa%2Bfb2P8bE8d0xhj/dj1IWDVY9%2B%2BfTpx4oRiYmJ82mNiYlRWVlbra3JycpSdnV2jPT4%2BvlFqBCTJ9ZS/K0Cg4Heh8fCzbTyN%2BbPdv3%2B/XC5X432DWhCwGsjhcPjsG2NqtJ2SlZWladOmefdPnjyp77//Xh07dqzzNU2lsrJS8fHxKikpUXh4uF9raWotee5Sy55/S567xPxb8vxb8tylH%2B8gdenSRZGRkU3%2BvQlY9YiKilLr1q1rrFaVl5fXWNU6xel0yul0%2BrSdf/75jVbjTxEeHt4iLzapZc9datnzb8lzl5h/S55/S567JLVq1fSPnPOQez1CQ0PVt29f5ebm%2BrTn5uZq0KBBfqoKAAAEMlawGmDatGkaO3as%2BvXrp%2BTkZL344ovatWuX7rrrLn%2BXBgAAAlDrWbNmzfJ3EYEuKSlJHTt21OzZs/Xkk0/qyJEj%2Buc//6mePXv6u7SfpHXr1kpJSVGbNi0vX7fkuUste/4tee4S82/J82/Jc5f8N3%2BH8cd7FwEAAIIYz2ABAABYRsACAACwjIAFAABgGQELAADAMgJWM5eTk6P%2B/fsrLCxM0dHRuuGGG7Rt2zafMSkpKXI4HD7bqFGjfMYcOHBAY8eOlcvlksvl0tixY3Xw4MGmnMpPUt/8v/nmmxpzP7W9%2Beab3nG19f/jH//wx5TOyfPPP68ePXp4P0QwOTlZ7733nrff4/Fo0qRJioqKUocOHTRixAjt3r3b5xi7du3S8OHD1aFDB0VFRWny5Mk6duxYU0/lnJ1t7t9//70mTZqkSy65RO3bt1eXLl00efJk798FPaW5nnep/nMfzNf92eYe7Nd8bXJycuRwODRlyhRvWzBf%2B6c7c%2B4Bde0bNGtDhgwx8%2BfPN0VFRaawsNAMHTrUdOnSxRw6dMg75qqrrjIZGRmmtLTUux08eNDnONddd51JSkoyeXl5Ji8vzyQlJZlhw4Y19XTOWX3zP378uM%2B8S0tLTXZ2tunQoYOpqqryHkeSmT9/vs%2B4H374wV/TarBly5aZd99912zbts1s27bN3H///SYkJMQUFRUZY4y56667zAUXXGByc3NNQUGBSU1NNT179jTHjx83xvz480lKSjKpqammoKDA5Obmmri4OJOZmenPaTXI2ea%2BZcsWM3LkSLNs2TKzY8cOs3r1apOYmGhuvPFGn2M01/NuTP3nPpiv%2B7PNPdiv%2BTN99tln5sILLzQ9evQw99xzj7c9mK/9U2qbeyBd%2BwSsIFNeXm4kmTVr1njbrrrqKp8L70xffPGFkWTWr1/vbVu3bp2RZP73f/%2B3Ueu1rbb5n6lXr17mtttu82mTZJYuXdrY5TWJiIgI8/LLL5uDBw%2BakJAQs3jxYm/fnj17TKtWrcyKFSuMMcYsX77ctGrVyuzZs8c75vXXXzdOp9NUVFQ0ee0/16m51%2BaNN94woaGhprq62tsWTOfdGN/5t6Tr3pizn/tgvearqqpMYmKiyc3N9TnfLeHar2vutfHXtc8twiBzahn0zD9suXDhQkVFRalbt26aMWOGqqqqvH3r1q2Ty%2BXSgAEDvG0DBw6Uy%2BVSXl5e0xRuSV3zPyU/P1%2BFhYWaMGFCjb7MzExFRUWpf//%2B%2Bsc//qGTJ082aq22nThxQosXL9bhw4eVnJys/Px8VVdXKy0tzTsmLi5OSUlJ3vO6bt06JSUlKS4uzjtmyJAh8ng8ys/Pb/I5/FRnzr02FRUVCg8Pr/Fhg839vEt1z78lXPf1nftgvuYnTpyooUOH6pprrvFpbwnXfl1zr42/rv2W%2BbGuQcoYo2nTpumKK65QUlKSt33MmDFKSEiQ2%2B1WUVGRsrKy9Pnnn3v/vmJZWZmio6NrHC86OrrGH7kOZHXN/3Rz587VpZdeWuPvSD7yyCMaPHiw2rVrp9WrV2v69Onat2%2BfHnjggaYo/WfZsmWLkpOTdfToUZ133nlaunSpLrvsMhUWFio0NFQRERE%2B42NiYrzntaysrMYfLY%2BIiFBoaGizOPd1zf1M%2B/fv1yOPPKI777zTp705n3fp7PMP9uu%2Boec%2BGK95SVq8eLHy8/O1cePGGn1lZWVBfe2fbe5n8ue1T8AKIpmZmdq8ebM%2B%2BeQTn/aMjAzv10lJSUpMTFS/fv1UUFCgPn36SPrxgb8zGWNqbQ9Udc3/lCNHjmjRokV68MEHa/SdflH16tVLkvTwww83i//YXnLJJSosLNTBgwf13//93xo3bpzWrFlT5/gzz2tzPvd1zf30f2grKys1dOhQXXbZZfrTn/7k8/rmfN6ls88/2K/7hpz7YL3mS0pKdM8992jlypVq27Ztg18XDNf%2Buczd79d%2Bo96ARJPJzMw0nTt3Nl9//XW9Y0%2BePOlzf37u3LnG5XLVGOdyucy8efOs19oYGjL/V1991YSEhJjy8vJ6j/fJJ58YSaasrMxmmU1i8ODB5o477jCrV682ksz333/v09%2BjRw/z0EMPGWOMefDBB02PHj18%2Br///nsjyXzwwQdNVrMtp%2BZ%2BSmVlpUlOTjaDBw82R44cqff1zfm8G1Nz/qcLxuv%2BdLXNPViv%2BaVLlxpJpnXr1t5NknE4HKZ169Zm1apVQXvt1zf3Uw/xB8K1zzNYzZwxRpmZmXrrrbf0wQcfKCEhod7XbN26VdXV1YqNjZUkJScnq6KiQp999pl3zKeffqqKiooay%2BqB5lzmP3fuXI0YMUKdOnWq97ibNm1S27Ztdf7559sst0kYY%2BTxeNS3b1%2BFhIR4bwlJUmlpqYqKirznNTk5WUVFRSotLfWOWblypZxOp/r27dvktf9cp%2BYu/fh/r2lpaQoNDdWyZcsa9H/6zfm8S77zP1MwXfe1qW3uwXrNDx48WFu2bFFhYaF369evn8aMGeP9Oliv/frm3rp168C59q1FNfjF73//e%2BNyucxHH31U69tNd%2BzYYbKzs82GDRtMcXGxeffdd81//Md/mN69e3uTvjE/vl27R48eZt26dWbdunWme/fuzeLt2vXN/5Tt27cbh8Nh3nvvvRrHWLZsmXnxxRfNli1bzI4dO8xLL71kwsPDzeTJk5tqGj9ZVlaWWbt2rSkuLjabN282999/v2nVqpVZuXKlMebHt2p37tzZrFq1yhQUFJirr7661rdqDx482BQUFJhVq1aZzp07N4u3ap9t7pWVlWbAgAGme/fuZseOHT6/G6fm3pzPuzFnn3%2BwX/f1/d4bE7zXfF3OfCddMF/7Zzp97oF07ROwmjlJtW7z5883xhiza9cuc%2BWVV5rIyEgTGhpqLrroIjN58mSzf/9%2Bn%2BPs37/fjBkzxoSFhZmwsDAzZswYc%2BDAAT/M6NzUN/9TsrKyTOfOnc2JEydqHOO9994zvXr1Muedd55p3769SUpKMs8884zPW3oD1W233Wa6du1qQkNDTadOnczgwYN9/pE5cuSIyczMNJGRkaZdu3Zm2LBhZteuXT7H2Llzpxk6dKhp166diYyMNJmZmebo0aNNPZVzdra5f/jhh3X%2BbhQXFxtjmvd5N%2Bbs8w/2676%2B33tjgvear8uZASuYr/0znT73QLr2HcYYY289DAAAADyDBQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAsv8HnjS675I27hMAAAAASUVORK5CYII%3D"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common7550354334602978036">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">318.5</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">294.0</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">343.0</td>
        <td class="number">128</td>
        <td class="number">16.7%</td>
        <td>
            <div class="bar" style="width:67%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">367.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">245.0</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">269.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">416.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme7550354334602978036">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">245.0</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">269.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">294.0</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">318.5</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">343.0</td>
        <td class="number">128</td>
        <td class="number">16.7%</td>
        <td>
            <div class="bar" style="width:67%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">294.0</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">318.5</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">343.0</td>
        <td class="number">128</td>
        <td class="number">16.7%</td>
        <td>
            <div class="bar" style="width:67%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">367.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">416.5</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_X4">X4<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>4</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>0.5%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (n)</th>
                    <td>0</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>

        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>176.6</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>110.25</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>220.5</td>
                </tr>
                <tr class="ignore">
                    <th>Zeros (%)</th>
                    <td>0.0%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram4291415193915693963">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAAbpJREFUeJzt3EHOk1AUhuFD4xQW8Ad24SLck2P35CLcBaQLoHHgwOJA68Tfz7TRizTPMyxpDqW8IUByu23btmpsWZaapqn1WA5unucax7HpzDdNp/3Q931Vff/BwzDssQscyLquNU3Tz/OmpV0C6bquqqqGYRDIE3v7/uPd3/n04d1vt93Om5ZOzSfCgQgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAS7rIt1VPeu85TWeOIYXEEgEAgEAoFAIBAIBAKBQPAUj3n/9jL7cOMKAoFAIBAIBAKBQCAQCAQCgUDw370HeeSdBvwrriAQCAQCgUAgEAgEAoFAINjlMe%2B2bVVVta7rL9u%2BfvncZB9em/0n9%2B7bIzOeySP/5WvH7PbZ7bxpqdt2mLosS03T1HosBzfPc43j2HTmLoFcr9c6n8/V9311Xdd6PAezbVtdLpd6eXmp06ntXcEugcBRuEmHQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIPgGu5xWh7X32gkAAAAASUVORK5CYII%3D">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives4291415193915693963,#minihistogram4291415193915693963"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives4291415193915693963">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles4291415193915693963"
                                                  aria-controls="quantiles4291415193915693963" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram4291415193915693963" aria-controls="histogram4291415193915693963"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common4291415193915693963" aria-controls="common4291415193915693963"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme4291415193915693963" aria-controls="extreme4291415193915693963"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>

    </ul>

    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles4291415193915693963">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>110.25</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>110.25</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>140.88</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>183.75</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>220.5</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>220.5</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>220.5</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>110.25</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>79.625</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>45.166</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.25575</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>-1.7769</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>176.6</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>43.896</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>-0.16276</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>135630</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>2040</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>6.1 KiB</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram4291415193915693963">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAIABJREFUeJzt3X9UVXW%2B//HXkR9HJTiJCAeCvNzJHBX15o%2BroFP%2BCjTRyuZmF2O069APBceEmsjbRDOTdMuyH9685Z2yKRtqVmq2NBLL/LEATZQU85qVo6ggZnhA0wPq/v7Rcn87Akq25Rzg%2BVhrr8X%2BfD57n/f%2B2MBrPmeffWyGYRgCAACAZTp4uwAAAIC2hoAFAABgMQIWAACAxQhYAAAAFiNgAQAAWIyABQAAYDECFgAAgMUIWAAAABYjYAEAAFiMgAUAAGAxAhYAAIDFCFgAAAAWI2ABAABYjIAFAABgMQIWAACAxQhYAAAAFiNgAQAAWIyABQAAYDECFgAAgMUIWAAAABYjYAEAAFiMgAUAAGAxAhYAAIDFCFgAAAAWI2ABAABYjIAFAABgMQIWAACAxQhYAAAAFiNgAQAAWIyABQAAYDECFgAAgMUIWAAAABYjYAEAAFiMgAUAAGAxAhYAAIDFCFgAAAAWI2ABAABYrF0FrNzcXNlsNs2ePdtsc7vdysjIUFhYmIKCgjRx4kQdPHjQ47gDBw5owoQJCgoKUlhYmGbNmqW6urqWLh8AALQS/t4uoKV89tlnevXVV9WvXz%2BP9tmzZ%2BuDDz5QXl6eunbtqszMTCUnJ6ukpER%2Bfn46e/asxo8fr27dumnTpk06duyYpk6dKsMw9NJLLzXrtc%2BdO6fDhw8rODhYNpvtSlweAAC4gGEYqq2tVVRUlDp0aOE1JaMdqK2tNXr06GEUFBQYN910k/G73/3OMAzDOH78uBEQEGDk5eWZYw8dOmR06NDByM/PNwzDMFavXm106NDBOHTokDnmb3/7m2G32w2Xy9Ws1y8vLzcksbGxsbGxsXlhKy8vtzBVNE%2B7WMGaOXOmxo8frzFjxujPf/6z2V5SUqL6%2BnolJiaabVFRUYqLi1NhYaGSkpJUVFSkuLg4RUVFmWOSkpLkdrtVUlKikSNHXvL1g4ODJUnl5eUKCQmx8MoAAEBTampqFBMTY/4dbkltPmDl5eWppKREW7dubdBXWVmpwMBAdenSxaM9IiJClZWV5piIiAiP/i5duigwMNAccyG32y23223u19bWSpJCQkIIWAAAtDBv3J7Tpm9yLy8v1%2B9%2B9zstXbpUHTt2bPZxhmF4/GM09g9z4Zgfy83NlcPhMLeYmJifXjwAAGi12nTAKikpUVVVlQYOHCh/f3/5%2B/tr/fr1evHFF%2BXv76%2BIiAjV1dWpurra47iqqipz1crpdDZYqaqurlZ9fX2Dla3zsrOz5XK5zK28vPzKXCAAAPBJbTpgjR49Wjt37lRpaam5DRo0SFOmTDF/DggIUEFBgXlMRUWFysrKlJCQIEmKj49XWVmZKioqzDFr1qyR3W7XwIEDG31du91uvh3I24IAALQ/bfoerODgYMXFxXm0BQUFqWvXrmb79OnTlZmZqa5duyo0NFRZWVnq27evxowZI0lKTExU7969lZqaqmeeeUbfffedsrKylJaWRnACAACNatMBqzkWLFggf39/3XnnnTp16pRGjx6tJUuWyM/PT5Lk5%2BenVatWacaMGRo2bJg6deqklJQUzZ8/38uVAwAAX2UzDMPwdhFtXU1NjRwOh1wuF6teAAC0EG/%2B/W3T92ABAAB4AwELAADAYgQsAAAAixGwAAAALEbAAgAAsBgBCwAAwGLt/jlYAAC0VYPm5nu7hGbb%2BuRYb5dgKVawAAAALEbAAgAAsBgBCwAAwGIELAAAAIsRsAAAACxGwAIAALAYAQsAAMBiBCwAAACLEbAAAAAsRsACAACwGAELAADAYgQsAAAAixGwAAAALEbAAgAAsBgBCwAAwGIELAAAAIsRsAAAACxGwAIAALAYAQsAAMBiBCwAAACLEbAAAAAsRsACAACwWJsOWIsWLVK/fv0UEhKikJAQxcfH68MPPzT7R4wYIZvN5rHdddddHueorq5WamqqHA6HHA6HUlNTdfz48Za%2BFAAA0Iq06YAVHR2tp556Slu3btXWrVs1atQo3Xrrrdq1a5c5Ji0tTRUVFeb2yiuveJwjJSVFpaWlys/PV35%2BvkpLS5WamtrSlwIAAFoRf28XcCVNmDDBY//JJ5/UokWLVFxcrD59%2BkiSOnfuLKfT2ejxu3fvVn5%2BvoqLizVkyBBJ0uLFixUfH689e/aoZ8%2BeV/YCAABAq9SmV7B%2B7OzZs8rLy9PJkycVHx9vti9dulRhYWHq06ePsrKyVFtba/YVFRXJ4XCY4UqShg4dKofDocLCwiZfy%2B12q6amxmMDAADtR5tewZKknTt3Kj4%2BXqdPn9ZVV12l5cuXq3fv3pKkKVOmKDY2Vk6nU2VlZcrOztbnn3%2BugoICSVJlZaXCw8MbnDM8PFyVlZVNvmZubq6eeOKJK3NBAADA57X5gNWzZ0%2BVlpbq%2BPHjeu%2B99zR16lStX79evXv3VlpamjkuLi5OPXr00KBBg7Rt2zYNGDBAkmSz2Rqc0zCMRtvPy87O1pw5c8z9mpoaxcTEWHhVAADAl7X5gBUYGKjrrrtOkjRo0CB99tlneuGFFxrczC5JAwYMUEBAgPbu3asBAwbI6XTqyJEjDcYdPXpUERERTb6m3W6X3W637iIAAECr0m7uwTrPMAy53e5G%2B3bt2qX6%2BnpFRkZKkuLj4%2BVyubRlyxZzzObNm%2BVyuZSQkNAi9QIAgNanTa9gPfrooxo3bpxiYmJUW1urvLw8ffrpp8rPz9fXX3%2BtpUuX6pZbblFYWJi%2B%2BOILZWZm6oYbbtCwYcMkSb169dLYsWOVlpZmrnjde%2B%2B9Sk5O5hOEAACgSW06YB05ckSpqamqqKiQw%2BFQv379lJ%2Bfr5tvvlnl5eX6%2BOOP9cILL%2BjEiROKiYnR%2BPHj9fjjj8vPz888x9KlSzVr1iwlJiZKkiZOnKiFCxd665IAAEArYDMMw/B2EW1dTU2NHA6HXC6XQkJCvF0OAKCdGDQ339slNNvWJ8dafk5v/v1td/dgAQAAXGkELAAAAIsRsAAAACxGwAIAALAYAQsAAMBiBCwAAACLEbAAAAAsRsACAACwGAELAADAYgQsAAAAixGwAAAALEbAAgAAsBgBCwAAwGIELAAAAIsRsAAAACxGwAIAALAYAQsAAMBiBCwAAACLEbAAAAAsRsACAACwGAELAADAYgQsAAAAixGwAAAALEbAAgAAsBgBCwAAwGIELAAAAIsRsAAAACxGwAIAALBYmw5YixYtUr9%2B/RQSEqKQkBDFx8frww8/NPvdbrcyMjIUFhamoKAgTZw4UQcPHvQ4x4EDBzRhwgQFBQUpLCxMs2bNUl1dXUtfCgAAaEXadMCKjo7WU089pa1bt2rr1q0aNWqUbr31Vu3atUuSNHv2bC1fvlx5eXnatGmTTpw4oeTkZJ09e1aSdPbsWY0fP14nT57Upk2blJeXp/fee0%2BZmZnevCwAAODjbIZhGN4uoiWFhobqmWee0a9//Wt169ZNb775piZPnixJOnz4sGJiYrR69WolJSXpww8/VHJyssrLyxUVFSVJysvL07Rp01RVVaWQkJBmvWZNTY0cDodcLlezjwEA4OcaNDff2yU029Ynx1p%2BTm/%2B/W3TK1g/dvbsWeXl5enkyZOKj49XSUmJ6uvrlZiYaI6JiopSXFycCgsLJUlFRUWKi4szw5UkJSUlye12q6SkpMWvAQAAtA7%2B3i7gStu5c6fi4%2BN1%2BvRpXXXVVVq%2BfLl69%2B6t0tJSBQYGqkuXLh7jIyIiVFlZKUmqrKxURESER3%2BXLl0UGBhojmmM2%2B2W2%2B0292tqaiy8IgAA4Ova/ApWz549VVpaquLiYj3wwAOaOnWqvvjiiybHG4Yhm81m7v/456bGXCg3N1cOh8PcYmJift5FAACAVqXNB6zAwEBdd911GjRokHJzc9W/f3%2B98MILcjqdqqurU3V1tcf4qqoqc9XK6XQ2WKmqrq5WfX19g5WtH8vOzpbL5TK38vJy6y8MAAD4rDYfsC5kGIbcbrcGDhyogIAAFRQUmH0VFRUqKytTQkKCJCk%2BPl5lZWWqqKgwx6xZs0Z2u10DBw5s8jXsdrv5aIjzGwAAaD/a9D1Yjz76qMaNG6eYmBjV1tYqLy9Pn376qfLz8%2BVwODR9%2BnRlZmaqa9euCg0NVVZWlvr27asxY8ZIkhITE9W7d2%2BlpqbqmWee0XfffaesrCylpaURmgAAQJPadMA6cuSIUlNTVVFRIYfDoX79%2Bik/P18333yzJGnBggXy9/fXnXfeqVOnTmn06NFasmSJ/Pz8JEl%2Bfn5atWqVZsyYoWHDhqlTp05KSUnR/PnzvXlZAADAx7W752B5A8/BAgB4A8/B4jlYAAAAbQYBCwAAwGIELAAAAIsRsAAAACxGwAIAALAYAQsAAMBiBCwAAACLEbAAAAAsRsACAACwGAELAADAYgQsAAAAixGwAAAALEbAAgAAsBgBCwAAwGIELAAAAIsRsAAAACxGwAIAALAYAQsAAMBiBCwAAACLEbAAAAAsRsACAACwGAELAADAYgQsAAAAixGwAAAALEbAAgAAsBgBCwAAwGIELAAAAIsRsAAAACxGwAIAALBYmw5Yubm5Gjx4sIKDgxUeHq7bbrtNe/bs8RgzYsQI2Ww2j%2B2uu%2B7yGFNdXa3U1FQ5HA45HA6lpqbq%2BPHjLXkpAACgFWnTAWv9%2BvWaOXOmiouLVVBQoDNnzigxMVEnT570GJeWlqaKigpze%2BWVVzz6U1JSVFpaqvz8fOXn56u0tFSpqakteSkAAKAV8fd2AVdSfn6%2Bx/7rr7%2Bu8PBwlZSU6MYbbzTbO3fuLKfT2eg5du/erfz8fBUXF2vIkCGSpMWLFys%2BPl579uxRz549r9wFAACAVqlNr2BdyOVySZJCQ0M92pcuXaqwsDD16dNHWVlZqq2tNfuKiorkcDjMcCVJQ4cOlcPhUGFhYcsUDgAAWpU2vYL1Y4ZhaM6cORo%2BfLji4uLM9ilTpig2NlZOp1NlZWXKzs7W559/roKCAklSZWWlwsPDG5wvPDxclZWVjb6W2%2B2W2%2B0292tqaiy%2BGgAA4MvaTcBKT0/Xjh07tGnTJo/2tLQ08%2Be4uDj16NFDgwYN0rZt2zRgwABJks1ma3A%2BwzAabZd%2BuLn%2BiSeesLB6AADQmrSLtwgzMjK0cuVKrVu3TtHR0RcdO2DAAAUEBGjv3r2SJKfTqSNHjjQYd/ToUUVERDR6juzsbLlcLnMrLy//%2BRcBAABajTYdsAzDUHp6upYtW6ZPPvlEsbGxlzxm165dqq%2BvV2RkpCQpPj5eLpdLW7ZsMcds3rxZLpdLCQkJjZ7DbrcrJCTEYwMAAO1Hm36LcObMmXr77bf1/vvvKzg42LxnyuFwqFOnTvr666%2B1dOlS3XLLLQoLC9MXX3yhzMxM3XDDDRo2bJgkqVevXho7dqzS0tLMxzfce%2B%2B9Sk5O5hOEAACgUT67gvXWW2/p9OnTP%2BscixYtksvl0ogRIxQZGWlu77zzjiQpMDBQH3/8sZKSktSzZ0/NmjVLiYmJWrt2rfz8/MzzLF26VH379lViYqISExPVr18/vfnmmz%2BrNgAA0HbZDMMwvF1EY8LDw1VXV6fJkydr%2BvTp%2Btd//Vdvl3TZampq5HA45HK5eLsQANBiBs3Nv/QgH7H1ybGWn9Obf399dgXr8OHDeu2111RRUaHhw4erT58%2BevbZZ3X06FFvlwYAAHBRPhuw/P39NWnSJK1cuVIHDhzQ1KlT9dprryk6OlqTJk3SqlWr5KOLbwAAoJ3z2YD1Y06nU6NHjza/mHnr1q1KSUlRjx49tHHjRm%2BXBwAA4MGnA9a3336r559/Xv3799ewYcNUVVWlFStWaP/%2B/Tp06JCSk5P1m9/8xttlAgAAePDZxzTcfvvtWr16tWJjY/Xb3/5WU6dOVbdu3cz%2Bq666Sg8//LBefPFFL1YJAADQkM8GrJCQEK1du1a/%2BtWvmhwTGRlpPnEdAADAV/hswHrjjTcuOcZms%2BkXv/hFC1QDAADQfD57D9aDDz6ohQsXNmj/7//%2Bb2VmZnqhIgAAgObx2YD197//XUOHDm3QHh8fbz6JHQAAwBf5bMD69ttv1aVLlwbtISEh%2Bvbbb71QEQAAQPP4bMD6xS9%2BoY8%2B%2BqhB%2B0cffaTY2FgvVAQAANA8PnuT%2B%2BzZszV79mwdO3ZMo0aNkiR9/PHHevrppzV//nwvVwcAANA0nw1YaWlpOn36tObNm6fHH39ckhQdHa0XX3xR//Ef/%2BHl6gAAAJrmswFLkjIyMpSRkaGKigp16tRJV199tbdLAgAAuCSfDljnRUZGersEAACAZvPZm9yPHj2qe%2B65R9dee606duyowMBAjw0AAMBX%2BewK1rRp0/T111/roYceUmRkpGw2m7dLAgAAaBafDVgbNmzQhg0bdMMNN3i7FAAAgJ/EZ98ijI6OZtUKAAC0Sj4bsBYsWKDs7GwdPHjQ26UAAAD8JD77FmFqaqpqa2vVvXt3hYSEKCAgwKO/qqrKS5UBAABcnM8GrKeeesrbJQAAAFwWnw1Y06dP93YJAAAAl8Vn78GSpH/84x/KyclRamqq%2BZbgmjVrtHv3bi9XBgAA0DSfDVgbN25Unz59tH79er377rs6ceKEJGnbtm36wx/%2B4OXqAAAAmuazAev3v/%2B9cnJytG7dOo8nt48aNUrFxcVerAwAAODifDZg7dixQ7/%2B9a8btIeHh%2Bvo0aNeqAgAAKB5fDZgXX311aqsrGzQXlpaqmuuucYLFQEAADSPzwasu%2B66S4888oiOHj1qPtF98%2BbNysrK0t133%2B3l6gAAAJrmswFr3rx5cjqdioyM1IkTJ9S7d28lJCRo8ODBeuyxx5p1jtzcXA0ePFjBwcEKDw/Xbbfdpj179niMcbvdysjIUFhYmIKCgjRx4sQGT48/cOCAJkyYoKCgIIWFhWnWrFmqq6uz7FoBAEDb4rPPwQoMDNQ777yjL7/8Utu2bdO5c%2Bc0YMAA/fKXv2z2OdavX6%2BZM2dq8ODBOnPmjObOnavExER98cUXCgoKkiTNnj1bH3zwgfLy8tS1a1dlZmYqOTlZJSUl8vPz09mzZzV%2B/Hh169ZNmzZt0rFjxzR16lQZhqGXXnrpSl0%2BAABoxWyGYRjeLqKlHD16VOHh4Vq/fr1uvPFGuVwudevWTW%2B%2B%2BaYmT54sSTp8%2BLBiYmK0evVqJSUl6cMPP1RycrLKy8sVFRUlScrLy9O0adNUVVWlkJCQS75uTU2NHA6HXC5Xs8YDAGCFQXPzvV1Cs219cqzl5/Tm31%2BfXcG69957L9r/6quv/uRzulwuSVJoaKgkqaSkRPX19UpMTDTHREVFKS4uToWFhUpKSlJRUZHi4uLMcCVJSUlJcrvdKikp0ciRIxu8jtvtltvtNvdramp%2Bcq0AAKD18tmAVVFR4bFfX1%2BvXbt2qba2VjfeeONPPp9hGJozZ46GDx%2BuuLg4SVJlZaUCAwPVpUsXj7ERERHmJxgrKysVERHh0d%2BlSxcFBgY2%2BilH6Yd7v5544omfXCMAAGgbfDZgffDBBw3azpw5owceeEC9evX6yedLT0/Xjh07tGnTpkuONQzD/OSiJI%2BfmxrzY9nZ2ZozZ465X1NTo5iYmJ9cMwAAaJ189lOEjfH391dWVpaeeeaZn3RcRkaGVq5cqXXr1ik6OtpsdzqdqqurU3V1tcf4qqoqc9XK6XQ2WKmqrq5WfX19g5Wt8%2Bx2u0JCQjw2AADQfrSqgCVJ33zzjerr65s11jAMpaena9myZfrkk08UGxvr0T9w4EAFBASooKDAbKuoqFBZWZkSEhIkSfHx8SorK/N4y3LNmjWy2%2B0aOHCgBVcEAADaGp99i/Dhhx/22DcMQxUVFVq5cqWmTJnSrHPMnDlTb7/9tt5//30FBwebK1EOh0OdOnWSw%2BHQ9OnTlZmZqa5duyo0NFRZWVnq27evxowZI0lKTExU7969lZqaqmeeeUbfffedsrKylJaWxsoUAABolM8GrKKiIo/9Dh06qFu3bnrqqaeUlpbWrHMsWrRIkjRixAiP9tdff13Tpk2TJC1YsED%2B/v668847derUKY0ePVpLliyRn5%2BfJMnPz0%2BrVq3SjBkzNGzYMHXq1EkpKSmaP3/%2Bz7tAAADQZrWr52B5C8/BAgB4A8/B4jlYAH6m9v6LFAB8ic8GrMGDBzf5GIQLbdmy5QpXAwAA0Hw%2BG7BGjhypV155Rddff73i4%2BMlScXFxdqzZ4/uu%2B8%2B2e12L1cIAADQOJ8NWMePH9fMmTM1b948j/a5c%2BfqyJEj%2Bt///V8vVQYAAHBxPvscrHfffVf33HNPg/Zp06bp73//uxcqAgAAaB6fDVh2u12FhYUN2gsLC3l7EAAA%2BDSffYtw1qxZuv/%2B%2B7V9%2B3YNHTpU0g/3YC1evFiPPvqol6sDAABoms8GrLlz5yo2NlYvvPCCXnvtNUlSr169tHjxYqWkpHi5OgAAgKb5bMCSpJSUFMIUAABodXz2HizphyewLlmyRH/4wx9UXV0tSfr88889vngZAADA1/jsClZZWZnGjBmjzp07q7y8XNOmTVOXLl307rvv6uDBg3rjjTe8XSIAAECjfHYF68EHH1RKSoq%2B/vprdezY0WwfP368NmzY4MXKAAAALs5nV7A%2B%2B%2BwzLVq0qMHX5VxzzTW8RQgAAHyaz65gBQYG6sSJEw3a9%2B7dq7CwMC9UBAAA0Dw%2BG7AmTpyoP/3pTzpz5owkyWaz6dChQ3rkkUc0adIkL1cHAADQNJ8NWM8%2B%2B6wOHz4sp9OpU6dOadSoUfrnf/5ndezYscH3EwIAAPgSn70Hy%2BFwqLCwUAUFBdq2bZvOnTunAQMGKCkpqcF9WQAAAL7EJwNWfX29brnlFr388stKTExUYmKit0sCAABoNp98izAgIEDbt29npQoAALRKPhmwJOnuu%2B/W66%2B/7u0yAAAAfjKffIvwvIULF2rt2rUaNGiQgoKCPPqefvppL1UFAABwcT4bsEpKStSvXz9J0o4dOzz6eOsQAAD4Mp8LWN98841iY2O1ceNGb5cCAABwWXzuHqwePXro6NGj5v7kyZN15MgRL1YEAADw0/hcwDIMw2N/9erVOnnypJeqAQAA%2BOl8LmABAAC0dj4XsGw2W4Ob2LmpHQAAtCY%2Bd5O7YRiaNm2a7Ha7JOn06dO6//77GzymYdmyZd4oDwAA4JJ8bgVr6tSpCg8Pl8PhkMPh0N13362oqChz//zWHBs2bNCECRMUFRUlm82mFStWePRPmzbNXDE7vw0dOtRjjNvtVkZGhsLCwhQUFKSJEyfq4MGDll0vAABoe3xuBcvKp7efPHlS/fv31z333KM77rij0TFjx471eM3AwECP/tmzZ%2BuDDz5QXl6eunbtqszMTCUnJ6ukpER%2Bfn6W1QoAANoOnwtYVho3bpzGjRt30TF2u11Op7PRPpfLpb/85S968803NWbMGEnSW2%2B9pZiYGK1du1ZJSUmW1wwAAFo/n3uLsKV9%2BumnCg8P1/XXX6%2B0tDRVVVWZfSUlJaqvr1diYqLZFhUVpbi4OBUWFnqjXAAA0Aq06RWsSxk3bpz%2B7d/%2BTd27d9e%2Bffv02GOPadSoUSopKZHdbldlZaUCAwPVpUsXj%2BMiIiJUWVnZ5Hndbrfcbre5X1NTc8WuAQAA%2BJ52HbAmT55s/hwXF6dBgwape/fuWrVqlSZNmtTkcYZhXPTREbm5uXriiScsrRUAALQe7f4twh%2BLjIxU9%2B7dtXfvXkmS0%2BlUXV2dqqurPcZVVVUpIiKiyfNkZ2fL5XKZW3l5%2BRWtGwAA%2BBYC1o8cO3ZM5eXlioyMlCQNHDhQAQEBKigoMMdUVFSorKxMCQkJTZ7HbrcrJCTEYwMAAO1Hm36L8MSJE/rqq6/M/X379qm0tFShoaEKDQ1VTk6O7rjjDkVGRuof//iHHn30UYWFhen222%2BXJDkcDk2fPl2ZmZnq2rWrQkNDlZWVpb59%2B5qfKgQAALhQmw5YW7du1ciRI839OXPmSPrhYaaLFi3Szp079de//lXHjx9XZGSkRo4cqXfeeUfBwcHmMQsWLJC/v7/uvPNOnTp1SqNHj9aSJUt4BhYAAGhSmw5YI0aMkGEYTfZ/9NFHlzxHx44d9dJLL%2Bmll16ysjQAANCGcQ8WAACAxQhYAAAAFiNgAQAAWIyABQAAYDECFgAAgMUIWAAAABYjYAEAAFiMgAUAAGAxAhYAAIDFCFgAAAAWI2ABAABYrE1/FyF8y6C5%2Bd4u4SfZ%2BuRYb5cAAGilWMECAACwGAELAADAYgQsAAAAixGwAAAALEbAAgAAsBgBCwAAwGIELAAAAIsRsAAAACxGwAIAALAYAQsAAMBiBCwAAACLEbAAAAAsRsACAACwGAELAADAYgQsAAAAixGwAAAALEbAAgAAsFibDlgbNmzQhAkTFBUVJZvNphUrVnj0G4ahnJwcRUVFqVOnThoxYoR27drlMaa6ulqpqalyOBxyOBxKTU3V8ePHW/IyAABAK9OmA9bJkyfVv39/LVy4sNH%2Bp59%2BWs8995wWLlyozz77TE6nUzfffLNqa2vNMSkpKSotLVV%2Bfr7y8/NVWlqq1NTUlroEAADQCvl7u4Arady4cRo3blyjfYZh6Pnnn9fcuXM1adIkSdIbb7yhiIgIvf3227rvvvu0e/du5efnq7i4WEOGDJEkLV68WPHx8dqzZ4969uzZYtcCAABajza9gnUx%2B/btU2VlpRITE802u92um266SYWFhZKkoqIiORwOM1xJ0tChQ%2BVwOMwxAAAAF2rTK1gXU1lZKUmKiIjwaI%2BIiND%2B/fvNMeHh4Q2ODQ8PN49vjNvtltvtNvdramqsKBkAALQS7XYF6zybzeaxbxiGR9uF/Y2NuVBubq55U7zD4VBMTIx1BQMAAJ/XbgOW0%2BmUpAYrUVVVVeaqltPp1JEjRxoce/To0QYrXz%2BWnZ0tl8tlbuXl5RZWDgAAfF27DVixsbHTKvEQAAAP6klEQVRyOp0qKCgw2%2Brq6rR%2B/XolJCRIkuLj4%2BVyubRlyxZzzObNm%2BVyucwxjbHb7QoJCfHYAABA%2B9Gm78E6ceKEvvrqK3N/3759Ki0tVWhoqK699lrNnj1b8%2BbNU48ePdSjRw/NmzdPnTt3VkpKiiSpV69eGjt2rNLS0vTKK69Iku69914lJyfzCUIAANCkNh2wtm7dqpEjR5r7c%2BbMkSRNnTpVS5Ys0cMPP6xTp05pxowZqq6u1pAhQ7RmzRoFBwebxyxdulSzZs0yP204ceLEJp%2BrBQAAILXxgDVixAgZhtFkv81mU05OjnJycpocExoaqrfeeusKVAcAANqqdnsPFgAAwJVCwAIAALAYAQsAAMBiBCwAAACLEbAAAAAsRsACAACwGAELAADAYgQsAAAAixGwAAAALEbAAgAAsBgBCwAAwGIELAAAAIsRsAAAACxGwAIAALAYAQsAAMBiBCwAAACLEbAAAAAsRsACAACwGAELAADAYgQsAAAAixGwAAAALEbAAgAAsJi/twvAzzNobr63SwAAABdgBQsAAMBiBCwAAACLEbAAAAAsRsACAACwGAELAADAYgQsAAAAi7X7gJWTkyObzeaxOZ1Os98wDOXk5CgqKkqdOnXSiBEjtGvXLi9WDAAAfF27D1iS1KdPH1VUVJjbzp07zb6nn35azz33nBYuXKjPPvtMTqdTN998s2pra71YMQAA8GUELEn%2B/v5yOp3m1q1bN0k/rF49//zzmjt3riZNmqS4uDi98cYb%2Bv777/X22297uWoAAOCrCFiS9u7dq6ioKMXGxuquu%2B7SN998I0nat2%2BfKisrlZiYaI612%2B266aabVFhY2OT53G63ampqPDYAANB%2BtPuANWTIEP31r3/VRx99pMWLF6uyslIJCQk6duyYKisrJUkREREex0RERJh9jcnNzZXD4TC3mJiYK3oNAADAt7T7gDVu3Djdcccd6tu3r8aMGaNVq1ZJkt544w1zjM1m8zjGMIwGbT%2BWnZ0tl8tlbuXl5VemeAAA4JPafcC6UFBQkPr27au9e/eanya8cLWqqqqqwarWj9ntdoWEhHhsAACg/SBgXcDtdmv37t2KjIxUbGysnE6nCgoKzP66ujqtX79eCQkJXqwSAAD4Mn9vF%2BBtWVlZmjBhgq699lpVVVXpz3/%2Bs2pqajR16lTZbDbNnj1b8%2BbNU48ePdSjRw/NmzdPnTt3VkpKirdLBwAAPqrdB6yDBw/q3//93/Xtt9%2BqW7duGjp0qIqLi9W9e3dJ0sMPP6xTp05pxowZqq6u1pAhQ7RmzRoFBwd7uXIAAOCr2n3AysvLu2i/zWZTTk6OcnJyWqYgAADQ6nEPFgAAgMUIWAAAABYjYAEAAFiMgAUAAGAxAhYAAIDFCFgAAAAWI2ABAABYjIAFAABgMQIWAACAxQhYAAAAFiNgAQAAWIyABQAAYDECFgAAgMUIWAAAABYjYAEAAFiMgAUAAGAxAhYAAIDFCFgAAAAWI2ABAABYjIAFAABgMQIWAACAxQhYAAAAFiNgAQAAWIyABQAAYDECFgAAgMUIWAAAABYjYAEAAFiMgAUAAGAxAhYAAIDFCFjN9PLLLys2NlYdO3bUwIEDtXHjRm%2BXBAAAfBQBqxneeecdzZ49W3PnztX27dv1q1/9SuPGjdOBAwe8XRoAAPBBBKxmeO655zR9%2BnT99re/Va9evfT8888rJiZGixYt8nZpAADAB/l7uwBfV1dXp5KSEj3yyCMe7YmJiSosLGz0GLfbLbfbbe67XC5JUk1NjeX1nXWftPyc%2BMGV%2BPe6klrTfwutbW6B1qq9/144f07DMCw/96UQsC7h22%2B/1dmzZxUREeHRHhERocrKykaPyc3N1RNPPNGgPSYm5orUiCvD8ay3K2i7mFsAF7qSvxdqa2vlcDiu3As0goDVTDabzWPfMIwGbedlZ2drzpw55v65c%2Bf03XffqWvXrk0e05bU1NQoJiZG5eXlCgkJ8XY5rQ7zd/mYu8vH3F0%2B5u7yXem5MwxDtbW1ioqKsvzcl0LAuoSwsDD5%2Bfk1WK2qqqpqsKp1nt1ul91u92i7%2Buqrr1iNviokJIRfNj8D83f5mLvLx9xdPubu8l3JuWvplavzuMn9EgIDAzVw4EAVFBR4tBcUFCghIcFLVQEAAF/GClYzzJkzR6mpqRo0aJDi4%2BP16quv6sCBA7r//vu9XRoAAPBBfjk5OTneLsLXxcXFqWvXrpo3b57mz5%2BvU6dO6c0331T//v29XZrP8vPz04gRI%2BTvT4a/HMzf5WPuLh9zd/mYu8vXVufOZnjjs4sAAABtGPdgAQAAWIyABQAAYDECFgAAgMUIWAAAABYjYKHZNmzYoAkTJigqKko2m00rVqww%2B%2Brr6/X73/9effv2VVBQkKKiovSb3/xGhw8f9jhHdXW1UlNT5XA45HA4lJqaquPHj7f0pbS4i83dhe677z7ZbDY9//zzHu3MXdNzt3v3bk2cOFEOh0PBwcEaOnSoDhw4YPa73W5lZGQoLCxMQUFBmjhxog4ePNiSl%2BEVl5q7EydOKD09XdHR0erUqZN69erV4Evs2%2Bvc5ebmavDgwQoODlZ4eLhuu%2B027dmzx2NMc%2BbmwIEDmjBhgoKCghQWFqZZs2aprq6uJS%2BlxV1q7r777jtlZGSoZ8%2Be6ty5s6699lrNmjXL/N7e81r73BGw0GwnT55U//79tXDhwgZ933//vbZt26bHHntM27Zt07Jly/Tll19q4sSJHuNSUlJUWlqq/Px85efnq7S0VKmpqS11CV5zsbn7sRUrVmjz5s2Nfq0Dc9f43H399dcaPny4fvnLX%2BrTTz/V559/rscee0wdO3Y0x8yePVvLly9XXl6eNm3apBMnTig5OVlnz55tqcvwikvN3YMPPqj8/Hy99dZb2r17tx588EFlZGTo/fffN8e017lbv369Zs6cqeLiYhUUFOjMmTNKTEzUyZP//8uTLzU3Z8%2Be1fjx43Xy5Elt2rRJeXl5eu%2B995SZmemty2oRl5q7w4cP6/Dhw5o/f7527typJUuWKD8/X9OnTzfP0SbmzgAugyRj%2BfLlFx2zZcsWQ5Kxf/9%2BwzAM44svvjAkGcXFxeaYoqIiQ5Lxf//3f1e0Xl/S1NwdPHjQuOaaa4yysjKje/fuxoIFC8w%2B5u4Hjc3d5MmTjbvvvrvJY44fP24EBAQYeXl5ZtuhQ4eMDh06GPn5%2BVesVl/T2Nz16dPH%2BOMf/%2BjRNmDAAOM///M/DcNg7n6sqqrKkGSsX7/eMIzmzc3q1auNDh06GIcOHTLH/O1vfzPsdrvhcrla9gK86MK5a8y7775rBAYGGvX19YZhtI25YwULV4zL5ZLNZjO/h7GoqEgOh0NDhgwxxwwdOlQOh0OFhYXeKtMnnDt3TqmpqXrooYfUp0%2BfBv3MXePOnTunVatW6frrr1dSUpLCw8M1ZMgQj7fCSkpKVF9fr8TERLMtKipKcXFx7XruJGn48OFauXKlDh06JMMwtG7dOn355ZdKSkqSxNz92Pm3r0JDQyU1b26KiooUFxfnsSKdlJQkt9utkpKSFqzeuy6cu6bGhISEmA8bbQtzR8DCFXH69Gk98sgjSklJMb/As7KyUuHh4Q3GhoeHN/gy7fbmv/7rv%2BTv769Zs2Y12s/cNa6qqkonTpzQU089pbFjx2rNmjW6/fbbNWnSJK1fv17SD3MXGBioLl26eBwbERHRrudOkl588UX17t1b0dHRCgwM1NixY/Xyyy9r%2BPDhkpi78wzD0Jw5czR8%2BHDFxcVJat7cVFZWKiIiwqO/S5cuCgwMbDfz19jcXejYsWP605/%2BpPvuu89sawtz17aeSw%2BfUF9fr7vuukvnzp3Tyy%2B/7NFns9kajDcMo9H29qKkpEQvvPCCtm3bdtF5YO4aOnfunCTp1ltv1YMPPihJ%2Bpd/%2BRcVFhbqf/7nf3TTTTc1eWx7nzvph4BVXFyslStXqnv37tqwYYNmzJihyMhIjRkzpsnj2tvcpaena8eOHdq0adMlx144N%2B39f7eXmruamhqNHz9evXv31uOPP%2B7R19rnjhUsWKq%2Bvl533nmn9u3bp4KCAnP1SpKcTqeOHDnS4JijR482%2BH8q7cnGjRtVVVWla6%2B9Vv7%2B/vL399f%2B/fuVmZmpf/qnf5LE3DUlLCxM/v7%2B6t27t0d7r169zE8ROp1O1dXVqbq62mNMVVVVu567U6dO6dFHH9Vzzz2nCRMmqF%2B/fkpPT9fkyZM1f/58ScydJGVkZGjlypVat26doqOjzfbmzI3T6Wyw2lJdXa36%2Bvp2MX9Nzd15tbW1Gjt2rK666iotX75cAQEBZl9bmDsCFixzPlzt3btXa9euVdeuXT364%2BPj5XK5tGXLFrNt8%2BbNcrlcSkhIaOlyfUZqaqp27Nih0tJSc4uKitJDDz2kjz76SBJz15TAwEANHjy4wcfnv/zyS3Xv3l2SNHDgQAUEBKigoMDsr6ioUFlZWbueu/r6etXX16tDB88/A35%2BfubKYHueO8MwlJ6ermXLlumTTz5RbGysR39z5iY%2BPl5lZWWqqKgwx6xZs0Z2u10DBw5smQvxgkvNnfTDylViYqICAwO1cuVKj0/9Sm1k7rxzbz1ao9raWmP79u3G9u3bDUnGc889Z2zfvt3Yv3%2B/UV9fb0ycONGIjo42SktLjYqKCnNzu93mOcaOHWv069fPKCoqMoqKioy%2BffsaycnJXryqlnGxuWvMhZ8iNAzmrqm5W7ZsmREQEGC8%2Buqrxt69e42XXnrJ8PPzMzZu3Gie4/777zeio6ONtWvXGtu2bTNGjRpl9O/f3zhz5oy3LqtFXGrubrrpJqNPnz7GunXrjG%2B%2B%2BcZ4/fXXjY4dOxovv/yyeY72OncPPPCA4XA4jE8//dTj99n3339vjrnU3Jw5c8aIi4szRo8ebWzbts1Yu3atER0dbaSnp3vrslrEpeaupqbGGDJkiNG3b1/jq6%2B%2B8hjTluaOgIVmW7dunSGpwTZ16lRj3759jfZJMtatW2ee49ixY8aUKVOM4OBgIzg42JgyZYpRXV3tvYtqIRebu8Y0FrCYu6bn7i9/%2BYtx3XXXGR07djT69%2B9vrFixwuMcp06dMtLT043Q0FCjU6dORnJysnHgwIEWvpKWd6m5q6ioMKZNm2ZERUUZHTt2NHr27Gk8%2B%2Byzxrlz58xztNe5a%2Br32euvv26Oac7c7N%2B/3xg/frzRqVMnIzQ01EhPTzdOnz7dwlfTsi41d039dynJ2Ldvn3me1j53NsMwDOvXxQAAANov7sECAACwGAELAADAYgQsAAAAixGwAAAALEbAAgAAsBgBCwAAwGIELAAAAIsRsAAAACxGwAIAALAYAQsAAMBiBCwAAACLEbAAAAAsRsACAACwGAELAADAYgQsAAAAixGwAAAALEbAAgAAsBgBCwAAwGL/DwULhEamhAPDAAAAAElFTkSuQmCC"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common4291415193915693963">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">220.5</td>
        <td class="number">384</td>
        <td class="number">50.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">147.0</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">122.5</td>
        <td class="number">128</td>
        <td class="number">16.7%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">110.25</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:17%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme4291415193915693963">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">110.25</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:17%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">122.5</td>
        <td class="number">128</td>
        <td class="number">16.7%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">147.0</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">220.5</td>
        <td class="number">384</td>
        <td class="number">50.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">110.25</td>
        <td class="number">64</td>
        <td class="number">8.3%</td>
        <td>
            <div class="bar" style="width:17%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">122.5</td>
        <td class="number">128</td>
        <td class="number">16.7%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">147.0</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:50%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">220.5</td>
        <td class="number">384</td>
        <td class="number">50.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_X5">X5<br/>
            <small>Boolean</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr class="">
                    <th>Distinct count</th>
                    <td>2</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>0.3%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (n)</th>
                    <td>0</td>
                </tr>
            </table>
        </div>
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Mean</th>
                    <td>5.25</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minifreqtable6001261039780697607">
    <table class="mini freq">
        <tr class="">
    <th>3.5</th>
    <td>
        <div class="bar" style="width:100%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 50.0%">
            384
        </div>
        
    </td>
</tr><tr class="">
    <th>7.0</th>
    <td>
        <div class="bar" style="width:100%" data-toggle="tooltip" data-placement="right" data-html="true"
             data-delay=500 title="Percentage: 50.0%">
            384
        </div>
        
    </td>
</tr>
    </table>
</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#freqtable6001261039780697607, #minifreqtable6001261039780697607"
        aria-expanded="true" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="col-md-12 extrapadding collapse" id="freqtable6001261039780697607">
    
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">3.5</td>
        <td class="number">384</td>
        <td class="number">50.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">7.0</td>
        <td class="number">384</td>
        <td class="number">50.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_X6">X6<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>4</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>0.5%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (n)</th>
                    <td>0</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>

        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>3.5</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>2</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>5</td>
                </tr>
                <tr class="ignore">
                    <th>Zeros (%)</th>
                    <td>0.0%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram8524031355942495325">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAAaBJREFUeJzt1zGu00AUhtHr6LXOAiJ7FyyCPVGzJxbBLmxlAY4oKMhQQGge%2BlGkMJalc0pHmnsjz1d4aK216mxd15rnufdYDm5ZlpqmqevMt67TfhvHsap%2B/eHz%2BbzHChzItm01z/Ofe9PTLoEMw1BVVefz%2BV0gHz59efq8r58/vmSvf3l2N3u99l0%2B7k1Pp%2B4T4UAEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKB4G2Poa21qqratu3dbz%2B%2Bf3v6vL%2Bd8z88u5u9XvMuH88e96anoe0wdV3Xmue591gOblmWmqap68xdArnf73W9XmscxxqGofd4Dqa1VrfbrS6XS51Ofb8KdgkEjsJHOgQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBALBTxCJXdsE/cbWAAAAAElFTkSuQmCC">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives8524031355942495325,#minihistogram8524031355942495325"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives8524031355942495325">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles8524031355942495325"
                                                  aria-controls="quantiles8524031355942495325" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram8524031355942495325" aria-controls="histogram8524031355942495325"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common8524031355942495325" aria-controls="common8524031355942495325"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme8524031355942495325" aria-controls="extreme8524031355942495325"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>

    </ul>

    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles8524031355942495325">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>2</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>2</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>2.75</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>3.5</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>4.25</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>5</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>5</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>3</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>1.5</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>1.1188</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.31965</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>-1.361</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>3.5</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>1</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>2688</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>1.2516</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>6.1 KiB</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram8524031355942495325">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAIABJREFUeJzt3XtcVWW%2Bx/HvVmB7CUi8sGFEI49Wipq3UbRG1MLIy2mcJh2NwTKqyUuKjiN5KuyUON2m0pOnm5cmTetU5pwaEyvBDlLe8P7yeKHEAtFGAU23aOv80biPW0DQHtgL9uf9eq3Xi/WsZy1%2B6%2Bnp1bdnrb1xWJZlCQAAAMY08HUBAAAA9Q0BCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACG1euAlZ6erl69eik4OFitWrXSHXfcoT179nj1cbvdmjhxolq0aKGmTZtq%2BPDhOnTokFefgwcPatiwYWratKlatGihSZMm6cyZM7V5KwAAoA4J8HUBNSkzM1Pjx49Xr169dPbsWc2cOVPx8fHatWuXmjZtKkmaPHmy/va3v2nZsmVq3ry5pk6dqqFDh2rTpk1q2LChzp07pyFDhqhly5b64osv9P333yspKUmWZWnu3LnVquPHH3/Ud999p%2BDgYDkcjpq8ZQAA8E%2BWZam0tFSRkZFq0KCW15QsP1JUVGRJsjIzMy3Lsqzjx49bgYGB1rJlyzx9vv32W6tBgwbWqlWrLMuyrI8//thq0KCB9e2333r6vP3225bT6bSKi4ur9Xvz8/MtSWxsbGxsbGw%2B2PLz8w2mieqp1ytYFysuLpYkhYWFSZI2bdqksrIyxcfHe/pERkYqJiZG2dnZGjx4sNavX6%2BYmBhFRkZ6%2BgwePFhut1ubNm3SgAEDqvy9wcHBkqT8/HyFhISYvCUAAFCJkpISRUVFef47XJv8JmBZlqWUlBTddNNNiomJkSQVFhYqKChIzZo18%2BobHh6uwsJCT5/w8HCv482aNVNQUJCnz8Xcbrfcbrdnv7S0VJIUEhJCwAIAoJb54vWcev2S%2B4UmTJigbdu26e23366yr2VZXv8wKvoHc3GfC6Wnpys0NNSzRUVFXXnhAACgzvGLgDVx4kStXLlSn3/%2BuVq3bu1pd7lcOnPmjI4dO%2BbVv6ioyLNq5XK5yq1UHTt2TGVlZeVWts5LTU1VcXGxZ8vPzzd8RwAAwM7qdcCyLEsTJkzQ%2B%2B%2B/r88%2B%2B0zR0dFex3v06KHAwEBlZGR42goKCrRjxw717dtXkhQbG6sdO3aooKDA02f16tVyOp3q0aNHhb/X6XR6HgfyWBAAAP9Tr9/BGj9%2BvJYuXaoPP/xQwcHBnpWo0NBQNW7cWKGhoRo3bpymTp2q5s2bKywsTNOmTVPnzp11yy23SJLi4%2BPVsWNHJSYm6plnntE//vEPTZs2TcnJyQQnAABQIYdlWZavi6gplb0jtXDhQo0dO1aSdPr0af3xj3/U0qVLderUKQ0aNEgvv/yy13tTBw8e1EMPPaTPPvtMjRs31ujRo/Xss8/K6XRWq46SkhKFhoaquLiYUAYAQC3x5X9/63XAsgsCFgAAtc%2BX//2t1%2B9gAQAA%2BAIBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAyr11806g96zlzl6xKqbeNTt/m6hHqNuYDzmAs4j7ngO6xgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAw%2Bp1wMrKytKwYcMUGRkph8OhFStWeB13OBwVbs8884ynzzXXXFPu%2BIwZM2r7VgAAQB0S4OsCatLJkyfVtWtX3XPPPfrNb35T7nhBQYHX/t///neNGzeuXN8nnnhCycnJnv2rrrqqZgoGAAD1Qr0OWAkJCUpISKj0uMvl8tr/8MMPNWDAAF177bVe7cHBweX6AgAAVKZePyK8HIcPH9ZHH32kcePGlTv25z//Wc2bN9eNN96op556SmfOnLnktdxut0pKSrw2AADgP%2Br1CtblWLx4sYKDgzVixAiv9ocffljdu3dXs2bN9NVXXyk1NVV5eXl6/fXXK71Wenq6Zs2aVdMlAwAAmyJg/dOCBQs0ZswYNWrUyKt9ypQpnp%2B7dOmiZs2a6c477/SsalUkNTVVKSkpnv2SkhJFRUXVTOEAAMB2CFiS1q1bpz179mj58uVV9u3Tp48kad%2B%2BfZUGLKfTKafTabRGAABQd/AOlqQ33nhDPXr0UNeuXavsu2XLFklSRERETZcFAADqqHq9gnXixAnt27fPs5%2BXl6fc3FyFhYWpTZs2kn56fPfuu%2B/queeeK3f%2B%2BvXrlZOTowEDBig0NFQbNmzQlClTNHz4cM/5AAAAF6vXAWvjxo0aMGCAZ//8e1FJSUlatGiRJGnZsmWyLEu/%2B93vyp3vdDq1fPlyzZo1S263W23btlVycrKmT59eK/UDAIC6qV4HrLi4OFmWdck%2B999/v%2B6///4Kj3Xv3l05OTk1URoAAKjHeAcLAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhtXrgJWVlaVhw4YpMjJSDodDK1as8Do%2BduxYORwOr61Pnz5efdxutyZOnKgWLVqoadOmGj58uA4dOlSbtwEAAOqYeh2wTp48qa5du2revHmV9rnttttUUFDg2T7%2B%2BGOv45MnT9YHH3ygZcuW6YsvvtCJEyc0dOhQnTt3rqbLBwAAdVSArwuoSQkJCUpISLhkH6fTKZfLVeGx4uJivfHGG/rrX/%2BqW265RZL01ltvKSoqSmvWrNHgwYON1wwAAOq%2Ber2CVR1r165Vq1at1KFDByUnJ6uoqMhzbNOmTSorK1N8fLynLTIyUjExMcrOzvZFuQAAoA6o1ytYVUlISNBvf/tbtW3bVnl5eXr00Uc1cOBAbdq0SU6nU4WFhQoKClKzZs28zgsPD1dhYWGl13W73XK73Z79kpKSGrsHAABgP34dsEaOHOn5OSYmRj179lTbtm310UcfacSIEZWeZ1mWHA5HpcfT09M1a9Yso7UCAIC6w%2B8fEV4oIiJCbdu21d69eyVJLpdLZ86c0bFjx7z6FRUVKTw8vNLrpKamqri42LPl5%2BfXaN0AAMBeCFgX%2BP7775Wfn6%2BIiAhJUo8ePRQYGKiMjAxPn4KCAu3YsUN9%2B/at9DpOp1MhISFeGwAA8B/1%2BhHhiRMntG/fPs9%2BXl6ecnNzFRYWprCwMKWlpek3v/mNIiIi9PXXX%2BuRRx5RixYt9Otf/1qSFBoaqnHjxmnq1Klq3ry5wsLCNG3aNHXu3NnzqUIAAICL1euAtXHjRg0YMMCzn5KSIklKSkrS/PnztX37dr355ps6fvy4IiIiNGDAAC1fvlzBwcGec/7yl78oICBAd911l06dOqVBgwZp0aJFatiwYa3fDwAAqBvqdcCKi4uTZVmVHv/kk0%2BqvEajRo00d%2B5czZ0712RpAACgHuMdLAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCsXgesrKwsDRs2TJGRkXI4HFqxYoXnWFlZmf70pz%2Bpc%2BfOatq0qSIjI/X73/9e3333ndc1rrnmGjkcDq9txowZtX0rAACgDqnXAevkyZPq2rWr5s2bV%2B7YDz/8oM2bN%2BvRRx/V5s2b9f777%2Bt///d/NXz48HJ9n3jiCRUUFHi2f/u3f6uN8gEAQB0V4OsCalJCQoISEhIqPBYaGqqMjAyvtrlz5%2BqXv/ylDh48qDZt2njag4OD5XK5arRWAABQf9TrFazLVVxcLIfDoauvvtqr/c9//rOaN2%2BuG2%2B8UU899ZTOnDnjowoBAEBdUK9XsC7H6dOnNWPGDI0ePVohISGe9ocffljdu3dXs2bN9NVXXyk1NVV5eXl6/fXXK72W2%2B2W2%2B327JeUlNRo7QAAwF4IWPrphfdRo0bpxx9/1Msvv%2Bx1bMqUKZ6fu3TpombNmunOO%2B/0rGpVJD09XbNmzarRmgEAgH35/SPCsrIy3XXXXcrLy1NGRobX6lVF%2BvTpI0nat29fpX1SU1NVXFzs2fLz843WDAAA7M2vV7DOh6u9e/fq888/r3RF6kJbtmyRJEVERFTax%2Bl0yul0GqsTAADULfU6YJ04ccJrpSkvL0%2B5ubkKCwtTZGSk7rzzTm3evFn//d//rXPnzqmwsFCSFBYWpqCgIK1fv145OTkaMGCAQkNDtWHDBk2ZMkXDhw/3%2BpQhAADAhWz7iPCtt97S6dOnf9Y1Nm7cqG7duqlbt26SpJSUFHXr1k2PPfaYDh06pJUrV%2BrQoUO68cYbFRER4dmys7Ml/bQStXz5csXFxaljx4567LHHlJycrLfffvtn3x8AAKi/bLuClZKSogkTJmjkyJEaN26cfvnLX172NeLi4mRZVqXHL3VMkrp3766cnJzL/r0AAMC/2XYF67vvvtOCBQtUUFCgm266SZ06ddJzzz2nI0eO%2BLo0AACAS7JtwAoICNCIESO0cuVKHTx4UElJSVqwYIFat26tESNG6KOPPqpyBQoAAMAXbBuwLuRyuTRo0CDFxcXJ4XBo48aNGj16tNq3b69169b5ujwAAAAvtg5YR48e1QsvvKCuXbuqX79%2BKioq0ooVK/TNN9/o22%2B/1dChQ/X73//e12UCAAB4se1L7r/%2B9a/18ccfKzo6Wvfdd5%2BSkpLUsmVLz/GrrrpK06dP10svveTDKgEAAMqzbcAKCQnRmjVrdPPNN1faJyIiQnv37q3FqgAAAKpm24C1ePHiKvs4HA61a9euFqoBAACoPtu%2BgzVlyhTNmzevXPt//Md/aOrUqT6oCAAAoHpsG7Deffddzx9WvlBsbKyWL1/ug4oAAACqx7YB6%2BjRo2rWrFm59pCQEB09etQHFQEAAFSPbQNWu3bt9Mknn5Rr/%2BSTTxQdHe2DigAAAKrHti%2B5T548WZMnT9b333%2BvgQMHSpI%2B/fRTPf3003r22Wd9XB0AAEDlbBuwkpOTdfr0ac2ePVuPP/64JKl169Z66aWXdO%2B99/q4OgAAgMrZNmBJ0sSJEzVx4kQVFBSocePGuvrqq31dEgAAQJVsHbDOi4iI8HUJAAAA1Wbbl9yPHDmie%2B65R23atFGjRo0UFBTktQEAANiVbVewxo4dq/379%2BuPf/yjIiIi5HA4fF0SAABAtdg2YGVlZSkrK0vdunXzdSkAAACXxbaPCFu3bs2qFQAAqJNsG7D%2B8pe/KDU1VYcOHfJ1KQAAAJfFto8IExMTVVpaqrZt2yokJESBgYFex4uKinxUGQAAwKXZNmDNmTPH1yUAAABcEdsGrHHjxvm6BAAAgCti23ewJOnrr79WWlqaEhMTPY8EV69erd27d/u4MgAAgMrZNmCtW7dOnTp1UmZmpt555x2dOHFCkrR582Y99thjPq4OAACgcrYNWH/605%2BUlpamzz//3Oub2wcOHKicnBwfVgYAAHBptg1Y27Zt05133lmuvVWrVjpy5IgPKgIAAKge2wasq6%2B%2BWoWFheXac3Nz9Ytf/MIHFQEAAFSPbQPWqFGjNGPGDB05csTzje5ffvmlpk2bprvvvtvH1QEAAFTOtgFr9uzZcrlcioiI0IkTJ9SxY0f17dtXvXr10qOPPlqta2RlZWnYsGGKjIyUw%2BHQihUrvI5blqW0tDRFRkaqcePGiouL086dO736HDt2TImJiQoNDVVoaKgSExN1/PhxY/cJAADqH9sGrKCgIC1fvly7du3S0qVLtWDBAu3cuVNvv/22AgKq9/VdJ0%2BeVNeuXTVv3rwKjz/99NN6/vnnNW/ePG3YsEEul0u33nqrSktLPX1Gjx6t3NxcrVq1SqtWrVJubq4SExON3CMAAKifbPtFo%2Bd16NBBHTp0uKJzExISlJCQUOExy7L0wgsvaObMmRoxYoQkafHixQoPD9fSpUv1wAMPaPfu3Vq1apVycnLUu3dvSdJrr72m2NhY7dmzR9ddd92V3RQAAKjXbBuw7r///ksef/XVV3/W9fPy8lRYWKj4%2BHhPm9PpVP/%2B/ZWdna0HHnhA69evV2hoqCdcSVKfPn0UGhqq7OzsSgOW2%2B2W2%2B327JeUlPysWgEAQN1i24BVUFDgtV9WVqadO3eqtLRUv/rVr3729c9/QjE8PNyrPTw8XN98842nT6tWrcqd26pVqwo/4Xheenq6Zs2a9bNrBAAAdZNtA9bf/va3cm1nz57VH/7wB91www3Gfs/5TyieZ1mWV9vFxyvqc7HU1FSlpKR49ktKShQVFWWgWgAAUBfY9iX3igQEBGjatGl65plnfva1XC6XJJVbiSoqKvKsarlcLh0%2BfLjcuUeOHCm38nUhp9OpkJAQrw0AAPiPOhWwJOnAgQMqKyv72deJjo6Wy%2BVSRkaGp%2B3MmTPKzMxU3759JUmxsbEqLi7WV1995enz5Zdfqri42NMHAADgYrZ9RDh9%2BnSvfcuyVFBQoJUrV2rMmDHVusaJEye0b98%2Bz35eXp5yc3MVFhamNm3aaPLkyZo9e7bat2%2Bv9u3ba/bs2WrSpIlGjx4tSbrhhht02223KTk5Wa%2B88oqkn16%2BHzp0KJ8gBAAAlbJtwFq/fr3XfoMGDdSyZUvNmTNHycnJ1brGxo0bNWDAAM/%2B%2BfeikpKStGjRIk2fPl2nTp3SQw89pGPHjql3795avXq1goODPecsWbJEkyZN8nzacPjw4ZV%2BrxYAAIBk44C1bt26n32NuLg4WZZV6XGHw6G0tDSlpaVV2icsLExvvfXWz64FAAD4jzr3DhYAAIDd2XYFq1evXpf8KoQLXfgSOgAAgK/ZNmANGDBAr7zyijp06KDY2FhJUk5Ojvbs2aMHHnhATqfTxxUCAABUzLYB6/jx4xo/frxmz57t1T5z5kwdPnxYr7/%2Buo8qAwAAuDTbvoP1zjvv6J577inXPnbsWL377rs%2BqAgAAKB6bBuwnE6nsrOzy7VnZ2fzeBAAANiabR8RTpo0SQ8%2B%2BKC2bNmiPn36SPrpHazXXntNjzzyiI%2BrAwAAqJxtA9bMmTMVHR2tF198UQsWLJD00zerv/baa55vWgcAALAj2wYsSRo9ejRhCgAA1Dm2fQdLkkpKSrRo0SI99thjOnbsmCRp69atKigo8HFlAAAAlbPtCtaOHTt0yy23qEmTJsrPz9fYsWPVrFkzvfPOOzp06JAWL17s6xIBAAAqZNsVrClTpmj06NHav3%2B/GjVq5GkfMmSIsrKyfFgZAADApdl2BWvDhg2aP39%2BuT%2BX84tf/IJHhAAAwNZsu4IVFBSkEydOlGvfu3evWrRo4YOKAAAAqse2AWv48OH693//d509e1aS5HA49O2332rGjBkaMWKEj6sDAAConG0D1nPPPafvvvtOLpdLp06d0sCBA3XttdeqUaNG5f4%2BIQAAgJ3Y9h2s0NBQZWdnKyMjQ5s3b9aPP/6o7t27a/DgweXeywIAALATWwassrIy3X777Xr55ZcVHx%2Bv%2BPh4X5cEAABQbbZ8RBgYGKgtW7awUgUAAOokWwYsSbr77ru1cOFCX5cBAABw2Wz5iPC8efPmac2aNerZs6eaNm3qdezpp5/2UVUAAACXZtuAtWnTJnXp0kWStG3bNq9jPDoEAAB2ZruAdeDAAUVHR2vdunW%2BLgUAAOCK2O4drPbt2%2BvIkSOe/ZEjR%2Brw4cM%2BrAgAAODy2C5gWZbltf/xxx/r5MmTPqoGAADg8tkuYAEAANR1tgtYDoej3EvsvNQOAADqEtu95G5ZlsaOHSun0ylJOn36tB588MFyX9Pw/vvv%2B6I8AACAKtluBSspKUmtWrVSaGioQkNDdffddysyMtKzf34z5ZprrvGsml24jR8/XpIUFxdX7tioUaOM/X4AAFD/2G4Fq7a/vX3Dhg06d%2B6cZ3/Hjh269dZb9dvf/tbTlpycrCeeeMKz37hx41qtEQAA1C22C1i1rWXLll77c%2BbMUbt27dS/f39PW5MmTeRyuWq7NAAAUEfZ7hGhL505c0ZvvfWW7r33Xq8X65csWaIWLVqoU6dOmjZtmkpLS31YJQAAsDu/X8G60IoVK3T8%2BHGNHTvW0zZmzBhFR0fL5XJpx44dSk1N1datW5WRkVHpddxut9xut2e/pKSkJssGAAA2Q8C6wBtvvKGEhARFRkZ62pKTkz0/x8TEqH379urZs6c2b96s7t27V3id9PR0zZo1q8brBQAA9sQjwn/65ptvtGbNGt13332X7Ne9e3cFBgZq7969lfZJTU1VcXGxZ8vPzzddLgAAsDFWsP5p4cKFatWqlYYMGXLJfjt37lRZWZkiIiIq7eN0Oj3f4wUAAPwPAUvSjz/%2BqIULFyopKUkBAf8/JPv379eSJUt0%2B%2B23q0WLFtq1a5emTp2qbt26qV%2B/fj6sGAAA2BkBS9KaNWt08OBB3XvvvV7tQUFB%2BvTTT/Xiiy/qxIkTioqK0pAhQ/T444%2BrYcOGPqoWAADYHQFLUnx8vCzLKtceFRWlzMxMH1QEAADqMl5yBwAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMP8PmClpaXJ4XB4bS6Xy3PcsiylpaUpMjJSjRs3VlxcnHbu3OnDigEAgN35fcCSpE6dOqmgoMCzbd%2B%2B3XPs6aef1vPPP6958%2BZpw4YNcrlcuvXWW1VaWurDigEAgJ0RsCQFBATI5XJ5tpYtW0r6afXqhRde0MyZMzVixAjFxMRo8eLF%2BuGHH7R06VIfVw0AAOyKgCVp7969ioyMVHR0tEaNGqUDBw5IkvLy8lRYWKj4%2BHhPX6fTqf79%2Bys7O9tX5QIAAJsL8HUBvta7d2%2B9%2Beab6tChgw4fPqwnn3xSffv21c6dO1VYWChJCg8P9zonPDxc33zzTaXXdLvdcrvdnv2SkpKaKR4AANiS3weshIQEz8%2BdO3dWbGys2rVrp8WLF6tPnz6SJIfD4XWOZVnl2i6Unp6uWbNm1UzBAADA9nhEeJGmTZuqc%2BfO2rt3r%2BfThOdXss4rKioqt6p1odTUVBUXF3u2/Pz8Gq0ZAADYCwHrIm63W7t371ZERISio6PlcrmUkZHhOX7mzBllZmaqb9%2B%2BlV7D6XQqJCTEawMAAP7D7x8RTps2TcOGDVObNm1UVFSkJ598UiUlJUpKSpLD4dDkyZM1e/ZstW/fXu3bt9fs2bPVpEkTjR492telAwAAm/L7gHXo0CH97ne/09GjR9WyZUv16dNHOTk5atu2rSRp%2BvTpOnXqlB566CEdO3ZMvXv31urVqxUcHOzjygEAgF35fcBatmzZJY87HA6lpaUpLS2tdgoCAAB1Hu9gAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGF%2BH7DS09PVq1cvBQcHq1WrVrrjjju0Z88erz5xcXFyOBxe26hRo3xUMQAAsDu/D1iZmZkaP368cnJylJGRobNnzyo%2BPl4nT5706pecnKyCggLP9sorr/ioYgAAYHcBvi7A11atWuW1v3DhQrVq1UqbNm3Sr371K097kyZN5HK5ars8AABQB/n9CtbFiouLJUlhYWFe7UuWLFGLFi3UqVMnTZs2TaWlpZVew%2B12q6SkxGsDAAD%2Bw%2B9XsC5kWZZSUlJ00003KSYmxtM%2BZswYRUdHy%2BVyaceOHUpNTdXWrVuVkZFR4XXS09M1a9as2iobAADYDAHrAhMmTNC2bdv0xRdfeLUnJyd7fo6JiVH79u3Vs2dPbd68Wd27dy93ndTUVKWkpHj2S0pKFBUVVXOFAwAAWyFg/dPEiRO1cuVKZWVlqXXr1pfs2717dwUGBmrv3r0VBiyn0ymn01lTpQIAAJvz%2B4BlWZYmTpyoDz74QGvXrlV0dHSV5%2BzcuVNlZWWKiIiohQoBAEBd4/cBa/z48Vq6dKk%2B/PBDBQcHq7CwUJIUGhqqxo0ba//%2B/VqyZIluv/12tWjRQrt27dLUqVPVrVs39evXz8fVAwAAO/L7TxHOnz9fxcXFiouLU0REhGdbvny5JCkoKEiffvqpBg/gcLBAAAAKHUlEQVQerOuuu06TJk1SfHy81qxZo4YNG/q4egAAYEd%2Bv4JlWdYlj0dFRSkzM7OWqgEAAPWB369gAQAAmEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEELAAAAMMIWAAAAIYRsAAAAAwjYAEAABhGwAIAADCMgAUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwDACFgAAgGEErGp6%2BeWXFR0drUaNGqlHjx5at26dr0sCAAA2RcCqhuXLl2vy5MmaOXOmtmzZoptvvlkJCQk6ePCgr0sDAAA2RMCqhueff17jxo3TfffdpxtuuEEvvPCCoqKiNH/%2BfF%2BXBgAAbCjA1wXY3ZkzZ7Rp0ybNmDHDqz0%2BPl7Z2dkVnuN2u%2BV2uz37xcXFkqSSkhLj9Z1znzR%2BzZpSE/eP/8dcwHnMBZzn73Ph/DUtyzJ%2B7aoQsKpw9OhRnTt3TuHh4V7t4eHhKiwsrPCc9PR0zZo1q1x7VFRUjdRYV4Q%2B5%2BsKYBfMBZzHXMB5NTkXSktLFRoaWnO/oAIErGpyOBxe%2B5ZllWs7LzU1VSkpKZ79H3/8Uf/4xz/UvHnzSs%2B5EiUlJYqKilJ%2Bfr5CQkKMXbc%2BYYwujfGpGmNUNcbo0hifqtXUGFmWpdLSUkVGRhq7ZnURsKrQokULNWzYsNxqVVFRUblVrfOcTqecTqdX29VXX11jNYaEhPAvbRUYo0tjfKrGGFWNMbo0xqdqNTFGtb1ydR4vuVchKChIPXr0UEZGhld7RkaG%2Bvbt66OqAACAnbGCVQ0pKSlKTExUz549FRsbq1dffVUHDx7Ugw8%2B6OvSAACADTVMS0tL83URdhcTE6PmzZtr9uzZevbZZ3Xq1Cn99a9/VdeuXX1dmho2bKi4uDgFBJCVK8MYXRrjUzXGqGqM0aUxPlWrb2PksHzx2UUAAIB6jHewAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBy6bS09PVq1cvBQcHq1WrVrrjjju0Z8%2BeKs9777331LFjRzmdTnXs2FEffPBBLVTrG1cyRosWLZLD4Si3nT59upaqrj3z589Xly5dPF/cFxsbq7///e%2BXPMef5o90%2BWPkT/OnIunp6XI4HJo8efIl%2B/nbPLpQdcbI3%2BZRWlpauXt1uVyXPCczM1M9evRQo0aNdO211%2Bo///M/a6lacwhYNpWZmanx48crJydHGRkZOnv2rOLj43XyZOV/uHP9%2BvUaOXKkEhMTtXXrViUmJuquu%2B7Sl19%2BWYuV154rGSPpp28KLigo8NoaNWpUS1XXntatW2vOnDnauHGjNm7cqIEDB%2Bpf//VftXPnzgr7%2B9v8kS5/jCT/mT8X27Bhg1599VV16dLlkv38cR6dV90xkvxvHnXq1MnrXrdv315p37y8PN1%2B%2B%2B26%2BeabtWXLFj3yyCOaNGmS3nvvvVqs2AALdUJRUZElycrMzKy0z1133WXddtttXm2DBw%2B2Ro0aVdPl2UJ1xmjhwoVWaGhoLVZlL82aNbNef/31Co/5%2B/w571Jj5K/zp7S01Grfvr2VkZFh9e/f33r44Ycr7euv8%2Bhyxsjf5tHjjz9ude3atdr9p0%2Bfbl1//fVebQ888IDVp08f06XVKFaw6oji4mJJUlhYWKV91q9fr/j4eK%2B2wYMHKzs7u0Zrs4vqjJEknThxQm3btlXr1q01dOhQbdmypTbK86lz585p2bJlOnnypGJjYyvs4%2B/zpzpjJPnn/Bk/fryGDBmiW265pcq%2B/jqPLmeMJP%2BbR3v37lVkZKSio6M1atQoHThwoNK%2Blc2hjRs3qqysrKZLNaZ%2BfF1qPWdZllJSUnTTTTcpJiam0n6FhYXl/gB1eHh4uT9UXR9Vd4yuv/56LVq0SJ07d1ZJSYlefPFF9evXT1u3blX79u1rseLasX37dsXGxur06dO66qqr9MEHH6hjx44V9vXX%2BXM5Y%2BRv80eSli1bpk2bNmnjxo3V6u%2BP8%2Bhyx8jf5lHv3r315ptvqkOHDjp8%2BLCefPJJ9e3bVzt37lTz5s3L9a9sDp09e1ZHjx5VREREbZX%2BsxCw6oAJEyZo27Zt%2BuKLL6rs63A4vPYtyyrXVh9Vd4z69OmjPn36ePb79eun7t27a%2B7cuXrppZdqusxad9111yk3N1fHjx/Xe%2B%2B9p6SkJGVmZlYaIPxx/lzOGPnb/MnPz9fDDz%2Bs1atXX9b7Qf40j65kjPxtHiUkJHh%2B7ty5s2JjY9WuXTstXrxYKSkpFZ5T0RyqqN3OCFg2N3HiRK1cuVJZWVlq3br1Jfu6XK5y/5dYVFRU7v8E6pvLGaOLNWjQQL169dLevXtrqDrfCgoK0r/8y79Iknr27KkNGzboxRdf1CuvvFKur7/On8sZo4vV9/mzadMmFRUVqUePHp62c%2BfOKSsrS/PmzZPb7VbDhg29zvG3eXQlY3Sx%2Bj6PLta0aVN17ty50vutbA4FBARUuOJlV7yDZVOWZWnChAl6//339dlnnyk6OrrKc2JjY5WRkeHVtnr1avXt27emyvSpKxmjiq6Rm5tbZ5acfy7LsuR2uys85m/zpzKXGqOK%2Btbn%2BTNo0CBt375dubm5nq1nz54aM2aMcnNzKwwO/jaPrmSMLlbf59HF3G63du/eXen9VjaHevbsqcDAwNoo0QyfvFqPKv3hD3%2BwQkNDrbVr11oFBQWe7YcffvD0SUxMtGbMmOHZ/5//%2BR%2BrYcOG1pw5c6zdu3dbc%2BbMsQICAqycnBxf3EKNu5IxSktLs1atWmXt37/f2rJli3XPPfdYAQEB1pdffumLW6hRqampVlZWlpWXl2dt27bNeuSRR6wGDRpYq1evtiyL%2BWNZlz9G/jR/KnPxJ%2BSYR%2BVVNUb%2BNo%2BmTp1qrV271jpw4ICVk5NjDR061AoODra%2B/vpry7Isa8aMGVZiYqKn/4EDB6wmTZpYU6ZMsXbt2mW98cYbVmBgoPVf//VfvrqFK0LAsilJFW4LFy709Onfv7%2BVlJTkdd67775rXXfddVZgYKB1/fXXW%2B%2B9917tFl6LrmSMJk%2BebLVp08YKCgqyWrZsacXHx1vZ2dm1X3wtuPfee622bdt67nXQoEGe4GBZzB/Luvwx8qf5U5mLwwPzqLyqxsjf5tHIkSOtiIgIKzAw0IqMjLRGjBhh7dy503M8KSnJ6t%2B/v9c5a9eutbp162YFBQVZ11xzjTV//vxarvrnc1jWP98cAwAAgBG8gwUAAGAYAQsAAMAwAhYAAIBhBCwAAADDCFgAAACGEbAAAAAMI2ABAAAYRsACAAAwjIAFAABgGAELAADAMAIWAACAYQQsAAAAwwhYAAAAhhGwAAAADCNgAQAAGEbAAgAAMIyABQAAYBgBCwAAwLD/AzIKwHbTSBWoAAAAAElFTkSuQmCC"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common8524031355942495325">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">5</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme8524031355942495325">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">2</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">2</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5</td>
        <td class="number">192</td>
        <td class="number">25.0%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_X7">X7<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>4</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>0.5%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (n)</th>
                    <td>0</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>

        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>0.23438</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>0</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>0.4</td>
                </tr>
                <tr class="alert">
                    <th>Zeros (%)</th>
                    <td>6.2%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram1601895208561110408">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAAbFJREFUeJzt3TGu00AUhtHriNZewJO9CxbBnqjZE4tgF7ayAEcUFGQoIDSgH0U8xjI6p0xkX481nyIrhYfWWqvOtm2rZVl6j%2BXk1nWteZ67znzTddoP4zhW1fcFT9N0xCVwIvu%2B17IsP/dNT4cEMgxDVVVN03RYIG/ff3z6mE8f3v2DK/l7z66l1zpe%2Bx4/9k1Pl%2B4T4UQEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBIe8xDP5n16uyfn5BYFAIBAIBAKBQCAQCAQCgUAgEAgEh/xR2Fqrqqp933/57uuXz0%2Bf73fn%2BZNec3p4di291vFa9/jx2WPf9DS0A6Zu21bLsvQey8mt61rzPHedeUgg9/u9rtdrjeNYwzD0Hs/JtNbqdrvVy8tLXS59nwoOCQTOwkM6BAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAsE3RBJd3VHGtOsAAAAASUVORK5CYII%3D">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives1601895208561110408,#minihistogram1601895208561110408"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives1601895208561110408">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles1601895208561110408"
                                                  aria-controls="quantiles1601895208561110408" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram1601895208561110408" aria-controls="histogram1601895208561110408"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common1601895208561110408" aria-controls="common1601895208561110408"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme1601895208561110408" aria-controls="extreme1601895208561110408"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>

    </ul>

    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles1601895208561110408">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>0.1</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>0.25</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>0.4</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>0.4</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>0.4</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>0.4</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>0.3</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>0.13322</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.56841</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>-1.3276</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>0.23438</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>0.11328</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>-0.060254</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>180</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>0.017748</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>6.1 KiB</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram1601895208561110408">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAIABJREFUeJzt3Xt8VPWd//H3mMskZpOBEJJJIKQpJd5CqVwKQZGAGEgFarErNCVLXBbrKlgaWBfKtsTWJhRFaUWtUhW8oq5oce1SgiJgQ0QRKoQsRa6hJgQUc0Ecbt/fHz6Yn2MSSOCbzEl4PR%2BP8zBzzne%2B8/nkzJH348yZE5cxxggAAADWXBLsAgAAADoaAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALOvQAauoqEgDBgxQdHS04uPjddNNN2nHjh0BYzIzM%2BVyuQKWCRMmBIw5cuSIcnNz5fF45PF4lJubq88%2B%2B6wtWwEAAO2Iyxhjgl1Eaxk1apQmTJigAQMG6OTJk5ozZ462bt2q7du3KyoqStKXASstLU2/%2BtWv/M%2BLjIyUx%2BPxP87OztaBAwf0%2BOOPS5Juu%2B02feMb39Drr7/erDpOnz6tjz/%2BWNHR0XK5XBY7BAAATTHGqK6uTklJSbrkkjY%2Bp2QuItXV1UaSWbt2rX/d0KFDzU9/%2BtMmn7N9%2B3YjyZSWlvrXbdiwwUgy//d//9es162oqDCSWFhYWFhYWIKwVFRUnH94OE%2BhuojU1NRIkmJjYwPWP/fcc3r22WeVkJCg7OxszZ07V9HR0ZKkDRs2yOPxaODAgf7xgwYNksfjUUlJiS677LJzvu6ZuSoqKhQTE2OrHQAAcBa1tbVKTk72/zvcli6agGWMUX5%2Bvq699lqlp6f71//4xz9WamqqvF6vtm3bptmzZ%2Btvf/ubiouLJUlVVVWKj49vMF98fLyqqqoafS2fzyefz%2Bd/XFdXJ0mKiYkhYAEA0MaCcXnORROwpk6dqg8//FDvvPNOwPopU6b4f05PT1evXr3Uv39/ffDBB%2Brbt6%2BkxneMMabJHVZUVKR77rnHYvUAAKA96dDfIjxj2rRpWrFihdasWaPu3bufdWzfvn0VFhamnTt3SpK8Xq8OHjzYYNyhQ4eUkJDQ6ByzZ89WTU2Nf6moqLjwJgAAQLvRoQOWMUZTp07V8uXL9dZbbyk1NfWczykrK9OJEyeUmJgoScrIyFBNTY02btzoH/Puu%2B%2BqpqZGgwcPbnQOt9vt/ziQjwUBALj4dOjbNNxxxx16/vnn9ac//SngYnSPx6PIyEjt2rVLzz33nL73ve8pLi5O27dv14wZMxQZGan33ntPISEhkr68TcPHH3%2Bsxx57TNKXt2lISUlp9m0aamtr5fF4VFNTQ9gCAKCNBPPf3w4dsJq6Ruqpp55SXl6eKioqNHHiRG3btk319fVKTk7WjTfeqLlz5wZ80/DTTz/VXXfdpRUrVkiSxo4dq0WLFqlTp07NqoOABQBA2yNgdXAELAAA2l4w//3t0NdgAQAABAMBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACy7aP4WIdBS/eesDHYJLfL%2Bb0YFuwQ4RHt67/K%2BbV28F4KHM1gAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMCyDh2wioqKNGDAAEVHRys%2BPl433XSTduzYETDG5/Np2rRpiouLU1RUlMaOHasDBw4EjNm/f7/GjBmjqKgoxcXF6a677tLx48fbshUAANCOdOiAtXbtWt15550qLS1VcXGxTp48qaysLB09etQ/Zvr06Xr11Ve1bNkyvfPOO6qvr9fo0aN16tQpSdKpU6d044036ujRo3rnnXe0bNkyvfLKK5oxY0aw2gIAAA4XGuwCWtPKlSsDHj/11FOKj4/Xpk2bdN1116mmpkZPPPGEnnnmGY0YMUKS9Oyzzyo5OVmrV6/WyJEjtWrVKm3fvl0VFRVKSkqSJC1YsEB5eXn6zW9%2Bo5iYmDbvCwAAOFuHPoP1dTU1NZKk2NhYSdKmTZt04sQJZWVl%2BcckJSUpPT1dJSUlkqQNGzYoPT3dH64kaeTIkfL5fNq0aVOjr%2BPz%2BVRbWxuwAACAi8dFE7CMMcrPz9e1116r9PR0SVJVVZXCw8PVuXPngLEJCQmqqqryj0lISAjY3rlzZ4WHh/vHfF1RUZE8Ho9/SU5OboWOAACAU100AWvq1Kn68MMP9cILL5xzrDFGLpfL//irPzc15qtmz56tmpoa/1JRUXH%2BhQMAgHbnoghY06ZN04oVK7RmzRp1797dv97r9er48eM6cuRIwPjq6mr/WSuv19vgTNWRI0d04sSJBme2znC73YqJiQlYAADAxaNDByxjjKZOnarly5frrbfeUmpqasD2fv36KSwsTMXFxf51lZWV2rZtmwYPHixJysjI0LZt21RZWekfs2rVKrndbvXr169tGgEAAO1Kh/4W4Z133qnnn39ef/rTnxQdHe0/E%2BXxeBQZGSmPx6PJkydrxowZ6tKli2JjYzVz5kz17t3b/63CrKwsXXnllcrNzdV9992nTz/9VDNnztSUKVM4MwUAABrVoQPWo48%2BKknKzMwMWP/UU08pLy9PkvTggw8qNDRUt9xyi44dO6brr79eS5YsUUhIiCQpJCREb7zxhu644w5dc801ioyMVE5Oju6///62bAUAALQjHTpgGWPOOSYiIkIPPfSQHnrooSbH9OjRQ//zP/9jszQAANCBdehrsAAAAIKBgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlHTpgrVu3TmPGjFFSUpJcLpdee%2B21gO15eXlyuVwBy6BBgwLG%2BHw%2BTZs2TXFxcYqKitLYsWN14MCBtmwDAAC0M44NWM8%2B%2B6y%2B%2BOKLC5rj6NGj6tOnjxYtWtTkmFGjRqmystK//PnPfw7YPn36dL366qtatmyZ3nnnHdXX12v06NE6derUBdUGAAA6rtBgF9CU/Px8TZ06VePHj9fkyZP13e9%2Bt8VzZGdnKzs7%2B6xj3G63vF5vo9tqamr0xBNP6JlnntGIESMkfRn8kpOTtXr1ao0cObLFNQEAgI7PsWewPv74Yz355JOqrKzUtddeq6uuukoLFizQoUOHrL7O22%2B/rfj4eKWlpWnKlCmqrq72b9u0aZNOnDihrKws/7qkpCSlp6erpKTEah0AAKDjcGzACg0N1bhx47RixQrt379fkyZN0pNPPqnu3btr3LhxeuONN2SMuaDXyM7O1nPPPae33npLCxYs0Hvvvafhw4fL5/NJkqqqqhQeHq7OnTsHPC8hIUFVVVVNzuvz%2BVRbWxuwAACAi4djA9ZXeb1eXX/99crMzJTL5dL777%2BvnJwc9erVS%2BvXrz/vecePH68bb7xR6enpGjNmjP73f/9Xf//73/XGG2%2Bc9XnGGLlcria3FxUVyePx%2BJfk5OTzrhEAALQ/jg5Yhw8f1sKFC9WnTx9dc801qq6u1muvvaZ9%2B/bpH//4h0aPHq1/%2BZd/sfZ6iYmJSklJ0c6dOyV9GeyOHz%2BuI0eOBIyrrq5WQkJCk/PMnj1bNTU1/qWiosJajQAAwPkcG7B%2B8IMfqFu3bvrDH/6g3NxcVVRU6OWXX9aoUaPkcrn0T//0T7r77ru1b98%2Ba6/5ySefqKKiQomJiZKkfv36KSwsTMXFxf4xlZWV2rZtmwYPHtzkPG63WzExMQELAAC4eDj2W4QxMTFavXq1hgwZ0uSYxMRE/9mmxtTX1%2Bujjz7yP96zZ4%2B2bNmi2NhYxcbGqqCgQDfffLMSExO1d%2B9e/fznP1dcXJx%2B8IMfSJI8Ho8mT56sGTNmqEuXLoqNjdXMmTPVu3dv/7cKAQAAvs6xAWvp0qXnHONyudSzZ88mt7///vsaNmyY/3F%2Bfr4kadKkSXr00Ue1detWPf300/rss8%2BUmJioYcOG6cUXX1R0dLT/OQ8%2B%2BKBCQ0N1yy236NixY7r%2B%2Buu1ZMkShYSEXEB3AACgI3NswPrZz36mnj17aurUqQHrH374Ye3evVsLFiw45xyZmZln/abhX/7yl3POERERoYceekgPPfTQuYsGAACQg6/Bevnllxv82RpJysjI0IsvvhiEigAAAJrHsQHr8OHDDe4/JX15bdbhw4eDUBEAAEDzODZg9ezZs9GP8P7yl78oNTU1CBUBAAA0j2OvwZo%2BfbqmT5%2BuTz75RMOHD5ckvfnmm5o/f77uv//%2BIFcHAADQNMcGrClTpuiLL75QYWGh5s6dK0nq3r27fv/73%2Btf//Vfg1wdAABA0xwbsCRp2rRpmjZtmiorKxUZGalOnToFuyQAAIBzcnTAOuPMndUBAADaA8de5H7o0CHdeuut6tGjhyIiIhQeHh6wAAAAOJVjz2Dl5eVp165d%2Bo//%2BA8lJibK5XIFuyQAAIBmcWzAWrdundatW6err7462KUAAAC0iGM/IuzevTtnrQAAQLvk2ID14IMPavbs2Tpw4ECwSwEAAGgRx35EmJubq7q6OqWkpCgmJkZhYWEB26urq4NUGQAAwNk5NmDNmzcv2CUAAACcF8cGrMmTJwe7BAAAgPPi2GuwJGnv3r0qKChQbm6u/yPBVatWqby8PMiVAQAANM2xAWv9%2BvW66qqrtHbtWr300kuqr6%2BXJH3wwQf65S9/GeTqAAAAmubYgPWf//mfKigo0Jo1awLu3D58%2BHCVlpYGsTIAAICzc2zA%2BvDDD/XDH/6wwfr4%2BHgdOnQoCBUBAAA0j2MDVqdOnVRVVdVg/ZYtW9StW7cgVAQAANA8jg1YEyZM0KxZs3To0CH/Hd3fffddzZw5UxMnTgxydQAAAE1zbMAqLCyU1%2BtVYmKi6uvrdeWVV2rw4MEaMGCAfvGLXwS7PAAAgCY59j5Y4eHhevHFF/X3v/9dH3zwgU6fPq2%2Bffvq8ssvD3ZpAAAAZ%2BXYgHVGWlqa0tLSgl0GAABAszk2YN12221n3f7444%2B3USUAAAAt49iAVVlZGfD4xIkTKisrU11dna677rogVQUAAHBujg1Yr7/%2BeoN1J0%2Be1L//%2B7/riiuuCEJFAAAAzePYbxE2JjQ0VDNnztR9990X7FIAAACa1K4CliTt3r1bJ06cCHYZAAAATXLsR4R33313wGNjjCorK7VixQr9%2BMc/DlJVAAAA5%2BbYgLVhw4aAx5dccom6du2qefPmacqUKUGqCgAA4NwcG7DWr18f7BIAAADOS7u7BgsAAMDpHHsGa8CAAf4/8nwuGzdubOVqAAAAms%2BxAWvYsGF67LHHlJaWpoyMDElSaWmpduzYoZ/85Cdyu91BrhAAAKBxjg1Yn332me68804VFhYGrJ8zZ44OHjyoP/7xj0GqDAAA4Owcew3WSy%2B9pFtvvbXB%2Bry8PL388stBqAgAAKB5HBuw3G63SkpKGqwvKSnh40EAAOBojv2I8K677tLtt9%2BuzZs3a9CgQZK%2BvAZr8eLF%2BvnPfx7k6gAAAJrm2IA1Z84cpaam6ne/%2B52efPJJSdIVV1yhxYsXKycnJ8jVAQAANM2xAUuScnJyCFMAAKDdcew1WJJUW1urJUuW6Je//KWOHDkiSfrb3/6mysrKIFcGAADQNMeewdq2bZtGjBihSy%2B9VBUVFcrLy1Pnzp310ksv6cCBA1q6dGmwSwQAAGiUY89g/exnP1NOTo527dqliIgI//obb7xR69atC2JlAAAAZ%2BfYM1jvvfeeHn300QZ/Lqdbt258RAgAABzNsWewwsPDVV9f32D9zp07FRcXF4SKAAAAmsexAWvs2LH69a9/rZMnT0qSXC6X/vGPf2jWrFkaN25ckKsDAABommMD1oIFC/Txxx/L6/Xq2LFjGj58uL75zW8qIiKiwd8nBAAAcBLHXoPl8XhUUlKi4uJiffDBBzp9%2BrT69u2rkSNHNrguCwAAwEkcGbBOnDih733ve3rkkUeUlZWlrKysYJcEAADQbI78iDAsLEybN2/mTBUAAGiXHBmwJGnixIl66qmngl0GAABAiznyI8IzFi1apNWrV6t///6KiooK2DZ//vwgVQUAAHB2jj2DtWnTJn37299WeHi4PvzwQ23YsMG/lJaWNmuOdevWacyYMUpKSpLL5dJrr70WsN0Yo4KCAiUlJSkyMlKZmZkqKysLGHPkyBHl5ubK4/HI4/EoNzdXn332mbU%2BAQBAx%2BO4M1i7d%2B9Wamqq1q9ff8FzHT16VH369NGtt96qm2%2B%2BucH2%2BfPn64EHHtCSJUuUlpame%2B%2B9VzfccIN27Nih6OhoSVJOTo4OHDiglStXSpJuu%2B025ebm6vXXX7/g%2BgAAQMfkuIDVq1cvVVZWKj4%2BXpI0fvx4/f73v1dCQkKL58rOzlZ2dnaj24wxWrhwoebMmeO/cenSpUuVkJCg559/Xj/5yU9UXl6ulStXqrS0VAMHDpQkLV68WBkZGdqxY4cuu%2Byy8%2BwSAAB0ZI77iNAYE/D4z3/%2Bs44ePWr9dfbs2aOqqqqAW0C43W4NHTpUJSUlkqQNGzbI4/H4w5UkDRo0yH%2BPrqb4fD7V1tYGLAAA4OLhuIDVVqqqqiSpwZmxhIQE/7aqqir/mbSvio%2BP949pTFFRkf%2BaLY/Ho%2BTkZIuVAwAAp3NcwHK5XA3uf9Wa98P6%2BtzGmIB1jb3218d83ezZs1VTU%2BNfKioq7BUMAAAcz3HXYBljlJeXJ7fbLUn64osvdPvttze4TcPy5csv6HW8Xq%2BkL89SJSYm%2BtdXV1f7z2p5vV4dPHiwwXMPHTp01mvC3G63v34AAHDxcdwZrEmTJik%2BPt7/8drEiROVlJQU8JGbx%2BO54NdJTU2V1%2BtVcXGxf93x48e1du1aDR48WJKUkZGhmpoabdy40T/m3XffVU1NjX8MAADA1znuDJbNu7fX19fro48%2B8j/es2ePtmzZotjYWPXo0UPTp09XYWGhevXqpV69eqmwsFCXXnqpcnJyJElXXHGFRo0apSlTpuixxx6T9OVtGkaPHs03CAEAQJMcF7Bsev/99zVs2DD/4/z8fElfniVbsmSJ7r77bh07dkx33HGHjhw5ooEDB2rVqlX%2Be2BJ0nPPPae77rrL/23DsWPHatGiRW3bCAAAaFc6dMDKzMxscNuHr3K5XCooKFBBQUGTY2JjY/Xss8%2B2QnUAAKCjctw1WAAAAO0dAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLLvqAVVBQIJfLFbB4vV7/dmOMCgoKlJSUpMjISGVmZqqsrCyIFQMAAKe76AOWJF111VWqrKz0L1u3bvVvmz9/vh544AEtWrRI7733nrxer2644QbV1dUFsWIAAOBkBCxJoaGh8nq9/qVr166Svjx7tXDhQs2ZM0fjxo1Tenq6li5dqs8//1zPP/98kKsGAABORcCStHPnTiUlJSk1NVUTJkzQ7t27JUl79uxRVVWVsrKy/GPdbreGDh2qkpKSYJULAAAcLjTYBQTbwIED9fTTTystLU0HDx7Uvffeq8GDB6usrExVVVWSpISEhIDnJCQkaN%2B%2BfU3O6fP55PP5/I9ra2tbp3gAAOBIF33Ays7O9v/cu3dvZWRkqGfPnlq6dKkGDRokSXK5XAHPMcY0WPdVRUVFuueee1qnYAAA4Hh8RPg1UVFR6t27t3bu3On/NuGZM1lnVFdXNzir9VWzZ89WTU2Nf6moqGjVmgEAgLNc9Gewvs7n86m8vFxDhgxRamqqvF6viouLdfXVV0uSjh8/rrVr1%2Bq3v/1tk3O43W653e42qbf/nJVt8jo2vP%2BbUcEuAQCANnHRB6yZM2dqzJgx6tGjh6qrq3XvvfeqtrZWkyZNksvl0vTp01VYWKhevXqpV69eKiws1KWXXqqcnJxglw4AABzqog9YBw4c0I9%2B9CMdPnxYXbt21aBBg1RaWqqUlBRJ0t13361jx47pjjvu0JEjRzRw4ECtWrVK0dHRQa4cAAA41UUfsJYtW3bW7S6XSwUFBSooKGibggAAQLvHRe4AAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQELAADAMgIWAACAZQQsAAAAywhYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAsI2ABAABYRsACAACwjIAFAABgGQGrmR555BGlpqYqIiJC/fr10/r164NdEgAAcCgCVjO8%2BOKLmj59uubMmaPNmzdryJAhys7O1v79%2B4NdGgAAcCACVjM88MADmjx5sv7t3/5NV1xxhRYuXKjk5GQ9%2BuijwS4NAAA4UGiwC3C648ePa9OmTZo1a1bA%2BqysLJWUlDT6HJ/PJ5/P539cU1MjSaqtrbVe3ynfUetztpbW6L81taffrdT%2Bfr9oPe3pvcv7tnVd7O%2BFM3MaY6zPfS4ErHM4fPiwTp06pYSEhID1CQkJqqqqavQ5RUVFuueeexqsT05ObpUa2wvPgmBX0LHx%2B0V7xPsWZ7Tme6Gurk4ej6f1XqARBKxmcrlcAY%2BNMQ3WnTF79mzl5%2Bf7H58%2BfVqffvqpunTp0uRzzkdtba2Sk5NVUVGhmJgYa/MGW0ftS%2Bq4vXXUviR6a486al9Sx%2B2ttfoyxqiurk5JSUnW5mwuAtY5xMXFKSQkpMHZqurq6gZntc5wu91yu90B6zp16tRqNcbExHSoA%2B2MjtqX1HF766h9SfTWHnXUvqSO21tr9NXWZ67O4CL3cwgPD1e/fv1UXFwcsL64uFiDBw8OUlUAAMDJOIPVDPn5%2BcrNzVX//v2VkZGhxx9/XPv379ftt98e7NIAAIADhRQUFBQEuwinS09PV5cuXVRYWKj7779fx44d0zPPPKM%2BffoEuzSFhIQoMzNToaEdKyt31L6kjttbR%2B1Lorf2qKP2JXXc3jpaXy4TjO8uAgAAdGBcgwUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgO8sgjjyg1NVURERHq16%2Bf1q9ff9bxr7zyiq688kq53W5deeWVevXVVwO2G2NUUFCgpKQkRUZGKjMzU2VlZa3ZQpNs95aXlyeXyxWwDBo0qDVbaFRL%2BiorK9PNN9%2Bsb3zjG3K5XFq4cOEFz9mabPdWUFDQYJ95vd7WbKFJLelt8eLFGjJkiDp37qzOnTtrxIgR2rhxY8AYpxxrtvtyynEmtay35cuXq3///urUqZOioqL0ne98R88880zAmPa4z5rTV3vdZ1%2B1bNkyuVwu3XTTTQHrnbLPms3AEZYtW2bCwsLM4sWLzfbt281Pf/pTExUVZfbt29fo%2BJKSEhMSEmIKCwtNeXm5KSwsNKGhoaa0tNQ/Zt68eSY6Otq88sorZuvWrWb8%2BPEmMTHR1NbWtlVbxpjW6W3SpElm1KhRprKy0r988sknbdWSMablfW3cuNHMnDnTvPDCC8br9ZoHH3zwgudsLa3R29y5c81VV10VsM%2Bqq6tbu5UGWtpbTk6Oefjhh83mzZtNeXm5ufXWW43H4zEHDhzwj3HCsdYafTnhODOm5b2tWbPGLF%2Ba6ZPmAAAG00lEQVS%2B3Gzfvt189NFHZuHChSYkJMSsXLnSP6Y97rPm9NVe99kZe/fuNd26dTNDhgwx3//%2B9wO2OWGftQQByyG%2B%2B93vmttvvz1g3eWXX25mzZrV6PhbbrnFjBo1KmDdyJEjzYQJE4wxxpw%2Bfdp4vV4zb948//YvvvjCeDwe84c//MFy9WdnuzdjvvyfyNcPvrbW0r6%2BKiUlpdEQciFz2tQavc2dO9f06dPHWo3n60J/xydPnjTR0dFm6dKlxhjnHGu2%2BzLGGceZMXaOi6uvvtr813/9lzGm4%2BwzYwL7MqZ977OTJ0%2Baa665xvzxj39s0IdT9llL8BGhAxw/flybNm1SVlZWwPqsrCyVlJQ0%2BpwNGzY0GD9y5Ej/%2BD179qiqqipgjNvt1tChQ5ucszW0Rm9nvP3224qPj1daWpqmTJmi6upqu8Wfxfn0FYw5nVbHzp07lZSUpNTUVE2YMEG7d%2B%2B%2BoPlaykZvn3/%2BuU6cOKHY2FhJzjjWWqOvM4J5nEkX3psxRm%2B%2B%2BaZ27Nih6667TlLH2GeN9XVGe91nv/rVr9S1a1dNnjy5wTYn7LOW6hi3S23nDh8%2BrFOnTjX449EJCQkN/sj0GVVVVWcdf%2Ba/jY3Zt2%2BfrdLPqTV6k6Ts7Gz98z//s1JSUrRnzx794he/0PDhw7Vp06YGf2i7NZxPX8GY00l1DBw4UE8//bTS0tJ08OBB3XvvvRo8eLDKysrUpUuXCy27WWz0NmvWLHXr1k0jRoyQ5IxjrTX6koJ/nEnn31tNTY26desmn8%2BnkJAQPfLII7rhhhskte99dra%2BpPa7z/7617/qiSee0JYtWxrd7oR91lIELAdxuVwBj40xDda1dHxL52wttnsbP368/%2Bf09HT1799fKSkpeuONNzRu3DhLVZ9ba/x%2B2%2Bs%2BO5fs7Gz/z71791ZGRoZ69uyppUuXKj8//7znPR/n29v8%2BfP1wgsv6O2331ZERISVOW2y3ZdTjjOp5b1FR0dry5Ytqq%2Bv15tvvqn8/Hx985vfVGZm5nnP2Rps99Ue91ldXZ0mTpyoxYsXKy4uzsqcTkDAcoC4uDiFhIQ0SPbV1dUN0voZXq/3rOPPfDurqqpKiYmJzZqzNbRGb41JTExUSkqKdu7ceeFFN8P59BWMOZ1cR1RUlHr37t1m%2B0y6sN7uv/9%2BFRYWavXq1fr2t7/tX%2B%2BEY601%2BmpMWx9n0vn3dskll%2Bhb3/qWJOk73/mOysvLVVRUpMzMzHa9z87WV2Pawz7btWuX9u7dqzFjxvjXnT59WpIUGhqqHTt2OGKftRTXYDlAeHi4%2BvXrp%2BLi4oD1xcXFGjx4cKPPycjIaDB%2B1apV/vGpqanyer0BY44fP661a9c2OWdraI3eGvPJJ5%2BooqIi4MBrTefTVzDmdHIdPp9P5eXlbbbPpPPv7b777tOvf/1rrVy5Uv379w/Y5oRjrTX6akxbH2eSvfejMUY%2Bn09S%2B95nX/fVvhrTHvbZ5Zdfrq1bt2rLli3%2BZezYsRo2bJi2bNmi5ORkR%2ByzFmvba%2BrRlDNfaX3iiSfM9u3bzfTp001UVJTZu3evMcaY3NzcgG9f/PWvfzUhISFm3rx5pry83MybN6/R2zR4PB6zfPlys3XrVvOjH/0oqLdpsNVbXV2dmTFjhikpKTF79uwxa9asMRkZGaZbt25B%2BYp1c/vy%2BXxm8%2BbNZvPmzSYxMdHMnDnTbN682ezcubPZc7bn3mbMmGHefvtts3v3blNaWmpGjx5toqOjHd/bb3/7WxMeHm7%2B%2B7//O%2BCr73V1df4xTjjWbPfllOPsfHorLCw0q1atMrt27TLl5eVmwYIFJjQ01CxevNg/pj3us3P11Z732dc19m1IJ%2ByzliBgOcjDDz9sUlJSTHh4uOnbt69Zu3atf9vQoUPNpEmTAsa//PLL5rLLLjNhYWHm8ssvN6%2B88krA9tOnT5u5c%2Bcar9dr3G63ue6668zWrVvbopUGbPb2%2Beefm6ysLNO1a1cTFhZmevToYSZNmmT279/fVu34taSvPXv2GEkNlqFDhzZ7zrZku7cz96wJCwszSUlJZty4caasrKwNO/r/WtJbSkpKo73NnTvXP8Ypx5rNvpx0nBnTst7mzJljvvWtb5mIiAjTuXNnk5GRYZYtWxYwX3vcZ%2Bfqqz3vs69rLGA5ZZ81l8sYY9r2nBkAAEDHxjVYAAAAlhGwAAAALCNgAQAAWEbAAgAAsIyABQAAYBkBCwAAwDICFgAAgGUELAAAAMsIWAAAAJYRsAAAACwjYAEAAFhGwAIAALCMgAUAAGAZAQsAAMAyAhYAAIBlBCwAAADLCFgAAACWEbAAAAAs%2B38XMze9teupZgAAAABJRU5ErkJggg%3D%3D"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common1601895208561110408">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.4</td>
        <td class="number">240</td>
        <td class="number">31.2%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.1</td>
        <td class="number">240</td>
        <td class="number">31.2%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.25</td>
        <td class="number">240</td>
        <td class="number">31.2%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">48</td>
        <td class="number">6.2%</td>
        <td>
            <div class="bar" style="width:20%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme1601895208561110408">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">48</td>
        <td class="number">6.2%</td>
        <td>
            <div class="bar" style="width:20%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.1</td>
        <td class="number">240</td>
        <td class="number">31.2%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.25</td>
        <td class="number">240</td>
        <td class="number">31.2%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.4</td>
        <td class="number">240</td>
        <td class="number">31.2%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0.0</td>
        <td class="number">48</td>
        <td class="number">6.2%</td>
        <td>
            <div class="bar" style="width:20%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.1</td>
        <td class="number">240</td>
        <td class="number">31.2%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.25</td>
        <td class="number">240</td>
        <td class="number">31.2%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0.4</td>
        <td class="number">240</td>
        <td class="number">31.2%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_X8">X8<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>6</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>0.8%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (n)</th>
                    <td>0</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>

        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>2.8125</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>0</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>5</td>
                </tr>
                <tr class="alert">
                    <th>Zeros (%)</th>
                    <td>6.2%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram2085472178238802677">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAAbFJREFUeJzt3TGO01AUhtHriNZZQGTvgkWwJ2r2xCLYha0swBEFBXkUEBpG/zApnsfSOaWj%2BD5H75NlKZKH1lqrztZ1rXmee4/l4JZlqWmaus780HXaH%2BM4VtXvCz6fz3ssgQPZtq3mef67b3raJZBhGKqq6nw%2B7xbIx89f3/ydb18%2BvbsZz8zpMeNZaW2PfdPTqftEOBCBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEu7z%2BIOn1ygD4H%2B4gEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCDY5e/urbWqqtq27Z/Pfv74/ubzvXSe1/SY816vpdfv9YyX1vY49tg3PQ1th6nrutY8z73HcnDLstQ0TV1n7hLI/X6v6/Va4zjWMAy9x3MwrbW63W51uVzqdOr7VLBLIHAUHtIhEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCH4BV99mnnUxlIoAAAAASUVORK5CYII%3D">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives2085472178238802677,#minihistogram2085472178238802677"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives2085472178238802677">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles2085472178238802677"
                                                  aria-controls="quantiles2085472178238802677" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram2085472178238802677" aria-controls="histogram2085472178238802677"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common2085472178238802677" aria-controls="common2085472178238802677"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme2085472178238802677" aria-controls="extreme2085472178238802677"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>

    </ul>

    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles2085472178238802677">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>0</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>1.75</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>3</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>4</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>5</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>5</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>5</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>2.25</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>1.551</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.55145</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>-1.1487</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>2.8125</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>1.3359</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>-0.088689</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>2160</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>2.4055</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>6.1 KiB</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram2085472178238802677">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAIABJREFUeJzt3X10VPWB//HPSMgEaDKYYCakBE3dYJEnERASn4JgNMtDXc6utcEpWIqw8tAYEUlZ3dTVRFExShYKbAUqULBboeyujYTVJnhCgARSkcNBVCpBEgLdMCEI4Wl%2Bf7jMr2MIxPU7uXcm79c59xzu996595MrHj7nO9%2BZOHw%2Bn08AAAAw5hqrAwAAAIQbChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGBZhdYCO4OLFizpy5Iiio6PlcDisjgMAQIfg8/l08uRJJSYm6ppr2ndOiYLVDo4cOaKkpCSrYwAA0CHV1NSoV69e7XpPClY7iI6OlvTVf%2BCYmBiL0wAA0DE0NjYqKSnJ/%2B9we6JgtYNLbwvGxMRQsAAAaGdWLM9hkTsAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADOOXPQOtGDq/2OoI30jl8/dbHaHNeLbBFUrPl2eLS0Lt78LVMIMFAABgGAULAADAMAoWAACAYWFdsMrKyjRu3DglJibK4XBo48aNrZ47bdo0ORwOFRYWBow3NDTI4/HI5XLJ5XLJ4/HoxIkTwY4OAABCWFgXrFOnTmnQoEEqKiq64nkbN27U9u3blZiY2OJYVlaWqqurVVxcrOLiYlVXV8vj8QQrMgAACANh/SnCzMxMZWZmXvGcL774QjNnztS7776rMWPGBBzbt2%2BfiouLVVFRoeHDh0uSli9frtTUVO3fv1833XRT0LIDAIDQFdYzWFdz8eJFeTwePfnkk%2BrXr1%2BL49u2bZPL5fKXK0kaMWKEXC6XysvL2zMqAAAIIWE9g3U1L774oiIiIjR79uzLHq%2Brq1N8fHyL8fj4eNXV1bV63ebmZjU3N/v3Gxsbv31YAAAQMjrsDFZVVZVee%2B01rVy5Ug6Ho9XzLnfM5/Nd8TUFBQX%2BRfEul0tJSUlGMgMAgNDQYQvW1q1bVV9fr969eysiIkIRERH6/PPP9cQTT%2BiGG26QJCUkJOjo0aMtXnvs2DG53e5Wr52bmyuv1%2BvfampqgvVjAAAAG%2BqwbxF6PB6NHj06YOy%2B%2B%2B6Tx%2BPRI488IklKTU2V1%2BvVjh07dNttt0mStm/fLq/Xq7S0tFav7XQ65XQ6gxceAADYWlgXrKamJn3yySf%2B/YMHD6q6ulqxsbHq3bu34uLiAs7v3LmzEhIS/J8O7Nu3r%2B6//35NnTpVS5culSQ9%2BuijGjt2LJ8gBAAArQrrtwgrKys1ePBgDR48WJKUk5OjwYMH65lnnmnzNdasWaMBAwYoIyNDGRkZGjhwoN58881gRQYAAGEgrGew0tPT5fP52nz%2Bn//85xZjsbGxWr16tcFUAAAg3IX1DBYAAIAVKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYWFdsMrKyjRu3DglJibK4XBo48aN/mPnzp3TU089pQEDBqhbt25KTEzUj3/8Yx05ciTgGg0NDfJ4PHK5XHK5XPJ4PDpx4kR7/ygAACCEhHXBOnXqlAYNGqSioqIWx7788kvt2rVLTz/9tHbt2qW3335bH3/8scaPHx9wXlZWlqqrq1VcXKzi4mJVV1fL4/G0148AAABCUITVAYIpMzNTmZmZlz3mcrlUUlISMLZo0SLddtttOnTokHr37q19%2B/apuLhYFRUVGj58uCRp%2BfLlSk1N1f79%2B3XTTTcF/WcAAAChJ6xnsL4pr9crh8Oh7t27S5K2bdsml8vlL1eSNGLECLlcLpWXl7d6nebmZjU2NgZsAACg46Bg/a8zZ85o3rx5ysrKUkxMjCSprq5O8fHxLc6Nj49XXV1dq9cqKCjwr9lyuVxKSkoKWm4AAGA/FCx9teD9oYce0sWLF7V48eKAYw6Ho8X5Pp/vsuOX5Obmyuv1%2BreamhrjmQEAgH2F9Rqstjh37pwefPBBHTx4UO%2B9955/9kqSEhISdPTo0RavOXbsmNxud6vXdDqdcjqdQckLAADsr0PPYF0qVwcOHNCWLVsUFxcXcDw1NVVer1c7duzwj23fvl1er1dpaWntHRcAAISIsJ7Bampq0ieffOLfP3jwoKqrqxUbG6vExET9/d//vXbt2qX//M//1IULF/zrqmJjYxUZGam%2Bffvq/vvv19SpU7V06VJJ0qOPPqqxY8fyCUIAANCqsC5YlZWVGjlypH8/JydHkjRp0iTl5eVp06ZNkqRbbrkl4HXvv/%2B%2B0tPTJUlr1qzR7NmzlZGRIUkaP378Zb9XCwAA4JKwLljp6eny%2BXytHr/SsUtiY2O1evVqk7EAAECY69BrsAAAAIKBggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMC%2BuCVVZWpnHjxikxMVEOh0MbN24MOO7z%2BZSXl6fExER16dJF6enp2rt3b8A5DQ0N8ng8crlccrlc8ng8OnHiRHv%2BGAAAIMSEdcE6deqUBg0apKKiosseX7BggRYuXKiioiLt3LlTCQkJuvfee3Xy5En/OVlZWaqurlZxcbGKi4tVXV0tj8fTXj8CAAAIQRFWBwimzMxMZWZmXvaYz%2BdTYWGh5s%2BfrwkTJkiSVq1aJbfbrbVr12ratGnat2%2BfiouLVVFRoeHDh0uSli9frtTUVO3fv1833XRTu/0sAAAgdIT1DNaVHDx4UHV1dcrIyPCPOZ1O3X333SovL5ckbdu2TS6Xy1%2BuJGnEiBFyuVz%2Bcy6nublZjY2NARsAAOg4OmzBqqurkyS53e6Acbfb7T9WV1en%2BPj4Fq%2BNj4/3n3M5BQUF/jVbLpdLSUlJBpMDAAC767AF6xKHwxGw7/P5Asa%2Bfvxy53xdbm6uvF6vf6upqTEXGAAA2F5Yr8G6koSEBElfzVL17NnTP15fX%2B%2Bf1UpISNDRo0dbvPbYsWMtZr7%2BmtPplNPpNJwYAACEig47g5WcnKyEhASVlJT4x86ePavS0lKlpaVJklJTU%2BX1erVjxw7/Odu3b5fX6/WfAwAA8HVhPYPV1NSkTz75xL9/8OBBVVdXKzY2Vr1791Z2drby8/OVkpKilJQU5efnq2vXrsrKypIk9e3bV/fff7%2BmTp2qpUuXSpIeffRRjR07lk8QAgCAVoV1waqsrNTIkSP9%2Bzk5OZKkSZMmaeXKlZo7d65Onz6txx57TA0NDRo%2BfLg2b96s6Oho/2vWrFmj2bNn%2Bz9tOH78%2BFa/VwsAAEAK84KVnp4un8/X6nGHw6G8vDzl5eW1ek5sbKxWr14dhHQAACBcddg1WAAAAMFCwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgmG0L1urVq3XmzBmrYwAAAHxjti1YOTk5SkhI0LRp07Rjxw6r4wAAALSZbQvWkSNH9MYbb6i2tlZ33HGH%2BvXrp1deeUXHjh2zOhoAAMAV2bZgRUREaMKECdq0aZMOHTqkSZMm6Y033lCvXr00YcIE/dd//Zd8Pp/VMQEAAFqwbcH6awkJCRo1apTS09PlcDhUWVmprKwspaSkaOvWrVbHAwAACGDrgnX8%2BHEVFhZq0KBBuv3221VfX6%2BNGzfq888/1xdffKGxY8fqxz/%2BsdUxAQAAAkRYHaA1f/d3f6d33nlHycnJ%2BulPf6pJkybpuuuu8x//zne%2Bo7lz5%2Br111%2B3MCUAAEBLti1YMTEx2rJli%2B68885Wz%2BnZs6cOHDjQjqkAAACuzrYFa9WqVVc9x%2BFw6MYbb2yHNAAAAG1n2zVYjz/%2BuIqKilqM/%2Bu//queeOIJCxIBAAC0jW0L1m9/%2B1uNGDGixXhqaqrWr19vQSIAAIC2sW3BOn78uK699toW4zExMTp%2B/LgFiQAAANrGtgXrxhtv1Lvvvtti/N1331VycrIFiQAAANrGtovcs7OzlZ2drb/85S%2B65557JEn//d//rQULFujll1%2B2OB0AAEDrbFuwpk6dqjNnzig/P1///M//LEnq1auXXn/9df3kJz%2BxOB0AAEDrbFuwJGnWrFmaNWuWamtr1aVLF3Xv3t3qSAAAAFdl64J1Sc%2BePa2OAAAA0Ga2XeR%2B7NgxPfLII%2Brdu7eioqIUGRkZsAEAANiVbWewJk%2BerE8//VRPPvmkevbsKYfDYXUkAACANrFtwSorK1NZWZkGDx5sdRQAAIBvxLZvEfbq1atdZq3Onz%2Bvf/qnf1JycrK6dOmi733ve3r22Wd18eJF/zk%2Bn095eXlKTExUly5dlJ6err179wY9GwAACE22LVivvvqqcnNzdfjw4aDe58UXX9Qvf/lLFRUVad%2B%2BfVqwYIFeeuklLVq0yH/OggULtHDhQhUVFWnnzp1KSEjQvffeq5MnTwY1GwAACE22fYvQ4/Ho5MmTuv766xUTE6POnTsHHK%2Bvrzdyn23btukHP/iBxowZI0m64YYb9Jvf/EaVlZWSvpq9Kiws1Pz58zVhwgRJ0qpVq%2BR2u7V27VpNmzbNSA4AABA%2BbFuwXnjhhXa5zx133KFf/vKX%2Bvjjj9WnTx/96U9/0gcffKDCwkJJ0sGDB1VXV6eMjAz/a5xOp%2B6%2B%2B26Vl5dTsAAAQAu2LVhTpkxpl/s89dRT8nq9%2Bv73v69OnTrpwoULev755/WjH/1IklRXVydJcrvdAa9zu936/PPPL3vN5uZmNTc3%2B/cbGxuDlB4AANiRbddgSdKf//xn5eXlyePx%2BN8S3Lx5s/bt22fsHuvXr9fq1au1du1a7dq1S6tWrdLLL7%2BsVatWBZz39QX3Pp%2Bv1UX4BQUFcrlc/i0pKclYXgAAYH%2B2LVhbt25Vv379VFpaqrfeektNTU2SpF27dumZZ54xdp8nn3xS8%2BbN00MPPaQBAwbI4/Ho8ccfV0FBgSQpISFB0v%2Bfybqkvr6%2BxazWJbm5ufJ6vf6tpqbGWF4AAGB/ti1YTz31lPLy8vT%2B%2B%2B8HfHP7Pffco4qKCmP3%2BfLLL3XNNYGPoVOnTv6vaUhOTlZCQoJKSkr8x8%2BePavS0lKlpaVd9ppOp1MxMTEBGwAA6Dhsuwbrww8/1Jo1a1qMx8fH69ixY8buM27cOD3//PPq3bu3%2BvXrp927d2vhwoX6yU9%2BIumrtwazs7OVn5%2BvlJQUpaSkKD8/X127dlVWVpaxHAAAIHzYtmB1795ddXV1Sk5ODhivrq7Wd7/7XWP3WbRokZ5%2B%2Bmk99thjqq%2BvV2JioqZNmxbwNuTcuXN1%2BvRpPfbYY2poaNDw4cO1efNmRUdHG8sBAADCh20L1kMPPaR58%2Bbp3//93/2Lybdv3645c%2Bbo4YcfNnaf6OhoFRYW%2Br%2BW4XIcDofy8vKUl5dn7L4AACB82XYNVn5%2BvhISEtSzZ081NTXp5ptvVlpamoYNG6ann37a6ngAAACtsu0MVmRkpNavX6%2BPP/5Yu3bt0sWLF3Xrrbfq%2B9//vtXRAAAArsi2BeuSPn36qE%2BfPlbHAAAAaDPbFqxHH330iseXLVvWTkkAAAC%2BGdsWrNra2oD9c%2BfOae/evTp58qTuuusui1IBAABcnW0L1n/8x3%2B0GDt//rz%2B8R//UX379rUgEQAAQNvY9lOElxMREaE5c%2BbopZdesjoKAABAq0KqYEnSZ599pnPnzlkdAwAAoFW2fYtw7ty5Afs%2Bn0%2B1tbXatGmTJk6caFEqAACAq7Ntwdq2bVvA/jXXXKPrrrtOL7zwgqZOnWpRKgAAgKuzbcHaunWr1REAAAD%2BT0JuDRYAAIDd2XYGa9iwYf5f8nw1O3bsCHIaAACAtrNtwRo5cqSWLl2qPn36KDU1VZJUUVGh/fv3a9q0aXI6nRYnBAAAuDzbFqwTJ05oxowZys/PDxifP3%2B%2Bjh49qn/7t3%2BzKBkAAMCV2XYN1ltvvaVHHnmkxfjkyZP129/%2B1oJEAAAAbWPbguV0OlVeXt5ivLy8nLcHAQCArdn2LcLZs2dr%2BvTp2r17t0aMGCHpqzVYy5cv189//nOL0wEAALTOtgVr/vz5Sk5O1muvvaY33nhDktS3b18tX75cWVlZFqcDAABonW0LliRlZWVRpgAAQMix7RosSWpsbNTKlSv1zDPPqKGhQZL0pz/9SbW1tRYnAwAAaJ1tZ7A%2B%2BugjjR49Wl27dlVNTY0mT56sa6%2B9Vm%2B99ZYOHz6sVatWWR0RAADgsmw7g/X4448rKytLn376qaKiovzjY8aMUVlZmYXJAAAArsy2M1g7d%2B7UkiVLWvy6nO9%2B97u8RQgAAGzNtjNYkZGRampqajF%2B4MAB9ejRw4JEAAAAbWPbgjV%2B/Hj9y7/8i86fPy9Jcjgc%2BuKLLzRv3jxNmDDB4nQAAACts23BeuWVV3TkyBElJCTo9OnTuueee/S9731PUVFRLX4/IQAAgJ3Ydg2Wy%2BVSeXm5SkpKtGvXLl28eFG33nqr7rvvvhbrsgAAAOzElgXr3Llz%2Btu//VstXrxYGRkZysjIsDoSAABAm9nyLcLOnTtr9%2B7dzFQBAICQZMuCJUkPP/ywVqxYYXUMAACAb8yWbxFeUlRUpC1btmjo0KHq1q1bwLEFCxZYlAoAAODKbDuDVVVVpYEDByoyMlIffvihtm3b5t8qKiqM3uuLL77Qww8/rLi4OHXt2lW33HKLqqqq/Md9Pp/y8vKUmJioLl26KD09XXv37jWaAQAAhA/bzWB99tlnSk5O1tatW9vlfg0NDbr99ts1cuRI/eEPf1B8fLw%2B/fRTde/e3X/OggULtHDhQq1cuVJ9%2BvTRc889p3vvvVf79%2B9XdHR0u%2BQEAAChw3YzWCkpKTp27Jh//4c//KGOHj0atPu9%2BOKLSkpK0ooVK3Tbbbfphhtu0KhRo3TjjTdK%2Bmr2qrCwUPPnz9eECRPUv39/rVq1Sl9%2B%2BaXWrl0btFwAACB02a5g%2BXy%2BgP133nlHp06dCtr9Nm3apKFDh%2Bof/uEfFB8fr8GDB2v58uX%2B4wcPHlRdXV3AV0U4nU7dfffdKi8vv%2Bw1m5ub1djYGLABAICOw3YFq7199tlnWrJkiVJSUvTuu%2B9q%2BvTpmj17tn79619Lkurq6iRJbrc74HVut9t/7OsKCgrkcrn8W1JSUnB/CAAAYCu2K1gOh6PF918F8/uwLn1DfH5%2BvgYPHqxp06Zp6tSpWrJkyRUz%2BHy%2BVnPl5ubK6/X6t5qamqDlBwAA9mO7Re4%2Bn0%2BTJ0%2BW0%2BmUJJ05c0bTp09v8TUNb7/9tpH79ezZUzfffHPAWN%2B%2BffW73/1OkpSQkCDpq5msnj17%2Bs%2Bpr69vMat1idPp9OcHAAAdj%2B0K1qRJkwL2H3744aDe7/bbb9f%2B/fsDxj7%2B%2BGNdf/31kqTk5GQlJCSopKREgwcPliSdPXtWpaWlevHFF4OaDQAAhCbbFaz2/vb2xx9/XGlpacrPz9eDDz6oHTt2aNmyZVq2bJmkr94azM7OVn5%2BvlJSUpSSkqL8/Hx17dpVWVlZ7ZoVAACEBtsVrPY2bNgwbdiwQbm5uXr22WeVnJyswsJCTZw40X/O3Llzdfr0aT322GNqaGjQ8OHDtXnzZr4DCwAAXFaHL1iSNHbsWI0dO7bV4w6HQ3l5ecrLy2u/UAAAIGTZ7lOEAAAAoY6CBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgWITVAfDtDJ1fbHWENqt8/n6rIwAA0C6YwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRSsv1JQUCCHw6Hs7Gz/WHNzs2bNmqUePXqoW7duGj9%2BvA4fPmxhSgAAYHcUrP%2B1c%2BdOLVu2TAMHDgwYz87O1oYNG7Ru3Tp98MEHampq0tixY3XhwgWLkgIAALujYElqamrSxIkTtXz5cl177bX%2Bca/Xq1/96ld65ZVXNHr0aA0ePFirV6/Wnj17tGXLFgsTAwAAO6NgSZoxY4bGjBmj0aNHB4xXVVXp3LlzysjI8I8lJiaqf//%2BKi8vb/V6zc3NamxsDNgAAEDHEWF1AKutW7dOVVVVqqysbHGsrq5OkZGRAbNakuR2u1VXV9fqNQsKCvSLX/zCeFYAABAaOvQMVk1NjX72s59pzZo1ioqKavPrfD6fHA5Hq8dzc3Pl9Xr9W01NjYm4AAAgRHToglVVVaX6%2BnoNGTJEERERioiIUGlpqV5//XVFRETI7Xbr7NmzamhoCHhdfX293G53q9d1Op2KiYkJ2AAAQMfRoQvWqFGjtGfPHlVXV/u3oUOHauLEif4/d%2B7cWSUlJf7X1NbW6qOPPlJaWpqFyQEAgJ116DVY0dHR6t%2B/f8BYt27dFBcX5x%2BfMmWKnnjiCcXFxSk2NlZz5szRgAEDWiyIBwAAuKRDF6y2ePXVVxUREaEHH3xQp0%2Bf1qhRo7Ry5Up16tTJ6mgAAMCmKFhf88c//jFgPyoqSosWLdKiRYusCQQAAEJOh16DBQAAEAwULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGBYhy9YBQUFGjZsmKKjoxUfH68HHnhA%2B/fvDzinublZs2bNUo8ePdStWzeNHz9ehw8ftigxAACwuw5fsEpLSzVjxgxVVFSopKRE58%2BfV0ZGhk6dOuU/Jzs7Wxs2bNC6dev0wQcfqKmpSWPHjtWFCxcsTA4AAOwqwuoAVisuLg7YX7FiheLj41VVVaW77rpLXq9Xv/rVr/Tmm29q9OjRkqTVq1crKSlJW7Zs0X333WdFbAAAYGMdfgbr67xeryQpNjZWklRVVaVz584pIyPDf05iYqL69%2B%2Bv8vJySzICAAB76/AzWH/N5/MpJydHd9xxh/r37y9JqqurU2RkpK699tqAc91ut%2Brq6i57nebmZjU3N/v3GxsbgxcaAADYDjNYf2XmzJn68MMP9Zvf/Oaq5/p8PjkcjsseKygokMvl8m9JSUmmowIAABujYP2vWbNmadOmTXr//ffVq1cv/3hCQoLOnj2rhoaGgPPr6%2Bvldrsve63c3Fx5vV7/VlNTE9TsAADAXjp8wfL5fJo5c6befvttvffee0pOTg44PmTIEHXu3FklJSX%2BsdraWn300UdKS0u77DWdTqdiYmICNgAA0HF0%2BDVYM2bM0Nq1a/X73/9e0dHR/nVVLpdLXbp0kcvl0pQpU/TEE08oLi5OsbGxmjNnjgYMGOD/VCEAAMBf6/AFa8mSJZKk9PT0gPEVK1Zo8uTJkqRXX31VERERevDBB3X69GmNGjVKK1euVKdOndo5LQAACAUdvmD5fL6rnhMVFaVFixZp0aJF7ZAIAACEug6/BgsAAMA0ChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAWrjRYvXqzk5GRFRUVpyJAh2rp1q9WRAACATVGw2mD9%2BvXKzs7W/PnztXv3bt15553KzMzUoUOHrI4GAABsiILVBgsXLtSUKVP005/%2BVH379lVhYaGSkpK0ZMkSq6MBAAAbirA6gN2dPXtWVVVVmjdvXsB4RkaGysvLL/ua5uZmNTc3%2B/e9Xq8kqbGx0Xi%2BC82njF8zWILx8wdTKD1bKbSeL882uELp%2BfJscUkw/i5cuqbP5zN%2B7auhYF3F8ePHdeHCBbnd7oBxt9uturq6y76moKBAv/jFL1qMJyUlBSVjqHC9YnWC8MbzDR6ebfDwbHFJMP8unDx5Ui6XK3g3uAwKVhs5HI6AfZ/P12LsktzcXOXk5Pj3L168qP/5n/9RXFxcq68Qbnb7AAAGRElEQVT5v2hsbFRSUpJqamoUExNj7Lrg2QYTzza4eL7Bw7MNnmA9W5/Pp5MnTyoxMdHYNduKgnUVPXr0UKdOnVrMVtXX17eY1brE6XTK6XQGjHXv3j1oGWNiYvifPUh4tsHDsw0unm/w8GyDJxjPtr1nri5hkftVREZGasiQISopKQkYLykpUVpamkWpAACAnTGD1QY5OTnyeDwaOnSoUlNTtWzZMh06dEjTp0%2B3OhoAALChTnl5eXlWh7C7/v37Ky4uTvn5%2BXr55Zd1%2BvRpvfnmmxo0aJDV0dSpUyelp6crIoKubBrPNnh4tsHF8w0enm3whNuzdfis%2BOwiAABAGGMNFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYIWoxYsXKzk5WVFRURoyZIi2bt1qdaSwUFZWpnHjxikxMVEOh0MbN260OlLYKCgo0LBhwxQdHa34%2BHg98MAD2r9/v9WxwsKSJUs0cOBA/5c0pqam6g9/%2BIPVscJSQUGBHA6HsrOzrY4SFvLy8uRwOAK2hIQEq2MZQcEKQevXr1d2drbmz5%2Bv3bt3684771RmZqYOHTpkdbSQd%2BrUKQ0aNEhFRUVWRwk7paWlmjFjhioqKlRSUqLz588rIyNDp07xy3O/rV69eumFF15QZWWlKisrdc899%2BgHP/iB9u7da3W0sLJz504tW7ZMAwcOtDpKWOnXr59qa2v92549e6yOZARf0xCChg8frltvvVVLlizxj/Xt21cPPPCACgoKLEwWXhwOhzZs2KAHHnjA6ihh6dixY4qPj1dpaanuuusuq%2BOEndjYWL300kuaMmWK1VHCQlNTk2699VYtXrxYzz33nG655RYVFhZaHSvk5eXlaePGjaqurrY6inHMYIWYs2fPqqqqShkZGQHjGRkZKi8vtygV8M15vV5JXxUBmHPhwgWtW7dOp06dUmpqqtVxwsaMGTM0ZswYjR492uooYefAgQNKTExUcnKyHnroIX322WdWRzIiPL4utQM5fvy4Lly40OIXTbvd7ha/kBqwK5/Pp5ycHN1xxx3q37%2B/1XHCwp49e5SamqozZ87oO9/5jjZs2KCbb77Z6lhhYd26daqqqlJlZaXVUcLO8OHD9etf/1p9%2BvTR0aNH9dxzzyktLU179%2B5VXFyc1fG%2BFQpWiHI4HAH7Pp%2BvxRhgVzNnztSHH36oDz74wOooYeOmm25SdXW1Tpw4od/97neaNGmSSktLKVnfUk1NjX72s59p8%2BbNioqKsjpO2MnMzPT/ecCAAUpNTdWNN96oVatWKScnx8Jk3x4FK8T06NFDnTp1ajFbVV9f32JWC7CjWbNmadOmTSorK1OvXr2sjhM2IiMj9Td/8zeSpKFDh2rnzp167bXXtHTpUouThbaqqirV19dryJAh/rELFy6orKxMRUVFam5uVqdOnSxMGF66deumAQMG6MCBA1ZH%2BdZYgxViIiMjNWTIEJWUlASMl5SUKC0tzaJUwNX5fD7NnDlTb7/9tt577z0lJydbHSms%2BXw%2BNTc3Wx0j5I0aNUp79uxRdXW1fxs6dKgmTpyo6upqypVhzc3N2rdvn3r27Gl1lG%2BNGawQlJOTI4/Ho6FDhyo1NVXLli3ToUOHNH36dKujhbympiZ98skn/v2DBw%2BqurpasbGx6t27t4XJQt%2BMGTO0du1a/f73v1d0dLR/FtblcqlLly4WpwttP//5z5WZmamkpCSdPHlS69at0x//%2BEcVFxdbHS3kRUdHt1gn2K1bN8XFxbF%2B0IA5c%2BZo3Lhx6t27t%2Brr6/Xcc8%2BpsbFRkyZNsjrat0bBCkE//OEP9Ze//EXPPvusamtr1b9/f73zzju6/vrrrY4W8iorKzVy5Ej//qU1AJMmTdLKlSstShUeLn2tSHp6esD4ihUrNHny5PYPFEaOHj0qj8ej2tpauVwuDRw4UMXFxbr33nutjgZc0eHDh/WjH/1Ix48f13XXXacRI0aooqIiLP4943uwAAAADGMNFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAw/4fUtwRnPimvUwAAAAASUVORK5CYII%3D"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common2085472178238802677">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">5</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">1</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">0</td>
        <td class="number">48</td>
        <td class="number">6.2%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme2085472178238802677">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">0</td>
        <td class="number">48</td>
        <td class="number">6.2%</td>
        <td>
            <div class="bar" style="width:34%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">1</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">1</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">2</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">3</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">4</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">5</td>
        <td class="number">144</td>
        <td class="number">18.8%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Y1">Y1<br/>
            <small>Numeric</small>
        </p>
    </div><div class="col-md-6">
    <div class="row">
        <div class="col-sm-6">
            <table class="stats ">
                <tr>
                    <th>Distinct count</th>
                    <td>587</td>
                </tr>
                <tr>
                    <th>Unique (%)</th>
                    <td>76.4%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Missing (n)</th>
                    <td>0</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (%)</th>
                    <td>0.0%</td>
                </tr>
                <tr class="ignore">
                    <th>Infinite (n)</th>
                    <td>0</td>
                </tr>
            </table>

        </div>
        <div class="col-sm-6">
            <table class="stats ">

                <tr>
                    <th>Mean</th>
                    <td>22.307</td>
                </tr>
                <tr>
                    <th>Minimum</th>
                    <td>6.01</td>
                </tr>
                <tr>
                    <th>Maximum</th>
                    <td>43.1</td>
                </tr>
                <tr class="ignore">
                    <th>Zeros (%)</th>
                    <td>0.0%</td>
                </tr>
            </table>
        </div>
    </div>
</div>
<div class="col-md-3 collapse in" id="minihistogram809341647872992039">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABLCAYAAAA1fMjoAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAAdFJREFUeJzt3DGO00AYhuHfEa2TPopvwSG4EzUl9%2BEQ3CJRDmCLgoKYArIN8Iks7NjZPE8ZKxpHmlfjseV08zzP1djpdKphGFoPy507Ho91OByajvmm6Wg/9X1fVT9%2B8Ha7XeIUuCPjONYwDE/zpqVFAum6rqqqttvtfwnk7ftPN3/n84d3/zwubV3nTUub5iPCHREIBAKBQCAQCAQCgUAgEAgEAsEiDwrXwMNF/oYVBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBA87AtTr8lzXv661aO%2BLGYFgUAgEAgEAoFAIBAI3MVamRZ3pJ7jUf8myQoCgUAgcInFi3kNl2VWEAisIDdY6waal2MFgUAgEAgEAoFAsLpNuo0wa2IFgUAgEAgEgkX2IPM8V1XVOI6/HPv29Uvr02FFfjcnrp9d501LiwQyTVNVVQ3DsMTwrNju45%2BPTdNUu92u3clUVTcvkOXlcqnz%2BVx931fXda2H587M81zTNNV%2Bv6/Npu2uYJFA4F7YpEMgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQCAQCgUAgEAgEAoFAIBAIBAKBQCAQfAeRk1Cy2vhfZQAAAABJRU5ErkJggg%3D%3D">

</div>
<div class="col-md-12 text-right">
    <a role="button" data-toggle="collapse" data-target="#descriptives809341647872992039,#minihistogram809341647872992039"
       aria-expanded="false" aria-controls="collapseExample">
        Toggle details
    </a>
</div>
<div class="row collapse col-md-12" id="descriptives809341647872992039">
    <ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#quantiles809341647872992039"
                                                  aria-controls="quantiles809341647872992039" role="tab"
                                                  data-toggle="tab">Statistics</a></li>
        <li role="presentation"><a href="#histogram809341647872992039" aria-controls="histogram809341647872992039"
                                   role="tab" data-toggle="tab">Histogram</a></li>
        <li role="presentation"><a href="#common809341647872992039" aria-controls="common809341647872992039"
                                   role="tab" data-toggle="tab">Common Values</a></li>
        <li role="presentation"><a href="#extreme809341647872992039" aria-controls="extreme809341647872992039"
                                   role="tab" data-toggle="tab">Extreme Values</a></li>

    </ul>

    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active row" id="quantiles809341647872992039">
            <div class="col-md-4 col-md-offset-1">
                <p class="h4">Quantile statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Minimum</th>
                        <td>6.01</td>
                    </tr>
                    <tr>
                        <th>5-th percentile</th>
                        <td>10.463</td>
                    </tr>
                    <tr>
                        <th>Q1</th>
                        <td>12.992</td>
                    </tr>
                    <tr>
                        <th>Median</th>
                        <td>18.95</td>
                    </tr>
                    <tr>
                        <th>Q3</th>
                        <td>31.668</td>
                    </tr>
                    <tr>
                        <th>95-th percentile</th>
                        <td>39.86</td>
                    </tr>
                    <tr>
                        <th>Maximum</th>
                        <td>43.1</td>
                    </tr>
                    <tr>
                        <th>Range</th>
                        <td>37.09</td>
                    </tr>
                    <tr>
                        <th>Interquartile range</th>
                        <td>18.675</td>
                    </tr>
                </table>
            </div>
            <div class="col-md-4 col-md-offset-2">
                <p class="h4">Descriptive statistics</p>
                <table class="stats indent">
                    <tr>
                        <th>Standard deviation</th>
                        <td>10.09</td>
                    </tr>
                    <tr>
                        <th>Coef of variation</th>
                        <td>0.45233</td>
                    </tr>
                    <tr>
                        <th>Kurtosis</th>
                        <td>-1.2456</td>
                    </tr>
                    <tr>
                        <th>Mean</th>
                        <td>22.307</td>
                    </tr>
                    <tr>
                        <th>MAD</th>
                        <td>9.1446</td>
                    </tr>
                    <tr class="">
                        <th>Skewness</th>
                        <td>0.36045</td>
                    </tr>
                    <tr>
                        <th>Sum</th>
                        <td>17132</td>
                    </tr>
                    <tr>
                        <th>Variance</th>
                        <td>101.81</td>
                    </tr>
                    <tr>
                        <th>Memory size</th>
                        <td>6.1 KiB</td>
                    </tr>
                </table>
            </div>
        </div>
        <div role="tabpanel" class="tab-pane col-md-8 col-md-offset-2" id="histogram809341647872992039">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAGQCAYAAAByNR6YAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAIABJREFUeJzt3X9YVXWCx/HPTeD6I7iJCBdGJMbFtsRIpFGsSbQiKXXKanI0B8vY2jHL0HUktwnbBHOmplo3t59mk63WM2nu2jhio2iPuQpKKfU4aJSUINoqP0yvpGf/aLzTFfFHfeEcLu/X85zn4XzPuYfPd87M42e%2B93Cvy7IsSwAAADDmArsDAAAABBsKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYFmJ3gI7gxIkT2rt3r8LDw%2BVyueyOAwBAh2BZlhoaGhQXF6cLLmjbNSUKVhvYu3ev4uPj7Y4BAECHVFVVpV69erXp76RgtYHw8HBJ397giIgIm9MAANAx1NfXKz4%2B3v/vcFuiYLWBk28LRkREULAAAGhjdjyew0PuAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCML3tGm0mbtcruCOelZM4IuyMAANopVrAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGFBXbDWr1%2BvUaNGKS4uTi6XS8uXLw847nK5Trv99re/9Z9z8cUXNzs%2Bc%2BbMtp4KAABoR0LsDtCaDh8%2BrJSUFN1111269dZbmx2vrq4O2P/Tn/6kSZMmNTv3scceU05Ojn//wgsvbJ3AAAAgKAR1wcrKylJWVlaLx71eb8D%2BO%2B%2B8o2HDhunHP/5xwHh4eHizcwEAAFoS1G8Rno99%2B/Zp5cqVmjRpUrNjTzzxhHr06KErrrhCc%2BbM0bFjx2xICAAA2ougXsE6H4sWLVJ4eLjGjBkTMP7ggw8qNTVV3bt31%2BbNm5WXl6fKykq99NJLLV7L5/PJ5/P59%2Bvr61stNwAAcB4K1t%2B88sorGj9%2BvDp37hww/tBDD/l/vvzyy9W9e3fddttt/lWt0yksLNTs2bNbNS8AAHAu3iKUtGHDBu3cuVP33HPPWc8dPHiwJGnXrl0tnpOXl6e6ujr/VlVVZSwrAABwPlawJL388ssaOHCgUlJSznrutm3bJEmxsbEtnuN2u%2BV2u43lAwAA7UtQF6zGxsaAlabKykqVlZUpMjJSvXv3lvTt81FvvfWWnnzyyWav/%2BCDD7Rp0yYNGzZMHo9HW7Zs0UMPPaTRo0f7Xw8AAHCqoC5YJSUlGjZsmH8/NzdXkpSdna1XX31VkrRkyRJZlqVf/OIXzV7vdru1dOlSzZ49Wz6fTwkJCcrJydGMGTPaJD8AAGifXJZlWXaHCHb19fXyeDyqq6tTRESE3XFskzZrld0RzkvJnBF2RwAA/AB2/vvLQ%2B4AAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAsBC7A7Sm9evX67e//a1KS0tVXV2tZcuW6eabb/YfnzhxohYtWhTwmkGDBmnTpk3%2BfZ/Pp%2BnTp%2Bu//uu/dOTIEV177bV67rnn1KtXrzabB%2ByRNmuV3RHOS8mcEXZHAAD8TVCvYB0%2BfFgpKSmaP39%2Bi%2BeMGDFC1dXV/u3dd98NOD516lQtW7ZMS5Ys0fvvv6/GxkaNHDlSx48fb%2B34AACgnQrqFaysrCxlZWWd8Ry32y2v13vaY3V1dXr55Zf1hz/8Qdddd50k6fXXX1d8fLzWrFmjG264wXhmAADQ/gX1Cta5WLdunaKjo9W3b1/l5OSotrbWf6y0tFRNTU3KzMz0j8XFxSk5OVkbN260Iy4AAGgHgnoF62yysrJ0%2B%2B23KyEhQZWVlXrkkUc0fPhwlZaWyu12q6amRmFhYerevXvA62JiYlRTU9PidX0%2Bn3w%2Bn3%2B/vr6%2B1eYAAACcp0MXrDvuuMP/c3JystLS0pSQkKCVK1dqzJgxLb7Osiy5XK4WjxcWFmr27NlGswIAgPajw79F%2BF2xsbFKSEhQRUWFJMnr9erYsWM6ePBgwHm1tbWKiYlp8Tp5eXmqq6vzb1VVVa2aGwAAOAsF6zu%2B%2BuorVVVVKTY2VpI0cOBAhYaGqqioyH9OdXW1duzYoSFDhrR4HbfbrYiIiIANAAB0HEH9FmFjY6N27drl36%2BsrFRZWZkiIyMVGRmp/Px83XrrrYqNjdVnn32mhx9%2BWFFRUbrlllskSR6PR5MmTdK0adPUo0cPRUZGavr06erfv7//rwoBAABOFdQFq6SkRMOGDfPv5%2BbmSpKys7O1YMECbd%2B%2BXa%2B99poOHTqk2NhYDRs2TEuXLlV4eLj/Nb///e8VEhKin//85/4PGn311VfVqVOnNp8PAABoH1yWZVl2hwh29fX18ng8qqur69BvF7a3T0Zvb/gkdwAIZOe/vzyDBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAwL6oK1fv16jRo1SnFxcXK5XFq%2BfLn/WFNTk37961%2Brf//%2B6tatm%2BLi4vTLX/5Se/fuDbjGxRdfLJfLFbDNnDmzracCAADakaAuWIcPH1ZKSormz5/f7NjXX3%2BtrVu36pFHHtHWrVv19ttv669//atGjx7d7NzHHntM1dXV/u1f//Vf2yI%2BAABop0LsDtCasrKylJWVddpjHo9HRUVFAWP//u//rp/85Cfas2ePevfu7R8PDw%2BX1%2Btt1awAACB4BPUK1vmqq6uTy%2BXSRRddFDD%2BxBNPqEePHrriiis0Z84cHTt27IzX8fl8qq%2BvD9gAAEDHEdQrWOfj6NGjmjlzpsaNG6eIiAj/%2BIMPPqjU1FR1795dmzdvVl5eniorK/XSSy%2B1eK3CwkLNnj27LWIDAAAHclmWZdkdoi24XC4tW7ZMN998c7NjTU1Nuv3227Vnzx6tW7cuoGCd6o9//KNuu%2B02HThwQD169DjtOT6fTz6fz79fX1%2Bv%2BPh41dXVnfHawS5t1iq7IwS1kjkj7I4AAI5SX18vj8djy7%2B/HX4Fq6mpST//%2Bc9VWVmpv/zlL2e9AYMHD5Yk7dq1q8WC5Xa75Xa7jWcFAADtQ4cuWCfLVUVFhdauXdtiYfqubdu2SZJiY2NbOx4AAGingrpgNTY2ateuXf79yspKlZWVKTIyUnFxcbrtttu0detW/c///I%2BOHz%2BumpoaSVJkZKTCwsL0wQcfaNOmTRo2bJg8Ho%2B2bNmihx56SKNHjw74K0MAAIDvCuqCVVJSomHDhvn3c3NzJUnZ2dnKz8/XihUrJElXXHFFwOvWrl2rjIwMud1uLV26VLNnz5bP51NCQoJycnI0Y8aMtpsEAABod4K6YGVkZOhMz/Cf7fn%2B1NRUbdq0yXQsAAAQ5PgcLAAAAMMoWAAAAIZRsAAAAAxzbMF6/fXXdfToUbtjAAAAnDfHFqzc3Fx5vV7de%2B%2B92rx5s91xAAAAzpljC9bevXv1yiuvqLq6WldffbX69eunJ598Uvv377c7GgAAwBk5tmCFhIRozJgxWrFihfbs2aPs7Gy98sor6tWrl8aMGaOVK1ee9WMWAAAA7ODYgvVdXq9X1157rTIyMuRyuVRSUqJx48YpKSlJGzZssDseAABAAEcXrAMHDujpp59WSkqKrrrqKtXW1mr58uX6/PPP9eWXX2rkyJH65S9/aXdMAACAAI79JPdbbrlF7777rhITE3XPPfcoOztbPXv29B%2B/8MILNWPGDD377LM2pgQAAGjOsQUrIiJCa9as0U9/%2BtMWz4mNjVVFRUUbpgIAADg7xxasRYsWnfUcl8ulPn36tEEaAACAc%2BfYZ7AeeughzZ8/v9n4f/zHf2jatGk2JAIAADg3ji1Yb731lgYPHtxsPD09XUuXLrUhEQAAwLlxbME6cOCAunfv3mw8IiJCBw4csCERAADAuXFswerTp4/%2B/Oc/Nxv/85//rMTERBsSAQAAnBvHPuQ%2BdepUTZ06VV999ZWGDx8uSXrvvfc0b948/e53v7M5HQAAQMscW7BycnJ09OhRFRQU6NFHH5Uk9erVS88%2B%2B6zuvvtum9MBAAC0zLEFS5KmTJmiKVOmqLq6Wl26dNFFF11kdyQAAICzcnTBOik2NtbuCAAAAOfMsQ%2B579%2B/X3fddZd69%2B6tzp07KywsLGADAABwKseuYE2cOFG7d%2B/Wv/zLvyg2NlYul8vuSAAAAOfEsQVr/fr1Wr9%2BvQYMGGB3FAAAgPPi2LcIe/XqxaoVAABolxxbsH7/%2B98rLy9PX3zxhd1RAAAAzotj3yKcMGGCGhoalJCQoIiICIWGhgYcr62ttSkZAADAmTm2YM2dO9fuCAAAAN%2BLYwvWpEmT7I4AAADwvTj2GSxJ%2Buyzz5Sfn68JEyb43xJcvXq1PvnkE5uTAQAAtMyxBWvDhg3q16%2BfiouL9eabb6qxsVGStHXrVv3mN7%2BxOR0AAEDLHFuwfv3rXys/P19r164N%2BOT24cOHa9OmTed0jfXr12vUqFGKi4uTy%2BXS8uXLA45blqX8/HzFxcWpS5cuysjIUHl5ecA5Bw8e1IQJE%2BTxeOTxeDRhwgQdOnToh08QAAAELccWrI8%2B%2Bki33XZbs/Ho6Gjt37//nK5x%2BPBhpaSkaP78%2Bac9Pm/ePD311FOaP3%2B%2BtmzZIq/Xq%2Buvv14NDQ3%2Bc8aNG6eysjKtWrVKq1atUllZmSZMmPD9JgUAADoExz7kftFFF6mmpkaJiYkB42VlZfrRj350TtfIyspSVlbWaY9ZlqWnn35as2bN0pgxYyRJixYtUkxMjN544w3de%2B%2B9%2BuSTT7Rq1Spt2rRJgwYNkiS9%2BOKLSk9P186dO3XJJZf8gBkCAIBg5dgVrLFjx2rmzJnav3%2B//xPd//d//1fTp0/XnXfe%2BYOvX1lZqZqaGmVmZvrH3G63hg4dqo0bN0qSPvjgA3k8Hn%2B5kqTBgwfL4/H4zzkdn8%2Bn%2Bvr6gA0AAHQcji1YBQUF8nq9io2NVWNjoy677DINGTJEV155pR555JEffP2amhpJUkxMTMB4TEyM/1hNTY2io6ObvTY6Otp/zukUFhb6n9nyeDyKj4//wXkBAED74di3CMPCwrR06VL99a9/1datW3XixAmlpqbqH//xH43%2BnlO/79CyrICx030f4qnnnCovL0%2B5ubn%2B/fr6ekoWAAAdiGML1kl9%2B/ZV3759jV/X6/VK%2BnaVKjY21j9eW1vrX9Xyer3at29fs9fu37%2B/2crXd7ndbrndbsOJAQBAe%2BHYgvVP//RPZzz%2Bwgsv/KDrJyYmyuv1qqioSAMGDJAkHTt2TMXFxXriiSckSenp6aqrq9PmzZv1k5/8RNK3z4HV1dVpyJAhP%2Bj3AwCA4OXYglVdXR2w39TUpPLycjU0NOiaa645p2s0NjZq165d/v3KykqVlZUpMjJSvXv31tSpU1VQUKCkpCQlJSWpoKBAXbt21bhx4yRJl156qUaMGKGcnBw9//zzkr4tfiNHjuQvCAEAQIscW7D%2B%2B7//u9nYN998o3/%2B53/WpZdeek7XKCkp0bBhw/z7J5%2BLys7O1quvvqoZM2boyJEj%2BtWvfqWDBw9q0KBBWr16tcLDw/2vWbx4sR544AH/XxuOHj26xc/VAgAAkCSXZVmW3SHOx86dO5WRkdFshcvJ6uvr5fF4VFdXp4iICLvj2CZt1iq7IwS1kjkj7I4AAI5i57%2B/jv2YhpZ8%2BumnampqsjsGAABAixz7FuGMGTMC9i3LUnV1tVasWKHx48fblAoAAODsHFuwPvjgg4D9Cy64QD179tTcuXOVk5NjUyoAAICzc2zB2rBhg90RAAAAvpd29wwWAACA0zl2BevKK68849fRfNfmzZtbOQ0AAMC5c2zBGjZsmJ5//nn17dtX6enpkqRNmzZp586duvfee/kqGgAA4FiOLViHDh3S5MmTVVBQEDA%2Ba9Ys7du3Ty%2B99JJNyQAAAM7Msc9gvfnmm7rrrruajU%2BcOFFvvfWWDYkAAADOjWMLltvt1saNG5uNb9y4kbcHAQCAozn2LcIHHnhA9913n7Zt26bBgwdL%2BvYZrBdffFEPP/ywzekAAABa5tiCNWvWLCUmJuqZZ57RK6%2B8Ikm69NJL9eKLL2rcuHE2pwMAAGiZYwuWJI0bN44yBQAA2h3HPoMlffst2K%2B%2B%2Bqp%2B85vf6ODBg5KkDz/8UNXV1TYnAwAAaJljV7B27Nih6667Tl27dlVVVZUmTpyo7t27680339QXX3yhRYsW2R0RAADgtBy7gvXQQw9p3Lhx2r17tzp37uwfv%2Bmmm7R%2B/XobkwEAAJyZY1ewtmzZogULFjT7upwf/ehHvEUIAAAczbErWGFhYWpsbGw2XlFRoaioKBsSAQAAnBvHFqzRo0fr3/7t3/TNN99Iklwul7788kvNnDlTY8aMsTkdAABAyxxbsJ588knt3btXXq9XR44c0fDhw/XjH/9YnTt3bvb9hAAAAE7i2GewPB6PNm7cqKKiIm3dulUnTpxQamqqbrjhhmbPZQEAADiJIwtWU1OTbrzxRj333HPKzMxUZmam3ZEAoN1Im7XK7ghBq2TOCLsjoJ1w5FuEoaGh2rZtGytVAACgXXJkwZKkO%2B%2B8UwsXLrQ7BgAAwHlz5FuEJ82fP19r1qxRWlqaunXrFnBs3rx5NqUCAAA4M8cWrNLSUl1%2B%2BeWSpI8%2B%2BijgGG8dAgAAJ3Ncwfr000%2BVmJioDRs22B0FAADge3HcM1hJSUnav3%2B/f/%2BOO%2B7Qvn37bEwEAABwfhxXsCzLCth/9913dfjwYZvSAAAAnD/HFay2dvHFF8vlcjXbJk%2BeLEnKyMhodmzs2LE2pwYAAE7muGewTpaYU8day5YtW3T8%2BHH//o4dO3T99dfr9ttv94/l5OToscce8%2B936dKl1fIAAID2z3EFy7IsTZw4UW63W5J09OhR3Xfffc0%2BpuHtt9828vt69uwZsD937lz16dNHQ4cO9Y917dpVXq/XyO8DAADBz3FvEWZnZys6Oloej0cej0d33nmn4uLi/Psnt9Zw7Ngxvf7667r77rsDVs0WL16sqKgo9evXT9OnT1dDQ0Or/H4AABAcHLeCZeenty9fvlyHDh3SxIkT/WPjx49XYmKivF6vduzYoby8PH344YcqKipq8To%2Bn08%2Bn8%2B/X19f35qxAQCAwziuYNnp5ZdfVlZWluLi4vxjOTk5/p%2BTk5OVlJSktLQ0bd26Vampqae9TmFhoWbPnt3qeQEAgDM57i1Cu3z%2B%2Bedas2aN7rnnnjOel5qaqtDQUFVUVLR4Tl5enurq6vxbVVWV6bgAAMDBWMH6m4ULFyo6Olo33XTTGc8rLy9XU1OTYmNjWzzH7Xb7H9IHAAAdDwVL0okTJ7Rw4UJlZ2crJOTv/5Hs3r1bixcv1o033qioqCh9/PHHmjZtmgYMGKCrrrrKxsQAAMDJKFiS1qxZoz179ujuu%2B8OGA8LC9N7772nZ555Ro2NjYqPj9dNN92kRx99VJ06dbIpLQAAcDoKlqTMzMxmX9EjSfHx8SouLrYhEQAAaM94yB0AAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIaF2B0AAJwubdYquyPAIdrbfxdK5oywO0KHxQoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEdvmDl5%2BfL5XIFbF6v13/csizl5%2BcrLi5OXbp0UUZGhsrLy21MDAAAnK7DFyxJ6tevn6qrq/3b9u3b/cfmzZunp556SvPnz9eWLVvk9Xp1/fXXq6GhwcbEAADAyShYkkJCQuT1ev1bz549JX27evX0009r1qxZGjNmjJKTk7Vo0SJ9/fXXeuONN2xODQAAnIqCJamiokJxcXFKTEzU2LFj9emnn0qSKisrVVNTo8zMTP%2B5brdbQ4cO1caNG%2B2KCwAAHC7E7gB2GzRokF577TX17dtX%2B/bt0%2BOPP64hQ4aovLxcNTU1kqSYmJiA18TExOjzzz9v8Zo%2Bn08%2Bn8%2B/X19f3zrhAQCAI3X4gpWVleX/uX///kpPT1efPn20aNEiDR48WJLkcrkCXmNZVrOx7yosLNTs2bNbJzAAAHA83iI8Rbdu3dS/f39VVFT4/5rw5ErWSbW1tc1Wtb4rLy9PdXV1/q2qqqpVMwMAAGehYJ3C5/Ppk08%2BUWxsrBITE%2BX1elVUVOQ/fuzYMRUXF2vIkCEtXsPtdisiIiJgAwAAHUeHf4tw%2BvTpGjVqlHr37q3a2lo9/vjjqq%2BvV3Z2tlwul6ZOnaqCggIlJSUpKSlJBQUF6tq1q8aNG2d3dAAA4FAdvmB98cUX%2BsUvfqEDBw6oZ8%2BeGjx4sDZt2qSEhARJ0owZM3TkyBH96le/0sGDBzVo0CCtXr1a4eHhNicHAABO1eEL1pIlS8543OVyKT8/X/n5%2BW0TCAAAtHsdvmABABCs0matsjvCOSuZM8LuCEbxkDsAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAw/iqHCBItKevxACAYMcKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwzp8wSosLNSVV16p8PBwRUdH6%2Babb9bOnTsDzsnIyJDL5QrYxo4da1NiAADgdB2%2BYBUXF2vy5MnatGmTioqK9M033ygzM1OHDx8OOC8nJ0fV1dX%2B7fnnn7cpMQAAcLoQuwPYbdWqVQH7CxcuVHR0tEpLS3XNNdf4x7t27Sqv19vW8QAAQDvU4VewTlVXVydJioyMDBhfvHixoqKi1K9fP02fPl0NDQ0tXsPn86m%2Bvj5gAwAAHUeHX8H6LsuylJubq6uvvlrJycn%2B8fHjxysxMVFer1c7duxQXl6ePvzwQxUVFZ32OoWFhZo9e3ZbxQYAAA7jsizLsjuEU0yePFkrV67U%2B%2B%2B/r169erV4XmlpqdLS0lRaWqrU1NRmx30%2Bn3w%2Bn3%2B/vr5e8fHxqqurU0RERKtkbw/SZq06%2B0kAgA6pZM4I49esr6%2BXx%2BOx5d9fVrD%2BZsqUKVqxYoXWr19/xnIlSampqQoNDVVFRcVpC5bb7Zbb7W6tqAAAwOE6fMGyLEtTpkzRsmXLtG7dOiUmJp71NeXl5WpqalJsbGwbJAQAAO1Nhy9YkydP1htvvKF33nlH4eHhqqmpkSR5PB516dJFu3fv1uLFi3XjjTcqKipKH3/8saZNm6YBAwboqquusjk9AABwog7/V4QLFixQXV2dMjIyFBsb69%2BWLl0qSQoLC9N7772nG264QZdccokeeOABZWZmas2aNerUqZPN6QEAgBN1%2BBWssz3jHx8fr%2BLi4jZKAwAAgkGHX8ECAAAwrcOvYLV3fPQBAADOwwoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAwjIIFAABgGAULAADAMAoWAACAYRQsAAAAwyhYAAAAhlGwAAAADKNgnaPnnntOiYmJ6ty5swYOHKgNGzbYHQkAADgUBescLF26VFOnTtWsWbO0bds2/fSnP1VWVpb27NljdzQAAOBAFKxz8NRTT2nSpEm65557dOmll%2Brpp59WfHy8FixYYHc0AADgQCF2B3C6Y8eOqbS0VDNnzgwYz8zM1MaNG0/7Gp/PJ5/P59%2Bvq6uTJNXX1xvPd9x32Pg1AQBoa63xb%2BTJa1qWZfzaZ0PBOosDBw7o%2BPHjiomJCRiPiYlRTU3NaV9TWFio2bNnNxuPj49vlYwAALR3nidb79oNDQ3yeDyt9wtOg4J1jlwuV8C%2BZVnNxk7Ky8tTbm6uf//EiRP6v//7P/Xo0aPF10jfNu34%2BHhVVVUpIiLCTHCHYq7Bp6PMU2KuwYq5Bh/LstTQ0KC4uLg2/90UrLOIiopSp06dmq1W1dbWNlvVOsntdsvtdgeMXXTRRef8OyMiIoL6v/DfxVyDT0eZp8RcgxVzDS5tvXJ1Eg81C2PrAAAImUlEQVS5n0VYWJgGDhyooqKigPGioiINGTLEplQAAMDJWME6B7m5uZowYYLS0tKUnp6uF154QXv27NF9991ndzQAAOBAnfLz8/PtDuF0ycnJ6tGjhwoKCvS73/1OR44c0R/%2B8AelpKQY/12dOnVSRkaGQkKCv/sy1%2BDTUeYpMddgxVxhisuy428XAQAAghjPYAEAABhGwQIAADCMggUAAGAYBQsAAMAwCpYD5Ofny%2BVyBWxer9fuWEasX79eo0aNUlxcnFwul5YvXx5w3LIs5efnKy4uTl26dFFGRobKy8ttSvv9nW2eEydObHaPBw8ebFPaH6awsFBXXnmlwsPDFR0drZtvvlk7d%2B4MOMfn82nKlCmKiopSt27dNHr0aH3xxRc2Jf5%2BzmWeGRkZze7r2LFjbUr8/S1YsECXX365/0Mn09PT9ac//cl/PBju50lnm2uw3NPTKSwslMvl0tSpU/1jwXRvnYaC5RD9%2BvVTdXW1f9u%2BfbvdkYw4fPiwUlJSNH/%2B/NMenzdvnp566inNnz9fW7Zskdfr1fXXX6%2BGhoY2TvrDnG2ekjRixIiAe/zuu%2B%2B2YUJziouLNXnyZG3atElFRUX65ptvlJmZqcOH//7F41OnTtWyZcu0ZMkSvf/%2B%2B2psbNTIkSN1/PhxG5Ofn3OZpyTl5OQE3Nfnn3/epsTfX69evTR37lyVlJSopKREw4cP189%2B9jP//9kJhvt50tnmKgXHPT3Vli1b9MILL%2Bjyyy8PGA%2Bme%2Bs4Fmz36KOPWikpKXbHaHWSrGXLlvn3T5w4YXm9Xmvu3Ln%2BsaNHj1oej8f6z//8TzsiGnHqPC3LsrKzs62f/exnNiVqXbW1tZYkq7i42LIsyzp06JAVGhpqLVmyxH/Ol19%2BaV1wwQXWqlWr7Ir5g506T8uyrKFDh1oPPvigjalaT/fu3a2XXnopaO/nd52cq2UF5z1taGiwkpKSrKKiooD5dYR7aydWsByioqJCcXFxSkxM1NixY/Xpp5/aHanVVVZWqqamRpmZmf4xt9utoUOHauPGjTYmax3r1q1TdHS0%2Bvbtq5ycHNXW1todyYi6ujpJUmRkpCSptLRUTU1NAfc1Li5OycnJ7fq%2BnjrPkxYvXqyoqCj169dP06dPb3err6c6fvy4lixZosOHDys9PT1o76fUfK4nBds9nTx5sm666SZdd911AePBfG%2BdgI9vdYBBgwbptddeU9%2B%2BfbVv3z49/vjjGjJkiMrLy9WjRw%2B747Wak1%2BgfeqXZsfExOjzzz%2B3I1KrycrK0u23366EhARVVlbqkUce0fDhw1VaWtrsi8HbE8uylJubq6uvvlrJycmSvr2vYWFh6t69e8C5MTExzb40vb043Twlafz48UpMTJTX69WOHTuUl5enDz/8sNl3l7YH27dvV3p6uo4ePaoLL7xQy5Yt02WXXaaysrKgu58tzVUKrnsqSUuWLFFpaalKSkqaHQvG/606CQXLAbKysvw/9%2B/fX%2Bnp6erTp48WLVqk3NxcG5O1DZfLFbBvWVazsfbujjvu8P%2BcnJystLQ0JSQkaOXKlRozZoyNyX6Y%2B%2B%2B/Xx999JHef//9s57bnu9rS/PMycnx/5ycnKykpCSlpaVp69atSk1NbeuYP8gll1yisrIyHTp0SH/84x%2BVnZ2t4uLiFs9vz/ezpbledtllQXVPq6qq9OCDD2r16tXq3LnzOb%2BuPd9bJ%2BEtQgfq1q2b%2Bvfvr4qKCrujtKqTfyl56v9Tqq2tbbaqFWxiY2OVkJDQru/xlClTtGLFCq1du1a9evXyj3u9Xh07dkwHDx4MOL%2B93teW5nk6qampCg0NbZf3NSwsTP/wD/%2BgtLQ0FRYWKiUlRc8880zQ3U%2Bp5bmeTnu%2Bp6WlpaqtrdXAgQMVEhKikJAQFRcX69lnn1VISIhiYmKC7t46CQXLgXw%2Bnz755BPFxsbaHaVVnVyG/%2B7S%2B7Fjx1RcXKwhQ4bYmKz1ffXVV6qqqmqX99iyLN1///16%2B%2B239Ze//EWJiYkBxwcOHKjQ0NCA%2B1pdXa0dO3a0q/t6tnmeTnl5uZqamtrlfT2VZVny%2BXxBcz/P5ORcT6c939Nrr71W27dvV1lZmX9LS0vT%2BPHj/T8H%2B721E28ROsD06dM1atQo9e7dW7W1tXr88cdVX1%2Bv7Oxsu6P9YI2Njdq1a5d/v7KyUmVlZYqMjFTv3r01depUFRQUKCkpSUlJSSooKFDXrl01btw4G1OfvzPNMzIyUvn5%2Bbr11lsVGxurzz77TA8//LCioqJ0yy232Jj6%2B5k8ebLeeOMNvfPOOwoPD/evQHo8HnXp0kUej0eTJk3StGnT1KNHD0VGRmr69Onq379/s4dsnexs89y9e7cWL16sG2%2B8UVFRUfr44481bdo0DRgwQFdddZXN6c/Pww8/rKysLMXHx6uhoUFLlizRunXrtGrVqqC5nyedaa7BdE8lKTw8POCZQenbd0h69OjhHw%2Bme%2Bs4dv35Iv7ujjvusGJjY63Q0FArLi7OGjNmjFVeXm53LCPWrl1rSWq2ZWdnW5b17Uc1PProo5bX67Xcbrd1zTXXWNu3b7c39Pdwpnl%2B/fXXVmZmptWzZ08rNDTU6t27t5WdnW3t2bPH7tjfy%2BnmKclauHCh/5wjR45Y999/vxUZGWl16dLFGjlyZLub79nmuWfPHuuaa66xIiMjrbCwMKtPnz7WAw88YH311Vf2Bv8e7r77bishIcEKCwuzevbsaV177bXW6tWr/ceD4X6edKa5BtM9bcmpH0MRTPfWaVyWZVltWegAAACCHc9gAQAAGEbBAgAAMIyCBQAAYBgFCwAAwDAKFgAAgGEULAAAAMMoWAAAAIZRsAAAAAyjYAEAABhGwQIAADCMggUAAGAYBQsAAMAwChYAAIBhFCwAAADDKFgAAACGUbAAAAAMo2ABAAAYRsECAAAw7P8BSs0UDn%2B3T%2BsAAAAASUVORK5CYII%3D"/>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12" id="common809341647872992039">
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">15.16</td>
        <td class="number">6</td>
        <td class="number">0.8%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">13.0</td>
        <td class="number">5</td>
        <td class="number">0.7%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">12.93</td>
        <td class="number">4</td>
        <td class="number">0.5%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">32.31</td>
        <td class="number">4</td>
        <td class="number">0.5%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">14.6</td>
        <td class="number">4</td>
        <td class="number">0.5%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">10.68</td>
        <td class="number">4</td>
        <td class="number">0.5%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">15.09</td>
        <td class="number">4</td>
        <td class="number">0.5%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">15.55</td>
        <td class="number">4</td>
        <td class="number">0.5%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">15.23</td>
        <td class="number">4</td>
        <td class="number">0.5%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">28.15</td>
        <td class="number">4</td>
        <td class="number">0.5%</td>
        <td>
            <div class="bar" style="width:1%">&nbsp;</div>
        </td>
</tr><tr class="other">
        <td class="fillremaining">Other values (577)</td>
        <td class="number">725</td>
        <td class="number">94.4%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
        <div role="tabpanel" class="tab-pane col-md-12"  id="extreme809341647872992039">
            <p class="h4">Minimum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">6.01</td>
        <td class="number">1</td>
        <td class="number">0.1%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">6.04</td>
        <td class="number">1</td>
        <td class="number">0.1%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">6.05</td>
        <td class="number">1</td>
        <td class="number">0.1%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">6.07</td>
        <td class="number">1</td>
        <td class="number">0.1%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">6.366</td>
        <td class="number">1</td>
        <td class="number">0.1%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
            <p class="h4">Maximum 5 values</p>
            
<table class="freq table table-hover">
    <thead>
    <tr>
        <td class="fillremaining">Value</td>
        <td class="number">Count</td>
        <td class="number">Frequency (%)</td>
        <td style="min-width:200px">&nbsp;</td>
    </tr>
    </thead>
    <tr class="">
        <td class="fillremaining">42.62</td>
        <td class="number">1</td>
        <td class="number">0.1%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">42.74</td>
        <td class="number">1</td>
        <td class="number">0.1%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">42.77</td>
        <td class="number">1</td>
        <td class="number">0.1%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">42.96</td>
        <td class="number">1</td>
        <td class="number">0.1%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr><tr class="">
        <td class="fillremaining">43.1</td>
        <td class="number">1</td>
        <td class="number">0.1%</td>
        <td>
            <div class="bar" style="width:100%">&nbsp;</div>
        </td>
</tr>
</table>
        </div>
    </div>
</div>
</div><div class="row variablerow ignore">
    <div class="col-md-3 namecol">
        <p class="h4 pp-anchor" id="pp_var_Y2"><s>Y2</s><br/>
            <small>Highly correlated</small>
        </p>
    </div><div class="col-md-3">
    <p><em>This variable is highly correlated with <a href="#pp_var_Y1"><code>Y1</code></a> and should be ignored for analysis</em></p>
</div>
<div class="col-md-6">
    <table class="stats ">
        <tr>
            <th>Correlation</th>
            <td>0.97586</td>
        </tr>
    </table>
</div>
</div>
    <div class="row headerrow highlight">
        <h1>Correlations</h1>
    </div>
    <div class="row variablerow">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAkEAAAH1CAYAAADxtypTAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAIABJREFUeJzs3XtclHXe//HXoMjBE66AmlkSCm5iimB00jzQrzXzsGXYTZ62sF1BQBbKvNvScDPddTOVrPRuazPddE1bK%2B/c3WrNe/NQia5oeCuiskkJkimHEZD5/UHO3QTmjPMddXbez8djHiPXXPOez1wC8%2BFzXXONxWaz2RARERHxMX6XuwARERGRy0FNkIiIiPgkNUEiIiLik9QEiYiIiE9SEyQiIiI%2BSU2QiIiI%2BCQ1QSIiIuKT1ASJiIiIT1ITJCIiIj6p5eUuQMTb/etf/2LYsGHnvd3f3582bdrQvXt3Bg8ezPjx42nTps0lrFBERJpj0cdmiLjnu01QVFRUkwanrq6OiooKvvjiCwCuuuoqXn31Va699tpLXquIiPwfNUEibvpuE/Taa6%2BRkJDQ7Hrbt28nNTWVyspKYmNjeeONNy5lmSIi8j06JkjkEklISOCXv/wlAPn5%2BRQUFFzmikREfJuaIJFL6I477rD/e/fu3ZexEhER0YHRIpdQ27Zt7f%2BuqqpyuO2TTz5hxYoV7Ny5k5MnT9KuXTv69evHhAkTuPnmm5vNO3XqFG%2B88QabN2/m4MGDVFZWEhQUxDXXXMOQIUOYOHEi7du3d7hPdHQ0AP/4xz%2BYN28e77//Pn5%2BfvTu3Zvf//73tGzZkt27d/Pqq6%2Byb98%2BSktLCQgIICIigsTERJKTk5s9sNtqtfLGG2%2BwceNGDh48SF1dHZ06deKWW27hwQcfpHv37g7rb9%2B%2BnYkTJ9K3b19WrlzJihUreOuttzhy5Aj%2B/v707t2bCRMmkJiYeDGbWkTkgtQEiVxCR44csf%2B7c%2BfO9n8vWLCA5cuXA9C%2BfXuioqI4fvw477//Pu%2B//z4pKSk88sgjDlmHDx9m8uTJlJaW0rJlS6655hq6du3KF198wd69e9m7dy/vvvsub775Jq1bt25SS3p6Ovn5%2BURFRVFRUUFYWBgtW7bkL3/5C1lZWdTX19OhQwd69OhBVVUV//znP9m9ezcbNmzgjTfecGiEvvzyS372s59x6NAhALp3707r1q0pKipi9erVvPXWW8ybN4%2B77rqrSR11dXVMmTKFrVu30qFDByIjIykuLmbbtm1s27aN2bNn8x//8R/ubXgRkebYRMQtJSUltqioKFtUVJRt27ZtP7juo48%2BaouKirL17t3bVlZWZrPZbLY//vGPtqioKFt8fLztz3/%2Bs33dhoYG27vvvmvr16%2BfLSoqyrZmzRqHrPHjx9uioqJsSUlJtq%2B%2B%2BsrhfuvXr7f16tXLFhUVZXv99dcd7neu1piYGNuOHTtsNpvNdvbsWdvXX39tO3v2rO3WW2%2B1RUVF2ZYvX26rr6%2B336%2BgoMB200032aKiomwvvfSSfXl9fb1t9OjRtqioKNudd95p%2B/zzz%2B23nT592vb444/bn/OuXbvst23bts1eS79%2B/WwbNmyw33bq1CnbpEmTbFFRUbYbb7zRVldX94PbVUTkYuiYIBEPs1qt7Nu3j1mzZvHWW28BMHnyZEJDQ6mtrWXJkiUAzJ07l1GjRtnvZ7FYuOuuu%2BwToCVLllBfXw/AiRMnOHDgAABz5swhPDzc4X5jxozhxhtvBGD//v3N1jV8%2BHAGDBgAgJ%2BfHyEhIVRUVFBWVgZAUlISLVq0sK/fu3dvsrKySExMJCQkxL78vffe4/PPPycgIIDly5fTq1cv%2B21t2rTh17/%2BNQMHDqSuro6FCxc2W0tGRgYjR460f922bVv78z558iTFxcXn2boiIhdPu8NEDJo4ceIF17nvvvvIzMwEGt8lVl5eTuvWrc97wsVRo0YxZ84cvvrqK/bt28cNN9xAx44d2bZtG1arlcDAwCb3OXv2rH13ldVqbTY3Li6uybIOHTrQvn17vvnmG3Jycpg6dSp9%2B/bFz6/x76WkpCSSkpIc7vPBBx8AMHToULp169bsY/3sZz9jy5Yt7Nixg9OnTzscGwUwZMiQJveJjIy0//vUqVPN5oqIuENNkIhB3z9ZosViISAggJCQEKKjo0lMTKRHjx72289Nc%2Brq6njggQfOm9uiRQsaGho4dOgQN9xwg315YGAgpaWl7N69m6NHj1JSUkJRURGff/451dXVADQ0NDSbGRYW1uzj5OTk8MQTT7B582Y2b95M%2B/btSUhI4NZbb2Xw4MEOxzIB9ilN7969z1v/udvOnj3LkSNHiImJcbi9U6dOTe7z3ebu7Nmz580WEblYaoJEDPrVr3513pMlNuf06dMA1NbWsnPnzguu/92JyKFDh/jNb37D5s2bHRqdNm3aEB8fz/HjxyksLDxvVnMTJGic9lx77bW88sorfPzxx3zzzTf85S9/4S9/%2BQsWi4XBgwcze/ZsezNUWVkJ0GS6813fbQy//644aPxokR9i0zldRcQD1ASJXEZBQUFA46Rk3bp1Tt/vxIkTjB8/nhMnTnDVVVeRlJTE9ddfz3XXXcfVV1%2BNxWIhOzv7B5ugH5KQkEBCQgJWq5VPP/2UTz75hC1btrB3714%2B/PBDSktLeeutt7BYLPZ3np1r6Jrz3eatuXeqiYhcDmqCRC6jiIgIoPHt7vX19bRs2fRH0mazsX37djp37sxVV11Fq1atePPNNzlx4gQhISG8%2Beab/OhHP2pyv6%2B%2B%2BsrlempraykpKaGyspK%2BffsSGBjIbbfdxm233UZWVhbvvvsuv/zlLyksLGT//v306tWL6667jn379rF3797z5u7Zswdo3D14zTXXuFyXiIgn6N1hIpfRgAEDaNu2LVVVVeedBL399ttMmjSJ4cOH8%2BWXXwKNn1cGjR/G2lwDdPDgQXbt2gW4djzNRx99xF133cXDDz9MbW1tk9tvueUW%2B7/P5Z47qPmDDz6gpKSk2dzXXnsNgH79%2BtGuXTun6xER8SQ1QSKXUXBwMA8//DAATz/9NG%2B%2B%2BabD8T1/%2B9vfmDVrFtD4lvZzU5TrrrsOgMLCQjZt2mRf32az8dFHH5GSkkJdXR0ANTU1TtczaNAgOnTowMmTJ5kxYwYnT56031ZVVcX8%2BfMB6NKlCz179gTgJz/5CdHR0Zw5c4YpU6Y47IKrrKzkiSee4H/%2B539o2bIlOTk5zm8cEREP0%2B4wkctsypQplJSUsGbNGv7zP/%2BT3/72t1x99dV89dVXHD9%2BHID%2B/fvz61//2n6fsWPHsmrVKo4cOUJGRgZdu3alQ4cOlJaWcuLECfz9/bnxxhvZsWOHS7vFWrVqxaJFi3jooYfYuHEj77//Ptdccw1%2Bfn6UlJRQXV1NUFAQ8%2BbNo1WrVgC0bNmSpUuXMmXKFA4dOsTo0aMdzhh97m38Tz31FPHx8WY3noiIG9QEiVxmFouFOXPmcOedd/LGG2%2Bwa9cu%2B8kH%2B/Xrx9133824cePsTQc0vttq7dq1LF%2B%2BnA8//JB//etflJeX07lzZwYPHsykSZMIDg4mMTGRwsJCjh07xlVXXeVUPQkJCfzpT3/ilVde4bPPPuPw4cO0bNmSzp07c9ttt/Hggw82ybr66qt58803%2BeMf/8h7771HUVERX375JV26dGHgwIE88MADTT47TETkcrPY9N5TERER8UE6JkhERER8kpogERER8UlqgkRERMQnqQkSERERoyoqKrjjjjvYvn37edfZvHkzI0eOpF%2B/fgwfPpwPP/zQ4fbly5czaNAg%2BvXrx4QJEzh06JDxOtUEiYiIiDGfffYZ48aN4%2BjRo%2Bdd5/Dhw6Snp5OZmcmnn35Keno606dPt5/SY/369axYsYKXX36Z7du307t3bzIyMox/jqCaIBERETFi/fr15OTkkJWVdcH14uPjSUxMpGXLltx1110MGDCA1atXA7BmzRqSk5Pp2bMnAQEBZGdnc%2BzYsR%2BcLF0MNUEiIiICwPHjx9m7d6/D5dxJW51x22238de//pW77rrrB9c7ePAgUVFRDst69OhhP%2BP892/39/ene/fuF/2h0OfjPSdLtFjM5kVEwIED0LMnFBcbi23hZ3ZUFxEB%2B/dDdLTRMgHYts1cVkAA9OkDe/bAmTPmcgGSkszmdesGH34IQ4bAeT7q6qIVt%2BljLuzqq%2BGdd%2BDuu%2BHbzwoz5p//NJsn3uPBB83mhYXBvHnw2GNQVmY2u77eXFZYGCxYADk55utcscJsnrNMvy4CqxcvJi8vz2HZtGnTSE9Pd%2Br%2BYWFhTq1XVVVFUFCQw7LAwECqq6udut0U72mCTAsJgRYtGq%2BvYF5SJi1aNP48tmhxuSu5sHbtGuu84j/Hs23bxkLbtr3clTjHYgFvOPeq6jQrOBj8/Bqvr2TeUudlNm7cOIYOHeqwzNnGxhVBQUFYrVaHZVarldatWzt1uym%2B2wSJiIh4Mz/zR7SEh4cTHh5uPPf7oqKi2Lt3r8OygwcPEhMTA0DPnj05cOAAQ4YMAaCuro7Dhw832YXmLh0TJCIiIpfUqFGj2LFjBxs3bqS%2Bvp6NGzeyY8cORo8eDcC9997L66%2B/TmFhIWfOnOF3v/sdoaGhxj%2BEWZMgERERb%2BSBSZAnxcbG8tRTTzFq1CgiIyN5/vnnWbBgAY8//jhdu3ZlyZIlREREADB27FhOnz5NWloaFRUV9OnTh5deegl/f3%2BjNakJEhER8UZXeBO0f/9%2Bh6/z8/Mdvh44cCADBw5s9r4Wi4UHH3yQB00fyP89V/YWFBEREfEQTYJERES80RU%2BCfIG2oIiIiLikzQJEhER8UaaBLlNTZCIiIg3UhPkNm1BERER8UmaBImIiHgjTYLcpi0oIiIiPkmTIBEREW%2BkSZDb1ASJiIh4IzVBbtMWFBEREZ/kdBNUXFxMXFwcy5Ytc1heUVHBsGHDyMvLsy87e/Ys06ZNY8mSJeYqFRERkf/j52f%2B4mOcfsYRERHMnz%2BfRYsWsXXrVgBqa2tJS0sjJiaGtLQ0AI4dO8bDDz/MX//6V89ULCIiImKAS21fYmIiKSkpZGVlUVpayqxZs7BarcybNw%2BLxUJxcTE//elP6du3L7GxsZ6qWURERDQJcpvLB0ZnZmZSUFBAcnIytbW1rF27lqCgIADCwsL429/%2BRtu2bfnkk08uuqjjx49TVlbmsCwsIoLwkJCLzmyiVy/Ha0NiDX8PRUc7XpsUHGwuKzDQ8dqk3r3N5kVGOl4bFfxjc1kREY7XIiZce63ZvC5dHK9Nqq83l%2BWpOo8cMZvnCh9sWkyz2Gw2m6t32rRpExkZGYwYMYJnn3222XUmTJjAjTfeSHp6ustFLVmyxOEYI4BpqamkZ2a6nCUiIuIxEybAihWX57E90XiWlprPvIK5PAk6evQoTz75JJMnT2bVqlWsWbOGpKQko0WNGzeOoUOHOiwLGzkS/vAHcw/SqxesWgXJyVBYaCw23m%2BnsSxonACtXAkPPAD79xuNNro5AwMbJytFRWC1mssFmDHDbF5kJCxaBJmZjfWa9E6wwZ%2BFiAiYP79xAxQXm8sFWL3abB6AxQKu/0116fl6nU89ZTavSxf4%2Bc/hpZfMv4CangSlpcHzz//7vNBrEuQ2l5qgyspKpk6dyuDBg5k5cyaRkZHk5uYSHR1N3759jRUVHh5OeHi440LTLwLnFBZCfr6xuHwPfU/u32%2B0TACqq83mQWMDZDp3716zeecUFXkgu83nhgNp/N7/3AO54ps8tfumtNR8tskm6BxP1Cley%2BmX7IaGBnJycggICCA3NxeApKQkRo4cSXp6OuXl5R4rUkRERL5HB0a7zelnvHDhQnbt2kVeXh4BAQH25bNnz6Zjx45Mnz6dek907SIiItKUmiC3Ob07LDs7m%2Bzs7CbLAwICWL9%2BfZPlKy7XgWIiIiIiTtBnh4mIiHgjH5zcmKYtKCIiIj5JkyARERFvpEmQ29QEiYiIeCM1QW7TFhQRERGfpEmQiIiIN9IkyG3agiIiIuKTNAkSERHxRpoEuU1NkIiIiDdSE%2BQ2bUERERHxSZoEiYiIeCNNgtymJkhERMQbqQlym7agiIiI%2BCRNgkRERLyRJkFu85omqIWfzWherB98CsT77STf4PfR2QaLuTCAhlhgJ5829IeGfLPZfp%2Bay7IEAdfT27IP/GrM5QLFDfcYzcMWA7zLO7YR0FBgNLqn9YixrOvPwJ%2BB0WfWsM9qLBaAA5j9eRKwYfZn3%2BKBTID7q18xmhdhhXnAY9bZFFcbjaauzlzWdVZYAORYf82hKnO5AOvMxskl5DVNkIiIiHyHJkFuUxMkIiLijdQEuU1bUERERHySJkEiIiLeSJMgt2kLioiIiE/SJEhERMQbaRLkNjVBIiIi3khNkNvUBImIiIgRJ06c4IknnmDHjh20aNGCUaNGMWPGDFq2dGw3UlJS%2BOyzzxyWVVdXM27cOHJzcykvL%2BfWW28lODjYfnuHDh344IMPjNarJkhERMQbXYGToOnTp9OpUye2bNlCeXk5U6dO5dVXXyUlJcVhvf/6r/9y%2BHrt2rXk5eUxbdo0APbs2UPXrl2NNz3fd%2BVtQREREfE6R44cYceOHTzyyCMEBQXRrVs3UlNTWbly5Q/e79ChQ8yZM4cFCxYQHh4ONDZBMTExHq9ZkyARERFv5IFJ0PHjxykrK3NYFhYWZm9OfsiBAwcICQmhU6dO9mWRkZEcO3aMU6dO0a5du2bv99RTTzFmzBji4%2BPty/bs2cM333zD3XffTXl5OX369GHGjBn06NHjIp9Z89QEiYiIeCMPNEGrV68mLy/PYdm0adNIT0%2B/4H2rqqoICgpyWHbu6%2Brq6maboE8//ZTdu3ezYMECh%2BXt2rWjR48eTJkyhVatWrFo0SJ%2B9rOfsXHjRtq2bevq0zovNUEiIiICwLhx4xg6dKjDsrCwMKfuGxwcTE2N4wdon/u6devWzd5n9erVDB8%2BvMlj/O53v3P4eubMmbz55pt8%2BumnDBkyxKl6nKEmSERExBt5YBIUHh7u1K6v5vTs2ZOTJ09SXl5OaGgoAEVFRXTu3LnZ6U19fT3vv/8%2Bzz//vMPyyspKnn/%2BecaPH0/Xrl0BOHv2LPX19QQGBl5UbeejA6NFRETEbd27dycuLo65c%2BdSWVlJSUkJS5cuZezYsc2uv3//fs6cOUP//v0dlrdp04aPP/6Y%2BfPnc/r0aaqqqpgzZw5XX321w3FDJqgJEhER8UZ%2BfuYvblq8eDH19fUMGzaMpKQkBg4cSGpqKgCxsbFs2LDBvm5JSQnt27cnICCgSc7SpUtpaGggMTGRgQMHUlZWxvLly/H393e7xu/S7jARERFvdAWeJyg0NJTFixc3e1t%2Bfr7D1z/5yU/4yU9%2B0uy6Xbt2bXKAtic4vQWLi4uJi4tj2bJlDssrKioYNmwYeXl5fP311zz22GPceuutDBgwgEmTJvH5558bL1pERETEXU43QREREcyfP59FixaxdetWAGpra0lLSyMmJoa0tDQef/xxvv76a9555x3%2B8Y9/0L9/f1JSUqiurvbYExAREfFJV%2BDuMG/j0jNOTEwkJSWFrKwsSktLmTVrFlarlXnz5gFgsVjIzMykQ4cOtGrVioceeojy8nIOHz7sidpFRERELprLxwRlZmZSUFBAcnIytbW1rF271n4ypO%2B/ze29994jODiYiIgIlx6juTNWRkSEERJycW/ba050tOO1MQ2xZvN69XK8Nul7J7Vyy7m3LRp%2B%2ByIApk%2BdHhnpeG3Q9QaPsrvuOsdrERNc/HV8QVdd5XhtUn29uaxv32ltvzbl0CGzeS7xwcmNaRabzWZz9U6bNm0iIyODESNG8Oyzzza7zvvvv092djazZ89mzJgxLuUvWbKkyQFRqanTyMy88BkrRURELpV77oF16y7Tg48caT7z7bfNZ17BXP679ejRozz55JNMnjyZVatWsWbNGpKSkuy322w2XnjhBZYvX87cuXO56667XC6quTNWjh4dxooVLkedV3Q0rFwJDzwA%2B/eby/20of%2BFV3JFr16wahUkJ0Nhodns1183lxUY2DiyOHQIrFZzuQCPPGI2LzISFi%2BGjAwoKjIaPbrlu8ayrrsOFi6ErCzzf23%2B%2BS2X//a5MIsFXP%2Bb6tLzUJ02LEbzPLU5Z840m3fVVY0/SosXw7FjZrNNT4Kyshp/pr74wlyueDeXmqDKykqmTp3K4MGDmTlzJpGRkeTm5hIdHU3fvn2pqakhKyuLAwcOsHLlSq6//vqLKqq5M1YWF19U1AXt3w/fe9eeexpMhn1HYaHhQoHvnd7cCKvVfG5Bgdm8c4qKjGfva2U0DmhsgPbtM58rvslTv0uPHTOfXVdnNg8aG6DLugvLJO0Oc5vTW7ChoYGcnBwCAgLIzc0FICkpiZEjR5Kenk55eTlZWVl8%2BeWXvPnmmxfdAImIiIhcCk5PghYuXMiuXbtYt26dw9kdZ8%2Bezf33388dd9xBdXU1rVq1avLhZsuXLzd%2BqmsRERGfpkmQ25xugrKzs8nOzm6yPCAggPXr1xstSkRERC5ATZDbtAVFRETEJ%2Bmzw0RERLyRJkFu0xYUERERn6RJkIiIiDfSJMhtaoJERES8kZogt2kLioiIiE/SJEhERMQbaRLkNm1BERER8UmaBImIiHgjTYLcpiZIRETEG6kJcpu2oIiIiPgkTYJERES8kSZBblMTJCIi4o3UBLlNW1BERER8kiZBIiIi3kiTILd5TRO0bZvZvODgxus//AGqqw0G%2B31qMAwICmq8fv11qKkxmx0fby4rNhZ27oTx4yE/31wu8M7bNqN57drBIOCjGe9y6pTRaA74bzIX1rYtcAt/nvExnD5tLheA/2c4TyyY/T4FiwcyYXX8ArOB4eHAJOb9%2BA/Q8bjZ7MBAc1lhYcD9LIh/A64tM5cLQLrhPLlUvKYJEhERke/QJMhtaoJERES8kZogt2kLioiIiE/SJEhERMQbaRLkNm1BERER8UmaBImIiHgjTYLcpiZIRETEG6kJcpu2oIiIiPgkTYJERES8kSZBbtMWFBEREZ%2BkSZCIiIg30iTIbWqCREREvJGaILdpC4qIiIhP0iRIRETEG2kS5DZtQREREfFJmgSJiIh4I02C3KYmSERExBupCXKb01uwuLiYuLg4li1b5rC8oqKCYcOGkZeXxxdffMEvfvELBgwYQHx8PKmpqZSUlBgvWkRERK48J06cIDU1lfj4eBISEnj66aepr69vdt2UlBT69OlDbGys/fLRRx8BcPbsWebPn88tt9xCbGwsU6dO5fjx48brdboJioiIYP78%2BSxatIitW7cCUFtbS1paGjExMaSlpZGenk54eDhbtmxhy5YttG7dmpkzZxovWkRExOf5%2BZm/uGn69OkEBwezZcsW1q5dy9atW3n11VebXbegoICXX36Z/Px8%2B2XQoEEAvPDCC/zjH//gzTffZMuWLQQGBvKrX/3K7fq%2Bz6VnnJiYSEpKCllZWZSWljJr1iysVivz5s3DYrHwxz/%2BkSeeeILAwEAqKyupqqriRz/6kfGiRURE5Mpy5MgRduzYwSOPPEJQUBDdunUjNTWVlStXNlm3pKSEb775huuvv77ZrD/96U9MmTKFLl260KZNGx5//HE%2B%2Bugj43uXXD4mKDMzk4KCApKTk6mtrWXt2rUEBQUBEBAQAEB2djbvvvsuYWFh5%2B0Af8jx48cpKytzWFZdHUZoaLjLWecTGOh4bYwlyGyexwoFYmPNZfXq5XhtULt2ZvPatHG8NqplW3NZrVs7XouYEG7u9ygA5/7Q9cQfvN%2B%2BphjRoYPjtSnfe626pDxwTFBzr79hYWGEO/F9c%2BDAAUJCQujUqZN9WWRkJMeOHePUqVO0%2B84v8z179tC6dWuysrLYs2cPoaGhTJ48mbFjx3L69Gm%2B/PJLoqKi7OuHhobSvn179u/fT7du3Qw800YuN0F%2Bfn4kJSWRkZHBiBEj6NKlS5N1nn76aebMmcNzzz3HxIkTee%2B992jb1vkXh9WrV5OXl%2BewLC1tGhkZ6a6We0GRkaYTm%2B9q3XbddeYzd%2B40n7lqlfHIQcYTG/Xv74nUW8xH9u1rPtNTLJbLXYFzfLnOSZPMZwKMHOmZXNPuvNNs3pIlZvNc4YEmqLnX32nTppGefuHX36qqKvtQ5JxzX1dXVzs0QbW1tfTr14%2BsrCx69uzJ9u3bSU9Pp3Xr1sR%2B%2Bwd6cHCwQ1ZgYCBVVVUX9bzOx%2BUm6OjRozz55JNMnjyZVatWsWbNGpKSkhzWCfx2ajFjxgz%2B9Kc/sW3bNu644w6nH2PcuHEMHTrUYVl5eRh797pa7fkFBjY2QEVFYLWay%2B1t2WcuDBoLve46OHTIbKEA48eby%2BrVq7EBSk6GwkJzucBHz5lt1tq0aWyAdu6Eykqj0Qxq%2BbG5sNatGxug3bvB8A8%2BN99sNg8aX7BtNvO5pvl6na%2B9ZjbvRz9qbIDefhsqKsxmm54E3XknbNoEX39tLvffTHOvv2FhYU7dNzg4mJqaGodl575u/b2J9pgxYxgzZoz969tuu40xY8bw3//939xyyy0O9z3HarU2yXGXS01QZWUlU6dOZfDgwcycOZPIyEhyc3OJjo4mOjqa0aNH89vf/pYbbrgBaDy6u6Ghgfbt27tUVHh4eJPR2yefQHW1SzFOsVoN5/rVXHidi2G1Qo3h7Px8s3nQ2AAZzj11ymicXWWlB7L9TxsOpLEBOu2BXPFNHniHDdDYAJnO9sRhAF9/fXl3YZnkgUlQc6%2B/zurZsycnT56kvLyc0NBQAIqKiujcuXOTvUFr166ldevWDB8%2B3L6straWgIAA2rdvT6dOnTh48KB9l1hZWRknT5502EVmgtNbsKGhgZycHAICAsjNzQUgKSmJkSNHkp6eTmVlJT169OC3v/0tFRUVVFVVkZubS/fu3enXr5/RokVEROTK0r17d%2BLi4pg7dy6VlZWUlJSwdOlSxo4d22TdyspK5syZw759%2B2hoaODvf/8777zzDuPGjQPgnnvu4YUXXqCkpITKykrmzp3LjTfeyDXXXGO0ZqcnQQsXLmTXrl2sW7fOfgA0wOzk89I4AAAgAElEQVTZs7n//vuZPn06ixcvZsGCBYwYMQKLxcLNN9/M8uXLadWqldGiRUREfN4VeLLExYsXk5uby7Bhw/Dz82PMmDGkpqYCEBsby1NPPcWoUaOYNGkS1dXVTJs2jRMnTtCtWzfmz59PfHw8AGlpadTX1/PAAw9QVVVFQkICzz33nPF6nW6CsrOzyc7ObrI8ICCA9evX27%2BeO3eumcpERETk/K7AJig0NJTFixc3e1v%2Bdw6VsFgspKam2huk7/P39ycnJ4ecnByP1HnOlbcFRURERC4BfXaYiIiIN7oCJ0HeRltQREREfJImQSIiIt5IkyC3qQkSERHxRmqC3KYtKCIiIj5JkyARERFvpEmQ27QFRURExCdpEiQiIuKNNAlym5ogERERb6QmyG3agiIiIuKTNAkSERHxRpoEuU1NkIiIiDdSE%2BQ2bUERERHxSRabzWa73EU4IyLCbF7v3vDOO3D33bB3r7nc4oZrzYUBxMTAu%2B/CiBFQUGA0%2Bp3njxjLatcOBg2Cjz6CU6eMxQJw90iL2cDYWNi5E/r3h/x8o9EBrcz9OPXrB9u3Q0IC7NplLBaAM1YP/NhbLOANv058vM7IHmZ/nnr3hg0bYNQos79LTfNknUVFZvOc9tJL5jN//nPzmVcwTYJERETEJ%2BmYIBEREW%2BkY4LcpiZIRETEG6kJcpu2oIiIiPgkTYJERES8kSZBbtMWFBEREZ%2BkSZCIiIg30iTIbWqCREREvJGaILdpC4qIiIhP0iRIRETEG2kS5DZtQREREfFJmgSJiIh4I02C3KYmSERExBupCXKbtqCIiIj4JE2CREREvJEmQW7TFhQRERGfpEmQiIiIN9IkyG1Ob8Hi4mLi4uJYtmyZw/KKigqGDRtGXl6ew/KFCxcydOhQM1WKiIiIIz8/8xcf4/QzjoiIYP78%2BSxatIitW7cCUFtbS1paGjExMaSlpdnX3bp1Ky%2B//LL5akVEREQMcantS0xMJCUlhaysLEpLS5k1axZWq5V58%2BZhsVgAKC8v51e/%2BhUTJkzwSMEiIiKCJkEGuHxMUGZmJgUFBSQnJ1NbW8vatWsJCgoCoKGhgZycHKZMmUKrVq3YtGnTRRV1/PhxysrKHJZddVUYHTqEX1RecyIjHa%2BNscWYzfNYodCunbmsNm0cr42KjTWb16uX47VB/fzNZUVHO16LmNC7t9m8665zvL5SearOvXvN5smlZbHZbDZX77Rp0yYyMjIYMWIEzz77rH35888/T2FhIUuWLGHdunXk5eXxwQcfuFzUkiVLmhxjlJo6jczMdJezREREPCUyEoqKLtODv/22%2BcyRI81nXsFcngQdPXqUJ598ksmTJ7Nq1SrWrFlDUlISn3zyCevWrWPdunVuFzVu3LgmB1U//HAYf/2r29F2kZGwaBFkZpr9Bn7HNsJcGDQWungxZGQY/0n7aMa7xrLatIH%2B/WHnTqisNBYLwKDp/c0G9uoFq1ZBcjIUFhqNTvDfaSwrOhpeew0mToT9%2B43FArB9m8t/%2B1yYxQKu/0116fl4naNGW4zmXXcdPPccTJ8Ohw4ZjTbKW%2Bp0iQ/uvjLNpSaosrKSqVOnMnjwYGbOnElkZCS5ublER0ezYcMG%2BzvFAOrq6jhz5gzx8fG8%2BOKLxMfHO/044eHhhIc77vo6dqzxYlpRkeFxZkOBwbDvKCqCArPZp04ZjQMaGyDjufn5hgO/VVhoPHtXK6NxQGMDtGuX%2BVzxTZ7afXPokHfsGvKWOuXScLoJOne8T0BAALm5uQAkJSWRn59Peno669atY86cOfb13dkdJiIiIhegSZDbnN6CCxcuZNeuXeTl5REQEGBfPnv2bDp27Mj06dOpr6/3SJEiIiIipjk9CcrOziY7O7vJ8oCAANavX99k%2BT333MM999zjXnUiIiLSPE2C3KaPzRAREfFGaoLcpiZIREREjDhx4gRPPPEEO3bsoEWLFowaNYoZM2bQsmXTduOPf/wjr776KsePHyc8PJyJEyfywAMPAI3HIcfFxWGz2ewnYwb4xz/%2BQXBwsLF61QSJiIh4oytwEjR9%2BnQ6derEli1bKC8vZ%2BrUqbz66qukpKQ4rPe3v/2NZ599luXLl9O3b1927drFww8/TGhoKHfeeScHDx6krq6OnTt30qqVB952%2B60rbwuKiIiI1zly5Ag7duzgkUceISgoiG7dupGamsrKlSubrPvVV18xZcoU%2BvXrh8ViITY2loSEBD755BMA9uzZQ3R0tEcbINAkSERExDt5YBLU3MdWhYWFNTl3X3MOHDhASEgInTp1si%2BLjIzk2LFjnDp1inbf%2Baymc7u9zjlx4gSffPIJM2fOBBqboDNnznDvvffyxRdfEBkZSXZ2Nv37mz15rpogERERb%2BSBJmj16tVNPrZq2rRppKdf%2BGOrqqqq7J8les65r6urqx2aoO8qKyvj5z//OTExMdx9990ABAYGcsMNN5CZmUn79u1ZuXIlDz30EBs2bKBbt24X89SapSZIREREgOY/tiosLMyp%2BwYHB1NTU%2BOw7NzXrVu3bvY%2Bu3btIjMzk/j4eJ555hn7AdSPPfaYw3oPPfQQ69atY/PmzYwfP96pepyhJkhERMQbeWAS1NzHVjmrZ8%2BenDx5kvLyckJDQwEoKiqic%2BfOtG3btsn6a9eu5de//jUZGRk8%2BOCDDrctXLiQO%2B%2B8k%2Buvv96%2BrLa21uFkzSbowGgRERFxW/fu3YmLi2Pu3LlUVlZSUlLC0qVLGTt2bJN1N23axOzZs1myZEmTBgjgf//3f3n66acpKyujtraWvLw8KisrueOOO4zWrCZIRETEG/n5mb%2B4afHixdTX1zNs2DCSkpIYOHAgqampAMTGxrJhwwYA8vLyOHv2LBkZGcTGxtovTz75JADPPPMM11xzDaNHjyYhIYEdO3bwyiuvEBIS4naN36XdYSIiIt7oCjxPUGhoKIsXL272tvz8fPu/33777R/MCQkJ4ZlnnjFaW3OuvC0oIiIicgloEiQiIuKNrsBJkLdREyQiIuKN1AS5TVtQREREfJLXTIKK2/QxGxj8Y2AN7wQnQZvPjcX2tB4xlgVwfUv4MzC65bvsM/wRKgf8N5kLa9kWuIVBLT8G/9PmcoGAVjajef38YTuQ4L%2BTXYa36Zlay4VXclZdLLCT7XX9oTb/gqu7psFwnniLolpzZ9sFoC4G%2BG821A2H2gKz2WfPmss60wfYxIYzd0LNHnO5ABwznOckTYLcpi0oIiIiPslrJkEiIiLyHZoEuU1NkIiIiDdSE%2BQ2bUERERHxSZoEiYiIeCNNgtymLSgiIiI%2BSZMgERERb6RJkNvUBImIiHgjNUFu0xYUERERn6RJkIiIiDfSJMht2oIiIiLikzQJEhER8UaaBLlNTZCIiIg3UhPkNm1BERER8UmaBImIiHgjTYLcpi0oIiIiPsnpJqi4uJi4uDiWLVvmsLyiooJhw4aRl5dHeXk50dHRxMbG2i9Dhw41XrSIiIjP8/Mzf/ExTu8Oi4iIYP78%2BWRmZtKnTx9uvvlmamtrSUtLIyYmhrS0NP7%2B97/TtWtXPvjgA0/WLCIiIj7YtJjm0hZMTEwkJSWFrKwsSktLmTVrFlarlXnz5mGxWNizZw8xMTGeqlVERETEGJcPjM7MzKSgoIDk5GRqa2tZu3YtQUFBAOzZs4dvvvmGu%2B%2B%2Bm/Lycvr06cOMGTPo0aOHS49x/PhxysrKHJaFde5MeIcOrpZ7fhERjteGXH/GaBzXXed4bVTbtuayWrd2vDaoXz%2BzedHRjtdG1cWay%2BrVy/FaxATTf6hGRjpem9TQYC7r3OuQi69HF7Rnj9k8V2gS5DaLzWazuXqnTZs2kZGRwYgRI3j22Wfty7OzswkPD2fKlCm0atWKRYsW8d5777Fx40bauvCCu2TJEvLy8hyWTUtNJT0z09VSRUREPOeqq%2BDYscvz2F98YT6za1fzmVcwl5ugo0ePct999zFmzBhWrVrFE088QVJSUrPrNjQ0EB8fz%2B9%2B9zuGDBni9GM0OwmaPt38JGj%2BfJgxA4qLjcWOPrPGWBY0ToAWLoSsLDh0yGg0f57xsbmw1q2hb1/YvRuqqszlAglZtxjNi46G116DiRNh/36j0Wyv628urFcvWLUKkpOhsNBcLsBnn5nNA7BYwPW/qS49X6/zrrvM5kVGQl4eTJsGRUVms01Pgp5/HtLS4OBBc7l79ly%2BJqi01Hxmly7mM69gLu0Oq6ysZOrUqQwePJiZM2cSGRlJbm4u0dHRREZG8vzzzzN%2B/Hi6fttJnj17lvr6egIDA10qKjw8nPDwcMeFX37ZeDGtuBg%2B/9xY3D6rsSgHhw7Bvn2GQ0%2BfNhxIYwNkOHfXLqNxdvv3eyC7Nt9wII0NUL4HcsU3FRR4JreoyHz22bNm86CxAbqcu7BM0u4wtzm9BRsaGsjJySEgIIDc3FwAkpKSGDlyJOnp6VitVj7%2B%2BGPmz5/P6dOnqaqqYs6cOVx99dXEx8d77AmIiIiIXAynm6CFCxeya9cu8vLyCAgIsC%2BfPXs2HTt2ZPr06SxdupSGhgYSExMZOHAgZWVlLF%2B%2BHH9/f48ULyIi4rN0niC3Ob07LDs7m%2Bzs7CbLAwICWL9%2Bvf3r7x/QLCIiInIl0meHiYiIeCMfnNyYpiZIRETEG6kJcpu2oIiIiPgkTYJERES8kSZBbtMWFBEREZ%2BkSZCIiIg30iTIbWqCREREvJGaILdpC4qIiIhP0iRIRETEG2kS5DZtQREREfFJmgSJiIh4I02C3KYmSERExBupCXKbtqCIiIgYceLECVJTU4mPjychIYGnn36a%2Bvr6ZtfdvHkzI0eOpF%2B/fgwfPpwPP/zQ4fbly5czaNAg%2BvXrx4QJEzh06JDxetUEiYiIeCM/P/MXN02fPp3g4GC2bNnC2rVr2bp1K6%2B%2B%2BmqT9Q4fPkx6ejqZmZl8%2BumnpKenM336dL766isA1q9fz4oVK3j55ZfZvn07vXv3JiMjA5vN5naN36UmSERERNx25MgRduzYwSOPPEJQUBDdunUjNTWVlStXNll3/fr1xMfHk5iYSMuWLbnrrrsYMGAAq1evBmDNmjUkJyfTs2dPAgICyM7O5tixY2zfvt1ozd5zTNA//%2BmZ3G83uCkHMNulNrLw57c8kfv/zEfefLPxyDNWz2zT7ds8kdtgPvKzz8xnmj6WIDYWdu6EuDjIzzeX2%2BCB7enjbEdLjGdaANvG/zaf23DWeCYbN5rPvFw8cEzQ8ePHKSsrc1gWFhZGeHj4Be974MABQkJC6NSpk31ZZGQkx44d49SpU7Rr186%2B/ODBg0RFRTncv0ePHhQWFtpvnzJliv02f39/unfvTmFhITfddNNFPbfmeE8TJCIiInY2LMYzV69eTV5ensOyadOmkZ6efsH7VlVVERQU5LDs3NfV1dUOTVBz6wYGBlJdXe3U7aaoCRIREREAxo0bx9ChQx2WhYWFOXXf4OBgampqHJad%2B7p169YOy4OCgrBarQ7LrFarfb0L3W6KmiAREREv5Im9xeHh4U7t%2BmpOz549OXnyJOXl5YSGhgJQVFRE586dadu2rcO6UVFR7N2712HZwYMHiYmJsWcdOHCAIUOGAFBXV8fhw4eb7EJzlw6MFhER8UINDeYv7ujevTtxcXHMnTuXyspKSkpKWLp0KWPHjm2y7qhRo9ixYwcbN26kvr6ejRs3smPHDkaPHg3Avffey%2Buvv05hYSFnzpzhd7/7HaGhocTHx7tX5PeoCRIREREjFi9eTH19PcOGDSMpKYmBAweSmpoKQGxsLBs2bAAaD5h%2B/vnneemllxgwYABLly5lyZIlREREADB27FgmT55MWloaN910E/v27eOll17C39/faL0Wm%2Bk33XuKJ8q0WDyTa5rqNM9bavVUnZ56d1j//lf%2Bu8N8/P/eEwfTemqTGn93WIsWcNYD7zhr0cJ8phPOnDGfGRBgPvNKpkmQiIiI%2BCQdGC0iIuKFdBot96kJEhER8UJqgtyn3WEiIiLikzQJEhER8UKaBLlPkyARERHxSZoEiYiIeCFNgtynJkhERMQLqQlyn3aHiYiIiE/SJEhERMQLaRLkPk2CRERExCc53QQVFxcTFxfHsmXLHJZXVFQwbNgw8vLyaGhoIC8vj9tvv53Y2Fjuu%2B8%2B8k1%2BjpCIiIgAV96nyHsjp5ugiIgI5s%2Bfz6JFi9i6dSsAtbW1pKWlERMTQ1paGkuXLuWdd97h1Vdf5dNPP%2BWOO%2B7gF7/4BbW1tR57AiIiIr5ITZD7XDomKDExkZSUFLKysli/fj2LFy/GarUyb948Ghoa%2BMMf/sBzzz1HREQEAA899BA333yzRwoXERERcYfLB0ZnZmZSUFBAcnIytbW1rF27lqCgIIqKijh16hSnTp3innvu4YsvvuD6669n5syZtGrVyqXHOH78OGVlZQ7LwkJDCQ8Pd7VcEWlObKzZvF69HK9FxON8cXJjmstNkJ%2BfH0lJSWRkZDBixAi6dOkCwMmTJwFYsWIFS5YsoWPHjuTl5fHQQw%2BxceNG2rZt6/RjrF69mry8PIdl09LSSM/IcLXcC7NYzGd6guo0z1tq9USdO3eazwRYtcozuab58P%2B9p565RzZpixZXfubZs2bz5JKy2Gw2myt3OHr0KPfddx9jxoxh1apVPPHEEyQlJbFnzx7Gjh3LK6%2B8wi233AJAQ0MDcXFxPPfcc9x%2B%2B%2B1OP8YlmwRZLODa0788VKd53lKrp%2BqMizOb16tXYwOUnAyFheZyP/vMXNY5Pv5/b/NAG%2BSpTWppMNxgtGjhmabFE82aE774wnxm167mM69kLk2CKisrmTp1KoMHD2bmzJlERkaSm5tLdHQ0kZGRtGzZ0uEgaJvNZr%2B4Ijw8vGnD4w2/tES8hafetVlY6LlsEXGg3WHuc/rdYQ0NDeTk5BAQEEBubi4ASUlJjBw5kvT0dKxWK3fffTfPPPMM//rXv6itrWXBggW0a9eOm266yWNPQERERORiOD0JWrhwIbt27WLdunUEBATYl8%2BePZv777%2Bf6dOn8/vf/54lS5YwceJEvv76a2JiYnj55ZcJDAz0SPEiIiK%2BSpMg97l8TNBl45Edzr59bIBx3lIneE%2BtnqrTz/DJ4mNjGw%2B27t/f7O4wT/yW9/H/ex0T9O9zTNCRI%2BYzr73WfOaVTJ8dJiIi4oU0CXKfmiAREREvpCbIffoAVREREfFJmgSJiIh4IU2C3KdJkIiIiPgkTYJERES8kCZB7lMTJCIi4oXUBLlPu8NERETEJ2kSJCIi4oU0CXKfJkEiIiLikzQJEhER8UKaBLlPTZCIiIgXUhPkPu0OExEREZ%2BkSZCIiIgX0iTIfWqCRHyRp357fvaZ2Tw/w8Pq2FjYuRPi4iA/32z2tm3msoKDoU8fKCiA6mpzuYDl1CmjebRtCzfdhGX7Njh92mx2SIi5rOBg6N0bCguNb1MGDDCbJ5eMmiAREREvpEmQ%2B9QEiYiIeCE1Qe7TgdEiIiLikzQJEhER8UKaBLlPTZCIiIgXUhPkPu0OExEREZ%2BkSZCIiIgX0iTIfWqCRERE5JKorq5mzpw5fPDBB9TX1zNs2DBmzZpF69atm11/06ZNLF26lJKSEkJCQrjnnntITU3F79tziA0fPpxjx47ZvwZYu3YtkZGRTtWjJkhERMQLeeMkaM6cOZSWlrJp0ybOnj3L9OnTWbBgAbNmzWqybkFBAY8%2B%2BijPPfcct99%2BO8XFxUyZMoXg4GAefPBBKisrKS4u5v3336dr164XVY%2BOCRIREfFCDQ3mL55UU1PD22%2B/TUZGBiEhIXTs2JGcnBzWrVtHTU1Nk/W/%2BOIL7r//foYMGYKfnx%2BRkZHccccdfPLJJ0BjkxQSEnLRDRBoEiQiIiLfOn78OGVlZQ7LwsLCCA8Pd%2Br%2BVquVr776qtnbampqqKurIyoqyr4sMjISq9XK4cOH%2BfGPf%2Byw/p133smdd97pkP33v/%2BdkSNHArBnzx6CgoIYP348Bw4coGvXrqSnpzNkyBCnagU1QSIiIl7JE5Ob1atXk5eX57Bs2rRppKenO3X/3bt3M3HixGZvy8zMBCA4ONi%2BLCgoCICqqqofzK2srCQzM5PAwEAmT54MgMVioU%2BfPvzyl7/kqquu4r333iM9PZ3XX3%2Bdfv36OVWvmiAREREBYNy4cQwdOtRhWVhYmNP3T0hIYP/%2B/c3etm/fPhYtWkRNTY39QOhzu8HatGlz3sxDhw6RkZFBx44dee211%2BzrpqSkOKw3atQo3nnnHTZt2qQmSERE5N%2BZJyZB4eHhTu/6clVERAT%2B/v4cPHiQvn37AlBUVIS/vz/du3dv9j6bN2/ml7/8JUlJSWRnZ9Oy5f%2B1LS%2B//DLXX389N998s31ZbW0tAQEBTtekA6NFRES8kLcdGB0UFMTw4cNZsGABFRUVVFRUsGDBAu6%2B%2B24CAwObrL9r1y7S0tKYOXMmM2bMcGiAAEpLS3nqqacoKSmhvr6etWvXkp%2Bfz09/%2BlOna1ITJCIiIpfErFmz6N69OyNHjuQnP/kJV199NU8%2B%2BaT99hEjRvDiiy8C8OKLL1JfX8/TTz9NbGys/XJuN9ijjz7KoEGDSE5OJj4%2BnjfeeINly5Zx7bXXOl2PxWaz2cw%2BRQ/xRJkWi2dyTVOd5nlLrd5SJ3imVj/Df6fFxsLOndC/P%2BTnm83ets1cVnAw9OkDe/ZAdbW5XIBTp8zmtW0LN93U%2BPxPnzabHRJiLis4GHr3hr17zW/TAQPM5jlp0ybzmd95M5ZPcPo3THFxMXFxcSxbtsxheUVFBcOGDSMvL8%2BhU4uNjeWGG24gOjqafNO/bERERETc5PSB0REREcyfP5/MzEz69OnDzTffTG1tLWlpacTExJCWlsa0adPs69fX1/PQQw9x9dVXExsb65HiRUREfJU3njH6SuPSu8MSExNJSUkhKyuL9evXs3jxYqxWK/PmzcNisTis%2B8ILL3DixAmWL19utGARERFRE2SCy2%2BRz8zMpKCggOTkZGpra1m7dq39ZEfnHD16lGXLlrFixQpatWrlclHNnrEyNNRjb9sTkSuU6Slyr16O1yZ95wRwbjv3Tplm3jHjNtPHbZ173iaf//ezTfDUNjV9fJFcUi43QX5%2BfiQlJZGRkcGIESPo0qVLk3VefPFFbr/9dqdPVvR9zZ6xMi2N9IyMi8r7Qd%2BbYF2xVKd53lKrt9QJ5mvdudNs3jmrVnkm17SePS93Bc674YbLXYFznPx0cad9%2BzlWl4MmQe5zuQk6evQoTz75JJMnT2bVqlWsWbOGpKQk%2B%2B1VVVW8%2B%2B67bu0Ga/aMlaGh5v%2BC8ZZ33qhO87ylVm%2BpEzxTa1yc2bxevRoboORkKCw0m/2HP5jLCgxsbIAOHACr1VwuwAU%2BnsBlwcGNDdA//2l%2BKtK2rbmswMDGBqioyPw2Fa/lUhNUWVnJ1KlTGTx4MDNnziQyMpLc3Fyio6PtZ3/cvHkzP/rRjxjgxlsGmz1jpbe8EIiIOZ56Z2lhoflsT%2BwWsVrN55p%2BG/s51dXms1u0MJsHntmml4kmQe5z%2Bi3yDQ0N5OTkEBAQQG5uLgBJSUmMHDmS9PR0ysvLAdi5cydxcXFNDpQWERERc7ztjNFXIqeboIULF7Jr1y7y8vIcPpdj9uzZdOzYkenTp1NfX09JSQmdOnXySLEiIiIipji9Oyw7O5vs7OwmywMCAli/fr3965deeslMZSIiInJevji5MU2fHSYiIiI%2ByeV3h4mIiMjlp0mQ%2B9QEiYiIeCE1Qe7T7jARERHxSZoEiYiIeCFNgtynSZCIiIj4JE2CREREvJAmQe5TEyQiIuKF1AS5T7vDRERExCdpEiQiIuKFNAlynyZBIiIi4pM0CRIREfFCmgS5T02QiIiIF1IT5D7tDhMRERGfpEmQiIiIF9IkyH1qggyzYTGeafFYrs14pohR27aZzQsObrz%2Bwx%2Bgutps9k03mcuKjYWdO2HSJMjPN5cL8OWXZvNafvsy0rMn1NebzT73/2WC37c7Prp3V/cgdmqCREREvJB6OfepCRIREfFCaoLcpwOjRURExCdpEiQiIuKFNAlyn5ogERERL6QmyH3aHSYiIiI%2BSZMgERERL6RJkPs0CRIRERGfpEmQiIiIF9IkyH1qgkRERLyQmiD3aXeYiIiI%2BCRNgkRERLyQJkHu0yRIREREfJImQSIiIl5IkyD3qQkSERHxQmqC3KfdYSIiIuKTnG6CiouLiYuLY9myZQ7LKyoqGDZsGHl5eVRUVJCVlUVCQgIJCQmkpqZy7Ngx40WLiIj4uoYG8xdPq66uZubMmSQkJBAXF8ejjz5KVVXVedefNWsWMTExxMbG2i%2BrV6%2B23758%2BXIGDRpEv379mDBhAocOHXKpHqeboIiICObPn8%2BiRYvYunUrALW1taSlpRETE0NaWhq5ubn4%2Bfnx4Ycf8uGHHxIQEMDMmTNdKkhERET%2BPc2ZM4fS0lI2bdrEX/7yF0pLS1mwYMF519%2BzZw9z5swhPz/ffhk3bhwA69evZ8WKFbz88sts376d3r17k5GRgc1mc7oel3aHJSYmkpKSQlZWFqWlpcyaNQur1cq8efOwWCwUFRVhs9nsFz8/P4KCglx5CBEREXGCt02CampqePvtt8nIyCAkJISOHTuSk5PDunXrqKmpabJ%2BbW0t//u//0tMTEyzeWvWrCE5OZmePXsSEBBAdnY2x44dY/v27U7X5PKB0ZmZmRQUFJCcnExtbS1r1661NzpTp07l8ccfJy4uDoBrr72W119/3dWHEBERkQu4Eg%2BMtlqtfPXVV83eVlNTQ11dHVFRUfZlkZGRWK1WDh8%2BzI9//GOH9QsLC6mvr2fx4sV89tlntG3blnvvvZeUlBT8/Pw4ePAgU6ZMsa/v7%2B9P9%2B7dKSws5KabbnKqXpebID8/P5KSksjIyGDEiBF06dLFfltDQwPjxo1j6tSpnD17lscffx1BuAUAAB5FSURBVJzp06ezcuVKlx7j%2BPHjlJWVOSwLCw0lPDzc1XJFxJsFB5vNCwx0vDYpNtZcVq9ejtcmtTT8puAWLRyvTfIz%2BN6dc1kmM%2BHK7ETc0Ozrb1iY06%2B/u3fvZuLEic3elpmZCUDwd36uzw1Rmjsu6PTp09x4441MmDCBZ599ls8//5y0tDT8/PxISUmhqqqqyd6mwMBAqqurnaoVLqIJOnr0KE8%2B%2BSSTJ09m1apVrFmzhqSkJMrKynjsscf48MMPad%2B%2BPQCzZ89m0KBB7N%2B/n%2BjoaKcfY/Xq1eTl5Tksm5aWRnpGhqvlXpjFYjbOaNp3cj0S7IFQzxTqGd5Sq7fUCeZr7dPHbN45PXuaz9y503zmqlXmMz0lJORyV%2BAc04donD5tNs8Fnui/mn39nTaN9PR0p%2B6fkJDA/v37m71t3759LFq0iJqaGlq3bg1g3w3Wpk2bJuvfeuut3Hrrrfavb7jhBiZNmsTGjRtJSUkhKCgIq9XqcB%2Br1WrPdoZLTVBlZSVTp05l8ODBzJw5k8jISHJzc4mOjsbf35%2B6ujpqa2v/L/zbvzj8/f1deRjGjRvH0KFDHZaFhYaCCwc7OcViMZ5p80Bj4YEyG3O58renx3hLrd5SJ3im1oICs3mBgY0N0IED8L1fnm6bNMlcVq9ejQ1QcjIUFprLBfjrX83mtWjR2ACdPAlnz5rNNjmx8/NrbIBqav7tpjcmNfv6GxZmJDsiIgJ/f38OHjxI3759ASgqKrLvxvq%2Bv/3tb5SXl3P//ffbl9XW1hL47fdFz549OXDgAEOGDAGgrq6Ow4cPO%2BxuuxCnm6CGhgZycnIICAggNzcXgKSkJPLz80lPT2fdunV069aNp59%2Bmt/85jcAzJ37/9u796go6/wP4O/hKpgGKpj9TBFUSA0dBC%2Bx2UayWnhDSdp01dLEVhDxgrJ5QVlCz8GwgLTDWq6upbnqSTdXTbd197gqVqgIYiDquoJxs5LLcP3%2B/iCwiUuD8x2feZz365w5jz7P8OHtjAyf%2BXyfmXkL3t7erf7j2uPq6tpy9KaWXwREJE8HxtodotPJr52RIbce0NgAya5bVye3XpP6evm1TdGsPKjXgj8ApvhntPr7VxIHBwe88MILSExMxDvvvAMASExMxIQJE5obm58SQiAhIQF9%2B/bFqFGjcP78eezYsaP5VefTpk1DcnIyxowZg379%2BiEpKQk9evSAr6%2BvwZkMXhxNSkrC%2BfPnkZKSAnt7%2B%2Bb9sbGx6N69OxYvXoxt27YBaHwV2W9%2B8xsIIZCamgor2WuwREREFk5trw4DGt/3x83NDRMnTsT48ePRu3dvrFmzpvl4UFAQtm7dCgAIDAxETEwMYmNjodVqsXz5ckRERGDy5MkAgJCQEMyZMwcLFy7EqFGjkJ2djffff79Dq08a0ZEX1CvJJOtBXA6TW9DCl25MQS05AdNkTU%2BXW8/RsfE8o8xM%2BZMgA1%2BNYhCttvEcIx8f%2BZOg27fl1rOxAbp3B0pL5U%2BCZJ4Yb2UFdO4MVFTI/23fpYvcegZ68035NePj5dc0Z/zsMCIiIhV6SFb1FMV1KiIiIrJInAQRERGpECdBxmMTREREpEJsgozH5TAiIiKySJwEERERqRAnQcbjJIiIiIgsEidBREREKsRJkPHYBBEREakQmyDjcTmMiIiILBInQURERCrESZDxOAkiIiIii8RJEBERkQpxEmQ8NkFEREQqxCbIeFwOIyIiIouknknQa6/Jrde3LxAbC6xbB9y4Ia3sy5UfSqsFAP36ARs2ADExwLVrUktjj2%2BivGKursDs2cCOHUBRkby6ADy2Lpdab/Bg4OBBYNJkDbKypJbG1Zon5BUbMgT4%2B9%2BBF18ELl2SVxeA%2BO9NqfUAQANAQCO35g8/SK0HIRq3FRXA3btya9%2B%2BLa%2BWzY8PzZ9/DtTVyasLAI89JreeVgt8/TUQGAhkZMitbWcnr9awYcDZs0BAAHD%2BvLy6AFBdLbeegTgJMh4nQURERGSR1DMJIiIiomacBBmPTRAREZEKsQkyHpfDiIiIyCJxEkRERKRCnAQZj5MgIiIiskicBBEREakQJ0HGYxNERESkQmyCjMflMCIiIrJInAQRERGpECdBxmMTREREpEJsgozH5TAiIiKySJwEERERqRAnQcbjJIiIiIgsEidBREREKsRJkPHYBBEREakQmyDjcTmMiIiILJLBTVBsbCz8/f1RWlqqt7%2Burg7Tp09HWFgYhBAAgPr6eoSHhyM5OVluWiIiIgLQOAmSfbE0BjdBMTEx6NGjB2JiYvT2Jycno6SkBBs3boRGo0FBQQHmz5%2BPzz//XHpYIiIiIlkMboLs7e2RlJSEc%2BfOYefOnQCA9PR0bN%2B%2BHZs3b4aTkxOuXbuG4OBgDB06FFqt1mShiYiILB0nQcbr0InR7u7uWLNmDdatWwdfX1%2BsXLkS0dHR8Pb2BgC4uLjg%2BPHj6NKlC86dO3ffoYqKilBcXKy3z6VLF7h263bfNVvo1Ut/K0k/ndRyePxx/a1Urq7yajXdNzLvox8NHiy3nru7/laq2iHyanl46G8tUZcucus5OupvZbKR%2BDoTa2v9rUyyn6B6eelvZbK1lVfL01N/K8v583LrdYAlNi2yaUTTiTwdEB0djaNHj2Ls2LHYtGlTq9f53e9%2BhxEjRiAiIqLDoZKTk5GSkqK3L3zhQkQsWtThWkRERCZjbw9UVyvyrSdOlF/z0CH5Nc3ZfT11CQ8Px6efforIyEjZeQAAoaGhCAgI0Nvn8v77QGysvG/SqxcQFga8/z5QWCit7EpdrLRaQOMEaNEi4N13gYICqaWx4ck/yyvWrVvjT%2BShQ0BZmby6ACbtmy21nrs7sHkzsHgxkJ8vtTQO1r4gr5iHB5CSAoSHA1evyqsLQBz%2Bu9R6AKDRAB1/SvULNc%2BekVvQ0RHw9gYuXgQqK%2BXWHjBAXi1ra8DJCfjuO6C%2BXl5dAAgMlFvPywv46CPglVeAnBy5tWVPgnbsAGbNAq5ckVdXQZwEGe%2B%2BmiArKyu9rWyurq5w/flSzd27jRfZCguBGzeklbsm%2BXG1SUEBcO2a5KLdiyQXRGMDVCS3blaW1HLN8vNNULvmkuSCaGyALpmgrhqY4mceaGyAZNeuq5NbD2hsgGTXzciQW69JTo782nZ2cusBjQ2QgktYZF74ZolEREQqxEmQ8dgEERERqRCbIOPdVxPUu3dvXPmFNdWml9ETERERmSNOgoiIiFRIjZOgyspKxMXF4R//%2BAfq6urw/PPPY%2B3atejcuXOL665ZswaHfvZyNZ1Oh6effhrbtm1DQ0MDhg8fDiEENBpN83VOnToFRwPfBoOfHUZEREQPRFxcHAoLC3H06FEcO3YMhYWFSExMbPW669evR0ZGRvMlOTkZXbt2xcqVKwEAeXl5qK2tRXp6ut71DG2AADZBREREqqS2d4yuqqrCoUOHsGjRIjg5OaF79%2B5YtmwZ9u/fj6qqqna/tqysDMuWLcObb76JAT%2B%2BHUVmZiY8PT1hZ8SrCLkcRkREpEKmaFpa/cQGF5eWb1vTBp1Oh2%2B//bbVY1VVVaitrcXAgQOb93l4eECn0%2BH69et48skn26ybmJiIIUOGYNKkSc37MjMzUV1djWnTpuHWrVvw8PDA0qVL4ePjY1BWgE0QERER/WjPnj0tP7EhPNzgT3%2B4cOECZs2a1eqxpjdY/ulylYODAwCgoqKizZo3b97EwYMHsXfvXr39nTp1gre3NyIjI/Hoo49i165dmDt3Lg4ePIgnnnjCoLxsgoiIiFTIFJOgVj%2BxwcXF4K8fOXJkm68ez87OxjvvvIOqqqrmE6GblsEeeeSRNmvu27cPWq22xaSo6dygJnPnzsX%2B/ftx8uRJzJw506C8bIKIiIgIQBuf2CBJv379YGtri7y8PAwdOhQAcPXqVdja2sLNza3Nrzt27Bhee%2B21FvuTkpIwbtw4DBo0qHlfTU0N7O3tDc7EE6OJiIhUSG0nRjs4OOCFF15AYmIiysrKUFZWhsTEREyYMAGdOnVq9Wvu3LmDq1evws/Pr8Wxb775BvHx8SguLkZNTQ1SUlJQXl6OwA58Ph6bICIiIhVSWxMEAGvXroWbmxsmTpyI8ePHo3fv3lizZk3z8aCgIGzdurX57//73/8AAD179mxRKyEhAX369MHkyZMxcuRIpKen48MPP4STk5PBebgcRkRERA/EI488gri4OMTFxbV6/LPPPtP7%2B1NPPdXmOUZOTk5ISEgwKg%2BbICIiIhVS4ztGmxsuhxEREZFF4iSIiIhIhTgJMh6bICIiIhViE2Q8LocRERGRRVLPJKiuzjT16uqk1q6tlVYKgH5M2bXRxvsy3JemN6eyt5dbV23q6%2BXVanqa19Agty4ATYPcegAAa2v5dTvwUleDNL1df5cugLW1aWrLYPXj89NOneQ/3TfiwyZbZWt7byu7dk2NvFpND6C1tXLrKoiTIONxEkREREQWST2TICIiImrGSZDx2AQRERGpEJsg43E5jIiIiCwSJ0FEREQqxEmQ8TgJIiIiIovESRAREZEKcRJkPDZBREREKsQmyHhcDiMiIiKLxEkQERGRCnESZDw2QURERCrEJsh4XA4jIiIii8RJEBERkQpxEmQ8ToKIiIjIInESREREpEKcBBmPTRAREZEKsQkynsHLYbGxsfD390dpaane/rq6OkyfPh1hYWG4c%2BcOVq5cCX9/f/j5%2BWH27Nm4fPmy9NBERERExjK4CYqJiUGPHj0QExOjtz85ORklJSXYuHEj3nzzTdy5cwd/%2B9vfcOrUKfj4%2BGDevHmorKyUHpyIiMiSNTTIv1gag5sge3t7JCUl4dy5c9i5cycAID09Hdu3b8fmzZvx6KOPQqPRIDIyEs7OzrCzs8PcuXNRUlKC69evmyo/ERER0X3p0DlB7u7uWLNmDdatWwdfX1%2BsXLkS0dHR8Pb2BgCkpqbqXf/IkSNwdHREv379OhSqqKgIxcXFevtcunSBa7duHarTrl699LeSuOuklsP//Z/%2BVioXF3m1nJ31txINHiy3nru7/laq6qfk1erfX39riRwd5dbr1El/K5OVxBfbNtWSWbPJsGFy63l66m9lqq2VV8vLS38rS0aG3HodYImTG9k0QgjR0S%2BKjo7G0aNHMXbsWGzatKnV65w4cQJLly5FbGwspkyZ0qH6ycnJSElJ0dsXvnAhIhYt6mhUIiIi09FogI7/GpVC8nN4AEBhofya5uy%2BmqD//ve/CAwMxOeff44%2BffroHRNCYMuWLUhLS0N8fDxefPHFDodqdRKUmip/ErRwIZCaKvVeX6b7o7RaQOMEKCoKSEoCbt2SWhqJvrvlFXN2BsaNA44eBe7ckVcXwKSPXpZaz90d2LwZWLwYyM%2BXWhoHq8fJK9a/f%2BP/z4ULgbw8eXUB4PBhufUAwNoaqK%2BXWzMnR269Tp0ADw/g6lVAJ3ls6%2BYmr5aVFeDgAFRVyX%2B6HxAgt56nJ7BjBzBrFnDlitzasidBH30EvPKK3P9XGRlsglTsvl4ib/XjiNbqZ6PaqqoqREVFITc3F7t27cKgQYPuK5SrqytcXV31d96923iRrbAQuHFDWrn8Cmml9Ny6Jf8XNvoW//J1OurOHaBYbt2sLKnlmuXnm6B2VabkgmhsgDJNUFcNTPWiCp1Ofm1TrE2Y4mzV8%2Bfl1mty5Yr82jU1cusBjQ2QgktYMnE5zHhS3ycoKioKt2/fxr59%2B%2BDk5CSzNBEREZFU0pqgrKwsfPHFF7Czs8Nzzz2ndywtLQ2%2Bvr6yvhUREZHF4yTIePfVBPXu3RtXfrb2O3jw4Bb7iIiIyDTYBBmPH6BKREREFomfHUZERKRCnAQZj5MgIiIiskicBBEREakQJ0HGYxNERESkQmyCjMflMCIiIrJInAQRERGpECdBxuMkiIiIiCwSJ0FEREQqxEmQ8dgEERERqRCbIONxOYyIiIgsEpsgIiIiFWpokH95UKqqqhAaGor9%2B/e3e70LFy7gpZdeglarRUBAAPbu3at3/MCBAwgMDMSwYcMwdepUZGRkdCgHmyAiIiJ6YHJzczFjxgycP3%2B%2B3et9//33mD9/PqZMmYJz584hPj4eCQkJuHjxIgDg7NmziIuLw4YNG3Du3DlMmjQJb7zxBqqqqgzOwiaIiIhIhdQ4CTp9%2BjRmz56N4OBgPP744%2B1e99ixY3BycsKMGTNgY2OD0aNHY%2BLEidi1axcAYO/evQgKCsLw4cNha2uLOXPmwNnZGYcPHzY4D0%2BMJiIiUiFTNC1FRUUoLi7W2%2Bfi4gJXV1eDvl6n0%2BHbb79t9ZiLiwu8vLzwxRdfwN7eHh9%2B%2BGG7tXJzczFw4EC9ff3798df//pXAEBeXh6mTZvW4nhOTo5BWQE1NUE7d0otV1RUhD3JyQhdtMjgO9cQ7a9udlxRURGSk/cgOjpUas5GEdIqNd%2BeofJzXpUXE8C92/RPfzLFbVogrVLzbbpzpwlyylVUVIQ9e/bIv//9/OTVgmn/n8pUVFSEPR98YJqc1dVSyzXfpocOmf9tmpyM0CNHzDpnRwghv2Zy8h6kpKTo7QsPD0dEhGEPxBcuXMCsWbNaPZaamoqxY8canKWiogIODg56%2Bzp16oTKykqDjhvCYpfDiouLkZKS0qLjNTfMKZ9asqolJ6CerMwpn1qyqiWn0ppOVv7pJTQ01OCvHzlyJK5cudLqpSMNEAA4ODhAp9Pp7dPpdOjcubNBxw2hnkkQERERmZSrq6vZTMoGDhyIU6dO6e3Ly8vDgAEDAAADBgxAbm5ui%2BNjxowx%2BHtY7CSIiIiIzFdgYCBKSkqwfft21NbW4syZMzh06FDzeUAhISE4dOgQzpw5g9raWmzfvh2lpaUIDAw0%2BHuwCSIiIiKzEBQUhK1btwIAnJ2d8cEHH%2BDIkSMYOXIkVq1ahVWrVmHUqFEAgNGjR2Pt2rWIjY3FiBEj8NlnnyEtLQ1OTk4Gfz/r2NjYWFP8Q9Sgc%2BfOGDFiRIfWD5XAnPKpJatacgLqycqc8qklq1pyWorZs2fjySef1Ns3Y8YM%2BPr6Nv%2B9Z8%2BeCAkJQVhYGGbNmtXi%2Bl5eXpg5cyYWLFiA6dOn47HHHutQBo0Qpji/nIiIiMi8cTmMiIiILBKbICIiIrJIbIKIiIjIIrEJIiIiIovEJoiIiIgsEpsgIiIiskhsgoiIiMgisQkiIiIii8QmiIiIiCwSmyAiIiKySGyCzIhOp0NOTg6qq6tbHPvqq68USGSY8vJyHD9%2BHF9%2B%2BSXq6uqUjvOLvv76a6UjtKu8vBwnT57EmTNnUF9fr3ScFmpra5vv5/LycvzrX//CyZMnUVNTo3Cyey5cuKB0hPuWm5uL69evKx1D1dT6WEoKEGQWLl%2B%2BLPz9/YWnp6fQarXi008/1Tuu1WoVStbSzZs3xcyZM0VkZKTIz88X/v7%2BQqvViqFDh4opU6aI4uJipSO2y8/PT%2BkIegICApr/nJeXJ5555hnh4%2BMjvL29RVBQkCgoKFAwnb6MjAwxcuRIcenSJfHNN9%2BIZ555Rmi1WjFs2DDx7LPPiry8PKUjCiGE8PT0FKtXrxY1NTVKR2lXQUGBmDNnjnjjjTdESUmJmDFjhvD09BReXl4iNDTU7H%2BWzJGaHktJeQ/9B6impKT84nXCw8MfQJL2vfrqq9BqtXjttddw5MgRxMfHIyEhAePHjwcAaLVaZGRkKJyyUXh4OBwdHVFRUYFLly5h/PjxWLFiBerq6rB%2B/XrodDokJiYqHRMBAQHQaDQt9hcUFODxxx8HAJw4ceJBx2rhp/dtWFgY%2Bvbti5iYGNTV1SEuLg6lpaVITU1VOGWj0NBQjB07FnPnzsXrr7%2BOQYMGYcmSJWhoaEBiYiKys7Px5z//WemYGDp0KLRaLUpLSxEfHw9vb2%2BlI7UqPDwcdnZ20Gg0yMrKgru7O9auXQsbGxu89dZbAIBNmzYpnLIRH0vpYWSjdABTy8nJwYkTJ6DVamFtbd3ieGu/JJWQnZ2NtLQ02NjYICQkBM7Ozli%2BfDnc3Nzg5eVlNjkBID09Hf/%2B97/xww8/YMyYMYiKioKVlRXs7OwQExODcePGKR0RADBx4kRs27YNs2fPRv/%2B/QEAQgjExcWZxYN1k5/etxcvXkRSUhI0Gg1sbW2xYsUKPPvsswqm05eXl4fdu3dDo9EgOzsbW7ZsgUajgbW1NaKiojB69GilIwIArK2tsW3bNrz33nuYOXMmfv3rX2PWrFnw9fVVOpqeL7/8Ev/85z9RX18PX19ffPzxx3B2dgYArFu3rvkXtzngYyk9jB76Jujdd9/F66%2B/Dq1Wa1a/%2BH7O1tYWlZWV6Nq1KwDg%2Beefx7x58xAREYF9%2B/bB3AZ2Go0GLi4uCAoKgpXVvVPLampq0NDQoGCye6KiojBq1CisXr0abm5ueOmllwAAGzZsQHBwsMLpWtejRw/U1NTA0dERQGPTZmNjPj%2BmXbp0wc2bN9GnTx/06tULZWVleOyxxwAAxcXFcHJyUjjhPdbW1oiIiMDkyZOxdetWzJs3D926dYOvry969uyJpUuXKh0RQOPPUtPF1tZWb785nRPGx1J6GD30J0ZbWVkhLi4Of/nLX1BeXq50nDb96le/QnR0NHJycpr3/f73v4eHhwfmzJljNo0FAPj4%2BCAhIQH19fVITEyEnZ0dACAzMxORkZF47rnnFE54z%2BjRo7F7924cPnwYkZGRuHv3rtKRWqisrMTYsWOxePFiODo6Ii0tDUDjst2qVavg5%2BencMJ7QkJCsGDBApw%2BfRrz58/HsmXLcObMGZw8eRLz5s3DhAkTlI7YQp8%2BffDWW2/hP//5D5YvX46uXbsiNzdX6VgAgKeffhqrVq3C6tWrYW9vj9TUVJSVlaGwsBArVqyAj4%2BP0hGb8bGUHkYP/TlBTa5fvw5XV9fmZ9hNGhoa9CYZSvnuu%2B/whz/8AdbW1khOTm7er9PpsHjxYpw8eRKXL19WMOE9BQUFCA8Px%2B7du5sbIAAYP348%2Bvfvjz/%2B8Y9mNREAGicq7733Hg4cOICysjJ8/fXXZnPfl5aWIjMzs/nStWtXJCYmYuPGjTh9%2BjS2bNmCXr16KR0TQOPtmJqaih07duDu3bvNz6ptbGwwYcIExMXF6U0zlPJL533U19e3uqTzoJWVlWH9%2BvXIz8/HggULIITAihUrUF9fj969e%2BODDz7AE088oXRMPW09lpoLNT2WkhlQ5nzsByc1NbXNYyUlJWLmzJkPME3bfimnj4/PA0zTvray6nQ6s79N09PTxerVq0VpaalZ5xRCiPLycrO6PYW4l7W%2Bvl7k5eWJr776SmRmZoq7d%2B%2BaVdb2fp7M/b4vLi4W2dnZZnV7CiFEWFiY%2BOGHH5SO8YvCwsLE999/3%2BbxrKysB5iGzJ3yT4NNbP/%2B/ViyZEmL94u4ePEipk6dioqKCoWS6WvK%2BfP3Wrl48SKmTZuGvn37KpSspbZu0ytXrmDatGlmd5v%2BNKefnx9CQkIQHBxs1jkB4OrVq2Z1ewL3stbV1cHDwwM%2BPj4YMmQI8vPzzSprez/35njf//TnvkePHqitrTWr2xNonFpNmTIFWVlZSkdpV1lZGYKDg9vMOWjQoAeciMya0l2YqZWWloqXX35ZBAcHi9u3bwshhPjkk0/EkCFDRExMjKiurlY4YSO15BRCPVnbyvnUU0%2BpIqe53Z5CqCcrc8pXV1cn3n77bTFs2DDx8ccfKx2nTWrJSebhoW%2BChBCiurparFy5Uvj7%2B4slS5YIb29v8cknnygdqwW15BRCPVmZUz61ZGVO0zh79qwICAgQy5YtEzdv3hS3bt1qvpgTteQkZVnMidG3b9/GK6%2B8goKCArz66qtYsWKF0pFapZacgHqyMqd8asnKnKaRk5ODl19%2BuXm5UQgBjUZjdiccqyUnKeehPycIAM6cOYOpU6fC3d0d7777Lvbv34%2BYmBiz%2BqwjQD05AfVkZU751JKVOU1j165d%2BO1vf4tx48bh2LFjOH78OE6cOIHjx48rHU2PWnKSwpQdRJleWlqaGDx4sHj77bdFQ0ODEEKIGzduiKCgIBESEtK8Dq80teQUQj1ZmVM%2BtWRlTvlKS0tFWFiY0Gq14sCBA0rHaZNacpJ5eOiboOHDh4vjx4%2B32F9eXi7CwsLE6NGjFUjVklpyCqGerMwpn1qyMqd8/v7%2BYsqUKSI/P1/pKO1SS04yDw99E3Tt2rU2jzU0NIhNmzY9uDDtUEtOIdSTlTnlU0tW5pQvNjbWrF6t1ha15CTzYDEnRhMRERH9lEWcGE1ERET0c2yCiIiIyCKxCSIiIiKLxCaIiIiILBKbICIiIrJIbIKIiIjIIrEJIiIiIovEJoiIiIgs0v8Ds3ho0Ekc0A4AAAAASUVORK5CYII%3D" class="center-img">
    <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAkEAAAH1CAYAAADxtypTAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAPYQAAD2EBqD%2BnaQAAADl0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uIDIuMS4wLCBodHRwOi8vbWF0cGxvdGxpYi5vcmcvpW3flQAAIABJREFUeJzs3XtclHX6//HXoMjBQ5iCeUyWBEosEczKLE99yzxkZVh0ssJaQUAXO1CbGm6mv7VMJUvddi2TVkPdLXNztzJrW9MSKLUwRTyUrJwy5SQi8/uDZdYRzMH5jDo77%2BfjMY9xPnPf11xzIzMX1/2579titVqtiIiIiHgYr/OdgIiIiMj5oCJIREREPJKKIBEREfFIKoJERETEI6kIEhEREY%2BkIkhEREQ8koogERER8UgqgkRERMQjqQgSERERj9T8fCcgciHLzc0lMzOTTZs2cejQIY4dO8bFF19Mjx49GDhwIGPGjMHX1/d8pykiImfBostmiDRu/vz5vPrqq9TW1tKqVSu6deuGt7c3RUVFHDx4EICOHTvyyiuv0LNnz/OcrYiINJWKIJFGrFq1iqeffhp/f39eeOEFbrrpJpo1a2Z7Pi8vj6effpqcnBzatm3LunXruPjii89jxiIi0lSaEyTSiNdeew2AJ554gltuucWuAAIICQnh1VdfpV27dvz000%2B8%2Beab5yNNERFxgoogkVMcOXKE/fv3A3DVVVeddrmLL76YoUOHAvDNN9%2Bck9xERMQcTYwWOUXz5v/9tdiwYQNXXHHFaZdNTEzkgQceoF27draxp556ijVr1pCamsqAAQOYO3cuX375JdXV1Vx66aXcfvvt3H333fj4%2BDQa88svv2TZsmVkZWVx%2BPBh2rRpQ%2B/evbn//vu59tprG13nyJEj/PnPf2bjxo3s3r2bsrIy/Pz86NatG4MGDeKBBx7goosuslsnLCwMgM8//5xZs2bx0Ucf4eXlRc%2BePfnjH//Ib3/7W9asWcPvfvc7%2Bvbty4IFC/jiiy84evQoXbp04a677mLcuHFYLBb%2B/ve/88Ybb/Ddd99RW1tLeHg4EyZM4MYbb2yQa1VVFatWreLDDz9k586dHDlyhBYtWtCpUyeuv/56HnroITp06GC3zuDBg/nxxx9Zt24dJSUl/OEPf%2BDrr7%2BmoqKCLl26MGzYMB555BFatmx52p%2BViMipNCdIpBH33HMPWVlZWCwWbrvtNsaMGUOfPn0a7BZrTH0RdMcdd7B%2B/XoqKiro0aMHNTU17NmzB4CoqCgWLVpE69at7dadM2cOS5YsAeCiiy6iS5cuFBYWUlRUBEBcXByPP/643Tp79%2B5l3LhxFBQU0Lx5c7p164afnx8//vgjhw8fBiA4OJhVq1bZFQn1RVCfPn3Izs4mNDSU0tJS%2BvXrx4svvmj3Pv72t79RU1NDSEgIJSUltnweffRRLBYLixYtok2bNnTt2pX8/HwqKiqwWCwsXryYG264wfaapaWlPPjgg3z//fdYLBa6detG69atOXTokC1mu3btWL16NZdccoltvfoi6KGHHmLp0qW0aNGC7t278/PPP/Pvf/8bgMjISJYvX%2B7Qz0hEBACriDSwY8cOa%2B/eva2hoaG2W58%2Bfazjx4%2B3Llq0yJqTk2M9ceJEo%2Bs%2B%2BeSTtnUGDRpk/fbbb23PZWVlWa%2B77jpraGio9dlnn7Vb7%2B2337aGhoZao6OjrX/9619t47W1tdb333/fls/KlSvt1rvvvvusoaGh1piYGOuhQ4fs1luzZo01PDzcGhoaan3rrbfs1qvPMSIiwrplyxar1Wq1njhxwvrTTz81eB/33HOPtbCw0LbMU089ZQ0NDbWGh4dbw8LCrK%2B//rpte5SWllpHjx5tDQ0Ntd53332NbpubbrrJmp%2Bfb/fcp59%2Bar3qqqusoaGh1lmzZtk9N2jQIFsuTz31lPXIkSO29/jWW2/ZnvvHP/7R6M9ERKQxmhMk0ogrrriCd955h6ioKNtYWVkZGzdu5MUXXyQmJobrr7%2BeuXPnUllZ2WgMLy8vFi5cyOWXX24bi4yMZPbs2QC88847HDp0CIDq6moWLFgAwMyZMxk1apRtHYvFwq233mrrAC1YsICamhoASkpK2LVrFwAzZswgKCjIbr3Ro0dz9dVXA7Bz585G8xw2bBh9%2B/a15RwQEGD3fPPmzXnppZcIDAy0LfPoo48CUFtby2233cbDDz%2BMl1fdx0nbtm154IEHAPj2229tcWpqavjqq6%2BwWCykpqbSvXt3u9cZMGAAt956KwDff/99o7mGh4czc%2BZMWwfNYrFw77332rpaW7dubXQ9EZHGqAgSOY3LLruMjIwM/vKXvzBx4kQiIyPx9va2PV9SUsJrr73GqFGjbLtkTnbNNdcQHh7eYPz666%2BnS5cu1NbWsmHDBgCys7MpLi6mZcuWDBkypNF8Ro0ahZeXF4cOHbIVF%2B3ateOLL77g66%2B/JjQ0tME6J06coFWrVkDdXJzGnFzoNSYsLMxu1xRA586dbf9ubN5PfTFWVlZmG2vevDkffvghX3/9NQMHDmywjtVqxd/f/xdzHThwIBaLpcH4r371KwCOHj36i%2B9FRORkmhgtcgaXX345l19%2BOYmJiVRWVpKVlcU///lP/vrXv1JSUsL%2B/ftJTk5mxYoVdutdeeWVp40ZFhbGDz/8wN69ewFs3Zzjx49z7733nna9Zs2aUVtby549e%2Bzi%2B/r6UlBQwNdff83%2B/fs5cOAAeXl5fPfdd1RUVAB1XZvG1Hd4Tqdjx44Nxlq0aGH7d9u2bRs8f/Lk8lP5%2BPhQUlJCTk4Oe/fu5YcffmDPnj189913/Pzzz7%2BY68mdrpPVn7X7xIkTp38jIiKnUBEk0gR%2Bfn7079%2Bf/v37k5yczNNPP837779PTk4OO3bssDtz9KlHY52svuNx5MgR4L8djOrqarKyss6YR/16AHv27OH//b//x8aNG%2B2Kh1atWhEdHU1hYSG5ubmnjXWmy374%2Bfn94vP1u8EcUVRUxOzZs/nggw84fvy43Wv06tWLEydO/OIurZOLr8ZYdZyHiDSBiiCRU0ydOpUvvviC22%2B/nQkTJpx2OV9fX9LS0vj73//O8ePHyc/PtyuC6jswjanfTVR/aH19odGzZ09Wr17tcK4lJSXcd999lJSU0KlTJ2JiYrjiiiv41a9%2BRZcuXbBYLKSkpPxiEXSuHDt2jAcffJC8vDwCAgK45557iIiIICQkhG7dutGsWTPmzp2reT0ics6oCBI5xbFjx9i3bx8ffvjhLxZBUNdtadmyJYcPH25w2Yz6XVyNqS9KLrvsMqDuEHaoO9y9pqam0d1JVquVzZs3c8kll9CpUydatGjBqlWrKCkpISAggFWrVjV66Y76ydfn24cffkheXh7NmzdnxYoVDSZGA43OrRIRcRVNjBY5Rf2RWdu3bz9jV%2Baf//wnhw8fJiAgoMHZpT/99FPbuW9OtmHDBgoKCmjRogWDBw8GoG/fvrRu3Zry8vLTvuZ7773Hgw8%2ByLBhw2zFwg8//ABAp06dGi2Adu/eTU5ODnD%2B58vU59qyZctGC6Di4mI%2B%2BeQT4PznKiKeQUWQyCn69%2B/PzTffDMBvf/tbnn/%2BedsXeL1jx46xatUqJk2aBEBycnKDsxVXVFQQHx9PQUGBbWzz5s2kpqYCdScarD/U29/f33bY%2BfPPP8%2BqVavs5vd8%2BOGHTJs2Dag7pL1bt27Af4%2BKys3NZf369bblrVYrn376KXFxcba5N6c7lP9cqc/1559/5o033rCbv5OTk8NDDz1kO7nj%2Bc5VRDyDdoeJNGLOnDn4%2B/vzl7/8hTfffJM333yTTp060a5dO44dO8bevXuprq7G29ublJQUYmNjG8To3r073333HUOHDiU0NJSKigrb0WAjRozgscces1t%2B/PjxHDhwgJUrV/L000/z%2B9//ni5dunDo0CEKCwuBurM7/%2B53v7OtM2bMGDIyMti3bx9JSUl07tyZtm3bUlBQQElJCd7e3lx99dVs2bLlvO8WGzx4MJGRkWRnZzNz5kyWLFlChw4dKCoq4tChQ1gsFq677jr%2B9a9/UVhYiNVqbfRweBERU1QEiTSiRYsWzJo1i3vvvZd169axefNmDh06RG5uLn5%2BfgQHB3P99dczZswYW4fjVL169WLOnDnMnz%2BfrVu30rx5c66%2B%2Bmruuece20kBT2axWJgxYwY333wzf/7zn8nJyeG7777Dx8eH3r17M2LECMaOHWt3hFSrVq3IzMxkyZIlbNiwgR9%2B%2BIHi4mIuueQSBg4cyIMPPoi/vz9Dhw4lNzeXgwcP0qlTJ5dtt1/SrFkzli5dyrJly3j//fc5cOAA33//PYGBgdx6663ce%2B%2B99OzZk379%2BnH48GGysrLOeA4jERFn6NphIobVX3Nr5MiRzJkz53ynIyIip6E5QSIiIuKRVASJiIiIR1IRJCIiIh5JRZCIiIgYVVpayk033cTmzZtPu8zGjRsZOXIkvXv3ZtiwYbYLStdbsmQJN9xwA7179%2Bb%2B%2B%2B9nz549xvNUESRi2KxZs9i5c6cmRYuIR9q6dStjx45l//79p11m7969JCYmkpyczFdffUViYiKTJk2yncpjzZo1LFu2jNdff53NmzfTs2dPkpKSjF8fUEWQiIiIGLFmzRqmTJnC5MmTz7hcdHQ0Q4cOpXnz5tx666307duXFStWALBy5UpiY2Pp0aMHPj4%2BpKSkcPDgwV/sLJ0NFUEiIiICQGFhITt27LC71Z%2Bs1RHXX389//jHPxo9F9rJdu/eTWhoqN3YZZddZruu4qnPe3t70717d%2BMXg3afkyWaPnNscDDs2gU9ekB%2BvrGwFsy26lyUJgAff2wulq8vXHMNfPEFVFWZiwvw8MNm43XtChs2wKBBcOCA2dj5BJsL5spEXbBvXdyE6V%2BowECYNQueegoauVaeU0x%2BmAQFwdy5MHkyNOFL3SFvv202nqNccEb1FfPnk56ebjc2ceJEEhMTHVo/MDDQoeXKy8vx8/OzG/P19aWiosKh501xnyLItIAAaNas7v4C5iZp0rx53e9jIxc/v%2BC0aVO3Tdu0Od%2BZnIHbJPofFgu4w7lXladZ/v7g5VV3fyFzlzzPs7Fjx9ou7FzP0cKmKfz8/Kg6pcitqqqyXYPxTM%2Bb4gZfWSIiItKAl/kZLUFBQQQFBRmPe6rQ0FB27NhhN7Z7924iIiIA6NGjB7t27WLQoEEAHD9%2BnL179zbYheYszQkSERGRc2rUqFFs2bKFdevWUVNTw7p169iyZQu33XYbAHfeeSdvvfUWubm5HDt2jBdffJH27dsTHR1tNA91gkRERNyRCzpBrhQZGclzzz3HqFGjCAkJ4ZVXXmHOnDk888wzdO7cmQULFhAcXDevcsyYMRw9epSEhARKS0vp1asXixYtwtvb22hOKoJERETc0QVeBO3cudPucXZ2tt3jAQMGMGDAgEbXtVgsPPzwwzxseiL/KS7sLSgiIiLiIuoEiYiIuKMLvBPkDrQFRURExCOpEyQiIuKO1AlymoogERERd6QiyGnagiIiIuKR1AkSERFxR%2BoEOU1bUERERDySOkEiIiLuSJ0gp6kIEhERcUcqgpymLSgiIiIeyeEiKD8/n6ioKBYvXmw3XlpaypAhQ0hPT7eNnThxgokTJ7JgwQJzmYqIiMh/eXmZv3kYh99xcHAws2fPZt68eWzatAmA6upqEhISiIiIICEhAYCDBw/y6KOP8o9//MM1GYuIiIgY0KSyb%2BjQocTFxTF58mQKCgqYNm0aVVVVzJo1C4vFQn5%2BPrfffjtXXXUVkZGRrspZRERE1AlyWpMnRicnJ7N9%2B3ZiY2Oprq4mMzMTPz8/AAIDA/nwww9p3bo1X3755VknVVhYSFFRkd1YYHAwQQEBZx2zgfBw%2B3tDTJd%2BLkoTgFatzMXy97e/N6lnT7PxQkLs780ymKxrExVPdemlZuN17Gh/b9KxY%2BZidepkf2/K3r1m4zWFBxYtplmsVqu1qSutX7%2BepKQkhg8fzksvvdToMvfffz9XX301iYmJTU5qwYIFdnOMACbGx5OYnNzkWCIiIi5zzz3w9tvn57VdUXgWFJiPeQFrcido//79TJ06lXHjxpGRkcHKlSuJiYkxmtTYsWMZPHiw3VjgyJHwxhvmXiQ8HDIyIDYWcnONhe1DlrFY4LI0AVi0yFwsf/%2B6js2OHVBRYS4uwHPPmY0XEgLz5kFyMuTlmY29lhHmgrky0ffeMxsPwGKBpv9Nde55ep6mf6E6doTHHqv7QDH9BWq6E5SYCAsWwMGD5uKeT%2BoEOa1JRVBZWRkTJkxg4MCBpKamEhISQlpaGmFhYVx11VXGkgoKCiIoKMh%2BMD/fWHw7ubmQnW0snLlI9gynCUBZmdl4UFcAmY67Y4fZePXy8lwR2wXJuiZR8VT79rkmbkGB%2BdhVVWbjQV0BdD53YckFxeEysra2lilTpuDj40NaWhoAMTExjBw5ksTERIqLi12WpIiIiJxCE6Od5vA7njt3Ljk5OaSnp%2BPj42Mbnz59Ou3atWPSpEnU1NS4JEkRERE5hYogpzm8OywlJYWUlJQG4z4%2BPqxZs6bB%2BLJly5zLTERERMSFdO0wERERd%2BSBnRvTtAVFRETEI6kTJCIi4o7UCXKaiiARERF3pCLIadqCIiIi4pHUCRIREXFH6gQ5TVtQREREPJI6QSIiIu5InSCnqQgSERFxRyqCnKYtKCIiIh5JnSARERF3pE6Q01QEiYiIuCMVQU7TFhQRERGPpE6QiIiIO1InyGluUwRZsBqNFwlkAX3IIttgXCsWg9GgPtMs%2BoDRTAE%2BNhirFdCXvnwJlBmMC/k8bDQe9ATWspYRwA6jkYPJNxarLksYwVrDWUK%2B4d8ncR8P8Sej8S4FpgPTmc4%2Bo5GhymCs7sALQCovsNdgXIC3DceTc8dtiiARERE5iTpBTlMRJCIi4o5UBDlNW1BEREQ8kjpBIiIi7kidIKdpC4qIiIhHUidIRETEHakT5DQVQSIiIu5IRZDTVASJiIiIESUlJTz77LNs2bKFZs2aMWrUKJ588kmaN7cvN%2BLi4ti6davdWEVFBWPHjiUtLY3i4mL69%2B%2BPv7%2B/7fm2bdvy8ccmz2%2BnIkhERMQ9XYCdoEmTJtGhQwc%2B%2B%2BwziouLmTBhAkuXLiUuLs5uuT/84Q92jzMzM0lPT2fixIkAbNu2jc6dOxsvek514W1BERERcTv79u1jy5YtPP744/j5%2BdG1a1fi4%2BNZvnz5L663Z88eZsyYwZw5cwgKCgLqiqCIiAiX56xOkIiIiDtyQSeosLCQoqIiu7HAwEBbcfJLdu3aRUBAAB06dLCNhYSEcPDgQY4cOUKbNm0aXe%2B5555j9OjRREdH28a2bdvGzz//zIgRIyguLqZXr148%2BeSTXHbZZWf5zhqnIkhERMQduaAIWrFiBenp6XZjEydOJDEx8YzrlpeX4%2BfnZzdW/7iioqLRIuirr77i66%2B/Zs6cOXbjbdq04bLLLmP8%2BPG0aNGCefPm8dBDD7Fu3Tpat27d1Ld1WiqCREREBICxY8cyePBgu7HAwECH1vX396eystJurP5xy5YtG11nxYoVDBs2rMFrvPjii3aPU1NTWbVqFV999RWDBg1yKB9HqAgSERFxRy7oBAUFBTm066sxPXr04PDhwxQXF9O%2BfXsA8vLyuOSSSxrt3tTU1PDRRx/xyiuv2I2XlZXxyiuvcN9999G5c2cATpw4QU1NDb6%2BvmeV2%2BloYrSIiIg4rXv37kRFRTFz5kzKyso4cOAACxcuZMyYMY0uv3PnTo4dO0afPn3sxlu1asW//vUvZs%2BezdGjRykvL2fGjBl06dLFbt6QCSqCRERE3JGXl/mbk%2BbPn09NTQ1DhgwhJiaGAQMGEB8fD0BkZCTvvvuubdkDBw5w0UUX4ePj0yDOwoULqa2tZejQoQwYMICioiKWLFmCt7e30zmeTLvDRERE3NEFeJ6g9u3bM3/%2B/Eafy87Otnt8yy23cMsttzS6bOfOnRtM0HYFh7dgfn4%2BUVFRLF682G68tLSUIUOGkJ6ezk8//cRTTz1F//796du3Lw8%2B%2BCDfffed8aRFREREnOVwERQcHMzs2bOZN28emzZtAqC6upqEhAQiIiJISEjgmWee4aeffmLt2rV8/vnn9OnTh7i4OCoqKlz2BkRERDzSBbg7zN006R0PHTqUuLg4Jk%2BeTEFBAdOmTaOqqopZs2YBYLFYSE5Opm3btrRo0YJHHnmE4uJi9u7d64rcRURERM5ak%2BcEJScns337dmJjY6muriYzM9N2MqRTD3P74IMP8Pf3Jzg4uEmv0dgZK4ODAwkIOLvD9hoTHm5/b06k2XCuSxRatTIXq/4idydd7M6Ynj3NxgsJsb83yGSmLkxTPNill5qN17Gj/b1Jx46Zi9Wpk/29Kef1b3wP7NyYZrFardamrrR%2B/XqSkpIYPnw4L730UqPLfPTRR6SkpDB9%2BnRGjx7dpPgLFixoMCEqPn4iyclnPmOliIjIuXLPPfD22%2BfpxUeONB/zvffMx7yANbkTtH//fqZOncq4cePIyMhg5cqVxMTE2J63Wq28%2BuqrLFmyhJkzZ3Lrrbc2OanGzlg5cmQgb7zR5FCnFR4OGRkQGwu5uebiZtHnzAs1hasSBVi0yFwsf/%2B6js2OHWB6Dthzz5mNFxIC8%2BZBcjLk5RkNPYK1xmK5ME3Wvtfkv33OzGKBpv9Nde55eJ7Tn7MYjdexIzz2WN3HSUGB0dDGO0GJibBgARw8aC6uuLcmFUFlZWVMmDCBgQMHkpqaSkhICGlpaYSFhXHVVVdRWVnJ5MmT2bVrF8uXL%2BeKK644q6QaO2Nlfv5ZhTqj3Fw45ag9JxkN9l/mE4WyMrPxoK4AMh13xw6z8erl5RmP7YpMXZCmeLB9%2B1wTt6DAfOyqKrPxoK4A%2Bp%2BZpqrdYU5zeAvW1tYyZcoUfHx8SEtLAyAmJoaRI0eSmJhIcXExkydP5t///jerVq066wJIRERE5FxwuBM0d%2B5ccnJyWL16td3ZHadPn87dd9/NTTfdREVFBS1atGhwcbMlS5YYP9W1iIiIR1MnyGkOF0EpKSmkpKQ0GPfx8WHNmjVGkxIREZEzUBHkNG1BERER8Ui6dpiIiIg7UifIadqCIiIi4pHUCRIREXFH6gQ5TUWQiIiIO1IR5DRtQREREfFI6gSJiIi4I3WCnKYtKCIiIh5JnSARERF3pE6Q01QEiYiIuCMVQU7TFhQRERGPpE6QiIiIO1InyGkqgkRERNyRiiCnaQuKiIiIR1InSERExB2pE%2BQ0tymCPv7YbLxWreruFy2CsjKTkd0mURg82FysyEjIyoLHHoPsbHNxgewsq9F4fn4QDuTOWUtlpdHQ5FdtMhesZUvgStbO/AbKy83FBeAaw/HEXfwp9AWzATt0AB5merc/gs8hs7FbtzYXKzAQGMsLvVdA5yJzcQGYaDienCtuUwSJiIjISdQJcpqKIBEREXekIshp2oIiIiLikdQJEhERcUfqBDlNW1BEREQ8kjpBIiIi7kidIKepCBIREXFHKoKcpi0oIiIiHkmdIBEREXekTpDTtAVFRETEI6kTJCIi4o7UCXKaiiARERF3pCLIadqCIiIi4pHUCRIREXFH6gQ5TVtQREREPJI6QSIiIu5InSCnqQgSERFxRyqCnObwFszPzycqKorFixfbjZeWljJkyBDS09P58ccf%2BfWvf03fvn2Jjo4mPj6eAwcOGE9aRERELjwlJSXEx8cTHR1Nv379eP7556mpqWl02bi4OHr16kVkZKTt9umnnwJw4sQJZs%2BezXXXXUdkZCQTJkygsLDQeL4OF0HBwcHMnj2befPmsWnTJgCqq6tJSEggIiKChIQEEhMTCQoK4rPPPuOzzz6jZcuWpKamGk9aRETE43l5mb85adKkSfj7%2B/PZZ5%2BRmZnJpk2bWLp0aaPLbt%2B%2Bnddff53s7Gzb7YYbbgDg1Vdf5fPPP2fVqlV89tln%2BPr68tvf/tbp/E7VpHc8dOhQ4uLimDx5MgUFBUybNo2qqipmzZqFxWLh7bff5tlnn8XX15eysjLKy8u5%2BOKLjSctIiIiF5Z9%2B/axZcsWHn/8cfz8/OjatSvx8fEsX768wbIHDhzg559/5oorrmg01jvvvMP48ePp2LEjrVq14plnnuHTTz81vnepyXOCkpOT2b59O7GxsVRXV5OZmYmfnx8APj4%2BAKSkpPD%2B%2B%2B8TGBh42grwlxQWFlJUVGQ3VlUVSPv2QU2OdTr%2B/vb35rQyG851iUJkpLlY4eH29wb957%2BXMf/5b2q7N6pZS3Ox6t%2B46Q0gnq1DB7Px2rWzvzeppcHfp7Zt7e9NOeW76pxywZygxr5/AwMDCQo68/fvrl27CAgIoMNJ/8dCQkI4ePAgR44coU2bNrbxbdu20bJlSyZPnsy2bdto374948aNY8yYMRw9epR///vfhIaG2pZv3749F110ETt37qRr164G3mmdJhdBXl5exMTEkJSUxPDhw%2BnYsWODZZ5//nlmzJjByy%2B/zAMPPMAHH3xA69atHX6NFStWkJ6ebjeWkDCRYcMSm5ruGfXsaTpiX9MB65hPFLKyzMfMyDAe0nxZVSc42BVRrzQfskcP8zFdxWI53xk4xpPzfPhh8zEBbrvNNXFN%2B7//MxvvlO%2Bqc8oFRVBj378TJ04kMfHM37/l5eW2pki9%2BscVFRV2RVB1dTW9e/dm8uTJ9OjRg82bN5OYmEjLli2J/M8f6P6n/PHv6%2BtLeXn5Wb2v02lyEbR//36mTp3KuHHjyMjIYOXKlcTExNgt4%2BvrC8CTTz7JO%2B%2B8wxdffMFNN93k8GuMHTuWwYMHn/K6gXz5ZVOzPT1//7q6YscOqKgwF7cvBpME1yUK8Nhj5mKFh9cVQLGxkJtrLi6Qm2G2WPPxqSuA8vPh2DGjoQmv/sZcMD%2B/ugJo1y6orDQXF6BXL7PxoO4L22o1H9c0T8/zT38yG69du7oC6K9/hZISs7FNd4L%2B7//g73%2BHn34yF/d/TGPfv4GBgQ6t6%2B/vT%2BUpn1X1j1ue8rMcPXo0o0ePtj2%2B/vrrGT16NH/729%2B47rrr7NatV1VV1SCOs5pUBJWVlTFhwgRrk9B6AAAgAElEQVQGDhxIamoqISEhpKWlERYWRlhYGLfddhu///3vufLKur%2BGT5w4QW1tLRdddFGTkgoKCmrQeisshLKyJoVxSEWF6bguSBJckShkZ5uNB3UFkOG4pr//6x075oLYVWb/SgHqkjT81494sEOHXBO3pMR87CbsQXDYTz%2Bd311YJrmgE9TY96%2BjevToweHDhykuLqZ9%2B/YA5OXlcckllzTYG5SZmUnLli0ZNmyYbay6uhofHx8uuugiOnTowO7du227xIqKijh8%2BLDdLjITHN6CtbW1TJkyBR8fH9LS0gCIiYlh5MiRJCYmUlZWxmWXXcbvf/97SktLKS8vJy0tje7du9O7d2%2BjSYuIiMiFpXv37kRFRTFz5kzKyso4cOAACxcuZMyYMQ2WLSsrY8aMGXz77bfU1tbyySefsHbtWsaOHQvAHXfcwauvvsqBAwcoKytj5syZXH311XTr1s1ozg53gubOnUtOTg6rV6%2B2TYAGmD59OnfffTeTJk1i/vz5zJkzh%2BHDh2OxWLj22mtZsmQJLVq0MJq0iIiIx7sAT5Y4f/580tLSGDJkCF5eXowePZr4%2BHgAIiMjee655xg1ahQPPvggFRUVTJw4kZKSErp27crs2bOJjo4GICEhgZqaGu69917Ky8vp168fL7/8svF8HS6CUlJSSElJaTDu4%2BPDmjVrbI9nzpxpJjMRERE5vQuwCGrfvj3z589v9Lnsk6ZKWCwW4uPjbQXSqby9vZkyZQpTpkxxSZ71LrwtKCIiInIO6NphIiIi7ugC7AS5G21BERER8UjqBImIiLgjdYKcpiJIRETEHakIcpq2oIiIiHgkdYJERETckTpBTtMWFBEREY%2BkTpCIiIg7UifIaSqCRERE3JGKIKdpC4qIiIhHUidIRETEHakT5DQVQSIiIu5IRZDTtAVFRETEI1msVqv1fCfhiOBgs/F69oS1a2HECNixw1zcfNwkUSB7db6xWH5%2BEB4OublQWWksLACRfSyGA0ZCVhb06QPZ2UZDtw0w9%2Bt05ZWwcSPceCN8842xsAD8VOqCX3uLBdzh48TD84yKNvv7FB4Oy5fDvffW/f5fqFyZ59atZuM5bNEi8zEfe8x8zAuYOkEiIiLikTQnSERExB1pTpDTVASJiIi4IxVBTtMWFBEREY%2BkTpCIiIg7UifIadqCIiIi4pHUCRIREXFH6gQ5TUWQiIiIO1IR5DRtQREREfFI6gSJiIi4I3WCnKYtKCIiIh5JnSARERF3pE6Q01QEiYiIuCMVQU7TFhQRERGPpE6QiIiIO1InyGnagiIiIuKR1AkSERFxR%2BoEOc3hLZifn09UVBSLFy%2B2Gy8tLWXIkCGkp6fbjc%2BdO5fBgwebyVJERETseXmZv3kYh99xcHAws2fPZt68eWzatAmA6upqEhISiIiIICEhwbbspk2beP31181nKyIiImJIk8q%2BoUOHEhcXx%2BTJkykoKGDatGlUVVUxa9YsLBYLAMXFxfz2t7/l/vvvd0nCIiIigjpBBjR5TlBycjLbt28nNjaW6upqMjMz8fPzA6C2tpYpU6Ywfvx4WrRowfr1688qqcLCQoqKiuzGOnUKpG3boLOK15iQEPt7c3qaDee6RPnPj80IHx/7e6MiI83GCw%2B3vzfoytbmYvXoYX8vYoLp//bdu9vfX6hclWdurtl4cm5ZrFartakrrV%2B/nqSkJIYPH85LL71kG3/llVfIzc1lwYIFrF69mvT0dD7%2B%2BOMmJ7VgwYIGc4zi4yeSnJzY5FgiIiKuEhUFW7eepxd/7z3zMUeONB/zAtbkTtD%2B/fuZOnUq48aNIyMjg5UrVxITE8OXX37J6tWrWb16tdNJjR07tsGk6kcfDeQf/3A6tE1ICMybB8nJkJdnLu5aRpgLBq5LFMids9ZYLB8fCA6G/Hw4dsxYWADCY/sYDhgOGRkQG2v8z7gbW2cZi9WjB/zhDxAXB7t2GQsLwMZPmvy3z5lZLND0v6nOPQ/P8977LEbjde8Ozz8PzzwDe/caDW2Uu%2BTZJB64%2B8q0JhVBZWVlTJgwgYEDB5KamkpISAhpaWmEhYXx7rvv2o4UAzh%2B/DjHjh0jOjqa1157jejoaIdfJygoiKAg%2B11fBw/W3UzLy4MdO0xGNBrsv8wnSmWl0XBAXQFkPG52tuGA/5Gbazz2NwFGwwF1BdA335iPK57JVbtv9u51j11D7pKnnBsOF0H18318fHxIS0sDICYmhuzsbBITE1m9ejUzZsywLe/M7jARERE5A3WCnObwFpw7dy45OTmkp6fjc9Ls1%2BnTp9OuXTsmTZpETU2NS5IUERERMc3hTlBKSgopKSkNxn18fFizZk2D8TvuuIM77rjDuexERESkceoEOU2XzRAREXFHKoKcpiJIREREjCgpKeHZZ59ly5YtNGvWjFGjRvHkk0/SvHnDcuPtt99m6dKlFBYWEhQUxAMPPMC9994L1M1DjoqKwmq12k7GDPD555/j7%2B9vLF8VQSIiIu7oAuwETZo0iQ4dOvDZZ59RXFzMhAkTWLp0KXFxcXbLffjhh7z00kssWbKEq666ipycHB599FHat2/PzTffzO7duzl%2B/DhZWVm0aNHCZfleeFtQRERE3M6%2BffvYsmULjz/%2BOH5%2BfnTt2pX4%2BHiWL1/eYNlDhw4xfvx4evfujcViITIykn79%2BvHll18CsG3bNsLCwlxaAIE6QSIiIu7JBZ2gxi5bFRgY2ODcfY3ZtWsXAQEBdOjQwTYWEhLCwYMHOXLkCG3atLGN1%2B/2qldSUsKXX35JamoqUFcEHTt2jDvvvJMff/yRkJAQUlJS6NPH7MlzVQSJiIi4IxcUQStWrGhw2aqJEyeSmHjmy1aVl5fbriVar/5xRUWFXRF0sqKiIh577DEiIiIYMaLuqgu%2Bvr5ceeWVJCcnc9FFF7F8%2BXIeeeQR3n33Xbp27Xo2b61RKoJEREQEaPyyVYGBgQ6t6%2B/vT%2BUplwyof9yyZctG18nJySE5OZno6GheeOEF2wTqp556ym65Rx55hNWrV7Nx40buu%2B8%2Bh/JxhIogERERd%2BSCTlBjl61yVI8ePTh8%2BDDFxcW0b98egLy8PC655BJat27dYPnMzEx%2B97vfkZSUxMMPP2z33Ny5c7n55pu54oorbGPV1dV2J2s2QROjRURExGndu3cnKiqKmTNnUlZWxoEDB1i4cCFjxoxpsOz69euZPn06CxYsaFAAAXz//fc8//zzFBUVUV1dTXp6OmVlZdx0001Gc1YRJCIi4o68vMzfnDR//nxqamoYMmQIMTExDBgwgPj4eAAiIyN59913AUhPT%2BfEiRMkJSURGRlpu02dOhWAF154gW7dunHbbbfRr18/tmzZwp/%2B9CcCAsxepVq7w0RERNzRBXieoPbt2zN//vxGn8vOzrb9%2B7333vvFOAEBAbzwwgtGc2vMhbcFRURERM4BdYJERETc0QXYCXI3KoJERETckYogp2kLioiIiEdym05QPsGGI/YE1rKWEcAOY1GDyTcWC%2BqzhBGsNZhlnfyqTeaCNWsJXEl49TdQVW4uLtA2wGo03pWtYSNwY%2BssvjF7oAE/HbaceSFHHY0Esth4tA8czj7j4k1TazieuIutP/3KbMAjPYH3WH5kJPxk%2BlPKIJfmucdwPAepE%2BQ0bUERERHxSG7TCRIREZGTqBPkNBVBIiIi7khFkNO0BUVERMQjqRMkIiLijtQJcpq2oIiIiHgkdYJERETckTpBTlMRJCIi4o5UBDlNW1BEREQ8kjpBIiIi7kidIKdpC4qIiIhHUidIRETEHakT5DQVQSIiIu5IRZDTtAVFRETEI6kTJCIi4o7UCXKatqCIiIh4JIeLoPz8fKKioli8eLHdeGlpKUOGDCE9PZ3i4mLCwsKIjIy03QYPHmw8aREREY/n5WX%2B5mEc3h0WHBzM7NmzSU5OplevXlx77bVUV1eTkJBAREQECQkJfPLJJ3Tu3JmPP/7YlTmLiIiIBxYtpjVpCw4dOpS4uDgmT55MQUEB06ZNo6qqilmzZmGxWNi2bRsRERGuylVERETEmCZPjE5OTmb79u3ExsZSXV1NZmYmfn5%2BAGzbto2ff/6ZESNGUFxcTK9evXjyySe57LLLmvQahYWFFBUV2Y0FdupEUNu2TU339EJC7O8N6Wk0msvSrNOypblY//k/YLs36Morzcbr0cP%2B3qijkeZihYfb34uY0NPwp5RLP6QMclWeO3aYjdcU6gQ5zWK1Wq1NXWn9%2BvUkJSUxfPhwXnrpJdt4SkoKQUFBjB8/nhYtWjBv3jw%2B%2BOAD1q1bR%2BvWrR2Ov2DBAtLT0%2B3GJsbHk5ic3NRURUREXOdXv4I9e87Pa//4o/mYnTubj3kBa3IRtH//fu666y5Gjx5NRkYGzz77LDExMY0uW1tbS3R0NC%2B%2B%2BCKDBg1y%2BDUa7QQ9%2Bqj5TtC8eZCcDHl5xsKOYK2xWOCyNAFYO/Mbc8H8/OpaK7t2QWWlubjAjYlmW0E9esAf/gBxcXXpmrTxaB9zwcLDISMDYmMhN9dcXICtW83GA7BYoOl/U517np7nqFFm44WEwMsvw6RJ5j%2BkTHJVnjt2nL8iqKDAfMyOHc3HvIA1aXdYWVkZEyZMYODAgaSmphISEkJaWhphYWGEhITwyiuvcN9999H5P5XkiRMnqKmpwdfXt0lJBQUFERQUZD948GDdzbS8PKPtTFc1Rg2nWae83HBA6gogw3G/MVirnWzXLhfEPpxtOCB1BVC2C%2BKKZ3LV7huXfEi5gLvk6QjtDnOaw1uwtraWKVOm4OPjQ1paGgAxMTGMHDmSxMREqqqq%2BNe//sXs2bM5evQo5eXlzJgxgy5duhAdHe2yNyAiIiJyNhwugubOnUtOTg7p6en4%2BPjYxqdPn067du2YNGkSCxcupLa2lqFDhzJgwACKiopYsmQJ3t7eLkleRETEY%2Bk8QU5zeHdYSkoKKSkpDcZ9fHxYs2aN7fGpE5pFRERELkS6dpiIiIg78sDOjWkqgkRERNyRiiCnaQuKiIiIR1InSERExB2pE%2BQ0bUERERHxSOoEiYiIuCN1gpymIkhERMQdqQhymragiIiIeCR1gkRERNyROkFO0xYUERERj6ROkIiIiDtSJ8hpKoJERETckYogp2kLioiIiBElJSXEx8cTHR1Nv379eP7556mpqWl02Y0bNzJy5Eh69%2B7NsGHD2LBhg93zS5Ys4YYbbqB3797cf//97Nmzx3i%2BKoJERETckZeX%2BZuTJk2ahL%2B/P5999hmZmZls2rSJpUuXNlhu7969JCYmkpyczFdffUViYiKTJk3i0KFDAKxZs4Zly5bx%2Buuvs3nzZnr27ElSUhJWq9XpHE%2BmIkhERESctm/fPrZs2cLjjz%2BOn58fXbt2JT4%2BnuXLlzdYds2aNURHRzN06FCaN2/OrbfeSt%2B%2BfVmxYgUAK1euJDY2lh49euDj40NKSgoHDx5k8%2BbNRnN2nzlBLmiDAfDee0bD5WO2Sq1jYe17roh7jfmQvXoZD/lTqWu26cZPXBG31nzIrVvNxzQ9lyAyErKyICoKsrPNxa11wfb0cNY885%2BlFsD6rtnPUgBLzXHjMVm92nzM88UFc4IKCwspKiqyGwsMDCQoKOiM6%2B7atYuAgAA6dOhgGwsJCeHgwYMcOXKENm3a2MZ3795NaGio3fqXXXYZubm5tufHjx9ve87b25vu3buTm5vLNdeY%2B%2B5ynyJIREREbKxYjMdcsWIF6enpdmMTJ04kMTHxjOuWl5fj5%2BdnN1b/uKKiwq4IamxZX19fKioqHHreFBVBIiIiAsDYsWMZPHiw3VhgYKBD6/r7%2B1NZWWk3Vv%2B4ZcuWduN%2Bfn5UVVXZjVVVVdmWO9PzpqgIEhERcUOu2FscFBTk0K6vxvTo0YPDhw9TXFxM%2B/btAcjLy%2BOSSy6hdevWdsuGhoayY8cOu7Hdu3cTERFhi7Vr1y4GDRoEwPHjx9m7d2%2BDXWjO0sRoERERN1Rba/7mjO7duxMVFcXMmTMpKyvjwIEDLFy4kDFjxjRYdtSoUWzZsoV169ZRU1PDunXr2LJlC7fddhsAd955J2%2B99Ra5ubkcO3aMF198kfbt2xMdHe1ckqdQESQiIiJGzJ8/n5qaGoYMGUJMTAwDBgwgPj4egMjISN59912gbsL0K6%2B8wqJFi%2Bjbty8LFy5kwYIFBAcHAzBmzBjGjRtHQkIC11xzDd9%2B%2By2LFi3C29vbaL4Wq%2BmD7l3FFWlaLK6Ja5ryNM9dcnVVnq46OqxPnwv/6DAP/9m7YjKtqzap8aPDvL3huAuOODP8xeyoY8fMx/TxMR/zQqZOkIiIiHgkTYwWERFxQzqNlvNUBImIiLghFUHO0%2B4wERER8UjqBImIiLghdYKcp06QiIiIeCR1gkRERNyQOkHOUxEkIiLihlQEOU%2B7w0RERMQjqRMkIiLihtQJcp46QSIiIuKRHC6C8vPziYqKYvHixXbjpaWlDBkyhPT0dGpra0lPT%2BfGG28kMjKSu%2B66i2yT1xESERER4MK7irw7crgICg4OZvbs2cybN49NmzYBUF1dTUJCAhERESQkJLBw4ULWrl3L0qVL%2Beqrr7jpppv49a9/TXV1tcvegIiIiCdSEeS8Js0JGjp0KHFxcUyePJk1a9Ywf/58qqqqmDVrFrW1tbzxxhu8/PLLBAcHA/DII49w7bXXuiRxEREREWc0eWJ0cnIy27dvJzY2lurqajIzM/Hz8yMvL48jR45w5MgR7rjjDn788UeuuOIKUlNTadGiRZNeo7CwkKKiIruxwPbtCQoKamq6ItKYyEiz8cLD7e9FxOU8sXNjWpOLIC8vL2JiYkhKSmL48OF07NgRgMOHDwOwbNkyFixYQLt27UhPT%2BeRRx5h3bp1tG7d2uHXWLFiBenp6XZjExMSSExKamq6Z2axmI/pCsrTPHfJ1RV5ZmWZjwmQkeGauKZ58M/eVe/cJZvU2/vCj3n8uNl4ck5ZrFartSkr7N%2B/n7vuuovRo0eTkZHBs88%2BS0xMDNu2bWPMmDH86U9/4rrrrgOgtraWqKgoXn75ZW688UaHX%2BOcdYIsFmja2z8/lKd57pKrq/KMijIbLzy8rgCKjYXcXHNxt241F6ueh//srS4og1y1SS01hgsMb2/XFC2uKNYc8OOP5mN27mw%2B5oWsSZ2gsrIyJkyYwMCBA0lNTSUkJIS0tDTCwsIICQmhefPmdpOgrVar7dYUQUFBDQsed/jQEnEXrjpqMzfXdbFFxI52hznP4aPDamtrmTJlCj4%2BPqSlpQEQExPDyJEjSUxMpKqqihEjRvDCCy/www8/UF1dzZw5c2jTpg3XXHONy96AiIiIyNlwuBM0d%2B5ccnJyWL16NT4%2BPrbx6dOnc/fddzNp0iT%2B%2BMc/smDBAh544AF%2B%2BuknIiIieP311/H19XVJ8iIiIp5KnSDnNXlO0Hnjkh3Onj03wDh3yRPcJ1dX5ell%2BGTxkZF1k6379DG7O8wVn/Ie/rPXnKD/nTlB%2B/aZj3nppeZjXsh07TARERE3pE6Q81QEiYiIuCEVQc7TBVRFRETEI6kTJCIi4obUCXKeOkEiIiLikdQJEhERcUPqBDlPRZCIiIgbUhHkPO0OExEREY%2BkTpCIiIgbUifIeeoEiYiIiEdSJ0hERMQNqRPkPBVBIiIibkhFkPO0O0xEREQ8kjpBIiIibkidIOepCBLxRK769Ny61Ww8L8PN6shIyMqCqCjIzjYbOyvLXCw/PwgPh507obLSXFzAsmeP0XgEBMCQIVg%2B/ggOHzYb29/fXKw2baB/f9iyBY4cMRcXYNgws/HknFERJCIi4obUCXKeiiARERE3pCLIeZoYLSIiIh5JnSARERE3pE6Q81QEiYiIuCEVQc7T7jARERHxSOoEiYiIuCF1gpynIkhERETOiYqKCmbMmMHHH39MTU0NQ4YMYdq0abRs2bLR5devX8/ChQs5cOAAAQEB3HHHHcTHx%2BP1n3OIDRs2jIMHD9oeA2RmZhISEuJQPiqCRERE3JA7doJmzJhBQUEB69ev58SJE0yaNIk5c%2BYwbdq0Bstu376dJ554gpdffpkbb7yR/Px8xo8fj7%2B/Pw8//DBlZWXk5%2Bfz0Ucf0blz57PKR3OCRERE3FBtrfmbK1VWVvLee%2B%2BRlJREQEAA7dq1Y8qUKaxevZrKRs6M/uOPP3L33XczaNAgvLy8CAkJ4aabbuLLL78E6oqkgICAsy6AQJ0gERER%2BY/CwkKKiorsxgIDAwkKCnJo/aqqKg4dOtToc5WVlRw/fpzQ0FDbWEhICFVVVezdu5fLL7/cbvmbb76Zm2%2B%2B2S72J598wsiRIwHYtm0bfn5%2B3HfffezatYvOnTuTmJjIoEGDHMoVVASJiIi4JVd0blasWEF6errd2MSJE0lMTHRo/a%2B//poHHnig0eeSk5MB8D/pmnB%2Bfn4AlJeX/2LcsrIykpOT8fX1Zdy4cQBYLBZ69erFb37zGzp16sQHH3xAYmIib731Fr1793YoXxVBIiIiAsDYsWMZPHiw3VhgYKDD6/fr14%2BdO3c2%2Bty3337LvHnzqKystE2Ert8N1qpVq9PG3LNnD0lJSbRr144333zTtmxcXJzdcqNGjWLt2rWsX79eRZCIiMj/Mld0goKCghze9dVUwcHBeHt7s3v3bq666ioA8vLy8Pb2pnv37o2us3HjRn7zm98QExNDSkoKzZv/t2x5/fXXueKKK7j22mttY9XV1fj4%2BDickyZGi4iIuCF3mxjt5%2BfHsGHDmDNnDqWlpZSWljJnzhxGjBiBr69vg%2BVzcnJISEggNTWVJ5980q4AAigoKOC5557jwIED1NTUkJmZSXZ2NrfffrvDOakIEhERkXNi2rRpdO/enZEjR3LLLbfQpUsXpk6dant%2B%2BPDhvPbaawC89tpr1NTU8PzzzxMZGWm71e8Ge%2BKJJ7jhhhuIjY0lOjqaP//5zyxevJhLL73U4XwsVqvVavYtuogr0rRYXBPXNOVpnrvk6i55gmty9TL8d1pkJGRlQZ8%2BkJ1tNnZWlrlYfn4QHg65udDIocNO2bPHbLyAABgyBD76CA4fNhv7pAm0TmvTBvr3h88/hyNHzMUFGDbMbDwHrV9vPuZJB2N5BIc/YfLz84mKimLx4sV246WlpQwZMoT09HS7Si0yMpIrr7ySsLAwsk1/2IiIiIg4yeGJ0cHBwcyePZvk5GR69erFtddeS3V1NQkJCURERJCQkMDEiRNty9fU1PDII4/QpUsXIiMjXZK8iIiIp3LHM0ZfaJp0dNjQoUOJi4tj8uTJrFmzhvnz51NVVcWsWbOwWCx2y7766quUlJSwZMkSowmLiIiIiiATmnyIfHJyMtu3byc2Npbq6moyMzNtJzuqt3//fhYvXsyyZcto0aJFk5Nq9IyV7du77LA9EblAme4ih4fb35t0yuegU%2BoP8W3Cob4OCwgwG691a/t7kxo5Yuis1V%2Bg8zQX6jxrpucXyTnV5CLIy8uLmJgYkpKSGD58OB07dmywzGuvvcaNN97o8MmKTtXoGSsTEkhMSjqreL/olA7WBUt5mucuubpLnmA%2BV5OTjU%2BWkeGauKYFB5uP6YoCEODqq10T17Sz/F46rb/9zWy8JlAnyHlNLoL279/P1KlTGTduHBkZGaxcuZKYmBjb8%2BXl5bz//vtO7QZr9IyV7dubP/LEXY68UZ7muUuu7pInuCbXqCiz8cLD6wqg2Ni6I69MMllY%2BfjUFUD5%2BXDsmLm4AD/%2BaDZe69Z1BdCWLXD0qNnYpjtBvXtDTg6c4RIN4jmaVASVlZUxYcIEBg4cSGpqKiEhIaSlpREWFmY7%2B%2BPGjRu5%2BOKL6du371kn1egZK93li0BEzHHVkaW5ueZjmz6UHeoKINNxTR/GXu/o0Qv7EPl65eX/M7uw1AlynsOHyNfW1jJlyhR8fHxIS0sDICYmhpEjR5KYmEhxcTEAWVlZREVFNZgoLSIiIua42xmjL0QOF0Fz584lJyeH9PR0u%2BtyTJ8%2BnXbt2jFp0iRqamo4cOAAHTp0cEmyIiIiIqY4vDssJSWFlJSUBuM%2BPj6sWbPG9njRokVmMhMREZHT8sTOjWm6dpiIiIh4pCYfHSYiIiLnnzpBzlMRJCIi4oZUBDlPu8NERETEI6kTJCIi4obUCXKeOkEiIiLikdQJEhERcUPqBDlPRZCIiIgbUhHkPO0OExEREY%2BkTpCIiIgbUifIeeoEiYiIiEdSJ0hERMQNqRPkPBVBIiIibkhFkPO0O0xEREQ8kjpBIiIibkidIOepCBKRC1dWltl4fn519xkZUFlpNnafPuZiRUbWvffYWMjONhcXoLTUbLxmzeru%2B/aFEyfMxm7Rwlwsr//s%2BOjTR9WD2KgIEhERcUOq5ZynIkhERMQNqQhyniZGi4iIiEdSJ0hERMQNqRPkPBVBIiIibkhFkPO0O0xEREQ8kjpBIiIibkidIOepEyQiIiIeSZ0gERERN6ROkPNUBImIiLghFUHO0%2B4wERER8UjqBImIiLghdYKcp06QiIiIeCR1gkRERNyQOkHOUxEkIiLihlQEOU%2B7w0RERMQjOVwE5efnExUVxeLFi%2B3GS0tLGTJkCOnp6ZSWljJ58mT69etHv379iI%2BP5%2BDBg8aTFhER8XS1teZvrlZRUUFqair9%2BvUjKiqKJ554gvLy8tMuP23aNCIiIoiMjLTdVqxYYXt%2ByZIl3HDDDfTu3Zv777%2BfPXv2NCkfh4ug4OBgZs%2Bezbx589i0aRMA1dXVJCQkEBERQUJCAmlpaXh5ebFhwwY2bNiAj48PqampTUpIRERE/jfNmDGDgoIC1q9fz9///ncKCgqYM2fOaZfftm0bM2bMIDs723YbO3YsAGvWrGHZsmW8/vrrbN68mZ49e5KUlITVanU4nybtDhs6dChxcXFMnjyZgoICpk2bRlVVFbNmzcJisZCXl4fVarXdvLy88PPza8pLiBa8nYAAAB72SURBVIiIiAPcrRNUWVnJe%2B%2B9R1JSEgEBAbRr144pU6awevVqKisrGyxfXV3N999/T0RERKPxVq5cSWxsLD169MDHx4eUlBQOHjzI5s2bHc6pyROjk5OT2b59O7GxsVRXV5OZmWkrdCZMmMAzzzxDVFQUAJdeeilvvfVWU19CREREzuBCnBhdVVXFoUOHGn2usrKS48ePExoaahsLCQmhqqqKvXv3cvnll9stn5ubS01NDfPnz2fr1q20bt2aO%2B%2B8k7i4OLy8vNi9ezfjx4%2B3Le/t7U337t3Jzc3lmmuucSjfJhdBXl5exMTEkJSUxPDhw%2BnYsaPtudraWsaOHcuECRM4ceIEzzzzDJMmTWL58uVNeo3CwkKKiorsxgLbtycoKKip6YqIOzPdSfbxsb83KTLSXKzwcPt7k5o1MxvPy8v%2B3hWxTbBY/ntvMu6FWIk4odHv38BAh79/v/76ax544IFGn0tOTgbA39/fNlbfRGlsXtDRo0e5%2Buqruf/%2B%2B3nppZf47rvvSEhIwMvLi7i4OMrLyxvsbfL19aWiosKhXOEsiqD9%2B/czdepUxo0bR0ZGBitXriQmJoaioiKeeuopNmzYwEUXXQTA9OnTueGGG9i5cydhYWEOv8aKFStIT0%2B3G5uYkEBiUlJT0z2z%2Bl%2BMC53yNM9dcnWXPMF8rq4oAgCCg83HzMoyHzMjw3xMV2nV6nxn4BhfX7PxfmFSr6u5ov5q9Pt34kQSExMdWr9fv37s3Lmz0ee%2B/fZb5s2bR2VlJS1btgSw7QZr1cj/n/79%2B9O/f3/b4yuvvJIHH3yQdevWERcXh5%2BfH1VVVXbrVFVV2WI7oklFUFlZGRMmTGDgwIGkpqYSEhJCWloaYWFheHt7c/z4caqrq/8bvHldeG9v76a8DGPHjmXw4MF2Y4Ht20MTJjs5xGIxH9MVlOf/b%2B/eo6Ks8z%2BAvwcERJNABbOfFxQVUkMH8RabqUnaoilK4qqrlia2QYh31hvKIrkHwxpIO6zp2lqaiafcOmpYuXtcDStQAzHAaN3QuFmKMly/vz8m0JGLg/Mdn3mc9%2BucOaPPM3x4O%2BPMfObzfWZGPrVkVUtOwDJZm3kwvWdOToYG6IcfgMpKubVnzpRXy8fH0ADNnAnk5MirCwBffim3np2doQEqL5f/rNzK544WaTSGBkivV899SgFNPv%2B6u0up3atXLzg4OCAvLw%2BDBg0CAOTn5zcsY90pLS0NJSUlmDFjRsO2qqoqtP2tke3bty9yc3MxZswYAEB1dTUKCgqMltvuxuQmqK6uDsuWLYOTkxM2btwIAJg%2BfToyMjIQERGB1NRUdO/eHXFxcfjrX/8KANi0aRN8fX2b/Me1xMPDo/Hojf9piWxPEwdLSlFZKb92RobceoChAZJdt7ZWbr16dXXya8tcuqtfAhPigVnCssQ/o8nnX0mcnZ3x7LPPIiEhAW%2B88QYAICEhARMnTmxobG4nhEB8fDx69uyJESNGIDMzE7t372541/m0adOg0%2BkwatQo9OrVC4mJiejcuTP8/f1NzmTywmhiYiIyMzORlJQEp9vW02NiYtCpUycsXrwYO3bsAGB4F9kzzzwDIQSSk5NhZ4m1YiIiIhumtneHAYbP/fH09MSkSZMwYcIEdOvWDevWrWvYHxQUhO3btwMAAgMDER0djZiYGGi1WixfvhwRERGYPHkyACAkJATz5s3DK6%2B8ghEjRiA7Oxtvv/12q1afNKI1b6hXkiViqmWpgTnlU0tWteQELJM1M1NuPWdnw1JTTo78SZCfn7xaWq3hGCM/P/mToLIyufXs7QEXF%2BDaNfmTIEdHebXs7Ay3f0WF/Gf7VhyDItPq1fJrxsXJr2nN%2BN1hREREKvSArOopiutUREREZJM4CSIiIlIhToLMxyaIiIhIhdgEmY/LYURERGSTOAkiIiJSIU6CzMdJEBEREdkkToKIiIhUiJMg87EJIiIiUiE2QebjchgRERHZJE6CiIiIVIiTIPNxEkREREQ2iZMgIiIiFeIkyHxsgoiIiFSITZD5uBxGRERENkk9k6AXX5Rbr2dPICYG2LAB%2BPFHaWVfwE5ptYBbMWM2aGTGBADs7Bcvr1iXLobbaOdO4Oef5dUFMOTDaKn1fHyAPXuAWbM1yMmRWhrfXO0tr9iAAcChQ8BzzwFZWfLqAhD5F6XWAwANAAGN3JoXJed0dTX8B/jpJ%2BCXX%2BTWLiuTV8ve3nD%2B5ZdAba28ugDQsaPcelot8O23wOjRQEaG3NqOjvJqDR4MfPWVIWdmpry6AFBZKbeeiTgJMh8nQURERGST1DMJIiIiogacBJmPTRAREZEKsQkyH5fDiIiIyCZxEkRERKRCnASZj5MgIiIiskmcBBEREakQJ0HmYxNERESkQmyCzMflMCIiIrJJnAQRERGpECdB5mMTREREpEJsgszH5TAiIiKySZwEERERqRAnQebjJIiIiIhsEidBREREKsRJkPnYBBEREakQmyDzcTmMiIiIbJLJTVBMTAwCAgJQWlpqtL2mpgbTp09HWFgYhBAAgNraWoSHh0On08lNS0RERAAMkyDZJ1tjchMUHR2Nzp07Izo62mi7TqdDSUkJNm/eDI1Gg8LCQixcuBCfffaZ9LBEREREspjcBDk5OSExMRGnT5/Gu%2B%2B%2BCwBIT0/Hrl27sHXrVri6uuKHH35AcHAwBg0aBK1Wa7HQREREto6TIPO16sDo3r17Y926ddiwYQP8/f2xatUqrFixAr6%2BvgAAd3d3pKWloUOHDjh9%2BvQ9hyoqKkJxcbHRNvcOHeDRseM912yka1fjc0l6Sq1msZgGXbrIq9Wpk/G5RD4%2Bcut5ehqfS3VtgLxaXl7G57bI1VVuvQ4djM9lsreXV8vOzvhcJtkvUOvvoLLvqADg4CCvlre38bksmZly67WCLTYtsmlE/YE8rbBixQocOXIE48aNw5YtW5q8zB//%2BEcMGzYMERERrQ6l0%2BmQlJRktC38lVcQ8eqrra5FRERkMU5OQGWlIr960iT5NQ8dkl/Tmt3TW%2BTDw8Px0UcfITIyUnYeAEBoaCjGjh1rtM397beBmBh5v6RrVyAsDHj7beDyZWllYxAjrRZgsZgAgJge78gr1qkTMHky8NFHwB0Hz5tr1rEXpdbz9ATi4oDVq4GCAqmlseeaxEclLy9g61Zg8WIgP19eXQDiY/mPdBoN0PqXVHep%2BfkxuQU7dACGDQPS04Hr1%2BXWHjpUXi07O%2BChh4Dycvkv90ePllvPxwd47z1g5kwgJ0dubdmToN27gTlzgAsX5NVVECdB5runJsjutxGtnSVGtQA8PDzg4eFhvPH6dfkPWoChs/jxR2nl5FUyJjmmgdPPkgvC0AD9LLeu7MfVegUFFqh9NUtyQRgaoCwL1FWDX36xTN3r1%2BXXrq2VWw8wPMvJrpuRIbdevZwc%2BbUdHeXWAwwNkIJLWGRd%2BGGJREREKsRJkPnYBBEREakQmyDz3VMT1K1bN1y4y5pq/dvoiYiIiKwRJ0FEREQqpMZJ0M2bNxEbG4vPP/8cNTU1ePrpp7F%2B/Xq0b9%2B%2B0WXXrVuHQ3e8XU2v1%2BOJJ57Ajh07UFdXhyFDhkAIAY1G03CZEydOoF27dibl4XeHERER0X0RGxuLy5cv48iRIzh69CguX76MhISEJi%2B7ceNGZGRkNJx0Oh1cXFywatUqAEBeXh6qq6uRnp5udDlTGyCATRAREZEqqe0ToysqKnDo0CG8%2BuqrcHV1RadOnbBs2TKkpqaioqKixZ8tKyvDsmXLsHr1avTt2xcAcO7cOXh7e8PRjHcRcjmMiIhIhSzRtDT5jQ3u7o0/tqYZer0ePzfzMSkVFRWorq5Gv379GrZ5eXlBr9ejoKAAjz32WLN1ExISMHDgQDz33HMN286dO4fKykpMmzYNP/30E7y8vLB06VL4%2BfmZlBVgE0RERES/2bdvX%2BNvbAgPN/nbH86cOYM5c%2BY0ua/%2BA5ZvX65ydnYGANy4caPZmpcuXcLHH3%2BM/fv3G21v27YtfH19ERkZiYcffhh79uzB/Pnz8fHHH6N79%2B4m5WUTREREpEKWmAQ1%2BY0N7u4m//zw4cObffd4dnY23njjDVRUVDQcCF2/DPbQQw81W/PAgQPQarWNJkX1xwbVmz9/PlJTU3H8%2BHHMnj3bpLxsgoiIiAhAM9/YIEmvXr3g4OCAvLw8DBo0CACQn58PBwcHeLbwjdZHjx7Fiy82/vqkxMREjB8/Hv3792/YVlVVBScnJ5Mz8cBoIiIiFVLbgdHOzs549tlnkZCQgLKyMpSVlSEhIQETJ05E27Ztm/yZq1evIj8/H0Ob%2BG6%2B77//HnFxcSguLkZVVRWSkpJQXl6OwMBAkzOxCSIiIlIhtTVBALB%2B/Xp4enpi0qRJmDBhArp164Z169Y17A8KCsL27dsb/v6///0PANClS5dGteLj49GjRw9MnjwZw4cPR3p6Onbu3AlXV1eT83A5jIiIiO6Lhx56CLGxsYiNjW1y/yeffGL098cff7zZY4xcXV0RHx9vVh42QURERCqkxk%2BMtjZcDiMiIiKbxEkQERGRCnESZD42QURERCrEJsh8XA4jIiIim6SeSZBeL7deZeWtc4m1Jae0VEyDDh3k1frt0z/Rvr3cumQRmppq%2BUUdHOTXbcW3QZuk/rNI2raVX9uML3FsxO6316cODoC9vby6gNycgCFj/bns2lVV8mpVV986l1lXQZwEmY%2BTICIiIrJJ6pkEERERUQNOgszHJoiIiEiF2ASZj8thREREZJM4CSIiIlIhToLMx0kQERER2SROgoiIiFSIkyDzsQkiIiJSITZB5uNyGBEREdkkToKIiIhUiJMg87EJIiIiUiE2QebjchgRERHZJE6CiIiIVIiTIPNxEkREREQ2iZMgIiIiFeIkyHxsgoiIiFSITZD5TF4Oi4mJQUBAAEpLS42219TUYPr06QgLC8PVq1exatUqBAQEYOjQoZg7dy7Onz8vPTQRERGRuUxugqKjo9G5c2dER0cbbdfpdCgpKcHmzZuxevVqXL16Ff/85z9x4sQJ%2BPn5YcGCBbh586b04ERERLasrk7%2BydaY3AQ5OTkhMTERp0%2BfxrvvvgsASE9Px65du7B161Y8/PDD0Gg0iIyMhJubGxwdHTF//nyUlJSgoKDAUvmJiIiI7kmrjgnq3bs31q1bhw0bNsDf3x%2BrVq3CihUr4OvrCwBITk42uvzhw4fRrl079OrVq1WhioqKUFxcbLTN3cUFHh07tqpOix591PhcEk%2Bp1SwW08DdXV4tNzfjc4l8fOTW8/Q0Ppfq2gB5tby8jM9tkYuL3Hrt2xufy2Qn8c22Gs2tc5l1AWDwYLn1vL2Nz2WqrpZXq/6BRPYDSkaG3HqtYIuTG9k0QgjR2h9asWIFjhw5gnHjxmHLli1NXubYsWNYunQpYmJiMGXKlFbV1%2Bl0SEpKMtoW/soriHj11dZGJSIishyNBmj906gUXbvKr3n5svya1uyemqD//ve/CAwMxGeffYYePXoY7RNCYNu2bUhJSUFcXBx%2B//vftzpUk5OgN9%2BUPwmKiAB0OqCwUFrZaMRLqwVYLCYAIH7wPnnF3NyAZ54Bjh4Frl6VVxfArI9Dpdbz9ATi4oDVqwHZK7V7rk2SV8zLC9i6FVi8GMjPl1cXAFJT5dYDAAcHua/cASA9XW699u0Nk5DMTODGDbm1/fzk1dJogLZtAb1e/hPs6NFy63l7A7t3A3PmABcuyK0texL03nvAzJlATo68uhkZbIJU7J7eIm/323jW7o4xbUVFBaKiopCbm4s9e/agf//%2B9xTKw8MDHh4exhuvXTOcZCsslPpMKK%2BSMckxDf6v%2BO6Xaa2rV4FiuXVlPl7drqDAArWvZkkuCEMDlGWBumpgifs8YGiAZNeWuTZR/9gqhPw1j8xMufXqXbggv3ZVldx6gOFOr%2BASlkxcDjOf1M8JioqKwpUrV3DgwAG4urrKLE1EREQklbQmKCsrC1988QUcHR0xZswYo30pKSnw9/eX9auIiIhsHidB5runJqhbt264cMfa74ABAxptIyIiIstgE2Q%2BfoEqERER2SR%2BdxgREZEKcRJkPk6CiIiIyCZxEkRERKRCnASZj00QERGRCrEJMh%2BXw4iIiMgmcRJERESkQpwEmY%2BTICIiIrJJnAQRERGpECdB5mMTREREpEJsgszH5TAiIiKySWyCiIiIVKiuTv7pfqmoqEBoaChSU1NbvNyZM2fw/PPPQ6vVYuzYsdi/f7/R/oMHDyIwMBCDBw/G1KlTkZGR0aocbIKIiIjovsnNzcWsWbOQmZnZ4uV%2B/fVXLFy4EFOmTMHp06cRFxeH%2BPh4nD17FgDw1VdfITY2Fq%2B99hpOnz6N5557Di%2B//DIqKipMzsImiIiISIXUOAk6efIk5s6di%2BDgYDz66KMtXvbo0aNwdXXFrFmz0KZNG4wcORKTJk3Cnj17AAD79%2B9HUFAQhgwZAgcHB8ybNw9ubm749NNPTc7DA6OJiIhUyBJNS1FREYqLi422ubu7w8PDw6Sf1%2Bv1%2BPnnn5vc5%2B7uDh8fH3zxxRdwcnLCzp07W6yVm5uLfv36GW3r06cPPvzwQwBAXl4epk2b1mh/Tk6OSVkBNTVB778vtVxRURH26XQIjYoy%2BcY1hdyUhpw63T5ERYVKzWkQLq1Sw/UZKj/nN/JiArh1nSYmWuI6vSitUsN1umOHBXLKVVRUhH379sm//Z99Vl4tWPb/qUxFRUXY97e/WSZnZaXUcg3X6aFD1n%2Bd6nQIPXzYqnO2hhDya%2Bp0%2B5CUlGS0LTw8HBERESb9/JkzZzBnzpwm9yUnJ2PcuHEmZ7lx4wacnZ2NtrVt2xY3b940ab8pbHY5rLi4GElJSY06XmvDnPKpJatacgLqycqc8qklq1pyKq3%2BYOXbT6GhoSb//PDhw3HhwoUmT61pgADA2dkZer3eaJter0f79u1N2m8K9UyCiIiIyKI8PDysZlLWr18/nDhxwmhbXl4e%2BvbtCwDo27cvcnNzG%2B0fNWqUyb/DZidBREREZL0CAwNRUlKCXbt2obq6GqdOncKhQ4cajgMKCQnBoUOHcOrUKVRXV2PXrl0oLS1FYGCgyb%2BDTRARERFZhaCgIGzfvh0A4ObmhnfeeQeHDx/G8OHDsWbNGqxZswYjRowAAIwcORLr169HTEwMhg0bhk8%2B%2BQQpKSlwdXU1%2BffZx8TExFjiH6IG7du3x7Bhw1q1fqgE5pRPLVnVkhNQT1bmlE8tWdWS01bMnTsXjz32mNG2WbNmwd/fv%2BHvXbp0QUhICMLCwjBnzpxGl/fx8cHs2bOxaNEiTJ8%2BHY888kirMmiEsMTx5URERETWjcthREREZJPYBBEREZFNYhNERERENolNEBEREdkkNkFERERkk9gEERERkU1iE0REREQ2iU0QERER2SQ2QURERGST2AQRERGRTWITZEX0ej1ycnJQWVnZaN8333yjQCLTlJeXIy0tDV9//TVqamqUjnNX3377rdIRWlReXo7jx4/j1KlTqK2tVTpOI9XV1Q23c3l5Of71r3/h%2BPHjqKqqUjjZLWfOnFE6wj3Lzc1FQUGB0jFUTa2PpaQAQVbh/PnzIiAgQHh7ewutVis%2B%2Bugjo/1arVahZI1dunRJzJ49W0RGRoqLFy%2BKgIAAodVqxaBBg8SUKVNEcXGx0hFbNHToUKUjGBk7dmzDn/Py8sSTTz4p/Pz8hK%2BvrwgKChKFhYUKpjOWkZEhhg8fLr777jvx/fffiyeffFJotVoxePBg8dRTT4m8vDylIwohhPD29hZr164VVVVVSkdpUWFhoZg3b554%2BeWXRUlJiZg1a5bw9vYWPj4%2BIjQ01OrvS9ZITY%2BlpLwH/gtUk5KS7nqZ8PDw%2B5CkZS%2B88AK0Wi1efPFFHD58GHFxcYiPj8eECRMAAFqtFhkZGQqnNAgPD0e7du1w48YNfPfdd5gwYQJWrlyJmpoabNy4EXq9HgkJCUrHxNixY6HRaBptLywsxKOPPgoAOHbs2P2O1cjtt21YWBh69uyJ6Oho1NTUIDY2FqWlpUhOTlY4pUFoaCjGjRuH%2BfPn46WXXkL//v2xZMkS1NXVISEhAdnZ2fj73/%2BudEwMGjQIWq0WpaWliIuLg6%2Bvr9KRmhQeHg5HR0doNBpkZWWhd%2B/eWL9%2BPdq0aYNNmzYBALZs2aJwSgM%2BltKDqI3SASwtJycHx44dg1arhb29faP9TT1JKiE7OxspKSlo06YNQkJC4ObmhuXLl8PT0xM%2BPj5WkxMA0tPT8e9//xvXrl3DqFGjEBUVBTs7Ozg6OiI6Ohrjx49XOiIAYNKkSdixYwfmzp2LPn36AACEEIiNjbWKB%2Bt6t9%2B2Z8%2BeRWJiIjQaDRwcHLBy5Uo89dRTCqYzlpeXh71790Kj0SA7Oxvbtm2DRqOBvb09oqKiMHLkSKUjAgDs7e2xY8cOvPXWW5g9ezZGjx6NOXPmwN/fX%2BloRr7%2B%2Bmt8%2BeWXqK2thb%2B/P95//324ubkBADZs2NDwxG0N%2BFhKD6IHvgl688038dJLL0Gr1VrVE9%2BdHBwccPPmTbi4uAAAnn76aSxYsAARERE4cOAArG1gp9Fo4O7ujqCgINjZ3Tq0rKqqCnV1dQomuyUqKgojRozA2rVr4enpieeffx4A8NprryE4OFjhdE3r3Lkzqqqq0K5dOwCGpq1NG%2Bu5m3bo0AGXLl1Cjx490LVrV5SVleGRRx4BABQXF8PV1VXhhLfY29sjIiICkydPxvbt27FgwQJ07NgR/v7%2B6NKlC5YuXap0RACG%2B1L9ycHBwWi7NR0TxsdSehA98AdG29nZITY2Fv/4xz9QXl6udJxm/e53v8OKFSuQk5PTsO1Pf/oTvLy8MG/ePKtpLADAz88P8fHxqK2tRUJCAhwdHQEA586dQ2RkJMaMGaNwwltGjhyJvXv34tNPP0VkZCSuX7%2BudKRGbt68iXHjxmHx4sVo164dUlJSABiW7dasWYOhQ4cqnPCWkJAQLFq0CCdPnsTChQuxbNkynDp1CsePH8eCBQswceJEpSM20qNHD2zatAn/%2Bc9/sHz5cri4uCA3N1fpWACAJ554AmvWrMHatWvh5OSE5ORklJWV4fLly1i5ciX8/PyUjtiAj6X0IHrgjwmqV1BQAA8Pj4ZX2PXq6uqMJhlK%2BeWXX/DnP/8Z9vb20Ol0Ddv1ej0WL16M48eP4/z58womvKWwsBDh4eHYu3dvQwMEABMmTECfPn3wl7/8xaomAoBhovLWW2/h4MGDKCsrw7fffms1t31paSnOnTvXcHJxcUFCQgI2b96MkydPYtu2bejatavSMQEYrsfk5GTs3r0b169fb3hV3aZNG0ycOBGxsbFG0wyl3O24j9ra2iaXdO63srIybNy4ERcvXsSiRYsghMDKlStRW1uLbt264Z133kH37t2VjmmkucdSa6Gmx1KyAsocj33/JCcnN7uvpKREzJ49%2Bz6mad7dcvr5%2Bd3HNC1rLqter7f66zQ9PV2sXbtWlJaWWnVOIYQoLy%2B3qutTiFtZa2trRV5envjmm2/EuXPnxPXr160qa0v3J2u/7YuLi0V2drZVXZ9CCBEWFiauXbumdIy7CgsLE7/%2B%2Bmuz%2B7Oysu5jGrJ2yr8MtrDU1FQsWbKk0edFnD17FlOnTsWNGzcUSmasPuedn7Vy9uxZTJs2DT179lQoWWPNXacXLlzAtGnTrO46vT3n0KFDERISguDgYKvOCQD5%2BflWdX0Ct7LW1NTAy8sLfn5%2BGDhwIC5evGhVWVu631vjbX/7/b5z586orq62qusTMEytpkyZgqysLKWjtKisrAzBwcHN5uzfv/99TkRWTekuzNJKS0vFjBkzRHBwsLhy5YoQQogPPvhADBw4UERHR4vKykqFExqoJacQ6snaXM7HH39cFTmt7foUQj1ZmVO%2Bmpoa8frrr4vBgweL999/X%2Bk4zVJLTrIOD3wTJIQQlZWVYtWqVSIgIEAsWbJE%2BPr6ig8%2B%2BEDpWI2oJacQ6snKnPKpJStzWsZXX30lxo4dK5YtWyYuXbokfvrpp4aTNVFLTlKWzRwYfeXKFcycOROFhYV44YUXsHLlSqUjNUktOQH1ZGVO%2BdSSlTktIycnBzNmzGhYbhRCQKPRWN0Bx2rJScp54I8JAoBTp05h6tSp6N27N958802kpqYiOjraqr7rCFBPTkA9WZlTPrVkZU7L2LNnD/7whz9g/PjxOHr0KNLS0nDs2DGkpaUpHc2IWnKSwpQdRFleSkqKGDBggHj99ddFXV2dEEKIH3/8UQQFBYmQkJCGdXilqSWnEOrJypzyqSUrc8pXWloqwsLChFarFQcPHlQ6TrPUkpOswwPfBA0ZMkSkpaU12l5eXi7CwsLEyJEjFUjVmFpyCqGerMwpn1qyMqd8AQEBYsqUKeLixYtKR2mRWnKSdXjgm6Affvih2X11dXViy5Yt9y9MC9SSUwj1ZGVO%2BdSSlTnli4mJsap3qzVHLTnJOtjMgdFEREREt7OJA6OJiIiI7sQmiIiIiGwSmyAiIiKySWyCiIiIyCaxCSIiIiKbxCaIiIiIbBKbICIiIrJJbIKIiIjIJv0/90Dmmz1Qo8gAAAAASUVORK5CYII%3D" class="center-img">
</div>
    <div class="row headerrow highlight">
        <h1>Sample</h1>
    </div>
    <div class="row variablerow">
    <div class="col-md-12" style="overflow:scroll; width: 100%%; overflow-y: hidden;">
        <table border="1" class="dataframe sample">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>X1</th>
      <th>X2</th>
      <th>X3</th>
      <th>X4</th>
      <th>X5</th>
      <th>X6</th>
      <th>X7</th>
      <th>X8</th>
      <th>Y1</th>
      <th>Y2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.98</td>
      <td>514.5</td>
      <td>294.0</td>
      <td>110.25</td>
      <td>7.0</td>
      <td>2</td>
      <td>0.0</td>
      <td>0</td>
      <td>15.55</td>
      <td>21.33</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.98</td>
      <td>514.5</td>
      <td>294.0</td>
      <td>110.25</td>
      <td>7.0</td>
      <td>3</td>
      <td>0.0</td>
      <td>0</td>
      <td>15.55</td>
      <td>21.33</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.98</td>
      <td>514.5</td>
      <td>294.0</td>
      <td>110.25</td>
      <td>7.0</td>
      <td>4</td>
      <td>0.0</td>
      <td>0</td>
      <td>15.55</td>
      <td>21.33</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.98</td>
      <td>514.5</td>
      <td>294.0</td>
      <td>110.25</td>
      <td>7.0</td>
      <td>5</td>
      <td>0.0</td>
      <td>0</td>
      <td>15.55</td>
      <td>21.33</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.90</td>
      <td>563.5</td>
      <td>318.5</td>
      <td>122.50</td>
      <td>7.0</td>
      <td>2</td>
      <td>0.0</td>
      <td>0</td>
      <td>20.84</td>
      <td>28.28</td>
    </tr>
  </tbody>
</table>
    </div>
</div>
</div>




```python
sns.pairplot(df);
```


![png](output_5_0.png)



```python
profilereport.to_file(outputfile='Pandas_ProfilingOutput.html')
```

## [Detail HTML Pandas Profiling](Pandas_ProfilingOutput.html)

# Training Dataset Preparation
## Dataset Preprocessing
### Feature Selection
![title](x6.png)
<br>
In the Pearson Correlation and  the seaborn pairplot, we know that __X6__ Which is _'Orientation'_ doesn't affect the contribution where having 0 Correlation with other features. <br>
in this case, we will not using X6 as a training feature.


```python
X = df.drop(columns=['X6','Y1','Y2']).values
# Drop X6 features 
y = df[['Y1','Y2']].values
```


```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.20, random_state=5)

print("X train size : ",X_train.shape)
print("X test size : ",X_test.shape)
```

    X train size :  (614, 7)
    X test size :  (154, 7)
    

# Machine Learning Modeling

## Regression Model

Using 3 Regression model :
1. model_1 using LinearRegression
2. model_2 using SVR
3. model_3 using DecisionTreeRegressor
4. model_4 using GaussianProcessRegressor

### Import Library and Defining Model


```python
from sklearn.linear_model import LinearRegression
from sklearn.svm import SVR
from sklearn.multioutput import MultiOutputRegressor
from sklearn.tree import DecisionTreeRegressor
from sklearn.gaussian_process import GaussianProcessRegressor

model_1=LinearRegression()
model_2=MultiOutputRegressor(SVR())
model_3=DecisionTreeRegressor()
model_4=GaussianProcessRegressor()
```

### Training Model


```python
model_1.fit(X_train,y_train)
model_2.fit(X_train,y_train)
model_3.fit(X_train,y_train)
model_4.fit(X_train,y_train)
```




    GaussianProcessRegressor(alpha=1e-10, copy_X_train=True, kernel=None,
                 n_restarts_optimizer=0, normalize_y=False,
                 optimizer='fmin_l_bfgs_b', random_state=None)




```python
print("Model_1 score : ",model_1.score(X_test,y_test))
print("Model_2 score : ",model_2.score(X_test,y_test))
print("Model_3 score : ",model_3.score(X_test,y_test))
print("Model_4 score : ",model_4.score(X_test,y_test))
```

    Model_1 score :  0.8946319008299157
    Model_2 score :  0.9084879994487434
    Model_3 score :  0.9691885552790778
    Model_4 score :  0.9691783692593766
    

### R2 Score


```python
ypred_model1 = model_1.predict(X_test)
ypred_model2 = model_2.predict(X_test)
ypred_model3 = model_3.predict(X_test)
ypred_model4 = model_4.predict(X_test)

from sklearn.metrics import r2_score

r2_model1 = r2_score(y_test,ypred_model1)
r2_model2 = r2_score(y_test,ypred_model2)
r2_model3 = r2_score(y_test,ypred_model3)
r2_model4 = r2_score(y_test,ypred_model4)
```


```python
print("R2 Score Model 1 : ",r2_model1)
print("R2 Score Model 2 : ",r2_model2)
print("R2 Score Model 3 : ",r2_model3)
print("R2 Score Model 4 : ",r2_model4)
```

    R2 Score Model 1 :  0.8937846230881794
    R2 Score Model 2 :  0.9084879994487434
    R2 Score Model 3 :  0.9670858781551908
    R2 Score Model 4 :  0.9670747955119392
    

## Hyperparameter search using GridCV on model_3 (DecisionTreeReggressor)
### 1. Get Current Params


```python
model_3.get_params()
```




    {'criterion': 'mse',
     'max_depth': None,
     'max_features': None,
     'max_leaf_nodes': None,
     'min_impurity_decrease': 0.0,
     'min_impurity_split': None,
     'min_samples_leaf': 1,
     'min_samples_split': 2,
     'min_weight_fraction_leaf': 0.0,
     'presort': False,
     'random_state': None,
     'splitter': 'best'}



### 2. Tuning params DecisonTreeRegressor : _min samples leaf_ and _min samples split_


```python
from sklearn.model_selection import GridSearchCV
dtr_tuned_params =  { 'min_samples_leaf': [1, 3, 5],
                  'min_samples_split' : [2, 3, 5]}
GridDTR = GridSearchCV(DecisionTreeRegressor(),dtr_tuned_params, cv=5)
GridDTR.fit(X,y)

print("Best Params : ",GridDTR.best_params_)
print()
means = GridDTR.cv_results_['mean_test_score']
stds = GridDTR.cv_results_['std_test_score']

for mean, std, params in zip(means, stds, GridDTR.cv_results_['params']):
        print("%0.3f (+/-%0.03f) for %r"
              % (mean, std * 2, params))
```

    Best Params :  {'min_samples_leaf': 3, 'min_samples_split': 2}
    
    0.962 (+/-0.083) for {'min_samples_leaf': 1, 'min_samples_split': 2}
    0.962 (+/-0.083) for {'min_samples_leaf': 1, 'min_samples_split': 3}
    0.962 (+/-0.083) for {'min_samples_leaf': 1, 'min_samples_split': 5}
    0.963 (+/-0.082) for {'min_samples_leaf': 3, 'min_samples_split': 2}
    0.963 (+/-0.082) for {'min_samples_leaf': 3, 'min_samples_split': 3}
    0.963 (+/-0.082) for {'min_samples_leaf': 3, 'min_samples_split': 5}
    0.960 (+/-0.079) for {'min_samples_leaf': 5, 'min_samples_split': 2}
    0.960 (+/-0.079) for {'min_samples_leaf': 5, 'min_samples_split': 3}
    0.960 (+/-0.079) for {'min_samples_leaf': 5, 'min_samples_split': 5}
    

### 3. Apply best params to fit the model training


```python
Optimized_model=DecisionTreeRegressor(min_samples_leaf=3,min_samples_split=2)

Optimized_model.fit(X_train,y_train)
y_pred = Optimized_model.predict(X_test)
print()
print("R2 Score after Optimizing params : ",r2_score(y_test,y_pred))
```

    
    R2 Score after Optimizing params :  0.9667814327179937
    
