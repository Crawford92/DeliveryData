# README

## NOTES
- Stacked orders have base pay averaged across items in stack (2 items, 6 dollar base, 3 dollars each), but not distance or duration.
- Earnings only shows us stacks that are offered as stacks, an offer that is in addition to an existing dash must be dealt with manually
- If I forget to track the end time we use start time estimate, if I forget to track the start time, we might just want to drop the row for time/miles purposes
- DAYS: Offers and Deliveries - Offers can include single deliveries or a stack of two

## IDEAS
- for some of these metrics, we want to exclude restaurants that have only shown up once
- - df2 = df[df['Restaurants'].duplicated() == True] 
- - we might actually want to get a unique name count and exclude restaurants that are less than some chosen value
- Weather, either temp or clear/cloudy/rainy vs pay metrics
- Incorporate daily mileage data (miles driven vs hours or total miles vs delivery miles, etc)
- Annonymize restaurant names for public list .... A0-Z9 is 260 should be enough

## TARGET INSIGHTS
### TIER 1
- Highest tipping vs lowest tipping restaurants
- Days with best $/hr - Times with best $/hr
- Overall average hourly rate
- Overall average tip (and avg tip excluding non tips) [and tip vs non tip ratio, overall and per day of week and per hour of day, and I guess per restaurant too?]
- overall pay/dash time, overall pay/active time, per day
- On tipped orders, tip to base pay ratio
- Restaurant with closest tip to base pay ratio (not useful just kinda fun trivia)
- Restaurants ranked by total pay, base, tip, also by avg pay, base, and tip
- Most deliveries in a day, most tip pay in a day, most overall pay in a day, most miles, most hours (these aren't super interesting but why not)
- Highest pay lowest distance vs lowest pay highest distance
### TIER 2
- Days with most deliveries per active time (with some minimum of active time, 4 or 6 hours?)
- Does peak pay reduce tip (do people tip less on peak pay deliveries - avg tip w/ and w/o peak, also avg tip during normally peak hours without peak bonus e.g. friday night tips w/ and w/o peak)
- Tips/Pay by restaurant genre (would have to come up with genre definition scheme)
- Restaurant with greatest variance of overall pay and of tip, need to normalize because there's like 300 chick fil a orders
- Worst paying restaurant (again, normalized)
- Active time to dash time ratio across days (which has best/worst ratio, need to normalize across hours worked [thursday 1-3pm is different than thursday 5-8pm])

## TODO:
- PYTHON: Pull restaurant names from Dashes and Dash v1, alphabetize, check for typos
- Every day add data to Dashes and Days, every Monday add data to Weeks




## v2.0
- In the first 369 orders, two people tipped AFTER delivery instead of before, so we can just ignore that column and count it as tips
- We're gonna add an ID column equal to the row as a way of tracking orders that are stacked with other orders
-- And a stacked column, so if order 200 is stacked with 201 then the stacked attribute for that element will read 201, and 201's stacked attribute will read 200
--- For triple stacks we'll do Y, Z and then just sort it out with python later
- Changed Peak Pay to Peak Bonus
- Would be interesting to collect time data, its unavailable from the deliveries data but I could manually track it at the time of order acceptance
- Updated previous stacked orders from v1.0

## v3.0
- Time and mileage estimates per delivery estimates vary, mileage is pretty accurate baring abnormal traffic phenomena, ETA given usually more than necessary, as of 5/30 I have 95% on time or early, mostly early but I can't represent that with the given data just yet
-- Could grab the time of accept and the estimated mileage, and then grab the time of delivery as well, and just use the estimates for the first few days of v3.0
- Updated DoorDash Days to include "Offers" attribute and assigned values to Offers attribute
- DoorDash changed to Dash v1 (which includes v2), Dashes is v3, will need to stack v3 onto v1/v2 for certain insights
- Renamed Cornerstone Network, Inc. to Burger King, because it was Burger King

-- If we're looking for average time between orders, pause time needs to be taken into account
- Miles for stacked orders are average of distance
- Updated Days sheet attributes - Start, End to Start(24), End(24). Active/dash time changed from time format to duration format hh:mm, attribute + (h:m)

## v3.1 - Ease of Entry Updates
- Dashes
-- changed attribute order to Total, Pay, Tip, Peak Bonus, for ease of entry
-- changed attribute order to Start Time, Miles, End Time, for ease of entry
-- Changed all miles to doubles
-- Stacked orders show total distance instead of averaged distance for ease of entry [NOTE: base pay is still averaged, duration and miles are not]
-- Replaced - from Peak Bonus with $0.00
-- Updated formula in Total to include Peak Bonus for all elements 
-- Changed all Distance values to type double
- Dash v1
-- changed attribute order to Total, Pay, Tip, Peak Bonus(to mirror change to Dashes
-- Replaced - from Peak Bonus with $0.00
-- Updated formula in Total to include Peak Bonus for all elements
- Days
-- Changed Pay to Total, added Base and Tip attributes
-- Formula for Total is sum of all Total from Dashes matching the date of the element 
-- Formula for Tip is sum of all Tip from Dashes matching the date of the element
-- Formula for Base is Total-Tip, in order to more easily capture Peak and Base pay together
-- Added Version attribute for ease of filtering later
-- Added Pauses attribute to store string representing pauses by start and end time, set Pauses = Version for days before record of pause time available, Pauses = 0 when no pause occured in shift
- Weeks
-- Changed Pay to Base
-- Moved Total to right of values
-- Added Pre Total to represent Total before Adjustment and Other pay
-- Formula for Pre Total is sum of Base and Tip for that element
-- Formula for Base is sum of all Base from Days matching the Dates of the element
-- Formula for Tip is sum of all Tip from Days matching the Dates of the element
-- Updated formula in Deliveries to get the sum of all deliveries from Days matching the element
-- Added Version attribute for ease of filtering later



