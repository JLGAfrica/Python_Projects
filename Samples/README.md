# Overview
This is a collection of projects I did to practice and demonstrate my skills.
## 1_Efficient_Transportation
The schools I went to are very far from home, so I commuted regularly. While it was not common, I occasionally waited long times for jeepneys. So then I wondered, if public transportation is run by companies, how would they improve their operations to meet the demand.
### Creating a somewhat realistic sample information
Since I do not have a datasheet provided by a company, I just created an imaginary one. It contains the following information
- Boarding time: When did the passenger board (random number generator (RNG))
- Boarding place: Where did the passenger board. Let's say the stops are labeled using numbers (RNG not exceeding the route length)
- Boarding payment: How much did the passenger pay (function of the distance traveled)

Here is the first 10 rows out of 16000 of the sample datasheet
![df](images/1_df.png)
