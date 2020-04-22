## Mules-and-Warehouses-Extended: A mod for X4 Foundations
An continuation of Mules and Warehouses v4 & Mules Extentsion. That has been optimised and improved for current versions of X4 Foundation.

### For general releases of this project please see the following
* Steam Workshop: https://steamcommunity.com/sharedfiles/filedetails/?id=2013772865
* Nexus Mods: https://www.nexusmods.com/x4foundations/mods/416

### Station Mule Tips
I strongly suggest reading the original author's thread, station mule is described well:
https://forum.egosoft.com/viewtopic.php?t=417350

The way I've been using station mules is that every factory has one pointed to a big warehouse with "make target warehouse" checked. They also have two-way trade checked so they'll bring back needs for their factory. Then from the big warehouse, there are station mules out to each trade station warehouse with "make target checked". This ensures that every warehouse has their tradewares set, and off course the station mules will work at distributing your goods to those stations.

### Distribution Mule Tips
The new supply mule and the Distribution Mule have a little bit of overlap. The distribution mule can be a little bit confusing on what the three sliders do, but here's how it works.
- **Min Storage** - the desired percentage of storage for each ware on each target station in the list
- **static storage** - the desired stock level on the source station
- **Max Storage** - an amount on the source station that will force a distribution even if the target is above min storage.

Most of the time distribution mule bumps up against either min storage or static storage. To be less restrictive, you generally need to INCREASE min storage and DECREASE static storage to get wares moving. 

### Travel Mule Tips
- The Travel mule now has sliders that allow you to be less restrictive on what trades to take out from your stations. These options should allow you to open up your trade space to make sure goods are moving:

- **Player Buy Mod** A percentage multiplier applied to the player price in profit calculations. If it is 100, we use the station managers price. If it's 0, we ignore the price altogether. 50 means cut the manager's price in half, etc. In my experience this is usually the biggest problem; if your stock is somewhat low for any reason, the price will be too high for your travel mule to find a trade.
- **Fill Cargo Hold %** The Mule will require the trade to fill up their hold past this percentage for it to be considered valid.

You generally want a few travel mules on all of your warehouses so that your goods are going out to make you money. If you notice Travel Mules idling, either tinker with their sliders to encourage more trades, or re-assign them to a different task. It could be that you're just not producing or distributing fast enough to keep up with the amount of travel mules you have set.


### New Supply Mule Instructions
TLDR; just set some haulers to the Supply Mule without changing the defaults and they will supply your empires buy offers. You should never really need to touch them.

What is the point of the Supply Mule? The supply mule attempts to fulfill all of the buy orders in your empire at the cheapest price possible. While this may not directly produce profit, it reduces theoretical losses. For instance, lets say you have a factory that wants to sell graphene at 130 Cr/unit, and a tradestation that wants to buy it at 150 Cr/unit. The AI could theoretically come along and buy from your factory, then sell to your trade station, and you would realize a loss. The supply mule will see this trade and attempt to fill it. It reduces price inefficiencies in your empire. 

In general, the mule prioritizes build storage, then production needs, then tradewares. It prefers to source from player owned resources and will take any trade that reduces inefficiency before going out to AI stations. It has the ability to queue up multiple trades until it's cargo is full before coming to sell everything (this particular routine isn't optimal in terms of pathing).

There are branches of priorities depending on whether a station is set in the UI and whether that station is owned by the player or the AI. The priorities will be described after we go over the options.

**note**-the supply mule should obey your global blacklists.
- **Source Station** - Can be any station. Behavior in different scenarios is described below
- **Assign Ship To Station** - assigns the ship as a subordinate with the trader role. Serves no functional purpose except convenience
- **Serve Source Only** - will only supply the station that is set
- **Player Suppliers Only** - Will only source from player owned stations
- **Max Trades** - the max number of stops that can be made in an attempt to fill up the cargo hold. Each trade will have a minimum size of (100/MaxTrades)% of the cargo hold full. i.e. a setting of 2 means each trade must be at least 50% of the hold. You can use this to avoid supply mules running low volume trades.
- **Max Jumps** - in general when considering player to player trades there is no maximum jump distance. However, when sourcing from the AI or serving AI stations, this distance is obeyed. This is based on pilot skill.
- **Lock Wares to User** - If you choose specific wares, you need to check this option for your choices to stick across trade runs. Otherwise the script populates the list on the UI with the needs that it found when searching for things to do.
- **Build Storage ONLY** - will wait for build storage needs
- **Factory Supply ONLY** - will only supply wares needed for production on your factories.
- **Player Buy Mod** - can be used to fake the sell prices on player owned stations for profit calculations. As an extreme example, you can set it to 0 and the sell plrices from player stations will be ignored.

**Mule Priorities**
#### When No Station is Set
1. Buy from player, sell to player build storage
2. Buy from anyone, sell to player build storage
3. Buy from player, sell to player factories for production
4. Buy from anyone, sell to player factories for production
5. Buy from player, sell to player tradewares
6. Buy from anyone, sell to player tradewares

#### When the set station is player owned
1. Buy from player, sell to station build storage
2. Buy from anyone, sell to station build storage
3. Buy from player, sell to station production needs
4. Buy from anyone, sell to station production needs
5. Buy from player, sell to station tradewares
6. Buy from anyone, sell to station tradewares
continue with *no station set* logic.

#### When the set station is an AI station.
1. Buy from Player, sell to station
2. Buy from anyone, sell to station
3. Buy from player, sell anywhere in same sector
4. Buy from anyone, sell anywhere in same sector

### Some General Mule Advice
- Make small changes at one time. Don't add storage to every warehouse and factories all at once. Don't add 10 travel mules or supply mules all at once. Your goal is to have just enough traders to move your goods without your factories or your traders idling. 
- the only mule that can add tradewares is the station mule with the "make target wwarehouse" option checked.
- the new supply mule can actually work as a profit-seeking trader, when assigned to an AI station. He will prioritize player owned goods so a good way to move something you have a lot of stock of, is to assign a couple of supply mules to an AI station with a high volume of demand. 
- My preference is to chain together warehouses from one big warehouse that most factories feed into. This is the most efficient way to set up station mules such that all of your wares are added to your trade stations.
- if the mules are idle, it means they're not finding trades. 99% of the time this is not due to a bug, but simply because no trades pass their cuts. Usually this means you have too many mules and not enough production to keep them busy. What I like to do is repurpose idle mules either to other mule jobs, or some type of auto trading (Tater Trader is my preference) until there is more work for them. 

