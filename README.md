# DS2002 Midterm: Walmart Hurricane Predictions
**Team Members:** Henna Panjshiri, Kaavya Indrayan, Amran (Dina) Belete, Maya Yonathan

## Project Overview

For this project, we analyzed Walmart transaction data leading up to Hurricane Helene in September 2024 to understand how customer purchasing behavior changes before a hurricane. We built a data cleaning pipeline, standardized product SKUs, integrated historical weather data, and investigated demand patterns across Florida stores.

Our analysis addresses five questions covering demand surges, the Pop-Tarts effect, weather correlations, geographic patterns, and whether increased board game sales represent a genuine demand signal.

## Important Links

- **GitHub Repository:** [Walmart Hurricane Project](https://github.com/hpanjshiri/walmart-hurricane-project)
- **Main Python Notebook:** [team_notebook.ipynb](https://github.com/hpanjshiri/walmart-hurricane-project/blob/main/notebooks/team_notebook.ipynb)
- **Final Presentation:** [DS_2002_Presentation.pdf](https://github.com/hpanjshiri/walmart-hurricane-project/blob/main/DS_2002_Presentation.pdf)
- **Presentation Speaker Notes:** [Presentation_Speaker_Notes.pdf](https://github.com/hpanjshiri/walmart-hurricane-project/blob/main/Presentation_Speaker_Notes.pdf)

## How to Run the Notebook

Our main notebook is designed to run directly in **Google Colab**. The first code cell automatically clones our GitHub repository and accesses the required data files, so there is no need to manually download or upload any CSV files or the SQLite database.

**Steps:**

1. Open our [main notebook](https://github.com/hpanjshiri/walmart-hurricane-project/blob/main/notebooks/team_notebook.ipynb) on GitHub.
2. Click **Open in Colab**, or open the notebook through [Google Colab](https://colab.research.google.com/) using its GitHub import option.
3. In Colab, select **Runtime → Restart session and run all**.
4. Let the notebook to execute from top to bottom. The code will automatically clone the repository, load the data, perform cleaning, retrieve historical weather data, and generate the analyses and visualizations.

The notebook uses Python libraries including `pandas`, `numpy`, `matplotlib`, `sqlite3`, and `requests`. The weather data comes from the [Open-Meteo Historical Weather API](https://open-meteo.com/en/docs/historical-weather-api), which does not require an API key.

**Note:** An internet connection is required for the GitHub clone and weather API requests. The notebook should be run in Google Colab because its setup uses Colab-specific commands and file paths.

## Notebook Contents

The notebook includes:

- **Data Loading:** Imports transaction, store, product catalog, and distribution center CSV files, along with SQLite inventory and sales data.
- **Data Cleaning:** Handles duplicates, missing values, inconsistent timestamps, SKU variants, category names, and data types.
- **Question 1:** Identifies the largest pre-hurricane product category demand surges.
- **Question 2:** Consolidates Strawberry Pop-Tarts SKUs and compares fragmented versus standardized sales.
- **Question 3:** Integrates historical weather data and examines correlations with daily sales.
- **Question 4:** Compares demand patterns across Florida store locations.
- **Question 5:** Investigates whether increased board game sales represent a genuine demand signal or a possible data artifact.
- **Reflections:** Documents our team's experiences, decisions, challenges, and takeaways.

## Reflection Questions

Our reflection responses are included at the **bottom of the main notebook** and are reproduced below for convenience.

### 1. Data Quality Impact

One data quality issue we encountered was when handling missing timestamps. After our initial cleaning, we still faced 884 transactions with no valid date, which was very important because our analysis was dependent on comparing daily sales before Hurricane Helene. Rather than deleting these transactions or assigning estimated dates, we decided to keep them in our cleaned dataset but not include them in time-based analyses. If we gave incorrect dates, we could have hallucinated an inflated baseline or surge period, changing our 5.14x increase in Breakfast Foods demand. This choice helped us avoid introducing misleading information while keeping in tact transactions for analyses that didn't require dates. It also taught us that data isn't always about fixing or removing bad data, but understanding when certain records are best to use.

### 2. ETL Trade-offs

One important ETL trade-off I made was how to handle missing geographic coordinates when joining Walmart sales data with historical weather data. One store, FL-619 in Tallahassee, was missing its latitude and longitude, which meant we couldn't retrieve weather observations for that location. Instead of dropping the store, I used Tallahassee's approximate city-center coordinates as a proxy and documented the assumption. An alternative would have been to exclude the store entirely, but this would have reduced our geographic coverage and potentially biased the analysis by removing sales from a location affected by Hurricane Helene. Using the proxy allowed us to retain all 12 stores in the weather dataset, although the weather measurements may not perfectly reflect conditions at the actual store. This trade-off taught me that preserving data coverage is valuable, but it must be balanced with accuracy and transparency about assumptions.

### 3. Pipeline Trust

We agree with the CDO that the pipeline posed a greater risk than the analysis. Because Q1–Q5 all used the same cleaned dataset, any cleaning error could have affected every result without warning. Category and SKU standardisation was especially fragile: our initial mapping missed categories such as “Games & Entertainment” and “Health & Wellness,” which validation later caught. We consolidated 37 category spellings into 10 categories and 45 SKU codes into 20 products. Errors in either mapping could have split or merged categories incorrectly or spread Pop-Tarts demand across four codes, undermining the reported 5.14× breakfast surge and board game results. Other weaknesses included a data load that depended on cloning the repo and setting the working directory correctly, and missing Tallahassee coordinates, which we approximated with city-centre values to include all 12 stores in the weather join. The Q5 SQLite cross-check revealed another risk: it had no exact matches with transaction data across 338 store-date observations, limiting the strength of our board game recommendation. The project reinforced that modeling is the easy part; trust in results depends on careful cleaning, validation, and a clear decision log.

### 4. Business vs. Data

For Question 5, we had to decide whether the board games surge was real with data. The transaction data looked genuine. monopoly (2.60x) and playing Cards (1.99x) each rose on their own, all 12 stores surged, and entertainment tracked breakfast day by day (r = 0.98). The SQLite database showed the same surge pattern (about 2.5x), but its volumes did not reconcile with the transactions (0 of 338 store-days matched), so it supported the pattern but not the numbers.

We also treated our evidence as supporting, not proof. Nearly every category rises on the same prestorm calendar, and our measured increase,+125%, was well below the +250% in the case. We weighed that against business risk as well, a small pilot is cheap and board games don't spoil, while stocking at full scale based on one storm and unreconciled data could waste truck space in an emergency. We therefore recommended piloting a modest increase at selected stores, not stocking at scale. With more time or data, we would explain the gap between the database and the transaction file, check inventory stock outs, and test more than one storm.

### 5. Team Collaboration

Our team divided the work pretty evenly, and everyone was responsible for their own sections while still helping each other throughout the project. Everyone brought different strengths to the table, whether that was data cleaning, analysis, problem-solving, or communicating our findings. We also split the reflection questions evenly among ourselves to make sure everyone contributed to the write-up. Overall, I think each teammate made a fair contribution to the project. If we had another week, we would focus on planning things out a little earlier and setting up more regular check-ins with the TAs. We had a brief one toward the end, and having those conversations earlier would have given us more opportunities to get feedback and improve our analysis along the way. Overall, I think we worked really well together, and this project showed us how valuable consistent communication and feedback can be.
