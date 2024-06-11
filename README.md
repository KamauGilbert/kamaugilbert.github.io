# 📊 YouTube Influencer Marketing Project

This Data Analytics project assists marketers in identifying the most suitable YouTuber for promoting product sales, leveraging data-driven insights to maximize marketing effectiveness. 🚀

## 📁 Dataset
🔗 Check out the [Kaggle dataset](https://www.kaggle.com/datasets/bhavyadhingra00020/top-100-social-media-influencers-2024-countrywise?resource=download) for a comprehensive collection of data sourced from various social media platforms as of the first half of 2024.

## 📈 Approach

### 1. Viewing and Preparing the Dataset
- **Dataset:** [Top 100 UK YouTubers file](assets/dataset/youtube_data_united-kingdom.csv)
- **Mockup:** Created a mockup of the final dashboard view to understand the desired layout.
![Dashboard Mockup](assets/images/dashboard_mockup.png)

### 2. Data Extraction
- **Script:** Used a Python script to extract data from YouTube since the Kaggle dataset contained information in a foreign language.
  - **Script Path:** [extract_data_from_youtube_api.py](assets/python_script/extract_data_from_youtube_api.py)
  - **Extracted Dataset:** [youtube_data_from_python.csv](assets/dataset/youtube_data_from_python.csv)

### 3. Loading Data into SQL Server
- **Database:** Created a database called `youtube_db`.
- **Table:** Loaded the `youtube_data_from_python.csv` file into SQL Server and renamed the table to `top_uk_youtubers_2024`.
- **Data Cleaning:** Created the view `view_uk_youtubers_2024` with the following steps:

```sql
/*
# Data Cleaning Steps:
1. Remove unnecessary columns.
2. Extract the YouTube channel names.
3. Rename the column names.
*/

-- Select relevant columns
SELECT
    NOMBRE,
    total_subscribers,
    total_views,
    total_videos
FROM top_uk_youtubers_2024;

-- Extract channel names using CHARINDEX function
SELECT CHARINDEX('@', NOMBRE), NOMBRE FROM top_uk_youtubers_2024;

-- Using SUBSTRING function
SELECT 
    CAST(SUBSTRING(NOMBRE, 1, CHARINDEX('@', NOMBRE) - 1) AS VARCHAR(100)) AS channel_name,
    total_subscribers,
    total_views,
    total_videos
FROM top_uk_youtubers_2024;

-- Create view with relevant data
CREATE VIEW view_uk_youtubers_2024 AS
SELECT 
    CAST(SUBSTRING(NOMBRE, 1, CHARINDEX('@', NOMBRE) - 1) AS VARCHAR(100)) AS channel_name,
    total_subscribers,
    total_views,
    total_videos
FROM top_uk_youtubers_2024;
```
### 4. Data Quality Checks

```sql
/*
# Data Quality Tests:

1. Row count: Ensure there are 100 records.
2. Column count: Ensure there are 4 fields.
3. Data type checks: Validate data types.
4. Duplicate count check: Ensure records are unique.
*/

-- Row Count Test
SELECT COUNT(*) AS number_of_rows FROM view_uk_youtubers_2024;

-- Column Count Test
SELECT COUNT(*) AS column_count FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'view_uk_youtubers_2024';

-- Data Type Check
SELECT
    COLUMN_NAME,
    DATA_TYPE
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_NAME = 'view_uk_youtubers_2024';

-- Duplicate Count Check
SELECT
    channel_name,
    COUNT(*) AS duplicate_count
FROM 
    view_uk_youtubers_2024
GROUP BY 
    channel_name
HAVING 
    COUNT(*) > 1;
```



