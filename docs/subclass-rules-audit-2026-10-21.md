# Subclass rules audit

Audit of every subclass shipped in the app (117) against the published 5e rules: which features, levels, numbers and spell lists are right, which are wrong, and which are missing.

> **Read this first.** The 'rules' side of this audit comes from my own knowledge of the published books, not from the books themselves (only the SRD 5.1 is in the repository). Every finding carries a confidence level. **High** means I am sure of the rule. **Medium** means I am fairly sure but a source check is worth doing before changing data. **Low** means I could not confirm it from memory and the entry only flags the text as suspect. Nothing in this file was run in the app; it is an audit of the data files and a name-search of the engine code.

## Contents

- Scope and method
- Legend
- Summary
- Cross-cutting findings
- Artificer
- Barbarian
- Bard
- Cleric
- Druid
- Fighter
- Monk
- Paladin
- Ranger
- Rogue
- Sorcerer
- Warlock
- Wizard
- Appendix A: Warlock level 1 to 20 progression
- Appendix B: Engine wiring hints
- Appendix C: Official subclasses not in the app

## Scope and method

- **Data audited.** `src/lib/srd/subclasses.json` (105 subclasses) and the SRD subclass in `src/lib/srd/class-features.json` for each class (12; Artificer has none there). The SRD subclasses in `class-features.json` carry feature names and levels only, with no description text.
- **Checked for each subclass:** the level each feature arrives at, whether any feature is missing, the numbers and conditions in each feature's text, and the subclass spell list (always-prepared, expanded or fixed).
- **Not checked:** the item, spell and monster stat blocks the features refer to, the subclass's class-level interactions in the builder (for example how many invocations or maneuvers get picked), and 2024 Player's Handbook revisions. Where the app text follows an older printing I note it.
- **Engine hints.** Appendix B lists features whose exact name appears nowhere in the engine code (`src/lib/dm`, resource, effect and trait tables). A missing name means the feature is probably only shown on the sheet and not applied by the server. It is a heuristic only: a passive feature can legitimately need no code, and a feature can be applied under another name.

## Legend

| Mark | Meaning |
|---|---|
| 🔴 Wrong | The app text or data contradicts the rules. |
| 🟠 Missing | A feature, option or spell that the rules give is absent. |
| 🟡 Imprecise | Roughly right, but a number, limit, prerequisite or choice is left out or loosely worded. |
| Confidence | High / Medium / Low, as described above. |

## Summary

**117 subclasses audited. 91 findings: 26 wrong, 11 missing, 54 imprecise.**

60 subclasses had no discrepancy that I could find within the limits above (a clean entry means I did not spot a problem, not that it was proven correct); the other 57 have at least one finding.

| Class | Subclasses | 🔴 Wrong | 🟠 Missing | 🟡 Imprecise | Subclasses with no finding |
|---|---|---|---|---|---|
| Artificer | 4 | 1 | 1 | 7 | 0 |
| Barbarian | 8 | 5 | 2 | 8 | 0 |
| Bard | 8 | 2 | 1 | 2 | 4 |
| Cleric | 14 | 2 | 0 | 2 | 10 |
| Druid | 7 | 2 | 2 | 4 | 2 |
| Fighter | 10 | 1 | 1 | 4 | 4 |
| Monk | 10 | 1 | 1 | 3 | 6 |
| Paladin | 9 | 0 | 0 | 1 | 8 |
| Ranger | 8 | 1 | 1 | 1 | 5 |
| Rogue | 9 | 0 | 0 | 2 | 7 |
| Sorcerer | 8 | 1 | 0 | 2 | 5 |
| Warlock | 9 | 8 | 2 | 13 | 1 |
| Wizard | 13 | 2 | 0 | 5 | 8 |

### Verdict by subclass

| Class | Subclass | Source | Verdict |
|---|---|---|---|
| Artificer | [Alchemist](#artificer-alchemist) | Tasha's Cauldron of Everything | 🔴 1 🟡 3 |
| Artificer | [Armorer](#artificer-armorer) | Tasha's Cauldron of Everything | 🟡 2 |
| Artificer | [Artillerist](#artificer-artillerist) | Tasha's Cauldron of Everything | 🟡 1 |
| Artificer | [Battle Smith](#artificer-battle-smith) | Tasha's Cauldron of Everything | 🟠 1 🟡 1 |
| Barbarian | [Path of the Berserker](#barbarian-path-of-the-berserker) | SRD 5.1 / PHB | 🟡 1 |
| Barbarian | [Path of the Totem Warrior](#barbarian-path-of-the-totem-warrior) | PHB (Elk and Tiger options: Xanathar's) | 🔴 1 🟠 1 |
| Barbarian | [Path of the Ancestral Guardian](#barbarian-path-of-the-ancestral-guardian) | Xanathar's Guide to Everything | 🟡 1 |
| Barbarian | [Path of the Storm Herald](#barbarian-path-of-the-storm-herald) | PHB | 🔴 1 🟡 1 |
| Barbarian | [Path of the Zealot](#barbarian-path-of-the-zealot) | Xanathar's Guide to Everything | 🟡 2 |
| Barbarian | [Path of the Beast](#barbarian-path-of-the-beast) | Tasha's Cauldron of Everything | 🟡 1 |
| Barbarian | [Path of Wild Magic](#barbarian-path-of-wild-magic) | Tasha's Cauldron of Everything | 🟡 2 |
| Barbarian | [Path of the Giant](#barbarian-path-of-the-giant) | Bigby Presents: Glory of the Giants | 🔴 3 🟠 1 |
| Bard | [College of Lore](#bard-college-of-lore) | SRD 5.1 / PHB | 🟡 1 |
| Bard | [College of Valor](#bard-college-of-valor) | PHB | No discrepancy found |
| Bard | [College of Glamour](#bard-college-of-glamour) | Xanathar's Guide to Everything | 🔴 1 🟡 1 |
| Bard | [College of Swords](#bard-college-of-swords) | Xanathar's Guide to Everything | No discrepancy found |
| Bard | [College of Whispers](#bard-college-of-whispers) | Xanathar's Guide to Everything | No discrepancy found |
| Bard | [College of Eloquence](#bard-college-of-eloquence) | Mythic Odysseys of Theros / Tasha's | 🟠 1 |
| Bard | [College of Creation](#bard-college-of-creation) | Tasha's Cauldron of Everything | No discrepancy found |
| Bard | [College of Spirits](#bard-college-of-spirits) | Van Richten's Guide to Ravenloft | 🔴 1 |
| Cleric | [Life Domain](#cleric-life-domain) | SRD 5.1 / PHB | 🟡 1 |
| Cleric | [Knowledge Domain](#cleric-knowledge-domain) | PHB | No discrepancy found |
| Cleric | [Light Domain](#cleric-light-domain) | PHB | No discrepancy found |
| Cleric | [Nature Domain](#cleric-nature-domain) | PHB | No discrepancy found |
| Cleric | [Tempest Domain](#cleric-tempest-domain) | PHB | No discrepancy found |
| Cleric | [Trickery Domain](#cleric-trickery-domain) | PHB | No discrepancy found |
| Cleric | [War Domain](#cleric-war-domain) | PHB | No discrepancy found |
| Cleric | [Death Domain](#cleric-death-domain) | Dungeon Master's Guide | 🔴 1 |
| Cleric | [Forge Domain](#cleric-forge-domain) | Xanathar's Guide to Everything | No discrepancy found |
| Cleric | [Grave Domain](#cleric-grave-domain) | Xanathar's Guide to Everything | 🔴 1 |
| Cleric | [Order Domain](#cleric-order-domain) | Tasha's Cauldron of Everything | 🟡 1 |
| Cleric | [Peace Domain](#cleric-peace-domain) | Tasha's Cauldron of Everything | No discrepancy found |
| Cleric | [Twilight Domain](#cleric-twilight-domain) | Tasha's Cauldron of Everything | No discrepancy found |
| Cleric | [Arcana Domain](#cleric-arcana-domain) | Sword Coast Adventurer's Guide | No discrepancy found |
| Druid | [Circle of the Land](#druid-circle-of-the-land) | SRD 5.1 / PHB | 🟠 1 🟡 1 |
| Druid | [Circle of the Moon](#druid-circle-of-the-moon) | PHB | 🟠 1 |
| Druid | [Circle of Dreams](#druid-circle-of-dreams) | Xanathar's Guide to Everything | No discrepancy found |
| Druid | [Circle of the Shepherd](#druid-circle-of-the-shepherd) | Xanathar's Guide to Everything | 🟡 1 |
| Druid | [Circle of Spores](#druid-circle-of-spores) | Guildmasters' Guide to Ravnica | 🟡 2 |
| Druid | [Circle of Stars](#druid-circle-of-stars) | Tasha's Cauldron of Everything | No discrepancy found |
| Druid | [Circle of Wildfire](#druid-circle-of-wildfire) | Tasha's Cauldron of Everything | 🔴 2 |
| Fighter | [Champion](#fighter-champion) | SRD 5.1 / PHB | 🟡 1 |
| Fighter | [Battle Master](#fighter-battle-master) | PHB | 🟡 1 |
| Fighter | [Eldritch Knight](#fighter-eldritch-knight) | PHB | No discrepancy found |
| Fighter | [Arcane Archer](#fighter-arcane-archer) | Xanathar's Guide to Everything | 🟠 1 |
| Fighter | [Cavalier](#fighter-cavalier) | Xanathar's Guide to Everything | 🟡 1 |
| Fighter | [Samurai](#fighter-samurai) | Xanathar's Guide to Everything | 🔴 1 |
| Fighter | [Echo Knight](#fighter-echo-knight) | Explorer's Guide to Wildemount | No discrepancy found |
| Fighter | [Psi Warrior](#fighter-psi-warrior) | Tasha's Cauldron of Everything | No discrepancy found |
| Fighter | [Rune Knight](#fighter-rune-knight) | Tasha's Cauldron of Everything | No discrepancy found |
| Fighter | [Banneret](#fighter-banneret) | Sword Coast Adventurer's Guide (Purple Dragon Knight) | 🟡 1 |
| Monk | [Way of the Open Hand](#monk-way-of-the-open-hand) | SRD 5.1 / PHB | 🟡 1 |
| Monk | [Way of Shadow](#monk-way-of-shadow) | PHB | No discrepancy found |
| Monk | [Way of the Four Elements](#monk-way-of-the-four-elements) | PHB | No discrepancy found |
| Monk | [Way of the Drunken Master](#monk-way-of-the-drunken-master) | Xanathar's Guide to Everything | No discrepancy found |
| Monk | [Way of the Kensei](#monk-way-of-the-kensei) | Xanathar's Guide to Everything | 🔴 1 🟠 1 |
| Monk | [Way of the Sun Soul](#monk-way-of-the-sun-soul) | Xanathar's Guide to Everything | No discrepancy found |
| Monk | [Way of the Long Death](#monk-way-of-the-long-death) | Sword Coast Adventurer's Guide | No discrepancy found |
| Monk | [Way of Mercy](#monk-way-of-mercy) | Tasha's Cauldron of Everything | 🟡 1 |
| Monk | [Way of the Astral Self](#monk-way-of-the-astral-self) | Tasha's Cauldron of Everything | 🟡 1 |
| Monk | [Way of the Ascendant Dragon](#monk-way-of-the-ascendant-dragon) | Fizban's Treasury of Dragons | No discrepancy found |
| Paladin | [Oath of Devotion](#paladin-oath-of-devotion) | SRD 5.1 / PHB | 🟡 1 |
| Paladin | [Oath of the Ancients](#paladin-oath-of-the-ancients) | PHB | No discrepancy found |
| Paladin | [Oath of Vengeance](#paladin-oath-of-vengeance) | PHB | No discrepancy found |
| Paladin | [Oath of Conquest](#paladin-oath-of-conquest) | Xanathar's Guide to Everything | No discrepancy found |
| Paladin | [Oath of Redemption](#paladin-oath-of-redemption) | Xanathar's Guide to Everything | No discrepancy found |
| Paladin | [Oath of Glory](#paladin-oath-of-glory) | Mythic Odysseys of Theros / Tasha's | No discrepancy found |
| Paladin | [Oath of the Watchers](#paladin-oath-of-the-watchers) | Tasha's Cauldron of Everything | No discrepancy found |
| Paladin | [Oath of the Crown](#paladin-oath-of-the-crown) | Sword Coast Adventurer's Guide | No discrepancy found |
| Paladin | [Oathbreaker](#paladin-oathbreaker) | Dungeon Master's Guide | No discrepancy found |
| Ranger | [Hunter](#ranger-hunter) | SRD 5.1 / PHB | 🟡 1 |
| Ranger | [Beast Master](#ranger-beast-master) | PHB | 🟠 1 |
| Ranger | [Gloom Stalker](#ranger-gloom-stalker) | Xanathar's Guide to Everything | No discrepancy found |
| Ranger | [Horizon Walker](#ranger-horizon-walker) | Xanathar's Guide to Everything | No discrepancy found |
| Ranger | [Monster Slayer](#ranger-monster-slayer) | Xanathar's Guide to Everything | 🔴 1 |
| Ranger | [Fey Wanderer](#ranger-fey-wanderer) | Tasha's Cauldron of Everything | No discrepancy found |
| Ranger | [Swarmkeeper](#ranger-swarmkeeper) | Tasha's Cauldron of Everything | No discrepancy found |
| Ranger | [Drakewarden](#ranger-drakewarden) | Fizban's Treasury of Dragons | No discrepancy found |
| Rogue | [Thief](#rogue-thief) | SRD 5.1 / PHB | 🟡 1 |
| Rogue | [Assassin](#rogue-assassin) | PHB | No discrepancy found |
| Rogue | [Arcane Trickster](#rogue-arcane-trickster) | PHB | No discrepancy found |
| Rogue | [Mastermind](#rogue-mastermind) | Xanathar's Guide to Everything | No discrepancy found |
| Rogue | [Swashbuckler](#rogue-swashbuckler) | Xanathar's Guide to Everything | No discrepancy found |
| Rogue | [Inquisitive](#rogue-inquisitive) | Xanathar's Guide to Everything | No discrepancy found |
| Rogue | [Scout](#rogue-scout) | Xanathar's Guide to Everything | No discrepancy found |
| Rogue | [Phantom](#rogue-phantom) | Tasha's Cauldron of Everything | 🟡 1 |
| Rogue | [Soulknife](#rogue-soulknife) | Tasha's Cauldron of Everything | No discrepancy found |
| Sorcerer | [Draconic Bloodline](#sorcerer-draconic-bloodline) | SRD 5.1 / PHB | 🟡 1 |
| Sorcerer | [Wild Magic](#sorcerer-wild-magic) | PHB | No discrepancy found |
| Sorcerer | [Divine Soul](#sorcerer-divine-soul) | Xanathar's Guide to Everything | No discrepancy found |
| Sorcerer | [Shadow Magic](#sorcerer-shadow-magic) | Xanathar's Guide to Everything | No discrepancy found |
| Sorcerer | [Storm Sorcery](#sorcerer-storm-sorcery) | Xanathar's Guide to Everything | 🔴 1 |
| Sorcerer | [Aberrant Mind](#sorcerer-aberrant-mind) | Tasha's Cauldron of Everything | No discrepancy found |
| Sorcerer | [Clockwork Soul](#sorcerer-clockwork-soul) | Tasha's Cauldron of Everything | No discrepancy found |
| Sorcerer | [Lunar Sorcery](#sorcerer-lunar-sorcery) | Dragonlance: Shadow of the Dragon Queen | 🟡 1 |
| Warlock | [The Fiend](#warlock-the-fiend) | SRD 5.1 / PHB | 🟠 1 🟡 2 |
| Warlock | [The Archfey](#warlock-the-archfey) | PHB | 🔴 1 🟡 1 |
| Warlock | [The Great Old One](#warlock-the-great-old-one) | PHB | No discrepancy found |
| Warlock | [The Celestial](#warlock-the-celestial) | Xanathar's Guide to Everything | 🟡 2 |
| Warlock | [The Hexblade](#warlock-the-hexblade) | Xanathar's Guide to Everything | 🟡 6 |
| Warlock | [The Fathomless](#warlock-the-fathomless) | Tasha's Cauldron of Everything | 🔴 3 🟡 1 |
| Warlock | [The Genie](#warlock-the-genie) | Tasha's Cauldron of Everything | 🔴 1 🟠 1 |
| Warlock | [The Undead](#warlock-the-undead) | Van Richten's Guide to Ravenloft | 🔴 2 🟡 1 |
| Warlock | [The Undying](#warlock-the-undying) | Sword Coast Adventurer's Guide | 🔴 1 |
| Wizard | [School of Evocation](#wizard-school-of-evocation) | SRD 5.1 / PHB | 🟡 1 |
| Wizard | [School of Abjuration](#wizard-school-of-abjuration) | PHB | No discrepancy found |
| Wizard | [School of Conjuration](#wizard-school-of-conjuration) | PHB | No discrepancy found |
| Wizard | [School of Divination](#wizard-school-of-divination) | PHB | 🟡 1 |
| Wizard | [School of Enchantment](#wizard-school-of-enchantment) | PHB | 🔴 2 |
| Wizard | [School of Illusion](#wizard-school-of-illusion) | PHB | 🟡 1 |
| Wizard | [School of Necromancy](#wizard-school-of-necromancy) | PHB | No discrepancy found |
| Wizard | [School of Transmutation](#wizard-school-of-transmutation) | PHB | No discrepancy found |
| Wizard | [War Magic](#wizard-war-magic) | Xanathar's Guide to Everything | No discrepancy found |
| Wizard | [Bladesinging](#wizard-bladesinging) | Sword Coast Adventurer's Guide / Tasha's | No discrepancy found |
| Wizard | [Chronurgy Magic](#wizard-chronurgy-magic) | Explorer's Guide to Wildemount | No discrepancy found |
| Wizard | [Graviturgy Magic](#wizard-graviturgy-magic) | Explorer's Guide to Wildemount | No discrepancy found |
| Wizard | [Order of Scribes](#wizard-order-of-scribes) | Tasha's Cauldron of Everything | 🟡 2 |

### Most serious findings (wrong, high confidence)

| Class | Subclass | Where | Problem |
|---|---|---|---|
| Artificer | Alchemist | L9 Restorative Reagents | The number of free casts is your Intelligence modifier (minimum 1), not your proficiency bonus. The casts also require alchemist's supplies as the spellcasting focus. Elixirs also grant temporary hit points of 2d6 + Intelligence m… |
| Barbarian | Path of the Totem Warrior | L3 Totem Spirit: Eagle | While raging and not in heavy armor, other creatures have DISADVANTAGE on opportunity attack rolls against you, and you can Dash as a bonus action. The app text says 'no advantage', which is a different rule. |
| Bard | College of Glamour | L3 Enthralling Performance | The number is equal to your Charisma modifier (minimum 1), not your proficiency bonus. |
| Fighter | Samurai | L18 Strength before Death | When damage reduces you to 0 hit points, you use your reaction to delay falling unconscious and immediately take an extra turn. During it, damage causes death save failures and you fall unconscious at the end of the turn if still … |
| Monk | Way of the Kensei | L3 Path of the Kensei | Agile Parry: when you make an unarmed strike as part of the Attack action while wielding a kensei weapon, you gain +2 AC until the start of your next turn. It has nothing to do with ranged weapons or a bonus action. The bonus-acti… |
| Sorcerer | Storm Sorcery | Storm Guide level | Storm Guide is a 6th-level feature (with Heart of the Storm). 14th has Storm's Fury only. |
| Warlock | The Archfey | L10 Beguiling Defenses | The reaction forces a Wisdom save; on a failure the creature is charmed by you for 1 minute or until it takes damage. There is no 3d10 psychic damage. |
| Warlock | The Fathomless | L6 Oceanic Soul | Resistance to cold damage, not immunity. You can also communicate telepathically with any creature in the water within 120 ft that speaks a language. |
| Warlock | The Undying | L1 Among the Dead | If an undead targets you directly with an attack or a harmful spell it makes a Wisdom save; on a failure it must choose a new target or lose the attack or spell. It does not protect against area effects. The creature is immune for… |
| Wizard | School of Enchantment | L2 Hypnotic Gaze | Once you use it on a creature you cannot use it on that same creature again until you finish a long rest. It does not recharge on a short rest and isn't a pool for you. |
| Wizard | School of Enchantment | L6 Instinctive Charm | The limit is per attacker: on a successful save the attacker cannot be affected again until you finish a long rest. There is no short rest recharge. |

## Cross-cutting findings

1. **SRD subclasses carry names only.** The twelve SRD subclasses (Berserker, Lore, Life, Land, Champion, Open Hand, Devotion, Hunter, Thief, Draconic Bloodline, Fiend, Evocation) have feature names and levels but no description or numbers in `class-features.json`. Their text lives elsewhere (`help/srd-features.json`, the rulebook), so the same audit of numbers was not possible from the subclass files.
2. **Warlock patron spells are granted for free.** Cleric domain, paladin oath, druid circle and ranger/sorcerer/artificer subclass spells are always-prepared or always-known and do not count against the limit, which matches the rules. The warlock patron lists are different: they are an *expanded spell list*. The rules add them to the pool the warlock picks from; they are not granted. `subclassSpellsFor` in `src/lib/srd/features.ts` treats all of them as free, so an Archfey, Great Old One, Celestial, Hexblade, Fathomless, Genie, Undead or Undying warlock receives ten spells they should have to learn with their limited spells known. This is more generous than the rules. The Fiend has no list at all, so it is the one patron missing its expanded spells entirely.
3. **Several 'use' limits are wrong in the same way.** Where a feature's uses depend on an ability modifier, the data often says 'proficiency bonus' (Glamour's Enthralling Performance, Alchemist's Restorative Reagents) or leaves the limit out (Battle Smith's Arcane Jolt, Mercy's Flurry of Healing and Harm). These need a pass for every 'a proficiency-bonus number of times per long rest' string in the subclass file.
4. **Edition mix.** The data mostly follows the 2014 books. Zealot's Warrior of the Gods follows Xanathar's; the 2024 Player's Handbook version is different. A statement of which edition the app follows would settle several of the 'imprecise' items.
5. **Feature descriptions are single sentences.** Many of the 'imprecise' findings are only omitted numbers (damage dice, temporary hit point amounts, scaling by level, save types). If the descriptions are shown to players as the rules text, these are the gaps they will notice first.
6. **Subclass choice level is correct for every class.** Artificer, Barbarian, Bard, Fighter, Monk, Paladin, Ranger and Rogue pick at 3, Cleric, Sorcerer and Warlock at 1, Druid and Wizard at 2. No finding.

## Artificer

Subclass chosen at level 3.

<a id="artificer-alchemist"></a>
### Alchemist

- **Source:** Tasha's Cauldron of Everything
- **App description:** An artificer who works in tinctures and reagents, healing allies and corroding enemies with thrown concoctions.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Tool Proficiency (Alchemist) | Proficiency with alchemist's supplies. |
| 3 | Experimental Elixir | After a long rest you brew an elixir with a random effect: healing, swiftness, resilience, boldness, flight or transformation. You can brew more by spending spell slots, and the number rises with level. |
| 5 | Alchemical Savant | Add your Intelligence modifier to one damage or healing roll of any spell you cast through your alchemical tools. |
| 9 | Restorative Reagents | You can cast lesser restoration without a slot a proficiency-bonus number of times per long rest, and your elixirs also grant temporary hit points. |
| 15 | Chemical Mastery | You gain resistance to acid and poison damage, immunity to the poisoned condition, and can cast greater restoration and heal once per long rest each without a slot. |

**Spells in the data:** L3: Healing Word, Ray of Sickness; L5: Flaming Sphere, Acid Arrow; L9: Gaseous Form, Mass Healing Word; L13: Blight, Death Ward; L17: Cloudkill, Raise Dead

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L9 Restorative Reagents | Cast lesser restoration without a slot a proficiency-bonus number of times per long rest. | The number of free casts is your Intelligence modifier (minimum 1), not your proficiency bonus. The casts also require alchemist's supplies as the spellcasting focus. Elixirs also grant temporary hit points of 2d6 + Intelligence modifier. | High |
| 2 | 🟡 Imprecise | L9 Restorative Reagents | Elixirs also grant temporary hit points (no amount). | Drinking an Experimental Elixir grants 2d6 + Intelligence modifier temporary hit points. | High |
| 3 | 🟡 Imprecise | L5 Alchemical Savant | Add Intelligence modifier to one damage or healing roll of any spell cast through alchemical tools. | The bonus applies to one roll of acid, fire, necrotic or poison damage, or one roll of hit points restored, of a spell cast using alchemist's supplies as the focus. It is not for any damage type (minimum +1). | High |
| 4 | 🟡 Imprecise | L3 Experimental Elixir | Random effect, more by spending spell slots, number rises with level. | One elixir after a long rest (two at 6th, three at 15th). Effects on a d6: healing 2d4 + INT; swiftness +10 ft speed 1 hour; resilience +1 AC 10 minutes; boldness d4 to attacks and saves 1 minute; flight 10 ft fly 10 minutes; transformation as alter self 10 minutes. Extra elixirs cost a spell slot of 1st level or higher, one chosen effect. The app text gives none of the numbers or the six effects' numbers. | High |

**Engine hint:** no engine reference found for: Alchemical Savant, Chemical Mastery.

<a id="artificer-armorer"></a>
### Armorer

- **Source:** Tasha's Cauldron of Everything
- **App description:** An artificer who lives inside their own powered armor, built either to guard the line or to strike from the dark.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Tool Proficiency (Armorer) | Proficiency with heavy armor and smith's tools. |
| 3 | Arcane Armor | Your armor becomes a magical suit that needs no Strength, cannot be removed against your will, and replaces missing limbs. |
| 3 | Armor Model | Choose Guardian, whose thunder gauntlets punish enemies who ignore you and whose Defensive Field grants temporary hit points, or Infiltrator, whose lightning launcher fires at range and whose powered steps add speed and stealth. |
| 5 | Extra Attack | Attack twice whenever you take the Attack action. |
| 9 | Armor Modifications | Your armor counts as four separate items for infusions: its armor, its weapon, and its helmet and boots. |
| 15 | Perfected Armor | Guardian armor pulls in enemies within 30 feet and knocks them prone; Infiltrator armor makes a struck target glow, granting attackers advantage and dealing extra force damage. |

**Spells in the data:** L3: Magic Missile, Thunderwave; L5: Mirror Image, Shatter; L9: Hypnotic Pattern, Lightning Bolt; L13: Fire Shield, Greater Invisibility; L17: Passwall, Wall of Force

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L3 Armor Model | Guardian and Infiltrator described in one line. | Guardian: Thunder Gauntlets (1d8 thunder, target has disadvantage attacking others until your next turn) and Defensive Field (bonus action, temporary hit points equal to artificer level, proficiency-bonus uses per long rest). Infiltrator: Lightning Launcher (1d6 lightning, ranged 90/300, +1d6 once per turn to a creature), Powered Steps (+5 ft speed), Dampening Field (advantage on Stealth). Numbers are missing from the app text. | High |
| 2 | 🟡 Imprecise | L15 Perfected Armor | Guardian pulls in enemies within 30 feet and knocks them prone; Infiltrator marks a struck target. | Guardian: a reaction to pull a creature within 30 feet that damages a creature other than you (Strength save, ends adjacent) and Thunder Gauntlets attack. Infiltrator: a hit with the Lightning Launcher makes the target glow, giving attackers advantage and 1d6 extra damage. The prone wording is unverified. | Low |

**Engine hint:** no engine reference found for: Arcane Armor, Armor Modifications.

<a id="artificer-artillerist"></a>
### Artillerist

- **Source:** Tasha's Cauldron of Everything
- **App description:** An artificer who builds a small arcane cannon and turns a fight into a siege.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Tool Proficiency (Artillerist) | Proficiency with woodcarver's tools. |
| 3 | Eldritch Cannon | An action creates a Tiny or Small cannon, and a bonus action activates it: a flamethrower cone, a force ballista that pushes, or a protector that hands out temporary hit points. Once per long rest, or by spending a spell slot. |
| 5 | Arcane Firearm | A wand, staff or rod becomes your firearm, adding 1d8 to one damage roll of each artificer spell you cast through it. |
| 9 | Explosive Cannon | Your cannon deals an extra 1d8, and you can detonate it as an action for 3d8 force damage in a 20-foot radius, halved on a successful Dexterity save. |
| 15 | Fortified Position | You can have two cannons at once, and you and your allies have half cover within 10 feet of one. |

**Spells in the data:** L3: Shield, Thunderwave; L5: Scorching Ray, Shatter; L9: Fireball, Wind Wall; L13: Ice Storm, Wall of Fire; L17: Cone of Cold, Wall of Force

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L3 Eldritch Cannon | Flamethrower cone, force ballista, protector; once per long rest or by spending a slot. | Cannon has AC 18 and hit points equal to 5 x artificer level. Flamethrower: 15-ft cone, Dexterity save, 2d8 fire. Force Ballista: ranged spell attack 120 ft, 2d8 force, pushes 5 ft. Protector: 1d8 + INT modifier temporary hit points to creatures within 10 ft. Lasts 1 hour. None of the numbers are in the app text. | High |

**Engine hint:** no engine reference found for: Fortified Position.

<a id="artificer-battle-smith"></a>
### Battle Smith

- **Source:** Tasha's Cauldron of Everything
- **App description:** An artificer who fights beside a construct companion and repairs the party as readily as their gear.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Tool Proficiency (Battle Smith) | Proficiency with smith's tools. |
| 3 | Battle Ready | Proficiency with martial weapons, and you use Intelligence instead of Strength or Dexterity for attack and damage with magic weapons. |
| 3 | Steel Defender | A construct companion that acts on your turn, attacks with a force-empowered bite, and can use its reaction to impose disadvantage on an attack against a nearby ally. |
| 5 | Extra Attack | Attack twice whenever you take the Attack action. |
| 9 | Arcane Jolt | Once per turn, a hit from your magic weapon or your defender deals an extra 2d6 force damage, or heals a creature within 30 feet for 2d6. |
| 15 | Improved Defender | Arcane Jolt rises to 4d6, your defender gains +2 AC, and its deflecting reaction also deals force damage to the attacker. |

**Spells in the data:** L3: Heroism, Shield; L5: Branding Smite, Warding Bond; L9: Aura of Vitality, Conjure Barrage; L13: Aura of Purity, Fire Shield; L17: Banishing Smite, Mass Cure Wounds

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟠 Missing | L9 Arcane Jolt (use limit) | Once per turn, a hit deals an extra 2d6 force or heals 2d6. | It is limited to a number of uses per long rest equal to your Intelligence modifier (minimum 1). The app text has no use limit. | High |
| 2 | 🟡 Imprecise | L3 Steel Defender | Force-empowered bite; reaction to impose disadvantage. | The defender uses Force-Empowered Rend (1d8 + proficiency bonus force damage) and Deflect Attack (reaction, disadvantage on an attack against a creature within 5 ft of it). Hit points are 2 + INT modifier + 5 x artificer level; it is repaired with mending or the artificer's magic. | High |

## Barbarian

Subclass chosen at level 3.

<a id="barbarian-path-of-the-berserker"></a>
### Path of the Berserker

- **Source:** SRD 5.1 / PHB
- **App description:** A barbarian whose rage becomes a frenzy: an extra attack while raging, immunity to fear and charm, and a presence that cows enemies.
- **Data file:** `class-features.json` (SRD, names only)

**As shipped**

| Level | Feature |
|---|---|
| 3 | Frenzy |
| 6 | Mindless Rage |
| 10 | Intimidating Presence |
| 14 | Retaliation |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | All levels (SRD) | Feature names only (Frenzy, Mindless Rage, Intimidating Presence, Retaliation); no description text in the data file. | Frenzy 3 (bonus-action melee attack each turn while raging; one level of exhaustion when the rage ends), Mindless Rage 6 (immune to charmed and frightened while raging), Intimidating Presence 10 (action; frighten a creature within 30 ft, Wisdom save DC 8 + PB + CHA), Retaliation 14 (reaction melee attack against a creature within 5 ft that damaged you). Names and levels are correct. | High |

<a id="barbarian-path-of-the-totem-warrior"></a>
### Path of the Totem Warrior

- **Source:** PHB (Elk and Tiger options: Xanathar's)
- **App description:** A barbarian who takes a beast as a spirit guide, borrowing its strengths in and out of a rage.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Spirit Seeker | Ritual casting of beast sense and speak with animals. |
| 3 | Totem Spirit | Choose a totem: Bear gives resistance to all damage but psychic while raging, Eagle gives a Dash bonus action and no opportunity-attack advantage against you, Wolf gives allies advantage on melee attacks against enemies within 5 feet of you. |
| 6 | Aspect of the Beast | A permanent boon from the totem: Bear doubles carrying capacity and gives advantage on Strength checks to push, pull, lift or break, Eagle grants sight to 1 mile and no disadvantage on Perception in dim light, Wolf lets you track at a fast pace and move stealthily at a normal pace. |
| 10 | Spirit Walker | Cast commune with nature as a ritual; the answer arrives as a spirit beast. |
| 14 | Totemic Attunement | While raging: Bear makes enemies within 5 feet suffer disadvantage attacking anyone but you, Eagle grants flight equal to your walking speed for a turn at a time, Wolf lets a bonus action knock a Large or smaller creature prone when you hit it. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L3 Totem Spirit: Eagle | No opportunity-attack advantage against you. | While raging and not in heavy armor, other creatures have DISADVANTAGE on opportunity attack rolls against you, and you can Dash as a bonus action. The app text says 'no advantage', which is a different rule. | High |
| 2 | 🟠 Missing | L3 / L6 / L14 totem options | Only Bear, Eagle and Wolf are described. | Xanathar's adds Elk and Tiger as options for Totem Spirit, Aspect of the Beast and Totemic Attunement. They are missing at all three levels (details to be taken from the book). | High |

**Engine hint:** no engine reference found for: Spirit Seeker, Spirit Walker.

<a id="barbarian-path-of-the-ancestral-guardian"></a>
### Path of the Ancestral Guardian

- **Source:** Xanathar's Guide to Everything
- **App description:** Ancestral spirits answer this barbarian's rage and shield the people fighting beside them.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Ancestral Protectors | While raging, the first creature you hit each turn has disadvantage on attacks against anyone but you, and its damage against others is halved until your next turn. |
| 6 | Spirit Shield | While raging, a reaction reduces damage to an ally you can see within 30 feet by 2d6, rising to 3d6 at 10th level and 4d6 at 14th. |
| 10 | Consult the Spirits | Cast augury or clairvoyance once per short or long rest, using Wisdom, without a slot or components. |
| 14 | Vengeful Ancestors | Damage prevented by Spirit Shield is dealt straight back to the attacker as force damage. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L3 Ancestral Protectors | Target has disadvantage on attacks against anyone but you; its damage against others is halved. | The target has disadvantage on attacks that don't target you, and any creature it hits with such an attack other than you has resistance to that damage. Halving and resistance are equivalent only for damage the creature takes; the text should say resistance. | Medium |

**Engine hint:** no engine reference found for: Vengeful Ancestors.

<a id="barbarian-path-of-the-storm-herald"></a>
### Path of the Storm Herald

- **Source:** PHB
- **App description:** Rage manifests as a 10-foot aura of desert heat, arctic cold, or sea storm.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Storm Aura | A 10-foot aura while raging, activated as a bonus action: Desert deals 2 fire damage to every other creature in it, Sea forces one creature you pick to make a Dexterity save or take 1d6 lightning, Tundra grants every ally in it 2 temporary hit points. Damage and hit points scale with barbarian level. |
| 6 | Storm Soul | Resistance to your aura's damage type and a matching boon: Desert gives fire resistance and heat protection, Sea gives lightning resistance, swim speed and water breathing, Tundra gives cold resistance and lets you freeze 5 feet of water. |
| 10 | Shielding Storm | Every ally inside your Storm Aura shares your Storm Soul damage resistance. |
| 14 | Raging Storm | Your aura gains a rider once per turn: Desert deals fire damage to a creature that hits you, Sea knocks a creature prone on a failed Strength save, Tundra halves a creature's speed on a failed Strength save. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L14 Raging Storm: Tundra | Halves a creature's speed on a failed Strength save. | On a failed Strength save the creature's speed is reduced to 0 until the start of your next turn, not halved. | Medium |
| 2 | 🟡 Imprecise | L3 Storm Aura | Activated as a bonus action while raging; Desert deals 2 fire to every other creature in it. | The aura is activated when you rage and can be re-chosen as a bonus action on later turns. Numbers scale with barbarian level: Desert 2 fire at 3rd (3 at 5th, 4 at 10th, 5 at 15th, 6 at 20th), Sea 1d6 lightning at 3rd (2d6 at 10th, 3d6 at 15th, 4d6 at 20th, Dexterity save for half), Tundra 2 temporary hit points at 3rd (same steps as Desert). The app text states scaling exists but gives no table. | Medium |

**Engine hint:** no engine reference found for: Storm Soul.

<a id="barbarian-path-of-the-zealot"></a>
### Path of the Zealot

- **Source:** Xanathar's Guide to Everything
- **App description:** A divine champion whose fury is holy, and who death itself has trouble keeping.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Divine Fury | The first creature you hit each turn while raging takes an extra 1d6 + half your barbarian level radiant or necrotic damage. |
| 3 | Warrior of the Gods | A spell that restores you to life needs no material component. |
| 6 | Fanatical Focus | Once per rage, reroll a failed saving throw and use the new result. |
| 10 | Zealous Presence | A bonus action, once per long rest, grants up to ten allies within 60 feet advantage on attacks and saves until your next turn. |
| 14 | Rage Beyond Death | While raging, dropping to 0 hit points does not knock you out; you fight on and only die if the rage ends while you are still at 0. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L3 Warrior of the Gods | A spell that restores you to life needs no material component. | Correct for Xanathar's. The 2024 Player's Handbook version is different (a pool of four d12s to heal as a bonus action). Decide which edition the app follows. | High |
| 2 | 🟡 Imprecise | L10 Zealous Presence | Bonus action, once per long rest. | Once per long rest, or again by expending a spell slot of 4th level or higher. | High |

**Engine hint:** no engine reference found for: Warrior of the Gods, Rage Beyond Death.

<a id="barbarian-path-of-the-beast"></a>
### Path of the Beast

- **Source:** Tasha's Cauldron of Everything
- **App description:** Rage grows weapons out of the barbarian's own body: claws, a tail, savage teeth.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Form of the Beast | When you rage, grow a natural weapon until the rage ends: Bite deals 1d8 piercing and heals you when you are below half hit points, Claws deal 1d6 slashing and grant an extra attack with them, Tail deals 1d8 piercing with 10-foot reach and adds to AC as a reaction. |
| 6 | Bestial Soul | Your natural weapons count as magical, and you can adapt to swim, climb or jump after each rest. |
| 10 | Infectious Fury | A proficiency-bonus number of times per long rest, a natural-weapon hit forces a Wisdom save: the target either attacks a creature of your choice or takes 2d12 psychic damage. |
| 14 | Call the Hunt | A proficiency-bonus number of times per long rest, when you enter your rage, grant up to five allies an extra d6 of damage on one hit per turn, and gain 5 temporary hit points for each ally who accepts. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L3 Form of the Beast: numbers | Bite 1d8 and heals below half hit points; Claws 1d6 with an extra attack; Tail 1d8 with 10-foot reach. | Bite: once per turn, a bite that hits while you are below half your hit points restores hit points equal to your proficiency bonus. Claws: one extra claw attack each time you take the Attack action. Tail: reaction to add 1d8 to your AC against one attack. The app text gives the dice but not the heal amount. | Medium |

**Engine hint:** no engine reference found for: Bestial Soul.

<a id="barbarian-path-of-wild-magic"></a>
### Path of Wild Magic

- **Source:** Tasha's Cauldron of Everything
- **App description:** Raw magic leaks out of this barbarian every time their temper does.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Magic Awareness | As an action, sense spells and magic items within 60 feet, a number of times per long rest equal to your proficiency bonus. |
| 3 | Wild Surge | Roll on your wild magic table each time you rage; the rolled effect lasts as long as the rage. |
| 6 | Bolstering Magic | A proficiency-bonus number of times per long rest, touch an ally to grant a bonus d3 on attacks and ability checks for 10 minutes, or restore one expended spell slot of 3rd level or lower. |
| 10 | Unstable Backlash | When you take damage or fail a save while raging, a reaction rerolls your Wild Surge effect. |
| 14 | Controlled Surge | Roll twice on the wild magic table and choose which result takes hold. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L6 Bolstering Magic | Restore one expended spell slot of 3rd level or lower. | You roll a d3 and regain a spell slot of that level (not a chosen slot up to 3rd). The bonus-d3 option is correct. | Medium |
| 2 | 🟡 Imprecise | L3 Wild Surge | Roll on your wild magic table each time you rage. | A d8 table of eight effects; the surge lasts for the rage and can be re-triggered as a bonus action for some entries. The table itself is not in the subclass data. | Medium |

**Engine hint:** no engine reference found for: Controlled Surge.

<a id="barbarian-path-of-the-giant"></a>
### Path of the Giant

- **Source:** Bigby Presents: Glory of the Giants
- **App description:** A barbarian who channels giantkind, growing to their size and hurling what comes to hand.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Giant's Power | Learn Giant and the druidcraft cantrip. |
| 3 | Elemental Cleaver | While raging, one weapon deals your chosen elemental damage instead of its own type and gains the thrown property with a 20/60 range, returning to your hand each throw. |
| 6 | Mighty Impel | As a bonus action while raging, move a Large or smaller creature within 10 feet up to 30 feet through the air; an unwilling one gets a Strength save. |
| 10 | Demiurgic Colossus | While raging you grow to Large, your reach grows by 5 feet, Elemental Cleaver's throw range doubles and its damage grows by 1d10. |
| 14 | Elemental Fury | Once per turn while raging, add your rage damage to an Elemental Cleaver hit again, or force a Constitution save on a creature within 10 feet for 3d8 of your element. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | Feature levels | Giant's Power 3, Elemental Cleaver 3, Mighty Impel 6, Demiurgic Colossus 10, Elemental Fury 14. | Giant's Power 3, Giant's Havoc 3, Elemental Cleaver 6, Mighty Impel 10, Demiurgic Colossus 14. The app moves each feature earlier by one tier and adds a final feature. | Medium |
| 2 | 🟠 Missing | L3 Giant's Havoc | Not present. | Giant's Havoc: Crushing Throw (add rage damage to thrown weapon damage) and Giant Stature (reach +5 ft and Large size while raging). | Medium |
| 3 | 🔴 Wrong | L14 Elemental Fury | Once per turn add rage damage again, or force a Constitution save for 3d8 of your element. | As I recall it, the Path of the Giant has no 'Elemental Fury' feature; its 14th-level feature is Demiurgic Colossus, which the app places at 10th. Check against Bigby Presents. | Medium |
| 4 | 🔴 Wrong | L3 Giant's Power | Learn Giant and the druidcraft cantrip. | Learn Giant and your choice of thaumaturgy or druidcraft. | Medium |

## Bard

Subclass chosen at level 3.

<a id="bard-college-of-lore"></a>
### College of Lore

- **Source:** SRD 5.1 / PHB
- **App description:** A bard who collects knowledge from every discipline, cutting enemies down with words and borrowing spells from any class list.
- **Data file:** `class-features.json` (SRD, names only)

**As shipped**

| Level | Feature |
|---|---|
| 3 | Cutting Words |
| 3 | Bonus Proficiencies (Lore) |
| 6 | Additional Magical Secrets |
| 14 | Peerless Skill |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Cutting Words, Bonus Proficiencies (Lore), Additional Magical Secrets, Peerless Skill. | Names and levels match (3, 3, 6, 14). Cutting Words: reaction, spend a Bardic Inspiration die to subtract from a creature's attack, check or damage roll within 60 ft. Bonus Proficiencies: three skills. Additional Magical Secrets: two spells from any class. Peerless Skill: add a Bardic Inspiration die to your own check. | High |

**Engine hint:** no engine reference found for: Additional Magical Secrets.

<a id="bard-college-of-valor"></a>
### College of Valor

- **Source:** PHB
- **App description:** A battle-skald who fights in the front rank and turns the fight itself into the song.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Bonus Proficiencies (Valor) | Proficiency with medium armor, shields and martial weapons. |
| 3 | Combat Inspiration | A creature holding your Bardic Inspiration die may add it to a damage roll, or use a reaction to add it to AC against one attack. |
| 6 | Extra Attack | Attack twice when you take the Attack action. |
| 14 | Battle Magic | After casting a spell with an action, a bonus action makes one weapon attack. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

<a id="bard-college-of-glamour"></a>
### College of Glamour

- **Source:** Xanathar's Guide to Everything
- **App description:** Feywild-touched charm: this bard rescues allies with a word and commands rooms on presence alone.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Mantle of Inspiration | A bonus action spends a Bardic Inspiration to give allies within 60 feet temporary hit points and an immediate reaction-free move. |
| 3 | Enthralling Performance | After a minute of performance, charm up to a proficiency-bonus number of humanoids that fail a Wisdom save; once per short or long rest. |
| 6 | Mantle of Majesty | Cast command as a bonus action for a minute without a slot, with listeners charmed against retaliating; once per long rest. |
| 14 | Unbreakable Majesty | A bonus action makes you awesome to behold for a minute: an attacker must pass a Charisma save or lose the attack and be unable to target you that turn. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L3 Enthralling Performance | Charm up to a proficiency-bonus number of humanoids. | The number is equal to your Charisma modifier (minimum 1), not your proficiency bonus. | High |
| 2 | 🟡 Imprecise | L3 Mantle of Inspiration | Temporary hit points and a reaction-free move. | A number of creatures equal to your Charisma modifier (minimum 1), within 60 ft, each gain 5 temporary hit points (8 at 5th, 11 at 10th, 14 at 15th) and can use a reaction to move up to their speed without provoking. The app text omits the count and the scaling. | High |

<a id="bard-college-of-swords"></a>
### College of Swords

- **Source:** Xanathar's Guide to Everything
- **App description:** A blade-dancing performer whose weapon flourishes are the performance.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Bonus Proficiencies (Swords) | Proficiency with medium armor and the scimitar; your weapon can serve as a spellcasting focus. |
| 3 | Fighting Style (Blade) | Choose the Dueling or Two-Weapon Fighting style. |
| 3 | Blade Flourish | Your speed rises by 10 feet on any turn you take the Attack action, and once per turn you may spend a Bardic Inspiration die on a Defensive, Slashing or Mobile flourish. |
| 6 | Extra Attack | Attack twice when you take the Attack action. |
| 14 | Master's Flourish | Roll a d6 for a flourish instead of spending a Bardic Inspiration die. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Master's Flourish.

<a id="bard-college-of-whispers"></a>
### College of Whispers

- **Source:** Xanathar's Guide to Everything
- **App description:** A bard who trades in fear and secrets and wears other people's lives like costumes.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Psychic Blades | Spend a Bardic Inspiration die on a weapon hit for extra psychic damage, 2d6 at 3rd level and rising with bard level. |
| 3 | Words of Terror | A minute of private conversation frightens a humanoid of you or someone you name for an hour on a failed Wisdom save; once per short or long rest. |
| 6 | Mantle of Whispers | A reaction captures the shadow of a humanoid that just died, letting you wear their identity for an hour; once per short or long rest. |
| 14 | Shadow Lore | Whisper a creature's darkest secret: on a failed Wisdom save it is charmed and obeys your requests for 8 hours; once per long rest. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

<a id="bard-college-of-eloquence"></a>
### College of Eloquence

- **Source:** Mythic Odysseys of Theros / Tasha's
- **App description:** A rhetorician who never misses with a word and never wastes an inspiration die.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Silver Tongue | Treat any d20 of 9 or lower as a 10 on Persuasion and Deception checks. |
| 3 | Unsettling Words | A bonus action spends a Bardic Inspiration die to subtract it from one creature's next saving throw. |
| 6 | Unfailing Inspiration | An inspiration die that fails to change the outcome is not spent. |
| 14 | Infectious Inspiration | A reaction copies a successful inspiration die onto another creature, a Charisma-modifier number of times per long rest. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟠 Missing | L6 Universal Speech | Not present. | At 6th level Eloquence also gains Universal Speech: an action, once per long rest (or by spending a Bardic Inspiration die), lets a number of creatures equal to your Charisma modifier understand you for 1 hour. It is a level 6 feature alongside Unfailing Inspiration. | High |

**Engine hint:** no engine reference found for: Silver Tongue, Unfailing Inspiration.

<a id="bard-college-of-creation"></a>
### College of Creation

- **Source:** Tasha's Cauldron of Everything
- **App description:** The Song of Creation still echoes, and this bard can sing objects and dancing things into being.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Mote of Potential | Your Bardic Inspiration also produces a mote whose burst helps the holder: extra damage on an attack, temporary hit points on a check, or an ally bonus on a save. |
| 3 | Performance of Creation | Create a nonmagical item worth no more than 20 times your bard level as an action, once per long rest or with a spell slot. |
| 6 | Animating Performance | Animate a Large or smaller object as a dancing companion that acts on your turn and can make a thunderous Strength attack; once per long rest or with a spell slot. |
| 14 | Creative Crescendo | Performance of Creation makes several items at once and drops the cost limit on all but the largest. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Creative Crescendo.

<a id="bard-college-of-spirits"></a>
### College of Spirits

- **Source:** Van Richten's Guide to Ravenloft
- **App description:** A medium who calls up stories from the dead and lets the tale decide what happens next.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Guiding Whispers | You know guidance and can cast it at 60 feet. |
| 3 | Spiritual Focus | A candle, a deck, a talking board or the like serves as your spellcasting focus and adds a die to the damage or healing of your bard spells at higher levels. |
| 3 | Tales from Beyond | Spend a Bardic Inspiration die and roll on a table of spirit tales, each with its own effect you can release on a target. |
| 6 | Spirit Session | An hour-long seance with allies lets you learn one divination or necromancy spell of a level up to the ritual's length and cast it until your next long rest. |
| 14 | Mystical Connection | Reroll an unwanted Tales from Beyond result, and roll for it without spending a die once per short rest. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L6 Spirit Session | Learn one divination or necromancy spell of a level up to the ritual's length. | The ritual is an hour long with up to your proficiency bonus of willing creatures (including you). The spell's level cannot exceed the number of creatures who took part and must be a level you can cast; it can be from any class's list but must be divination or necromancy. The app ties the level to the ritual's duration, which is wrong. | Medium |

**Engine hint:** no engine reference found for: Guiding Whispers, Spiritual Focus, Tales from Beyond, Mystical Connection.

## Cleric

Subclass chosen at level 1.

<a id="cleric-life-domain"></a>
### Life Domain

- **Source:** SRD 5.1 / PHB
- **App description:** A cleric of healing and vitality, whose cures restore more, whose armor is heavy, and whose domain spells are always prepared.
- **Data file:** `class-features.json` (SRD, names only)

**As shipped**

| Level | Feature |
|---|---|
| 1 | Bonus Proficiency (heavy armor) |
| 1 | Disciple of Life |
| 2 | Channel Divinity: Preserve Life |
| 6 | Blessed Healer |
| 8 | Divine Strike |
| 17 | Supreme Healing |

**Spells in the data:** L1: Bless, Cure Wounds; L3: Lesser Restoration, Spiritual Weapon; L5: Beacon of Hope, Revivify; L7: Death Ward, Guardian of Faith; L9: Mass Cure Wounds, Raise Dead

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Bonus Proficiency (heavy armor), Disciple of Life, Preserve Life, Blessed Healer, Divine Strike, Supreme Healing. Domain spells are correct. | Names, levels (1, 1, 2, 6, 8, 17) and the domain spell list all match the SRD. Descriptions are not in the subclass file. | High |

<a id="cleric-knowledge-domain"></a>
### Knowledge Domain

- **Source:** PHB
- **App description:** A cleric of the gods of learning, for whom every secret is a form of power.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Blessings of Knowledge | Two extra languages, and proficiency plus expertise in two of Arcana, History, Nature and Religion. |
| 2 | Channel Divinity: Knowledge of the Ages | Gain proficiency with one skill or tool of your choice for 10 minutes. |
| 6 | Channel Divinity: Read Thoughts | Read a creature's surface thoughts for a minute and cast suggestion on it without a slot. |
| 8 | Potent Spellcasting | Add your Wisdom modifier to the damage of your cleric cantrips. |
| 17 | Visions of the Past | A minute of concentration reveals an object's or a place's recent history. |

**Spells in the data:** L1: Command, Identify; L3: Augury, Suggestion; L5: Nondetection, Speak with Dead; L7: Arcane Eye, Confusion; L9: Legend Lore, Scrying

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Potent Spellcasting, Visions of the Past.

<a id="cleric-light-domain"></a>
### Light Domain

- **Source:** PHB
- **App description:** A cleric of sun and flame who burns away darkness and the things that hide in it.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Bonus Cantrip (Light) | You know the light cantrip. |
| 1 | Warding Flare | A reaction imposes disadvantage on an attack against you, a Wisdom-modifier number of times per long rest. |
| 2 | Channel Divinity: Radiance of the Dawn | Dispel magical darkness within 30 feet and deal 2d10 + cleric level radiant damage to hostile creatures, halved on a Constitution save. |
| 6 | Improved Warding Flare | Warding Flare also protects an ally you can see within 30 feet. |
| 8 | Potent Spellcasting | Add your Wisdom modifier to the damage of your cleric cantrips. |
| 17 | Corona of Light | For a minute, shed 60 feet of bright light and give enemies in it disadvantage on saves against fire and radiant spells. |

**Spells in the data:** L1: Burning Hands, Faerie Fire; L3: Flaming Sphere, Scorching Ray; L5: Daylight, Fireball; L7: Guardian of Faith, Wall of Fire; L9: Flame Strike, Scrying

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Bonus Cantrip (Light), Potent Spellcasting.

<a id="cleric-nature-domain"></a>
### Nature Domain

- **Source:** PHB
- **App description:** A priest of the wild gods, as at home with beasts and weather as with prayer.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Acolyte of Nature | One druid cantrip and proficiency in Animal Handling, Nature or Survival. |
| 1 | Bonus Proficiency (heavy armor) | Proficiency with heavy armor. |
| 2 | Channel Divinity: Charm Animals and Plants | Beasts and plants within 30 feet that fail a Wisdom save are charmed for a minute and friendly to you and your allies. |
| 6 | Dampen Elements | A reaction grants you or an ally within 30 feet resistance to one instance of acid, cold, fire, lightning or thunder damage. |
| 8 | Divine Strike | Once per turn a weapon hit deals an extra 1d8 cold, fire or lightning damage, rising to 2d8 at 14th level. |
| 17 | Master of Nature | A bonus action commands creatures charmed by your Channel Divinity. |

**Spells in the data:** L1: Animal Friendship, Speak with Animals; L3: Barkskin, Spike Growth; L5: Plant Growth, Wind Wall; L7: Dominate Beast, Grasping Vine; L9: Insect Plague, Tree Stride

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

<a id="cleric-tempest-domain"></a>
### Tempest Domain

- **Source:** PHB
- **App description:** Thunder and lightning made doctrine: this cleric answers insult with a storm.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Bonus Proficiencies (Tempest) | Proficiency with martial weapons and heavy armor. |
| 1 | Wrath of the Storm | A reaction deals 2d8 lightning or thunder damage to a creature that hits you in melee, halved on a Dexterity save; a Wisdom-modifier number of times per long rest. |
| 2 | Channel Divinity: Destructive Wrath | Deal maximum damage instead of rolling for lightning or thunder damage. |
| 6 | Thunderbolt Strike | Lightning damage you deal pushes a Large or smaller creature 10 feet away. |
| 8 | Divine Strike | Once per turn a weapon hit deals an extra 1d8 thunder damage, rising to 2d8 at 14th level. |
| 17 | Stormborn | You have a flying speed equal to your walking speed while not underground or indoors. |

**Spells in the data:** L1: Fog Cloud, Thunderwave; L3: Gust of Wind, Shatter; L5: Call Lightning, Sleet Storm; L7: Control Water, Ice Storm; L9: Destructive Wave, Insect Plague

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Thunderbolt Strike.

<a id="cleric-trickery-domain"></a>
### Trickery Domain

- **Source:** PHB
- **App description:** A holy trickster whose blessings are disguises, duplicates and well-aimed mischief.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Blessing of the Trickster | Touch a willing creature to give it advantage on Stealth checks for an hour. |
| 2 | Channel Divinity: Invoke Duplicity | Create an illusory double for a minute that you can move, cast spells through, and use to gain attack advantage. |
| 6 | Channel Divinity: Cloak of Shadows | Become invisible until the end of your next turn or until you attack or cast. |
| 8 | Divine Strike | Once per turn a weapon hit deals an extra 1d8 poison damage, rising to 2d8 at 14th level. |
| 17 | Improved Duplicity | Invoke Duplicity creates up to four illusory duplicates. |

**Spells in the data:** L1: Charm Person, Disguise Self; L3: Mirror Image, Pass without Trace; L5: Blink, Dispel Magic; L7: Dimension Door, Polymorph; L9: Dominate Person, Modify Memory

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Improved Duplicity.

<a id="cleric-war-domain"></a>
### War Domain

- **Source:** PHB
- **App description:** A battle priest whose god expects the front line, not the back rank.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Bonus Proficiencies (War) | Proficiency with martial weapons and heavy armor. |
| 1 | War Priest | A bonus-action weapon attack when you take the Attack action, a Wisdom-modifier number of times per long rest. |
| 2 | Channel Divinity: Guided Strike | Add +10 to one attack roll, before or after seeing the result. |
| 6 | Channel Divinity: War God's Blessing | A reaction grants an ally within 30 feet +10 on an attack roll. |
| 8 | Divine Strike | Once per turn a weapon hit deals an extra 1d8 damage of the weapon's type, rising to 2d8 at 14th level. |
| 17 | Avatar of Battle | Resistance to nonmagical bludgeoning, piercing and slashing damage. |

**Spells in the data:** L1: Divine Favor, Shield of Faith; L3: Magic Weapon, Spiritual Weapon; L5: Crusader's Mantle, Spirit Guardians; L7: Freedom of Movement, Stoneskin; L9: Flame Strike, Hold Monster

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Avatar of Battle.

<a id="cleric-death-domain"></a>
### Death Domain

- **Source:** Dungeon Master's Guide
- **App description:** A cleric of the gods of endings, comfortable with necrotic power and the grave's authority.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Bonus Proficiency (martial weapons) | Proficiency with martial weapons. |
| 1 | Reaper | Learn a necromancy cantrip; single-target necromancy cantrips can hit a second creature within 5 feet of the first. |
| 2 | Channel Divinity: Touch of Death | A melee hit deals extra necrotic damage equal to 5 + twice your cleric level. |
| 6 | Inescapable Destruction | Your necrotic damage ignores resistance. |
| 8 | Divine Strike | Once per turn a weapon hit deals an extra 1d8 necrotic damage, rising to 2d8 at 14th level. |
| 17 | Improved Reaper | Cast a necromancy spell of 1st to 5th level that targets one creature at two creatures within 5 feet, at the cost of 1d8 of your own hit points per spell level. |

**Spells in the data:** L1: False Life, Ray of Sickness; L3: Blindness/Deafness, Ray of Enfeeblement; L5: Animate Dead, Vampiric Touch; L7: Blight, Death Ward; L9: Antilife Shell, Cloudkill

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L17 Improved Reaper | Cast a necromancy spell of 1st to 5th level that targets one creature at two creatures within 5 feet, at the cost of 1d8 of your own hit points per spell level. | No hit point cost exists. Improved Reaper simply lets a single-target necromancy spell of 1st through 5th level target two creatures within 5 feet of each other; you must provide the material components for each target if they are consumed. | Medium |

<a id="cleric-forge-domain"></a>
### Forge Domain

- **Source:** Xanathar's Guide to Everything
- **App description:** A smith-priest who blesses armor and weapons and treats the anvil as an altar.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Bonus Proficiencies (Forge) | Proficiency with heavy armor and smith's tools. |
| 1 | Blessing of the Forge | After a long rest, enchant a weapon or suit of armor you touch with a +1 bonus until you use the feature again. |
| 2 | Channel Divinity: Artisan's Blessing | An hour of ritual smithing creates a simple metal item worth up to 100 gp. |
| 6 | Soul of the Forge | +1 AC while wearing heavy armor, and resistance to fire damage. |
| 8 | Divine Strike | Once per turn a weapon hit deals an extra 1d8 fire damage, rising to 2d8 at 14th level. |
| 17 | Saint of Forge and Fire | Immunity to fire damage, and resistance to nonmagical bludgeoning, piercing and slashing while in heavy armor. |

**Spells in the data:** L1: Identify, Searing Smite; L3: Heat Metal, Magic Weapon; L5: Elemental Weapon, Protection from Energy; L7: Fabricate, Wall of Fire; L9: Animate Objects, Creation

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

<a id="cleric-grave-domain"></a>
### Grave Domain

- **Source:** Xanathar's Guide to Everything
- **App description:** A warden of the border between life and death who hates undeath more than dying.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Circle of Mortality | Healing spells cast on a creature at 0 hit points heal the maximum; you can cast spare the dying at 30 feet as a bonus action. |
| 1 | Eyes of the Grave | Detect undead within 60 feet as an action, a Wisdom-modifier number of times per long rest. |
| 2 | Channel Divinity: Path to the Grave | Curse a creature so the next attack against it has advantage and doubles its damage vulnerability for that hit. |
| 6 | Sentinel at Death's Door | A reaction turns a critical hit against a creature within 30 feet into a normal hit, a Wisdom-modifier number of times per long rest. |
| 8 | Potent Spellcasting | Add your Wisdom modifier to the damage of your cleric cantrips. |
| 17 | Keeper of Souls | Once per turn, when an enemy within 60 feet dies, an ally you can see regains hit points equal to its Hit Dice. |

**Spells in the data:** L1: Bane, False Life; L3: Gentle Repose, Ray of Enfeeblement; L5: Revivify, Vampiric Touch; L7: Blight, Death Ward; L9: Antilife Shell, Raise Dead

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L2 Channel Divinity: Path to the Grave | The next attack against the cursed creature has advantage and doubles its damage vulnerability for that hit. | The creature is cursed until the end of your next turn. The next time you or an ally hits it with an attack, it is vulnerable to ALL of that attack's damage, then the curse ends. There is no advantage on the attack. | Medium |

**Engine hint:** no engine reference found for: Potent Spellcasting.

<a id="cleric-order-domain"></a>
### Order Domain

- **Source:** Tasha's Cauldron of Everything
- **App description:** A cleric of law and hierarchy who turns obedience into a battlefield asset.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Bonus Proficiencies (Order) | Heavy armor, plus proficiency in Intimidation or Persuasion. |
| 1 | Voice of Authority | After a spell of 1st level or higher targets an ally, that ally may use a reaction to make one weapon attack. |
| 2 | Channel Divinity: Order's Demand | Creatures of your choice within 30 feet are charmed for a minute on a failed Wisdom save, and you may drop or push one of them 30 feet. |
| 6 | Embodiment of the Law | A Wisdom-modifier number of times per long rest, cast an enchantment spell of 1st level or higher as a bonus action. |
| 8 | Divine Strike | Once per turn a weapon hit deals an extra 1d8 psychic damage, rising to 2d8 at 14th level. |
| 17 | Order's Wrath | Once per turn, a creature you damage with Divine Strike takes an extra 2d8 psychic damage the next time an ally hits it. |

**Spells in the data:** L1: Command, Heroism; L3: Hold Person, Zone of Truth; L5: Mass Healing Word, Slow; L7: Compulsion, Locate Creature; L9: Commune, Dominate Person

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L2 Channel Divinity: Order's Demand | Charm on a failed Wisdom save and you may drop or push one of them 30 feet. | As I recall it: creatures of your choice within 30 ft make a Wisdom save or are charmed until the end of your next turn, and a charmed creature drops what it is holding and takes psychic damage (2d10 + cleric level). I do not recall any 'push 30 feet' effect. Check against Tasha's. | Low |

<a id="cleric-peace-domain"></a>
### Peace Domain

- **Source:** Tasha's Cauldron of Everything
- **App description:** A binder of bonds whose blessing makes a party fight and endure as one thing.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Implement of Peace | Proficiency in Performance, Insight or Persuasion. |
| 1 | Emboldening Bond | Bond a proficiency-bonus number of creatures for 10 minutes; while near each other they may add a d4 to an attack, check or save, once per turn. |
| 2 | Channel Divinity: Balm of Peace | Move without provoking and heal each creature you pass within 5 feet for 2d6 + your Wisdom modifier. |
| 6 | Protective Bond | A bonded creature may use a reaction to teleport to another and take its damage instead. |
| 8 | Potent Spellcasting | Add your Wisdom modifier to the damage of your cleric cantrips. |
| 17 | Expansive Bond | Emboldening Bond's range grows to 60 feet and Protective Bond halves the damage taken. |

**Spells in the data:** L1: Heroism, Sanctuary; L3: Aid, Warding Bond; L5: Beacon of Hope, Sending; L7: Aura of Purity, Otiluke's Resilient Sphere; L9: Greater Restoration, Rary's Telepathic Bond

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Emboldening Bond, Potent Spellcasting, Expansive Bond.

<a id="cleric-twilight-domain"></a>
### Twilight Domain

- **Source:** Tasha's Cauldron of Everything
- **App description:** A shepherd of the border hour, granting sight in the dark and calm against terror.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Bonus Proficiencies (Twilight) | Martial weapons and heavy armor. |
| 1 | Eyes of Night | Darkvision out to 300 feet, shareable with a Wisdom-modifier number of creatures once per long rest. |
| 1 | Vigilant Blessing | Touch a creature to give it advantage on its next initiative roll. |
| 2 | Channel Divinity: Twilight Sanctuary | A 30-foot aura for a minute that grants each ally either temporary hit points of 1d6 + cleric level or an end to a charm or fear effect, each round. |
| 6 | Steps of Night | A bonus action in dim light or darkness grants flight for a minute, a proficiency-bonus number of times per long rest. |
| 8 | Divine Strike | Once per turn a weapon hit deals an extra 1d8 radiant damage, rising to 2d8 at 14th level. |
| 17 | Twilight Shroud | Allies inside Twilight Sanctuary have half cover. |

**Spells in the data:** L1: Faerie Fire, Sleep; L3: Moonbeam, See Invisibility; L5: Aura of Vitality, Leomund's Tiny Hut; L7: Aura of Life, Greater Invisibility; L9: Circle of Power, Mislead

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Twilight Shroud.

<a id="cleric-arcana-domain"></a>
### Arcana Domain

- **Source:** Sword Coast Adventurer's Guide
- **App description:** A cleric of the gods of magic itself, as much a scholar of spellcraft as a priest.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Arcane Initiate | Proficiency in Arcana and two wizard cantrips of your choice. |
| 2 | Channel Divinity: Arcane Abjuration | Turn one celestial, elemental, fey or fiend within 30 feet on a failed Wisdom save, banishing weaker ones outright at higher levels. |
| 6 | Spell Breaker | Restoring hit points with a spell also ends one spell of equal or lower level on the target. |
| 8 | Potent Spellcasting | Add your Wisdom modifier to the damage of your cleric cantrips. |
| 17 | Arcane Mastery | Add four wizard spells of 6th, 7th, 8th and 9th level to your cleric spell list. |

**Spells in the data:** L1: Detect Magic, Magic Missile; L3: Magic Weapon, Nystul's Magic Aura; L5: Dispel Magic, Magic Circle; L7: Arcane Eye, Leomund's Secret Chest; L9: Planar Binding, Teleportation Circle

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Spell Breaker, Potent Spellcasting, Arcane Mastery.

## Druid

Subclass chosen at level 2.

<a id="druid-circle-of-the-land"></a>
### Circle of the Land

- **Source:** SRD 5.1 / PHB
- **App description:** A druid attuned to one kind of terrain, with bonus spells drawn from it, an extra cantrip and slots recovered on a short rest.
- **Data file:** `class-features.json` (SRD, names only)

**As shipped**

| Level | Feature |
|---|---|
| 2 | Bonus Cantrip |
| 2 | Natural Recovery |
| 3 | Circle Spells |
| 6 | Land's Stride |
| 10 | Nature's Ward |
| 14 | Nature's Sanctuary |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟠 Missing | L3 Circle Spells (terrain lists) | Feature name only; no spell list in the data. | The Land druid picks a terrain at 2nd level (Arctic, Coast, Desert, Forest, Grassland, Mountain, Swamp, Underdark), each with its own always-prepared spells at 3, 5, 7 and 9. No terrain choice or terrain spell lists were found in the subclass data (not verified in the builder). | Medium |
| 2 | 🟡 Imprecise | All levels (SRD) | Names only: Bonus Cantrip, Natural Recovery, Circle Spells, Land's Stride, Nature's Ward, Nature's Sanctuary. | Names and levels match the SRD (2, 2, 3, 6, 10, 14). | High |

**Engine hint:** no engine reference found for: Bonus Cantrip, Circle Spells.

<a id="druid-circle-of-the-moon"></a>
### Circle of the Moon

- **Source:** PHB
- **App description:** A druid who fights as a beast, shifting fast and into far more dangerous shapes than most.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Combat Wild Shape | Wild Shape as a bonus action, and while transformed spend a spell slot as a bonus action to heal 1d8 per slot level. |
| 2 | Circle Forms | Wild Shape into beasts of challenge rating 1 at 2nd level, and up to a third of your druid level rounded down from 6th level on. |
| 6 | Primal Strike | Your beast form's attacks count as magical for overcoming resistance. |
| 10 | Elemental Wild Shape | Spend two Wild Shape uses at once to become an air, earth, fire or water elemental. |
| 14 | Thousand Forms | Cast alter self at will. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟠 Missing | L2 Circle Forms (beast-form AC) | Wild Shape into beasts of CR 1, rising to one third of your level. | While in beast form your AC can be 13 + Wisdom modifier if that's better than the beast's AC. The app text leaves out this AC rule. | High |

**Engine hint:** no engine reference found for: Circle Forms, Elemental Wild Shape, Thousand Forms.

<a id="druid-circle-of-dreams"></a>
### Circle of Dreams

- **Source:** Xanathar's Guide to Everything
- **App description:** A druid tied to the Summer Court, offering rest, safe camps and moonlit escape routes.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Balm of the Summer Court | A pool of d6s equal to your druid level, spendable as a bonus action to heal a creature within 120 feet and give it temporary hit points; refills on a long rest. |
| 6 | Hearth of Moonlight and Shadow | A 30-foot camp during a rest that grants concealment, silence, and a bonus to Perception and Stealth for everyone inside. |
| 10 | Hidden Paths | A bonus action teleports you 60 feet, or a creature you touch 30 feet, a Wisdom-modifier number of times per long rest. |
| 14 | Walker in Dreams | After a short rest, cast dream, scrying or teleportation circle without a slot, the circle keyed to a place you have seen. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Hearth of Moonlight and Shadow.

<a id="druid-circle-of-the-shepherd"></a>
### Circle of the Shepherd

- **Source:** Xanathar's Guide to Everything
- **App description:** A speaker for the spirits of beasts and fey who summons help and keeps it alive.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Speech of the Woods | You learn Sylvan and can speak with beasts. |
| 2 | Spirit Totem | A bonus action calls a 30-foot spirit aura for a minute, once per short or long rest: Bear grants temporary hit points and Strength advantage, Hawk grants attack advantage as a reaction and Perception advantage, Unicorn grants attack advantage against creatures in it and boosts your healing. |
| 6 | Mighty Summoner | Creatures you conjure gain 2 extra hit points per Hit Die and their attacks count as magical. |
| 10 | Guardian Spirit | Summoned creatures inside your Spirit Totem aura heal half your druid level at the end of your turns. |
| 14 | Faithful Summons | When you drop to 0 hit points, four dire wolves appear and defend you for an hour; recharges on a long rest. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L14 Faithful Summons | Four dire wolves appear and defend you for an hour. | You gain the benefits of conjure animals as if cast with a 9th-level slot, summoning four beasts of your choice of challenge rating 2 or lower (not fixed dire wolves), staying for an hour. | Medium |

**Engine hint:** no engine reference found for: Speech of the Woods, Mighty Summoner, Guardian Spirit.

<a id="druid-circle-of-spores"></a>
### Circle of Spores

- **Source:** Guildmasters' Guide to Ravnica
- **App description:** A druid of decay who sees rot as part of the cycle and wears a halo of necrotic spores.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Halo of Spores | A reaction deals 1d4 necrotic damage to a creature within 10 feet on a failed Constitution save, scaling with druid level. |
| 2 | Symbiotic Entity | Spend a Wild Shape use to gain four temporary hit points per druid level and add 1d6 necrotic to melee weapon hits for 10 minutes. |
| 6 | Fungal Infestation | A reaction reanimates a beast or humanoid that dies within 10 feet as a zombie under your command for an hour, a Wisdom-modifier number of times per long rest. |
| 10 | Spreading Spores | Hurl your spores into a 10-foot cube within 30 feet, damaging creatures that start their turn there instead of using Halo of Spores. |
| 14 | Fungal Body | Immunity to blindness, deafness, fear and poison, and critical hits against you count as normal hits. |

**Spells in the data:** L2: Chill Touch; L3: Blindness/Deafness, Gentle Repose; L5: Animate Dead, Gaseous Form; L7: Blight, Confusion; L9: Cloudkill, Contagion

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L10 Spreading Spores | Hurl spores into a 10-foot cube within 30 feet, damaging creatures that start there instead of using Halo of Spores. | It is a bonus action usable only while Symbiotic Entity is active, with a 10-foot cube within 30 ft for 1 minute; creatures that enter or start there make a Constitution save or take your Halo of Spores damage. The prerequisite is missing. | Medium |
| 2 | 🟡 Imprecise | L2 Halo of Spores | 1d4 necrotic, scaling with druid level. | 1d4, then 1d6 at 6th, 1d8 at 10th, 1d10 at 14th. | High |

**Engine hint:** no engine reference found for: Spreading Spores.

<a id="druid-circle-of-stars"></a>
### Circle of Stars

- **Source:** Tasha's Cauldron of Everything
- **App description:** A druid who reads the constellations and takes their shapes: archer, chalice and dragon.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Star Map | A charted map serving as your spellcasting focus; you know guidance and can cast guiding bolt a proficiency-bonus number of times per long rest without a slot. |
| 2 | Starry Form | Spend a Wild Shape use to glow with a constellation for 10 minutes: Archer gives a bonus-action ranged attack for 1d8 + Wisdom radiant damage, Chalice heals a second creature whenever you cast a healing spell, Dragon lets you treat a d20 of 9 or lower as a 10 on Intelligence and Wisdom checks and concentration saves. |
| 6 | Cosmic Omen | After a long rest, read your map for Weal or Woe; a reaction then adds or subtracts a d6 on a roll near you, a proficiency-bonus number of times per long rest. |
| 10 | Twinkling Constellations | Starry Form improves: Archer and Chalice roll 2d8, Dragon grants a 20-foot fly speed, and you may change constellation each turn. |
| 14 | Full of Stars | While in Starry Form you are partly incorporeal and resist bludgeoning, piercing and slashing damage. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Full of Stars.

<a id="druid-circle-of-wildfire"></a>
### Circle of Wildfire

- **Source:** Tasha's Cauldron of Everything
- **App description:** A druid bonded to a wildfire spirit, holding to the truth that fire clears ground for new growth.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Summon Wildfire Spirit | Spend a Wild Shape use to summon a wildfire spirit companion that acts on your turn, with a fiery teleport that damages creatures around its arrival. |
| 6 | Enhanced Bond | While the spirit is out, your fire and healing spells gain 1d8, and you may cast them from the spirit's position. |
| 10 | Cauterizing Flames | When a creature dies near you or the spirit, a flame springs up that either heals or burns the next creature to move through it for 2d10 + your Wisdom modifier. |
| 14 | Blazing Revival | Once per long rest, dropping to 0 hit points destroys the spirit and returns you to half your hit point maximum instead. |

**Spells in the data:** L2: Burning Hands, Cure Wounds; L3: Blur, Scorching Ray; L5: Revivify, Fireball; L7: Aura of Life, Fire Shield; L9: Flame Strike, Mass Cure Wounds

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | Circle Spells L3 | Blur, Scorching Ray. | Flaming Sphere, Scorching Ray. Blur is not a Wildfire spell. | Medium |
| 2 | 🔴 Wrong | Circle Spells L5 | Revivify, Fireball. | Plant Growth, Revivify. Fireball is not a Wildfire spell. | Medium |

## Fighter

Subclass chosen at level 3.

<a id="fighter-champion"></a>
### Champion

- **Source:** SRD 5.1 / PHB
- **App description:** A fighter of raw physical prowess: critical hits on a 19, remarkable athletics and a second fighting style.
- **Data file:** `class-features.json` (SRD, names only)

**As shipped**

| Level | Feature |
|---|---|
| 3 | Improved Critical |
| 7 | Remarkable Athlete |
| 10 | Additional Fighting Style |
| 15 | Superior Critical |
| 18 | Survivor |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Improved Critical, Remarkable Athlete, Additional Fighting Style, Superior Critical, Survivor. | Names and levels match (3, 7, 10, 15, 18). Descriptions are not in the subclass file. | High |

<a id="fighter-battle-master"></a>
### Battle Master

- **Source:** PHB
- **App description:** A student of martial technique who spends superiority dice on precise, tide-turning maneuvers.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Combat Superiority | Four superiority dice of d8, refilling on a short or long rest, spent on maneuvers with a save DC of 8 + proficiency + Strength or Dexterity. The dice grow to five at 7th level and six at 15th, and to d10 at 10th level and d12 at 18th. |
| 3 | Maneuvers | Three maneuvers known, such as Trip Attack, Disarming Attack, Riposte, Precision Attack, Menacing Attack and Commander's Strike, rising to five known at 7th level, seven at 10th and nine at 15th. |
| 3 | Student of War | Proficiency with one kind of artisan's tools. |
| 7 | Know Your Enemy | A minute of observation reveals whether a creature beats you in two chosen traits such as Strength, Dexterity, Constitution, AC, current hit points, class levels or fighter levels. |
| 15 | Relentless | When you roll initiative with no superiority dice left, you regain one. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L3 Combat Superiority / L10 / L18 | Improved Combat Superiority is folded into Combat Superiority. | Improved Combat Superiority is its own feature at 10th (d10) and 18th (d12). Dice (4/5/6) and maneuver counts (3/5/7/9) are correct in the text. | High |

<a id="fighter-eldritch-knight"></a>
### Eldritch Knight

- **Source:** PHB
- **App description:** A fighter who braids wizard magic into swordplay, casting and striking in the same breath.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Spellcasting (Eldritch Knight) | Third-caster wizard spellcasting keyed to Intelligence, drawn mostly from abjuration and evocation. |
| 3 | Weapon Bond | Bond up to two weapons in an hour-long ritual: you cannot be disarmed of them and can summon one to your hand as a bonus action. |
| 7 | War Magic | After casting a cantrip with your action, a bonus action makes one weapon attack. |
| 10 | Eldritch Strike | A weapon hit gives that creature disadvantage on its next save against a spell of yours before the end of your next turn. |
| 15 | Arcane Charge | Action Surge also teleports you up to 30 feet. |
| 18 | Improved War Magic | War Magic works after casting any spell, not only a cantrip. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Arcane Charge, Improved War Magic.

<a id="fighter-arcane-archer"></a>
### Arcane Archer

- **Source:** Xanathar's Guide to Everything
- **App description:** An elf-trained bowman who puts a spell on the arrow instead of in the air.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Arcane Archer Lore | The prestidigitation or druidcraft cantrip, and proficiency in Arcana or Nature. |
| 3 | Arcane Shot | Two Arcane Shot options known, used twice per short or long rest on an arrow hit: Banishing, Beguiling, Bursting, Enfeebling, Grasping, Piercing, Seeking or Shadow. More options are learned at 7th, 10th, 15th and 18th level. |
| 7 | Magic Arrow | Arrows you fire from a shortbow or longbow count as magical. |
| 7 | Curving Shot | A bonus action rerolls a missed magic-arrow attack against a different target within 60 feet. |
| 15 | Ever-Ready Shot | When you roll initiative with no Arcane Shot uses left, you regain one. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟠 Missing | L18 Arcane Shot damage | Not listed. | At 18th level the Arcane Shot damage increases (the 2d6 options become 4d6). The text mentions options at 18 but not the damage increase. | Medium |

<a id="fighter-cavalier"></a>
### Cavalier

- **Source:** Xanathar's Guide to Everything
- **App description:** A mounted champion and battlefield anchor who makes enemies regret walking past.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Bonus Proficiency (Cavalier) | Proficiency in Animal Handling, History, Insight, Performance or Persuasion, or one language of your choice. |
| 3 | Born to the Saddle | Advantage on saves to stay mounted, mounting costs only 5 feet of movement, and you land on your feet from a fall of 10 feet or less. |
| 3 | Unwavering Mark | A creature you hit in melee is marked until your next turn: it has disadvantage attacking anyone else, and on your next turn you can make a bonus attack against it that deals extra damage on a hit and gives it disadvantage on its next attack. Usable a Strength-modifier number of times per long rest. |
| 7 | Warding Maneuver | When you or a creature you can see within 5 feet is hit, a reaction adds 1d8 to the target's AC against that attack while you wield a melee weapon or a shield, and the target resists its damage if it still hits; a Constitution-modifier number of times per long rest. |
| 10 | Hold the Line | A reaction makes an opportunity attack against a creature that moves 5 feet or more while in your reach, stopping it dead. |
| 15 | Ferocious Charger | Once per turn, a hit after moving 10 feet in a straight line knocks a Large or smaller creature prone on a failed Strength save. |
| 18 | Vigilant Defender | You get a reaction opportunity attack on every other creature's turn, not just once per round. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L10 Hold the Line | Opportunity attack against a creature that moves within your reach, stopping it dead. | Creatures provoke an opportunity attack from you when they move 5 ft or more while within your reach, even if they Disengage; on a hit the creature's speed becomes 0 for the turn. | High |

**Engine hint:** no engine reference found for: Ferocious Charger.

<a id="fighter-samurai"></a>
### Samurai

- **Source:** Xanathar's Guide to Everything
- **App description:** A relentless warrior whose sheer conviction keeps them upright long past the point of sense.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Bonus Proficiency (Samurai) | Proficiency in History, Insight, Performance or Persuasion, or one language of your choice. |
| 3 | Fighting Spirit | A bonus action grants advantage on weapon attacks this turn and 5 temporary hit points, rising to 10 at 10th level and 15 at 15th; three uses per long rest. |
| 7 | Elegant Courtier | Add your Wisdom modifier to Persuasion checks, and gain proficiency in Wisdom saving throws (Intelligence or Charisma if you already have it). |
| 10 | Tireless Spirit | When you roll initiative with no Fighting Spirit uses left, you regain one. |
| 15 | Rapid Strike | Trade advantage on one attack for an extra attack against the same target. |
| 18 | Strength before Death | Once per long rest, a reaction on taking damage lets you act immediately before the damage is applied. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L18 Strength before Death | Once per long rest, a reaction on taking damage lets you act immediately before the damage is applied. | When damage reduces you to 0 hit points, you use your reaction to delay falling unconscious and immediately take an extra turn. During it, damage causes death save failures and you fall unconscious at the end of the turn if still at 0. It is not 'before the damage is applied'. | High |

<a id="fighter-echo-knight"></a>
### Echo Knight

- **Source:** Explorer's Guide to Wildemount
- **App description:** A fighter haunted by a shard of another timeline, fighting beside a grey echo of themselves.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Manifest Echo | A bonus action summons a magical echo of yourself within 15 feet with 1 hit point and your AC; you may attack from its space, swap places with it, and move it 30 feet as a bonus action. |
| 3 | Unleash Incarnation | Make one extra melee attack from your echo's space when you take the Attack action, a Constitution-modifier number of times per long rest. |
| 7 | Echo Avatar | Project your senses into the echo and send it up to 1,000 feet away for 10 minutes. |
| 10 | Shadow Martyr | A reaction makes your echo take an attack aimed at another creature; once per short or long rest. |
| 15 | Reclaim Potential | When your echo is destroyed by damage, gain temporary hit points of 2d6 + your Constitution modifier, a Constitution-modifier number of times per long rest. |
| 18 | Legion of One | Regain a use of Unleash Incarnation on initiative, and manifest two echoes at once. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Echo Avatar.

<a id="fighter-psi-warrior"></a>
### Psi Warrior

- **Source:** Tasha's Cauldron of Everything
- **App description:** A soldier whose mind is the weapon: telekinetic shoves, force shields and psionic strikes.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Psionic Power (Psi Warrior) | A pool of Psionic Energy dice, twice your proficiency bonus per long rest, starting at d6 and growing with fighter level; one die returns on a short rest. |
| 3 | Protective Field | A reaction spends a die to reduce damage to you or an ally within 30 feet by the die plus your Intelligence modifier. |
| 3 | Psionic Strike | Once per turn, spend a die to add it plus your Intelligence modifier as force damage to a hit. |
| 3 | Telekinetic Movement | Move a Large or smaller object or willing creature 30 feet with your mind, free once per short rest or by spending a die. |
| 7 | Telekinetic Adept | Psi-Powered Leap lets you fly briefly as a bonus action, and Telekinetic Thrust can knock a Psionic Strike target prone or push it 10 feet. |
| 10 | Guarded Mind | Resistance to psychic damage, and a reaction-free end to being charmed or frightened by spending a die. |
| 15 | Bulwark of Force | A bonus action gives yourself and up to your Intelligence modifier in allies half cover for a minute; once per long rest or by spending a die. |
| 18 | Telekinetic Master | Cast telekinesis for free once per long rest, and make one weapon attack as a bonus action while concentrating on it. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Guarded Mind.

<a id="fighter-rune-knight"></a>
### Rune Knight

- **Source:** Tasha's Cauldron of Everything
- **App description:** A warrior taught giant runecraft, carving power into gear and growing to giant size at need.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Bonus Proficiencies (Rune Knight) | Smith's tools and the Giant language. |
| 3 | Rune Carver | Inscribe runes on your gear, two at 3rd level and more at 7th, 10th and 15th: Cloud, Fire, Frost, Stone, Hill and Storm, each with a passive benefit and an invokable effect once per short or long rest. |
| 3 | Giant's Might | A bonus action for a minute makes you Large, gives advantage on Strength checks and saves, and adds 1d6 to one damage roll per turn; a proficiency-bonus number of times per long rest. |
| 7 | Runic Shield | A reaction forces an attacker to reroll a hit against an ally within 60 feet, a proficiency-bonus number of times per long rest. |
| 10 | Great Stature | You grow 3d4 inches permanently and Giant's Might's bonus damage becomes 1d8. |
| 15 | Master of Runes | Invoke each rune twice per short or long rest. |
| 18 | Runic Juggernaut | Giant's Might deals 1d10 and can make you Huge with 5 extra feet of reach. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Master of Runes.

<a id="fighter-banneret"></a>
### Banneret

- **Source:** Sword Coast Adventurer's Guide (Purple Dragon Knight)
- **App description:** A knight-commander whose presence steadies a whole unit and spreads their own resilience around.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Rallying Cry | Second Wind also heals up to three allies within 60 feet for your fighter level. |
| 7 | Royal Envoy | Proficiency and expertise in Persuasion, plus one extra language. |
| 10 | Inspiring Surge | Action Surge also lets an ally within 60 feet use a reaction to make one weapon attack, two allies from 18th level. |
| 15 | Bulwark | Indomitable also lets one ally within 60 feet reroll a failed ability check. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L7 Royal Envoy | Proficiency and expertise in Persuasion, plus one extra language. | Proficiency in Persuasion, or expertise if already proficient; plus one extra language. Not both at once. | Medium |

**Engine hint:** no engine reference found for: Rallying Cry.

## Monk

Subclass chosen at level 3.

<a id="monk-way-of-the-open-hand"></a>
### Way of the Open Hand

- **Source:** SRD 5.1 / PHB
- **App description:** A monk who masters unarmed combat, knocking foes down and away with every flurry and healing with ki.
- **Data file:** `class-features.json` (SRD, names only)

**As shipped**

| Level | Feature |
|---|---|
| 3 | Open Hand Technique |
| 6 | Wholeness of Body |
| 11 | Tranquility |
| 17 | Quivering Palm |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Open Hand Technique, Wholeness of Body, Tranquility, Quivering Palm. | Names and levels match (3, 6, 11, 17). | High |

<a id="monk-way-of-shadow"></a>
### Way of Shadow

- **Source:** PHB
- **App description:** A ninja-monk who bends darkness, steps between shadows and strikes from nowhere.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Shadow Arts | Spend 2 ki to cast darkness, darkvision, pass without trace or silence, and you know minor illusion. |
| 6 | Shadow Step | A bonus action teleports 60 feet between two patches of dim light or darkness, granting advantage on your next melee attack this turn. |
| 11 | Cloak of Shadows | Become invisible in dim light or darkness until you attack, cast, or step into light. |
| 17 | Opportunist | A reaction makes a melee attack against a creature within 5 feet that another creature just hit. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Shadow Arts.

<a id="monk-way-of-the-four-elements"></a>
### Way of the Four Elements

- **Source:** PHB
- **App description:** A monk who shapes ki into raw elemental magic: fists of fire, gusts of wind, walls of stone.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Disciple of the Elements | Learn elemental disciplines cast by spending ki, starting with Elemental Attunement and one other, and gaining more at 6th, 11th and 17th level; spell save DC and attacks use your ki save DC. |
| 6 | Elemental Discipline (6th) | One additional elemental discipline of a level your ki can pay for. |
| 11 | Elemental Discipline (11th) | One additional elemental discipline. |
| 17 | Elemental Discipline (17th) | One additional elemental discipline, up to the most powerful ones. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

<a id="monk-way-of-the-drunken-master"></a>
### Way of the Drunken Master

- **Source:** Xanathar's Guide to Everything
- **App description:** A monk who fights like a stumbling drunk and is never where the blow lands.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Bonus Proficiencies (Drunken Master) | Performance and brewer's supplies. |
| 3 | Drunken Technique | Flurry of Blows also grants the benefit of Disengage and 10 feet of extra movement. |
| 6 | Tipsy Sway | Leap to your feet from prone at the cost of 5 feet of movement, and spend 1 ki as a reaction to redirect a miss against you into another creature within 5 feet. |
| 11 | Drunkard's Luck | Spend 2 ki to cancel disadvantage on an attack, check or save. |
| 17 | Intoxicated Frenzy | Flurry of Blows grants up to three extra attacks, provided each one hits a different creature. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Drunken Technique.

<a id="monk-way-of-the-kensei"></a>
### Way of the Kensei

- **Source:** Xanathar's Guide to Everything
- **App description:** A weapon-saint who treats a chosen blade or bow as an extension of their unarmed art.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Path of the Kensei | Two kensei weapons count as monk weapons, one ranged kensei weapon adds +2 AC as a bonus action, and an unarmed strike as a bonus action follows a kensei attack. |
| 3 | Kensei's Shot | A bonus action adds 1d4 damage to ranged kensei attacks this turn. |
| 6 | One with the Blade | Kensei weapons count as magical, and 1 ki adds your martial arts die to a hit's damage once per turn. |
| 11 | Sharpen the Blade | Spend up to 3 ki as a bonus action for a matching bonus to attack and damage with a kensei weapon for a minute. |
| 17 | Unerring Accuracy | Reroll a missed monk-weapon attack once per turn. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L3 Path of the Kensei | Two kensei weapons count as monk weapons, one ranged kensei weapon adds +2 AC as a bonus action, and an unarmed strike as a bonus action follows a kensei attack. | Agile Parry: when you make an unarmed strike as part of the Attack action while wielding a kensei weapon, you gain +2 AC until the start of your next turn. It has nothing to do with ranged weapons or a bonus action. The bonus-action unarmed strike with a kensei weapon attack is also not part of the rules; only the monk's own Martial Arts bonus strike applies. | High |
| 2 | 🟠 Missing | L3 Kensei Weapons / Agile Parry | Agile Parry is not named. | The three level-3 pieces are Kensei Weapons (two: one melee, one ranged), Agile Parry and Kensei's Shot. | High |

**Engine hint:** no engine reference found for: One with the Blade, Unerring Accuracy.

<a id="monk-way-of-the-sun-soul"></a>
### Way of the Sun Soul

- **Source:** Xanathar's Guide to Everything
- **App description:** A monk who burns ki as radiant light, striking at range and flaring like a small sun.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Radiant Sun Bolt | A ranged spell attack for your martial arts die plus Dexterity in radiant damage at 30 feet, usable with the Attack action and extendable with ki as a bonus action. |
| 6 | Searing Arc Strike | After the Attack action, spend 2 ki or more as a bonus action to cast burning hands at a matching level. |
| 11 | Searing Sunburst | A 20-foot sphere within 150 feet deals 2d6 radiant on a failed Constitution save, up to 2d6 more per ki point spent. |
| 17 | Sun Shield | Shed 30 feet of bright light at will and deal 5 + Wisdom radiant damage to anyone who hits you in melee. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Sun Shield.

<a id="monk-way-of-the-long-death"></a>
### Way of the Long Death

- **Source:** Sword Coast Adventurer's Guide
- **App description:** A monk who studies dying itself, drawing strength from every death nearby.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Touch of Death | When a creature within 5 feet drops to 0 hit points, gain temporary hit points equal to your Wisdom modifier plus your monk level. |
| 6 | Hour of Reaping | An action frightens every creature within 30 feet that fails a Wisdom save until the end of your next turn. |
| 11 | Mastery of Death | Spend 1 ki to stay at 1 hit point instead of dropping to 0, as long as ki remains. |
| 17 | Touch of the Long Death | Spend up to 10 ki to deal 2d10 necrotic per point to a creature within 5 feet, halved on a Constitution save. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Hour of Reaping, Mastery of Death.

<a id="monk-way-of-mercy"></a>
### Way of Mercy

- **Source:** Tasha's Cauldron of Everything
- **App description:** A masked healer-assassin whose hands can close a wound or stop a heart with the same touch.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Implements of Mercy | Proficiency in Insight and Medicine and with a herbalism kit, plus a mask you are known by. |
| 3 | Hand of Healing | Spend 1 ki as an action to heal a creature within 5 feet for your martial arts die plus your Wisdom modifier, or replace one Flurry of Blows attack with it. |
| 3 | Hand of Harm | Once per turn, spend 1 ki on an unarmed hit for extra necrotic damage of your martial arts die plus your Wisdom modifier. |
| 6 | Physician's Touch | Hand of Healing also ends one condition; Hand of Harm can poison the target until the end of your next turn. |
| 11 | Flurry of Healing and Harm | Flurry of Blows can heal without ki cost, and one of its attacks can carry Hand of Harm free. |
| 17 | Hand of Ultimate Mercy | Once per long rest, spend 5 ki to return a creature dead no more than 24 hours to life with 4d10 + Wisdom hit points. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L11 Flurry of Healing and Harm | Flurry of Blows can heal without ki cost, and one of its attacks can carry Hand of Harm free. | When you use Flurry of Blows, you can replace EACH unarmed strike with Hand of Healing at no ki, and each strike can deal Hand of Harm at no ki, up to your Wisdom modifier times per long rest (min 1). 'One of its attacks' and the use limit are wrong or missing. | Medium |

**Engine hint:** no engine reference found for: Hand of Healing, Hand of Harm, Physician's Touch, Flurry of Healing and Harm.

<a id="monk-way-of-the-astral-self"></a>
### Way of the Astral Self

- **Source:** Tasha's Cauldron of Everything
- **App description:** A monk who summons their own spirit outward as glowing arms, a watching visage and a full body.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Arms of the Astral Self | Spend 1 ki as a bonus action to summon spectral arms for 10 minutes: they use Wisdom for unarmed attacks, deal your martial arts die in force damage, add 5 feet of reach, and can shove creatures within 10 feet. |
| 6 | Visage of the Astral Self | Spend 1 ki for a spirit visage granting darkvision, advantage on Insight and Intimidation, and an Astral Sight that pierces darkness. |
| 11 | Body of the Astral Self | With both arms and visage active, gain a Deflect Energy reaction and add 1d10 to a damage roll once per turn. |
| 17 | Awakened Astral Self | Spend 5 ki for the complete astral body: +2 AC and an extra Arms attack when taking the Attack action. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L3 Arms of the Astral Self | Arms use Wisdom, add 5 feet of reach, shove creatures within 10 feet. | The arms use Wisdom for attacks and damage (force), and let you make a special Arms of the Astral Self attack: when you take the Attack action, creatures of your choice within 10 ft make a Dexterity save or take force damage equal to your martial arts die. 'Shove' is not part of it. | Low |

<a id="monk-way-of-the-ascendant-dragon"></a>
### Way of the Ascendant Dragon

- **Source:** Fizban's Treasury of Dragons
- **App description:** A monk who studies dragons until their ki breathes, roars and flies like one.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Draconic Disciple | You know Draconic, can shift your unarmed damage to acid, cold, fire, lightning or poison, and can add a d4 to a Charisma check once per short rest. |
| 3 | Breath of the Dragon | Replace one Attack-action attack with a 20-foot cone or 30-foot line dealing two rolls of your martial arts die in damage, a proficiency-bonus number of times per long rest, or for 2 ki when none are left. |
| 6 | Wings Unfurled | While airborne from a jump or effect, gain a flying speed equal to your walking speed, a proficiency-bonus number of times per long rest. |
| 11 | Aspect of the Wyrm | A bonus action creates a 10-foot aura for a minute that either frightens enemies or grants allies resistance to your damage type; once per long rest or for 3 ki. |
| 17 | Ascendant Aspect | Aspect of the Wyrm reaches 30 feet, grants blindsight, and once per turn lets you deal 3d10 damage to up to three creatures in it as a reaction. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Draconic Disciple.

## Paladin

Subclass chosen at level 3.

<a id="paladin-oath-of-devotion"></a>
### Oath of Devotion

- **Source:** SRD 5.1 / PHB
- **App description:** A paladin sworn to honesty, courage and compassion, with a sacred weapon and protection from fiends and undead.
- **Data file:** `class-features.json` (SRD, names only)

**As shipped**

| Level | Feature |
|---|---|
| 3 | Channel Divinity: Sacred Weapon |
| 3 | Channel Divinity: Turn the Unholy |
| 7 | Aura of Devotion |
| 15 | Purity of Spirit |
| 20 | Holy Nimbus |

**Spells in the data:** L3: Protection from Evil and Good, Sanctuary; L5: Lesser Restoration, Zone of Truth; L9: Beacon of Hope, Dispel Magic; L13: Freedom of Movement, Guardian of Faith; L17: Commune, Flame Strike

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | All levels (SRD) | Names only; oath spells shipped and correct. | Sacred Weapon and Turn the Unholy (3), Aura of Devotion (7), Purity of Spirit (15), Holy Nimbus (20). Oath spells match the SRD. | High |

<a id="paladin-oath-of-the-ancients"></a>
### Oath of the Ancients

- **Source:** PHB
- **App description:** A paladin sworn to light and life rather than law, shielding what is good from despair.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Channel Divinity: Nature's Wrath | Spectral vines restrain a creature within 10 feet that fails a Strength or Dexterity save. |
| 3 | Channel Divinity: Turn the Faithless | Fey and fiends within 30 feet that fail a Wisdom save are turned for a minute, and their disguises drop. |
| 7 | Aura of Warding | You and allies within 10 feet have resistance to damage from spells, growing to 30 feet at 18th level. |
| 15 | Undying Sentinel | Once per long rest, drop to 1 hit point instead of 0, and you no longer age or weaken with time. |
| 20 | Elder Champion | For a minute: regain 10 hit points each turn, cast paladin spells as a bonus action, and enemies within 10 feet have disadvantage on saves against your spells and Channel Divinity. |

**Spells in the data:** L3: Ensnaring Strike, Speak with Animals; L5: Moonbeam, Misty Step; L9: Plant Growth, Protection from Energy; L13: Ice Storm, Stoneskin; L17: Commune with Nature, Tree Stride

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

<a id="paladin-oath-of-vengeance"></a>
### Oath of Vengeance

- **Source:** PHB
- **App description:** A paladin who has chosen one wrong to punish and will spend everything to reach it.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Channel Divinity: Abjure Enemy | One creature within 60 feet is frightened and its speed drops to 0 for a minute on a failed Wisdom save, half that on a success. |
| 3 | Channel Divinity: Vow of Enmity | A bonus action grants advantage on attacks against one creature within 10 feet for a minute. |
| 7 | Relentless Avenger | An opportunity-attack hit lets you move up to half your speed as part of the reaction, without provoking. |
| 15 | Soul of Vengeance | A reaction makes a melee attack against a creature under your Vow of Enmity when it attacks. |
| 20 | Avenging Angel | For an hour, gain a 60-foot flying speed and a 30-foot aura that frightens enemies who fail a Wisdom save, giving you advantage against them. |

**Spells in the data:** L3: Bane, Hunter's Mark; L5: Hold Person, Misty Step; L9: Haste, Protection from Energy; L13: Banishment, Dimension Door; L17: Hold Monster, Scrying

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

<a id="paladin-oath-of-conquest"></a>
### Oath of Conquest

- **Source:** Xanathar's Guide to Everything
- **App description:** A paladin who believes order is imposed, and rules the battlefield through fear.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Channel Divinity: Conquering Presence | Every creature within 30 feet that fails a Wisdom save is frightened for a minute, saving again each turn. |
| 3 | Channel Divinity: Guided Strike | Add +10 to one attack roll after seeing the result. |
| 7 | Aura of Conquest | Frightened creatures within 10 feet have their speed reduced to 0 and take psychic damage equal to half your paladin level each turn; 30 feet at 18th level. |
| 15 | Scornful Rebuke | A creature that hits you takes psychic damage equal to your Charisma modifier. |
| 20 | Invincible Conqueror | For a minute: resistance to all damage, an extra attack with the Attack action, and melee critical hits on a 19 or 20. |

**Spells in the data:** L3: Armor of Agathys, Command; L5: Hold Person, Spiritual Weapon; L9: Bestow Curse, Fear; L13: Dominate Beast, Stoneskin; L17: Cloudkill, Dominate Person

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Scornful Rebuke.

<a id="paladin-oath-of-redemption"></a>
### Oath of Redemption

- **Source:** Xanathar's Guide to Everything
- **App description:** A paladin who fights only when talking fails, and takes the blows meant for others.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Channel Divinity: Emissary of Peace | A +5 bonus to Persuasion checks for 10 minutes. |
| 3 | Channel Divinity: Rebuke the Violent | A reaction deals the same damage back to an attacker who wounded someone near you, halved on a Wisdom save. |
| 7 | Aura of the Guardian | A reaction takes the damage aimed at an ally within 10 feet onto yourself, ignoring resistance; 30 feet at 18th level. |
| 15 | Protective Spirit | Regain 1d6 + half your paladin level hit points at the end of your turn in combat when below half your maximum. |
| 20 | Emissary of Redemption | Resistance to all damage from other creatures, and attackers take radiant damage equal to half of what they dealt you, until you attack or harm someone. |

**Spells in the data:** L3: Sanctuary, Sleep; L5: Calm Emotions, Hold Person; L9: Counterspell, Hypnotic Pattern; L13: Otiluke's Resilient Sphere, Stoneskin; L17: Hold Monster, Wall of Force

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

<a id="paladin-oath-of-glory"></a>
### Oath of Glory

- **Source:** Mythic Odysseys of Theros / Tasha's
- **App description:** A paladin who chases the heroic deed itself, and drags the whole party up to that standard.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Channel Divinity: Peerless Athlete | For 10 minutes, advantage on Athletics and Acrobatics, extra carrying capacity, and 10 extra feet of jump distance. |
| 3 | Channel Divinity: Inspiring Smite | After a Divine Smite, hand out temporary hit points of 2d8 + paladin level split among creatures within 30 feet. |
| 7 | Aura of Alacrity | Your speed rises by 10 feet, and allies within 5 feet gain 10 feet of speed until their next turn; 10 feet of aura at 18th level. |
| 15 | Glorious Defense | A reaction adds your Charisma modifier to the AC of yourself or an ally within 10 feet, and a resulting miss lets you attack the attacker; a Charisma-modifier number of times per long rest. |
| 20 | Living Legend | For a minute: advantage on Charisma checks, a missed attack can be rerolled once per turn, and failed saves can be turned into successes once per turn. |

**Spells in the data:** L3: Guiding Bolt, Heroism; L5: Enhance Ability, Magic Weapon; L9: Haste, Protection from Energy; L13: Compulsion, Freedom of Movement; L17: Commune, Flame Strike

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

<a id="paladin-oath-of-the-watchers"></a>
### Oath of the Watchers

- **Source:** Tasha's Cauldron of Everything
- **App description:** A paladin trained to detect and destroy extraplanar intruders before they take root.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Channel Divinity: Watcher's Will | You and up to your Charisma modifier in allies gain advantage on Intelligence, Wisdom and Charisma saves for a minute. |
| 3 | Channel Divinity: Abjure the Extraplanar | Aberrations, celestials, elementals, fey and fiends within 30 feet that fail a Wisdom save are turned for a minute. |
| 7 | Aura of the Sentinel | You and allies within 10 feet add your proficiency bonus to initiative; 30 feet at 18th level. |
| 15 | Vigilant Rebuke | A reaction deals 2d8 + Charisma force damage to a creature whose spell or effect you just saved against. |
| 20 | Mortal Bulwark | For a minute: truesight to 120 feet, advantage on attacks against aberrations, celestials, elementals, fey and fiends, and a hit can banish one to its home plane on a failed Charisma save. |

**Spells in the data:** L3: Alarm, Detect Magic; L5: Moonbeam, See Invisibility; L9: Counterspell, Nondetection; L13: Aura of Purity, Banishment; L17: Hold Monster, Scrying

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Aura of the Sentinel.

<a id="paladin-oath-of-the-crown"></a>
### Oath of the Crown

- **Source:** Sword Coast Adventurer's Guide
- **App description:** A paladin sworn to a realm and its law, holding the line so others can fall back.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Channel Divinity: Champion Challenge | Creatures of your choice within 30 feet that fail a Wisdom save cannot move more than 30 feet away from you. |
| 3 | Channel Divinity: Turn the Tide | Creatures of your choice within 30 feet below half hit points regain 1d6 + your Charisma modifier hit points. |
| 7 | Divine Allegiance | A reaction takes the damage aimed at a creature within 5 feet onto yourself, ignoring resistance. |
| 15 | Unyielding Spirit | Advantage on saving throws against being paralyzed or stunned. |
| 20 | Exalted Champion | For an hour: resistance to nonmagical weapon damage, advantage on Wisdom saves for you and allies within 30 feet, and death saves at advantage for the same. |

**Spells in the data:** L3: Command, Compelled Duel; L5: Warding Bond, Zone of Truth; L9: Aura of Vitality, Spirit Guardians; L13: Banishment, Guardian of Faith; L17: Circle of Power, Geas

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

<a id="paladin-oathbreaker"></a>
### Oathbreaker

- **Source:** Dungeon Master's Guide
- **App description:** A paladin who broke their oath for darker power and now commands the undead. Usually a villain.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Channel Divinity: Control Undead | One undead of CR at or below your level within 30 feet obeys you for 24 hours on a failed Charisma save. |
| 3 | Channel Divinity: Dreadful Aspect | Creatures of your choice within 30 feet that fail a Wisdom save are frightened for a minute. |
| 7 | Aura of Hate | You, fiends and undead within 10 feet add your Charisma modifier to melee weapon damage; 30 feet at 18th level. |
| 15 | Supernatural Resistance | Resistance to nonmagical bludgeoning, piercing and slashing damage. |
| 20 | Dread Lord | For a minute: a 30-foot aura of magical darkness that deals 4d10 psychic damage to enemies who start their turn in it, gives attackers disadvantage against you, and lets you make a spectral 4d10 psychic attack as a bonus action. |

**Spells in the data:** L3: Hellish Rebuke, Inflict Wounds; L5: Crown of Madness, Darkness; L9: Animate Dead, Bestow Curse; L13: Blight, Confusion; L17: Contagion, Dominate Person

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Aura of Hate, Supernatural Resistance.

## Ranger

Subclass chosen at level 3.

<a id="ranger-hunter"></a>
### Hunter

- **Source:** SRD 5.1 / PHB
- **App description:** A ranger who studies prey and picks a fighting technique to bring down hordes or giants.
- **Data file:** `class-features.json` (SRD, names only)

**As shipped**

| Level | Feature |
|---|---|
| 3 | Hunter's Prey |
| 7 | Defensive Tactics |
| 11 | Multiattack |
| 15 | Superior Hunter's Defense |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Hunter's Prey, Defensive Tactics, Multiattack, Superior Hunter's Defense. | Names and levels match (3, 7, 11, 15). The options inside Hunter's Prey, Defensive Tactics and Multiattack are tracked as picks elsewhere. | High |

<a id="ranger-beast-master"></a>
### Beast Master

- **Source:** PHB
- **App description:** A ranger who fights alongside a bonded animal companion that acts on their command.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Ranger's Companion | A beast of CR 1/4 or lower obeys your commands, adds your proficiency bonus to its rolls and AC, uses your hit points formula and takes the Dodge action unless you command otherwise. |
| 7 | Exceptional Training | A bonus action lets the companion Dash, Disengage, Dodge or Help, and its attacks count as magical. |
| 11 | Bestial Fury | Your companion makes two attacks when you command it to attack. |
| 15 | Share Spells | A spell you cast on yourself also affects your companion within 30 feet. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟠 Missing | Tasha's optional companion | Only the original Ranger's Companion. | Tasha's replaces it with Primal Companion (Beast of the Land, Sea or Sky). That optional version is not offered. | High |

**Engine hint:** no engine reference found for: Exceptional Training, Bestial Fury, Share Spells.

<a id="ranger-gloom-stalker"></a>
### Gloom Stalker

- **Source:** Xanathar's Guide to Everything
- **App description:** A ranger of the lightless places who is deadliest in the first seconds of a fight.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Dread Ambusher | + your Wisdom modifier to initiative, and on your first turn 10 extra feet of speed plus one extra attack dealing an extra 1d8. |
| 3 | Umbral Sight | Darkvision 60 feet, or 30 more if you had it, and you are invisible to creatures relying on darkvision to see you in darkness. |
| 7 | Iron Mind | Proficiency in Wisdom saves, or Intelligence or Charisma if you already have it. |
| 11 | Stalker's Flurry | Once per turn, a missed attack lets you make another one. |
| 15 | Shadowy Dodge | A reaction imposes disadvantage on an attack against you that would otherwise hit. |

**Spells in the data:** L3: Disguise Self; L5: Rope Trick; L9: Fear; L13: Greater Invisibility; L17: Seeming

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Umbral Sight, Stalker's Flurry.

<a id="ranger-horizon-walker"></a>
### Horizon Walker

- **Source:** Xanathar's Guide to Everything
- **App description:** A ranger who patrols the seams between planes and steps through them mid-fight.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Detect Portal | Sense a planar portal within a mile once per short or long rest. |
| 3 | Planar Warrior | A bonus action marks a target so your next hit this turn deals its weapon damage plus 1d8 force as force damage; 2d8 from 11th level. |
| 7 | Ethereal Step | Cast etherealness as a bonus action for one turn, once per short or long rest. |
| 11 | Distant Strike | Teleport 10 feet before each attack, and attack a third creature if you struck two others this turn. |
| 15 | Spectral Defense | A reaction halves the damage of an attack that hits you. |

**Spells in the data:** L3: Protection from Evil and Good; L5: Misty Step; L9: Haste; L13: Banishment; L17: Teleportation Circle

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Distant Strike.

<a id="ranger-monster-slayer"></a>
### Monster Slayer

- **Source:** Xanathar's Guide to Everything
- **App description:** A ranger trained on the things that hunt people: their tricks, their saves, their weaknesses.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Hunter's Sense | An action reveals a creature's damage immunities, resistances and vulnerabilities, a Wisdom-modifier number of times per long rest. |
| 3 | Slayer's Prey | A bonus action marks a creature so your first hit against it each turn deals an extra 1d6. |
| 7 | Supernatural Defense | Add 1d6 to saves and escape checks against effects from your Slayer's Prey target. |
| 11 | Magic-User's Nemesis | A reaction forces a caster or teleporter within 60 feet to make a Wisdom save or lose the spell; once per short or long rest. |
| 15 | Slayer's Counter | A reaction attacks your prey when it forces a save on you, and a hit means the save automatically succeeds. |

**Spells in the data:** L3: Protection from Evil and Good; L5: Zone of Truth; L9: Magic Circle; L13: Dimension Door; L17: Hold Monster

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | Monster Slayer Magic L13 | Dimension Door. | Banishment. The list is Protection from Evil and Good (3), Zone of Truth (5), Magic Circle (9), Banishment (13), Hold Monster (17). | Medium |

<a id="ranger-fey-wanderer"></a>
### Fey Wanderer

- **Source:** Tasha's Cauldron of Everything
- **App description:** A ranger carrying a piece of the Feywild: a strange mien, cutting words and psychic strikes.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Dreadful Strikes | Once per turn, a weapon hit deals an extra 1d4 psychic damage, rising to 1d6 at 11th level. |
| 3 | Otherworldly Glamour | Add your Wisdom modifier to Charisma checks, and gain proficiency in Deception, Performance or Persuasion. |
| 3 | Fey Wanderer Magic | Charm person and other fey spells are always prepared and do not count against your spells known. |
| 7 | Beguiling Twist | Advantage on saves against being charmed or frightened, and a reaction redirects a nearby charm or fear onto another creature. |
| 11 | Fey Reinforcements | Summon fey is always prepared and can be cast once per long rest without a slot, with the summon able to turn invisible. |
| 15 | Misty Wanderer | Cast misty step without a slot a Wisdom-modifier number of times per long rest, bringing a willing creature within 5 feet along. |

**Spells in the data:** L3: Charm Person; L5: Misty Step; L9: Dispel Magic; L13: Dimension Door; L17: Mislead

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Dreadful Strikes, Fey Wanderer Magic.

<a id="ranger-swarmkeeper"></a>
### Swarmkeeper

- **Source:** Tasha's Cauldron of Everything
- **App description:** A ranger accompanied by a bound swarm of tiny nature spirits that bite, shove and carry.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Gathered Swarm | Once per turn, a hit lets your swarm deal 1d6 piercing damage, move the target 15 feet, or move you 5 feet without provoking; 1d8 from 11th level. |
| 3 | Swarmkeeper Magic | Mage hand and a short list of swarm spells are always prepared and do not count against your spells known. |
| 7 | Writhing Tide | A bonus action grants a 10-foot flying speed for a minute, a proficiency-bonus number of times per long rest. |
| 11 | Mighty Swarm | Gathered Swarm knocks moved targets prone, gives you half cover when it moves you, and adds 1d6 to its damage. |
| 15 | Swarming Dispersal | A reaction gives resistance to damage taken and teleports you 30 feet, a proficiency-bonus number of times per long rest. |

**Spells in the data:** L3: Faerie Fire; L5: Web; L9: Gaseous Form; L13: Arcane Eye; L17: Insect Plague

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Swarmkeeper Magic.

<a id="ranger-drakewarden"></a>
### Drakewarden

- **Source:** Fizban's Treasury of Dragons
- **App description:** A ranger bonded to a young draconic spirit that grows from a helper into a mount.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Draconic Gift | You know thaumaturgy and can speak Draconic. |
| 3 | Drake Companion | Summon a Small drake once per long rest or with a spell slot; it acts on your turn, adds your proficiency bonus to its rolls, and has an elemental breath. |
| 7 | Bond of Fang and Scale | While the drake is out, you gain resistance to its damage type, the drake gains a fly speed, and your attacks count as magical with extra elemental damage. |
| 11 | Drake's Breath | Once per long rest, the drake exhales a 30-foot cone dealing 8d6 of its element, halved on a Dexterity save. |
| 15 | Perfected Bond | The drake becomes Large and rideable, gains extra hit points, and adds your proficiency bonus in damage to its attacks. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Draconic Gift, Bond of Fang and Scale, Perfected Bond.

## Rogue

Subclass chosen at level 3.

<a id="rogue-thief"></a>
### Thief

- **Source:** SRD 5.1 / PHB
- **App description:** A rogue of quick hands and quick feet: faster climbing, items used as a bonus action, and stealth on the move.
- **Data file:** `class-features.json` (SRD, names only)

**As shipped**

| Level | Feature |
|---|---|
| 3 | Fast Hands |
| 3 | Second-Story Work |
| 9 | Supreme Sneak |
| 13 | Use Magic Device |
| 17 | Thief's Reflexes |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Fast Hands, Second-Story Work, Supreme Sneak, Use Magic Device, Thief's Reflexes. | Names and levels match (3, 3, 9, 13, 17). | High |

**Engine hint:** no engine reference found for: Use Magic Device.

<a id="rogue-assassin"></a>
### Assassin

- **Source:** PHB
- **App description:** A killer who opens fights already having won them, and wears other people's faces to get close.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Bonus Proficiencies (Assassin) | Proficiency with the disguise kit and the poisoner's kit. |
| 3 | Assassinate | Advantage on attacks against any creature that has not taken a turn yet, and any hit against a surprised creature is a critical hit. |
| 9 | Infiltration Expertise | Spend 25 gp and seven days to build a false identity you can adopt convincingly. |
| 13 | Impostor | After studying someone for three hours, mimic their speech, writing and behaviour well enough to fool most observers. |
| 17 | Death Strike | When you hit a surprised creature, it makes a Constitution save against 8 + Dexterity + proficiency or takes double damage. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Infiltration Expertise, Impostor, Death Strike.

<a id="rogue-arcane-trickster"></a>
### Arcane Trickster

- **Source:** PHB
- **App description:** A thief who studied just enough wizardry to make locks, pockets and minds easier work.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Spellcasting (Arcane Trickster) | Third-caster wizard spellcasting keyed to Intelligence, drawn mostly from enchantment and illusion. |
| 3 | Mage Hand Legerdemain | Your mage hand is invisible, and can pick locks, disarm traps, stow items and pick pockets at your Sleight of Hand bonus. |
| 9 | Magical Ambush | A creature you are hidden from has disadvantage on saves against your spell that turn. |
| 13 | Versatile Trickster | A bonus action uses mage hand to distract a creature, giving you advantage on attacks against it that turn. |
| 17 | Spell Thief | A reaction steals a spell cast at you on a failed save by the caster, denying them the spell for 8 hours and letting you cast it once; recharges on a long rest. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Mage Hand Legerdemain.

<a id="rogue-mastermind"></a>
### Mastermind

- **Source:** Xanathar's Guide to Everything
- **App description:** A schemer who reads people, plants words and directs the fight from behind an ally's shoulder.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Master of Intrigue | Proficiency with the disguise kit, forgery kit and one gaming set, two extra languages, and the ability to mimic a heard speech pattern and accent. |
| 3 | Master of Tactics | Use the Help action as a bonus action at up to 30 feet. |
| 9 | Insightful Manipulator | A minute of observation reveals whether a creature is your better or worse in Intelligence, Wisdom, Charisma and class levels. |
| 13 | Misdirection | A reaction redirects an attack aimed at you onto a creature within 5 feet that is giving you cover. |
| 17 | Soul of Deceit | Your thoughts cannot be read, magic always reads you as truthful, and you choose what a mind-reader finds. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Insightful Manipulator, Soul of Deceit.

<a id="rogue-swashbuckler"></a>
### Swashbuckler

- **Source:** Xanathar's Guide to Everything
- **App description:** A duellist who fights in the open, one on one, and talks the whole time.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Fancy Footwork | A creature you attack in melee cannot make opportunity attacks against you for the rest of your turn. |
| 3 | Rakish Audacity | Add your Charisma modifier to initiative, and sneak attack works whenever you are within 5 feet of your target alone, with no need for advantage. |
| 9 | Panache | A Persuasion check against a creature's Insight either charms it into following you peacefully or fixes its attention on you in a fight for a minute. |
| 13 | Elegant Maneuver | A bonus action grants advantage on your next Acrobatics or Athletics check. |
| 17 | Master Duelist | Once per short or long rest, reroll a missed attack with advantage. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Fancy Footwork, Panache.

<a id="rogue-inquisitive"></a>
### Inquisitive

- **Source:** Xanathar's Guide to Everything
- **App description:** A detective-rogue who finds the tell, the lie and the weak point, then puts a blade in it.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Ear for Deceit | Treat any Insight roll of 7 or lower as an 8. |
| 3 | Eye for Detail | A bonus action makes a Perception check for hidden things or an Investigation check for clues. |
| 3 | Insightful Fighting | A bonus-action Insight check against a creature's Deception lets you sneak attack it for a minute without needing advantage or an ally. |
| 9 | Steady Eye | Advantage on Perception and Investigation checks after moving no more than half your speed on your turn. |
| 13 | Unerring Eye | Sense illusions, shapechangers and other deceptions within 30 feet, a Wisdom-modifier number of times per long rest. |
| 17 | Eye for Weakness | Sneak attack against your Insightful Fighting target deals an extra 3d6. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Ear for Deceit.

<a id="rogue-scout"></a>
### Scout

- **Source:** Xanathar's Guide to Everything
- **App description:** A skirmisher and pathfinder who slips out of reach the moment anyone closes.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Skirmisher | A reaction moves you half your speed when an enemy ends its turn within 5 feet, without provoking. |
| 3 | Survivalist | Proficiency and expertise in Nature and Survival. |
| 9 | Superior Mobility | Your walking speed rises by 10 feet, along with any climb or swim speed you have. |
| 13 | Ambush Master | Advantage on initiative, and the first creature you hit on your first turn gives your allies advantage against it until your next turn. |
| 17 | Sudden Strike | A bonus action makes an extra attack, and you can sneak attack twice per turn as long as it is against different targets. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

<a id="rogue-phantom"></a>
### Phantom

- **Source:** Tasha's Cauldron of Everything
- **App description:** A rogue standing half in the land of the dead, collecting skills and souls as they go.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Whispers of the Dead | After each rest, gain proficiency in one skill or tool of your choice. |
| 3 | Wails from the Grave | After a sneak attack drops or wounds a creature, a second creature within 30 feet takes half your sneak attack dice in necrotic damage; a proficiency-bonus number of times per long rest. |
| 9 | Tokens of the Departed | A soul trinket forms when a creature dies near you: it grants advantage on death and Constitution saves, resistance to poison, and can be spent to power Wails from the Grave or question the dead. |
| 13 | Ghost Walk | A bonus action makes you spectral for 10 minutes with a 10-foot fly speed and the ability to move through creatures and objects; free once per long rest, otherwise costs a soul trinket. |
| 17 | Death's Friend | Wails from the Grave triggers alongside every sneak attack at full strength, and a soul trinket forms at the end of each long rest. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L9 Tokens of the Departed | Advantage on death and Constitution saves, resistance to poison. | Advantage on saving throws against Constitution and death saving throws while you carry a soul trinket. Resistance to poison is not part of the feature as I recall it. | Low |

**Engine hint:** no engine reference found for: Whispers of the Dead, Death's Friend.

<a id="rogue-soulknife"></a>
### Soulknife

- **Source:** Tasha's Cauldron of Everything
- **App description:** A rogue whose weapons are made of thought, thrown and re-formed at will.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 3 | Psionic Power (Soulknife) | A pool of Psionic Energy dice, twice your proficiency bonus per long rest, starting at d6 and growing with rogue level; one returns on a short rest. |
| 3 | Psi-Bolstered Knack | Spend a die to turn a failed check with a proficient skill or tool into a success if the die closes the gap. |
| 3 | Psychic Whispers | Establish telepathic communication with creatures you can see for hours, free once per long rest and otherwise for a die. |
| 3 | Psychic Blades | Manifest a thrown or melee psychic blade as part of an attack, dealing 1d6 psychic plus ability modifier, with a bonus-action second blade at 1d4; the dice grow with rogue level. |
| 9 | Soul Blades | Homing Strikes adds a die to a missed Psychic Blades attack, and Psychic Teleportation throws a blade to teleport yourself up to 10 times your die roll in feet. |
| 13 | Psychic Veil | Turn invisible for an hour as an action, free once per long rest and otherwise for a die; it ends if you deal damage or force a save. |
| 17 | Rend Mind | A sneak attack with Psychic Blades can stun a creature for a minute on a failed Wisdom save; free once per long rest and otherwise for three dice. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Soul Blades.

## Sorcerer

Subclass chosen at level 1.

<a id="sorcerer-draconic-bloodline"></a>
### Draconic Bloodline

- **Source:** SRD 5.1 / PHB
- **App description:** A sorcerer whose magic comes from a dragon ancestor: tougher scales, an elemental affinity and, in time, wings.
- **Data file:** `class-features.json` (SRD, names only)

**As shipped**

| Level | Feature |
|---|---|
| 1 | Dragon Ancestor |
| 1 | Draconic Resilience |
| 6 | Elemental Affinity |
| 14 | Dragon Wings |
| 18 | Draconic Presence |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Dragon Ancestor, Draconic Resilience, Elemental Affinity, Dragon Wings, Draconic Presence. | Names and levels match (1, 1, 6, 14, 18). | High |

<a id="sorcerer-wild-magic"></a>
### Wild Magic

- **Source:** PHB
- **App description:** A sorcerer whose power came from somewhere chaotic and has never fully settled down.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Wild Magic Surge | After casting a sorcerer spell of 1st level or higher, the DM may have you roll a d20; on a 1 you roll on the wild magic table. |
| 1 | Tides of Chaos | Gain advantage on one attack, check or save; the DM can then trigger a Wild Magic Surge to restore the use. |
| 6 | Bend Luck | A reaction spends 2 sorcery points to add or subtract 1d4 on another creature's attack, check or save. |
| 14 | Controlled Chaos | Roll twice on the wild magic table and pick either result. |
| 18 | Spell Bombardment | Once per turn, the highest damage die you roll for a spell explodes: roll it again and add it. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Controlled Chaos, Spell Bombardment.

<a id="sorcerer-divine-soul"></a>
### Divine Soul

- **Source:** Xanathar's Guide to Everything
- **App description:** A sorcerer whose bloodline runs back to something divine, mixing cleric magic into raw sorcery.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Divine Magic | Add the cleric spell list to your sorcerer spell choices, plus one affinity spell matching your divine bent. |
| 1 | Favored by the Gods | Once per short or long rest, add 2d4 to a failed save or missed attack. |
| 6 | Empowered Healing | Spend 1 sorcery point to reroll healing dice for yourself or an ally, once per turn. |
| 14 | Otherworldly Wings | A bonus action grants a 30-foot flying speed with spectral wings. |
| 18 | Unearthly Recovery | Once per long rest, a bonus action when you have fewer than half your hit points left restores hit points equal to half your maximum. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Divine Magic, Empowered Healing.

<a id="sorcerer-shadow-magic"></a>
### Shadow Magic

- **Source:** Xanathar's Guide to Everything
- **App description:** A sorcerer touched by the Shadowfell, hard to kill and accompanied by a hound of ill omen.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Eyes of the Dark | Darkvision to 120 feet, and darkness can be cast for 2 sorcery points and seen through. |
| 1 | Strength of the Grave | A Charisma save against 5 + the damage leaves you at 1 hit point instead of 0; once per long rest. |
| 6 | Hound of Ill Omen | Spend 3 sorcery points to summon a shadow dire wolf that chases one creature, gives your spells advantage against it, and blocks its escape. |
| 14 | Shadow Walk | A bonus action teleports 120 feet between dim or dark spaces. |
| 18 | Umbral Form | Spend 6 sorcery points to become shadow for a minute: resistance to all but force and radiant damage, and the ability to move through creatures and objects. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Eyes of the Dark.

<a id="sorcerer-storm-sorcery"></a>
### Storm Sorcery

- **Source:** Xanathar's Guide to Everything
- **App description:** A sorcerer of wind and thunder who drifts through fights on their own weather.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Wind Speaker | You speak, read and write Primordial and its dialects. |
| 1 | Tempestuous Magic | Before or after casting a spell of 1st level or higher, a bonus action lets you fly 10 feet without provoking. |
| 6 | Heart of the Storm | Resistance to lightning and thunder damage, and casting a spell of those types deals half your sorcerer level in damage to creatures within 10 feet. |
| 14 | Storm Guide | Stop rain around you or steer the wind's direction as a bonus action. |
| 14 | Storm's Fury | A reaction deals your sorcerer level in lightning damage to a melee attacker and pushes it 20 feet on a failed Strength save. |
| 18 | Wind Soul | Immunity to lightning and thunder damage, a 60-foot flying speed, and the ability to grant allies 30 feet of flight for an hour, once per short or long rest. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | Storm Guide level | Storm Guide at 14th level. | Storm Guide is a 6th-level feature (with Heart of the Storm). 14th has Storm's Fury only. | High |

**Engine hint:** no engine reference found for: Wind Speaker.

<a id="sorcerer-aberrant-mind"></a>
### Aberrant Mind

- **Source:** Tasha's Cauldron of Everything
- **App description:** A sorcerer whose gift came from something far outside, and speaks in telepathy and warped magic.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Psionic Spells | A set of psionic spells always known that do not count against your spells known, swappable for divination and enchantment spells as you level. |
| 1 | Telepathic Speech | A bonus action opens a telepathic link with one creature within 30 feet for minutes equal to your sorcerer level. |
| 6 | Psionic Sorcery | Cast your psionic spells by spending sorcery points instead of a slot, with no verbal, somatic or costly components. |
| 6 | Psychic Defenses | Resistance to psychic damage and advantage on saves against being charmed or frightened. |
| 14 | Revelation in Flesh | Spend 1 or more sorcery points for a minute of aberrant change: see invisible things, swim and breathe water, fly, or squeeze through tiny gaps. |
| 18 | Warping Implosion | Once per long rest, teleport 120 feet and pull creatures toward your old space for 3d10 force damage. |

**Spells in the data:** L1: Arms of Hadar, Dissonant Whispers, Mind Sliver; L3: Calm Emotions, Detect Thoughts; L5: Hunger of Hadar, Sending; L7: Evard's Black Tentacles, Summon Aberration; L9: Rary's Telepathic Bond, Telekinesis

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Psionic Spells, Psionic Sorcery, Psychic Defenses, Revelation in Flesh.

<a id="sorcerer-clockwork-soul"></a>
### Clockwork Soul

- **Source:** Tasha's Cauldron of Everything
- **App description:** A sorcerer aligned to the machinery of absolute order, smoothing chaos out of the world.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Clockwork Magic | A set of order-themed spells always known that do not count against your spells known, swappable for abjuration and transmutation spells as you level. |
| 1 | Restore Balance | A reaction cancels advantage or disadvantage on a roll within 60 feet, a proficiency-bonus number of times per long rest. |
| 6 | Bastion of Law | Spend 1 to 5 sorcery points to give a creature that many d8s to spend reducing damage, until your next long rest. |
| 14 | Trance of Order | For a minute, attacks against you cannot have advantage and you treat any d20 of 9 or lower as a 10 on attacks, checks and saves; once per long rest or for 5 sorcery points. |
| 18 | Clockwork Cavalcade | A 30-foot cube heals 100 hit points among creatures in it, ends spells of 6th level or lower, and repairs damaged objects; once per long rest or for 7 sorcery points. |

**Spells in the data:** L1: Alarm, Protection from Evil and Good; L3: Aid, Lesser Restoration; L5: Dispel Magic, Protection from Energy; L7: Freedom of Movement, Summon Construct; L9: Greater Restoration, Wall of Force

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

**Engine hint:** no engine reference found for: Clockwork Magic, Bastion of Law.

<a id="sorcerer-lunar-sorcery"></a>
### Lunar Sorcery

- **Source:** Dragonlance: Shadow of the Dragon Queen
- **App description:** A sorcerer whose magic waxes and wanes with a moon phase they can choose at will.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Lunar Embodiment | A rotating set of moon spells always known, and healing or damaging sorcerer spells you cast can shift to radiant or cold. |
| 1 | Moon Fire | A bonus action changes your lunar phase: Full grants a radiant aura, New grants concealment, Crescent grants extra reach and movement. |
| 6 | Lunar Boons | Your current phase empowers a matching school of spells, and a sorcery point can shift the phase mid-fight. |
| 14 | Waxing and Waning | Spend sorcery points to shift phase as a free action and add your Charisma modifier to a lunar spell's damage or healing once per turn. |
| 18 | Lunar Phenomenon | Once per long rest or for 5 sorcery points, unleash the full power of your phase in a 30-foot burst that heals allies, blinds enemies or hastens you depending on the moon. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | All levels | Feature names match the book, but descriptions read as a paraphrase. | I could not confirm the phase table, the lunar spell lists, Moon Fire or the effects of Lunar Boons and Lunar Phenomenon from memory. Check them against Dragonlance: Shadow of the Dragon Queen before trusting any numbers. | Low |

**Engine hint:** no engine reference found for: Lunar Embodiment, Lunar Boons, Waxing and Waning.

## Warlock

Subclass chosen at level 1.

<a id="warlock-the-fiend"></a>
### The Fiend

- **Source:** SRD 5.1 / PHB
- **App description:** A warlock whose patron is a fiend of the lower planes, granting fiery spells and temporary hit points on a kill.
- **Data file:** `class-features.json` (SRD, names only)

**As shipped**

| Level | Feature |
|---|---|
| 1 | Dark One's Blessing |
| 6 | Dark One's Own Luck |
| 10 | Fiendish Resilience |
| 14 | Hurl Through Hell |

**Spells in the data:** none.

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟠 Missing | Expanded spell list | No spells in the data at all. | Fiend expanded spells: burning hands, command (L1); blindness/deafness, scorching ray (L3); fireball, stinking cloud (L5); fire shield, wall of fire (L7); flame strike, hallow (L9). RAW they are added to the pool of spells the warlock may learn, not granted for free. | High |
| 2 | 🟡 Imprecise | All levels (SRD) | Names only: Dark One's Blessing, Dark One's Own Luck, Fiendish Resilience, Hurl Through Hell. | Names and levels match (1, 6, 10, 14). The engine implements Dark One's Blessing (Charisma modifier + warlock level temp HP on a kill), Dark One's Own Luck, Fiendish Resilience and Hurl Through Hell. | High |
| 3 | 🟡 Imprecise | L10 Fiendish Resilience | Resistance to the chosen damage type. | The chosen resistance does not apply to damage from magical weapons or silvered weapons. The resistance code (trait-rules.ts, chosenResistances) reads the chosen type from the feature name and I found no weapon exception. | High |

<a id="warlock-the-archfey"></a>
### The Archfey

- **Source:** PHB
- **App description:** A pact with a lord or lady of the Feywild: charm, illusion and very sharp bargains.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Fey Presence | An action charms or frightens every creature in a 10-foot cube that fails a Wisdom save until the end of your next turn; recharges on a short or long rest. |
| 6 | Misty Escape | A reaction on taking damage turns you invisible and teleports you 60 feet; recharges on a short or long rest. |
| 10 | Beguiling Defenses | Immunity to being charmed, and a reaction turns a charm attempt back on the caster for 1 minute and 3d10 psychic damage per turn. |
| 14 | Dark Delirium | Wrap a creature within 60 feet in illusion for a minute: charmed or frightened, unable to perceive anything else, and removable to the Feywild until the effect ends. |

**Spells in the data:** L1: Faerie Fire, Sleep; L3: Calm Emotions, Phantasmal Force; L5: Blink, Plant Growth; L7: Dominate Beast, Greater Invisibility; L9: Dominate Person, Seeming

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L10 Beguiling Defenses | A reaction turns a charm attempt back on the caster for 1 minute and 3d10 psychic damage per turn. | The reaction forces a Wisdom save; on a failure the creature is charmed by you for 1 minute or until it takes damage. There is no 3d10 psychic damage. | High |
| 2 | 🟡 Imprecise | L14 Dark Delirium | Wrap a creature in illusion for a minute; removable to the Feywild. | Action; a creature within 60 ft makes a Wisdom save or is charmed or frightened (your choice) for 1 minute or until your concentration is broken. It thinks it is in an illusory realm. Once per short or long rest. | Medium |

*See also cross-cutting finding 2: the expanded spells are granted for free by the app, but are only added to the choice pool by the rules.*

**Engine hint:** no engine reference found for: Dark Delirium.

<a id="warlock-the-great-old-one"></a>
### The Great Old One

- **Source:** PHB
- **App description:** A pact with something vast and probably unaware of you, paid in telepathy and madness.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Awakened Mind | Speak telepathically to any creature within 30 feet that shares a language with you. |
| 6 | Entropic Ward | A reaction imposes disadvantage on an attack against you, and a miss grants advantage on your next attack against that creature; recharges on a short or long rest. |
| 10 | Thought Shield | Your thoughts cannot be read, you resist psychic damage, and psychic damage dealt to you is dealt back to the attacker. |
| 14 | Create Thrall | An incapacitated humanoid you touch becomes charmed until a remove curse, with telepathic contact at any distance on the same plane. |

**Spells in the data:** L1: Dissonant Whispers, Tasha's Hideous Laughter; L3: Detect Thoughts, Phantasmal Force; L5: Clairvoyance, Sending; L7: Dominate Beast, Evard's Black Tentacles; L9: Dominate Person, Telekinesis

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell and the spell list matches.

*See also cross-cutting finding 2: the expanded spells are granted for free by the app, but are only added to the choice pool by the rules.*

**Engine hint:** no engine reference found for: Awakened Mind, Thought Shield, Create Thrall.

<a id="warlock-the-celestial"></a>
### The Celestial

- **Source:** Xanathar's Guide to Everything
- **App description:** A pact with a being of the Upper Planes: radiant fire in one hand, healing light in the other.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Bonus Cantrips (Celestial) | You know light and sacred flame. |
| 1 | Healing Light | A pool of d6s equal to 1 + your warlock level, spendable as a bonus action to heal a creature within 60 feet; refills on a long rest. |
| 6 | Radiant Soul | Resistance to radiant damage, and once per turn add your Charisma modifier to a radiant or fire spell's damage. |
| 10 | Celestial Resilience | Temporary hit points after every rest, for you and up to five allies, equal to your warlock level plus your Charisma modifier. |
| 14 | Searing Vengeance | When you would make a death save, rise instead at half hit points, blinding and burning enemies within 30 feet for 2d8 + Charisma radiant damage; recharges on a long rest. |

**Spells in the data:** L1: Cure Wounds, Guiding Bolt; L3: Flaming Sphere, Lesser Restoration; L5: Daylight, Revivify; L7: Guardian of Faith, Wall of Fire; L9: Flame Strike, Greater Restoration

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L10 Celestial Resilience | Temporary hit points after every rest, for you and up to five allies, equal to your warlock level plus your Charisma modifier. | You gain temporary hit points equal to your warlock level + Charisma modifier; up to five allies gain half your warlock level + Charisma modifier. The allies' amount is half. | High |
| 2 | 🟡 Imprecise | L1 Healing Light | A pool of d6s equal to 1 + your warlock level. | Matches; additionally you can spend up to your Charisma modifier (minimum 1) dice per use. | High |

*See also cross-cutting finding 2: the expanded spells are granted for free by the app, but are only added to the choice pool by the rules.*

**Engine hint:** no engine reference found for: Bonus Cantrips (Celestial), Radiant Soul.

<a id="warlock-the-hexblade"></a>
### The Hexblade

- **Source:** Xanathar's Guide to Everything
- **App description:** A pact with a sentient weapon from the Shadowfell: a warlock who fights in armor, with a blade.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Hexblade's Curse | A bonus action curses a creature for a minute: bonus damage equal to your proficiency bonus, critical hits on a 19 or 20, and hit points back when it dies; recharges on a short or long rest. |
| 1 | Hex Warrior | Proficiency with medium armor, shields and martial weapons, and one weapon each day uses Charisma for attack and damage. |
| 6 | Accursed Specter | A humanoid you kill rises as a specter under your command until your next long rest; recharges on a long rest. |
| 10 | Armor of Hexes | An attack from your cursed target misses on a coin-flip d6 roll even when it would hit. |
| 14 | Master of Hexes | When your cursed target dies, move the curse to a new creature without spending a rest. |

**Spells in the data:** L1: Shield, Wrathful Smite; L3: Blur, Branding Smite; L5: Blink, Elemental Weapon; L7: Phantasmal Killer, Staggering Smite; L9: Banishing Smite, Cone of Cold

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L1 Hex Warrior | One weapon each day uses Charisma for attack and damage. | At the end of a long rest, touch one weapon you are proficient with that lacks the two-handed property: you use Charisma for attack and damage with it until you use the feature again. If you have Pact of the Blade you can use Charisma with any pact weapon. 'Each day' is acceptable shorthand; the two-handed restriction and the Pact of the Blade interaction are missing. | High |
| 2 | 🟡 Imprecise | L1 Hexblade's Curse | Bonus damage, crit on 19 or 20, hit points back when it dies. | Bonus damage equals proficiency bonus; crits on 19 or 20; when the cursed creature dies you regain hit points equal to warlock level + Charisma modifier (minimum 1). Range 30 ft, 1 minute. The server only has guidance text for this feature; the bonus damage and crit range are not applied automatically (see engine appendix). | High |
| 3 | 🟡 Imprecise | L10 Armor of Hexes | Misses on a coin-flip d6 roll even when it would hit. | When the cursed target hits you with an attack, roll a d6: on a 4 or higher the attack misses. 'Coin flip' is loose; the threshold is 4+. | High |
| 4 | 🟡 Imprecise | L14 Master of Hexes | Move the curse to a new creature without spending a rest. | When the cursed creature dies, apply the curse to another creature you can see within 30 ft of the dead one while you're not incapacitated. You do not regain hit points from the first creature's death when you do so. | High |
| 5 | 🟡 Imprecise | L6 Accursed Specter | A humanoid you kill rises as a specter under your command until your next long rest. | As I recall it: when you slay a humanoid you can have its spirit rise as a specter at the end of your next turn; it has temporary hit points equal to half your warlock level and adds your Charisma modifier to its damage, and it is lost at your next long rest. Usable once per long rest. The temporary hit points and damage bonus are not in the app text. | Medium |
| 6 | 🟡 Imprecise | Expanded spells | Granted automatically. | Hexblade has an expanded spell list; the spells are added to the pool the warlock picks from, not granted free. The list itself is correct: shield, wrathful smite; blur, branding smite; blink, elemental weapon; phantasmal killer, staggering smite; banishing smite, cone of cold. | High |

*See also cross-cutting finding 2: the expanded spells are granted for free by the app, but are only added to the choice pool by the rules.*

**Engine hint:** no engine reference found for: Armor of Hexes, Master of Hexes.

<a id="warlock-the-fathomless"></a>
### The Fathomless

- **Source:** Tasha's Cauldron of Everything
- **App description:** A pact with something in the deep water, which sends a tentacle when called.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Tentacle of the Deep | A bonus action summons a spectral tentacle for a minute that attacks for 1d8 cold damage and halves a target's speed; a proficiency-bonus number of times per long rest. |
| 1 | Gift of the Sea | A 40-foot swimming speed and the ability to breathe underwater. |
| 6 | Oceanic Soul | Immunity to cold damage, full immersion comfort, and the ability to speak with any creature underwater. |
| 6 | Guardian Coil | Your tentacle reduces damage to you or a creature near it by 1d8, rising to 2d8 at 10th level. |
| 10 | Grasping Tentacles | You always have evard's black tentacles prepared, gain temporary hit points when you cast it, and keep concentration through damage more easily. |
| 14 | Fathomless Plunge | Teleport yourself and up to five willing creatures to a body of water within a mile; recharges on a short or long rest. |

**Spells in the data:** L1: Create or Destroy Water, Thunderwave; L3: Gust of Wind, Silence; L5: Lightning Bolt, Sleet Storm; L7: Control Water, Summon Elemental; L9: Cone of Cold, Commune with Nature

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L6 Oceanic Soul | Immunity to cold damage. | Resistance to cold damage, not immunity. You can also communicate telepathically with any creature in the water within 120 ft that speaks a language. | High |
| 2 | 🟡 Imprecise | L10 Grasping Tentacles | You always have evard's black tentacles prepared, gain temporary hit points when you cast it, and keep concentration through damage more easily. | You learn evard's black tentacles (it does not count against spells known) and can cast it once per long rest without a slot. Whenever you cast it you gain temporary hit points equal to your warlock level, and you have advantage on Constitution saves to keep concentration on it. The app says 'always prepared' and omits the free cast. | Medium |
| 3 | 🔴 Wrong | Expanded spells L9 | Cone of Cold, Commune with Nature. | Bigby's Hand and Cone of Cold. Commune with Nature is not on the Fathomless list. | Medium |
| 4 | 🔴 Wrong | L1 Tentacle of the Deep | 1d8 cold damage and halves a target's speed. | A hit reduces the target's speed by 10 feet until the start of your next turn, not by half. The damage is 1d8 cold, 2d8 from 10th level. Uses are proficiency bonus per long rest (correct). | Medium |

*See also cross-cutting finding 2: the expanded spells are granted for free by the app, but are only added to the choice pool by the rules.*

<a id="warlock-the-genie"></a>
### The Genie

- **Source:** Tasha's Cauldron of Everything
- **App description:** A pact with a noble genie, carrying their element and a vessel you can climb inside.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Genie's Vessel | A vessel tied to your patron: once per long rest, Bottled Respite lets you retreat inside it for 10 minutes, and Genie's Wrath adds your proficiency bonus in your patron's damage type to one hit per turn. |
| 6 | Elemental Gift | Resistance to your patron's damage type, and a bonus action grants a 30-foot flying speed for 10 minutes, a proficiency-bonus number of times per long rest. |
| 10 | Sanctuary Vessel | Bottled Respite takes up to five companions with you, and everyone who finishes a short rest inside gains temporary hit points. |
| 14 | Limited Wish | Once per 1d4 long rests, replicate any spell of 6th level or lower without components. |

**Spells in the data:** L1: Detect Evil and Good, Protection from Evil and Good; L3: Phantasmal Force, Gust of Wind; L5: Create Food and Water, Phantom Steed; L7: Arcane Eye, Fire Shield; L9: Creation, Seeming

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | Expanded spells (all tiers) | A single fixed list: Detect Evil and Good, Protection from Evil and Good; Phantasmal Force, Gust of Wind; Create Food and Water, Phantom Steed; Arcane Eye, Fire Shield; Creation, Seeming. | The Genie gets two spells per tier: one fixed (detect evil and good, phantasmal force, create food and water, phantasmal killer, creation) and one that depends on the genie kind (Dao, Djinni, Efreeti, Marid). The shipped list mixes a few correct fixed spells with spells that are not on the Genie list (protection from evil and good, phantom steed, arcane eye, seeming) and has no genie-kind choice. | Medium |
| 2 | 🟠 Missing | L1 Genie's Vessel (kind choice) | No kind choice (Dao, Djinni, Efreeti, Marid). | The patron's kind drives the damage type (bludgeoning, thunder, fire, cold), the expanded spells, the resistance at 6th and the flying/other riders. The app has a generic 'your patron's damage type'. | Medium |

*See also cross-cutting finding 2: the expanded spells are granted for free by the app, but are only added to the choice pool by the rules.*

<a id="warlock-the-undead"></a>
### The Undead

- **Source:** Van Richten's Guide to Ravenloft
- **App description:** A pact with a lich or worse: the warlock becomes a little more like their patron each level.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Form of Dread | A bonus action for a minute grants temporary hit points of 1d10 + warlock level, immunity to fear, and the power to frighten one creature you hit each turn; a proficiency-bonus number of times per long rest. |
| 6 | Grave Touched | You need no food, drink or air, and once per turn a damaging attack can deal necrotic damage instead, with a bonus die while in Form of Dread. |
| 10 | Necrotic Husk | Immunity to necrotic damage, and once per long rest dropping to 0 hit points instead leaves you at 1 and blasts nearby creatures for necrotic damage equal to your warlock level. |
| 14 | Spirit Projection | For an hour, project your spirit: resistance to most damage, the ability to move through solid matter, and necrotic and psychic spell damage that heals you; once per long rest or with a 5th-level slot. |

**Spells in the data:** L1: False Life, Ray of Sickness; L3: Blindness/Deafness, Phantasmal Force; L5: Phantom Steed, Speak with Dead; L7: Death Ward, Greater Invisibility; L9: Antilife Shell, Cloudkill

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | Expanded spells L1 | False Life, Ray of Sickness. | Bane, False Life. Ray of Sickness is on the Undying list, not the Undead. | Medium |
| 2 | 🔴 Wrong | L10 Necrotic Husk | Immunity to necrotic damage; blast nearby creatures for necrotic damage equal to your warlock level. | Resistance (not immunity) to necrotic damage. When reduced to 0 you drop to 1 instead and each creature within 30 ft takes necrotic damage equal to 2 + your warlock level. Once per long rest. | Medium |
| 3 | 🟡 Imprecise | L1 Form of Dread | Temp HP 1d10 + warlock level, immunity to fear, frighten one creature you hit each turn. | Matches; the frighten rider is a Wisdom save. | High |

*See also cross-cutting finding 2: the expanded spells are granted for free by the app, but are only added to the choice pool by the rules.*

**Engine hint:** no engine reference found for: Grave Touched.

<a id="warlock-the-undying"></a>
### The Undying

- **Source:** Sword Coast Adventurer's Guide
- **App description:** A pact with a deathless being that has cheated the grave and shares the trick.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 1 | Among the Dead | You know spare the dying, undead have disadvantage attacking you unless you attack them, and you have advantage on saves against disease. |
| 6 | Defy Death | Regain 1d8 + your Constitution modifier hit points when you succeed on a death save or stabilize a creature; recharges on a long rest. |
| 10 | Undying Nature | You barely need to breathe, eat or sleep, you no longer age, and you cannot be aged magically. |
| 14 | Indestructible Life | A bonus action heals 1d8 + your warlock level and reattaches severed body parts; recharges on a short or long rest. |

**Spells in the data:** L1: False Life, Ray of Sickness; L3: Blindness/Deafness, Silence; L5: Feign Death, Speak with Dead; L7: Aura of Life, Death Ward; L9: Contagion, Legend Lore

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L1 Among the Dead | Undead have disadvantage attacking you unless you attack them. | If an undead targets you directly with an attack or a harmful spell it makes a Wisdom save; on a failure it must choose a new target or lose the attack or spell. It does not protect against area effects. The creature is immune for 24 hours after succeeding or if you target it. | High |

*See also cross-cutting finding 2: the expanded spells are granted for free by the app, but are only added to the choice pool by the rules.*

**Engine hint:** no engine reference found for: Undying Nature.

## Wizard

Subclass chosen at level 2.

<a id="wizard-school-of-evocation"></a>
### School of Evocation

- **Source:** SRD 5.1 / PHB
- **App description:** A wizard who shapes destructive magic, sculpting spells around allies and making cantrips hit harder.
- **Data file:** `class-features.json` (SRD, names only)

**As shipped**

| Level | Feature |
|---|---|
| 2 | Evocation Savant |
| 2 | Sculpt Spells |
| 6 | Potent Cantrip |
| 10 | Empowered Evocation |
| 14 | Overchannel |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Evocation Savant, Sculpt Spells, Potent Cantrip, Empowered Evocation, Overchannel. | Names and levels match (2, 2, 6, 10, 14). | High |

**Engine hint:** no engine reference found for: Evocation Savant.

<a id="wizard-school-of-abjuration"></a>
### School of Abjuration

- **Source:** PHB
- **App description:** A wizard who specializes in wards and counters, carrying a rechargeable magical shield.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Abjuration Savant | Copying abjuration spells into your book costs half the time and gold. |
| 2 | Arcane Ward | Casting an abjuration spell of 1st level or higher raises a ward with hit points equal to twice your wizard level plus your Intelligence modifier, which absorbs damage aimed at you and recharges as you cast. |
| 6 | Projected Ward | A reaction makes your Arcane Ward absorb damage aimed at a creature within 30 feet. |
| 10 | Improved Abjuration | Add your proficiency bonus to ability checks made as part of an abjuration spell, such as counterspell and dispel magic. |
| 14 | Spell Resistance | Advantage on saves against spells, and resistance to damage from spells. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Abjuration Savant, Improved Abjuration.

<a id="wizard-school-of-conjuration"></a>
### School of Conjuration

- **Source:** PHB
- **App description:** A wizard who makes things appear: objects, allies, and themselves somewhere safer.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Conjuration Savant | Copying conjuration spells into your book costs half the time and gold. |
| 2 | Minor Conjuration | An action conjures a nonmagical object up to 3 feet on a side and 10 pounds for an hour. |
| 6 | Benign Transposition | Teleport 30 feet or swap places with a willing creature; recharges on a long rest or when you cast a conjuration spell. |
| 10 | Focused Conjuration | Concentration on a conjuration spell cannot be broken by taking damage. |
| 14 | Durable Summons | Creatures you summon get 30 temporary hit points. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Conjuration Savant, Minor Conjuration, Focused Conjuration.

<a id="wizard-school-of-divination"></a>
### School of Divination

- **Source:** PHB
- **App description:** A wizard who reads the future well enough to swap out the dice they do not like.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Divination Savant | Copying divination spells into your book costs half the time and gold. |
| 2 | Portent | Roll two d20s after each long rest and replace any attack, check or save made by you or a creature you can see with one of them; three dice from 14th level. |
| 6 | Expert Divination | Casting a divination spell of 2nd level or higher returns one expended slot of lower level. |
| 10 | The Third Eye | After a rest, gain darkvision, ethereal sight, greater comprehension or see invisibility until your next rest. |
| 14 | Greater Portent | You roll three Portent dice after each long rest instead of two. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L6 Expert Divination | Returns one expended slot of lower level. | You regain an expended spell slot of a level lower than the divination spell you cast, and the slot can be no higher than 5th level. | High |

**Engine hint:** no engine reference found for: Divination Savant, Expert Divination, The Third Eye.

<a id="wizard-school-of-enchantment"></a>
### School of Enchantment

- **Source:** PHB
- **App description:** A wizard who works on minds: charm, compulsion, and making an enemy forget which side they are on.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Enchantment Savant | Copying enchantment spells into your book costs half the time and gold. |
| 2 | Hypnotic Gaze | An action charms and incapacitates a creature within 5 feet until the end of your next turn, extendable each turn; recharges on a short or long rest. |
| 6 | Instinctive Charm | A reaction redirects an attack aimed at you onto another creature within the attacker's reach on a failed Wisdom save; recharges on a short or long rest. |
| 10 | Split Enchantment | An enchantment spell of 1st level or higher that targets one creature can target a second. |
| 14 | Alter Memories | A charmed creature forgets it was charmed, and you can strip up to your Charisma modifier plus one hour of memory on a failed save. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L2 Hypnotic Gaze | Recharges on a short or long rest. | Once you use it on a creature you cannot use it on that same creature again until you finish a long rest. It does not recharge on a short rest and isn't a pool for you. | High |
| 2 | 🔴 Wrong | L6 Instinctive Charm | Recharges on a short or long rest. | The limit is per attacker: on a successful save the attacker cannot be affected again until you finish a long rest. There is no short rest recharge. | High |

**Engine hint:** no engine reference found for: Enchantment Savant, Split Enchantment, Alter Memories.

<a id="wizard-school-of-illusion"></a>
### School of Illusion

- **Source:** PHB
- **App description:** A wizard who wins by making the battlefield disagree with everyone else's senses.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Illusion Savant | Copying illusion spells into your book costs half the time and gold. |
| 2 | Improved Minor Illusion | You know minor illusion and can create sound and an image with the same casting. |
| 6 | Malleable Illusion | An action reshapes an illusion you are concentrating on while it lasts. |
| 10 | Illusory Self | A reaction interposes an illusory duplicate so an attack that would hit you misses; recharges on a short or long rest. |
| 14 | Illusory Reality | A bonus action makes one object in your illusion real for a minute. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L6 name | Malleable Illusion. | The feature is named Malleable Illusions. | High |

**Engine hint:** no engine reference found for: Illusion Savant, Improved Minor Illusion, Malleable Illusion.

<a id="wizard-school-of-necromancy"></a>
### School of Necromancy

- **Source:** PHB
- **App description:** A wizard who studies the boundary of life and death, and keeps a retinue on the far side of it.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Necromancy Savant | Copying necromancy spells into your book costs half the time and gold. |
| 2 | Grim Harvest | Once per turn, killing a creature with a spell of 1st level or higher restores twice the spell's level in hit points, three times for necromancy spells. |
| 6 | Undead Thralls | You learn animate dead, raise one extra corpse per casting, and your undead gain extra hit points and add your proficiency bonus to weapon damage. |
| 10 | Inured to Undeath | Resistance to necrotic damage, and your hit point maximum cannot be reduced. |
| 14 | Command Undead | An action takes control of an undead creature on a failed Charisma save, with intelligent undead getting a fresh save each time they take damage. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Necromancy Savant, Grim Harvest, Undead Thralls, Inured to Undeath, Command Undead.

<a id="wizard-school-of-transmutation"></a>
### School of Transmutation

- **Source:** PHB
- **App description:** A wizard who changes what things are: matter, shape, and eventually the limits of the body.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Transmutation Savant | Copying transmutation spells into your book costs half the time and gold. |
| 2 | Minor Alchemy | Ten minutes of concentration converts one material into another, such as wood into stone, for an hour. |
| 6 | Transmuter's Stone | Craft a stone granting one of darkvision, 10 feet of speed, proficiency in Constitution saves, or resistance to an element, changeable when you cast a transmutation spell. |
| 10 | Shapechanger | You always have polymorph prepared and can cast it on yourself as a beast of CR 1 or lower without a slot, once per short or long rest. |
| 14 | Master Transmuter | Destroy your Transmuter's Stone to transform an object, remove all curses and disease from a creature, restore a creature's youth, or raise the dead. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Transmutation Savant, Minor Alchemy, Master Transmuter.

<a id="wizard-war-magic"></a>
### War Magic

- **Source:** Xanathar's Guide to Everything
- **App description:** A battle wizard who blends abjuration and evocation into a fighting style of shields and volleys.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Arcane Deflection | A reaction after being hit or failing a save grants +2 AC or +4 on the save, at the cost of casting only cantrips on your next turn. |
| 2 | Tactical Wit | Add your Intelligence modifier to initiative. |
| 6 | Power Surge | Store surges from dispelled or countered spells, up to your Intelligence modifier, and spend one to add half your wizard level to a damaging spell's damage. |
| 10 | Durable Magic | While concentrating on a spell, gain +2 AC and +2 on all saving throws. |
| 14 | Deflecting Shroud | Arcane Deflection also deals half your wizard level in force damage to up to three creatures within 60 feet. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Tactical Wit, Power Surge, Deflecting Shroud.

<a id="wizard-bladesinging"></a>
### Bladesinging

- **Source:** Sword Coast Adventurer's Guide / Tasha's
- **App description:** An elven tradition of sword-dance and spell, trading a heavy spellbook for footwork.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Training in War and Song | Proficiency with light armor, one one-handed melee weapon, and the Performance skill. |
| 2 | Bladesong | A bonus action for a minute grants +Intelligence to AC, 10 feet of speed, advantage on Acrobatics and a concentration-save bonus; a proficiency-bonus number of times per long rest. |
| 6 | Extra Attack | Attack twice with the Attack action, and one of those attacks may be a cantrip with a casting time of one action. |
| 10 | Song of Defense | A reaction while bladesinging spends a spell slot to reduce damage to you by five times the slot level. |
| 14 | Song of Victory | Add your Intelligence modifier to melee weapon damage while bladesinging. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Song of Victory.

<a id="wizard-chronurgy-magic"></a>
### Chronurgy Magic

- **Source:** Explorer's Guide to Wildemount
- **App description:** A wizard of Wildemount who manipulates time itself: initiative, durations and dice.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Chronal Shift | A reaction forces a creature within 30 feet to reroll an attack, check or save, twice per long rest. |
| 2 | Temporal Awareness | Add your Intelligence modifier to initiative. |
| 6 | Momentary Stasis | A Large or smaller creature within 60 feet is frozen in time until the end of your next turn on a failed Constitution save; a proficiency-bonus number of times per long rest. |
| 10 | Arcane Abeyance | Store a spell of 4th level or lower in a bead that anyone can break to cast it; recharges on a short or long rest. |
| 14 | Convergent Future | A reaction dictates the outcome of a roll within 60 feet as a success or a failure, at the cost of a level of exhaustion. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Temporal Awareness.

<a id="wizard-graviturgy-magic"></a>
### Graviturgy Magic

- **Source:** Explorer's Guide to Wildemount
- **App description:** A wizard of Wildemount who bends weight and gravity, crushing, slowing and hurling.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Adjust Density | Halve or double a creature's or object's weight for a minute, changing its speed, carrying capacity and Strength checks. |
| 6 | Gravity Well | Any creature you affect with a spell can be moved 5 feet to a space of your choice. |
| 10 | Violent Attraction | A reaction adds 1d10 to a falling or weapon damage roll within 60 feet, a proficiency-bonus number of times per long rest. |
| 14 | Event Horizon | A 30-foot field of crushing gravity for a minute that deals 2d10 force damage and halts movement on a failed Strength save; once per long rest or with a 4th-level slot. |

**Findings against the rules**

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

**Engine hint:** no engine reference found for: Gravity Well.

<a id="wizard-order-of-scribes"></a>
### Order of Scribes

- **Source:** Tasha's Cauldron of Everything
- **App description:** A wizard whose spellbook wakes up and starts talking, and whose spells can change their element.
- **Data file:** `subclasses.json`

**As shipped**

| Level | Feature | App text |
|---|---|---|
| 2 | Wizardly Quill | Conjure a magic quill that needs no ink, scribes at speed, and can erase what it wrote. |
| 2 | Awakened Spellbook | Your book gains a mind: it lets you change a spell's damage type, cast one ritual per long rest without preparing it, and replace lost pages. |
| 6 | Manifest Mind | A bonus action projects your book's spirit up to 300 feet, letting you cast spells from its position and shed light; free once per long rest, otherwise a slot. |
| 10 | Master Scrivener | After a long rest, create a spell scroll of a 1st or 2nd-level spell from your book, cast at maximum effect. |
| 14 | One with the Word | Once per long rest, avoid death by destroying your book's magic, then rebuild it over days of study. |

**Findings against the rules**

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L2 Awakened Spellbook | Change a spell's damage type, cast one ritual per long rest without preparing it. | As I recall it, the book lets you change a spell's damage type to another damage type from a spell in the book and lets you cast the ritual spells in it using a spell slot as a normal casting time, not once per long rest. I could not confirm 'one ritual per long rest'. Check against Tasha's. | Low |
| 2 | 🟡 Imprecise | L10 Master Scrivener | Create a spell scroll of a 1st or 2nd-level spell, cast at maximum effect. | After a long rest, create a spell scroll of a 1st- or 2nd-level spell from your book. I could not confirm the 'maximum effect' wording from memory; check the exact casting rule against Tasha's. | Low |

**Engine hint:** no engine reference found for: Awakened Spellbook.

## Appendix A: Warlock level 1 to 20 progression

Checked against the Player's Handbook warlock table. Slots, slot level, cantrips known, spells known, invocations known, Pact Boon, Mystic Arcanum and Eldritch Master all match the rules. Ability Score Improvements are at 4, 8, 12, 16 and 19.

| Level | Features | Cantrips | Spells known | Pact slots | Slot level | Invocations |
|---|---|---|---|---|---|---|
| 1 | Otherworldly Patron, Pact Magic | 2 | 2 | 1 | 1 | 0 |
| 2 | Eldritch Invocations | 2 | 3 | 2 | 1 | 2 |
| 3 | Pact Boon | 2 | 4 | 2 | 2 | 2 |
| 4 | Ability Score Improvement | 3 | 5 | 2 | 2 | 2 |
| 5 |  | 3 | 6 | 2 | 3 | 3 |
| 6 | Patron feature | 3 | 7 | 2 | 3 | 3 |
| 7 |  | 3 | 8 | 2 | 4 | 4 |
| 8 | Ability Score Improvement | 3 | 9 | 2 | 4 | 4 |
| 9 |  | 3 | 10 | 2 | 5 | 5 |
| 10 | Patron feature | 4 | 10 | 2 | 5 | 5 |
| 11 | Mystic Arcanum (6th) | 4 | 11 | 3 | 5 | 5 |
| 12 | Ability Score Improvement | 4 | 11 | 3 | 5 | 6 |
| 13 | Mystic Arcanum (7th) | 4 | 12 | 3 | 5 | 6 |
| 14 | Patron feature | 4 | 12 | 3 | 5 | 6 |
| 15 | Mystic Arcanum (8th) | 4 | 13 | 3 | 5 | 7 |
| 16 | Ability Score Improvement | 4 | 13 | 3 | 5 | 7 |
| 17 | Mystic Arcanum (9th) | 4 | 14 | 4 | 5 | 7 |
| 18 |  | 4 | 14 | 4 | 5 | 8 |
| 19 | Ability Score Improvement | 4 | 15 | 4 | 5 | 8 |
| 20 | Eldritch Master | 4 | 15 | 4 | 5 | 8 |

Patron features arrive at 1, 6, 10 and 14 for every warlock patron. Note the level 6, 10 and 14 patron features have no row in `class-features.json`'s base warlock table; they come from the subclass file.

## Appendix B: Engine wiring hints

Features whose exact name does not appear in the engine code (`src/lib/dm`, `class-resources*`, `authored-resources.json`, `feature-effects`, `authored-effects*`, `trait-rules`, `subclass-training`, `combat-rows`, `battlemap`, `options`, `level-up*`, spell and summon tables). Feature names are matched case-insensitively, with any `Channel Divinity:` prefix and bracketed suffix stripped. Treat as a lead, not a verdict.

| Class | Subclass | Level | Feature |
|---|---|---|---|
| Artificer | Alchemist | 5 | Alchemical Savant |
| Artificer | Alchemist | 15 | Chemical Mastery |
| Artificer | Armorer | 3 | Arcane Armor |
| Artificer | Armorer | 9 | Armor Modifications |
| Artificer | Artillerist | 15 | Fortified Position |
| Barbarian | Path of the Totem Warrior | 3 | Spirit Seeker |
| Barbarian | Path of the Totem Warrior | 10 | Spirit Walker |
| Barbarian | Path of the Ancestral Guardian | 14 | Vengeful Ancestors |
| Barbarian | Path of the Storm Herald | 6 | Storm Soul |
| Barbarian | Path of the Zealot | 3 | Warrior of the Gods |
| Barbarian | Path of the Zealot | 14 | Rage Beyond Death |
| Barbarian | Path of the Beast | 6 | Bestial Soul |
| Barbarian | Path of Wild Magic | 14 | Controlled Surge |
| Bard | College of Lore | 6 | Additional Magical Secrets |
| Bard | College of Swords | 14 | Master's Flourish |
| Bard | College of Eloquence | 3 | Silver Tongue |
| Bard | College of Eloquence | 6 | Unfailing Inspiration |
| Bard | College of Creation | 14 | Creative Crescendo |
| Bard | College of Spirits | 3 | Guiding Whispers |
| Bard | College of Spirits | 3 | Spiritual Focus |
| Bard | College of Spirits | 3 | Tales from Beyond |
| Bard | College of Spirits | 14 | Mystical Connection |
| Cleric | Knowledge Domain | 8 | Potent Spellcasting |
| Cleric | Knowledge Domain | 17 | Visions of the Past |
| Cleric | Light Domain | 1 | Bonus Cantrip (Light) |
| Cleric | Light Domain | 8 | Potent Spellcasting |
| Cleric | Tempest Domain | 6 | Thunderbolt Strike |
| Cleric | Trickery Domain | 17 | Improved Duplicity |
| Cleric | War Domain | 17 | Avatar of Battle |
| Cleric | Grave Domain | 8 | Potent Spellcasting |
| Cleric | Peace Domain | 1 | Emboldening Bond |
| Cleric | Peace Domain | 8 | Potent Spellcasting |
| Cleric | Peace Domain | 17 | Expansive Bond |
| Cleric | Twilight Domain | 17 | Twilight Shroud |
| Cleric | Arcana Domain | 6 | Spell Breaker |
| Cleric | Arcana Domain | 8 | Potent Spellcasting |
| Cleric | Arcana Domain | 17 | Arcane Mastery |
| Druid | Circle of the Land | 2 | Bonus Cantrip |
| Druid | Circle of the Land | 3 | Circle Spells |
| Druid | Circle of the Moon | 2 | Circle Forms |
| Druid | Circle of the Moon | 10 | Elemental Wild Shape |
| Druid | Circle of the Moon | 14 | Thousand Forms |
| Druid | Circle of Dreams | 6 | Hearth of Moonlight and Shadow |
| Druid | Circle of the Shepherd | 2 | Speech of the Woods |
| Druid | Circle of the Shepherd | 6 | Mighty Summoner |
| Druid | Circle of the Shepherd | 10 | Guardian Spirit |
| Druid | Circle of Spores | 10 | Spreading Spores |
| Druid | Circle of Stars | 14 | Full of Stars |
| Fighter | Eldritch Knight | 15 | Arcane Charge |
| Fighter | Eldritch Knight | 18 | Improved War Magic |
| Fighter | Cavalier | 15 | Ferocious Charger |
| Fighter | Echo Knight | 7 | Echo Avatar |
| Fighter | Psi Warrior | 10 | Guarded Mind |
| Fighter | Rune Knight | 15 | Master of Runes |
| Fighter | Banneret | 3 | Rallying Cry |
| Monk | Way of Shadow | 3 | Shadow Arts |
| Monk | Way of the Drunken Master | 3 | Drunken Technique |
| Monk | Way of the Kensei | 6 | One with the Blade |
| Monk | Way of the Kensei | 17 | Unerring Accuracy |
| Monk | Way of the Sun Soul | 17 | Sun Shield |
| Monk | Way of the Long Death | 6 | Hour of Reaping |
| Monk | Way of the Long Death | 11 | Mastery of Death |
| Monk | Way of Mercy | 3 | Hand of Healing |
| Monk | Way of Mercy | 3 | Hand of Harm |
| Monk | Way of Mercy | 6 | Physician's Touch |
| Monk | Way of Mercy | 11 | Flurry of Healing and Harm |
| Monk | Way of the Ascendant Dragon | 3 | Draconic Disciple |
| Paladin | Oath of Conquest | 15 | Scornful Rebuke |
| Paladin | Oath of the Watchers | 7 | Aura of the Sentinel |
| Paladin | Oathbreaker | 7 | Aura of Hate |
| Paladin | Oathbreaker | 15 | Supernatural Resistance |
| Ranger | Beast Master | 7 | Exceptional Training |
| Ranger | Beast Master | 11 | Bestial Fury |
| Ranger | Beast Master | 15 | Share Spells |
| Ranger | Gloom Stalker | 3 | Umbral Sight |
| Ranger | Gloom Stalker | 11 | Stalker's Flurry |
| Ranger | Horizon Walker | 11 | Distant Strike |
| Ranger | Fey Wanderer | 3 | Dreadful Strikes |
| Ranger | Fey Wanderer | 3 | Fey Wanderer Magic |
| Ranger | Swarmkeeper | 3 | Swarmkeeper Magic |
| Ranger | Drakewarden | 3 | Draconic Gift |
| Ranger | Drakewarden | 7 | Bond of Fang and Scale |
| Ranger | Drakewarden | 15 | Perfected Bond |
| Rogue | Thief | 13 | Use Magic Device |
| Rogue | Assassin | 9 | Infiltration Expertise |
| Rogue | Assassin | 13 | Impostor |
| Rogue | Assassin | 17 | Death Strike |
| Rogue | Arcane Trickster | 3 | Mage Hand Legerdemain |
| Rogue | Mastermind | 9 | Insightful Manipulator |
| Rogue | Mastermind | 17 | Soul of Deceit |
| Rogue | Swashbuckler | 3 | Fancy Footwork |
| Rogue | Swashbuckler | 9 | Panache |
| Rogue | Inquisitive | 3 | Ear for Deceit |
| Rogue | Phantom | 3 | Whispers of the Dead |
| Rogue | Phantom | 17 | Death's Friend |
| Rogue | Soulknife | 9 | Soul Blades |
| Sorcerer | Wild Magic | 14 | Controlled Chaos |
| Sorcerer | Wild Magic | 18 | Spell Bombardment |
| Sorcerer | Divine Soul | 1 | Divine Magic |
| Sorcerer | Divine Soul | 6 | Empowered Healing |
| Sorcerer | Shadow Magic | 1 | Eyes of the Dark |
| Sorcerer | Storm Sorcery | 1 | Wind Speaker |
| Sorcerer | Aberrant Mind | 1 | Psionic Spells |
| Sorcerer | Aberrant Mind | 6 | Psionic Sorcery |
| Sorcerer | Aberrant Mind | 6 | Psychic Defenses |
| Sorcerer | Aberrant Mind | 14 | Revelation in Flesh |
| Sorcerer | Clockwork Soul | 1 | Clockwork Magic |
| Sorcerer | Clockwork Soul | 6 | Bastion of Law |
| Sorcerer | Lunar Sorcery | 1 | Lunar Embodiment |
| Sorcerer | Lunar Sorcery | 6 | Lunar Boons |
| Sorcerer | Lunar Sorcery | 14 | Waxing and Waning |
| Warlock | The Archfey | 14 | Dark Delirium |
| Warlock | The Great Old One | 1 | Awakened Mind |
| Warlock | The Great Old One | 10 | Thought Shield |
| Warlock | The Great Old One | 14 | Create Thrall |
| Warlock | The Celestial | 1 | Bonus Cantrips (Celestial) |
| Warlock | The Celestial | 6 | Radiant Soul |
| Warlock | The Hexblade | 10 | Armor of Hexes |
| Warlock | The Hexblade | 14 | Master of Hexes |
| Warlock | The Undead | 6 | Grave Touched |
| Warlock | The Undying | 10 | Undying Nature |
| Wizard | School of Evocation | 2 | Evocation Savant |
| Wizard | School of Abjuration | 2 | Abjuration Savant |
| Wizard | School of Abjuration | 10 | Improved Abjuration |
| Wizard | School of Conjuration | 2 | Conjuration Savant |
| Wizard | School of Conjuration | 2 | Minor Conjuration |
| Wizard | School of Conjuration | 10 | Focused Conjuration |
| Wizard | School of Divination | 2 | Divination Savant |
| Wizard | School of Divination | 6 | Expert Divination |
| Wizard | School of Divination | 10 | The Third Eye |
| Wizard | School of Enchantment | 2 | Enchantment Savant |
| Wizard | School of Enchantment | 10 | Split Enchantment |
| Wizard | School of Enchantment | 14 | Alter Memories |
| Wizard | School of Illusion | 2 | Illusion Savant |
| Wizard | School of Illusion | 2 | Improved Minor Illusion |
| Wizard | School of Illusion | 6 | Malleable Illusion |
| Wizard | School of Necromancy | 2 | Necromancy Savant |
| Wizard | School of Necromancy | 2 | Grim Harvest |
| Wizard | School of Necromancy | 6 | Undead Thralls |
| Wizard | School of Necromancy | 10 | Inured to Undeath |
| Wizard | School of Necromancy | 14 | Command Undead |
| Wizard | School of Transmutation | 2 | Transmutation Savant |
| Wizard | School of Transmutation | 2 | Minor Alchemy |
| Wizard | School of Transmutation | 14 | Master Transmuter |
| Wizard | War Magic | 2 | Tactical Wit |
| Wizard | War Magic | 6 | Power Surge |
| Wizard | War Magic | 14 | Deflecting Shroud |
| Wizard | Bladesinging | 14 | Song of Victory |
| Wizard | Chronurgy Magic | 2 | Temporal Awareness |
| Wizard | Graviturgy Magic | 6 | Gravity Well |
| Wizard | Order of Scribes | 2 | Awakened Spellbook |

151 of 590 features have no engine reference by name.

## Appendix C: Official subclasses not in the app

Only subclasses I am sure exist (the Banneret in the app is the Purple Dragon Knight under another name, so it is not listed) in published 5e material are listed. This is not an exhaustive survey.

| Class | Subclass | Source |
|---|---|---|
| Bard | College of Dance | Player's Handbook (2024) |
| Monk | Way of the Cobalt Soul | Explorer's Guide to Wildemount |
| Sorcerer | Giant Soul | Bigby Presents: Glory of the Giants |
| Cleric | Ambition, Solidarity, Strength, Zeal domains | Mythic Odysseys of Theros |
| Druid | Circle of the Land terrain variants (Arctic, Coast, Desert, Forest, Grassland, Mountain, Swamp, Underdark) | Player's Handbook; no terrain data found |
