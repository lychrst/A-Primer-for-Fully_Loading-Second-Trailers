# A Primer for Fully-Loading Second Trailers

Author: Chris Lyons, April, 2022

## Badges  
[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)  

### Repo Contents
- [Primer For Fully-Loading Second Trailers Jupyter NotebooK](https://github.com/lychrst/A-Primer-for-Fully_Loading-Second-Trailers/blob/master/A_Primer_for_Fully-Loading_Second_Trailers.ipynb)
- [images](https://github.com/lychrst/A-Primer-for-Fully_Loading-Second-Trailers/tree/master/images)

### Table of Contents  

- [Abstract](#abstract)
- [The Business Problem](#business-problem)
- [The Data](#the-data)
- [Statistical Updating](#statistical-updating)
- [Priority Shipping](#priority-shipping)
- [Conclusions](#conclusions)
- [Next Steps](#next-steps)


## Abstract

  Maximizing the number of items on trailers represents a potential reduction in operating expenses for companies that ship goods.  It also represents a potential reduction in environmental impact as the amount of fuel per unit shipped may be reduced.  There are a number of factors that prevent companies from fully-loading trailers.  Among them are customer demand, weight of the items, labor costs, and labor shortages.  The following series of logical premises and equations are designed to help companies fully utilize trailers without sacrificing customer expectations.

# The Business Problem

Imagine the following scenario:  There is a company that ships goods.  In order to fulfill customer expectations, some items shipped need to be sent out on the earliest available trailer (a.k.a. "high-priority-items").  Other items need to be shipped in two to three days in order to meet customer expectations (a.k.a. "middle-priority-items").  And some need to be shipped in four or more days in order to meet customer expectations (a.k.a. "low-priority-items").  If one sends only the high-priority-items, the trailer will not be fully loaded.  If instead one sends all of the items, one will need more than one trailer and the second trailer will not be fully-loaded.  Moreover, there is no easy way to determine what is or isn't a 'high-priority-item' once it has been sent to the trailer to be loaded.  Ideally, one wants to send out 10 to 11 fully-loaded trailers per week to this destination as opposed to 14 trailers per week, 7 of which are only partially-loaded. 

One option to solve this problem is to program a calculator-like object to solve it.  This can be done through the programming language Python.  The first task one needs to complete is to define "What is a fully loaded trailer?".  Depending on what one is shipping and the methodology used to ship those items, this number can vary.   If one is shipping lighter items and is double-stacking pallets in a standard 53' trailer, that number is 60 pallets.  If one is instead using a fork-lift to fully-load a 53' trailer with single-stacked pallets, that number is 30 pallets.  If one is instead using a pallet jack to "straight-load" a 53' trailer with single-stacked pallets, that number is 24.  There are also other factors that can come into play such as non-standard pallets and trailers shorter than 53 feet such as box trucks. However, if one has a standardized way of shipping items to a destination, one can use that standard to define "fully-loaded-trailer".  

In addition to knowing how many pallets can fit, it is useful to know how many units typically fit into a trailer for a particular destination considering some destinations may typically receive larger units and other destinations may typically receive smaller units.  In the real world, such numbers can be obtained from a warehouse's past shipment history.  In this notebook, random number generators will be used as substitute.

## The Data

This is designed to be a simulation using random number generators to demonstrate a logical and mathematical framework real companies can use to help improve the fuel-efficiency of their logistical operations.  All data is fake, although it is designed to closely represent real-world scenarios. 

Three hypothetical destinations were used, two of which could benefit from using priority shipping to determine how many trailers to schedule.  As can be seen in the below graphical representations, the average number of customer orders per day for the Boston and DC locations usually exceeds the number of units a trailer can fit, but not always as there is significant variation in both the number of units that are ordered and the number of units that can fit into a trailer.  If one doesn't have a logical framework to deal with the aforementioned variations, it will result in logistical issues such as underutilized trailers or customers not getting their products on time.  


![Pittsbugh to DC](/images/Pittsburgh_to_DC.png)

![Pittsbugh to Boston](/images/Pittsburgh_to_Boston.png)

![Pittsbugh to WV](/images/Pittsburgh_to_WV.png)

![Pittsburgh Box and Whisker](/images/Pittsburgh_Box_and_Whisker.png)

## Statistical Updating 

Trying to predetermine how many units can fit into a fully-loaded trailer is imprecise as there is a lot of space to fill which can lead to a wide range of possible outcomes.  It is far more accurate to make ones estimates off of a half-filled or a three-quarter filled trailer as there is less space to fill and consequently a smaller range of possible outcomes.  In the case of standardized trailers being completely filled with pallets of a standardized size (which is commonplace in the industry), one can use the known number of pallets that can fit into a trailer to update one’s estimates.  The proposed methodology is as follows:

1.  Use a warehouse's past shipment history to provide an estimate for the minimum number of units expected to fit into a fully-loaded trailer. For example, one can look for all trailers that have 60 pallets in them and find the number of units that is -3 standard deviations away from the mean of said trailers.  This is intentionally a low-ball estimate and will be used as the initial planned volume.   
2.  Divide the minimum number of units expected to fit into a fully-loaded trailer by the number of pallets that fit in said trailers (60 in the above example).  This number enables one to determine how may units are expected to fit onto a pallet.
3.  Subtract the total number of pallets a trailer can fit by the number of loaded (or created) pallets to provide an updated number of remaining pallets.   
4.  Multiply the number of remaining pallets by the minimum number of units expected to fit on a pallet to provide an updated estimate of what can fit.  

All of this may seem quite overwhelming, but it is actually quite simple if one works in a warehouse that electronically keeps track of all this information.  All one needs to do is plug in a couple of numbers and one can get an accurate updated estimate of how many units are expected to fit.   As one is using a low-ball estimate, the risk of going over capacity is minimal.  And as one is updating ones estimates as one loads pallets, the risk of going to low is also minimal as the forecasts will become more accurate the closer one gets to having a fully-loaded trailer.

I have created two calculator-like objects to demonstrate the aforementioned approach.  They are fully operational.  If one wishes to use this approach, just substitute actual warehouse data with the fake data I am using.  The first set of functions updates one’s estimate based off of loaded pallets.   The second set of functions adds created pallets and sent units into the equation.    

## Priority Shipping

If everything that was ordered had to be shipped the same day it was ordered, there would be no way to maximize the utilization of trailers that ship customer orders.  But, as anyone who has had something upgraded to UPS 2-day or who has an Amazon Prime membership knows, there are different tiers of priority shipping.  A demonstration was created to take advantage of those tiers and calculate how many trailers were needed.  The same random number generators used to create the past 100-day warehouse histories were used to create the next 14 days of customer orders and fully-loaded trailers.  Customer orders were divided by one-third to create the priority shipments.  One-third of orders were high-priority, one-third were middle-priority, and one-third were low-priority.  High-priority items were always sent, and if a unit of lower priority wasn't sent, then it was upgraded to the next tier.  

While the present formulation differs significantly from a real-world scenario, the demonstration provided insight into the potential for fuel and cost savings through the use of priority shipping.  When priority shipping wasn't taken into account for the DC destination, 28 trailers were needed.  When it was taken into account, only 19 trailers were needed.

## Conclusions

* The calculator functions I created in the first part of the notebook are fully-operational.  If one wanted to, one could download this Jupyter notebook and run the code as is or modify it to one’s particular specifications. All one needs to know is the number of units that fit into past fully-utilized trailers, the level of risk of going over one wishes to have, how many pallets fit into the trailer, how many pallets have been loaded and created, and how many units have been sent.  Not all warehouses have this ability, but the warehouse I work at tracks everything electronically and I am sure it is not the only one that does so.  Thus, doing these calculations can be as simple as entering a couple of numbers. This real-time statistical updating solves one of the problems warehouses have, namely knowing how many items to ship in order to fully-load a trailer.

*  The function used to determine how many trailers to schedule (i.e. new truck) works well. In the demonstration, there was only one time that a trailer wasn't fully-utilized and it wasn't off by much.   Moreover, there wasn't any time when high-priority units weren't shipped.  This function can be modified to be used in a real-world scenario, but actual numbers will need to be used instead of just dividing a random number by 3.

* The functions loaded_truck and loaded_truck_2 are rough as they require manual entry of numbers and there is no real-world uses for them without significant modifications. There entire purpose was to create a simple logistical simulation. There is no plan to develop them further as they differ too significantly from real-world scenarios.

## Next Steps

* The plan is to use random number generators with the superstore dataframe in order to create my own fake warehouse that can be used to address logistical issues in a more realistic way (using SQL). This is in the works. 
* "Hitch-hiking" (for example, sending something from Pittsburgh to Boston so it can go to Foxborough) should be incorporated.
* Weight should be incorporated as size is not always the limiting factor.  For certain heavy items, such as sending pallets of water and paper, trailers can become overloaded.
* The amount of time it takes to load a trailer should also be taken into account as sometimes a lack of labor is what can lead to under-utilization.
* A "Snowstorm Andon" should be created in order to calculate how best to modify what one sends due to a warehouse not being operational due to an event such as a large snowstorm.
* Create code that calculates the amount of cost savings expected for reducing the number of trailers sent.
* Create code that calculates the amount of fuel savings expected for reducing the number of trailers sent.
* If there is a way a trailer can be more fully-loaded by employing more labor, create code that compares the labor cost of making the trailer "super-loaded" versus the cost-savings generated by adding x amount of units to the trailer. 

## License  

[MIT](https://choosealicense.com/licenses/mit/)