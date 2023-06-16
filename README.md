# README

## TODO:

- High priority: Transform relevant *Deliveries* dataframe attributes to utilize the Datetime class for ease of later analysis
- High priority: Function to anonymize restaurant name data within the *Deliveries* dataframe
- High priority: Function to convert string format "$##.##" to float
- High priority: Function to get aggregate AVG or SUM values for 'Total', 'Tip', and 'Base' sorted high to low or low to high
- High priority: Add *GPS* to imports, pull unique ('Restaurant Name', 'Raw Data') pairs, export out to CSV
- Medium priority: Check active/dash time from *Days* against *Weeks*
- Medium priority: Tier 1 insights complete
- Low priority: Tier 2 insights complete
- Low priority: utilize Sheets API to pull data in automatically instead of weekly CSV update
- Completed (6/13): Pull restaurant names from Delivery csvs, alphabetize, check for typos
- Complted (6/16): Check attributes other than 'Restaurant Names' for input errors

## NOTES
- Attributes appear in this document surrounded by single quotes 'like this'
- Sheets appear in this document italicized *like this*
- Things I want to remember later are in bold **like this**
- Stacked orders have base pay averaged across items in stack (2 items, 6 dollar base, 3 dollars each), but not distance or duration.
- If I forget to track the end time we use start time estimate, if I forget to track the start time, we want to drop the row for time/miles purposes
- *Days*: Offers and Deliveries - Offers can include single deliveries or a stack of two
- Natural stacks are offered as a stack, delayed stacks are offers that get other offers added
- *Days*: Active time represents the total time spent on deliveries, shift time (sometimes referenced as dash time) is the total length of a working shift


## IDEAS
- For some of these metrics, we want to exclude restaurants that have only shown up once
	- df2 = df[df['Restaurants'].duplicated() == True] 
	- we might actually want to get a unique name count and exclude restaurants that are less than some chosen value
- Weather, either temp or clear/cloudy/rainy vs pay metrics
- Incorporate daily mileage data (miles driven vs hours or total miles vs delivery miles, etc)

## TARGET INSIGHTS
### TIER 1
- Highest tipping vs lowest tipping restaurants
- Days with best pay/hr - Times with best pay/hr
- Overall average hourly rate
- Overall average tip (and avg tip excluding non tips) [and tip vs non tip ratio, overall and per day of week and per hour of day, and I guess per restaurant too?]
- overall pay/shift time, overall pay/active time, per day
- On tipped orders, tip to base pay ratio
- Restaurant with closest tip to base pay ratio (not useful just kinda fun trivia)
- Restaurants ranked by total pay, base, tip, also by avg pay, base, and tip
- Most deliveries in a day, most tip pay in a day, most overall pay in a day, most miles, most hours (these aren't super interesting but why not)
- Highest pay lowest distance vs lowest pay highest distance
- Histograms for both tip and total values
- Visualizations for which days and which hours worked
- Best and worst 'Pre Total' vs 'Active Time' ratios
### TIER 2
- Days with most deliveries per active time (with some minimum of active time, 4 or 6 hours?)
- Does peak pay reduce tip (do people tip less on peak pay deliveries - avg tip w/ and w/o peak, also avg tip during normally peak hours without peak bonus e.g. friday night tips w/ and w/o peak)
- Tips/Pay by restaurant genre (have to come up with genre definition scheme)
- Restaurant with greatest variance of overall pay and of tip, need to normalize because there's like 300 chick fil a orders
- Worst paying restaurant (again, normalized)
- Active time to shift time ratio across days (which has best/worst ratio, need to normalize across hours worked [thursday 1-3pm is different than thursday 5-8pm])
### TIER 3
- GPS Heatmap of pickup locations
- Weather vs Pay metrics



# CHANGE LOG

## v2.0
- Removed 'Tipped After Delivery' attribute and added values to 'Tips', since is was nonzero in 2 instances out of 369
- Added 'ID' attribute to each element to facilitate tracking of stacked orders
	- Added 'Stacked' attribute to designate other elements that shared a delivery stack with this order
		- **v3 will track delayed stacks, v2 and v1 do not, which will impact processing**
- Renamed 'Peak Pay' to 'Peak Bonus'
- Updated previous stacked orders from v1.0 to include natural stacks

## v3.0
- Updated *Days* to include 'Offers' attribute and assigned values to 'Offers' attribute
- *Deliveries* changed to *DeliveriesOld* (which includes v1 and v2), *Deliveries* is v3
- Renamed Cornerstone Network, Inc. to Burger King, because it was Burger King
- Updated *Days* attributes - 'Start', 'End' to 'Start(24)', 'End(24)'. Active/shift time changed from time format to duration format hh:mm, attribute + (h:m)

## v3.1 - Ease of Entry Updates
### *Deliveries*
- Changed attribute order to 'Total', 'Pay', 'Tip', 'Peak Bonus', for ease of entry
- Changed attribute order to 'Start Time', 'Miles', 'End Time', for ease of entry
- Changed 'Miles' from int and float to all float
- Stacked orders show total distance instead of averaged distance for ease of entry **base pay is still averaged, duration and miles are not**
- Replaced "-" from 'Peak Bonus' with "$0.00"
- Updated formula in 'Total' to include 'Peak Bonus' for all elements 
### *DeliveriesOld*
- Changed attribute order to 'Total', 'Pay', 'Tip', 'Peak Bonus' (to mirror change to *Deliveries*)
- Replaced "-" from 'Peak Bonus' with "$0.00"
- Updated formula in 'Total' to include 'Peak Bonus' for all elements
### *Days*
- Renamed 'Pay' to 'Total', added 'Base' and 'Tip' attributes
- Formula for 'Total' is sum of all 'Total' from *Deliveries* matching the date of the element 
- Formula for 'Tip' is sum of all 'Tip' from *Deliveries* matching the date of the element
- Formula for 'Base' is 'Total'-'Tip', in order to more easily capture Peak and Base pay together
- Added 'Version' attribute for ease of filtering 
- Added 'Pauses' attribute to store string representing pauses by start and end time, set Pauses = Version for days before record of pause time available, Pauses = 0 when no pause occured in shift
### *Weeks*
- Renamed 'Pay' to 'Base'
- Moved 'Total' to right of values
- Added 'Pre Total' to represent 'Total' before 'Adjustment' and 'Other' pay
- Formula for 'Pre Total' is sum of 'Base' and 'Tip' for that element
- Formula for 'Base' is sum of all 'Base' from *Days* matching the 'Dates' of the element
- Formula for 'Tip' is sum of all 'Tip' from *Days* matching the 'Dates' of the element
- Updated formula in 'Deliveries' to get the sum of all deliveries from *Days* matching the element
- Added 'Version' attribute for ease of filtering later
### Misc
- Added .gitignore and configured for *.csv, *.Rhistory *checkpoints
- Renamed files and repo
- Reworded readme to not use source app specific wording whenever practical

## v3.2 - A New Hope (that all the names on the backend don't change again)
- Added 'Mileage Start' and "Mileage End' to *Days*. Only available in version 3 onward, previous versions set to "-"
- Updated TODO
- Updated Target Insights
- Updated *DeliveriesOld* (ID=269).Version = 1, because it was blank
- Fixed typos/conflicts/corrections in restaurant names
	- Typos: 
		- 200 Chick replaced with Chick-fil-A. 
		- 159 Sonic Drive-in replaced with Sonic Drive-In. 
		- 78 The Privateer Coal Fire Pizz replaced with The Privateer Coal Fire Pizza

	- Corrections: 
		- 119 Pick Up Stix(SOF) replaced with Pick Up Stix. 
		- 592 Cornerstone Network, Inc. replaced with Burger King


	- Resolved Conflicts: 
	- Some 'Restaurant Name's that had been recorded in version 1 have different names in the source data since the 5/7 software update. The text below shows the previous 'Restaurant Name' and what it has been changed to, or notes 
		- (108) Applebee's Bar & Grill changed to Applebee's
		- (128, 139, 160, 259, 280) Board and Brew changed to Board & Brew
		- (99, 104, 209, 220, 228) Little Caesars Pizza changed to Little Caesars
		- (31, 33, 301, 372) Maan's Mediterranean Grill changed to Maans Mediterranean Grill and Kabobery
		- (80, 208, 214) Thai Thai Oceanside Restaurant changed to Thai Thai Oceanside restaurant
		- (87) Two Brothers From Italy Pizza changed to Two Brothers From Italy
		- (60) Wingstop changed to Wingstop Drive (although 60 still reads Wingstop in the raw data)
		- (2) Erika's Restaurant to Erika's Mexican Food & Seafood
		- (26) North Park Produce Bakery and Grill to North Park Produce
		- (137) La Hacienda to La Hacienda Restaurant
		- (187, 219) Broken Yolk to The Broken Yolk Cafe
		- (270) raw data is "Jack In the Box (98)" where all others are Jack in the Box( ), going to keep Jack in the Box for 'Restaurant Name'
		- (273, 308, 371, 394, 587, 593, 711 ) Poki Poki is "Poki Poke (Oceanside #1)" in the raw data but the sign out front says Poki Poki so going to keep that as the 'Restaurant Name'
		- (15, 105, 106, 267, 442, 457) Valerie's Taco Shop to Valerie's Taco Stand [I assume this all falls under the umbrella of Valerie's, so we'll use the "Taco Shop" for 'Restaurant Name']

	- Unresolved Conflicts
		- Jersey Mike's Subs. Three relevant stores, 20117, 20185, 20162. Two entries are labelled Oceanside Blvd and one entry is labelled North River Rd. We know that 20117 is definitely Oceanside Blvd, which is very strange because the raw data, after v2, gives us Jersey Mike's (Oceanside Boulevard) and Jersey Mike's (20117). I suspect, but cannot yet confirm, that 20162 is North River Rd, and that 20185 is on Mission Ave
		- (582, 651) Two different raw data values for Cusimano's Pizzeria, one shows (Oceanside) the other shows (3809 Plaza Drive), unclear why/how this would be observed
		- (239) China Fusion Restaurant instead of China Fusion











