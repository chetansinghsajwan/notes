https://maxroll.gg/d4/getting-started/damage-for-beginners
  
Dealing damage in Diablo 4 is roughly half the game. Some would even say it's more than that, after all, the demons won't kill themselves. You need to do it by bringing their Life to 0, and there's no better way to do it than damage! In this article, we describe how damage is calculated and the many different ways to deal _**more**_ damage. For a more advanced guide, consider reading our [In-depth damage guide](https://maxroll.gg/d4/resources/in-depth-damage-guide)!
## Damage Base
We need to start our damage calculation somewhere, and depending on the source of damage, there are two ways to do that.
### Weapon Damage
Skills and passives scale their damage based on your weapon. With the advanced skill tooltip (_a setting you need to check in the menu_), you can see a percentage next to the damage number. Multiply that percentage by the damage value on your weapon, and you get the damage that you deal with that skill. If you use multiple weapons, the damage from both weapons is added together.
[![](https://assets.maxroll.gg/wordpress/weapon_base.png)](https://assets.maxroll.gg/wordpress/weapon_base.png)
Weapon Base
Here, skill% is the number you see in your skill description with advanced tooltips enabled. It depends on the skill used and the number of ranks you have invested into it. DMGweapon is dependent on weapon type and its [item power](https://maxroll.gg/d4/resources/equipment). Note that weapons have a damage range instead of a single damage number; for these calculations, you can take anything: minimum, maximum, or average damage. The math stays the same.
### Flat Damage
Some effects, such as Thorns, deal damage that doesn't depend on your weapon. Instead, they have a flat value which is used as the base for the damage calculation. As of Season 4, procs from [Legendary Aspects](https://maxroll.gg/d4/wiki/legendary-aspects) (e.g. Arrow Storms or Trickster's) now scale with your weapon damage. That means that to calculate the damage dealt by these effects, you replace DMGbase with the flat damage value from these effects.
[![](https://assets.maxroll.gg/wordpress/thorns_base.png)](https://assets.maxroll.gg/wordpress/thorns_base.png)
Thorns Base
or
[![](https://assets.maxroll.gg/wordpress/proc_base.png)](https://assets.maxroll.gg/wordpress/proc_base.png)
Proc Base
Similar to DMGweapon, both Thorns and Legendary Aspect procs scale with item power.
## Damage Bonuses
Next, we have a number of various damage bonuses coming from our items, Glyphs, and paragon nodes. One thing all of these have in common is that there's **no "[x]"** before the percentage. Instead, these bonuses have a [+] in front of them, meaning all these damage bonuses are **additive**.
The following list contains some of the damage bonuses belonging to this additive category:
- Damage (including Damage After Killing an Elite)
- Skill Damage (Core/Basic/Blood/Cutthroat/Companion etc.)
- Physical/Fire/Cold/Shadow/Lightning/Poison Damage
- Damage to Close/Distant/Crowd Controlled etc Enemies
- Damage to Bleeding/Poisoned/Burning etc. Enemies
- Damage to Elites
- Damage to Healthy/Injured Enemies
- Damage while Healthy/Fortified
- Damage while Berserking
- Critical Strike Damage (with Lightning/Bone/Earth etc.)
- Vulnerable Damage
- Overpower Damage
[![](https://assets.maxroll.gg/wordpress/additive-bonuses.png)](https://assets.maxroll.gg/wordpress/additive-bonuses.png)
Additive Bonuses
The important takeaway is that all these bonuses don't provide as much benefit as you would think at a glance. An item saying [+]_20% Core Skill Damage_ gives you less than a 20% damage increase, even if you deal all your damage with a Core skill, and the more you stack these bonuses, the less effective they become. This becomes especially noticeable in the late game when you get tons of additive damage bonuses from your Paragon Boards and Glyphs.
## Separate Multipliers
Many bonuses, however, have a slightly different wording. They say: _"[x]30% increased damage if this and that condition is met"_. This means that the bonus in question is multiplicative. You get exactly what it says on the tin. [x]30% means 30% bigger number on your screen when the condition is fulfilled.
There's exactly one bonus that acts as a separate multiplier without having an "x" before its value. It is the Skill Damage bonus you get from your main Core Stat.
After adding multiplicative bonuses, our damage formula becomes:
[![](https://assets.maxroll.gg/wordpress/mult-bonuses.png)](https://assets.maxroll.gg/wordpress/mult-bonuses.png)
Mult Bonuses
**Important:** some Passives and Legendary Aspects that provide multiplicative bonuses also show up in your character stats. For example, the Offensive Aspect of Inner Calm increases your All Damage stat by 30% multiplicatively. This is **incorrect** and **misleading**! Your All Damage gained from items and paragons is still additive, and your Aspect provides a 30% increase to your total damage, including **all** additive bonuses and not just All Damage.
- Let's say you play a Barbarian and use Hammer of the Ancients. Without any bonuses, it deals 100 damage. Then we add the following stats:
    
	All these bonuses are additive, so now our skill deals:
    
	`100 * (1 + 100% + 40% + 150%) = 390` damage
    
	Next, we add the Unbridled Rage Key Passive. It shows up in our stats as a multiplicative bonus to Core Skill Damage:
    
	**Core Skill Damage**: 229% [(1+40%) * (1+135%) - 1]
    
	This would lead us to believe that our Hammer of the Ancients now deals:
    
	`100 * (1 + 100% + 229% + 150%) = 579` damage
    
	However, that's not what happens in reality. In reality, Unbridled Rage is a separate multiplier and we deal:
    
	`100 * (1 + 100% + 40% + 150%) * (1 + 135%) = 916.5` damage
    
	If we don't use the passive and instead just get 189% more Core Skill Damage from our gear and Paragon, our stats will be exactly the same, but the damage will be different.
    
### Main Stat
There are 4 different types of Core Stats in Diablo 4, and these stats provide the classes with something called Skill Damage. These core stats are Dexterity, Intelligence, Strength, and Willpower. Each class has one core stat assigned as their Main Core Stat, Rogues and Spiritborns use Dexterity, Necromancers and Sorcerers use Intelligence, Barbarians use Strength, and Druids use Willpower. Each class has a different scaling per point of their Core Stat, and their scaling is listed below.
- Druid, Necromancer, Sorcerer, and Spiritborn: 1% Skill Damage per 8 Main Stat
- Rogue: 1% Skill Damage per 9 Dexterity
- Barbarian: 1% Skill Damage per 10 Strength.
[![](https://assets.maxroll.gg/wordpress/vulnerable.jpg)](https://assets.maxroll.gg/wordpress/vulnerable.jpg)
### Vulnerable
Some skills can make enemies Vulnerable (their Life bar turns purple, or if you are affected, your Health Orb gets cracks). In this state, enemies take 20% more damage from all sources. This 20% is a separate multiplier, the rest of "Vulnerable Damage" bonuses are additive damage that only becomes active against Vulnerable targets, similar to how "Damage vs Burning Enemies" only works when enemies are on fire.
[![](https://assets.maxroll.gg/wordpress/crit.jpg)](https://assets.maxroll.gg/wordpress/crit.jpg)
### Critical Strikes
All damage can be divided into direct damage and Damage over Time (Burning, Bleeding, Poisoning). Direct damage happens instantly, while damage over time is spread over a set duration. We didn't mention this distinction earlier because, in terms of numbers and buffs, direct damage and DoTs behave exactly the same until we get to the topic of Critical Strikes. Only direct damage can Critically Strike, Damage over Time cannot.
Each hit has a random chance to be a Critical Strike. This chance is equal to your Critical Strike Chance (CSC) stat, which can be increased in many different ways: You get 5% baseline, 0.02% for each point of Dexterity or Intelligence depending on class, Affix rolls on Gloves, Rings, and Amulets, and many skills and passives.
When you trigger a Critical Strike, your damage is multiplied by x1.5 and all your Critical Strike Damage bonuses are added to the pile.
In other words, Critical Strikes simply add one more multiplier to the list of your multiplicative [x] bonuses and a bunch more (depending on how much Crit Damage you stack) to your total sum of additive damage.
Critical Strike Damage is no different from any other type of additive damage, and you can very easily compare them. Whatever has biggest value multiplied by uptime is better (as a bonus, for CSC you always know the exact uptime - your Crit Chance). To evaluate the effectiveness of Crit Chance, you have to look at your Crit Chance, Crit Damage, Total Additive Damage, and separate Multipliers that have crit as a condition (e.g. Heavy Handed or Aspect of Three Curses). As a rule of thumb, a single 7.5% roll of Crit Chance translates to 3.5% damage increase, which is slightly better than competing 20-30% additive damage rolls. With more Crit Damage and Crit Multiplies, Crit Chance pulls even further ahead.
[![](https://assets.maxroll.gg/wordpress/overpower.jpg)](https://assets.maxroll.gg/wordpress/overpower.jpg)
Damage Diablo 4
### Overpower
On top of Critical Strikes, there's another mechanic that lets you randomly get a 1.5x separate multiplier. There are several key differences between Overpower and Critical Strike. Chance to Overpower is not a stat, it's just a fixed 3% chance with no way to increase it. Instead, some skills have guaranteed Overpower when conditions are met.
Not related to the damage calculation, but there's one more difference between Overpower and Crits. Crits happen per target hit while Overpower happens per attack. This means that one attack hitting multiple enemies or multiple times will always Overpower (if you proc that 3% chance when you use it). As an unfortunate side effect of this system, Channeled Skills and DoTs are unable to Overpower.
## Weapon Speed
One more stat that doesn't directly contribute to your damage, but does affect your Damage Per Second (DPS) is Weapon Speed. Your Weapon Speed indicates how often you can cast your skills (although some skills have fixed animation length, not dependent on Weapon Speed). To calculate your Weapon Speed, you need to consider two parameters: your weapon's Attacks per Second (APS), which is averaged when dual-wielding, and your Attack Speed bonus. Attack Speed can roll as a stat on your Gloves and also comes from different skills, passives, Paragon nodes, and Legendary Aspects. Attack speed has two separate 100% caps. Some bonuses, such as rolls on gear, potion, and certain Aspects count towards the first cap, while Attack Speed you get from skills and skill-specific Attack Speed found on certain Uniques and tempers go into the second cap. All these bonuses are added together and act as a multiplier to your weapon's Attacks per Second:
[![](https://assets-ng.maxroll.gg/wordpress/ApSFormula.webp)](https://assets-ng.maxroll.gg/wordpress/ApSFormula.webp)
Ap S Formula
Now, for direct damage skills, you can simplify the concept to the following DPS formula:
[![](https://assets-ng.maxroll.gg/wordpress/dmgbeginners_APSDPS.png)](https://assets-ng.maxroll.gg/wordpress/dmgbeginners_APSDPS.png)
Dmgbeginners APSDPS
More or less, the same applies to DoT skills that can stack, except the actual damage is delayed by the DoT's duration. For non-stacking DoTs, Weapon Speed doesn't affect damage at all. See our [attack speed article](https://maxroll.gg/d4/resources/attack-speed-mechanics) for information regarding attack speed caps and more.
## Attack Power
The game offers you an approximation of your DPS called Attack Power, which you can see in your inventory window. However, you shouldn't rely on this number except as a very rough guideline. It doesn't include a lot of damage bonuses, specifically those that apply only to certain skills or against certain enemies. It also doesn't include any debuffs you can have on monsters, such as Vulnerable. On the other hand, it includes your Weapon Speed and Critical Strike stats, which can be very misleading if your damage comes from a DoT or a skill with fixed animation length.
## Summary
To get the Big Numbertm, you need to get yourself a good weapon, a powerful skill, and stack a lot of different damage bonuses, preferably those that have "x" in front of the percentage. Then you need to hit something over and over until you proc both Critical Strike and Overpower with the same attack. At that point, you only need to take a screenshot and post it online. Everyone will be impressed.
### Credits
Written by **Northwar**.
Reviewed by **wudijo, snail, Avarilyn**