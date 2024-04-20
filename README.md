# I3-Easy-to-Use

set up the file
```python
from google.colab import files
uploaded = files.upload()

import io
import pandas as pd
```

Open the file 'movie_data.csv'
```python
movie_data = pd.read_csv(io.BytesIO(uploaded['movie_data.csv']))
```

Display the first few rows to ensure it's loaded correctly
```python
movie_data.head()
```

# The average IMBD rate from 2000 - 2016
Filter the data for years from 2000 onward
```python
filtered_data = movie_data[movie_data['title_year'] >= 2000]
```

Group the filtered data by 'title_year' and calculate the mean IMDb score
```python
average_scores_per_year = filtered_data.groupby('title_year')['imdb_score'].mean().reset_index()
```

Plotting the average IMDb scores per year
```python
plt.figure(figsize=(14, 7))
sns.lineplot(x='title_year', y='imdb_score', data=average_scores_per_year, marker='o', color='b')
plt.title('Average IMDb Score per Year (2000 - Latest)')
plt.xlabel('Year')
plt.ylabel('Average IMDb Score')
plt.grid(True)
plt.xticks(rotation=45)  # Rotate x-axis labels for better visibility
plt.tight_layout()  # Adjust layout to fit labels
plt.show()
```


# Total number of movie that were rated in year 2015
Count the number of movies in the year 2015
```python
num_movies_2015 = movie_data[movie_data['title_year'] == 2015].shape[0]
```

Output the result
```python
print(f"Number of movies in the year 2015: {num_movies_2015}")
```

# Compare the highest and the lowest average IMBD rate by movie genres
Create a function to get genre counts for a given year
```python
def get_genre_counts(year):
    genres = movie_data[movie_data['title_year'] == year]['genres'].str.get_dummies(sep='|').sum()
    return genres.sort_values(ascending=True)  # Sort ascending for better visualization in horizontal plot
```

Get genre counts for 2015 and 2007
```python
genres_2015 = get_genre_counts(2015)
genres_2007 = get_genre_counts(2007)
```

Create a DataFrame for easy plotting
```python
genre_comparison = pd.DataFrame({'2015': genres_2015, '2007': genres_2007}).fillna(0)
```

Plotting the data horizontally
```python
plt.figure(figsize=(14, 10))
genre_comparison.plot(kind='barh', color=['skyblue', 'orange'], ax=plt.gca())
plt.title('Comparison of Movie Genres in 2015 and 2007')
plt.xlabel('Number of Movies')
plt.ylabel('Genres')
plt.legend(title='Year')
plt.tight_layout()  # Adjust layout to make room for labels
plt.show()
```
