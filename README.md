# README

## TODO:

- High priority: Create function to take average distance and duration for stacked deliveries
- High priority: Function to handle active mileage (instead of handling through Sheets)
- High priority: Tier 1 insights complete
- Medium priority: Function to calculate average time between deliveries, by day
- Medium priority: Function to take restaurant name as parameter and display insights
- Medium priority: Completed visualizations listed in notes.txt
- Low proirity: Use GPS worksheet to determine Drive vs Marketplace, and any virtual restaurants 
- Low priority: Function to anonymize restaurant name data within the *Deliveries* dataframe
- Low priority: Resolve missing addresses
- Low priority: Convert addresses to GPS coordinates
- Low priority: Tier 2, 3, 4 insights complete

## DONE:
- Completed (6/13): Pull restaurant names from Delivery csvs, alphabetize, check for typos
- Completed (6/16): Check attributes other than 'Restaurant Names' for input errors
- Completed (7/13): Function to convert string format "$##.##" to float
- Completed (7/13): Transform relevant *Deliveries* dataframe attributes to utilize the Datetime class for ease of later analysis
- Completed (7/13): Low priority: utilize Sheets API to pull data in automatically instead of weekly CSV update
- Completed (7/20): Homogenize attribute naming scheme across worksheets
- Completed (7/26): Process RawData and GPS sheets to express current known values
- Completed (7/29): Update stats notebook to handle adjusted column names
- Completed (7/29): Function to calculate estimated adjusted pay per order
- Completed (7/29): Function to calculate 'active mileage'
- Completed (8/10): Preserve aggregated data for use across insights

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
- Weather, either temp or clear/cloudy/rainy vs pay metrics

## TARGET INSIGHTS
### TIER 1
- Hour + Day of Week (ex: 7pm Friday) with best $/hour
- Daily and weekly $/hour, visualized
- Total pay (as base:tip:other ratio) by day of week, visualized
- (DONE) Restaurants by delivery count
- (DONE) Overall average tip, with and without untipped orders 
- (DONE) Tip:No Tip ratio
- (DONE) Days with best $/hour
- (DONE) Overall average hourly rate
- (DONE) Average delivery distance and duration
- (DONE) Restaurants ranked by total and avg pay, base, tip
- (DONE) Pre adjustment earnings / hours spent on deliveries, by week
- (DONE) Average earned per delivery, by day of week 
- (DONE) Average tip, by day of week 
- (DONE) Average hourly pay, by day of week
- (DONE) Highest total pay, base pay, tip, active time, dash time, deliveries, and distance, by day (except distance)
- (DONE) Highest distance and delivery time, by order
### TIER 2
- Average time between deliveries, by day of week
- Do people tip less at end of month?
- Earnings by restaurant genre (have to come up with genre definition scheme)
- Restaurant with greatest variance of overall pay and of tip
- Correlations
### TIER 3
- GPS Heatmap of pickup locations
	- Heatmap by average delivery total, filterable by hour or day of week
- Weather vs Pay metrics
### TIER 4
- Whether or not to accept an order, given new data from offer and historical data from deliveries
- Most profitable route speculations
- How to make the most money



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











