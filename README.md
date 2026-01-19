<p align="center">
  <img src="https://www.especial.gr/wp-content/uploads/2019/03/panepisthmio-dut-attikhs.png" alt="UNIWA" width="150"/>
</p>

<p align="center">
  <strong>UNIVERSITY OF WEST ATTICA</strong><br>
  SCHOOL OF ENGINEERING<br>
  DEPARTMENT OF COMPUTER ENGINEERING AND INFORMATICS
</p>

<hr/>

<p align="center">
  <strong>Information Technology Security</strong>
</p>

<h1 align="center" style="letter-spacing: 1px;">
  Android Repackaging
</h1>

<p align="center">
  <strong>Vasileios Evangelos Athanasiou</strong><br>
  Student ID: 19390005
</p>

<p align="center">
  <a href="https://github.com/Ath21" target="_blank">GitHub</a> ·
  <a href="https://www.linkedin.com/in/vasilis-athanasiou-7036b53a4/" target="_blank">LinkedIn</a>
</p>

<hr/>

<p align="center">
  <strong>Supervision</strong>
</p>

<p align="center">
  Supervisor: Ioanna Kantzavelou, Associate Professor<br>
</p>

<p align="center">
  <a href="https://ice.uniwa.gr/en/emd_person/ioanna-kantzavelou/" target="_blank">UNIWA Profile</a> ·
  <a href="https://www.linkedin.com/in/ioanna-kantzavelou-74685934/" target="_blank">LinkedIn</a>
</p>


<p align="center">
  Co-supervisor: Angelos Georgoulas, Assistant Professor<br>
</p>

<p align="center">
  <a href=https://scholar.google.com/citations?user=Djium2IAAAAJ&hl=en" target="_blank">UNIWA Profile</a> ·
  <a href="https://www.linkedin.com/in/aggelos-georgoulas/?originalSubdomain=uk" target="_blank">LinkedIn</a>
</p>

</hr>


<p align="center">
  Athens, June 2023
</p>


---

# Project Overview

This project investigates the potential relationship between socio-economic factors—specifically unemployment and poverty rates and incidents of police killings across various U.S. states between 2015 and 2016

---

## Table of Contents

| Section | Folder | Description |
|------:|--------|-------------|
| 1 | `assign/` | Assignment material for the Business Data & Management course |
| 1.1 | `assign/Assignment-BDMGMT-Apr24.pdf` | Assignment description in English |
| 1.2 | `assign/Εργασία-ΔΧΜΚ-Απρ24.pdf` | Assignment description in Greek |
| 2 | `docs/` | Documentation and reports on US police killings |
| 2.1 | `docs/Police-Killings-US.pdf` | English report |
| 2.2 | `docs/Δολοφονίες-Αστυνομικών-ΗΠΑ.pdf` | Greek report |
| 3 | `graphs/` | Visualizations and charts of datasets |
| 3.1 | `graphs/2015-*.png` | Various 2015 charts: elbow method, optimal clusters, percentages, logs |
| 3.2 | `graphs/2016-*.png` | Various 2016 charts: elbow method, optimal clusters, percentages, logs |
| 3.3 | `graphs/avg-*.png` | Average charts across years |
| 3.4 | `graphs/Clustering3-*.png` | Charts for 3-dataset clustering experiments |
| 4 | `src/` | Source code, datasets, and notebooks |
| 4.1 | `src/datasets/` | Raw datasets in CSV and JSON formats |
| 4.2 | `src/jupyter/` | Jupyter notebooks for analysis and preprocessing |
| 4.3 | `src/processed_datasets/` | Cleaned and processed datasets |
| 4.4 | `src/python/` | Python scripts for clustering and preprocessing |
| 5 | `README.md` | Repository overview, instructions, and summary |

## Project Overview

The research explores whether states with higher levels of unemployment and poverty also exhibit higher frequencies of police brutality. By grouping data into clusters, the study seeks to identify patterns and correlations that can inform social policy and crime understanding.

---

## Key Objectives

- **Correlation Analysis**  
  Investigating whether unemployment rates correlate with homicide incidents.

- **Pattern Detection**  
  Using clustering techniques to observe geographical and economic patterns in police killings.

- **Policy Support**  
  Providing data-driven insights for sociologists, economists, and policymakers to address social issues.

---

## Datasets

The analysis utilizes three primary datasets sourced from platforms such as **Kaggle** and **Opendatasoft**:

- **Police Killings Dataset**  
  Includes data on victims (age, sex, race), location (state/city), cause of death, and whether body cameras were used.

- **Unemployment / Poverty Dataset**  
  Contains poverty rates and absolute numbers of unemployed individuals per state.

- **US City Populations Dataset**  
  Experimental data for cities with more than 65,000 inhabitants, used to normalize results against total state populations.

---

## Methodology

The team employed **Cluster Analysis** using the **K-Means algorithm**. This method was chosen for its efficiency with relatively small datasets (51 entries) and its ability to detect hidden patterns without requiring pre-labeled data.

---

## Data Preprocessing

- **Time Alignment**  
  Data was pruned to include only the years **2015 and 2016** to ensure a common temporal scale.

- **Morphology Normalization**  
  State names were standardized to abbreviations (e.g., *New York → NY*) to enable successful dataset joining.

- **Normalization**  
  Logarithmic normalization and standard deviation scaling were applied to account for significant differences in population size between states.

- **Handling Data Gaps**  
  States with zero recorded murders (e.g., *Rhode Island in 2015*) were manually assigned a value of `0` rather than being omitted.

---

## Evaluation Metrics

To assess cluster quality, the following non-predictive evaluation metrics were used:

- **SSE (Sum of Squared Error)**  
  Measures the deviation of actual values from cluster centroids, indicating how close data points are within clusters.

- **Silhouette Coefficient**  
  Measures how well data points are separated between clusters; values closer to `1` indicate better clustering.

---

## Experimental Results

The analysis was conducted in **four distinct stages**, progressing from percentage-based metrics to log-normalized absolute values to improve accuracy.

| Analysis Stage       | Metric      | Year 2015 | Year 2016 |
|----------------------|-------------|-----------|-----------|
| Percentage Rates     | SSE         | 25,464    | 29,151    |
|                      | Silhouette  | 0.518     | 0.495     |
| Net Numbers          | SSE         | 18,885    | 19,478    |
|                      | Silhouette  | 0.481     | 0.484     |
| Log Normalized       | SSE         | 22,391    | —         |
|                      | Silhouette  | 0.554     | —         |

---


## Technologies Used

- **Programming Language:** Python 3  
- **Data Analysis & Machine Learning:**  
  - K-Means Clustering (Unsupervised Learning)  
  - Z-score Normalization  
  - Logarithmic Normalization  
- **Big Data & Statistical Concepts:**  
  - Cluster Analysis  
  - Socio-economic Data Correlation  
  - Population Normalization Factors  
- **Data Structures:**  
  - Pandas DataFrames  
  - NumPy Arrays  
- **Libraries & Frameworks:**  
  - `pandas` (data loading, preprocessing, merging)  
  - `numpy` (numerical operations, transformations)  
  - `scikit-learn` (KMeans, Silhouette Coefficient, SSE)  
  - `scipy` (`zscore` normalization)  
  - `matplotlib` (data visualization)  
  - `json` (US states name mapping)  
  - `os` (console handling)  
- **Development Environment:**  
  - Python scripts (`.py`)  
  - Jupyter Notebooks (`.ipynb`)  

---

# Installation & Run Guide

## Prerequisites

This project requires **Python 3** to be installed on your system.

Verify your Python installation by running:
```bash
python --version
```
or
```bash
python3 --version
```
If Python is not installed, download it from:

https://www.python.org/downloads/

Additionally, install the required Python libraries:

```bash
pip install pandas numpy scikit-learn scipy matplotlib
```

## Installation
Clone the repository to your local machine:

```bash
git clone https://github.com/Big-Data-Management-aka-Uniwa/US-Police-Killing-Search.git
```

Navigate to the project directory:
```bash
cd US-Police-Killing-Search/src/python
```

Ensure the following folder structure exists:

```
datasets/
processed_datasets/
```

## Data Preprocessing
Before running the clustering experiments, preprocess the raw datasets:

```bash
python preprocessData.py
```

This step:
- Filters police killing and poverty data for 2015–2016
- Normalizes U.S. state names to two-letter abbreviations
- Computes population statistics
- Produces cleaned datasets in processed_datasets/
- Generates average datasets for 2015–2016

### Run Clustering (2 Datasets)
Execute clustering using Police Killings & Poverty datasets:

```bash
python Clustering_2_Datasets.py
```
You will be prompted to select:
- Year (2015 / 2016 / Average)
- Data representation (rates, absolute numbers, logarithmic normalization)
- Number of clusters (k)

The program outputs:
- Cluster visualizations
- SSE (Sum of Squared Errors)
- Silhouette Coefficient
- Elbow Method plot for optimal k

## Run Clustering (3 Datasets)
Execute clustering with Police Killings, Poverty, and Population normalization:

```bash
python Clustering_3_Datasets.py
```
This version:
- Normalizes killings and poverty by total state population
- Performs K-Means clustering
- Visualizes clusters and centroids
- Reports SSE and Silhouette metrics

## Jupyter Notebook Support
All source files are also implemented as Jupyter Notebooks (.ipynb), allowing:
- Interactive execution
- Step-by-step analysis
- Inline visualizations

Launch Jupyter Notebook with:
```bash
jupyter notebook
```

Then open the corresponding .ipynb files by navigating to project directory

Navigate to the project directory:
```bash
cd US-Police-Killing-Search/src/jupyter
```

## Output
- Cluster scatter plots with centroids
- Quantitative clustering metrics (SSE, Silhouette Coefficient)
- CSV files containing processed and merged datasets

The analysis terminates after all clusters and evaluation metrics are displayed.

---

## Open the Documentation
1. Navigate to the `docs/` directory
2. Open the report corresponding to your preferred language:
    - English: `Police-Killings-US.pdf`
    - Greek: `Δολοφονίες-Αστυνομικών-ΗΠΑ.pdf`