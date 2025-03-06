# ServerRewards

Oxide plugin for Rust. Earn reward points in various ways and spend them on many different options in the GUI reward store

## Features

* GUI reward store
* Purchase Items, Kits and Commands
* Exchange Reward Points (RP) for Economics money (and vice-versa) with the built in money exchanger *(requires Economics)*
* Add descriptions to your Kits/Commands
* Add pictures to your items
* Add multiple commands to 1 purchase (See below for datafile layout)
* The item store can hold 21 items per page and as many pages as your wish to add
* Pricing is using RP only; Economics money is added as a means of exchanging for points to use with GUI Shop
* Option to disable any category of reward
* Create custom permissions to give VIPs more points for playtime
* Logs all points added and spent to combat scammers
* NPC Reward dealers
* Assign individual store lists to NPCs

Players can use the reward points (RP) to buy kits/items or commands through the reward store.

## Adding a Kit

To add a kit to the reward store you must first create the kit using the Kits plugin. Once you have done that it is as simple as writing a chat command.

ex. `/rewards add kit <name> <kitname> <cost>` - `<name>` is the name in store, `<kitname>` is the name of the kit, `<cost>` is the price in RP

This can also be achieved by using the console command with the same syntax
Once you have completed that step you can also add a description (if you wish). This can be done via the "edit" chat or console commands provided (see below).
Alternatively you can generate a description using the kit's contents by enabling "Display kit contents as the description" in the UIOption section of the config.

## Adding an Item

To add a item to the reward store is simple. Spawn the item you want to use, place it in your hands, and then type the chat command.

ex. `/rewards add item <cost>` - This will add the item that is in your hand to the reward store for a RP price of `<cost>`. It will also save the amount and skin of the item so keep this is mind when adding your prices

**- Through console commands**
The syntax for adding a item via console commands is as follows;
`rewards add item <shortname> <skinId> <amount> <cost>`
If I wanted to add a Assault Rifle for 10RP I would type like so - "rewards add item rifle.ak 0 1 10"
*Note - Items will be automatically sorted into the appropriate category!

## Adding a Command

To add a command use the chat command to add a console command with variables.

ex. `/rewards add command <name> <command> <cost>` - `<name>` is the name in store, `<command>` is the chat command you wish to run, `<cost>` is the price in RP. 

This can also be achieved by using the console command with the same syntax.
Once that is complete you can add a description using the commands provided (see below)

One example of a command is "say RockOn!, $player.name!" which will make the player say RockOn! :p

The current variables you can use in a command are:
```csharp
$player.name - Player display name
$player.id - Player Steam ID
$player.x, $player.y, $player.z - Vector3 coordinates
```

To add or remove commands to this purchase you can use the commands provided (see below). Make sure all commands entered are inside of quotation marks!

**Adding descriptions and images to your store items**
To add descriptions and images (to kits and commands only) you must use the commands provided

**How to add/remove a NPC Reward Dealer**

 * Stand infront of the NPC you wish to add or remove
 * Type '_/srnpc add_' to add or _'/srnpc remove'_ to remove
 * You will then be prompted to "use" the NPC (default E)
 * The next NPC you "use" will be the one that is affected

**How to add custom loot table to a NPC Reward Dealer**

 * Stand infront of the NPC you wish to add
 * Type '_/srnpc loot_'
 * Press the "USE" key on the NPC you wish to edit.
 * A GUI will popup, simply select the items/kits/commands you wish this NPC to sell and press save

**How to activate items for sale (Requires data file editing)**

 * Upon load your server will generate a datafile. It can be found inside the "oxide/data/ServerRewards/" folder and is named "sale_data.json"
 * Inside this file you will see a entry for every in game item and skin. This list will be automatically updated as new items or skins are added
 * An example is below. **The sale price is the price per unit!** So 1 wood, 1 stone, 1 Assault Rifle etc. This price is multiplied by the amount the user wishes to sell
 * Simply adjust the "price" of the item to your desired amount, and set "enabled" to true

```json
"rifle.ak": { // This is the items shortname. Below this you will find all recorded skin ID's for this item with their own sale price, name, and status
      "0": { // This is the skin ID of this item
        "price": 0.0, // The price per unit it sells for
        "displayName": "Assault Rifle", // The name displayed in the sell menu
        "enabled": false // Whether or not this item can be sold in the sell menu
      },
      "10135": {
        "price": 0.0,
        "displayName": "Digital Camo AK47",
        "enabled": false
      },
      "10137": {
        "price": 0.0,
        "displayName": "Military Camo AK47",
        "enabled": false
      },
      "10138": {
        "price": 0.0,
        "displayName": "Tempered AK47",
        "enabled": false
      }
    },
```

## Lusty Map Icons

There is support to add a map icon through **LustyMap** on this dealers location. To add a icon simply place the icon images in the `data/LustyMap/custom` folder prior to adding the new NPC. This file ***MUST*** be name `rewarddealer.png`. The icon will automatically be loaded into LustyMap. Here is a simple icon to use, or you can create your own.

## Logging

The point log can be found under `logs/ServerRewards_RP_Log.txt`. It shows all points that have been added and spent, it will record the players name (if online) and steam ID, the time and the amount of points added/subtracted

## Chat Commands

* `/rewards check` - Display current reward points and total time played on server
* `/s` - Opens the reward store

## Admin Commands

* `/rewards add kit <name> <kitname> <cost>` - Add a new kit to the reward store. 
* `/rewards add item <cost> <opt:bp>` - Add a new item to the reward store (Add "bp" to the end of the command to make this item a blueprint). 
* `/rewards add command <name> <command> <cost>` - Add a new command to the reward store. 

* `/rewards edit kit <ID> <cost|description|name|icon|cooldown>` - Edit information for the specified reward kit
* `/rewards edit item<ID> <cost|amount|icon|cooldown>` - Edit information for the specified reward kit
* `/rewards edit command<ID> <cost|description|name|icon|add|remove|cooldown>` - Edit information for the specified reward kit

* `/rewards remove kit <name>` - Remove a kit from the reward store. 
* `/rewards remove item <name>` - Remove a kit from the reward store. 
* `/rewards remove command <name>` - Remove a kit from the reward store. 

* `/rewards list <items|commands|kits>` - Lists all rewards of the specified type, along with their specific ID in console.

**Manual RP addition, deduction, checking**
* `/sr add <playername> <amount>` - Add points to players profile
* `/sr take <playername> <amount>` - Subtracts points from players profile
* `/sr clear <playername>` - Removes the players reward profile
* `/sr check<playername>` - Display the players points

**---- NPC Dealers ----**
* `/srnpc add` - Adds the NPC you are looking at to the Reward Dealer list
* `/srnpc remove` - Remove the NPC you are looking at from the Reward Dealer list
* `/srnpc loot` - Create or replace a custom loot list for a NPC


## Console Commands

* `sr add <playername> <amount>` - See above^
* `sr take <playername> <amount>` ^^
* `sr clear <playername>` ^^
* `sr check <playername>` ^^

* `rewards add kit <name> <kitname> <cost>` - Add a new kit to the reward store.
* `rewards add item <shortname> <skinId> <amount> <cost> <opt:bp>` - Add a new item to the reward store (Add "bp" to the end of the command to make this item a blueprint).
* `rewards add command "name" "command" <cost>` - Add a new command to the reward store.

* `rewards edit kit <ID> <cost|description|name|icon|cooldown>` - Edit information for the specified reward kit
* `rewards edit item<ID> <cost|amount|icon|cooldown>` - Edit information for the specified reward kit
* `rewards edit command<ID> <cost|description|name|icon|add|remove|cooldown>` - Edit information for the specified reward kit

* `rewards remove kit <name>` - Remove a kit from the reward store.
* `rewards remove item <name>` - Remove a kit from the reward store.
* `rewards remove command <name>` - Remove a kit from the reward store.

* `/rewards list <items|commands|kits>` - Lists all rewards of the specified type, along with their specific ID in console.`

## Configuration

```json
{
  "Active categories (global)": { // Enable or disable tabs from being displayed
    "Show commands tab": true,
    "Show exchange tab": true,
    "Show items tab": true,
    "Show kits tab": true,
    "Show seller tab": true,
    "Show transfer tab": true
  },
  "Coloring": { // Change text and UI coloring
    "Background color": {
      "Hex color": "#2a2a2a",
      "Transparency (0 - 1)": 0.98
    },
    "Button color - accept": {
      "Hex color": "#00cd00",
      "Transparency (0 - 1)": 0.9
    },
    "Button color - inactive": {
      "Hex color": "#a8a8a8",
      "Transparency (0 - 1)": 0.9
    },
    "Button color - standard": {
      "Hex color": "#2a2a2a",
      "Transparency (0 - 1)": 0.9
    },
    "Primary panel color": {
      "Hex color": "#696969",
      "Transparency (0 - 1)": 0.3
    },
    "Primary text color": "#ce422b",
    "Secondary panel color": {
      "Hex color": "#373737",
      "Transparency (0 - 1)": 0.98
    },
    "Secondary text color": "#939393"
  },
  "Currency exchange rates": { // Currency exchange rates
    "Value of Economics": 100,
    "Value of RP": 1
  },
  "Options": {
    "Data save interval": 600,
    "Log all transactions": true,
    "Use NPC dealers only": false
  },
  "UI Options": {
    "Disable fade in effect": true,
    "Display kit contents as the description": true,
    "Display user playtime": true
  }
}
```

## Localization

## Developer API

```csharp
object AddPoints(ulong playerID, int amount) // Returns true if successful, otherwise returns null
object TakePoints(ulong playerID, int amount) // Returns true if successful, otherwise returns null
object object CheckPoints(ulong ID) // Returns int, or null if no data is saved
```

### API Example

```csharp
[PluginReference]
private Plugin ServerRewards;

private void ExampleFunction(BasePlayer player)
{
     ServerRewards?.Call("AddPoints", player.userID, 5);
     ServerRewards?.Call("TakePoints", player.userID, 5);
}
```