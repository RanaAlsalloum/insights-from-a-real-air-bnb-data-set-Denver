# insights-from-a-real-air-bnb-data-set-Denver
Python code for insights on Airbnb booking of Denver, we are using calendar and listing csv files.
Please find the link for downloading
https://data.insideairbnb.com/united-states/co/denver/2024-09-28/data/calendar.csv.gz
<img width="1150" alt="Screenshot 1446-07-14 at 10 02 50 AM" src="https://github.com/user-attachments/assets/6727460e-0f48-4b0e-8926-50d3ef0cf133" />

```diff
import pandas as pd 
calendar = pd.read_csv('calendar.csv')
```
# ✅ Questions that you might want to address about the given data

Let`s dive into exciting insights!

# 1. Want to know the number of available and unavailable rooms

```python
calendar.available.value_counts()
```
<img width="233" alt="Screenshot 1446-07-28 at 9 39 39 AM" src="https://github.com/user-attachments/assets/6fbe5051-2a6d-49e1-add5-b26567a049bd" />

f (false) means not available, t(true) means available.

# 2. Purpose: Calculates the percentage of available (t) and unavailable (f) dates.

Explanation:

value_counts(normalize=True) gives proportions.

Multiplying by 100 converts the proportions into percentages

```python
 availability_percentage = calendar['available'].value_counts(normalize=True) * 100
availability_percentage
```
# 3. Let`s Count the busiest day! 🚩
Hint: We will be counting the most unavailable days (given by f)
```python
busiest_dates = calendar[calendar['available'] == 'f']['date'].value_counts()
print("Busiest Dates:")
print(busiest_dates.head())
```
# 4. Plot a bar graph to show availability percentage
```python
import matplotlib.pyplot as plt
availability_percentage.plot(kind='bar', color=['green', 'red'])
plt.title('Availability Percentages')
plt.ylabel('Percentage')
plt.xlabel('Available (t/f)')
plt.show()
```
<img width="724" alt="Screenshot 1446-07-28 at 9 48 04 AM" src="https://github.com/user-attachments/assets/2ac22fa8-33ab-4d10-9f40-6af8e5cb92e5" />

# 5. Plot the busiest day
```python
busiest_dates.head(10).plot(kind='bar', color='orange')
plt.title('Top 10 Busiest Dates')
plt.ylabel('Number of Listings Unavailable')
plt.xlabel('Date')
plt.show()
```
<img width="788" alt="Screenshot 1446-07-28 at 9 50 09 AM" src="https://github.com/user-attachments/assets/cd7ab55f-82d7-467e-8921-97d3ff075eb2" />

# 📄 Download listings dataset of Denver from
# https://data.insideairbnb.com/united-states/co/denver/2024-09-28/visualisations/listings.csv
```python
import pandas as pd
listings = pd.read_csv('listings.csv')
```
<img width="1043" alt="Screenshot 1446-07-28 at 9 56 53 AM" src="https://github.com/user-attachments/assets/63541d79-4b99-42e8-ab0c-b333f5f6f998" />

```python
listings.columns
```
<img width="558" alt="Screenshot 1446-07-28 at 9 59 41 AM" src="https://github.com/user-attachments/assets/497ca1e4-98ff-4609-85e5-17049458f485" />

# ✅ Room type and price is given seperately

Let`s Combine and visualize them
```python
import matplotlib.pyplot as plt
price_by_room = listings.groupby('room_type')['price'].mean()
print(price_by_room)

# Plot price by room type
price_by_room.plot(kind='bar', color='cyan')
plt.title('Average Price by Room Type')
plt.ylabel('Average Price')
plt.xlabel('Room Type')

plt.show()
```
<img width="758" alt="Screenshot 1446-07-28 at 10 02 41 AM" src="https://github.com/user-attachments/assets/2a831657-f09d-48f1-b5a2-19921b28c5c0" />

# ✅ Which are the top 10 neighborhoods with the most listings?

```python
neighborhood_counts = listings['neighbourhood'].value_counts().head(10)
print("Top 10 Neighborhoods by Listings:")
print(neighborhood_counts)

# Plot neighborhoods with most listings
neighborhood_counts.plot(kind='bar', color='lightcoral')
plt.title('Top 10 Neighborhoods by Listings')
plt.ylabel('Number of Listings')
plt.xlabel('Neighborhood')
plt.xticks(rotation=90)
plt.show()
```
<img width="703" alt="Screenshot 1446-07-28 at 10 05 05 AM" src="https://github.com/user-attachments/assets/80c6cb2a-2465-4e48-9d87-fbba95261c16" />

# ✅ Geographical Distribution of Listings (Price Colored)

```python
import matplotlib.pyplot as plt
import seaborn as sns
```
```python
plt.figure(figsize=(10, 6))
sns.scatterplot(data=listings, x='longitude', y='latitude', hue='price', palette='viridis', size='price', sizes=(10, 200))
plt.title('Geographical Distribution of Listings (Price Colored)')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```
<img width="982" alt="Screenshot 1446-07-28 at 10 08 11 AM" src="https://github.com/user-attachments/assets/dff01783-da01-4b65-bb56-94f913f9979f" />

# Let us see the listings on a real map

* High Density: The areas that appear in red or yellow (the "hot" colors) indicate higher density or concentration of listings. This means there are more listings in these areas.
Popular Locations: These regions might be more popular or in high demand. It could be near tourist attractions, popular neighborhoods, or central areas in Denver where people tend to stay more often.
Colder Areas (Green/Blue):

* Low Density: Areas with blue or green (the "cold" colors) indicate a lower concentration of listings. These regions have fewer listings available.
Less Popular Locations: These areas might be less popular or further from key attractions. If you're looking at pricing or other factors, lower density could imply less competition in these regions, which might indicate more affordable areas or less tourist traffic.
Density Patterns:

* Clustered Areas: If you notice clusters of heatmap intensity, they represent hotspots. These might correspond to high-traffic areas like downtown Denver, popular neighborhoods, or close to major attractions like parks or sports venues.
Spread-Out Listings: If the heatmap shows a more uniform distribution, it could suggest that listings are more evenly spread across the region, which may reflect a more balanced demand for rentals across different areas of Denver.

```python
import folium
from folium.plugins import HeatMap
import pandas as pd


Denver_data = listings[['latitude', 'longitude', 'price']]  # Example, you may add more columns

# Create a base map centered around Denver
m = folium.Map(location=[39.7392, -104.9903], zoom_start=12)

# Prepare the data for the heatmap
heat_data = [[row['latitude'], row['longitude']] for index, row in Denver_data.iterrows()]

# Add the heatmap to the map
HeatMap(heat_data).add_to(m)

# Save the map as an HTML file to view in a browser
m.save('denver_heatmap.html')

# If you're using Jupyter Notebook, you can display the map directly in the notebook:
m
```
<img width="1045" alt="Screenshot 1446-07-28 at 10 21 49 AM" src="https://github.com/user-attachments/assets/0fe29b0c-2f81-435b-82b5-e78113c6517c" />

