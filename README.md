# dashstats

IDEAS
-for some of these metrics, we want to exclude restaurants that have only shown up once
--df2 = df[df['Restaurants'].duplicated() == True] check notebook for syntax but it should be something like this
-Heatmap for time like 12 hour clock noon to midnight for when orders come in the mos
-Will need to take average of base pay across stacked orders, so if 12 for 2 restaurants base pay for each is 6. its not correct but it'll have to do unless they change the interface back to how it was
-Weather, either temp or clear/cloudy/rainy vs pay metrics
-Not really useful but tracking mileage per dash, or spending on gas, like idk but we already track miles per workday on v3
-Annonymize restaurant names for public list .... A0-Z9 is 260 should be enough
-Should we update Days to include rough miles driven and gas used?
-See if frozen header impacts csv export

TARGET INSIGHTS
-Highest tipping vs lowest tipping restaurants
-Days with best $/hr - Times with best $/hr
-Active time to dash time ratio across days (which has best/worst ratio)
-Overall average hourly rate
-Overall average tip (and avg tip excluding non tips) [and tip vs non tip ratio, overall and per day of week and per hour of day, and I guess per restaurant too?]
-Does peak pay reduce tip (do people tip less on peak pay deliveries)
-On tipped orders, tip to base pay ratio
-Tips/Pay by restaurant genre (would have to come up with genre definition scheme)
-Restaurant with greatest variance of overall pay and of tip, need to normalize because there's like 300 chick fil a orders
-Worst paying restaurant (again, normalized)
-Restaurant with closest tip to base pay ratio (not useful just kinda fun trivia)
-Restaurants ranked by total pay, base, tip, also by avg pay, base, and tip
-Days with most deliveries per active time
-Most deliveries in a day, most tip pay in a day, most overall pay in a day (these aren't super interesting but why not)
-overall pay/dash time, overall pay/active time, per day


NOTES
-Order is during promo if peak bonus is num, or is not -
--in preprocess just set all - to 0 so attribute is all nums
-Updating v1.0 to handle stacked, but in 2.0 onward we will take average of base pay across items stacked on a delivery
-DD has sets for offers vs deliveries, offers is accepted offers (1 stack = 1 offer), deliveries is deliveries
-For typo checking make sure the daily totals from DoorDash.csv equal DoorDash Days.csv (or whatever I called the files)
---recheck v1 for peak pay as possible source of incongruency
-So triple stacks dont show up as triple stacks in Earnings, they count as an additional offer, which means there's less math, and we can also for the sake of keeping everything numbered when possible, during processing set all empty stacks to 0
--And for v2 onwards, stacked orders will be chronological
-Get all restaurants listed alphabetically for typo check
-If you have 643 lifetime deliveries and the highest ID is 612, are you missing a day or two? check day counts vs dates in sheets
--so friday at 1:58pm I got a burger king order, 7 items, 5.9 miles total, for $10.94, deliver by 2:30pm. it does not show up in my earnings page for that day OKAY SO FOR SOME *@#$ING REASON THE BURGER KING ORDER IS LABELED AS CORNERSTONE NETWORK INC IN THE APP
-RENAME CORNERSTONE NETWORK, INC. TO BURGER KING
-How are we going to monitor pause time?
-It looks like the only orders that show as stacked from Earnings are orders that are stacked together on offer, not when an additional order is offered during an ongoing order, which explains why triple stacks are absent
--Will need to come up with a means of handling this math, second/third orders update delivery times, but if we manually capture the time at end of delivery we should be fine there, might cause a slight kerfuffle for aggregation down the road but we'll burn that bridge when we get to it can't do much besides utilize the data available. Like it doesn't give miles for these stacks it gives additional distance which is an inaccurate representation for what we want to do....maybe we just keep these as 0 for now and remove them when looking for insights related to distance OR we get the full distance of the stack and divide it across since thats how Earnings stacked orders are yeah okay we're gonna do that
-For v3 can look at shortest/longest delivery for miles and time
-Dont have est duration as attribute, use start and end time, could include est duration as attribute its only an extra formula in the sheet, for easier to input start and end time and handle processing after
-In averaging stacked order values, we will still go halfsies for distance, but once manual end time is tracked we can use the end time for that part of the stack as the start time for the other half....although that isn't really accurate. If we want to look at when orders come in, fake start times is not accurate data, but if we want to consider duration of orders, stacked orders negatively impact duration. Maybe take half the total duration as each subduration per stacked element? Can decide later, important just to keep accurate entries for now, but I think half total duration each would be the sensible choice when making relevant evaluations. Its worth noting that a lot of the time, one part of the stack is MUCH further than the other
-If I forget to track the end time we use start time estimate, if I forget to track the start time, we might just want to drop the row for time/miles purposes

-So manually record start/end time of each dash
-While entering, stack orders that were "additional offers" in addition to natural stacks


v2.0
-In the first 369 orders, two people tipped AFTER delivery instead of before, so we can just ignore that column and count it as tips
-We're gonna add an ID column equal to the row as a way of tracking orders that are stacked with other orders
--And a stacked column, so if order 200 is stacked with 201 then the stacked attribute for that element will read 201, and 201's stacked attribute will read 200
---For triple stacks we'll do Y, Z and then just sort it out with python later
-Changed Peak Pay to Peak Bonus
-Would be interesting to collect time data, its unavailable from the deliveries data but I could manually track it at the time of order acceptance
-Updated previous stacked orders from v1.0

v3.0
Time and mileage estimates per delivery
-estimates vary, mileage is pretty accurate baring abnormal traffic phenomena, ETA given usually more than necessary, as of 5/30 I have 95% on time or early, mostly early but I can't represent that with the given data just yet
--Could grab the time of accept and the estimated mileage, and then grab the time of delivery as well, and just use the estimates for the first few days of v3.0
-Updated DoorDash Days to include "Offers" attribute and assigned values to Offers attribute
-DoorDash changed to Dash v1 (which includes v2), Dashes is v3, will need to stack v3 onto v1/v2 for certain insights
-Renamed Cornerstone Network, Inc. to Burger King, because it was Burger King
-Figure out where to store Pause Time 
--[(5/26, 3:21pm, 3:45pm), (5/27, 6:17pm, 6:38pm)] 
--If we're looking for average time between orders, pause time needs to be taken into account
-Miles for stacked orders are average of distance


