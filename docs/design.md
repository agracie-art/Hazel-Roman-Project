# Game design (from the designer's notes and sketches)

## Idea
You are a person with a base. You run into the big area full of eggs and take one. The treadmill in your base
gives you speed. Heavier eggs slow you down, and the best eggs are at the back, so you need enough speed to
carry them. A big Roman soldier in the arena swings his sword and kicks you back to your base, and you lose
the egg. Then you go again and pick a lighter egg you can actually carry.

Capture an egg, place it in your base, open it, and see which Roman you got. The Roman tells you a fact about
that type of Roman. Keep it if you like it, or sell it for money.

## Setting and look
- 3D but pixelated, landscape orientation
- Looks like the Roblox game it is inspired by
- Roman theme. Base is a Roman temple. The egg area is the Colosseum
- No lions in the Colosseum. Only the soldier and the eggs. Gladiator and Lion Tamer are Romans you can hatch

## Screen layout from the sketch ("Safe zone")
Money, Facts, Base upgrades, Speed (starts at 120), Your base, Selling shop, Treadmill, You, and a large
play area with eggs and a soldier holding a "Shop up" sign. Prototype currently has money, speed and Facts.

## Cards (rarity ranks Common, Uncommon, Rare, Epic, Legendary, Mythic, Secret)
| Name | Rank | Fact |
|---|---|---|
| Peasant | Common | Britain used to be called Britannia. |
| Slave | Common | Most slaves were people from places Rome had invaded. |
| Soldier | Uncommon | If you were a strong person from a country Rome had invaded, you had to join the Roman army. |
| Gladiator | Uncommon | The gladiators used so many lions that there are no more lions in North Africa today! |
| Tax Collector | Rare | When Rome got bigger, the rich got richer but the poor Romans got poorer. |
| Celt | Rare | Some Celts painted their faces with berries. |
| Lion Tamer | Epic | Gladiators fought lions to become Roman citizens from the emperor. |
| Governor | Epic | A governor was the best job you could get apart from the emperor. |
| Julius Caesar | Legendary | Julius Caesar was the first emperor to have his face put on coins. |
| Emperor Nero | Legendary | Emperor Nero nearly lost Britain when he was emperor. |
| Boudicca | Mythic | When Prasutagus died, the Romans kicked Boudicca out of her house. |
| Prasutagus | Mythic | He was allies with the Romans, but when he died they kicked Boudicca out of her house. |
| Jupiter | Secret | He was the most powerful god. |
| Juno | Secret | She was Jupiter's wife and she was the most powerful goddess. |

Fact-check note for a possible later pass: Julius Caesar was a dictator, not an emperor (Augustus was the first
emperor). Kept as the designer wrote it.

## Prototype assumptions (made up, change freely)
Egg tier equals card rarity. Speed needed = 100 + 30 * tier. Sell price = 5 * rarity value (1, 3, 8, 20, 50,
120, 300). Soldier chases only when you carry an egg.
