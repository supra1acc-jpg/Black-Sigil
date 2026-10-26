# Black Sigil — Combat & Gameplay Systems

Last updated: October 26, 2026  
Status: Living design document  
Scope note: Some systems are **vertical-slice active**. Others are **approved direction** for later versions.

---

## 1. Design Intent

Black Sigil combat and progression should feel:

- Grounded and readable
- Skill-based rather than stat-bloated
- Risky outside the stronghold
- Rewarding to master
- Compatible with solo play and small-team co-op

Dark Souls-like combat is the primary inspiration for timing, spacing, and tension.  
This is not a pure action-RPG power fantasy and not a generic Roblox tycoon combat layer.

---

## 2. Combat

### 2.1 Core Principles
- Attacks should have clear startup, active, and recovery timing
- Enemy attacks must be readable before they become lethal
- Player success should come from positioning, stamina management, and timing
- Server-authoritative hit validation is required
- Animations and feedback should communicate weight

### 2.2 Player Combat Actions
Initial supported actions:
- Light attack
- Heavy attack later
- Dodge / roll
- Block later
- Item use
- Equip / sheathe weapon

Vertical-slice minimum:
- One weapon attack
- Short server-side cooldown
- Reliable hit detection against the Ashen Ghoul

### 2.3 Hit Rules
- Damage is applied only from server-validated hits
- Spam-clicking must not create extra attacks
- Invulnerability frames during dodge should be intentional and limited
- Safe-zone protection overrides enemy attack completion if the target is inside the safe zone at the moment of damage application

### 2.4 Feedback Requirements
Every successful combat interaction should communicate result through at least one of:
- Hit reaction
- Sound
- Impact effect
- UI/health change

---

## 3. Stamina

### 3.1 Purpose
Stamina is the main limiter that keeps combat deliberate.

It should restrain:
- Attacking
- Dodging
- Sprinting
- Blocking later

### 3.2 Working Model
- Actions consume stamina
- Stamina regenerates after a short delay when not spending it
- Exhaustion should matter, but not feel crippling for basic movement in the early game

### 3.3 Initial Action Costs
Prototype targets:
- Light attack: low cost
- Dodge: medium cost
- Sprint: drain over time
- Item use: little or no stamina cost

Exact numbers are balance values and should live in config, not scattered through scripts.

### 3.4 Design Notes
- Dodging must cost enough that it cannot be spammed without consequence
- Players should be able to recover if they play carefully
- Harder difficulties may reduce regeneration or increase costs later

---

## 4. Enemy AI

### 4.1 Goals
Enemies should feel dangerous because they are readable and punishing, not because they are spongy or unfair.

### 4.2 Ashen Ghoul — Vertical Slice Enemy
State flow:
1. Idle
2. Chase
3. Attack wind-up
4. Recover

Behavior rules:
- Aggro on proximity
- Move toward target while chasing
- Broadcast attack intention with a clear wind-up
- Return to idle/recover after attacking
- Leash back toward spawn if pulled too far
- Never target or damage players inside the SafeZone
- If the target enters SafeZone during wind-up, cancel the attack with no damage

### 4.3 Combat Feel Targets
- Dies in about 4–6 hits from the starter sword
- Drops 1–2 Scrap Iron
- Should teach spacing and patience

### 4.4 Future Enemy Direction
Later enemies may include:
- Faster aggressive types
- Heavier commitment attackers
- Ranged threats
- Elite variants
- Boss encounters

All future enemies should preserve readable telegraphs.

---

## 5. Death Penalties

Death should create tension without making the early game miserable.

### 5.1 Shared Rules
- Shared stronghold vault contents remain safe
- Personal carried loot is the main risk surface
- Respawn point defaults to the stronghold campfire / safe respawn location

### 5.2 Difficulty-Based Penalties

#### Easy
- Lose a small portion of personal currency
- Carried loot is not dropped
- Respawn at stronghold

#### Medium
- Lose a small portion of personal currency
- Carried loot drops at death location
- Loot remains recoverable for a limited time
- Respawn at stronghold

#### Hard
- Lose a larger portion of personal currency
- Carried loot drops at death location
- Failing to recover before another death can make the drop permanently lost
- Respawn at stronghold

#### Hardcore
- Character death is permanent
- Handling of equipment and shared-contribution remains unresolved and must be decided before implementation

### 5.3 Vertical-Slice Death Rules
For the current slice:
- Medium-style behavior is the default test case
- Death drops carried Scrap Iron
- Drop lasts 10 minutes
- Player respawns at campfire

---

## 6. Difficulty Selection

### 6.1 World-Level Setting
Difficulty is chosen when a world is created and applies to that world only.

Supported settings:
- Easy
- Medium
- Hard
- Hardcore

### 6.2 What Difficulty Affects
Difficulty may modify:
- Enemy health
- Enemy damage
- Aggression
- Attack speed
- Enemy variety
- Boss complexity
- Death penalties
- Reward rates later

### 6.3 Provisional Multipliers
These are starting balance ideas only:

| Setting  | Enemy Health | Enemy Damage | Aggression | Attack Speed |
|----------|--------------|--------------|------------|--------------|
| Easy     | 75%          | 70%          | Low        | Slightly slower |
| Medium   | 100%         | 100%         | Normal     | Normal |
| Hard     | 150%         | 150%         | High       | Slightly faster |
| Hardcore | 200%         | 200%         | Very high  | Faster |

All values require playtesting.

---

## 7. Co-Op

### 7.1 Design Intent
Players may:
- Explore together
- Split up and contribute to the same world
- Share stronghold progression
- Keep personal character progression separate within that world

### 7.2 Initial Target
- Up to 3 players per world/session for early versions
- PvE only
- PvP is deferred

### 7.3 Co-Op Rules
- Combat remains server-authoritative for all players
- Enemy ownership and damage attribution must remain deterministic
- Shared stronghold systems must validate permissions server-side
- One player’s death does not automatically fail the run for others

### 7.4 Not in Vertical Slice
Multiplayer networking, ranks, and shared vault permissions are out of scope for the current starting-area slice unless explicitly requested.

---

## 8. World-Specific Progression

### 8.1 Core Rule
Each world is its own progression container.

That means separate:
- Character progression
- Inventory
- Equipment
- Resources
- Currency
- Castle state
- Difficulty

### 8.2 Practical Consequences
- A player may use different classes or builds in different worlds
- Nothing transfers between worlds by default
- Returning to a world restores that world’s saved character state
- Joining a world for the first time creates a character bound to that world

### 8.3 Ownership Direction
- World creator has ownership/control in the initial design
- Authorized members should be able to enter while the owner is offline
- Exact offline simulation behavior remains an open implementation question

---

## 9. Equipment Rules

### 9.1 Purpose
Equipment should express build identity and risk/reward, not become an endless stat treadmill.

### 9.2 Vertical-Slice Equipment
- One starter weapon: RustySword
- Damage attribute driven
- No complex upgrade tree yet
- No full inventory UI required for the slice

### 9.3 Future Equipment Direction
Possible later systems:
- Weapons with distinct movesets
- Armor sets with readable silhouettes and moderate set identity
- Upgrade paths tied to resources and exploration
- Class-oriented equipment without hard locking every item

### 9.4 Rules
- Equipment effects must be server-validated
- Visual cosmetics must not grant hidden power
- Carried equipment is subject to the world’s death rules
- Stored stronghold equipment should remain safer than carried gear
- Item use animations must feel physical and intentional
  - sword grip
  - torch carry
  - flask use
  should all read as grounded actions

### 9.5 Art Direction Note
Armor should trend toward a slimmer, more detailed knightly silhouette rather than bulky generic plate. Dark Souls III Knight-set readability is a useful reference for proportion and clarity, not a target for copying protected designs.

---

## 10. System Interactions

### Combat + Stamina
Players who spam attacks or dodges should become vulnerable.

### Combat + SafeZone
The courtyard remains a protected recovery space. Enemies do not continue effective offense into it.

### Combat + Death
Failed combat creates resource risk through dropped carried items and difficulty-based penalties.

### Combat + Co-Op
Players can rescue one another, contest the same enemies, and share world progress without sharing personal loadouts across worlds.

### Combat + Equipment
Better gear should improve options and power, but never replace the need for timing and positioning.

---

## 11. Implementation Priorities

### Now
- Reliable starter attack
- Ashen Ghoul readable AI
- Death drop / respawn loop
- SafeZone cancel rules
- Config-driven combat numbers

### Next
- Stamina as a real combat constraint
- Better equip/use animations for sword, torch, and flask
- Clearer hit feedback

### Later
- Full difficulty matrix
- Co-op session flow
- Equipment upgrades
- Expanded enemy roster
- Hardcore resolution rules

---

## 12. Open Questions
- Final Hardcore handling of gear and shared contributions
- Exact stamina numbers after playtest
- Whether blocking arrives before or after heavy attacks
- How much equipment power budget exists before combat readability suffers
- Offline world behavior for co-op strongholds

---

## 13. Design Rule

Do not expand combat systems for content’s sake.

First make one enemy, one weapon, one death loop, and one recovery space feel good.  
Only then layer stamina depth, equipment depth, and co-op complexity on top.
