## Mules-and-Warehouses-Extended: A mod for X4 Foundations
An continuation of Mules and Warehouses v4 & Mules Extentsion.

### For general releases of this project please see the follwing
* Steam Workshop: https://steamcommunity.com/sharedfiles/filedetails/?id=2013772865
* Nexus Mods: https://www.nexusmods.com/x4foundations/mods/416

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

