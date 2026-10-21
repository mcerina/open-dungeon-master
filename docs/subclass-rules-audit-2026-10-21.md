# Subclass rules audit

Audit of every subclass shipped in the app (117) against the published 5e rules: which features, levels, numbers and spell lists are right, which are wrong, and which are missing.

## Contents

- Scope, sources and method
- Legend
- Where the sources disagree
- Edition and errata map
- Class options that subclass features depend on
- Claims about the repository
- Limits of this audit
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

## Scope, sources and method

- **Baseline.** The 2014 rules: the SRD 5.1 and the later 2014-era books (Player's Handbook, Dungeon Master's Guide, Xanathar's, Tasha's, Sword Coast Adventurer's Guide, Explorer's Guide to Wildemount, Van Richten's, Mythic Odysseys of Theros, Guildmasters' Guide to Ravnica, Fizban's, Bigby Presents: Glory of the Giants, Dragonlance: Shadow of the Dragon Queen), with their official errata. 2024 differences are noted in the edition map and are never counted as app errors.
- **Data audited.** `src/lib/srd/subclasses.json` (105 subclasses) and the SRD subclass in `src/lib/srd/class-features.json` for each class (12; Artificer has none there). The SRD subclasses in `class-features.json` carry feature names and levels only; their text is shown from `help/srd-features.json` and the effect guidance in `authored-resources.json`, which were also checked.
- **Checked for each subclass:** the level each feature arrives at, whether any feature is missing or invented, the numbers and conditions in each feature's text (dice, saves, DCs, ranges, durations, use limits, action types), the feature names, and the subclass spell list (always prepared, expanded or fixed). Every subclass was read side by side with the class data on 5e.tools, and its feature names, levels and spell lists were compared programmatically.
- **Sources.**
  - Publisher text: the SRD 5.1 PDF for the 12 SRD subclasses; Wizards of the Coast errata PDFs for the Player's Handbook (v3.0, 2021), Xanathar's (v1.0, 2020), Tasha's (2021), Sword Coast Adventurer's Guide (v2.2, 2021), the Dungeon Master's Guide and Volo's; the D&D Beyond Sage Advice and errata pages (Eberron: Rising from the Last War, Dragonlance: Shadow of the Dragon Queen) and the Sage Advice Compendium; the D&D Beyond compendium text of Tasha's (The Fathomless) and the Sword Coast Adventurer's Guide (Oath of the Crown, Purple Dragon Knight); and a D&D Beyond forum post quoting the Xanathar's text of Keeper of Souls.
  - Transcriptions: the dnd5e wiki (one page per subclass) and the 5e.tools class data (`5etools-src`, `data/class/*.json`) and bestiary (Dancing Item, Steel Defender, Drake Companion, Wildfire Spirit).
  - Repository: `subclasses.json`, `class-features.json`, `options.json`, `authored-resources.json`, `help/srd-features.json` and the code that reads them. Nothing was run.
- **Not checked:** the item, spell and monster stat blocks the features refer to (except the four above), the subclass's class-level interactions in the builder (for example how many invocations or maneuvers get picked), and 2024 revisions.
- **Engine hints.** Appendix B lists features whose exact name appears nowhere in the engine code. A missing name means the feature is probably only shown on the sheet and not applied by the server. It is a heuristic only: a passive feature can legitimately need no code, and a feature can be applied under another name.

## Legend

| Mark | Meaning |
|---|---|
| 🔴 Wrong | The app text or data contradicts the rules. |
| 🟠 Missing | A feature, option or spell that the rules give is absent. |
| 🟡 Imprecise | Roughly right, but a number, limit, prerequisite or choice is left out or loosely worded. |
| Verified (2 sources) | Confirmed against both the dnd5e wiki and the 5e.tools data, which are separate transcriptions of the books. |
| Verified (printed text) | Confirmed against a source that reproduces the printed text: the SRD 5.1 PDF, an official Wizards of the Coast errata document, or the D&D Beyond reprint of such a document. Used only where the source covers the whole row. |

## Where the sources disagree

In three places the dnd5e wiki differs from both 5e.tools and the D&D Beyond material. The wiki is therefore not used as a tie-breaker; where it disagrees with the other sources, the other sources are followed.

- **Keeper of Souls (Grave Domain, 17th).** The range is 60 feet and the recipient is "you or one creature of your choice within 60 feet". Evidence: 5e.tools; a March 2019 D&D Beyond forum post quoting "the text from Xanathar's Guide" with 60 feet and "one creature of your choice"; and the official Xanathar's errata (v1.0, 2020), which lists no change to this feature. The wiki's "30 feet" and "one ally of your choice" is the outlier. The app's 60 ft is correct; the remaining finding is that the app says "an ally" and omits the incapacitated restriction and the once-per-round wording.
- **Divine Soul, 14th level.** The feature is Otherworldly Wings. The D&D Beyond feature table for Divine Soul (Xanathar's) reads "6th Empowered Healing, 14th Otherworldly Wings, 18th Unearthly Recovery", and 5e.tools agrees. "Angelic Form" appears only on the dnd5e wiki; it is not a 2024 name either, because the 2024 Player's Handbook has no Divine Soul. The app is right.
- **Oath of the Crown, 15th level.** The feature is Unyielding Spirit. The D&D Beyond copy of the Sword Coast Adventurer's Guide shows a heading ending in "Spirit" followed by "Starting at 15th level, you have advantage on saving throws to avoid becoming paralyzed or stunned", a D&D Beyond search for "unyielding saint" returns nothing, and 5e.tools agrees. The app is right.

## Edition and errata map

Which printing the app text matches. Items marked 2024 are differences, not errors.

| Feature | Printing history | App text |
|---|---|---|
| Ancestral Guardian features table | Xanathar's errata (4th printing on): the three "d8" in the table became "d6", matching the text | Uses 2d6, 3d6, 4d6: matches the corrected book |
| Arcane Shot | Xanathar's errata: "fire a magic arrow" became "fire an arrow" | "On an arrow hit": matches the errata |
| Armorer, Perfected Armor (Guardian) | Tasha's errata: pulled up to 25 ft to an unoccupied space | The finding uses 25 ft; the app text is wrong regardless |
| Animating Performance, Gathered Swarm, Warping Implosion | Tasha's errata (first sentence; "While you're alive"; "against your spell save DC") | Animating Performance and Gathered Swarm consistent; Warping Implosion omits the save (see its finding) |
| Bladesong, Extra Attack (Bladesinging) | Sword Coast errata v2.2: proficiency-bonus uses per long rest; a cantrip may replace one attack | Subclass text matches; `authored-resources.json` line 1067 says short rest (wrong) |
| Radiant Sun Bolt, Rakish Audacity, Inspiring Surge (Banneret) | Sword Coast errata: features reprinted; Inspiring Surge's second tier moved from 17th to 18th level | Subclass text matches (Inspiring Surge says "two allies from 18th level") |
| Ranger's Companion, Exceptional Training | Player's Handbook errata v3.0: "Dodge," deleted; magical attacks added | Exceptional Training follows the pre-errata printing (flagged) |
| Eternal Mountain Defense | Player's Handbook errata: 11th level became 17th | `options.json` says 17th: matches |
| Wild Magic Surge, Overchannel, Empowered Evocation, Minor Conjuration | Player's Handbook errata: once per turn; 1st to 5th level; "one damage roll"; "takes or deals any damage" | Subclass text consistent; the Empowered Evocation help text is not (see its finding) |
| Lunar Boons | Dragonlance errata: New Moon "Evocation" became "Enchantment", Crescent Moon "Divination" became "Illusion" | Not stated in the app; 5e.tools already has the corrected pairs |
| Steel Defender | Eberron errata replaced the stat block (AC 15, Force-Empowered Rend) | The finding matches the corrected block |
| Divine Soul 14th, Oath of the Crown 15th | No rename in any errata; the 2024 Player's Handbook has neither subclass | Otherworldly Wings and Unyielding Spirit are the printed names |
| Zealot (2024 differences) | 2024: Warrior of the Gods is a pool of d12s, Fanatical Focus adds the rage damage bonus, Rage of the Gods replaces Rage Beyond Death | App follows Xanathar's (2014); not errors |
| Glamour, Mantle of Inspiration (2024 difference) | 2014: 5, 8, 11, 14 temporary hit points; 2024: twice the Bardic Inspiration die | App gives no number |
| Circle of the Moon (2024 difference) | 2024: 13 + Wisdom AC in beast form | App follows 2014 |
| Tasha's optional features | Primal Companion (Beast Master) is listed as Missing; Blessed Strikes (a Cleric replacement for Divine Strike or Potent Spellcasting) is not listed | Neither optional feature is offered |

## Class options that subclass features depend on

| Area | Where it lives | Result |
|---|---|---|
| Battle Master maneuvers (23) | `options.json` | All 23 names right (16 from the Player's Handbook, 7 from Tasha's); effects right. Only trims: Commander's Strike omits that it uses a bonus action, Sweeping Attack omits "if the attack roll would hit". No finding. |
| Arcane Shot options (8) | Names only in `subclasses.json` and `authored-resources.json` | Names right; the save DC (8 + proficiency + Intelligence) in `authored-resources.json` is right. Option text is not in the data. |
| Elemental disciplines (17) | `options.json` | Names, ki costs and prerequisites right. One imprecise text: Elemental Attunement (see Way of the Four Elements). |
| Rune Knight runes (6) | `options.json` | Names, effects and the 7th-level prerequisite of the Hill and Storm runes right. No finding. |
| Hunter's choices (11) | `options.json`, `help/srd-features.json` | `options.json` right. The help text for Superior Hunter's Defense is wrong and for Defensive Tactics imprecise (see Hunter). |
| Totem options | `authored-effects-data.ts` (Bear, Eagle, Wolf), `subclasses.json` | Elk and Tiger absent: a finding under Path of the Totem Warrior. |
| Wild magic tables | Not in the data (Barbarian d8, Sorcerer d100) | The Barbarian d8 summary in the Wild Surge finding matches the Tasha's text. The Sorcerer table is not in the app. |
| Metamagic (10) | `options.json` | All 10 right. No finding. |
| Invocations and Pact boons | `options.json` | Checked only where a subclass depends on them (Hex Warrior with Pact of the Blade; a finding). The 48 invocations themselves are outside this audit. |

## Claims about the repository

| Claim | Result |
|---|---|
| `subclassSpellsFor` in `src/lib/srd/features.ts` grants warlock patron spells for free (Cross-cutting 2) | True. The function returns every spell in the subclass `spells` table up to the character's level, its comment says they are free and never count against the known/prepared ceiling, and `submit.ts` and `useBuilderDerived.ts` use it for warlocks unchanged. The rules side is confirmed by the publisher: the Sage Advice Compendium (2014) says expanded-list spells "aren't automatically known" and are added to the warlock list, and the SRD says the same for the Fiend. |
| The Fiend has no spell list in the data | True. In `class-features.json` only Life Domain and Oath of Devotion carry a `spells` table; The Fiend and Circle of the Land have none, and neither does Lunar Sorcery in `subclasses.json`. |
| Dark One's Blessing is implemented; Fiendish Resilience is read from the feature name with no weapon exception | True. `feature-hooks.ts` implements Dark One's Blessing (Charisma modifier + warlock level, minimum 1). `chosenResistances` in `trait-rules.ts` reads the type from a feature named `Fiendish Resilience (type)` and `pc-defenses.ts` applies it with only a magical/spell test, no silvered/magical weapon exception. |
| Hexblade's Curse has guidance text only | True. It is a `guidance` string in `authored-resources.json` (`sub_hexblades_curse`); no code applies the bonus damage or the 19-20 crit range. |
| Appendix B (151 of 590 features without an engine reference) | The 590 features reproduce exactly. A search over `src/lib/dm`, `srd`, `battlemap` and `classes` finds 147 without a reference, not 151; the four extra (Silver Tongue, Divine Magic, Thought Shield, Use Magic Device) are in files outside the directories Appendix B states, so the appendix is consistent with its own method. |
| Appendix A (warlock table) | Every cell matches the Player's Handbook table. |
| `authored-resources.json` repeats the wrong facts of `subclasses.json` (Cross-cutting 7) | True; the entries with line numbers are in that finding. One detail: Call the Hunt's temporary hit points per ally are right; only "up to five allies" is wrong. |
| SRD text lives in `help/srd-features.json` (Cross-cutting 1) | The help file was checked: five of its lines are inaccurate (see the Hunter, Land, Devotion and Evocation findings). |
| Subclass choice levels (Cross-cutting 6) | True for all 13 classes. |

## Limits of this audit

- No printed book was consulted. Rows marked `Verified (2 sources)` rest on 5e.tools and the dnd5e wiki; rows marked `Verified (printed text)` rest on the SRD or official errata as stated.
- Bigby Presents (Path of the Giant), Explorer's Guide to Wildemount, Mythic Odysseys of Theros, Van Richten's and Fizban's have no publisher text or errata available, so their rows rest on 5e.tools and the wiki. The two differ on the name Giant Power versus Giant's Power; the app's Giant's Power is not flagged.
- Why the dnd5e wiki differs from the book in three places was not determined.
- The 48 invocations and the text of the Arcane Shot options are not in scope.
- Nothing was run in the app; statements about the engine come from reading the code. "Is not applied by the server" is a name-search heuristic, not a test.
- The 11 SRD "names only" rows (Berserker, Lore, Life, Champion, Open Hand, Devotion, Hunter, Thief, Draconic Bloodline, Fiend, Evocation) are not rules errors; they record that the data file carries feature names without text.

## Summary

**117 subclasses audited. 174 findings: 74 wrong, 9 missing, 91 imprecise.**

29 subclasses had no discrepancy that I could find; the other 88 have at least one finding.

Eleven of the imprecise rows only record that an SRD subclass ships feature names without text (Berserker, Lore, Life, Champion, Open Hand, Devotion, Hunter, Thief, Draconic Bloodline, Fiend, Evocation); they are not rules errors. The text of those features is audited under the other rows of the same subclasses.

| Class | Subclasses | 🔴 Wrong | 🟠 Missing | 🟡 Imprecise | Subclasses with no finding |
|---|---|---|---|---|---|
| Artificer | 4 | 2 | 0 | 8 | 0 |
| Barbarian | 8 | 9 | 2 | 8 | 0 |
| Bard | 8 | 6 | 1 | 3 | 3 |
| Cleric | 14 | 3 | 0 | 8 | 5 |
| Druid | 7 | 5 | 1 | 8 | 2 |
| Fighter | 10 | 5 | 0 | 7 | 2 |
| Monk | 10 | 6 | 1 | 6 | 4 |
| Paladin | 9 | 3 | 0 | 5 | 3 |
| Ranger | 8 | 7 | 1 | 6 | 1 |
| Rogue | 9 | 3 | 0 | 4 | 4 |
| Sorcerer | 8 | 4 | 1 | 3 | 2 |
| Warlock | 9 | 14 | 2 | 15 | 0 |
| Wizard | 13 | 7 | 0 | 10 | 3 |

### Verdict by subclass

| Class | Subclass | Source | Verdict |
|---|---|---|---|
| Artificer | [Alchemist](#artificer-alchemist) | Tasha's Cauldron of Everything | 🔴 1 🟡 3 |
| Artificer | [Armorer](#artificer-armorer) | Tasha's Cauldron of Everything | 🔴 1 🟡 2 |
| Artificer | [Artillerist](#artificer-artillerist) | Tasha's Cauldron of Everything | 🟡 1 |
| Artificer | [Battle Smith](#artificer-battle-smith) | Tasha's Cauldron of Everything | 🟡 2 |
| Barbarian | [Path of the Berserker](#barbarian-path-of-the-berserker) | SRD 5.1 / PHB | 🟡 1 |
| Barbarian | [Path of the Totem Warrior](#barbarian-path-of-the-totem-warrior) | PHB (Elk and Tiger options: Xanathar's) | 🔴 1 🟠 1 |
| Barbarian | [Path of the Ancestral Guardian](#barbarian-path-of-the-ancestral-guardian) | Xanathar's Guide to Everything | 🟡 1 |
| Barbarian | [Path of the Storm Herald](#barbarian-path-of-the-storm-herald) | PHB | 🔴 1 🟡 2 |
| Barbarian | [Path of the Zealot](#barbarian-path-of-the-zealot) | Xanathar's Guide to Everything | 🟡 1 |
| Barbarian | [Path of the Beast](#barbarian-path-of-the-beast) | Tasha's Cauldron of Everything | 🔴 1 🟡 1 |
| Barbarian | [Path of Wild Magic](#barbarian-path-of-wild-magic) | Tasha's Cauldron of Everything | 🟡 2 |
| Barbarian | [Path of the Giant](#barbarian-path-of-the-giant) | Bigby Presents: Glory of the Giants | 🔴 6 🟠 1 |
| Bard | [College of Lore](#bard-college-of-lore) | SRD 5.1 / PHB | 🟡 1 |
| Bard | [College of Valor](#bard-college-of-valor) | PHB | No discrepancy found |
| Bard | [College of Glamour](#bard-college-of-glamour) | Xanathar's Guide to Everything | 🔴 1 🟡 2 |
| Bard | [College of Swords](#bard-college-of-swords) | Xanathar's Guide to Everything | No discrepancy found |
| Bard | [College of Whispers](#bard-college-of-whispers) | Xanathar's Guide to Everything | No discrepancy found |
| Bard | [College of Eloquence](#bard-college-of-eloquence) | Mythic Odysseys of Theros / Tasha's | 🟠 1 |
| Bard | [College of Creation](#bard-college-of-creation) | Tasha's Cauldron of Everything | 🔴 3 |
| Bard | [College of Spirits](#bard-college-of-spirits) | Van Richten's Guide to Ravenloft | 🔴 2 |
| Cleric | [Life Domain](#cleric-life-domain) | SRD 5.1 / PHB | 🟡 1 |
| Cleric | [Knowledge Domain](#cleric-knowledge-domain) | PHB | 🟡 1 |
| Cleric | [Light Domain](#cleric-light-domain) | PHB | 🟡 1 |
| Cleric | [Nature Domain](#cleric-nature-domain) | PHB | No discrepancy found |
| Cleric | [Tempest Domain](#cleric-tempest-domain) | PHB | No discrepancy found |
| Cleric | [Trickery Domain](#cleric-trickery-domain) | PHB | No discrepancy found |
| Cleric | [War Domain](#cleric-war-domain) | PHB | 🟡 1 |
| Cleric | [Death Domain](#cleric-death-domain) | Dungeon Master's Guide | 🔴 1 |
| Cleric | [Forge Domain](#cleric-forge-domain) | Xanathar's Guide to Everything | No discrepancy found |
| Cleric | [Grave Domain](#cleric-grave-domain) | Xanathar's Guide to Everything | 🔴 1 🟡 1 |
| Cleric | [Order Domain](#cleric-order-domain) | Tasha's Cauldron of Everything | 🔴 1 |
| Cleric | [Peace Domain](#cleric-peace-domain) | Tasha's Cauldron of Everything | 🟡 1 |
| Cleric | [Twilight Domain](#cleric-twilight-domain) | Tasha's Cauldron of Everything | No discrepancy found |
| Cleric | [Arcana Domain](#cleric-arcana-domain) | Sword Coast Adventurer's Guide | 🟡 2 |
| Druid | [Circle of the Land](#druid-circle-of-the-land) | SRD 5.1 / PHB | 🟠 1 🟡 2 |
| Druid | [Circle of the Moon](#druid-circle-of-the-moon) | PHB | No discrepancy found |
| Druid | [Circle of Dreams](#druid-circle-of-dreams) | Xanathar's Guide to Everything | 🔴 1 🟡 3 |
| Druid | [Circle of the Shepherd](#druid-circle-of-the-shepherd) | Xanathar's Guide to Everything | 🔴 2 |
| Druid | [Circle of Spores](#druid-circle-of-spores) | Guildmasters' Guide to Ravnica | 🟡 2 |
| Druid | [Circle of Stars](#druid-circle-of-stars) | Tasha's Cauldron of Everything | No discrepancy found |
| Druid | [Circle of Wildfire](#druid-circle-of-wildfire) | Tasha's Cauldron of Everything | 🔴 2 🟡 1 |
| Fighter | [Champion](#fighter-champion) | SRD 5.1 / PHB | 🟡 1 |
| Fighter | [Battle Master](#fighter-battle-master) | PHB | 🟡 1 |
| Fighter | [Eldritch Knight](#fighter-eldritch-knight) | PHB | No discrepancy found |
| Fighter | [Arcane Archer](#fighter-arcane-archer) | Xanathar's Guide to Everything | 🟡 2 |
| Fighter | [Cavalier](#fighter-cavalier) | Xanathar's Guide to Everything | 🔴 1 🟡 2 |
| Fighter | [Samurai](#fighter-samurai) | Xanathar's Guide to Everything | 🔴 1 |
| Fighter | [Echo Knight](#fighter-echo-knight) | Explorer's Guide to Wildemount | 🔴 1 |
| Fighter | [Psi Warrior](#fighter-psi-warrior) | Tasha's Cauldron of Everything | 🟡 1 |
| Fighter | [Rune Knight](#fighter-rune-knight) | Tasha's Cauldron of Everything | No discrepancy found |
| Fighter | [Banneret](#fighter-banneret) | Sword Coast Adventurer's Guide (Purple Dragon Knight) | 🔴 2 |
| Monk | [Way of the Open Hand](#monk-way-of-the-open-hand) | SRD 5.1 / PHB | 🟡 1 |
| Monk | [Way of Shadow](#monk-way-of-shadow) | PHB | No discrepancy found |
| Monk | [Way of the Four Elements](#monk-way-of-the-four-elements) | PHB | 🟡 1 |
| Monk | [Way of the Drunken Master](#monk-way-of-the-drunken-master) | Xanathar's Guide to Everything | No discrepancy found |
| Monk | [Way of the Kensei](#monk-way-of-the-kensei) | Xanathar's Guide to Everything | 🔴 1 🟠 1 🟡 1 |
| Monk | [Way of the Sun Soul](#monk-way-of-the-sun-soul) | Xanathar's Guide to Everything | No discrepancy found |
| Monk | [Way of the Long Death](#monk-way-of-the-long-death) | Sword Coast Adventurer's Guide | 🟡 1 |
| Monk | [Way of Mercy](#monk-way-of-mercy) | Tasha's Cauldron of Everything | No discrepancy found |
| Monk | [Way of the Astral Self](#monk-way-of-the-astral-self) | Tasha's Cauldron of Everything | 🔴 2 🟡 1 |
| Monk | [Way of the Ascendant Dragon](#monk-way-of-the-ascendant-dragon) | Fizban's Treasury of Dragons | 🔴 3 🟡 1 |
| Paladin | [Oath of Devotion](#paladin-oath-of-devotion) | SRD 5.1 / PHB | 🟡 2 |
| Paladin | [Oath of the Ancients](#paladin-oath-of-the-ancients) | PHB | No discrepancy found |
| Paladin | [Oath of Vengeance](#paladin-oath-of-vengeance) | PHB | 🟡 1 |
| Paladin | [Oath of Conquest](#paladin-oath-of-conquest) | Xanathar's Guide to Everything | No discrepancy found |
| Paladin | [Oath of Redemption](#paladin-oath-of-redemption) | Xanathar's Guide to Everything | 🟡 1 |
| Paladin | [Oath of Glory](#paladin-oath-of-glory) | Mythic Odysseys of Theros / Tasha's | 🔴 1 |
| Paladin | [Oath of the Watchers](#paladin-oath-of-the-watchers) | Tasha's Cauldron of Everything | 🟡 1 |
| Paladin | [Oath of the Crown](#paladin-oath-of-the-crown) | Sword Coast Adventurer's Guide | No discrepancy found |
| Paladin | [Oathbreaker](#paladin-oathbreaker) | Dungeon Master's Guide | 🔴 2 |
| Ranger | [Hunter](#ranger-hunter) | SRD 5.1 / PHB | 🔴 1 🟡 2 |
| Ranger | [Beast Master](#ranger-beast-master) | PHB | 🟠 1 🟡 2 |
| Ranger | [Gloom Stalker](#ranger-gloom-stalker) | Xanathar's Guide to Everything | 🔴 1 |
| Ranger | [Horizon Walker](#ranger-horizon-walker) | Xanathar's Guide to Everything | No discrepancy found |
| Ranger | [Monster Slayer](#ranger-monster-slayer) | Xanathar's Guide to Everything | 🔴 1 |
| Ranger | [Fey Wanderer](#ranger-fey-wanderer) | Tasha's Cauldron of Everything | 🔴 1 |
| Ranger | [Swarmkeeper](#ranger-swarmkeeper) | Tasha's Cauldron of Everything | 🟡 1 |
| Ranger | [Drakewarden](#ranger-drakewarden) | Fizban's Treasury of Dragons | 🔴 3 🟡 1 |
| Rogue | [Thief](#rogue-thief) | SRD 5.1 / PHB | 🟡 1 |
| Rogue | [Assassin](#rogue-assassin) | PHB | No discrepancy found |
| Rogue | [Arcane Trickster](#rogue-arcane-trickster) | PHB | 🔴 1 |
| Rogue | [Mastermind](#rogue-mastermind) | Xanathar's Guide to Everything | No discrepancy found |
| Rogue | [Swashbuckler](#rogue-swashbuckler) | Xanathar's Guide to Everything | No discrepancy found |
| Rogue | [Inquisitive](#rogue-inquisitive) | Xanathar's Guide to Everything | 🟡 1 |
| Rogue | [Scout](#rogue-scout) | Xanathar's Guide to Everything | No discrepancy found |
| Rogue | [Phantom](#rogue-phantom) | Tasha's Cauldron of Everything | 🔴 2 |
| Rogue | [Soulknife](#rogue-soulknife) | Tasha's Cauldron of Everything | 🟡 2 |
| Sorcerer | [Draconic Bloodline](#sorcerer-draconic-bloodline) | SRD 5.1 / PHB | 🟡 1 |
| Sorcerer | [Wild Magic](#sorcerer-wild-magic) | PHB | No discrepancy found |
| Sorcerer | [Divine Soul](#sorcerer-divine-soul) | Xanathar's Guide to Everything | 🟠 1 |
| Sorcerer | [Shadow Magic](#sorcerer-shadow-magic) | Xanathar's Guide to Everything | 🔴 1 🟡 1 |
| Sorcerer | [Storm Sorcery](#sorcerer-storm-sorcery) | Xanathar's Guide to Everything | 🔴 1 |
| Sorcerer | [Aberrant Mind](#sorcerer-aberrant-mind) | Tasha's Cauldron of Everything | 🔴 1 🟡 1 |
| Sorcerer | [Clockwork Soul](#sorcerer-clockwork-soul) | Tasha's Cauldron of Everything | No discrepancy found |
| Sorcerer | [Lunar Sorcery](#sorcerer-lunar-sorcery) | Dragonlance: Shadow of the Dragon Queen | 🔴 1 |
| Warlock | [The Fiend](#warlock-the-fiend) | SRD 5.1 / PHB | 🟠 1 🟡 2 |
| Warlock | [The Archfey](#warlock-the-archfey) | PHB | 🔴 2 |
| Warlock | [The Great Old One](#warlock-the-great-old-one) | PHB | 🔴 1 |
| Warlock | [The Celestial](#warlock-the-celestial) | Xanathar's Guide to Everything | 🟡 3 |
| Warlock | [The Hexblade](#warlock-the-hexblade) | Xanathar's Guide to Everything | 🟡 6 |
| Warlock | [The Fathomless](#warlock-the-fathomless) | Tasha's Cauldron of Everything | 🔴 3 🟡 2 |
| Warlock | [The Genie](#warlock-the-genie) | Tasha's Cauldron of Everything | 🔴 3 🟠 1 🟡 1 |
| Warlock | [The Undead](#warlock-the-undead) | Van Richten's Guide to Ravenloft | 🔴 3 🟡 1 |
| Warlock | [The Undying](#warlock-the-undying) | Sword Coast Adventurer's Guide | 🔴 2 |
| Wizard | [School of Evocation](#wizard-school-of-evocation) | SRD 5.1 / PHB | 🟡 2 |
| Wizard | [School of Abjuration](#wizard-school-of-abjuration) | PHB | No discrepancy found |
| Wizard | [School of Conjuration](#wizard-school-of-conjuration) | PHB | No discrepancy found |
| Wizard | [School of Divination](#wizard-school-of-divination) | PHB | 🟡 2 |
| Wizard | [School of Enchantment](#wizard-school-of-enchantment) | PHB | 🔴 2 |
| Wizard | [School of Illusion](#wizard-school-of-illusion) | PHB | 🟡 1 |
| Wizard | [School of Necromancy](#wizard-school-of-necromancy) | PHB | 🔴 1 |
| Wizard | [School of Transmutation](#wizard-school-of-transmutation) | PHB | 🟡 1 |
| Wizard | [War Magic](#wizard-war-magic) | Xanathar's Guide to Everything | 🟡 1 |
| Wizard | [Bladesinging](#wizard-bladesinging) | Sword Coast Adventurer's Guide / Tasha's | No discrepancy found |
| Wizard | [Chronurgy Magic](#wizard-chronurgy-magic) | Explorer's Guide to Wildemount | 🔴 1 |
| Wizard | [Graviturgy Magic](#wizard-graviturgy-magic) | Explorer's Guide to Wildemount | 🔴 2 |
| Wizard | [Order of Scribes](#wizard-order-of-scribes) | Tasha's Cauldron of Everything | 🔴 1 🟡 3 |

### Most serious findings (wrong, verified)

| Class | Subclass | Where | Problem |
|---|---|---|---|
| Artificer | Alchemist | L9 Restorative Reagents | The number of free casts is your Intelligence modifier (minimum once), not your proficiency bonus, and the spell is cast without a slot or preparation only when alchemist's supplies are the spellcasting focus. Uses return on a lon… |
| Artificer | Armorer | L15 Perfected Armor | Guardian: a reaction, usable a proficiency-bonus number of times per long rest, when a Huge or smaller creature you can see ends its turn within 30 ft: Strength save or it is pulled up to 25 ft to a space near you, and if it lands… |
| Barbarian | Path of the Totem Warrior | L3 Totem Spirit: Eagle | While raging and not in heavy armor, other creatures have DISADVANTAGE on opportunity attack rolls against you, and you can Dash as a bonus action. The app text says 'no advantage', which is a different rule. |
| Barbarian | Path of the Storm Herald | L14 Raging Storm: Tundra | On a failed Strength save the creature's speed is reduced to 0 until the start of your next turn, not halved. |
| Barbarian | Path of the Beast | L14 Call the Hunt | The number of other willing creatures you can see within 30 ft is equal to your Constitution modifier (minimum one creature), not five. You gain 5 temporary hit points for each creature that accepts (the app has this right). Until… |
| Barbarian | Path of the Giant | Feature levels | The path has exactly five features: Giant Power 3, Giant's Havoc 3, Elemental Cleaver 6, Mighty Impel 10, Demiurgic Colossus 14. In the app Elemental Cleaver, Mighty Impel and Demiurgic Colossus are each one tier early, Giant's Ha… |
| Barbarian | Path of the Giant | L14 Elemental Fury | No such feature exists. The 14th-level feature is Demiurgic Colossus. |
| Barbarian | Path of the Giant | L3 Giant's Power | Learn Giant (or another language if you already know it) and one cantrip of your choice from druidcraft or thaumaturgy, with Wisdom as its spellcasting ability. |
| Barbarian | Path of the Giant | L3 Elemental Cleaver (placed at 3rd) | Belongs at 6th. When you rage, infuse a held weapon with acid, cold, fire, lightning or thunder: its damage type becomes that type AND it deals an extra 1d6 of it. It gains thrown (20/60) and returns; its benefits are suppressed i… |
| Barbarian | Path of the Giant | L6 Mighty Impel (placed at 6th) | Belongs at 10th. A Medium or smaller creature within your reach (not 10 ft); moved to an unoccupied space you see within 30 ft; DC 8 + proficiency + Strength to avoid; it falls and lands prone if unsupported. |
| Barbarian | Path of the Giant | L10 Demiurgic Colossus (placed at 10th) | Belongs at 14th. Reach +10 ft, you can become Large or Huge, Mighty Impel can move Large creatures, Elemental Cleaver's extra damage becomes 2d6. |
| Bard | College of Glamour | L3 Enthralling Performance | The number is equal to your Charisma modifier (minimum 1), not your proficiency bonus. |
| Bard | College of Creation | L3 Mote of Potential | Ability check: the creature rolls the die again and takes the better result. Attack roll: the mote bursts, thunder damage equal to the die roll (Constitution save, creatures within 5 ft). Saving throw: temporary hit points equal t… |
| Bard | College of Creation | L14 Creative Crescendo | The number of items equals your Charisma modifier (minimum two). Only one can be the maximum size; the rest must be Small or Tiny. You are no longer limited by gp value at all when creating items with Performance of Creation, not… |
| Bard | College of Creation | L6 Animating Performance | The Dancing Item stat block has Force-Empowered Slam: melee attack using your spell attack modifier, reach 5 ft, 1d10 + proficiency bonus force damage. It is not thunder damage and does not use Strength. It lives 1 hour, has 10 +… |
| Bard | College of Spirits | L6 Spirit Session | The ritual lasts an hour (during a short or long rest) with up to your proficiency bonus in willing creatures including you. The spell's level can be no higher than the NUMBER OF CREATURES who took part, must be a level you can ca… |
| Bard | College of Spirits | L14 Mystical Connection | When you roll on the Spirit Tales table you roll the die twice and choose which effect to bestow; if both dice show the same number you may choose any entry. There is no reroll and no free roll once per short rest. |
| Cleric | Death Domain | L17 Improved Reaper | No hit point cost exists. Improved Reaper simply lets a single-target necromancy spell of 1st through 5th level target two creatures within 5 feet of each other; you must provide the material components for each target if they are… |
| Cleric | Grave Domain | L2 Channel Divinity: Path to the Grave | The creature is cursed until the end of your next turn. The next time you or an ally hits it with an attack, it is vulnerable to ALL of that attack's damage, then the curse ends. There is no advantage on the attack. |
| Cleric | Order Domain | L2 Channel Divinity: Order's Demand | A failed Wisdom save charms the creature only until the end of your next turn (and the charm ends if it takes damage); you can also make charmed creatures drop whatever they hold. There is no push, and it is not a minute. |
| Druid | Circle of Dreams | L14 Walker in Dreams | When you finish a short rest you can cast dream (you as the messenger), scrying or teleportation circle without a slot or material components. Teleportation circle opens a portal to the last location where you finished a long rest… |
| Druid | Circle of the Shepherd | L14 Faithful Summons | If you are reduced to 0 hit points or are incapacitated against your will, you immediately gain the effects of conjure animals as if cast with a 9th-level slot: four beasts of your choice that are CR 2 or lower, appearing within 2… |
| Druid | Circle of the Shepherd | L2 Spirit Totem: Unicorn | Unicorn Spirit gives you and your allies advantage on checks to detect creatures in the aura, and when you cast a healing spell with a slot, chosen creatures in the aura regain hit points equal to your druid level. Attack advantag… |
| Druid | Circle of Wildfire | Circle Spells L3 | Flaming Sphere, Scorching Ray. Blur is not a Wildfire spell. |
| Druid | Circle of Wildfire | Circle Spells L5 | Plant Growth, Revivify. Fireball is not a Wildfire spell. |
| Fighter | Cavalier | L3 Unwavering Mark | The mark lasts until the end of your next turn. While it is within 5 ft of you, the marked creature has disadvantage on attack rolls that do not target you. If it deals damage to anyone other than you, you can make a special melee… |
| Fighter | Samurai | L18 Strength before Death | When damage reduces you to 0 hit points, you use your reaction to delay falling unconscious and immediately take an extra turn. During it, damage causes death save failures and you fall unconscious at the end of the turn if still… |
| Fighter | Echo Knight | L3 Manifest Echo | The echo has AC 14 + your proficiency bonus (not your AC), 1 hit point and immunity to all conditions, and uses your saving throw bonuses. You manifest it as a bonus action within 15 ft. On your turn you mentally command it to mov… |
| Fighter | Banneret | L7 Royal Envoy | Proficiency in Persuasion, or if you already have it, proficiency in one of Animal Handling, Insight, Intimidation or Performance; and your proficiency bonus is doubled on Persuasion checks either way. There is no extra language. |
| Fighter | Banneret | L15 Bulwark | It applies to Intelligence, Wisdom or Charisma SAVING THROWS: when you use Indomitable on one, choose an ally within 60 ft that also failed the same save and can see or hear you; it rerolls the save and must use the new result. |
| Monk | Way of the Kensei | L3 Path of the Kensei | Agile Parry: when you make an unarmed strike as part of the Attack action while wielding a kensei weapon, you gain +2 AC until the start of your next turn. It has nothing to do with ranged weapons or a bonus action. The bonus-acti… |
| Monk | Way of the Astral Self | L3 Arms of the Astral Self | On summoning, creatures you choose within 10 ft make a Dexterity save or take force damage equal to two rolls of your martial arts die. The arms then add 5 ft of reach and let you use Wisdom for attack and damage. There is no shov… |
| Monk | Way of the Astral Self | L11 Body of the Astral Self | Empowered Arms adds one extra roll of your martial arts die (not a fixed 1d10) once per turn when you hit with the arms. Deflect Energy reduces acid, cold, fire, force, lightning or thunder damage by 1d10 + Wisdom modifier. |
| Monk | Way of the Ascendant Dragon | L3 Draconic Disciple | Three parts: Draconic Presence (reaction to reroll a failed Intimidation or Persuasion check, once per long rest after it turns a failure into a success), Draconic Strike (change an unarmed strike's damage type) and Tongue of Drag… |
| Monk | Way of the Ascendant Dragon | L6 Wings Unfurled | Triggered by Step of the Wind: spectral wings give a flying speed equal to your walking speed until the end of the turn, a proficiency-bonus number of times per long rest. There is no 'airborne' condition. |
| Monk | Way of the Ascendant Dragon | L17 Ascendant Aspect | Three parts: Augment Breath (1 ki makes Breath of the Dragon a 60-ft cone or 90-ft line dealing four martial arts dice), Blindsight (10 ft) and Explosive Fury (when you activate Aspect of the Wyrm, creatures in the aura make a Dex… |
| Paladin | Oath of Glory | L20 Living Legend | Once on each of your turns a missed weapon attack hits instead (it is not rerolled), and when you fail a saving throw you can use your reaction to reroll it and must use the new roll (it is not turned into a success). Advantage on… |
| Paladin | Oathbreaker | L20 Dread Lord | The aura is gloom that turns bright light to dim, and the 4d10 psychic damage applies only to ENEMIES FRIGHTENED OF YOU when they start their turn there. Your bonus-action attack is a melee spell attack dealing 3d10 + Charisma mod… |
| Paladin | Oathbreaker | L3 Control Undead | As an action, one undead you can see within 30 ft must make a Wisdom saving throw (not Charisma). On a failure it obeys your commands for 24 hours or until you use this option again. An undead whose challenge rating is equal to or… |
| Ranger | Hunter | L15 Superior Hunter's Defense (help text, src/lib/help/srd-features.json line 133) | The three options are Evasion (no damage on a successful Dexterity save for half damage, half on a failure), Stand Against the Tide (when a hostile creature misses you with a melee attack, your reaction makes it repeat the attack… |
| Ranger | Gloom Stalker | L15 Shadowy Dodge | Whenever a creature makes an attack roll against you and does not have advantage on the roll, you can use your reaction to impose disadvantage on it. You must use the feature before you know the outcome of the roll, so it cannot b… |
| Ranger | Monster Slayer | Monster Slayer Magic L13 | Banishment. The list is Protection from Evil and Good (3), Zone of Truth (5), Magic Circle (9), Banishment (13), Hold Monster (17). |
| Ranger | Fey Wanderer | L11 Fey Reinforcements | You know summon fey; it does not count against your spells known and you can cast it without a material component. You can cast it once without a spell slot, regaining that on a long rest. Whenever you start casting it you can mod… |
| Ranger | Drakewarden | L7 Bond of Fang and Scale | The drake grows wings (fly speed = walking speed), becomes Medium and can be ridden, its Bite deals an extra 1d6 of its essence type, and you gain resistance to that type. Your own attacks do not become magical or deal extra damag… |
| Ranger | Drakewarden | L15 Perfected Bond | The drake becomes Large, its Bite's extra damage rises to 2d6 total, and Reflexive Resistance lets you give you or the drake resistance to damage as a reaction (proficiency-bonus uses per long rest). No extra hit points or profici… |
| Ranger | Drakewarden | L3 Drake Companion | The Drake Companion stat block has a Bite (1d6 + proficiency bonus piercing), a Draconic Essence trait (you choose acid, cold, fire, lightning or poison when you summon it; it is immune to that type) and an Infused Strikes reactio… |
| Rogue | Arcane Trickster | L17 Spell Thief | Immediately after a creature casts a spell that targets you or includes you in its area, you use your reaction to force it to make a saving throw with its spellcasting ability modifier against your spell save DC. On a failure you… |
| Rogue | Phantom | L9 Tokens of the Departed | While you carry a soul trinket you have advantage on Constitution saving throws and death saving throws. There is no poison resistance. |
| Rogue | Phantom | L17 Death's Friend | Wails from the Grave now also deals its necrotic damage to the first creature (not just the second). It is still your choice to use it, it still spends one of your proficiency-bonus uses (or a soul trinket), and it does not trigge… |
| Sorcerer | Shadow Magic | L1 Eyes of the Dark | 120 ft darkvision is level 1, but you learn darkness at 3rd level (not 1st). The sorcery-point casting and seeing through it are correct. |
| Sorcerer | Storm Sorcery | Storm Guide level | Storm Guide is a 6th-level feature (with Heart of the Storm). 14th has Storm's Fury only. |
| Sorcerer | Aberrant Mind | L14 Revelation in Flesh | As a bonus action you spend 1 or more sorcery points to transform for 10 minutes (not 1 minute). For each point spent you pick one benefit: see invisible creatures within 60 ft; fly at your walking speed and hover; swim at twice y… |
| Sorcerer | Lunar Sorcery | Whole subclass | Real features: Lunar Embodiment (1st: you learn fifteen spells as sorcerer spells that do not count against spells known, one per phase at sorcerer levels 1, 3, 5, 7 and 9 - Full Moon shield, lesser restoration, dispel magic, deat… |
| Warlock | The Archfey | L10 Beguiling Defenses | The reaction forces a Wisdom save; on a failure the creature is charmed by you for 1 minute or until it takes damage. There is no 3d10 psychic damage. |
| Warlock | The Archfey | L14 Dark Delirium | Action; a creature within 60 ft makes a Wisdom save or is charmed or frightened by you (your choice) for 1 minute or until your concentration is broken or it takes damage. It thinks it is lost in an illusory misty realm and can se… |
| Warlock | The Great Old One | L1 Awakened Mind | The creature does not need to share your language, but it must understand at least one language. |
| Warlock | The Fathomless | L6 Oceanic Soul | Resistance to cold damage, not immunity. In addition, when you are fully submerged, any creature that is also fully submerged can understand your speech and you can understand theirs (it is not telepathy and has no 120-ft range).… |
| Warlock | The Fathomless | Expanded spells L9 | Bigby's Hand and Cone of Cold. Commune with Nature is not on the Fathomless list. |
| Warlock | The Fathomless | L1 Tentacle of the Deep | A hit reduces the target's speed by 10 feet until the start of your next turn, not by half. The damage is 1d8 cold, 2d8 from 10th level. Uses are proficiency bonus per long rest (correct). The feature is named Tentacle of the Deep… |
| Warlock | The Genie | Expanded spells (all tiers) | Each tier has one spell every genie gets (detect evil and good, phantasmal force, create food and water, phantasmal killer, creation) and one by kind. Dao: sanctuary, spike growth, meld into stone, stone shape, wall of stone. Djin… |
| Warlock | The Genie | L1 Genie's Vessel: Bottled Respite | You stay for up to twice your proficiency bonus in HOURS, can leave early with a bonus action, and it returns after a long rest. The vessel is an object with AC equal to your spell save DC and hit points equal to warlock level + p… |
| Warlock | The Genie | L10 Sanctuary Vessel | Up to five willing creatures within 30 ft can enter; anyone who stays 10 minutes gets the benefits of a short rest, and Hit Dice spent inside add your proficiency bonus to the hit points regained. There are no temporary hit points… |
| Warlock | The Undead | Expanded spells L1 | Bane, False Life. Ray of Sickness is on the Undying list, not the Undead. |
| Warlock | The Undead | L10 Necrotic Husk | Resistance to necrotic damage, and immunity to it while transformed by Form of Dread. As a reaction when reduced to 0 hit points you drop to 1 instead, and creatures of your choice within 30 ft take 2d10 + your warlock level necro… |
| Warlock | The Undead | L14 Spirit Projection | Resistance to bludgeoning, piercing and slashing damage; flying speed and hover; moving through creatures costs force damage if you end inside one; while in Form of Dread you heal half of any necrotic damage you deal once per turn… |
| Warlock | The Undying | L1 Among the Dead | If an undead targets you directly with an attack or a harmful spell it makes a Wisdom save; on a failure it must choose a new target or lose the attack or spell. It does not protect against area effects. The creature is immune for… |
| Warlock | The Undying | L10 Undying Nature | You age one year for every ten that pass and cannot be aged magically. You also hold your breath indefinitely and need no food, water or sleep (you still need rest to reduce exhaustion). 'No longer age' contradicts the one-in-ten… |
| Wizard | School of Enchantment | L2 Hypnotic Gaze | Once you use it on a creature you cannot use it on that same creature again until you finish a long rest. It does not recharge on a short rest and isn't a pool for you. |
| Wizard | School of Enchantment | L6 Instinctive Charm | The limit is per attacker: on a successful save the attacker cannot be affected again until you finish a long rest. There is no short rest recharge. |
| Wizard | School of Necromancy | L14 Command Undead | An undead that fails becomes friendly and obeys you until you use the feature again. If its Intelligence is 8 or higher it has advantage on the save; if it fails and has Intelligence 12 or higher it repeats the save at the end of… |
| Wizard | Chronurgy Magic | L6 Momentary Stasis | Uses equal your Intelligence modifier (minimum 1) per long rest. The target is incapacitated with speed 0 until the end of your next turn or until it takes damage. |
| Wizard | Graviturgy Magic | L10 Violent Attraction | Uses equal your Intelligence modifier (minimum 1). It adds 1d10 to a weapon attack's damage from a creature you see within 60 ft, or 2d10 to fall damage. |
| Wizard | Graviturgy Magic | L14 Event Horizon | Once per long rest or by expending a spell slot of 3rd level or higher. Creatures that start their turn within 30 ft make a Strength save: failed they take 2d10 force and speed 0, success half damage and each foot costs 2 extra. |
| Wizard | Order of Scribes | L10 Master Scrivener | After a long rest, copy a 1st- or 2nd-level spell with a casting time of 1 action from your book onto a scroll; the spell counts as one level higher when cast; the scroll vanishes after being cast or at your next long rest. No max… |

## Cross-cutting findings

1. **SRD subclasses carry names only.** The twelve SRD subclasses (Berserker, Lore, Life, Land, Champion, Open Hand, Devotion, Hunter, Thief, Draconic Bloodline, Fiend, Evocation) have feature names and levels but no description or numbers in `class-features.json`. Their text lives elsewhere (`help/srd-features.json`, the rulebook), so the same audit of numbers was not possible from the subclass files. The help text for these features was checked against the SRD 5.1: five lines are inaccurate (rows added under Circle of the Land, Oath of Devotion, Hunter and School of Evocation) and the rest read correctly or only abbreviate.
2. **Warlock patron spells are granted for free.** Cleric domain, paladin oath, druid circle and ranger/sorcerer/artificer subclass spells are always-prepared or always-known and do not count against the limit, which matches the rules. The warlock patron lists are different: they are an *expanded spell list*. The rules add them to the pool the warlock picks from; they are not granted. `subclassSpellsFor` in `src/lib/srd/features.ts` treats all of them as free, so an Archfey, Great Old One, Celestial, Hexblade, Fathomless, Genie, Undead or Undying warlock receives ten spells they should have to learn with their limited spells known. This is more generous than the rules, and the publisher confirms the rule: the Sage Advice Compendium (2014) says expanded-list spells are not automatically known, and the SRD says the Fiend's list is 'added to the warlock spell list for you'. The Fiend has no list at all, so it is the only patron that does not receive the free spells, and is the one that is missing a list.
3. **Several 'use' limits are wrong in the same way.** Where a feature's uses depend on an ability modifier, the data often says 'proficiency bonus' (Glamour's Enthralling Performance, Alchemist's Restorative Reagents, Chronurgy's Momentary Stasis, Graviturgy's Violent Attraction), gives a fixed count (Beast's Call the Hunt: 'five' instead of the Constitution modifier) or leaves the limit out (Battle Smith's Arcane Jolt, Peace's Emboldening Bond, Wildfire's Cauterizing Flames, Order of Scribes' Manifest Mind). These need a pass for every 'a proficiency-bonus number of times per long rest' string in the subclass file.
4. **Edition mix.** The data mostly follows the 2014 books. Zealot's Warrior of the Gods follows Xanathar's; the 2024 Player's Handbook version is different. A statement of which edition the app follows would settle several of the 'imprecise' items.
5. **Feature descriptions are single sentences.** Many of the 'imprecise' findings are only omitted numbers (damage dice, temporary hit point amounts, scaling by level, save types). If the descriptions are shown to players as the rules text, these are the gaps they will notice first.
6. **Subclass choice level is correct for every class.** Artificer, Barbarian, Bard, Fighter, Monk, Paladin, Ranger and Rogue pick at 3, Cleric, Sorcerer and Warlock at 1, Druid and Wizard at 2. No finding. Checked against `class-features.json` (`subclassLevel`).
7. **The same wrong facts are repeated in `authored-resources.json`, and it has errors of its own.** Its `guidance` strings, which the DM prompt uses for resource-backed features, and its numeric fields restate the errors found here and add others. The table below lists every entry found, with line numbers (the file has 141 entries: 126 subclass features, 8 race features and 7 artificer features). Fixing `subclasses.json` alone will not fix what the DM is told. Entries that are right where `subclasses.json` is wrong or incomplete: Unwavering Mark (line 1783), Strength before Death (line 154), Cauterizing Flames (line 1772) and One with the Word (line 1185). The same wrong facts also appear in code: `src/lib/srd/condition-effects.ts` line 650 (Tentacle of the Deep halves speed) and `src/lib/srd/authored-conditions.ts` lines 220-233 (Dread Lord: 4d10 psychic to all enemies and a 4d10 psychic strike).

**Cross-cutting 7 in detail: `src/lib/srd/authored-resources.json`.** Lines are for the field named. Not counted in the Summary.

| Entry | Line | What the file says | What the rules say |
|---|---|---|---|
| `sub_call_the_hunt` | 621 | "up to five allies" | Constitution modifier (minimum one) willing creatures. The temporary hit points per creature are right. |
| `sub_faithful_summons` | 344 | "four dire wolves appear", only when you drop to 0 | Four beasts of your choice, CR 2 or lower; also triggers when you are incapacitated against your will. |
| `sub_walker_in_dreams` | 406, 407 | Recharge "short"; no destination | Once per long rest; teleportation circle opens to the last place you finished a long rest. |
| `sub_enthralling_performance` | 418 | A proficiency-bonus number of humanoids | Charisma modifier (minimum 1). |
| `sub_tentacle_of_the_deep` | 734 | "Halves the target's speed" | Speed reduced by 10 ft until the start of your next turn. |
| `sub_genies_vessel` | 838 | "Up to 10 minutes"; companions "gain temporary hit points" | Up to twice your proficiency bonus in hours; no temporary hit points (short-rest benefits and extra hit points from Hit Dice). |
| `sub_fey_reinforcements` | 1294 | "The summoned fey can turn invisible" | No invisibility; it can drop concentration (1 minute). |
| `sub_spell_thief` | 1392 | "Letting the rogue cast it once" | You know and can cast the spell for 8 hours. |
| `sub_manifest_mind` | 1157 | "Up to 300 feet" | Appears within 60 ft, moves 30 ft per bonus action, ends beyond 300 ft; casts from it a proficiency-bonus number of times a day. |
| `sub_event_horizon` | 1140 | "A 4th-level slot" | A slot of 3rd level or higher. |
| `sub_violent_attraction` | 1125, 1128 | Proficiency-bonus uses; "2d10 per 10 feet" of a fall | Intelligence-modifier uses; a flat extra 2d10 fall damage. |
| `sub_momentary_stasis` | 1096 | Proficiency-bonus uses | Intelligence-modifier uses. |
| `sub_instinctive_charm`, `sub_hypnotic_gaze` | 1015, 1027 | Recharge "short" | No short-rest recharge; per creature until a long rest. |
| `sub_master_scrivener` | 1173 | "Cast at maximum effect" | Counts as one level higher; no maximum effect. |
| `sub_spirit_projection` | 794 | "Resistance to most damage"; psychic spell damage heals | Resistance to bludgeoning, piercing, slashing; in Form of Dread you heal half the necrotic damage you deal, once per turn. |
| `sub_wings_unfurled` | 1464 | "While airborne" | Triggered by Step of the Wind. |
| `sub_bolstering_magic` | 593 | Restore a slot of 3rd level or lower | A d3 roll caps the slot level; each creature once per long rest. |
| `sub_perfected_armor` | 1941 | Large or smaller within 30 ft, "pulled in and knocked prone" | Huge or smaller, pulled up to 25 ft, no prone. |
| `sub_dread_lord` | 1890 | 4d10 psychic to enemies in the aura; a 4d10 psychic spectral strike | 4d10 psychic only to enemies frightened of you; the strike is a melee spell attack for 3d10 + Charisma necrotic. |
| `sub_living_legend` | 1854 | "The reroll of a miss and the turned save" | A miss becomes a hit; a failed save is rerolled. |
| `sub_strength_of_the_grave` | 900 | No exclusions | Not against radiant damage or a critical hit. |
| `sub_psionic_energy` | 43 | "One die returns on a short rest" | A bonus action returns one die, once per short or long rest. |
| `art_restorative_reagents` | 1724 | Proficiency-bonus uses | Intelligence-modifier uses. |
| `sub_mantle_of_majesty` | 435 | "A creature charmed by the bard cannot attack them" | A creature charmed by you automatically fails its save against the command. |
| `sub_lunar_phenomenon` | 971 | "Healing, blinding radiance or a burst of speed" | Full Moon: blind and heal; New Moon: necrotic damage, speed 0 and invisibility; Crescent Moon: teleport and resistance. |
| `sub_fighting_spirit` | 60 | `tempHp` is the constant "5" | 5, 10 and 15 at 3rd, 10th and 15th level (the guidance on line 55 says so). |
| `sub_eyes_of_the_grave`, `sub_magic_awareness` | 218, 580 | "Through walls"; "through most barriers" | Blocked by total cover. |
| `sub_eyes_of_night` | 239 | `ability` "wis", so uses equal the Wisdom modifier | One use per long rest (a spell slot of any level restores it); the Wisdom modifier is the number of creatures. |
| `sub_balm_of_the_summer_court` | 279 | "Spend any number" of dice | At most half your druid level per use. |
| `sub_necrotic_husk`, `sub_limited_wish` | 780, 848 | Recharge "long" | Returns after 1d4 long rests. |
| `sub_bladesong` | 1067 | Recharge "short" | Long rest (Sword Coast errata). |
| `sub_chronal_shift` | 1086 | "Before or after the result is known" | After you see the result. |
| `sub_drakes_breath` | 1321 | `dice` constant "8d6" | 10d6 from 15th level. |
| `sub_breath_of_the_dragon` | 1445 | `dice` constant "2d6" | Two rolls of the martial arts die, three from 11th level. |
| `sub_psychic_veil` | 1363 | `rounds` 100 (10 minutes) | 1 hour. |
| `art_experimental_elixir` | 1655-1666 | Scale 1, 2, 3 at levels 3, 9, 15 | 1 at 3rd, 2 at 6th, 3 at 15th. |


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
| 1 | 🔴 Wrong | L9 Restorative Reagents | Cast lesser restoration without a slot a proficiency-bonus number of times per long rest. | The number of free casts is your Intelligence modifier (minimum once), not your proficiency bonus, and the spell is cast without a slot or preparation only when alchemist's supplies are the spellcasting focus. Uses return on a long rest. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L9 Restorative Reagents | Elixirs also grant temporary hit points (no amount). | Drinking an Experimental Elixir grants 2d6 + Intelligence modifier temporary hit points. | Verified (2 sources) |
| 3 | 🟡 Imprecise | L5 Alchemical Savant | Add Intelligence modifier to one damage or healing roll of any spell cast through alchemical tools. | The bonus applies to one roll of a spell cast with alchemist's supplies as the focus, and only if that roll restores hit points or deals acid, fire, necrotic or poison damage. The bonus equals your Intelligence modifier (minimum +1). It is not for every damage type. | Verified (2 sources) |
| 4 | 🟡 Imprecise | L3 Experimental Elixir | Random effect, more by spending spell slots, number rises with level. | One elixir after a long rest (two at 6th, three at 15th). Effects on a d6: healing 2d4 + INT; swiftness +10 ft speed 1 hour; resilience +1 AC 10 minutes; boldness d4 to attacks and saves 1 minute; flight 10 ft fly 10 minutes; transformation as alter self 10 minutes. Extra elixirs cost a spell slot of 1st level or higher, one chosen effect. The app text gives none of the numbers or the six effects' numbers. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L3 Armor Model | Guardian and Infiltrator described in one line. | Guardian: Thunder Gauntlets (1d8 thunder, target has disadvantage attacking others until your next turn) and Defensive Field (bonus action, temporary hit points equal to artificer level, proficiency-bonus uses per long rest). Infiltrator: Lightning Launcher (1d6 lightning, ranged 90/300, +1d6 once per turn to a creature), Powered Steps (+5 ft speed), Dampening Field (advantage on Stealth). Numbers are missing from the app text. | Verified (2 sources) |
| 2 | 🔴 Wrong | L15 Perfected Armor | Guardian armor pulls in enemies within 30 feet and knocks them prone; Infiltrator armor makes a struck target glow, granting attackers advantage and dealing extra force damage. | Guardian: a reaction, usable a proficiency-bonus number of times per long rest, when a Huge or smaller creature you can see ends its turn within 30 ft: Strength save or it is pulled up to 25 ft to a space near you, and if it lands within 5 ft you can make a melee weapon attack against it. There is no prone. Infiltrator: a creature damaged by the Lightning Launcher glows, has disadvantage on attacks against you, the next attack roll against it has advantage, and that hit deals an extra 1d6 lightning (not force) damage. | Verified (2 sources) |
| 3 | 🟡 Imprecise | L9 Armor Modifications | Armor counts as four separate items for infusions. | Also: you can infuse two more items at once, and those two must be part of your Arcane Armor. The app text leaves out the two extra infusions. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L3 Eldritch Cannon | Flamethrower cone, force ballista, protector; once per long rest or by spending a slot. | Cannon has AC 18 and hit points equal to 5 x artificer level. Flamethrower: 15-ft cone, Dexterity save, 2d8 fire. Force Ballista: ranged spell attack 120 ft, 2d8 force, pushes 5 ft. Protector: 1d8 + INT modifier temporary hit points to creatures within 10 ft. Lasts 1 hour. None of the numbers are in the app text. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L9 Arcane Jolt (use limit) | Once per turn, a hit deals an extra 2d6 force or heals 2d6. | It is limited to a number of uses per long rest equal to your Intelligence modifier (minimum 1). The app text has no use limit. The healing goes to a creature or object you can see within 30 feet of the target you hit, not within 30 feet of you. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L3 Steel Defender | Force-empowered bite; reaction to impose disadvantage. | The defender attacks with Force-Empowered Rend (melee, your spell attack modifier, 1d8 + proficiency bonus force damage), not a bite, and has Deflect Attack (reaction: disadvantage on the attack roll of a creature within 5 ft of it, when the attack is against a creature other than the defender). Hit points are 2 + Intelligence modifier + 5 x artificer level. The stat block is reproduced in the official Eberron: Rising from the Last War errata. | Verified (printed text) |

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
| 1 | 🟡 Imprecise | All levels (SRD) | Feature names only (Frenzy, Mindless Rage, Intimidating Presence, Retaliation); no description text in the data file. | Frenzy 3 (bonus-action melee attack each turn while raging; one level of exhaustion when the rage ends), Mindless Rage 6 (immune to charmed and frightened while raging), Intimidating Presence 10 (action; frighten a creature within 30 ft, Wisdom save DC 8 + PB + CHA), Retaliation 14 (reaction melee attack against a creature within 5 ft that damaged you). Names and levels are correct. | Verified (printed text) |

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
| 1 | 🔴 Wrong | L3 Totem Spirit: Eagle | No opportunity-attack advantage against you. | While raging and not in heavy armor, other creatures have DISADVANTAGE on opportunity attack rolls against you, and you can Dash as a bonus action. The app text says 'no advantage', which is a different rule. | Verified (2 sources) |
| 2 | 🟠 Missing | L3 / L6 / L14 totem options | Only Bear, Eagle and Wolf are described. | Xanathar's adds Elk and Tiger at all three levels. Totem Spirit: Elk +15 ft walking speed while raging and not wearing heavy armor; Tiger +10 ft to long jump and +3 ft to high jump while raging. Aspect of the Beast: Elk doubles travel pace for you and up to ten companions within 60 ft; Tiger gives proficiency in two of Athletics, Acrobatics, Stealth, Survival. Totemic Attunement: Elk lets you pass through a Large or smaller creature's space as a bonus action during your move (Strength save, DC 8 + Strength + proficiency, or prone and 1d12 + Strength bludgeoning); Tiger gives a bonus-action extra melee attack after moving 20 ft straight at a Large or smaller target. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L3 Ancestral Protectors | Target has disadvantage on attacks against anyone but you; its damage against others is halved. | Until the start of your next turn the target has disadvantage on any attack roll that isn't against you, and when it hits a creature other than you, that creature has resistance to the damage of that attack. The effect ends early if your rage ends. The app says the damage is halved; the rule is resistance (which is also halving, but it applies to the creature hit and interacts with other resistances). | Verified (2 sources) |

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
| 1 | 🔴 Wrong | L14 Raging Storm: Tundra | Halves a creature's speed on a failed Strength save. | On a failed Strength save the creature's speed is reduced to 0 until the start of your next turn, not halved. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L3 Storm Aura | Activated as a bonus action while raging; Desert deals 2 fire to every other creature in it. | The aura's effect activates when you enter your rage, and you can activate it again on each of your turns as a bonus action. You can change your environment whenever you gain a level. Numbers scale with barbarian level: Desert 2 fire at 3rd (3 at 5th, 4 at 10th, 5 at 15th, 6 at 20th), Sea 1d6 lightning at 3rd (2d6 at 10th, 3d6 at 15th, 4d6 at 20th, Dexterity save for half), Tundra 2 temporary hit points at 3rd (3, 4, 5, 6 at the same levels). Save DC 8 + proficiency + Constitution. The app states that scaling exists but gives no table. | Verified (2 sources) |
| 3 | 🟡 Imprecise | L14 Raging Storm: Desert | Desert deals fire damage to a creature that hits you, once per turn. | Desert: immediately after a creature in your aura hits you, use your reaction to force it to make a Dexterity saving throw; on a failure it takes fire damage equal to half your barbarian level. There is no save mentioned in the app text and no once-per-turn limit (it is limited by your reaction). Sea is also a reaction when you hit a creature in the aura (Strength save or prone), and Tundra rides on activating the aura. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L14 Rage Beyond Death | While raging, dropping to 0 hit points does not knock you out; you fight on and only die if the rage ends while you are still at 0. | While you are raging, having 0 hit points does not knock you unconscious, but you still make death saving throws and suffer the normal effects of taking damage at 0 hit points. If you would die from failing death saving throws you do not die until your rage ends, and then only if you still have 0 hit points. The app text drops the death saving throws and makes the end of the rage the only way to die. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L3 Form of the Beast: numbers | Bite 1d8 and heals below half hit points; Claws 1d6 with an extra attack; Tail 1d8 with 10-foot reach. | Bite: once per turn, a bite that hits while you are below half your hit points restores hit points equal to your proficiency bonus. Claws: one extra claw attack each time you take the Attack action. Tail: reaction to add 1d8 to your AC against one attack. The app text gives the dice but not the heal amount. | Verified (2 sources) |
| 2 | 🔴 Wrong | L14 Call the Hunt | Grant up to five allies an extra d6 of damage and gain 5 temporary hit points for each ally who accepts. | The number of other willing creatures you can see within 30 ft is equal to your Constitution modifier (minimum one creature), not five. You gain 5 temporary hit points for each creature that accepts (the app has this right). Until the rage ends each chosen creature can, once on each of its turns, roll a d6 and add it to damage after a hit. Uses are proficiency bonus per long rest (the app has this right). | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L6 Bolstering Magic | Restore one expended spell slot of 3rd level or lower. | The slot you regain is capped by a d3 roll (level up to the roll), not a free choice of up to 3rd, and each creature can get the slot benefit only once per long rest. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L3 Wild Surge | Roll on your wild magic table each time you rage. | A d8 table: 1 necrotic burst (Con save) with temp HP; 2 teleport 30 ft; 3 flumph/pixie spirit force burst; 4 weapon becomes force-damage, light, thrown; 5 reprisal 1d6 force; 6 +1 AC for you and allies; 7 difficult terrain; 8 bolt of light, blinds. Several entries can be re-triggered as a bonus action. The table is not in the subclass data; the DC is 8 + proficiency + Constitution. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | Feature levels | Giant's Power 3, Elemental Cleaver 3, Mighty Impel 6, Demiurgic Colossus 10, Elemental Fury 14. | The path has exactly five features: Giant Power 3, Giant's Havoc 3, Elemental Cleaver 6, Mighty Impel 10, Demiurgic Colossus 14. In the app Elemental Cleaver, Mighty Impel and Demiurgic Colossus are each one tier early, Giant's Havoc is missing and Elemental Fury is invented. | Verified (2 sources) |
| 2 | 🟠 Missing | L3 Giant's Havoc | Not present. | While raging: Crushing Throw (add rage damage to a Strength-based thrown weapon hit) and Giant Stature (reach +5 ft; if smaller than Large you become Large). | Verified (2 sources) |
| 3 | 🔴 Wrong | L14 Elemental Fury | Once per turn add rage damage again, or force a Constitution save for 3d8 of your element. | No such feature exists. The 14th-level feature is Demiurgic Colossus. | Verified (2 sources) |
| 4 | 🔴 Wrong | L3 Giant's Power | Learn Giant and the druidcraft cantrip. | Learn Giant (or another language if you already know it) and one cantrip of your choice from druidcraft or thaumaturgy, with Wisdom as its spellcasting ability. | Verified (2 sources) |
| 5 | 🔴 Wrong | L3 Elemental Cleaver (placed at 3rd) | One weapon deals your chosen elemental damage instead of its own type and gains the thrown property with a 20/60 range, returning to your hand each throw. | Belongs at 6th. When you rage, infuse a held weapon with acid, cold, fire, lightning or thunder: its damage type becomes that type AND it deals an extra 1d6 of it. It gains thrown (20/60) and returns; its benefits are suppressed in other hands; a bonus action changes the type. The app text omits the extra 1d6. | Verified (2 sources) |
| 6 | 🔴 Wrong | L6 Mighty Impel (placed at 6th) | Move a Large or smaller creature within 10 feet up to 30 feet; an unwilling one gets a Strength save. | Belongs at 10th. A Medium or smaller creature within your reach (not 10 ft); moved to an unoccupied space you see within 30 ft; DC 8 + proficiency + Strength to avoid; it falls and lands prone if unsupported. | Verified (2 sources) |
| 7 | 🔴 Wrong | L10 Demiurgic Colossus (placed at 10th) | Grow to Large, reach +5, cleaver throw range doubles and damage grows by 1d10. | Belongs at 14th. Reach +10 ft, you can become Large or Huge, Mighty Impel can move Large creatures, Elemental Cleaver's extra damage becomes 2d6. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Cutting Words, Bonus Proficiencies (Lore), Additional Magical Secrets, Peerless Skill. | Names and levels match (3, 3, 6, 14). Cutting Words: reaction, spend a Bardic Inspiration die to subtract from a creature's attack, check or damage roll within 60 ft. Bonus Proficiencies: three skills. Additional Magical Secrets: two spells from any class. Peerless Skill: add a Bardic Inspiration die to your own check. | Verified (printed text) |

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
| 1 | 🔴 Wrong | L3 Enthralling Performance | Charm up to a proficiency-bonus number of humanoids. | The number is equal to your Charisma modifier (minimum 1), not your proficiency bonus. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L3 Mantle of Inspiration | Temporary hit points and a reaction-free move. | A number of creatures equal to your Charisma modifier (minimum 1), within 60 ft, each gain 5 temporary hit points (8 at 5th, 11 at 10th, 14 at 15th) and can use a reaction to move up to their speed without provoking. The app text omits the count and the scaling. | Verified (2 sources) |
| 3 | 🟡 Imprecise | L6 Mantle of Majesty | Cast command as a bonus action for a minute without a slot, with listeners charmed against retaliating; once per long rest. | As a bonus action you cast command without a slot and take on an appearance of unearthly beauty for 1 minute or until your concentration ends; on each of your turns you can cast command again as a bonus action, no slot. The only rider is that any creature charmed by you automatically fails its saving throw against that command. Nothing stops charmed listeners from retaliating, so 'charmed against retaliating' is garbled. | Verified (2 sources) |

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
| 1 | 🟠 Missing | L6 Universal Speech | Not present. | At 6th level Eloquence also gains Universal Speech: an action, up to a number of creatures within 60 ft equal to your Charisma modifier (minimum one) understand you for 1 hour. Once used, it cannot be used again until a long rest unless you expend a spell slot. It is a level 6 feature alongside Unfailing Inspiration. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L3 Mote of Potential | Extra damage on an attack, temporary hit points on a check, or an ally bonus on a save. | Ability check: the creature rolls the die again and takes the better result. Attack roll: the mote bursts, thunder damage equal to the die roll (Constitution save, creatures within 5 ft). Saving throw: temporary hit points equal to the die roll + your Charisma modifier. The app swaps the check and save effects and invents an ally save bonus. | Verified (2 sources) |
| 2 | 🔴 Wrong | L14 Creative Crescendo | Performance of Creation makes several items at once and drops the cost limit on all but the largest. | The number of items equals your Charisma modifier (minimum two). Only one can be the maximum size; the rest must be Small or Tiny. You are no longer limited by gp value at all when creating items with Performance of Creation, not only for the smaller items. | Verified (2 sources) |
| 3 | 🔴 Wrong | L6 Animating Performance | The dancing companion can make a thunderous Strength attack. | The Dancing Item stat block has Force-Empowered Slam: melee attack using your spell attack modifier, reach 5 ft, 1d10 + proficiency bonus force damage. It is not thunder damage and does not use Strength. It lives 1 hour, has 10 + 5 x bard level hit points, and its Irrepressible Dance changes speed of nearby creatures by 10 ft. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | L6 Spirit Session | Learn one divination or necromancy spell of a level up to the ritual's length. | The ritual lasts an hour (during a short or long rest) with up to your proficiency bonus in willing creatures including you. The spell's level can be no higher than the NUMBER OF CREATURES who took part, must be a level you can cast, and must be divination or necromancy from any class's list. You keep it until you start a long rest. The app ties the level to the ritual's length. | Verified (2 sources) |
| 2 | 🔴 Wrong | L14 Mystical Connection | Reroll an unwanted Tales from Beyond result, and roll for it without spending a die once per short rest. | When you roll on the Spirit Tales table you roll the die twice and choose which effect to bestow; if both dice show the same number you may choose any entry. There is no reroll and no free roll once per short rest. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Bonus Proficiency (heavy armor), Disciple of Life, Preserve Life, Blessed Healer, Divine Strike, Supreme Healing. Domain spells are correct. | Names, levels (1, 1, 2, 6, 8, 17) and the domain spell list all match the SRD. Descriptions are not in the subclass file. | Verified (printed text) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L17 Visions of the Past | A minute of concentration reveals an object's or a place's recent history. | You spend at least 1 minute in meditation and may keep it up for a number of minutes equal to your Wisdom score, concentrating as on a spell. Object Reading shows how the holder of an object acquired and lost it and the most recent significant event involving them (earlier owners within a number of days equal to your Wisdom score, 1 extra minute each); Area Reading shows significant events in a space up to a 50-foot cube, going back a number of days equal to your Wisdom score. Once used it needs a short or long rest. The app gives no duration, no reach back in time and no use limit. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L6 name | Improved Warding Flare. | The feature is named Improved Flare. Its effect (Warding Flare for a creature other than you) is described correctly. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L2 Channel Divinity: Guided Strike | Add +10 to one attack roll, before or after seeing the result. | You choose after you see the roll but before the DM says whether it hits or misses; it cannot be used before the roll. (The app words the identical Oath of Conquest feature correctly.) | Verified (2 sources) |

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
| 1 | 🔴 Wrong | L17 Improved Reaper | Cast a necromancy spell of 1st to 5th level that targets one creature at two creatures within 5 feet, at the cost of 1d8 of your own hit points per spell level. | No hit point cost exists. Improved Reaper simply lets a single-target necromancy spell of 1st through 5th level target two creatures within 5 feet of each other; you must provide the material components for each target if they are consumed. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | L2 Channel Divinity: Path to the Grave | The next attack against the cursed creature has advantage and doubles its damage vulnerability for that hit. | The creature is cursed until the end of your next turn. The next time you or an ally hits it with an attack, it is vulnerable to ALL of that attack's damage, then the curse ends. There is no advantage on the attack. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L17 Keeper of Souls | Once per turn, when an enemy within 60 feet dies, an ally you can see regains hit points equal to its Hit Dice. | When an enemy you can see dies within 60 ft of you, you or one creature of your choice within 60 ft regains hit points equal to the enemy's number of Hit Dice. You cannot use it while incapacitated, and after using it you cannot use it again until the start of your next turn (once per round, not once per turn). The app says 'an ally' and omits that you can be the recipient and the incapacitated restriction. The 60 ft range is right: the 30 ft and 'one ally' wording on the dnd5e wiki is not the Xanathar's text (see Third review). | Verified (2 sources) |

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
| 1 | 🔴 Wrong | L2 Channel Divinity: Order's Demand | Creatures of your choice within 30 feet are charmed for a minute on a failed Wisdom save, and you may drop or push one of them 30 feet. | A failed Wisdom save charms the creature only until the end of your next turn (and the charm ends if it takes damage); you can also make charmed creatures drop whatever they hold. There is no push, and it is not a minute. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L1 Emboldening Bond | Bond a proficiency-bonus number of creatures for 10 minutes; while near each other they may add a d4 to an attack, check or save, once per turn. | As an action you choose that many willing creatures within 30 ft (you may be one). While any bonded creature is within 30 ft of another it can add the d4. You can use the feature a number of times equal to your proficiency bonus per long rest. The app omits the uses per long rest and both 30 ft ranges. Expansive Bond makes both the bond and Protective Bond work at 60 ft and gives the creature that takes the damage resistance to it. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L2 Channel Divinity: Arcane Abjuration (Arcane Banishment rider) | Turn one celestial, elemental, fey or fiend within 30 feet on a failed Wisdom save, banishing weaker ones outright at higher levels. | After you reach 5th level, a creature that fails the save is also banished for 1 minute (as banishment, no concentration) if it is not on its plane of origin and its challenge rating is at or below a limit: 1/2 at 5th, 1 at 8th, 2 at 11th, 3 at 14th, 4 at 17th. This is part of Arcane Abjuration, not a separate feature. The app gives no thresholds. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L17 Arcane Mastery | Add four wizard spells of 6th, 7th, 8th and 9th level to your cleric spell list. | Choose one wizard spell each of 6th, 7th, 8th and 9th level; they become domain spells (always prepared) and count as cleric spells. | Verified (2 sources) |

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
| 1 | 🟠 Missing | Circle Spells (terrain lists) | Feature name only (listed at 3rd level); no spell list in the data. | When you join the circle you choose a land (Arctic, Coast, Desert, Forest, Grassland, Mountain and Swamp in the SRD; Underdark in the Player's Handbook), each with its own always-prepared spells at druid levels 3, 5, 7 and 9 (for example Forest: barkskin, spider climb; call lightning, plant growth; divination, freedom of movement; commune with nature, tree stride). No terrain choice or terrain lists were found in the subclass data (builder not checked). The SRD and 5e.tools list the feature at 2nd level; only the spells begin at 3rd. | Verified (2 sources) |
| 2 | 🟡 Imprecise | Circle Spells level (SRD) | Names only: Bonus Cantrip, Natural Recovery, Circle Spells, Land's Stride, Nature's Ward, Nature's Sanctuary. Circle Spells is listed at 3rd level. | The names match the SRD 5.1, and so do the levels except Circle Spells. The SRD says the circle grants features at 2nd, 6th, 10th and 14th level; Circle Spells is a 2nd-level feature whose spells arrive at 3rd, 5th, 7th and 9th. The data lists it at 3rd.  | Verified (printed text) |
| 3 | 🟡 Imprecise | L6 Land's Stride (help text, src/lib/help/srd-features.json line 80) | Difficult terrain from plants costs you no extra movement, and magical plants cannot slow or hurt you without a save. | Moving through nonmagical difficult terrain of any kind costs no extra movement; you also pass through nonmagical plants without being slowed or damaged; and you have advantage on saving throws against plants magically created or manipulated to impede movement. The help text narrows the first part to plant terrain and replaces the advantage with 'without a save'. | Verified (printed text) |

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

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L14 Walker in Dreams | After a short rest, cast dream, scrying or teleportation circle without a slot, the circle keyed to a place you have seen. | When you finish a short rest you can cast dream (you as the messenger), scrying or teleportation circle without a slot or material components. Teleportation circle opens a portal to the last location where you finished a long rest on your current plane, not to a place you have seen (it fails, but is not wasted, if you have not taken a long rest on that plane). Once used, you cannot use it again until you finish a long rest. The app has the wrong destination and no limit. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L6 Hearth of Moonlight and Shadow | A 30-foot camp during a rest that grants concealment, silence, and a bonus to Perception and Stealth for everyone inside. | During a short or long rest you touch a point and an invisible 30-foot-radius sphere appears (total cover blocks it). You and your allies inside get +5 to Dexterity (Stealth) and Wisdom (Perception) checks, and light from open flames inside is not visible outside. There is no silence effect and no concealment effect. | Verified (2 sources) |
| 3 | 🟡 Imprecise | L10 Hidden Paths | A bonus action teleports you 60 feet, or a creature you touch 30 feet. | Teleporting yourself up to 60 ft to an unoccupied space you can see is a bonus action; teleporting one willing creature you touch up to 30 ft is an action. Uses are a Wisdom modifier number (minimum once) per long rest (correct in the app). | Verified (2 sources) |
| 4 | 🟡 Imprecise | L2 Balm of the Summer Court | A pool of d6s equal to your druid level, spendable as a bonus action to heal a creature within 120 feet and give it temporary hit points; refills on a long rest. | You can spend at most half your druid level in dice in one use; the target regains the total rolled and gains 1 temporary hit point per die spent. The dice return on a long rest. The app has no cap per use and gives no temporary hit point amount. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | L14 Faithful Summons | Four dire wolves appear and defend you for an hour. | If you are reduced to 0 hit points or are incapacitated against your will, you immediately gain the effects of conjure animals as if cast with a 9th-level slot: four beasts of your choice that are CR 2 or lower, appearing within 20 ft; without commands they protect you and attack your foes. It lasts 1 hour with no concentration (or until you dismiss it). Once per long rest. Not fixed dire wolves, and incapacitation also triggers it. | Verified (2 sources) |
| 2 | 🔴 Wrong | L2 Spirit Totem: Unicorn | Unicorn grants attack advantage against creatures in it and boosts your healing. | Unicorn Spirit gives you and your allies advantage on checks to detect creatures in the aura, and when you cast a healing spell with a slot, chosen creatures in the aura regain hit points equal to your druid level. Attack advantage is Hawk Spirit (a reaction, for an attack against a target in the aura). | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L10 Spreading Spores | Hurl spores into a 10-foot cube within 30 feet, damaging creatures that start there instead of using Halo of Spores. | It is a bonus action usable only while Symbiotic Entity is active, with a 10-foot cube within 30 ft for 1 minute; creatures that enter or start there make a Constitution save or take your Halo of Spores damage. The prerequisite is missing. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L2 Halo of Spores | 1d4 necrotic, scaling with druid level. | 1d4, then 1d6 at 6th, 1d8 at 10th, 1d10 at 14th. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | Circle Spells L3 | Blur, Scorching Ray. | Flaming Sphere, Scorching Ray. Blur is not a Wildfire spell. | Verified (2 sources) |
| 2 | 🔴 Wrong | Circle Spells L5 | Revivify, Fireball. | Plant Growth, Revivify. Fireball is not a Wildfire spell. | Verified (2 sources) |
| 3 | 🟡 Imprecise | L10 Cauterizing Flames | When a creature dies near you or the spirit, a flame springs up that either heals or burns the next creature to move through it for 2d10 + your Wisdom modifier. | When a Small or larger creature dies within 30 ft of you or your wildfire spirit, a spectral flame lasts 1 minute in its space. When a creature you can see enters that space you can use your reaction to extinguish it and heal or deal fire damage to that creature for 2d10 + Wisdom modifier. You can do this a number of times equal to your proficiency bonus per long rest. The app omits the uses and the reaction. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Improved Critical, Remarkable Athlete, Additional Fighting Style, Superior Critical, Survivor. | Names and levels match (3, 7, 10, 15, 18). Descriptions are not in the subclass file. | Verified (printed text) |

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
| 1 | 🟡 Imprecise | L3 Combat Superiority / L10 / L18 | Improved Combat Superiority is folded into Combat Superiority. | Improved Combat Superiority is its own feature at 10th (d10) and 18th (d12). Dice (4/5/6) and maneuver counts (3/5/7/9) are correct in the text. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L18 Arcane Shot (damage increase) | Not listed. Arcane Shot says only that more options are learned at 7th, 10th, 15th and 18th. | At 18th level each Arcane Shot option also improves: the 2d6 options (beguiling, bursting, enfeebling, grasping, shadow) become 4d6, piercing and seeking go from 1d6 to 2d6, and banishing arrow, which has no damage before, gains 2d6 force. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L3 Arcane Shot (timing) | Two options known, used twice per short or long rest on an arrow hit. | Once per turn when you fire an arrow from a shortbow or longbow as part of the Attack action you can apply one option. You decide to use it when the arrow hits a creature, unless the option does not involve an attack roll (piercing and seeking arrows make no attack roll; a Dexterity save replaces it). You have two uses, regained on a short or long rest. Damage dice by option: beguiling, bursting, enfeebling, grasping, shadow 2d6 (4d6 at 18th); piercing, seeking 1d6 (2d6 at 18th); banishing none (2d6 force at 18th). The app omits the once-per-turn rule, the bow requirement and the no-attack-roll options. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L10 Hold the Line | A reaction makes an opportunity attack against a creature that moves 5 feet or more while in your reach, stopping it dead. | Creatures provoke an opportunity attack from you when they move 5 ft or more while within your reach, and if you hit a creature with an opportunity attack its speed becomes 0 until the end of the current turn. The app already has the movement trigger; what it leaves out is that the creature is stopped only if the attack hits, and only until the end of the turn. | Verified (2 sources) |
| 2 | 🔴 Wrong | L3 Unwavering Mark | A creature you hit in melee is marked until your next turn: it has disadvantage attacking anyone else, and on your next turn you can make a bonus attack against it that deals extra damage on a hit and gives it disadvantage on its next attack. | The mark lasts until the end of your next turn. While it is within 5 ft of you, the marked creature has disadvantage on attack rolls that do not target you. If it deals damage to anyone other than you, you can make a special melee weapon attack against it as a bonus action on your next turn with advantage on the attack roll; on a hit it takes extra damage equal to half your fighter level. There is no disadvantage on its next attack. Uses: Strength modifier (minimum once) per long rest (the app has this right). | Verified (2 sources) |
| 3 | 🟡 Imprecise | L15 Ferocious Charger | Once per turn, a hit after moving 10 feet in a straight line knocks a Large or smaller creature prone on a failed Strength save. | If you move at least 10 ft in a straight line right before attacking a creature and hit it, the target must succeed on a Strength saving throw (8 + proficiency bonus + Strength modifier) or be knocked prone. Once on each of your turns. There is no size restriction in the text. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | L18 Strength before Death | Once per long rest, a reaction on taking damage lets you act immediately before the damage is applied. | When damage reduces you to 0 hit points, you use your reaction to delay falling unconscious and immediately take an extra turn. During it, damage causes death save failures and you fall unconscious at the end of the turn if still at 0. It is not 'before the damage is applied'. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L3 Manifest Echo | The echo has 1 hit point and your AC. | The echo has AC 14 + your proficiency bonus (not your AC), 1 hit point and immunity to all conditions, and uses your saving throw bonuses. You manifest it as a bonus action within 15 ft. On your turn you mentally command it to move up to 30 ft with no action required (the app says bonus action). Swapping places with it is a bonus action and costs 15 ft of your movement. It is destroyed if it is more than 30 ft from you at the end of your turn. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L3 Psionic Power | A pool of Psionic Energy dice, twice your proficiency bonus per long rest, starting at d6 and growing with fighter level; one die returns on a short rest. | You have a number of Psionic Energy dice equal to twice your proficiency bonus (d6, d8 at 5th, d10 at 11th, d12 at 17th) and regain all of them on a long rest. In addition, as a bonus action you can regain one expended die, once until you finish a short or long rest. A die does not return automatically on a short rest. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | L7 Royal Envoy | Proficiency and expertise in Persuasion, plus one extra language. | Proficiency in Persuasion, or if you already have it, proficiency in one of Animal Handling, Insight, Intimidation or Performance; and your proficiency bonus is doubled on Persuasion checks either way. There is no extra language. | Verified (2 sources) |
| 2 | 🔴 Wrong | L15 Bulwark | Indomitable also lets one ally within 60 feet reroll a failed ability check. | It applies to Intelligence, Wisdom or Charisma SAVING THROWS: when you use Indomitable on one, choose an ally within 60 ft that also failed the same save and can see or hear you; it rerolls the save and must use the new result. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Open Hand Technique, Wholeness of Body, Tranquility, Quivering Palm. | Names and levels match (3, 6, 11, 17). | Verified (printed text) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | Elemental Attunement (options.json, line 134) | A minor elemental trick: snuff or light a flame, chill or warm an object, or shape a Medium mass of earth, fire, water or mist for a moment. | As an action, within 30 feet of you (the Player's Handbook errata replaced 'nearby'): a harmless sensory effect; light or snuff a candle, torch or small campfire; chill or warm up to 1 pound of nonliving material for up to 1 hour; or make earth, fire, water or mist that fits in a 1-foot cube take a crude shape for 1 minute. A 'Medium mass' is far larger than a 1-foot cube. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | L3 Path of the Kensei | Two kensei weapons count as monk weapons, one ranged kensei weapon adds +2 AC as a bonus action, and an unarmed strike as a bonus action follows a kensei attack. | Agile Parry: when you make an unarmed strike as part of the Attack action while wielding a kensei weapon, you gain +2 AC until the start of your next turn. It has nothing to do with ranged weapons or a bonus action. The bonus-action unarmed strike with a kensei weapon attack is also not part of the rules; only the monk's own Martial Arts bonus strike applies. | Verified (2 sources) |
| 2 | 🟠 Missing | L3 Kensei Weapons / Agile Parry | Agile Parry is not named. | The three level-3 pieces are Kensei Weapons (two: one melee, one ranged), Agile Parry and Kensei's Shot. | Verified (2 sources) |
| 3 | 🟡 Imprecise | L3 Kensei Weapons | Two kensei weapons count as monk weapons. | One melee and one ranged simple or martial weapon without heavy or special (plus longbow); you can choose another kensei weapon at 6th, 11th and 17th level. The additional picks are not mentioned. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L3 Touch of Death | When a creature within 5 feet drops to 0 hit points, gain temporary hit points equal to your Wisdom modifier plus your monk level. | When you reduce a creature within 5 feet of you to 0 hit points, you gain temporary hit points equal to your Wisdom modifier + your monk level (minimum 1). It is not triggered by any creature dropping near you, only by your own damage. | Verified (2 sources) |

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

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

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
| 1 | 🔴 Wrong | L3 Arms of the Astral Self | Spend 1 ki; arms use Wisdom, add 5 feet of reach, shove creatures within 10 feet. | On summoning, creatures you choose within 10 ft make a Dexterity save or take force damage equal to two rolls of your martial arts die. The arms then add 5 ft of reach and let you use Wisdom for attack and damage. There is no shove. | Verified (2 sources) |
| 2 | 🔴 Wrong | L11 Body of the Astral Self | Deflect Energy reaction and add 1d10 to a damage roll once per turn. | Empowered Arms adds one extra roll of your martial arts die (not a fixed 1d10) once per turn when you hit with the arms. Deflect Energy reduces acid, cold, fire, force, lightning or thunder damage by 1d10 + Wisdom modifier. | Verified (2 sources) |
| 3 | 🟡 Imprecise | L6 Visage of the Astral Self | Darkvision, advantage on Insight and Intimidation, Astral Sight. | Astral Sight is the darkvision (120 ft, magical and nonmagical darkness); also Word of the Spirit (speak privately to one creature within 60 ft or project your voice 600 ft), which the app omits. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L3 Draconic Disciple | You know Draconic, can shift your unarmed damage to acid, cold, fire, lightning or poison, and can add a d4 to a Charisma check once per short rest. | Three parts: Draconic Presence (reaction to reroll a failed Intimidation or Persuasion check, once per long rest after it turns a failure into a success), Draconic Strike (change an unarmed strike's damage type) and Tongue of Dragons (Draconic or another language). The d4 to a Charisma check does not exist. | Verified (2 sources) |
| 2 | 🔴 Wrong | L6 Wings Unfurled | While airborne from a jump or effect, gain a flying speed equal to your walking speed. | Triggered by Step of the Wind: spectral wings give a flying speed equal to your walking speed until the end of the turn, a proficiency-bonus number of times per long rest. There is no 'airborne' condition. | Verified (2 sources) |
| 3 | 🔴 Wrong | L17 Ascendant Aspect | Aspect of the Wyrm reaches 30 feet, grants blindsight, and once per turn lets you deal 3d10 damage to up to three creatures as a reaction. | Three parts: Augment Breath (1 ki makes Breath of the Dragon a 60-ft cone or 90-ft line dealing four martial arts dice), Blindsight (10 ft) and Explosive Fury (when you activate Aspect of the Wyrm, creatures in the aura make a Dexterity save or take 3d10). The aura does not grow to 30 ft, and Explosive Fury is not a reaction. | Verified (2 sources) |
| 4 | 🟡 Imprecise | L3 Breath of the Dragon (scaling) | Two rolls of your martial arts die. | Two rolls of your martial arts die on a failed save (half on a success), and three rolls from 11th level. The app has the two rolls right but leaves out the increase at 11th level. The cone is 20 ft and the line 30 ft by 5 ft (correct in the app); uses are proficiency bonus per long rest, or 2 ki when none remain (correct). | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | All levels (SRD) | Names only; oath spells shipped and correct. | Sacred Weapon and Turn the Unholy (3), Aura of Devotion (7), Purity of Spirit (15), Holy Nimbus (20). Oath spells match the SRD. | Verified (printed text) |
| 2 | 🟡 Imprecise | L20 Holy Nimbus (help text, src/lib/help/srd-features.json line 69) | Once per long rest, blaze with light for a minute: enemies near you take radiant damage and you resist their spells. | As an action you emit bright light in a 30-foot radius (and dim light 30 feet beyond) for 1 minute; an enemy that starts its turn in the bright light takes 10 radiant damage, and you have advantage on saving throws against spells cast by fiends or undead. 'Resist their spells' is not advantage on saves, and the damage amount and trigger are left out. | Verified (printed text) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L20 Avenging Angel | For an hour, gain a 60-foot flying speed and a 30-foot aura that frightens enemies who fail a Wisdom save, giving you advantage against them. | Wings give a 60-ft flying speed and a 30-ft aura of menace. The first time an enemy enters the aura or starts its turn there during a battle it must succeed on a Wisdom save or be frightened for 1 minute or until it takes damage. Attack rolls against the frightened creature have advantage (for every attacker, not only you). Once per long rest. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L20 Emissary of Redemption | Resistance to all damage from other creatures, and attackers take radiant damage equal to half of what they dealt you, until you attack or harm someone. | You have resistance to all damage dealt by other creatures, and a creature that hits you takes radiant damage equal to half the damage you take from the attack. If you attack a creature, cast a spell on it or damage it by any means except this feature, neither benefit works against that creature until you finish a long rest. The loss is per creature and lasts until a long rest; the app ends it for everyone when you act against anyone. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L20 Living Legend | A missed attack can be rerolled once per turn, and failed saves can be turned into successes once per turn. | Once on each of your turns a missed weapon attack hits instead (it is not rerolled), and when you fail a saving throw you can use your reaction to reroll it and must use the new roll (it is not turned into a success). Advantage on Charisma checks is right. It recharges on a long rest or by expending a 5th-level spell slot. The app has the two effects the wrong way round. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L15 Vigilant Rebuke | A reaction deals 2d8 + Charisma force damage to a creature whose spell or effect you just saved against. | Whenever you or a creature you can see within 30 ft succeeds on an Intelligence, Wisdom or Charisma saving throw, you can use your reaction to deal 2d8 + Charisma modifier force damage to the creature that forced the save. The app limits it to your own saves and omits the ability-score restriction. | Verified (2 sources) |

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

No discrepancy found. Levels, features and numbers match the published rules as far as I can tell.

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L20 Dread Lord | A 30-foot aura of magical darkness that deals 4d10 psychic damage to enemies who start their turn in it, gives attackers disadvantage against you, and lets you make a spectral 4d10 psychic attack as a bonus action. | The aura is gloom that turns bright light to dim, and the 4d10 psychic damage applies only to ENEMIES FRIGHTENED OF YOU when they start their turn there. Your bonus-action attack is a melee spell attack dealing 3d10 + Charisma modifier NECROTIC damage. Sight-reliant attackers have disadvantage against you. | Verified (2 sources) |
| 2 | 🔴 Wrong | L3 Control Undead | One undead of CR at or below your level within 30 feet obeys you for 24 hours on a failed Charisma save. | As an action, one undead you can see within 30 ft must make a Wisdom saving throw (not Charisma). On a failure it obeys your commands for 24 hours or until you use this option again. An undead whose challenge rating is equal to or greater than your paladin level is immune, so the CR must be strictly below your level (the app says at or below). | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Hunter's Prey, Defensive Tactics, Multiattack, Superior Hunter's Defense. | Names and levels match (3, 7, 11, 15). The options inside Hunter's Prey, Defensive Tactics and Multiattack are tracked as picks elsewhere. | Verified (printed text) |
| 2 | 🔴 Wrong | L15 Superior Hunter's Defense (help text, src/lib/help/srd-features.json line 133) | A defensive option from your ranger archetype: dodge one attack as a reaction, resist one damage type, or retaliate when an ally is hit. | The three options are Evasion (no damage on a successful Dexterity save for half damage, half on a failure), Stand Against the Tide (when a hostile creature misses you with a melee attack, your reaction makes it repeat the attack against another creature of your choice) and Uncanny Dodge (your reaction halves the damage of an attack that hits you). There is no 'resist one damage type' option and nothing triggers when an ally is hit. options.json has the correct text for the same three picks. | Verified (printed text) |
| 3 | 🟡 Imprecise | L7 Defensive Tactics (help text, src/lib/help/srd-features.json line 28) | A defensive option from your ranger archetype: shrug off multiattacks, escape opportunity attacks, or resist being frightened by beasts. | Steel Will gives advantage on saving throws against being frightened, from any source, not only by beasts. Escape the Horde and Multiattack Defense are described correctly. | Verified (printed text) |

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
| 1 | 🟠 Missing | Tasha's optional companion | Only the original Ranger's Companion. | Tasha's replaces it with Primal Companion (Beast of the Land, Sea or Sky). That optional version is not offered. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L7 Exceptional Training | A bonus action lets the companion Dash, Disengage, Dodge or Help. | On turns when the beast does not attack, you can use a bonus action to command it to Dash, Disengage or Help (not Dodge). Its attacks count as magical. The official Player's Handbook errata (v3.0) deleted 'Dodge,' from this feature and added the sentence about magical attacks, so the app text follows the pre-errata printing. | Verified (printed text) |
| 3 | 🟡 Imprecise | L3 Ranger's Companion | A beast of CR 1/4 or lower obeys your commands, adds your proficiency bonus to its rolls and AC, uses your hit points formula and takes the Dodge action unless you command otherwise. | Choose a beast no larger than Medium with CR 1/4 or lower. Add your proficiency bonus to its AC, attack rolls, damage rolls and the saves and skills it is proficient in. Its hit point maximum equals its normal maximum or four times your ranger level, whichever is higher. It takes your initiative; you command its movement with no action and use your action to command Attack, Dash, Disengage or Help; without a command it takes Dodge. 'Uses your hit points formula' does not describe any rule. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L15 Shadowy Dodge | A reaction imposes disadvantage on an attack against you that would otherwise hit. | Whenever a creature makes an attack roll against you and does not have advantage on the roll, you can use your reaction to impose disadvantage on it. You must use the feature before you know the outcome of the roll, so it cannot be limited to attacks that would hit. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | Monster Slayer Magic L13 | Dimension Door. | Banishment. The list is Protection from Evil and Good (3), Zone of Truth (5), Magic Circle (9), Banishment (13), Hold Monster (17). | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L11 Fey Reinforcements | Summon fey is always prepared and can be cast once per long rest without a slot, with the summon able to turn invisible. | You know summon fey; it does not count against your spells known and you can cast it without a material component. You can cast it once without a spell slot, regaining that on a long rest. Whenever you start casting it you can modify it so it does not require concentration; its duration then becomes 1 minute. Nothing makes the summoned fey invisible. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L11 Mighty Swarm | Knocks moved targets prone, gives you half cover when it moves you, and adds 1d6 to its damage. | The swarm's damage die simply becomes 1d8 (already stated for Gathered Swarm); 'adds 1d6' double counts it. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L7 Bond of Fang and Scale | While the drake is out, you gain resistance to its damage type, the drake gains a fly speed, and your attacks count as magical with extra elemental damage. | The drake grows wings (fly speed = walking speed), becomes Medium and can be ridden, its Bite deals an extra 1d6 of its essence type, and you gain resistance to that type. Your own attacks do not become magical or deal extra damage. | Verified (2 sources) |
| 2 | 🔴 Wrong | L15 Perfected Bond | The drake becomes Large and rideable, gains extra hit points, and adds your proficiency bonus in damage to its attacks. | The drake becomes Large, its Bite's extra damage rises to 2d6 total, and Reflexive Resistance lets you give you or the drake resistance to damage as a reaction (proficiency-bonus uses per long rest). No extra hit points or proficiency damage. | Verified (2 sources) |
| 3 | 🟡 Imprecise | L11 Drake's Breath | Once per long rest, the drake exhales a 30-foot cone dealing 8d6, halved on a Dexterity save. | You or your drake can exhale it, you choose the element, damage is 10d6 from 15th level, and it can be used again by expending a 3rd-level or higher spell slot. | Verified (2 sources) |
| 4 | 🔴 Wrong | L3 Drake Companion | Summon a Small drake once per long rest or with a spell slot; it acts on your turn, adds your proficiency bonus to its rolls, and has an elemental breath. | The Drake Companion stat block has a Bite (1d6 + proficiency bonus piercing), a Draconic Essence trait (you choose acid, cold, fire, lightning or poison when you summon it; it is immune to that type) and an Infused Strikes reaction (extra 1d6 of that type when another creature within 30 ft hits). Hit points are 5 + 5 x ranger level. A breath weapon comes only from Drake's Breath at 11th level, which you or the drake can use. Resummoning after the first time each long rest costs a spell slot of 1st level or higher. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Fast Hands, Second-Story Work, Supreme Sneak, Use Magic Device, Thief's Reflexes. | Names and levels match (3, 3, 9, 13, 17). | Verified (printed text) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L17 Spell Thief | A reaction steals a spell cast at you on a failed save by the caster, denying them the spell for 8 hours and letting you cast it once; recharges on a long rest. | Immediately after a creature casts a spell that targets you or includes you in its area, you use your reaction to force it to make a saving throw with its spellcasting ability modifier against your spell save DC. On a failure you negate the spell's effect against you and, if the spell is 1st level or higher and of a level you can cast, steal it: for the next 8 hours you know it and can cast it using your spell slots, and the creature cannot cast it. You can use Spell Thief once per long rest. The app says you can cast the spell only once. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L3 Ear for Deceit | Treat any Insight roll of 7 or lower as an 8. | Whenever you make a Wisdom (Insight) check to determine whether a creature is lying, treat a roll of 7 or lower on the d20 as an 8. It does not apply to other Insight checks. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | L9 Tokens of the Departed | Advantage on death and Constitution saves, resistance to poison. | While you carry a soul trinket you have advantage on Constitution saving throws and death saving throws. There is no poison resistance. | Verified (2 sources) |
| 2 | 🔴 Wrong | L17 Death's Friend | Wails from the Grave triggers alongside every sneak attack at full strength, and a soul trinket forms at the end of each long rest. | Wails from the Grave now also deals its necrotic damage to the first creature (not just the second). It is still your choice to use it, it still spends one of your proficiency-bonus uses (or a soul trinket), and it does not trigger on every sneak attack. A soul trinket appears at the end of a long rest only if you have none. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L3 Psychic Blades | Dice grow with rogue level. | The blades stay 1d6 and 1d4; it is the Psionic Energy dice that grow (d6, d8 at 5, d10 at 11, d12 at 17). | Verified (2 sources) |
| 2 | 🟡 Imprecise | L3 Psionic Power (Soulknife) | A pool of Psionic Energy dice, twice your proficiency bonus per long rest, starting at d6 and growing with rogue level; one returns on a short rest. | Twice your proficiency bonus in Psionic Energy dice (d6; d8 at 5th, d10 at 11th, d12 at 17th), all regained on a long rest. As a bonus action you can also regain one expended die, once until you finish a short or long rest. A die does not return automatically on a short rest. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Dragon Ancestor, Draconic Resilience, Elemental Affinity, Dragon Wings, Draconic Presence. | Names and levels match (1, 1, 6, 14, 18). | Verified (printed text) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟠 Missing | L1 Divine Magic (affinity spells) | Add the cleric spell list to your sorcerer spell choices, plus one affinity spell matching your divine bent. | You choose an affinity (good, evil, law, chaos or neutrality) and learn one extra spell for it, a sorcerer spell that does not count against your spells known: cure wounds (good), inflict wounds (evil), bless (law), bane (chaos) or protection from evil and good (neutrality). If you replace it, it must be replaced by a cleric spell. The affinity choice and its spell are not in the subclass data. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L1 Eyes of the Dark | Darkvision to 120 feet, and darkness can be cast for 2 sorcery points and seen through. | 120 ft darkvision is level 1, but you learn darkness at 3rd level (not 1st). The sorcery-point casting and seeing through it are correct. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L1 Strength of the Grave | A Charisma save against 5 + the damage leaves you at 1 hit point instead of 0; once per long rest. | The save is a Charisma save against DC 5 + the damage taken, and the feature does not work if you are reduced to 0 hit points by radiant damage or by a critical hit. The long-rest lockout applies only after the save succeeds. The app lists neither exclusion. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | Storm Guide level | Storm Guide at 14th level. | Storm Guide is a 6th-level feature (with Heart of the Storm). 14th has Storm's Fury only. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L14 Revelation in Flesh | Spend 1 or more sorcery points for a minute of aberrant change: see invisible things, swim and breathe water, fly, or squeeze through tiny gaps. | As a bonus action you spend 1 or more sorcery points to transform for 10 minutes (not 1 minute). For each point spent you pick one benefit: see invisible creatures within 60 ft; fly at your walking speed and hover; swim at twice your walking speed and breathe underwater; or squeeze through spaces as narrow as 1 inch and escape nonmagical restraints or grapples for 5 ft of movement. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L18 Warping Implosion | Once per long rest, teleport 120 feet and pull creatures toward your old space for 3d10 force damage. | As an action you teleport up to 120 feet; each creature within 30 feet of the space you left makes a Strength saving throw against your spell save DC (the Tasha's errata added 'against your spell save DC'): 3d10 force damage and pulled toward that space on a failure, half damage and not pulled on a success. You can use it again before a long rest by spending 5 sorcery points. The app omits the save, the radius and the sorcery-point option. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | Whole subclass | Lunar Embodiment shifts spells to radiant or cold; Moon Fire changes your phase; Waxing and Waning at 14th; Lunar Phenomenon at 18th. | Real features: Lunar Embodiment (1st: you learn fifteen spells as sorcerer spells that do not count against spells known, one per phase at sorcerer levels 1, 3, 5, 7 and 9 - Full Moon shield, lesser restoration, dispel magic, death ward, Rary's telepathic bond; New Moon ray of sickness, blindness/deafness, vampiric touch, confusion, hold monster; Crescent Moon color spray, alter self, phantom steed, hallucinatory terrain, mislead - and after each long rest you choose a moon phase, in which you can cast one 1st-level spell of that phase once without a slot), Moon Fire (1st: sacred flame, targeting two creatures within 5 ft), Lunar Boons (6th: metamagic cost reduced by 1 for spells of your phase's schools - Full Moon abjuration and divination, New Moon enchantment and necromancy, Crescent Moon illusion and transmutation, as corrected by the Dragonlance errata), Waxing and Waning (6th, not 14th: switch phase for 1 sorcery point and cast one 1st-level spell of each phase), Lunar Empowerment (14th, missing: phase-specific boons) and Lunar Phenomenon (18th: 3d8 heal and blind / 3d10 necrotic / 60-ft teleport). The shipped text and the 14th-level feature are wrong; the spell lists are absent. | Verified (2 sources) |

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
| 1 | 🟠 Missing | Expanded spell list | No spells in the data at all. | Fiend expanded spells: burning hands, command (L1); blindness/deafness, scorching ray (L3); fireball, stinking cloud (L5); fire shield, wall of fire (L7); flame strike, hallow (L9). RAW they are added to the pool of spells the warlock may learn, not granted for free. | Verified (printed text) |
| 2 | 🟡 Imprecise | All levels (SRD) | Names only: Dark One's Blessing, Dark One's Own Luck, Fiendish Resilience, Hurl Through Hell. | Names and levels match (1, 6, 10, 14). The engine implements Dark One's Blessing (Charisma modifier + warlock level temp HP on a kill), Dark One's Own Luck, Fiendish Resilience and Hurl Through Hell. | Verified (printed text) |
| 3 | 🟡 Imprecise | L10 Fiendish Resilience | Resistance to the chosen damage type. | The chosen resistance does not apply to damage from magical weapons or silvered weapons. The resistance code (trait-rules.ts, chosenResistances) reads the chosen type from the feature name and I found no weapon exception. | Verified (printed text) |

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
| 1 | 🔴 Wrong | L10 Beguiling Defenses | A reaction turns a charm attempt back on the caster for 1 minute and 3d10 psychic damage per turn. | The reaction forces a Wisdom save; on a failure the creature is charmed by you for 1 minute or until it takes damage. There is no 3d10 psychic damage. | Verified (2 sources) |
| 2 | 🔴 Wrong | L14 Dark Delirium | Wrap a creature in illusion for a minute; removable to the Feywild. | Action; a creature within 60 ft makes a Wisdom save or is charmed or frightened by you (your choice) for 1 minute or until your concentration is broken or it takes damage. It thinks it is lost in an illusory misty realm and can see and hear only itself, you and the illusion; it is not removed anywhere, so 'removable to the Feywild' is invented. Once per short or long rest. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L1 Awakened Mind | Speak telepathically to any creature within 30 feet that shares a language with you. | The creature does not need to share your language, but it must understand at least one language. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L10 Celestial Resilience | Temporary hit points after every rest, for you and up to five allies, equal to your warlock level plus your Charisma modifier. | You gain temporary hit points equal to your warlock level + Charisma modifier; up to five allies gain half your warlock level + Charisma modifier. The allies' amount is half. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L1 Healing Light | A pool of d6s equal to 1 + your warlock level. | Matches; additionally you can spend up to your Charisma modifier (minimum 1) dice per use. | Verified (2 sources) |
| 3 | 🟡 Imprecise | L6 Radiant Soul | Resistance to radiant damage, and once per turn add your Charisma modifier to a radiant or fire spell's damage. | Resistance to radiant damage. When you cast a spell that deals radiant or fire damage, you can add your Charisma modifier to one radiant or fire damage roll of that spell against one of its targets. The limit is one roll per spell, not once per turn. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L1 Hex Warrior | One weapon each day uses Charisma for attack and damage. | At the end of a long rest, touch one weapon you are proficient with that lacks the two-handed property: you use Charisma for attack and damage with it until you use the feature again. If you have Pact of the Blade you can use Charisma with any pact weapon. 'Each day' is acceptable shorthand; the two-handed restriction and the Pact of the Blade interaction are missing. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L1 Hexblade's Curse | Bonus damage, crit on 19 or 20, hit points back when it dies. | Bonus damage equals proficiency bonus; crits on 19 or 20; when the cursed creature dies you regain hit points equal to warlock level + Charisma modifier (minimum 1). Range 30 ft, 1 minute. The server only has guidance text for this feature; the bonus damage and crit range are not applied automatically (see engine appendix). | Verified (2 sources) |
| 3 | 🟡 Imprecise | L10 Armor of Hexes | Misses on a coin-flip d6 roll even when it would hit. | When the cursed target hits you with an attack roll, you can use your REACTION to roll a d6: on a 4 or higher the attack misses regardless of its roll. The reaction and the 4+ threshold are missing. | Verified (2 sources) |
| 4 | 🟡 Imprecise | L14 Master of Hexes | Move the curse to a new creature without spending a rest. | When the cursed creature dies, apply the curse to another creature you can see within 30 ft of the dead one while you're not incapacitated. You do not regain hit points from the first creature's death when you do so. | Verified (2 sources) |
| 5 | 🟡 Imprecise | L6 Accursed Specter | A humanoid you kill rises as a specter under your command until your next long rest. | The specter has temporary hit points equal to half your warlock level and a bonus to its attack rolls equal to your Charisma modifier (minimum +0). It obeys your verbal commands, leaves at the end of your next long rest, and the feature recharges on a long rest. The app omits the temporary hit points and the attack bonus. | Verified (2 sources) |
| 6 | 🟡 Imprecise | Expanded spells | Granted automatically. | Hexblade has an expanded spell list; the spells are added to the pool the warlock picks from, not granted free. The list itself is correct: shield, wrathful smite; blur, branding smite; blink, elemental weapon; phantasmal killer, staggering smite; banishing smite, cone of cold. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | L6 Oceanic Soul | Immunity to cold damage. | Resistance to cold damage, not immunity. In addition, when you are fully submerged, any creature that is also fully submerged can understand your speech and you can understand theirs (it is not telepathy and has no 120-ft range). The app says you can speak with any creature underwater, which overstates it. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L10 Grasping Tentacles | You always have evard's black tentacles prepared, gain temporary hit points when you cast it, and keep concentration through damage more easily. | You learn evard's black tentacles (counts as a warlock spell, not against spells known) and can cast it once per long rest without a slot. Casting it gives temporary hit points equal to your warlock level, and damage cannot break your concentration on it (not just 'more easily'). | Verified (2 sources) |
| 3 | 🔴 Wrong | Expanded spells L9 | Cone of Cold, Commune with Nature. | Bigby's Hand and Cone of Cold. Commune with Nature is not on the Fathomless list. | Verified (2 sources) |
| 4 | 🔴 Wrong | L1 Tentacle of the Deep | 1d8 cold damage and halves a target's speed. | A hit reduces the target's speed by 10 feet until the start of your next turn, not by half. The damage is 1d8 cold, 2d8 from 10th level. Uses are proficiency bonus per long rest (correct). The feature is named Tentacle of the Deeps in Tasha's (the D&D Beyond copy of the book reads 'Your Tentacle of the Deeps can defend you'); the app and the resource id drop the final s. | Verified (2 sources) |
| 5 | 🟡 Imprecise | Expanded spells L7 | Control Water, Summon Elemental. | Matches, but Summon Elemental is limited to water elementals. Also, Oceanic Soul lets submerged creatures understand each other's speech rather than speak to any creature. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | Expanded spells (all tiers) | A single fixed list: Detect Evil and Good, Protection from Evil and Good; Phantasmal Force, Gust of Wind; Create Food and Water, Phantom Steed; Arcane Eye, Fire Shield; Creation, Seeming. | Each tier has one spell every genie gets (detect evil and good, phantasmal force, create food and water, phantasmal killer, creation) and one by kind. Dao: sanctuary, spike growth, meld into stone, stone shape, wall of stone. Djinni: thunderwave, gust of wind, wind wall, greater invisibility, seeming. Efreeti: burning hands, scorching ray, fireball, fire shield, flame strike. Marid: fog cloud, blur, sleet storm, control water, cone of cold. The shipped list takes some of these but also protection from evil and good, phantom steed and arcane eye, which are not on any Genie list, and has no kind choice. | Verified (2 sources) |
| 2 | 🟠 Missing | L1 Genie's Vessel (kind choice) | No kind choice (Dao, Djinni, Efreeti, Marid). | The patron's kind drives the damage type (bludgeoning, thunder, fire, cold), the expanded spells, the resistance at 6th and the flying/other riders. The app has a generic 'your patron's damage type'. | Verified (2 sources) |
| 3 | 🔴 Wrong | L1 Genie's Vessel: Bottled Respite | Retreat inside it for 10 minutes. | You stay for up to twice your proficiency bonus in HOURS, can leave early with a bonus action, and it returns after a long rest. The vessel is an object with AC equal to your spell save DC and hit points equal to warlock level + proficiency bonus. | Verified (2 sources) |
| 4 | 🔴 Wrong | L10 Sanctuary Vessel | Bottled Respite takes up to five companions with you, and everyone who finishes a short rest inside gains temporary hit points. | Up to five willing creatures within 30 ft can enter; anyone who stays 10 minutes gets the benefits of a short rest, and Hit Dice spent inside add your proficiency bonus to the hit points regained. There are no temporary hit points. | Verified (2 sources) |
| 5 | 🟡 Imprecise | L14 Limited Wish | Replicate any spell of 6th level or lower without components. | As an action, the effect of one spell of 6th level or lower with a casting time of 1 action, from any class list, ignoring its requirements and costly components; it returns after 1d4 long rests. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | Expanded spells L1 | False Life, Ray of Sickness. | Bane, False Life. Ray of Sickness is on the Undying list, not the Undead. | Verified (2 sources) |
| 2 | 🔴 Wrong | L10 Necrotic Husk | Immunity to necrotic damage; blast nearby creatures for necrotic damage equal to your warlock level. | Resistance to necrotic damage, and immunity to it while transformed by Form of Dread. As a reaction when reduced to 0 hit points you drop to 1 instead, and creatures of your choice within 30 ft take 2d10 + your warlock level necrotic damage; you then gain one level of exhaustion. It returns only after 1d4 long rests (not once per long rest). | Verified (2 sources) |
| 3 | 🟡 Imprecise | L1 Form of Dread | Temp HP 1d10 + warlock level, immunity to fear, frighten one creature you hit each turn. | Matches; the frighten rider is a Wisdom save. | Verified (2 sources) |
| 4 | 🔴 Wrong | L14 Spirit Projection | Resistance to most damage; necrotic and psychic spell damage heals you; once per long rest or with a 5th-level slot. | Resistance to bludgeoning, piercing and slashing damage; flying speed and hover; moving through creatures costs force damage if you end inside one; while in Form of Dread you heal half of any necrotic damage you deal once per turn. Conjuration and necromancy spells need no verbal or somatic components. The app's psychic healing, 'most damage' and slot option do not appear in the wiki text. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | L1 Among the Dead | Undead have disadvantage attacking you unless you attack them. | If an undead targets you directly with an attack or a harmful spell it makes a Wisdom save; on a failure it must choose a new target or lose the attack or spell. It does not protect against area effects. The creature is immune for 24 hours after succeeding or if you target it. | Verified (2 sources) |
| 2 | 🔴 Wrong | L10 Undying Nature | You no longer age. | You age one year for every ten that pass and cannot be aged magically. You also hold your breath indefinitely and need no food, water or sleep (you still need rest to reduce exhaustion). 'No longer age' contradicts the one-in-ten rate. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | All levels (SRD) | Names only: Evocation Savant, Sculpt Spells, Potent Cantrip, Empowered Evocation, Overchannel. | Names and levels match (2, 2, 6, 10, 14). | Verified (printed text) |
| 2 | 🟡 Imprecise | L10 Empowered Evocation (help text, src/lib/help/srd-features.json line 52) | Add your Intelligence modifier to the damage of your evocation spells. | You add your Intelligence modifier to one damage roll of any wizard evocation spell you cast (SRD 5.1 and Player's Handbook errata: 'one damage roll'), not to every roll. | Verified (printed text) |

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
| 1 | 🟡 Imprecise | L6 Expert Divination | Returns one expended slot of lower level. | You regain an expended spell slot of a level lower than the divination spell you cast, and the slot can be no higher than 5th level. | Verified (2 sources) |
| 2 | 🟡 Imprecise | L10 The Third Eye | After a rest, gain darkvision, ethereal sight, greater comprehension or see invisibility until your next rest. | It is an action, usable once per short or long rest: darkvision 60 ft, ethereal sight 60 ft, read any language, or see invisible creatures within 10 ft. It lasts until you are incapacitated or rest. | Verified (2 sources) |

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
| 1 | 🔴 Wrong | L2 Hypnotic Gaze | Recharges on a short or long rest. | Once you use it on a creature you cannot use it on that same creature again until you finish a long rest. It does not recharge on a short rest and isn't a pool for you. | Verified (2 sources) |
| 2 | 🔴 Wrong | L6 Instinctive Charm | Recharges on a short or long rest. | The limit is per attacker: on a successful save the attacker cannot be affected again until you finish a long rest. There is no short rest recharge. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L6 name and wording | Malleable Illusion: 'An action reshapes an illusion you are concentrating on while it lasts.' | The feature is named Malleable Illusions: when you cast an illusion spell with a duration of 1 minute or longer you can use your action to change its nature (within the spell's normal parameters) while you can see it. Concentration is not required, which the app text implies. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L14 Command Undead | Intelligent undead get a fresh save each time they take damage. | An undead that fails becomes friendly and obeys you until you use the feature again. If its Intelligence is 8 or higher it has advantage on the save; if it fails and has Intelligence 12 or higher it repeats the save at the end of every hour. Damage is not a trigger. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L10 Shapechanger | You always have polymorph prepared and can cast it on yourself as a beast of CR 1 or lower without a slot, once per short or long rest. | You add polymorph to your spellbook if it is not there already (a wizard still prepares it from the book like any spell). You can cast it without a spell slot, targeting only yourself and becoming a beast of CR 1 or lower; after doing so you cannot do it again until a short or long rest, though you can still cast it normally with a slot. It is not 'always prepared'. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🟡 Imprecise | L6 Power Surge | Store surges from dispelled or countered spells, up to your Intelligence modifier, and spend one to add half your wizard level to a damaging spell's damage. | You can store up to your Intelligence modifier (minimum one) surges. After a long rest you reset to one surge, you gain one when you successfully end a spell with dispel magic or counterspell, and you gain one if you end a short rest with none. Once per turn when you damage a creature or object with a wizard spell you can spend one for extra force damage equal to half your wizard level. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L6 Momentary Stasis | A proficiency-bonus number of times per long rest. | Uses equal your Intelligence modifier (minimum 1) per long rest. The target is incapacitated with speed 0 until the end of your next turn or until it takes damage. | Verified (2 sources) |

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

| # | | Where | What the app has | What the rules say | Confidence |
|---|---|---|---|---|---|
| 1 | 🔴 Wrong | L10 Violent Attraction | A proficiency-bonus number of times per long rest. | Uses equal your Intelligence modifier (minimum 1). It adds 1d10 to a weapon attack's damage from a creature you see within 60 ft, or 2d10 to fall damage. | Verified (2 sources) |
| 2 | 🔴 Wrong | L14 Event Horizon | Once per long rest or with a 4th-level slot. | Once per long rest or by expending a spell slot of 3rd level or higher. Creatures that start their turn within 30 ft make a Strength save: failed they take 2d10 force and speed 0, success half damage and each foot costs 2 extra. | Verified (2 sources) |

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
| 1 | 🟡 Imprecise | L6 Manifest Mind | A bonus action projects your book's spirit up to 300 feet, letting you cast spells from its position and shed light; free once per long rest, otherwise a slot. | As a bonus action you manifest the mind as a Tiny spectral object in an unoccupied space within 60 ft (it sheds dim light in 10 ft, has darkvision 60 ft and can hover 30 ft per bonus action). Whenever you cast a wizard spell on your turn you can cast it from the mind's space, a number of times per day equal to your proficiency bonus (regained on a long rest). The mind ends if it is more than 300 ft from you. Conjuring it is free once per long rest, or by spending a spell slot of any level (the app has this right). The app's 'up to 300 feet' and the missing proficiency-bonus limit are the errors. | Verified (2 sources) |
| 2 | 🔴 Wrong | L10 Master Scrivener | Create a spell scroll of a 1st or 2nd-level spell, cast at maximum effect. | After a long rest, copy a 1st- or 2nd-level spell with a casting time of 1 action from your book onto a scroll; the spell counts as one level higher when cast; the scroll vanishes after being cast or at your next long rest. No maximum-effect rule. | Verified (2 sources) |
| 3 | 🟡 Imprecise | L2 Awakened Spellbook | Change a spell's damage type, cast one ritual per long rest without preparing it. | Ritual: cast a ritual spell from the book at its normal casting time instead of adding 10 minutes, once per long rest. Damage type: when casting a wizard spell with a slot, change its damage type to one found in another spell in your book of the same level as the slot, for that casting. Replacing lost pages is also part of it. | Verified (2 sources) |
| 4 | 🟡 Imprecise | L14 One with the Word | Once per long rest, avoid death by destroying your book's magic, then rebuild it over days of study. | While the book is on your person you have advantage on Intelligence (Arcana) checks. If you take damage while the spellbook's mind is manifested, you can use your reaction to dismiss it and prevent all that damage (not only lethal damage); roll 3d6 and spells of your choice with combined level at least that total vanish from the book (if there are not enough spells you drop to 0 hit points), and you cannot cast them until you finish 1d6 long rests. Once per long rest. The app omits the Arcana advantage, the requirement that the mind is manifested, and the 3d6 / 1d6-long-rest cost. | Verified (2 sources) |

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

Every cell of this table matches the Player's Handbook warlock table and `class-features.json`. Patron features arrive at 1, 6, 10 and 14 for every warlock patron. Note the level 6, 10 and 14 patron features have no row in `class-features.json`'s base warlock table; they come from the subclass file.

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

151 of 590 features have no engine reference by name. The list is reproducible from the search described above. It remains a heuristic: a missing name does not prove the feature is unimplemented.

## Appendix C: Official subclasses not in the app

Notes on sources: Way of the Cobalt Soul is Critical Role's *Tal'Dorei Campaign Setting Reborn* (partnered content, not a Wizards of the Coast book). Giant Soul is a 2018 Unearthed Arcana playtest, not part of *Bigby Presents: Glory of the Giants* (that book's only subclass is the Path of the Giant barbarian, which the app has). The Ambition, Solidarity, Strength and Zeal domains are in *Plane Shift: Amonkhet*. This is still not an exhaustive survey.

| Class | Subclass | Source |
|---|---|---|
| Barbarian | Path of the Battlerager | Sword Coast Adventurer's Guide (dwarves only) |
| Bard | College of Dance | Player's Handbook (2024), a 2024-rules subclass |
| Cleric | Ambition, Solidarity, Strength, Zeal domains (and a Knowledge variant) | Plane Shift: Amonkhet (free Wizards of the Coast PDF) |
| Druid | Circle of the Land terrain variants (Arctic, Coast, Desert, Forest, Grassland, Mountain, Swamp, Underdark) | Player's Handbook; no terrain data found |
| Sorcerer | Pyromancer | Plane Shift: Kaladesh (free Wizards of the Coast PDF) |

Not listed because they use the 2024 rules: the 2025 and 2026 subclasses (for example Artificer Cartographer in *Eberron: Forge of the Artificer*, Reanimator and Shadow Sorcery in *Ravenloft: The Horrors Within*, Spellfire Sorcery in *Forgotten Realms: Heroes of Faerun*) and the 2024 Player's Handbook reprints. Not listed because they are not Wizards of the Coast publications: Way of the Cobalt Soul (Critical Role).
