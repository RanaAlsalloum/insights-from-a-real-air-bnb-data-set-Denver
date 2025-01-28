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

```diff
calendar.available.value_counts()
```
<img width="233" alt="Screenshot 1446-07-28 at 9 39 39 AM" src="https://github.com/user-attachments/assets/6fbe5051-2a6d-49e1-add5-b26567a049bd" />

f (false) means not available, t(true) means available.

# 2. Purpose: Calculates the percentage of available (t) and unavailable (f) dates.

Explanation:

value_counts(normalize=True) gives proportions.

Multiplying by 100 converts the proportions into percentages

```diff
 availability_percentage = calendar['available'].value_counts(normalize=True) * 100
availability_percentage
```
# 3. Let`s Count the busiest day! 🚩
Hint: We will be counting the most unavailable days (given by f)
```diff
busiest_dates = calendar[calendar['available'] == 'f']['date'].value_counts()
print("Busiest Dates:")
print(busiest_dates.head())
```
# 4. Plot a bar graph to show availability percentage
```diff
import matplotlib.pyplot as plt
availability_percentage.plot(kind='bar', color=['green', 'red'])
plt.title('Availability Percentages')
plt.ylabel('Percentage')
plt.xlabel('Available (t/f)')
plt.show()
```
<img width="724" alt="Screenshot 1446-07-28 at 9 48 04 AM" src="https://github.com/user-attachments/assets/2ac22fa8-33ab-4d10-9f40-6af8e5cb92e5" />

# 5. Plot the busiest day
```diff
busiest_dates.head(10).plot(kind='bar', color='orange')
plt.title('Top 10 Busiest Dates')
plt.ylabel('Number of Listings Unavailable')
plt.xlabel('Date')
plt.show()
```
<img width="788" alt="Screenshot 1446-07-28 at 9 50 09 AM" src="https://github.com/user-attachments/assets/cd7ab55f-82d7-467e-8921-97d3ff075eb2" />


