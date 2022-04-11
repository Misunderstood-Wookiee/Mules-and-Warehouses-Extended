# Mules-and-Warehouses-Extended: A mod for X4 Foundations
A continuation and extension of Mules and Warehouses v4.
That has been optimised and improved for current versions of X4 Foundation.

Based on [Mules and Warehouses v4.0](https://forum.egosoft.com/viewtopic.php?t=417350) by LegionOfOne, which in turn is based on [Station Mule - Multiple ware station supply](https://forum.egosoft.com/viewtopic.php?f=181&t=411837) by leecarter.

## Table of Contents

- [Install](#install)
- [Usage](#usage)
    - [Station Mule](#station-mule)
    - [Distribution Mule](#distribution-mule)
    - [Travel Mule](#travel-mule)
    - [Supply Mule](#supply-mule)
- [Some General Mule Advice](#some-general-mule-advice)

## Install
For general releases of this project please see the following

* Steam Workshop: https://steamcommunity.com/sharedfiles/filedetails/?id=2013772865
* Nexus Mods: https://www.nexusmods.com/x4foundations/mods/416

## Usage

We **strongly suggest** reading the original author's thread, where the *station mule*, *distribution mule*, and *travel mule* are described well:
https://forum.egosoft.com/viewtopic.php?t=417350

### Station Mule
**TLDR**: Station mules provide a way to set up static trade routes in your empire.

Each mule can be set up to bring wares from a factory (**Source Station**) to a warehouse (**Target Station**) and return with the goods needed for production (if **Two-way trade** is checked).
A second set of station mules can be enlisted at your warehouses to bring excess wares to your trade stations (with **Make Target Warehouse** checked).

### Distribution Mule
**TLDR**: The distribution mules are meant to distribute wares between your warehouses so that every warehouse has at least some of each ware in stock.

It will try to take the **Wares** from your **Source Warehouse** to your **Target Warehouses** depending on their respective stock levels.

- **Min Storage** - The desired percentage of storage for each ware on each target station in the list
- **Static storage** - The desired stock level on the source station
- **Max Storage** - An amount on the source station that will force a distribution even if the target is above min storage.

Most of the time distribution mule bumps up against either min storage or static storage. To be less restrictive, you generally need to *increase* min storage and *decrease* static storage to get wares moving.

### Travel Mule
**TLDR**: The travel mules are meant to sell your wares to the AI to make money.

They will take **Wares**, based on the highest volume, from the **Source Warehouse** and sell it to AI stations in the range of **Max Jumps** if a profit can be made.
You can overwrite the target AI station selection by setting a **Custom Stop List**.

The Travel mule now has sliders that allow you to be less restrictive on what trades to take out from your stations. These options should allow you to open up your trade space to make sure goods are moving:

- **Player Buy Mod** A percentage multiplier applied to the player price in profit calculations. If it is 100, we use the station managers price. If it's 0, we ignore the price altogether. 50 means cut the manager's price in half, etc. In our experience this is usually the biggest problem; if your stock is somewhat low for any reason, the price will be too high for your travel mule to find a trade.
- **Fill Cargo Hold %** The Mule will require the trade to fill up their hold past this percentage for it to be considered valid.

You generally want a few travel mules on all of your warehouses so that your goods are going out to make you money. If you notice Travel Mules idling, either tinker with their sliders to encourage more trades, or re-assign them to a different task. It could be that you're just not producing or distributing fast enough to keep up with the number of travel mules you have set.


### Supply Mule
**TLDR**; The supply mules are meant to buy supplies for your station to keep production going or buy cheap wares for your trade hubs.

The supply mule attempts to fulfil all of the players buy orders in the selected area at the cheapest price possible. While this may not directly produce profit, it reduces theoretical losses. For instance, let us say you have a factory that wants to sell graphene at 130 Cr/unit, and a TradeStation that wants to buy it at 150 Cr/unit. The AI could theoretically come along and buy from your factory, then sell to your trade station, and you would realize a loss. The supply mule will see this trade and attempt to fill it. It reduces price inefficiencies in your empire.
The supply mule can also be assigned to AI stations to boost their production while earning you a bit of cash. This can be especially useful to supply shipyards or contested regions of space.

In general, the mule prioritizes build storage, then production needs, then intermediates, then tradewares. It prefers to source from player-owned resources and will take any trade that reduces inefficiency before going out to AI stations. It has the ability to queue up multiple trades until its cargo is full before coming to sell everything (this particular routine isn't optimal in terms of pathing). The supply mule obeys your global blacklists and any trade rules.

#### Options

- **Home Station/Sector** - Can be any station or sector. Behaviour in different scenarios is described below.
- **Assign Ship-To Station** - Assigns the ship as a subordinate with the trader role. Serves no functional purpose except convenience.
- **Max Jumps From Home** - Maximum distance from home to serve stations as a supplier and to source supplies. This is based on pilot skill.
- **Serve Source Only** - Will only supply the station/sector that is set.
- **Gather Player / AI Faction** - Allows mule to buy wares from player / AI stations respectively.
- **Allow Buildstorage / Resources / Intermediates / Tradewares** - Allow a mule to supply the stations build storage / resources / intermediates / trade-wares respecively.
- **Lock Ware List** - If you choose specific wares, you need to check this option for your choices to stick across trade runs. Otherwise, the script populates the list on the UI with the needs that it found when searching for things to do.
- **Warebasket** - Wares the mule trades with (if not locked, will be auto-populated and updated by the script with all wares of the station to supply)
- **Max Trades** - The max number of stops that can be made in an attempt to fill up the cargo hold. Each trade will have a minimum size of (100 / MaxTrades)% of the cargo hold full. i.e. a setting of 2 means each trade must be at least 50% of the hold. You can use this to avoid supply mules running low volume trades.
- **Player Buy Mod** - Can be used to fake the sell prices on player-owned stations for profit calculations. As an extreme example, you can set it to 0 and the sell prices from player stations will be ignored.

There are branches of priorities depending on whether a station is set in the UI and whether that station is owned by the player or the AI.

#### Mule Priorities
##### When a sector is set as home
1. Supply all player stations in **home sector**.
2. Supply all player stations in **Max Jumps** range (unless **Serve Source Only** is set).

##### When a player-owned station is set as home
1. Supply **home station**.
2. Supply all player stations in **home sector** (unless **Serve Source Only** is checked).
3. Supply all player stations in **Max Jumps** range (unless **Serve Source Only** is checked).

##### When an AI owned station is set as home
1. Supply assigned AI station.
2. Supply all AI stations in **sector** (unless **Serve Source Only** is checked).

##### Supply priorities based on the ware type
1. **build storage** (if checked),
2. **resources** for production (if checked),
3. **intermediates** (if checked),
2.  and lastly **tradewares** (if checked).

##### Supplier priorities (from whom to buy the needed ware)
1. **Player** faction (if checked)
2. Any friendly **AI** faction (if checked).
If there are multiple possible trades, the supply mule will choose **based on potential profit**.


### Some General Mule Advice
- Make small changes at one time. Don't add storage to every Warehouse/Factory all at once. Don't add 10 travel mules or supply mules all at once. Your goal is to have just enough traders to move your goods without your factories or your traders idling. 
- The only mule that can add tradewares is the station mule with the "make target warehouse" option checked.
- The new supply mule can actually work as a profit-seeking trader when assigned to an AI station and will prioritize player-owned goods. So a great way to move something you have a lot of stock of is to assign a couple of supply mules to an AI station with a high volume of demand. 
- Our preference is to chain together warehouses from one big warehouse that most factories feed into. This is the most efficient way to set up station mules such that all of your wares are added to your trade stations.
- If the mules are idle, it means they're not finding trades. 99% of the time this is not due to a bug, but simply because no trades pass their cuts. Usually, this means you have too many mules and not enough production to keep them busy. What I like to do is repurpose idle mules either to other mule jobs, or some type of auto trading (Tater Trader is our preference) until there is more work for them.
- **For a minimal performance impact**, reduce the maximum number of trade offers the mule evaluates! **Reduce** the number of **Max Jumps** for travel and supply mules as much as possible, since every buy offer will be compared to every sell offer!



[![CC BY 4.0][cc-by-shield]][cc-by]

This work is licensed under a
[Creative Commons Attribution 4.0 International License][cc-by].

[![CC BY 4.0][cc-by-image]][cc-by]

[cc-by]: http://creativecommons.org/licenses/by/4.0/
[cc-by-image]: https://i.creativecommons.org/l/by/4.0/88x31.png
[cc-by-shield]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg
