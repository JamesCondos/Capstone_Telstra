# Network performance prediction for Telstra’s radio access network

**James Condos and Sithira Jayasinghe**

In this engineering capstone with Telstra and the University of Melbourne, we asked how well six hours of 4G network measurements can predict the next hour. Several frequency layers can cover the same area. One layer can become busy while another still has capacity. A forecast of demand and radio conditions could help engineers compare load-balancing settings before the busy period arrives.

[Read the capstone paper (PDF)](./Engineering_Capstone_Report_Paper.pdf)

## What we forecast

- **Sector performance.** A sector covers a directional area around a mobile site. We forecast ten performance indicators for each sector, including throughput, spectral efficiency and service drop rates. We compared a model shared across sectors with models trained on groups of sectors that had similar traffic patterns.
- **Cell conditions.** Individual cells within a sector can operate on different frequencies. We forecast twelve measurements of cell load and radio conditions. These are inputs to an existing model that estimates how the network would perform under a candidate configuration.
- **Site throughput.** In a separate experiment, we forecast average downlink throughput for an entire site. We compared an adapted Qwen2.5-3B-Instruct language model with smaller neural networks using the same forecast windows.

## Data and evaluation

Telstra supplied measurements recorded every 15 minutes from 6 February to 15 April 2026. The forecasting dataset covered **69 days, 453 sectors and 1,299 cells** before cleaning. The neural models used 24 observations to predict values 15, 30, 45 and 60 minutes ahead.

For the sector and cell experiments, we split sectors into training, validation and test sets (70%, 15% and 15%). Every cell from a given sector stayed in the same set. This tested how well a model carried over to unfamiliar sectors during the observation period. The site-throughput experiment used a chronological split to test forecasts on later observations.

## What we found

A residual multilayer perceptron (MLP) learned to predict the change from the latest measurement. We compared it with **persistence**, a baseline that repeats that measurement at every forecast horizon. The MLP reduced mean absolute error (MAE) by **14.1%** on average across the ten sector indicators and **9.7%** on average across the twelve cell inputs. It improved on persistence for eleven of the twelve cell inputs (Tables 16 and 22).

For site throughput, the long short-term memory network (LSTM) had the lowest error across **2,384 test windows from 149 sites**. The figures below pool all four forecast horizons (Table 26).

| Site-throughput forecast | MAE (Mbit/s) |
| --- | ---: |
| Repeat the latest measurement | 4.881 |
| Repeat the past hour’s mean | 4.801 |
| Adapted Qwen2.5-3B-Instruct | 4.422 |
| MLP | 4.366 |
| CNN–LSTM–MLP | 4.342 |
| LSTM | **4.281** |

## Scope and next steps

The 69-day study leaves open how the sector and cell models perform across seasons or after network configuration changes. A next step is to feed the cell forecasts into the existing performance model and measure how often forecast errors change the choice of configuration. Establishing whether those choices improve service would require controlled network trials.

This repository currently contains the paper. Project code will be added later.
