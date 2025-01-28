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
