# 🎶Exploratory Data Analysis on Spotify 2023 Dataset🎵

##  Introduction

This project delivers a comprehensive exploratory data analysis (EDA) of the **Top Spotify Songs of 2023** dataset, highlighting streaming trends, popular patterns, and insights within contemporary music. Utilizing Python and powerful data visualization tools, this analysis delves into the unique features of highly streamed tracks, artist trajectories, and genre representation to shed light on the elements driving this year's biggest streaming hits. Within this repository, you’ll find the complete code, visualizations, and findings essential for grasping the evolving trends in music streaming.

> **Note:**  
> 💡 This analysis was conducted on the dataset provided on [Kaggle](https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023). Feel free to download the dataset via the linked text for reference.
##  Dataset Overview
 Libraries that are important for data analysis and visualization

 ```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```
The downloaded CSV file for data analysis is loaded into the spotify variable.
```python
# Load the data
spotify = pd.read_csv('spotify-2023.csv')
spotify
```
![image](https://github.com/user-attachments/assets/078f6a03-5074-4a53-88e8-7290bd7d729d)
The dataframe above lists the top 953 songs on Spotify, containing 24 columns.

Input:
```python
print(f"Dataset Dimensions:\nRows: {spotify.shape[0]}, Columns: {spotify.shape[1]}")
spotify.info()

missing_values = spotify.isnull().sum()
missing_values = missing_values[missing_values > 0].reset_index()
missing_values.columns = ['Column', 'Missing Values']
missing_values
```
Output:

![image](https://github.com/user-attachments/assets/9c5ac1fe-0077-44b6-8d49-d964c64f771c)

This display the dataset dimensions, basic information and missing values.

Input:
```python
print(f"Rows: {spotify.shape[0]} \nColumns: {spotify.shape[1]}")
```
Displaying the rows and column of the dataset for better understanding, cleaning, and analyzation of the dataset

## General Statistics 
In this section, we will tackle about the mean,median, and standard deviation of strems, and the distribution of "released_year" and "artist_count", also the noticeable trends or outliers.
```python
spotify.describe()
```
By using .describe() we can get the general statistic of the dataset
![image](https://github.com/user-attachments/assets/fa06c161-9578-4afd-a6cf-df07930844b7)
![image](https://github.com/user-attachments/assets/3fd8950a-d567-47b7-857a-f470ecde4504)

Input:
```python
spotify['streams'] = pd.to_numeric(spotify['streams'].astype(str).str.replace(',', ''), errors='coerce')

mean_streams = spotify['streams'].mean()
median_streams = spotify['streams'].median()
std_streams = spotify['streams'].std()

stream_stats = pd.DataFrame({
    'Statistic': ['Mean', 'Median', 'Standard Deviation'],
    'Value': [mean_streams, median_streams, std_streams]
})
stream_stats.style.format({"Value": "{:.2f}"})
```
Output:

![image](https://github.com/user-attachments/assets/650316ec-be1e-4398-8027-63b8395209d5)

Statistic for streams are shown above.
## Released Year and Artist Count
### Release Year
Input:
```python
# Distribution of release years
year_counts = spotify['released_year'].value_counts().sort_index()

# Plot distribution with rotated x-axis labels for better readability
plt.figure(figsize=(10, 5))
sns.barplot(x=year_counts.index, y=year_counts.values, color='lightblue')
plt.title('Tracks by Release Year')
plt.xlabel('Year')
plt.ylabel('Number of Tracks')
plt.xticks(rotation=90)  # Rotate x-axis labels vertically
plt.tight_layout()
plt.show()

```
Output:
![image](https://github.com/user-attachments/assets/f9e13d55-bcd0-4c89-90aa-12d2c1116ccd)
The data shows a notable surge in track releases in 2022, suggesting that this year had the highest volume of significant music releases. Furthermore, an upward trend in popular music is observed starting from 2014, signaling the beginning of an increase in popular tracks and emerging musical trends.

### Artist Count
input:
```python
# Set Seaborn style and create a simplified artist count distribution plot
plt.figure(figsize=(8, 5))
sns.histplot(spotify['artist_count'], binwidth=1, color="coral", edgecolor="black")
plt.title("Distribution of Tracks by Artist Count", fontsize=14)
plt.xlabel("Number of Artists", fontsize=12)
plt.ylabel("Number of Tracks", fontsize=12)
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.tight_layout()
plt.show()
```
Output:
![image](https://github.com/user-attachments/assets/eb4a1793-29c9-480a-a688-807e8503b428)
The data indicates that most of the released tracks are solo productions, though a significant portion also includes collaborations with other artists.

## Top Performers
In this section, we split it in to 2 parts. The top 5 most streamed tracks, and The top 5 most frequent artist.
### Top 5 Most Streamed Tracks
Input:
```python
spotify['streams'] = pd.to_numeric(spotify['streams'].astype(str).str.replace(',', ''), errors='coerce')
top_5_streams_df = spotify.sort_values(by='streams', ascending=False).head(5).reset_index(drop=True)
top_5_streams_df
```
Output:
![image](https://github.com/user-attachments/assets/c6e24775-c551-44a4-aecb-08410b14553d)
This shows that Blinding Lights by the Weeknd is the most streamed tracks in 2023
### Top 5 Most Frequent 
Input:
```python
top_artists = spotify['artist(s)_name'].str.split(', ').explode().value_counts().nlargest(5).reset_index()
top_artists.columns = ['Artist', 'Track Count']
top_artists
```
Output:

![image](https://github.com/user-attachments/assets/fd098be7-fe54-494d-8410-65ce16452913)

The leading artist during 2023 was Bad Bunny having 40 tracks in the spotify list

## Temporal Trends
In this section, we analyze the trends in the number of tracks released over time, and if the number of tracks released per month follow any noticable patterns.
Input:
```python
# Trend: Number of tracks released per year
# Tracks released per year with vertical bar chart and rotated x-axis labels
plt.figure(figsize=(12, 6))
sns.barplot(x=tracks_per_year.index, y=tracks_per_year.values, color='teal')
plt.title('Number of Tracks Released Per Year', fontsize=14)
plt.xlabel('Year', fontsize=12)
plt.ylabel('Number of Tracks', fontsize=12)
plt.xticks(rotation=45, ha='right')  # Rotate labels for readability
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.tight_layout()
plt.show()


# Trend: Tracks released by month
spotify['released_month'] = pd.to_numeric(spotify['released_month'], errors='coerce')
tracks_per_month = spotify['released_month'].value_counts().sort_index()

plt.figure(figsize=(10, 5))
sns.barplot(x=range(1, 13), y=tracks_per_month, color='skyblue')
plt.title('Tracks Released by Month')
plt.xlabel('Month')
plt.ylabel('Number of Tracks')
plt.xticks(ticks=range(12), labels=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])
plt.show()
```
Output:
![image](https://github.com/user-attachments/assets/36cc920c-9055-4fe2-8534-0c50f62484d4)
![image](https://github.com/user-attachments/assets/dc1e74dc-b024-4816-a0cf-269d7833ec06)
According to the yearly graph, 2022 stands out as a pivotal year, showing a substantial increase in the number of popular tracks released. Meanwhile, the monthly graph reveals a notable spike in popular music releases during January and May, indicating that these months see a peak in track popularity.

## Genre and Music Characteristics
In this section, we examine the connection between music genres and their defining characteristics, such as danceability, valence, energy, BPM, and acousticness. By investigating how these attributes differ across genres, we aim to identify trends and preferences that contribute to streaming success and influence listener choices on Spotify.
### Streams and Attribute Correlation
Input:
```python
# Average musical attributes by release year
attributes_by_year = spotify.groupby('released_year')[['danceability_%', 'energy_%', 'acousticness_%', 'valence_%']].mean()

# FacetGrid for individual attribute trends over time
sns.set(style="whitegrid")
attributes_by_year_melted = attributes_by_year.reset_index().melt(id_vars='released_year', var_name='Attribute', value_name='Average Value')

g = sns.FacetGrid(attributes_by_year_melted, col="Attribute", col_wrap=2, height=4, sharey=False)
g.map(sns.lineplot, 'released_year', 'Average Value', color='teal', marker='o')

g.set_axis_labels("Year", "Average Value (%)")
g.set_titles("{col_name}")
g.add_legend()
plt.suptitle("Yearly Trends of Musical Attributes", y=1.02, fontsize=16, fontweight='bold')
plt.show()
```
Output:
![image](https://github.com/user-attachments/assets/bb52d11d-75fe-4fda-bcaa-7b62caa4c855)
In the FacetGrid analysis, most attributes show a negative correlation with streams, suggesting that these characteristics do not strongly influence track popularity. This could be due to the varying preferences among different listener

### Correlation of Streams and Musical Attributes
Input:
```python
# Set up the figure for subplots
fig, axes = plt.subplots(1, 4, figsize=(24, 6))

# Scatter plot for Streams vs Danceability
sns.scatterplot(ax=axes[0], x='danceability_%', y='streams', data=spotify, s=100, color='skyblue', edgecolor='black')
axes[0].set_title("Streams vs Danceability %", fontsize=14)
axes[0].set_xlabel("Danceability (%)", fontsize=12)
axes[0].set_ylabel("Streams", fontsize=12)

# Scatter plot for Streams vs BPM
sns.scatterplot(ax=axes[1], x='bpm', y='streams', data=spotify, s=100, color='lightgreen', edgecolor='black')
axes[1].set_title("Streams vs BPM", fontsize=14)
axes[1].set_xlabel("BPM", fontsize=12)
axes[1].set_ylabel("Streams", fontsize=12)

# Scatter plot for Streams vs Energy
sns.scatterplot(ax=axes[2], x='energy_%', y='streams', data=spotify, s=100, color='orange', edgecolor='black')
axes[2].set_title("Streams vs Energy %", fontsize=14)
axes[2].set_xlabel("Energy (%)", fontsize=12)
axes[2].set_ylabel("Streams", fontsize=12)

# Scatter plot for Streams vs Valence
sns.scatterplot(ax=axes[3], x='valence_%', y='streams', data=spotify, s=100, color='salmon', edgecolor='black')
axes[3].set_title("Streams vs Valence %", fontsize=14)
axes[3].set_xlabel("Valence (%)", fontsize=12)
axes[3].set_ylabel("Streams", fontsize=12)

# Adjust layout for better spacing
plt.tight_layout()

# Show the plot
plt.show()
```
Output:
![image](https://github.com/user-attachments/assets/7edbf545-a0e7-4c71-a9d9-70de9e0f83a1)
The scatterplot analysis revealed no significant correlation between the different music attributes and the number of streams. This finding led to the realization that the characteristics of a song may not necessarily determine its popularity.

### Attributes Correlation
Input:
```python
# Set up the figure for subplots
fig, axes = plt.subplots(1, 2, figsize=(14, 6))

# Scatter plot for Danceability vs Energy
sns.scatterplot(ax=axes[0], x='danceability_%', y='energy_%', data=spotify, s=100, color='dodgerblue', edgecolor='black')
axes[0].set_title("Danceability % vs Energy %", fontsize=14)
axes[0].set_xlabel("Danceability (%)", fontsize=12)
axes[0].set_ylabel("Energy (%)", fontsize=12)

# Scatter plot for Valence vs Acousticness
sns.scatterplot(ax=axes[1], x='valence_%', y='acousticness_%', data=spotify, s=100, color='darkorange', edgecolor='black')
axes[1].set_title("Valence % vs Acousticness %", fontsize=14)
axes[1].set_xlabel("Valence (%)", fontsize=12)
axes[1].set_ylabel("Acousticness (%)", fontsize=12)

# Adjust layout to prevent overlapping
plt.tight_layout()

# Show the plot
plt.show()
```
Output:
![image](https://github.com/user-attachments/assets/ad09a945-84c9-449b-b2d9-99018091b292)
A strong correlation was observed between danceability and energy, indicating that as the danceability of a track increases, so does its energy level, and vice versa. On the other hand, valence and acousticness showed almost no correlation, suggesting that these two attributes are independent of each other.

## Platform Popularity
In this section, we will explore the popularity and performance of different music streaming platform.
Input:
```python
# Platform popularity
platform_cols = ['in_spotify_playlists', 'in_apple_playlists', 'in_deezer_playlists']
spotify[platform_cols] = spotify[platform_cols].apply(pd.to_numeric, errors='coerce')
platform_data = spotify[platform_cols].sum().reset_index()
platform_data.columns = ['Platform', 'Count']
platform_data.style.set_caption("Popularity of Tracks Across Platforms")

# Plotting platform popularity
plt.figure(figsize=(10, 6))
sns.barplot(data=platform_data, x='Platform', y='Count')
plt.title('Popularity of Tracks Across Platforms')
plt.xlabel('Platform')
plt.ylabel('Number of Tracks')
plt.show()
```
Output:
![image](https://github.com/user-attachments/assets/fe510ec1-94b1-4ad0-a2d3-a4988481908d)
The bar graph indicates that Spotify playlists feature the most popular songs among all platforms, highlighting Spotify's prominence in the music streaming space.
## Advance Analysis
In this section, we will identify the patterns among tracks with the same key or mode, and identifying if a certain genre or artist consistently appear in more playlist or charts.
### Key Distribution
Input:
```python
# Distribution by key and mode
key_mode_counts = spotify.groupby(['key', 'mode']).size().reset_index(name='Count')

plt.figure(figsize=(12, 6))
sns.barplot(data=key_mode_counts, x='key', y='Count', hue='mode', palette='coolwarm')
plt.title('Distribution of Tracks by Key and Mode')
plt.xlabel('Key')
plt.ylabel('Number of Tracks')
plt.legend(title='Mode')
plt.show()
```
Output:
![image](https://github.com/user-attachments/assets/f54160c1-324b-48e6-a292-da699c5b8f9e)
The bar graph shows that C# has the highest number of tracks, whether in a minor or major key, while D# is the least used minor key and A is the least used major key.
### Top 10 Most Frequent Artist in Charts
Input:
```python
# Popular artists in playlists and charts
platform_columns = ['in_spotify_playlists', 'in_spotify_charts', 'in_apple_playlists', 'in_apple_charts', 'in_deezer_playlists', 'in_deezer_charts']
artist_counts = spotify.groupby("artist(s)_name")[platform_columns].sum().sum(axis=1).sort_values(ascending=False)
top_10_artists = artist_counts.head(10).reset_index()
top_10_artists.columns = ['Artist', 'Appearances']

# Display top 10 most frequently appearing artists in playlists and charts
top_10_artists.style.set_caption("Top 10 Artists in Playlists/Charts")

# Plotting top artists
plt.figure(figsize=(14, 7))
sns.barplot(data=top_10_artists, x='Appearances', y='Artist', hue='Artist', palette='coolwarm')
plt.title('Top 10 Artists in Playlists/Charts')
plt.xlabel('Appearances')
plt.ylabel('Artist')
plt.show()
```
Output:
![image](https://github.com/user-attachments/assets/61073fa2-5ae0-4b5d-bcd1-de309f409553)

The top three artists most frequently appearing in playlists and charts are The Weeknd, Taylor Swift, and Ed Sheeran, known for their pop, romance, and R&B songs, respectively.









