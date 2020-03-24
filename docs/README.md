# RTS Balancing

I am Marc Gallardo Quesada, a student in the [Bachelor’s Degree in Video Games by UPC at CITM](https://www.citm.upc.edu/cat/estudis/graus-videojocs/). This research is generated for the second year’s subject Project II.


# Introduction

In this project, I will be covering the basic concepts of balancing an RTS game and taking a deep dive in the complexity that they  carry. Furthermore, I will explain some of the methods used in order to reach our desired objective.

# Index
* 1. Game balance

* 2. Unit balancing
    * Applying zero-sum games to RTS
      * Results analysis
    * Theory applied to armies
    

* 3. Asymmetrical balancing
    * Ways to approach asymmetric design

* 4. Defining gameplay styles in RTS

* 5. Resource systems, management and control of its economy
    * Rate of expense
    * Types of gathering
    * Control of economy and player agency
    
* 6. Technology tree
    * How to create our own technological tree?

* 7. Artificial Intelligence (AI)
    * Make machine driven units more resilient
    * Make the AI have different personalities
    * AI needs to be predictable
    * AI should be able to interact with the game's systems
    * Make the AI react to player's actions
    * Good AI for friendlies!
    
* 8. Single-player modes
 
* 9. Map design and balance for SCII
    * Chokepoints
    * Openness
    * Resource
    * Base location
    * Map symmetry
    
* 10. Conclusions and personal thoughts on the topic
  
* 11. Bibliography, Webgraphy and Videography
  * Links
  * Abstracts/PDFs
  * Videography
    
## 1. Game Balance

First of all, in game design, the concept of balance is closely related to adjusting the rules and fine-tuning the stats for different game elements in order to avoid the abuse of one really strong mechanic/unit. Contrarily, you also don't want an ineffective or useless element that no one desires to use because of the fact that said element is overshadowed by everything else.

In RTS, balancing is a core aspect of the design of this specific genre. This is because a slight over tune on a stat on a specific troop can make the game revolve around creating and swarming with said troop. Thus making the game stale and one-dimensional rather than one relying on wits and outmaneuvering your opponent with a superior strategy.

<iframe width="560" height="315" src="https://www.youtube.com/embed/e31OSVZF77w" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 2. Unit balancing

Arguably one of the most important aspects of an RTS game is the interactions between units in the battlefield to make the game feel like the strategies they've developed during the match have paid off.   

In RTS games, the player has a wide array of possible options when it comes to troop creation. Said troops **must** have a strength. This means that the unit must be better than other ones at doing a specific task or job. Nevertheless, they also **must** have some kind of inherent weakness that the other player facing that unit can exploit. And, more often than not, the mix of weaknesses of the units it's what makes the game truly fun, and that's what we are aiming to create.

### Zero-sum games
First and foremost, I will explain the most basic and traditional way of balancing elements, not just in video games but it can be used in board games or any game in general. And that's achieved by applying the essential theory of a zero-sum game. This theory explains that no matter what choices or decisions both players take, the sum of the outcome between positive or negative results **is always equal to 0**.  
Therefore, this means that there is no "best strategy" because of the fact that one choice can be countered by something else.

To exemplify this concept, I will use the prime example of a zero-sum game, Rock-Paper-Scissors. I will use a [pay-off table](http://kfknowledgebank.kaplan.co.uk/KFKB/Wiki%20Pages/Payoff%20tables.aspx) to better visualize all the possible outcomes. Keep in mind that we will be going back to this concept and using it as the research progresses.

R-P-S represents the choices we have.

r-p-s represents the choices the enemy has.

|-|r|p|s|
|---|---|---|---|
| **R**| 0 | -1 | +1|
| **P**| +1| 0  |-1 |
| **S**|-1 | +1| 0|

Let's convert these numbers into the ratio of one player choosing one option vs the others. Theoretically speaking, the ideal ratio would be 1:1:1 since there's no throw that's used more often than the others and no choice grants you a higher win percentage.

Taking into account the results provided by the table, we can extract the following formulas that will help us determine the payoff for each choice:
```
R = 0r + (-1)p + 1s

P = 1r + 0p + (-1)s

S = (-1)r + 1p + 0s
```

Using basic simplification methods we obtain:

```
R = s - p

P = r - s

S = p - r
```

Since this is a zero-sum game, we can assume that the total payoff, regardless of the matchup that takes place the place, will be of 0. Therefore, we can assume that `R = P = S = 0`

And the fact that our opponent must select throw, we can also make the assumption that 

`r + p + s = 1`

With all these simple equations we can solve:

```
R = 0 = s - p -> p = s
```
We apply the exact same procedure for the last 2 equations and we get the following:

```
r + p + s = r + r + r = 1
3r = 1 
r = 1/3
```

Since r + p + s = 1, **p = 1/3** and **s = 1/3**

Theoretically speaking, these results shows us that the pick rate of how often an action is taken is equal across all choices. But in the end of the day, humans would try to play randomly but they will inevitably fall under certain patterns and if you manage to pick up the habits that a certain player has, you can exploit it in order to win more games by countering their choice that is most likely to be taken.

**Note:** This is just the extensive analysis of a zero-sum game. In the next section we will be using the formulas we got from here and we will try to apply it to any RTS game in order to balance it.

### Applying zero-sum games to RTS

Now we will create our own theoretical RTS game where we have 3 units: The soldier, the sniper and the jetpack. The soldier counters the sniper, the sniper counters the jetpack and the jetpack counters the soldier.
For the calculations, instead of using ratios, we will now use a term widely used in RTS, and that's the resources units cost. We will also subrtract a percentage of the cost based on the HP that the unit has lost but it's not dead yet.

|-|Cost| % of health lost|
|---|---|---|
| **Soldier**| 50 | 0 | 
| **Sniper**| 100 | 40% |
| **Jetpack**| 150 | 60%|

The calculations for the following payoff table will be similar to what we have applied in the first section of the research where we were calculating the ratio of usage. In this case we will apply very similar equations to get the payoff or remaining value of a unit, or the value remaining when they face each other, for every possible encounter with our 3 theoretical units. The equation that we will use is the following 

`Payoff = Enemy cost * (% of lost HP / 100) - Allied cost`

|-|Soldier|Sniper|Jetpack|
|---|---|---|---|
| **Soldier**| 50 - 50 | 100 - (50 * (40 / 100)) | (50 * (0 / 100)) - 150  |
| **Sniper**| (50 * (40 / 100 )) - 100| 100 - 100  | 150 - (100 * (60 / 100)) |
| **Jetpack**| 150 - (50 * (0 / 100)) | (100 * (60 / 100)) - 150 | 150 - 150 |

Payoff equations for each possible encounter:

```
Soldier(S) vs Soldier(s') payoff: 50 - 50
Soldier(S) vs Sniper(p') payoff: 100 - (50 * (40 / 100))
Soldier(S) vs Jetpack(j'): (50 * (0 / 100)) - 150

Sniper(P) vs Soldier payoff: (50 * (40 / 100 )) - 100
Sniper(P) vs Sniper(p') payoff: 100 - 100
Sniper(P) vs Soldier payoff: 150 - (100 * (60 / 100))

Jetpack(J) vs Soldier(p') payoff: 150 - (50 * (0 / 100))
Jetpack(J) vs Sniper(p') payoff: (100 * (60 / 100)) - 150
Jetpack(J) vs Jetpack(j') payoff: 150 - 150
```
Payoff table for our theoretical units:

|-|Soldier(S)|Sniper(P)|Jetpack(J)|
|---|---|---|---|
| **Soldier(s')**| 0 | 80 | -150 |
| **Sniper(p')**| -80 | 0  | 90 |
| **Jetpack(j')**| 150 | -90 | 0 |

Now we will analyze the pick ratio of these units. Remember that in a R-P-S game or zero-sum game the pick ratio should be close to 1/3.

```
S =  0s' + 80p' + (-150j')
P = (-80s') + 0p' + 90j'
J = 150s' + (-90p') + 0j'
```
**Note:** We know that for total payoff of a zero-sum game will be 0. So we can assume `S + P + J = 0`. We can also assume that the probability of playing something will be of 1 because they will have to crate units in order to win. So we can also assume that `s' + p' + j' = 1`

```
S = -150j' + 80p' = 0
P = -80s' + 90j' = 0
J = -90p' + 150s' = 0

80p' = 150j' --> p' = (15/8)j'
90j' = 80s'  --> j' = (9/8)s'
150s' = 90p' --> p' = (9/15)s'

s' + p' + j' = 1

s' + (9/8)s' + (9/15)s' = 1 --> 109/40s' = 1 --> s' = 0.37
j' = (9/8) * 0.37  --> j' = 0.41
p' = 1 - s' - j' = 1 - 0.37 - 0.41 = 0.22

```

#### Results analysis

As we can see from the pick ratios we can assume that our game is decently balanced because the pick rates are fairly similar. There is no dominant unit that rules above other, thus there's no unit that players can abuse and utilize to win games. Some will argue that the Jetpack is a pretty dominant unit who is chosen 41% of the time. To balance the fundamental strength of a unit, we can lock it behind an upgrade in our technological tree, which will be explain further into the research but you can check what this concept is [here.](https://github.com/Marchusky/RTS-Balancig/blob/master/docs/README.md#6-technology-tree)

In case we would want a perfectly balanced game where all units are chosen the same amount of time, by simply looking at the ratios, we can assume that either snipers are not very cost effective, therefore we should lower their cost, or jetpacks are offer way more value for its cost than other unit. So by rising the cost of this unit it should drive people away from  mass-producing this unit. Although, this procedure is **not encouraged** unless there's a clear dominance from one unit.

What we actually want is not a perfectly balanced game where units have the perfect trade off every single time. We want **some units to deviate from the balanced curve** that our game inherently has. Developers from Wizards of the Coast called it ***The Jedi Curve***, and they allowed their cards to deviate from said curve positively or negatively by 10-15%. this brough a wider array of options to choose from by simply making some units stronger than others.

<p align="center">
  <img  src="https://raw.githubusercontent.com/Scarzard/RTS_Balancing/master/docs/Web%20Images/jedi%20curve.png" width ="500">
</p>


### Theory applied to armies

As we all know, RTS does not involve in 1v1 fights between only 3 possible units. In this genre, massive-scale battles takes place in the map and throughout the course of the match so, even though the calculations we did previously greatly helps in balancing units, **it is not a definitive way to balance them.** The process is **arduous and repetitive because it involes a lot of playtesting and trial and error.**

In this section, we will discuss a way to get the abstract value of an army vs the other. For simplicity's sake, we will use the units we have created before.

We will assume that one player has a the following army: 15 soldiers, 19 snipers and 10 jetpacks. 

And the other has an army of:                            26 soldiers, 7 snipers and 12 jetpacks. 

P1 has 44 total units. P2 has 45 units. So the armies have an extremely similar value when it comes to numbers. Now let's try to analyze this mathematically.

`Army value(AV) = PP + NP`

`Negative payoffs (NP) = (soldier_count / enemy_jetpack_count) * payoff + (sniper_count / enemy_soldier_count) * payoff  + (jetpack_count / enemy_sniper_count) * payoff `


`Positive payoffs (PP) = (soldier_count / enemy_sniper_count) * payoff + (sniper_count / enemy_jetpack_count) * payoff  + (jetpack_count / enemy_soldier_count) * payoff `



First, we will calculate the overall value for one army and the the other. This is P1's value:

```

Negative payoffs (NP1) = (15 / 12) * -150 + (19 / 26) * -80  + (10 / 7) * -90
NP1 = -187.5 - 58.4 - 128.6 = -374.5
Positive payoffs (PP1) = (15 / 7) * 80 + (19 / 12) * 90  + (10 / 26) * 150
PP1 = 171.4 + 142.5 + 57.7 = 371.6
VA1 = 371.6 - 374.5 = -2.9

Negative payoffs (NP2) = (26 / 10) * -150 + (7 / 15) * -80  + (12 / 19) * -90
NP1 = -390 -37.3 - 56.8 = -484.1
Positive payoffs (PP2) = (26 / 19) * 80 + (7 / 10) * 90  + (12 / 15) * 150
PP2 = 109.5 + 63 + 120 = 292.5
VA2 = -484.1 + 292.5 = -191.6

VA2 < VA1 --> [-191.6 < -2.9]
```

In this clash of compositions, the winning team would theoretically be the VA1, assuming they play a perfect micro.

This was an excercice to determine which army would win vs another. But in practice this would be rendered useless because of the fact that there are **so many** variables to take into account that determines the outcome of a match that is virtually impossible for someone to pinpoint said variables. And it's way more difficult to actually give the variables a mathematical value which we can use to compute what will be the outcome.


## 3. Asymmetrical balancing

So far, we have assumed that both players have the same exact units and structures. But in reality, some RTS follow this design and some choose to apply asymmetry in their game. And even if they don't apply and asymmetric approach, like in Warcraft II, where units from the Alliance or the Horde have the exact same stats (Dragon and Gryphon, Grunt and Footman) but they are visually different. What differs is the spells that each player gets from their units. This is a great way to make you game symmetrical when it comes to troop stats so it's easier to balance them. But having differences in the spells that you can use makes the faction adapt to a certain play style or another.

Asymmetrical balancing is the way to make a game with factions or different themes for troops unique and make the players employ a certain strategy or another based on the strengths and weaknesses that each different faction carries.

It is not necessary to make you game have additional factions because it might not even improve the game and making a game that has no mirrored factions is resource-consuming for the developer team. And you might be working on a feature that has no real positive impact on your game whatsoever, will make it harder to balance and you will lose time on a feature that might hinder the basic game flow.

<p align="center">
  <img  src="https://raw.githubusercontent.com/Scarzard/RTS_Balancing/master/docs/Web%20Images/terran%20protoss%20zerg.png" width ="500">
</p>

*All credits for this image belongs to [Keyan3D](https://www.deviantart.com/keyan3d)*

### Ways to approach asymmetric design

The most basic and yet most important factor on considering the implementation of different factions is **variation.** This concept prevents the players to feel that the game is getting being repetitive and stagnant.

The second one, closely related to the different strategis that players might want to utilize. Because, as we said, each faction has its inherent strengths and weaknesses, thus, catering a certain player base or another.

Asymmetric design can be a tough thing to deal with when not implemented correctly because even if each faction excels in some way, having them balanced out in a taking into account all the possible variables that an RTS offers, can be a daunting task to deal with. What we should keep in mind is that even though certain factions are strong at employing a specific strategy, there's also a weakness to deal with. And following the concept of strength vs weakness from the unit balacing section, weaknesses often are the essential part that every faction should carry so that his enemy can create a plan around exploiting that weakness and giving him an edge.

You should play around you game's strengths and reinforce them with asymmetric design. What does this mean? That if you game has an excellent combat system, don't confuse the player by also implementing a complex resource system. Streamline the resource management as much as possible so that the players spend more time fighting than colleting.

Asymmetric design if often related to map design, more so in StraCraft, where map design plays a huge role on keeping in check that strengths that each faction has. We will cover this topic later on the research.

This video explains further into what asymmetry requieres and how it makes games fun. It also gives a lot of examples:

<iframe width="560" height="315" src="https://www.youtube.com/embed/F1w-qCbYVe8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 4. Defining gameplay styles in RTS

- **Rushing:** Rushing is the most simple yet effective strategy in an RTS game. This strategy revolves around the early creation and manipulation of cheap and expendable units as fast as possible. Then you send them rushing into the enemies' base before thay even get the chance to build up and economy or even build enough defensive troops. However, if the enemy anticipates this move and builds a counter for it, it sets the rushing player in a really precarious position where they spent all their initial resources into an army that died. Thus, setting them behind in all the aspects of the game. Such as expanding, base building and tech tree climbing.

  - **Zerg rush:** This concept is brought to its peak with the StarCraft faction, the Zergs. The Zergs are able to rapidly produce a huge army of small, offensive units called Zerglings. This army allows the player utilizing the strategy to quickly overwhelm his enemy early in the game. 
  
  **Trivia:** If you seach "Zerg rush" in Google, an easter egg wil pop put where you have to destroy "zerglings"! It even counts the APM (Actions Per Minute)and it will keep track of the killcount.

- **Turtling:** Tutrtling, as the name defines, it focuses on base building and creating an impregnable defense that no matter what army the enemy brings, it wil hold off any offensive approaches. Then, when the player manages to repel and kill the enemy army, he retaliated with a counter attack that the enemies can't react in time to because they have spent all their resources on and army that is long dead. 

- **Map control:** Map control refers to the strategy involving control major parts of the map so he has more options to expand to and collect more resources than the enemy. Thus establishing an economic advantage that the enemy can't, in any way, counter because he doesn't have enough agency to do so. The downside for this tactic is that having to defend each point of contention in a wide area is often not even possible without prediction and foresight.

- **Booming:** This strategy focuses on climbing the tech tree as fast as possible in order to unlock sturdier and more effective troops than his enemy. This leaves you main base completely defenseless against early enemy attacks. So you must identify players that will rush by scouting their base earlier than usual.

- **Raiding:** Raiding focuses on small-scale skirmishes to make the enemy react to his actions. This often distracts the foe from effectively gathering resources and make small mistakes that over time, can cost him the game. And if the enemy doesn't defend from the raids, he can lose potential map control and thus, agency that will make him fall behind in every aspect of the game. A major downside is that if you gradually get countered when you raiding, the one falling behing would be one employing the strategy. 


## 5. Resource systems, management and control of its economy

The dawn of resource systems can be tracked back in the 1990s where Dune 2 had a single collectable resource that was melange (or spice) that was gathered from sand and could be sold at certain locations for credits. And credits were used to upgrade units and structures.

The system were a worker (or harvester) has to manually go to the location of the resource, collect it and bring it back to the main base was also developed in this game. 

Since this pioneering design, different games have experimented different types of resources at the same time, its collection or gathering and the rate of expense of said resources.

### Rate of expense

- **Continuous:** When spending any resource on something, the credit count is **continuosly** subtracted from the player's bank until the purchase/upgrade is complete. This means that you can actually buy something that you initially don't have enough credits to buy. It can also be possible that you make multiple purchases and none of them will be completed. This system plays heavily with the income per second of the player.

- **Discrete:** When making any kind of transaction, the credits are substracted **immediately** from the player's bank. This means that the player cannot make a purchase they cannot afford at that exact moment. This really simnplifies the economy that the players has to manage and allows the players to focus more on other important aspects of the games, such as the state of the macro or in base-building.

**Note:** Essentially, both systems would yield the same exact amount of resource (assuming that the gathering rate is the exact same for both systems). However, the decision making wildly varies between these two systems. The first one you could actually begin to build something ahead of time in order to cut off in building time. The second one is way more intuitive as anyone knows if they have enough credits to make a purchase. And in the case they don't, usually the game itself will alert the player that his action cannot be taken.

### Types of gathering

- **Active:** This usually requires 3 game entities in order to start producing income. One is the storage, the other one the worker and the last one is the resource field. This often means that there are limitations in the amount of resource available for each player. They will have to fight in order to establish control and be able to outgrow the enemy in terms of economy.

- **Passive:** The only entity needed is the one that would be extracting/gathering the resource. This simplifies the gathering a lot as the only thing the players has to do is to defend his collectors and try to sabotage or destroy the oponents'. Usually, the amount of resource is infinite and it's not bound to the abundance that the maps has.

### Control of economy and player agency

In this section, we will analyze how the control of the game's economy (its resources) affects how the player can influence the game's state. 

In RTS, one of the quintessential aspects is having an economic system of some sort so that players must have a deep understanding of said system in order to find any possible exploits in order to leverage the advantage to their side. The absolute core of an RTS is not having hordes of units having a massive battle, but it's closely related to the acquisition and expenditure of what we will call **value.** Value is the pure definition for resources, not just the resource that any game uses per se, such as minerals and gas StarCraft but anything that you can purchase, trade or upgrade with it.  

This is where player agency comes to play. The definition for this concept can be summarized as "the ability of any player to interact and change the game's state throughout the course of the match.

So why this aspect of RTS is the root of almost all games in this genre? The key word is progress. This system allows putting pressure on the players the moment a match begins for a race on who is the one that can collect resources more efficiently and spend it in the best possible way taking into consideration how his enemy plays. This is closely related to the overall state of the economy for both players. Progress is often represented with physical objects that both users can interact with (units, structures or upgrades). The player has to intentionally invest his income in order to progress through the match by building, fighting and upgrading his base and army. 

This means that both players have the agency to **directly** interfere with his enemies' economy at any point of the game by sending troops to sabotage or preferably to destroy his collectors or kill his harvesters. This means that players **ought to lose** invested value in the form of buildings/troops. The more proactive and thoughtful actions that one players takes, the more overall agency he will possess. This means that he will have a wider array of options to influence the state of the game compared to his opponent, who fell behind because one player took more proactive actions, therefore, rising his agency throughout the course of the match.


## 6. Technology tree

A technology tree is the visual representation of all the possible upgrades and what said upgrades unlock. This is closely related to the relationship between building and units and how both of these concepts are structured in a hiercarchical manner. 

The main usage for these graphs, is for the developers to have a deep understanding of the game elements in their application and in which way they interact with one or the other. The second usage for these is related to its player base. They are extremely useful for new players to slowly grasp the main mechanics of the game and, therefore, develop their own strategies when they have enough experience. It should be noted that the vast majority of the games **won't offer this graph to its players**. But there are instances where the players themselves reverse-engineer the game to obtain the graph by their own means.

As we can see in the following images, TTs can vary from fairly simple, to complex, to absolutely bonkers.


<p align="center">
  <img  src="https://raw.githubusercontent.com/Scarzard/RTS_Balancing/master/docs/Web%20Images/SCII%20TT.jpg" width ="500">
</p>

*StarCraft II Tech Tree*

<p align="center">
  <img  src="https://raw.githubusercontent.com/Scarzard/RTS_Balancing/master/docs/Web%20Images/Warcraft%20TT.jpg" width ="500">
</p>

*Warcraft Tech Tree*

<p align="center">
  <img  src="https://raw.githubusercontent.com/Scarzard/RTS_Balancing/master/docs/Web%20Images/PoE%20TT.jpg" width ="500">
</p>

*Path of Exile Tech Tree*


### How to create our own technological tree?

In order to make our own technology tree(TT) we have to know what elements compose a TT: building and its upgrades, units and its upgrades.

For simplicity's sake, we will use the units he have created earlier in the unit balancing section. So we have 3 possible units that we are able to create. We will also need some sort of buildings that manages the upgrades of the units and allows us to create more powerful ones. Here's a list of what we will have available:

|Buildings|Units|Upgrades|
|---|---|---|
| Town hall| Soldier | Soldier upgrade | 
| Barracks| Sniper | Sniper upgrade  | 
| Barracks II| Jetpck |  Jetpak upgrade | 

The TT should be a representation of how all these elements are linked together to understand how your own games works. But it should also be useful for players as a visual guide on how different game elements interact with eachother. 

The next step is thinking on what units should be available the moment a match starts and which units should be unlocked as the player progresses on the TT. What I would do in this specific case, is to have the snipers unlocked at the beginning while the other troops are locked behind a building. This means that you have to build said building to unlock it. After you build the barracks, you unlock the counter for sniper's the soldier. And at the same time, the sniper upgrade is offered to the player by the town hall. So the first important decision that the player should take is whether upgrade snipers or unlock their counter. After building barracks, the player is offered if he wants to upgrade his soldiers or unlock the counter for the soldiers, the jectpacks, by upgrading his barracks. And last, the barracks II offers the player to upgarde his jetpacks so thei are more powerful. 

Let's schematize what we just discussed in the image below:

<p align="center">
  <img  src="https://raw.githubusercontent.com/Scarzard/RTS_Balancing/master/docs/Web%20Images/tech%20tree.png">
</p>

## 7. Artificial Intelligence (AI)

AI must fit the game's intended experience. Good AI is not only the one who can beat the player. It's one that offers interesting gameplay situations and complex decision-making. The complexity of the AI within any game is **directly proportional** on how you realistic you want the machine to feel.

In this section, we will go through some main points that you can apply to you game so that AI feels more realistic or challenging.

*Keep in mind that this in not a technical guide on how to implement AI in your game*

### Make machine driven units more resilient

By making enemy units that the game itself controls, it makes the player thing that they are more intelligent that they actually are. This sensation is achieved because he feels challenged when facing tougher enemies, therefore, he needs to build and think strategies that can work against units that have more HP and damage. The process of making the player come up with a strategy indirectly makes him think that the AI is harder to deal with.

As we can see in the image below, the percentage of players that claimed that the AI was "very intelligent" jumped from a mere 8% when playeres where facing low damagin, low HP monsters. In contrast, this percentage skyrocketed to an astounding 43% when players where facing high damaging, high HP monsters.

<p align="center">
  <img  src="https://raw.githubusercontent.com/Scarzard/RTS_Balancing/master/docs/Web%20Images/tougher%20enemies%20graph.png" width ="500">
</p>

*Photo taken from [this](https://www.youtube.com/watch?v=9bbhJi0NBkk) video By GameMaker'sTollkit*

### Make the AI have different personalities

You should make every encounter and battle feel unique in a way that can be memorable for the player. Maybe one opponent can have a build order optimized in getting map control early in the game so they establish a superior economy than the player, so they would be owerwhelmed by a huge army. Perhaps another can have a build order that's suited for fast-paced games where the AI rushes the players base early on and tries to win via early and successful skirmishes.

A great example of this concept is portrayed in the game Civilization's single-player modes, where the leaders all have their own uniqueness, therefore, you have to develop a strategy for that single leader. And probably the same strategy won't work against another foe.

### AI needs to be predictable

Halo's Tech Lead, Chris Butcher said: 
> "*The goal is not to create something that is unpredictable. What you want is AI that is consistent so that the player can give certain inputs. The player can do things and expect the AI will react in a certain way.*"

This allows to play the game with **intentionality.** This is defined by Far-Cry's 2 designer as the following:

> "*The ability of the player to devise his own meaningful goals through his understanding of the game dynamics.*"

By making AI predictable, it encourages the player to develop and ideate plans that when executed successfully, is satisfying to observe the results. It wouldn't be the same if the same plan only worked half of the time. 

### AI should be able to interact with the game's systems

This is crucial when making an AI seem like it's alive and smart. This makes the player feel like the machine has the same choices. This behaviour can also be exploited by the player if he ever perceives a pattern in its behaviour. 

In RTS games, this could be done by making the AI gather and manage resources in order to develop his own strategy (even if the machine has a higher income to make it harder to play against), instead of granting them for free and make them unlimited for the AI. Thus, making it nearly impossible for the player to deal with.

### Make the AI react to player's actions

If the machine notices that the player is focusing on getting a superior map control, then the AI should fight for control too. You have to make that the AI is constantly trying to sabotage and get and edge over the player. It shouldn't blindly follow a programmed build order and stick to it no matter what. These are the instances where the machine feels like a machine. And we should always try to avoid that.

This is also crucial to make every encounter feel unique. Because it prevents the player from just setting up a specific build order and beating every campaign with the same boring, unchanging strategy.

### Good AI for friendlies!

This is not intended to be applied to RTS because rarely, if not never, you won't be working alongside an AI-driven ally. But it's a nice way to add more value and meaning to the game. 

An extremely good example analysing Final Fantasy XV's character, Prompto, who will be taking selfies when certain thresholds have been triggered or when the character feels like doing so. Gameplay wise, this has no value whatsoever. But it adds a tremendous amount of complexity to Prompto's character and makes him feel like he's self-aware.

<p align="center">
  <img  src="https://raw.githubusercontent.com/Scarzard/RTS_Balancing/master/docs/Web%20Images/prompto%20selfie%20system.jpg" width ="500">
</p>

*Prompto's selfie system in the form of a flowchart*

In this video, it goes more in-depth about more sections that you should take into consideration when creating AI:

<iframe width="560" height="315" src="https://www.youtube.com/embed/9bbhJi0NBkk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 8. Single-player modes 

So far, we have discussed balancing taking into consideration that our player is facing another player. But what abou the importance of single-player modes? These offer the players to play the game in a way that cannot be thought about in PvP games. This often rely on advanced map design and making the player think outside the box in order to beat the levels.

I will be discussing the fascinating world of single-player modes on specific level in the [original Command&Conquer](https://en.wikipedia.org/wiki/Command_%26_Conquer).

*Disclaimer: All the images and information are extracted from the following website. All credits to the original author, [Dr. Mark R. Johnson](http://www.ultimaratioregum.co.uk/about-me/)*

First of all, the objective of this mission is to earch the main objective, it's marked witha green cross. As we can observe, there's a chokepoint and the path is blocked by a pair of Mammoth Tanks(MT) one of these units alone would be enough to wipe you entire army, that is composed of 1 Mobile Contruction Vehicle, vital to beat this game, 1 light tank and 2 attack bikes with 2 buggies attached to them. Making a total of 6 starting units. This level requires you to cleverly utilize your extremely limited army to beat this seemingly impossible level and take advantage of the map's layout.

<p align="center">
  <img  src="https://raw.githubusercontent.com/Scarzard/RTS_Balancing/master/docs/Web%20Images/step1.png">
</p>

The first step in order to beat this level is having one of you bikes lure a single MT and move it away from the chokepoint. The only thing you should do is lure it clockwise with the huge montain in the top-middle of the map. Once lured, you proceed to do the same whith the other buggy when the 1st one reaches 12 o'clock. After this has been done, you have to move your MCV to the destination.

<p align="center">
  <img  src="https://raw.githubusercontent.com/Scarzard/RTS_Balancing/master/docs/Web%20Images/step2.png">
</p>

After this is done and you have your base under construction, you have 2 options: destroy the tanks or have the lured forever. The first choice is the more optimal one since having to constantly micromanage 2 bikes while defending, expanding and upgrading you base would distract you from efficiently carrying out these tasks. So the way to do it is to send one attack bike after each MT so the MT chases the buggy while the attack bike pokes it from behind untill both are destroyed. 

<p align="center">
  <img  src="https://raw.githubusercontent.com/Scarzard/RTS_Balancing/master/docs/Web%20Images/strategy_chart.png">
</p>

*Visual representation of this tactic*

After you have dealt with both MTs, the waves will start to roll in and you'll have to defend your base at all costs!

**Summary:** this level design is carefully crafted so that the smallest detail has to play a big role in order for the player to beat the level. The first part of the mission a meticulous execution in order to surpass a seemingly impossible barrier that the MTs make.

If you wnat to design your own single player, keep in mind how this specific level is designed and take the points that you think are relevalnt for your making your ow. Because making levels that are based on base-building, expanding and evolving and the only variable factor is how tough/numerous the enemies are can become really tiresome to complete.


## 9. Map design and balance for SCII

Map design is a though and complex topic to handle, especially with abstract units and with a game that theoretically doesn't exist. That's the reasoning behind using one of the most famous RTS game of all time, StarCraft II.

In StarCraft II, unit balancing is not a topic that developers touch with frequency. What they focus on is designing maps that counter the factions themselves. What this means is that the developers utilize the map's layout and design to counter the inherent strengths of a certain faction. Be aware that this is not a statement claiming that StarCraft developer's don't balance out units often. Balancing is a recurrent topic when it comes to designing videogames for this specific genre. For example, every main base **needs** a defensive ramp to counter zerg's mobility. 

### Chokepoints 

Chokepoints are essential when designing a map because it creates a point of contention for both players because whatever lies on the other is significantly easier to defend, thus, making successful attacks to the location is greatly reduced. Chokepoints are made by two main concepts: the **width** and if it's a **ramp** or not. 
- **Width:** The width of a chokepoint is extremely important since a tight chokepoint makes ranged units excel and usually small skirmishes happen. But wider and more open chokepoints makes melee troops able to swarm and overwhelm. It also favors larger scale battles.

- **Ramp:** If a chokepoint lead to a different elevation then it's called a ramp. This is especially important in StarCraft because having control of the high ground means receiving a passive buff that lasts for as long as you control said zone. Ramps is the most common way to balance or neutralize the strengths of the Zerg faction, more specifically, the [Zerg Rushing strategy](https://github.com/Marchusky/RTS-Balancig/blob/master/docs/README.md#4-defining-gameplay-styles-in-rts)

<p align="center">
  <img  src="https://raw.githubusercontent.com/Scarzard/RTS_Balancing/master/docs/Web%20Images/ramp.jpg">
</p>


### Openness

The openness of a map refers to the shape of the overall region where the match is going to take place. This also determines how the zones of the map relate to each other, establishing at the same time how easy or hard it will be to defend said zone. 

The elevations of each zone should also be included in this section.

### Resource

In this section, you should decide how resources are split in the map. It's important that you place them in places where there will be contention or a fight for resources because you dont want players to play a parallel match where they just gather resources and then go into a big, decisive fight. The location is also important for the fact that the players will build bases around resource locations.

### Starting points

This needs to be well-defined because it's where the players will start on the map. It is extremely important that belligerent factions are not close to each other when starting. You should locate this points strategically so that the players will have a delayed encounter(in the ideal situation) when they are expanding their bases. They should allow the player to move to whatever location he desires, so it shouldn't block possible paths that both players can take.

### Base location

An important property of locations is the amount of resources they contain. To be able to create balance in our maps, we should distribute our resources equally for each player. This doesn't mean that the enemy can't take this points of interest from you, even though it was designed that you should have it. A base location should be equidistant from all near resources. 

The base layout is extremely important that they are close physically in order to counter [Medivac Drops](https://www.youtube.com/watch?v=YFm4MOPN3ME) for the Terran faction.

### Map symmetry

This is the absolute basic concept for balancing a map. Make sure that players are able to achieve the same thing and collect the same amount of resources. If a map lack this property, most likely, it will be deemed as extremely unbalanced and unfair.

Often, map creators will the the artifical look that a symmetrical map produces by changing game elements that don't have any impact on gameplay whatsoever. For example, changing the textures throughout the map so that the player feels in a more natural environment. This practice is called *functional symmetry*.

<p align="center">
  <img  src="https://raw.githubusercontent.com/Scarzard/RTS_Balancing/master/docs/Web%20Images/Circuit_Breakers.jpg">
</p>

*Competitive map: Circuit Breaker. Often tagged as the most balanced map of all time*

You can also see competitive maps that are currently being used [here](https://liquipedia.net/starcraft2/Maps/Competition_Maps).

## 10. Conclusions and personal thoughts on the topic

The methods explained here are for using them as a guideline, they're not definitive. They will focus your game in the right direction while establishing a balanced base to build your own system around.

One of the big points while designing your system is to playtest as much as posible, trial and error is one of the best ways to find and discern where are the points that require changing as well as getting feedback from real players, we all know what happens if only the creators test their own videogame...

This theme was very interesting for me to research about, i have played some RTS games but i'm not what you woould call a fanatic of the genre, i know how they work and i can defend myself in a game but that's it, almost everything i learned doing this work was new to me.

That's why while researching this topic I focused on making sure that the ones reading it would understand it perfectly, which was on its own way another challenge because of the extensive that this theme is, the deeper you dig into this topic, the lower you fall into a rabbit hole, is like entering an infinite dungeon and wanting to explore all the rooms.

## 11. Bibliography, Webgraphy and Videography

### Links

#### Balancing
- **Game balance:** <https://en.wikipedia.org/wiki/Game_balance>

- **Introduction to unit balancing:** <http://www.oxeyegames.com/rts-game-play-part-5-introduction-to-unit-balancing/>

- **Intransitive mechanics:** <https://gamebalanceconcepts.wordpress.com/2010/09/01/level-9-intransitive-mechanics/>

- **Pay-off table definition:** <http://kfknowledgebank.kaplan.co.uk/KFKB/Wiki%20Pages/Payoff%20tables.aspx>

- **Unit balancing discussion in forums:** <https://www.gamedev.net/forums/topic/490139-balancing-units-in-a-rts/>

<https://forum.dune2k.com/topic/24505-rts-is-it-even-possible-to-balance/>

- **Strategies employed in RTS:** <https://www.abc.net.au/goodgamesp/episodes/5-up-top-rts-strategies/10405478>

  - **Rushing definition:** <https://en.wikipedia.org/wiki/Rush_(video_gaming)>


#### Resource/economy management 

- **Resource systems:** <http://www.oxeyegames.com/rts-game-play-part-2-resource-systems/>

- **Resource management in RTS:** <https://waywardstrategy.com/2014/12/18/random-thoughts-on-resource-management-in-rts/>

- **Control of the economic processes:** <https://waywardstrategy.com/2015/11/23/rts-design-thought-control-of-economic-processes/>

#### Tech trees

- **Technology tree:** <https://en.wikipedia.org/wiki/Technology_tree>

- **Technology Trees: Freedom and Determinism in Historical Strategy Game by Tuur Ghys:** <http://www.gamestudies.org/1201/articles/tuur_ghys/>



#### Single player campaigns and map design

- **Level design for single player campaigns:** <http://www.ultimaratioregum.co.uk/game/tag/rts/>

- **Competitive map pool for StarCraft II:** <https://liquipedia.net/starcraft2/Maps/Competition_Maps>


### Abstracts/PDFs

- ***Really extensive* article about Balancing Real-Time Strategy Games:** <https://brage.bibsys.no/xmlui/bitstream/handle/11250/2463289/13662_FULLTEXT.pdf?sequence=1&isAllowed=y>

- **Call for AI Research in RTS Games:** <https://skatgame.net/mburo/ps/RTS-AAAI04.pdf>

- **Balanced map generation for SC:** <http://nova.wolfwork.com/papers/PSMAGE_Balanced_Map_Generation_Starcraft.pdf>


### Videography
- **Perfect Imbalance by ExtraCredits:** <https://www.youtube.com/watch?v=e31OSVZF77w/>

- **What Makes Good AI? by Game Maker's Toolkit:** <https://youtu.be/9bbhJi0NBkk/>

- **What Makes RTS Games Fun: Asymmetric Design in RTS by GeneralsGentleman:** <https://youtu.be/F1w-qCbYVe8/>

