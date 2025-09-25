# Economic-Indicators

# Economic Indicators Analysis

> "What we learn from history is that people don't learn from history" - Warren Buffett

A comprehensive Python toolkit for retrieving, analyzing, and understanding economic data revisions from various sources including FRED, ALFRED, and derived datasets.

## Overview

Economic data is fundamental to financial analysis, policymaking, and investment strategies. However, many economic indicators are subject to revisions, meaning initial estimates may change over time as more accurate data becomes available. This project provides tools to:

- Retrieve economic data from multiple online sources
- Analyze data revisions and their impact on economic indicators
- Detect outliers in historical economic data
- Visualize trends and patterns in economic time series

## Features

### Data Sources
- **FRED (Federal Reserve Economic Data)**: Access to hundreds of thousands of economic data series
- **ALFRED (Archival FRED)**: Historical versions of economic data for revision analysis
- **FRED-MD & FRED-QD**: Monthly and quarterly datasets for macroeconomic research
- **Web Scraping**: Extract data from unstructured web sources
- **API Integration**: Automated data retrieval through APIs

### Key Capabilities
- 📊 **Data Retrieval**: Multiple methods for accessing economic data
- 🔄 **Revision Analysis**: Track how data changes over time
- 📈 **Visualization**: Built-in plotting functions for time series analysis
- 🎯 **Outlier Detection**: Identify anomalies in historical data
- 📋 **Popular Series Tracking**: Monitor most-watched economic indicators

## Installation

### Prerequisites
```bash
pip install numpy pandas matplotlib requests beautifulsoup4 lxml
```

### Required Libraries
- `numpy` - Numerical computing
- `pandas` - Data manipulation and analysis
- `matplotlib` - Plotting and visualization
- `requests` - HTTP requests for API calls
- `beautifulsoup4` - Web scraping
- `lxml` - XML/HTML parsing
- `finds` - Custom library for FRED data access (included)

### API Access
You'll need a free API key from FRED:
1. Register at [FRED API](https://fred.stlouisfed.org/docs/api/api_key.html)
2. Create a `secret.py` file with your credentials:

```python
credentials = {
    'fred': {
        'api_key': 'your_api_key_here'
    }
}
```

## Quick Start

### Basic Data Retrieval

```python
import numpy as np
import pandas as pd
from finds.readers import Alfred, fred_md, fred_qd

# Initialize ALFRED with your API key
alf = Alfred(api_key='your_api_key', verbose=0)

# Get unemployment rate data
unemployment = alf('UNRATE')
print(unemployment.tail())

# Download FRED-MD dataset
url = 'https://www.stlouisfed.org/-/media/project/frbstl/stlouisfed/research/fred-md/monthly/current.csv'
fred_md_data = pd.read_csv(url, header=0)
```

### Popular Economic Indicators

```python
# Get most popular FRED series
popular_series = Alfred.popular(10)
for series in popular_series:
    data = alf(series)
    print(f"{series}: {alf.header(series)}")
```

### Data Revision Analysis

```python
# Analyze revisions for Total Nonfarm Payrolls
series_id = 'PAYEMS'
start_date, end_date = 20230101, 20231231

# Get latest data with revision history
latest_data = alf(series_id, start=start_date, end=end_date, 
                  freq='M', realtime=True)
```

## Project Structure

```
economic-indicators/
├── README.md
├── requirements.txt
├── secret.py                 # API credentials (not in repo)
├── notebooks/
│   └── economic_analysis.ipynb
├── finds/
│   ├── readers.py           # Data retrieval classes
│   ├── utils.py            # Plotting and utility functions
│   └── recipes.py          # Analysis recipes
└── data/
    └── downloads/          # Cached data files
```

## Key Economic Series Covered

### Employment Indicators
- **PAYEMS**: Total Nonfarm Payrolls
- **UNRATE**: Unemployment Rate
- **CES Series**: Current Employment Statistics

### Financial Markets
- **T10Y2Y**: 10-Year Treasury Minus 2-Year Treasury Spread
- **FEDFUNDS**: Federal Funds Rate
- **MORTGAGE30US**: 30-Year Fixed Mortgage Rate

### Economic Growth
- **GDP**: Gross Domestic Product
- **GDPC1**: Real Gross Domestic Product
- **CPIAUCSL**: Consumer Price Index

### Money Supply
- **M1SL**: M1 Money Supply
- **M2SL**: M2 Money Supply

## Data Retrieval Methods

### 1. Structured File Downloads
```python
# Direct CSV download
url = 'https://example.com/data.csv'
df = pd.read_csv(url, header=0)
```

### 2. Web Scraping
```python
import requests
from bs4 import BeautifulSoup

# Scrape FRED popular series
url = "https://fred.stlouisfed.org/tags/series?ob=pv&pageID=1"
response = requests.get(url)
soup = BeautifulSoup(response.content, 'lxml')
```

### 3. API Calls
```python
# FRED API request
api_url = "https://api.stlouisfed.org/fred/series"
params = {
    'series_id': 'UNRATE',
    'file_type': 'json',
    'api_key': 'your_api_key'
}
response = requests.get(api_url, params=params)
```

## Understanding Data Revisions

Economic data revisions are common and can significantly impact analysis:

- **Initial Estimates**: First release, often based on incomplete data
- **Revised Estimates**: Updated figures as more data becomes available
- **Final Estimates**: Most accurate version after all source data is collected

Use ALFRED to track these changes:

```python
# Compare initial vs. revised estimates
vintage_data = alf.vintage_dates(series_id)
initial_estimate = alf(series_id, vintage_dates=vintage_data[0])
latest_estimate = alf(series_id, vintage_dates=vintage_data[-1])
```

## Visualization Examples

The project includes built-in plotting utilities:

```python
from finds.utils import plot_date, plot_groupbar

# Time series plot
fig, ax = plt.subplots(figsize=(12, 6))
plot_date(unemployment_data, ax=ax, title="Unemployment Rate")

# Multi-series comparison
plot_groupbar(data_dict, title="Economic Indicators Comparison")
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-indicator`)
3. Commit your changes (`git commit -am 'Add new economic indicator'`)
4. Push to the branch (`git push origin feature/new-indicator`)
5. Create a Pull Request

## Data Sources & Attribution

- **Federal Reserve Economic Data (FRED)**: Federal Reserve Bank of St. Louis
- **Bureau of Labor Statistics (BLS)**: U.S. Department of Labor
- **U.S. Treasury Department**: Treasury securities data
- **Various Federal Reserve Banks**: Regional economic data

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Disclaimer

This tool is for educational and research purposes. Economic data analysis should be conducted by qualified professionals. Past performance does not guarantee future results.

*"Economic data is fundamental to financial analysis, policymaking, and investment strategies. Understanding revisions is crucial for making informed decisions."*
