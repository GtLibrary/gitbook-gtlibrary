# TimeCube.sol

TimeCube.sol

::Understanding the Master Game Contract::

Relevant Contracts:

Constants.sol

Base.sol

Hero.sol

MyItems.sol

BaseLoot.sol

BaseSpells.sol

TimeCube.sol

### Constants.sol – Structures and Numbers <a href="#_tthj6yw7e9h0" id="_tthj6yw7e9h0"></a>

This contract is virtual in that it defines no contract at all but structures and contents used by the game.

The **Screwdrivers** struct is used by damage and healing in the game. The way it works is any damage or healing coming in, be they instant or Damage Over Time (DOT) or Healing Over Time (HOT) a screwdriver is made and then “tightened.”

The **HeroXYZ** struct is for the location of objects, specifically the heroes.

The **HeroItem** struct is for all the different stats for the items heroes can create using TimeCube.sol.

The **HPSummary** is a summary of stats from the Hero and the hero’s items’ HeroItems. Basically, all the HeroItems are summed into this one summary struct along with the hitpoints (HP) the hero has.

The **Stats** struct is for additional stats beyond just HP or Power. It lists secondary stats like intelligence, agility, and strength as well as the effects the hero currently has equipped.

**HeroTalents** and **PowSum** are currently not used but will need to be implemented if we want to have mana and talents in the game.

The “**SpellBook**” is 150 spells long. A buffer to 1000 is used in case we want to ever add additional spells.

**CUBE\_TIME** and **CUBE\_DUST** are used for testing TimeCube.sol.

Hero flags start at 10000 and are used to indicate if a hero is a Non Player Character (NPC), if the Hero is an enemy, or if the hero was summoned. These like those above may not conflict with the other numbers used in Constants.sol so far.

**EQUP\_SLOTS** is how many different items a Hero is allowed to wear at any one time.

**MAX\_CUBE\_SLOTS** is how many slots exist in the cube for holding items.

**Damage Types** are a bitfield of the four types of damage: **FIRE**, **ICE**, **PHYSICAL** and **META** damage.

**HEAL\_BY\_PERCENT** is also part of this bitfield. (Status of its use is unknown.)

**HOT\_SLOTS** and **DOT\_SLOTS** are the maximum number of “ticks” of healing or damage alowed. Currently only 5 ticks are allowed per.

The various **FIZZLES** are the different ways a spell or ability may fail to work properly for the user while still not resulting in a revert from the system. If someone casts a spell that is out of range for example, the spell will still cast and the transaction will still complete but will return with an error code of FIZZLE\_NOT\_IN\_RANGE.

**LEGO\_CAST** and **LEGO\_ACTIVATE** are flags for the legendary system to know how to play the legendary ability. For example, when someone casts Arcane Orb using castAO(), the legendary system is told to play the cast ability. But when the user activates the Arcane Orb using activateAO() the legendary subsystem is told to use the activate ability instead. This allows for a lego to work off of cast or activation of different spells and abilities.

The **HotsNDots** struct holds, in slots, the Screwdrivers from above.

There is currently only one of the 256 legendary effects and that effect is Iron Skin. The effects like **L\_IRON\_SKIN\_HEALS** are bitmasks.

### Base.sol – Upgradable ERC1155 Abstract Contract <a href="#_wnvqlpxiibot" id="_wnvqlpxiibot"></a>

This contract is abstract and needs other contracts to implement it. Basically, it is an upgradable ERC1155 which we use for both BaseLoot.sol and BaseSpells.sol.

The reason for this is spells can have a token component that can be transferred to the Hero.sol. In the case of BaseLoot.sol these ERC1155 tokens can be transferred to other player wallets.

### Hero.sol – A BookTradable/ERC721 <a href="#_1dgiglsanhbe" id="_1dgiglsanhbe"></a>

This is the “gate” contract, meaning users will not be able to log into the game without having a token from this contract. This contract is based on the ERC721 standard and minting should be done from the website/bookmarks. Currently the Hero contract depends on CulureCoin.sol, BaseSpells.sol and MyItems.sol.

Additionally, all mobs and monsters in the game are minted from this contract by the bridge. They then have their enemy flag set in BaseLoot.sol:

int constant FLAG\_IS\_ENEMY = 10002; // FROM Constants.sol

### BaseLoot.sol and MyItems.sol <a href="#_jzjb874pkero" id="_jzjb874pkero"></a>

For now let us assume that the users have their hero minted. They have already killed and looted a mob as well. So they have in their wallet one of the ERC1155 from the BaseLoot.sol which implements Base.sol:

The hero can then use the loot in the transmute functions inside Timecube.sol. The transmute functions turn the erc1155 they received into an erc721 token from MyItems.sol.

This conversion from erc1155 to erc721 happens in any of the transmute() functions in the TimeCube.sol code.

![](.gitbook/assets/0)

This is just one of the transmute functions in TimeCube.sol. The others take more than one loot type to “crunch” into items

So to recap, ERC1155 loot tokens have to be converted to the ERC721 item tokens which the heroes can wear using the transmute functions.

### Wearing and “Cubing” Items <a href="#_36hk05ltmr6i" id="_36hk05ltmr6i"></a>

The users can equip their items using the cube by calling equipItem().

![](.gitbook/assets/1)

This is a passthrough call into items.equiptItemFrom()

Just as the heroes can wear items using this function they can also put the item into a Timecube.sol slot of their choice. (If the slot is full, they get the old item out of the cube as they put the new one in.)

![](<.gitbook/assets/2 (2)>)

This function transfers from the user’s allet to the TimeCube.sol contract

Once the user has put items into the cube they can call transmuteCube() to take all the items in the cube and join their effects using the bitwise and operator.

![](<.gitbook/assets/3 (1)>)

This function destroys items in the cube for a hero and makes a new one for the user with all the effects from the originals

### Casting and Using Hero Abilities <a href="#_j14slptf74kz" id="_j14slptf74kz"></a>

Because of the need to use the effects of those items in the cube, all casts and abilities that have effects must call through the cube so that the effects can be applied.

![](<.gitbook/assets/4 (2)>)

This function, castAO(), calls spells.castAO() to do the work of the spell first, then it runs the items and cube effects

### Spells and Abilities Currently Implemented <a href="#_ey402qpjyv4c" id="_ey402qpjyv4c"></a>

function castAO(uint256 \_hId, uint256 \_to1, uint256 \_to2) public returns (int256) {

function activateAO(uint256 \_hId, uint256 \_target, int \_how, uint \_amount) public returns

function castRES(uint256 \_hId, uint256 \_target) public returns(uint) {

function castIS(uint256 \_hId) public returns(uint) {

It is important to realize we have to still fill in all the special abilities for the game. That means they will each get entry points in the TimeCube contract.

### The Legion Legendaries, AKA The Legion Legos <a href="#_62ycxm19qnw" id="_62ycxm19qnw"></a>

Currently there are no addon legion legendary effects written into the game. They would act as the expansion.

### The Map and Hero Movement <a href="#_2mmcd2fdlvtk" id="_2mmcd2fdlvtk"></a>

The map starts at 0,0,0, but if the hero is at 0,0,0 then they will automatically be jumped to 1ether, 1ether, 1ether when they begin their movement. Conventionally, 1 ether is 1 yard.

They call walkStart() to begin their movement, and walkEnd() to stop walking. They can only move 30 ether at a time between calls. This is to keep them from being able to move too far in 30 seconds. Ultimately this means, movement should be limited to 1 ether per second.

### The Mesh for the Map <a href="#_i36an0kunetp" id="_i36an0kunetp"></a>

The movement code relies on the triangle code. The mesh code uses nodes and edges to keep track of player movement.

The mesh is initialized such that the world has a triangle’s point at 1ether, 1ether, 1ether, a second point at 2 ether, 1 ether, and 1 ether as well as a final point at 1 ether, 2 ether, and 1 ether.

![](<.gitbook/assets/5 (2)>)

The world starts out as a triangle offset by x,y, and z of 1. Depth (z-axis) is not shown here.

The users should be able to move inside the red triangle. Currently, the contract creates a new point and triangles for the user when they move to an allowed point within the world. This means that all paths ever taken by players will be captured and available for replay. This bloats the network but for now that is fine. In the future we may need to clean up old points so the network doesn’t hate us

### Random Number Generation <a href="#_4qduawupzlz8" id="_4qduawupzlz8"></a>

There are no random numbers on the EVM so we currently simulate the random numbers by joining up entropy from the network and from the calls in an attempt to approximate random.

This randomness is used inside the diceTheItem() function. Inside it keccak256() is used to convert the sources of entropy into pseudo random numbers.

This is the reason we need to use Avalanche Subnets as they allow for random numbers to be generated from inside the contracts themselves.

### Other Functions <a href="#_gk6lz9qz8g6a" id="_gk6lz9qz8g6a"></a>

The other functions in the contract are support functions for these major features. They include cubeTime() which adds to the entropy as well as returns the current network time to the user.
