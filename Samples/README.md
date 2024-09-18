# Overview
This is a collection of projects I did to practice and demonstrate my skills.
## 1_Efficient_Transportation
The schools I went to are very far from home, so I commuted regularly. While it was not common, I occasionally waited long times for jeepneys. So then I wondered, if public transportation is run by companies, how many vehicles would they need to operate to meet the demand.
### Creating a somewhat realistic sample information
Since I do not have a datasheet provided by a company, I just created an imaginary one. It contains the following information
- Boarding time: When did the passenger board
- Boarding place: Where did the passenger board. Let's say the stops are labeled using numbers
- Boarding payment: How much did the passenger pay

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
import random
from datetime import time

# User-specific information
distance_unit = 'Kilometer'
route_length = 42 # the length of the route back to the starting point
minimum_fare = 13 # the fare passengers pay for the first minimum_distance kilometers
minimum_distance = 4 # distances longer than this require additional payment
additional_charge_per_km = 1.50 # the fare passengers pay for each succeeding kilometer

# Number of passengers for the day
n = 16000

# We are assuming that the number of passengers is dominated by commuting workers and students going to and from work and school respectively
# Therefore, the boarding time distribution is bimodal centered around 0700 and 1700
mu_time, sigma_time = 7, 3.75
mu2_time, sigma2_time = 17, 4.01
normal_time = np.random.normal(mu_time, sigma_time, n // 2)
normal2_time = np.random.normal(mu2_time, sigma2_time, n // 2)

# We are assuming that most passengers come from densely populated areas, and the route starting and midway segments usually are
# Therefore, the boarding place distribution is bimodal centered around 1 and route_length / 2
mu_place, sigma_place = 1, 3.87
mu2_place, sigma2_place = route_length / 2, 4.31
normal_place = np.random.normal(mu_place, sigma_place, n // 2)
normal2_place = np.random.normal(mu2_place, sigma2_place, n // 2)

# Removing invalid values (Time is only between 0000 and 2400)
for i in range(len(normal_time)):
    if normal_time[i] < 0 or normal_time[i] > 24:
        normal_time[i] = mu_time

for i in range(len(normal2_time)):
    if normal2_time[i] < 0 or normal2_time[i] > 24:
        normal2_time[i] = mu2_time

# Removing invalid values (Place is only between 0 and 42)
for i in range(len(normal_place)):
    if normal_place[i] < 0:
        normal_place[i] += 2 * (mu_place - normal_place[i])
    if normal_place[i] > route_length:
        normal_place[i] -= 2 * (normal_place[i] - mu_place)

for i in range(len(normal2_place)):
    if normal2_place[i] < 0:
        normal2_place[i] += 2 * (mu2_place - normal2_place[i])
    if normal2_place[i] > route_length:
        normal2_place[i] -= 2 * (normal2_place[i] - mu2_place)

bimodal_time = np.concatenate([normal_time, normal2_time])
bimodal_place = np.concatenate([normal_place, normal2_place])

# Turning the random numbers into random time objects and adding them to a DataFrame
info_dict = {'boarding_time': [], 'boarding_place': [], 'boarding_payment': []}

for i in bimodal_time:
    hour = int(i)
    minute = random.randint(0, 59)
    second = random.randint(0, 59)
    info_dict['boarding_time'].append(time(hour, minute, second))

for i in bimodal_place:
    place = int(i)
    info_dict['boarding_place'].append(place)
    travel_distance = random.randint(0, route_length - place)
    if travel_distance <= minimum_distance:
        info_dict['boarding_payment'].append(minimum_fare)
    else:
        info_dict['boarding_payment'].append(minimum_fare + additional_charge_per_km * (travel_distance - minimum_distance))


df = pd.DataFrame(info_dict)
```
