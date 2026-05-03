# 📊 Twitter Engagement & Sentiment Dashboard

This application integrates real-time data from Twitter to perform sentiment analysis and engagement forecasting. The application fetches tweets containing the term "iPhone", which can be any kind of product or keyword. Utilizing Twitter data and Google Trends, I explore how the product is perceived in terms of engagement, sentiment and interest over time. I combine sentiment analysis from tweets with trend analysis from Google. It also offers region-based insights and engagement metrics. I provide insights into consumer sentiment, engagement patterns and geographic interest. These insights can help professionals make informed decisions by identifying emerging products before they are on the market, spotting early trends and patterns and by evaluating a potential for a product to become a market hype. 

## Table of Contents

- [Features](#features)  
- [Prerequisites](#prerequisites)  
- [Installation](#installation)
- directory structure
- Key Modules and Components
- [Database Setup](#database-setup)  
  - [1. Elasticsearch](#1-elasticsearch)  
  - [2. SQLite](#2-sqlite)  
  - [3. Mysql](#3mysql)  
- [Running](#running)  
- [Running the Dashboard](#running-the-dashboard)  
- [Project Structure](#project-structure)  


---

## Features

- Fetches recent tweets via Twitter API v2  
- Computes engagement metrics (likes, retweets, clicks, adjusted engagement, sentiment-weighted, etc.)  
- Stores data in:
  1. **Elasticsearch** 
  2. **SQLite** 
  3. **MySQL** 
- Interactive Streamlit dashboard with:
  - Time-series forecasts (Prophet)
  - Engagement over time by metric & location
  - Hashtag engagement tables
  - Save & view “saved” metrics

---

## Prerequisites


- Here are the external Python libraries this project uses:
- pandas
- matplotlib
- textblob
- tweepy
- elasticsearch & elasticsearch-dsl
- prophet
- streamlit
---

## Installation

1. **Clone the repo**  
   ```bash
   git clone https://github.com/your-username/your-repo.git
   cd your-repo

   # Install the dependencies from the requirements file
pip install -r requirements.txt
```text

##  Directory Structure
├── src/
│   ├── config/          # Settings and environment loading
│   ├── models/          # Data structure definitions (e.g., Elasticsearch Document)
│   ├── repositories/    # Data Access Objects (Twitter, ES, MySQL, SQLite)
│   └── services/        # Business logic (Cleaning, Analysis, Auth)
├── dashboard.py         # Streamlit UI entry point
├── main.py              # Data ingestion entry point
├── requirements.txt     # Project dependencies
└── .env.example         # Template for configuration

```

## 4. Key Modules and Components

### 4.1 Authentication and User Management (Auth Service)
The `AuthService` handles all security-critical user operations:
*   **Registration**: Validates input, hashes passwords using Argon2id, and persists user data.
*   **Login**: Verifies credentials against the MySQL database and manages session states.
*   **Audit Integration**: Automatically logs all authentication attempts to the centralized audit repository.

### 4.2 Data Processing (Twitter Service)
The `TwitterService` is responsible for the transformation of raw API data into actionable insights:
*   **Cleaning**: Uses NLTK and Regex to remove URLs, handle emojis, strip stopwords, and perform lemmatization.
*   **Sentiment Analysis**: Utilizes `TextBlob` to assign a polarity score to each tweet.
*   **Engagement Calculation**: A custom algorithm that calculates "Final Engagement" based on likes, retweets, replies, clicks, and follower influence.

### 4.2 Analytics and Forecasting (Analysis Service)
This service handles complex data operations for the UI:
*   **Time-Series Forecasting**: Uses the **Facebook Prophet** library to predict future sentiment and engagement trends based on historical data.
*   **Hashtag Analysis**: Aggregates engagement metrics across different hashtags to identify high-performing topics.
*   **Location Tracking**: Processes and counts user locations for demographic filtering.

### 4.3 Data Management (Repositories)
*   **Twitter Repository**: Interfaces with the Tweepy library to search for recent tweets.
*   **Elasticsearch Repository**: Manages high-speed document storage and search for processed tweets.
*   **MySQL Repository**: Handles persistent user account data.
*   **SQLite Repository**: Manages a local database for "favorite" or "saved" engagement snapshots.

---

## 5. Application Workflow

### 5.1 Ingestion Pipeline (`main.py`)
1.  **Initialize**: Sets up the connection to Elasticsearch and prepares the index.
2.  **Fetch**: Retrieves the latest tweets for a specific product keyword (e.g., "iPhone").
3.  **Process**: Cleans the text and calculates engagement metrics.
4.  **Store**: Saves the enriched data into Elasticsearch for later analysis.
5.  **Visualize**: Generates a quick matplotlib plot showing immediate engagement trends.

### 5.2 Interactive Dashboard (`dashboard.py`)
The dashboard provides several views for the user:
*   **Twitter Sentiment**: Displays historical sentiment and a forecasted trend line for the next hour.
*   **Engagement Overview**: Allows filtering of metrics by user location and displays a table of top hashtags.
*   **Favorites**: Shows data points that the user has specifically saved to the local SQLite database.
*   **User Management**: Provides a tabbed interface for new user registration.




## Database Setup

This application uses three databases: **Elasticsearch**, **SQLite**, and **(Optional) MySQL/PostgreSQL** for storing and querying data. Follow the steps below to set up each of them.



### 1. Elasticsearch
Used for fast querying and full-text search of tweet data.

Download & Extract Elasticsearch (v8.x):
      ```bash
      # Download Elasticsearch
      wget [https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.6.2-linux-x86_64.tar.gz](https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.6.2-linux-x86_64.tar.gz)
      # Change into the directory
      cd elasticsearch-8.6.2/

Start elasticsearch

### Start Elasticsearch in the background
        ```bash
        bin/elasticsearch &

Wait a few seconds, then verify it's running:

        ```bash
        curl -X GET "localhost:9200"
        
✅ If successful, you'll see Elasticsearch's JSON response and can also open http://localhost:9200 in your browser. In the code I use an orm.


### SQLite
No installation is needed. A file named engagement_data.db will be automatically created in the root folder when you first run the app.

### MySQL Setup 
Download and install xampp. Start apache and a mysql server.
Create a Database and the tables with sql. The create statements are inside DB.sql.

### Run the python code to get twitter and google data

Create a Twitter Developer account and copy the Bearer Token into the python file. You can get 100 posts from the twitter feed with it.

This Python script automates the process of collecting, analyzing, storing and visualizing Twitter data for a specific product (in this case, "iPhone"). Here's what it does:

Connects to the Twitter API using the Tweepy library and search tweets about a given product, excluding retweets. It is saving 100 tweets in the elasticsrach db.

Extracts key tweet data such as text, timestamps, likes, retweets, replies, user location and follower count. It also performs sentiment analysis on each tweet using TextBlob.

Calculates various engagement metrics, including adjusted engagement and influencer impact (based on follower count) and sentiment-weighted engagement.

Stores the processed tweet data in an Elasticsearch index via with an ORM.



### How to run the dashboard

To run the Streamlit dashboard, navigate to the project's root directory in your terminal and execute the following command:

        ```bash
        streamlit run dashboard.py

This will open your browser and open a webpage with the dashboard.
![image](https://github.com/user-attachments/assets/1777210c-79a5-4032-a229-8c9669b172cf)
This is a image of the dashboard.
