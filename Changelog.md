# Changelog

Here you can discover the latest changes to the API.

### 30th September 2026
- `interactable` now gives mobs the same interaction priority as mesh entities: `onPlayerAltAction` runs before held-item actions. Native harvesting, mounting and pet actions are disabled while the setting is true; leave it false for ordinary mob behaviour.

### 29th September 2026
- Added project schematics to World Code! Use the `+` beside "Schematics" in the Code Editor to upload a `.bloxdschem` file or choose a snapshot from your profile. Projects support up to five schematics.
- Added [`api.setChunkAsSchematic`](#apiReference-setChunkAsSchematic) to replace one complete chunk, including air and block data. Positions are block coordinates selecting their containing chunks, like `copyChunk`.
- Added [`api.getSchematicDimensions()`](#apiReference-getSchematicDimensions) to read a schematic's stored `[x, y, z]` dimensions, including selection padding.
- `@plugins/sessionBasedGame` maps and lobbies now accept `source: { schematic: "maps/arena.bloxdschem" }` as an alternative to a world rectangle. Maps are pasted directly and must fit the available placement and block-data budgets.
- Schematic saved from profiles are **snapshots** - you must update the schematic in the world/custom game if you edit the linked schematic otherwise.
- See the [project schematics how-to](#howTo-Project schematics) for examples and details.

### 10th September 2026
- Added the `interactable` entity setting. Set it on a mesh entity or a mob (e.g. `api.setTargetedPlayerSettingForEveryone(eId, "interactable", true)`) to make alt actioning it count as an interaction: the alt action is consumed, so a held block isn't placed through the entity and a held throwable isn't used, and `onPlayerAltAction` fires with the entity as the target. Leave it off for decorative entities, or they will swallow player input. It also makes the entity pickable, so an entity can be `interactable` with `canAttack` off: the player's aim lands on it and interacting works, but swinging or shooting at it does nothing. On a mob it applies alongside the usual mounting and taming behaviour rather than replacing it, though there a held throwable or consumable is still used in preference to interacting.
- Entities marked `interactable` now show an interaction prompt by the crosshair while a player is looking at them, showing the key bound to the interact action (`E` by default) so players can tell which input to use. Set the new `interactionPrompt` entity setting to replace the label beside that key, e.g. `api.setTargetedPlayerSettingForEveryone(eId, "interactionPrompt", "Open Shop")`, and leave it off for the default "Interact". It takes plain text or `CustomTextStyling`, and describes what interacting does rather than which button to press, since the engine draws the key itself. Not shown on touchscreens, where a tap needs no prompt.

### 8th September 2026
- Added first-party plugins!
  - You can import helper scripts to help you from `@plugins`
    - E.g. `import { setTimeout } from "@plugins/helpers"`
  - See the [plugins how-to](#howTo-Plugins) for more information.

  - The biggest example of this is **Session-Based Game**, a helper to help you create a game with mini-matches, like Bedwars/Skywars etc.
    - `import { createGame } from "@plugins/sessionBasedGame"` to check it out
    - See the [SessionBasedGame how-to](#howTo-Building a SessionBasedGame) for more information.

### 28th August 2026
- Added vehicle spawn callbacks:
    - [`onPlayerAttemptSpawnVehicle`](#callbacks-onPlayerAttemptSpawnVehicle)
    - [`onWorldAttemptSpawnVehicle`](#callbacks-onWorldAttemptSpawnVehicle)
    - [`onPlayerSpawnVehicle`](#callbacks-onPlayerSpawnVehicle)
    - [`onWorldSpawnVehicle`](#callbacks-onWorldSpawnVehicle)
    - [`onVehicleDespawned`](#callbacks-onVehicleDespawned)
- Added [`onEntityDeleted`](#callbacks-onEntityDeleted), called whenever any non-player entity is deleted.

### 27th August 2026
- Added [`api.attemptSpawnVehicle`](#apiReference-attemptSpawnVehicle) to spawn rideable vehicles (boats, karts, cars) which players can mount with an alt action.

### 18th August 2026
- Added TypeScript support to World Code!
  - Rename your file from `index.js` to `index.ts` to use TypeScript files!
  - Import some of our types from `@bloxd`
    - E.g. `import type { PlayerId } from "@bloxd"`
  - Use your own types!
  - Type Checking is supported and errors will show if you use incorrect types.

- Multiple files are supported! Split your code across multiple files for better readability!
  - Create `.js` and `.ts` files
  - Use `import` / `export` syntax to be able to import files across between versions
  - `index.js` or `index.ts` MUST be where your callbacks are stored and cannot be imported anywhere else.
  - `require()` syntax technically works although isn't type-safe.

- Increased total code size from 64k characters to 512k characters.

- Added variations to custom games!
    - Each custom game can now have multiple variations, with `default` being the default variation.
    - [`api.getVariation`](#apiReference-getVariation)
    - [`api.matchmakeToVariation`](#apiReference-matchmakeToVariation)
    - See the [variations doc](#howTo-Variations) for more information.

### 11th August 2026
- Added currency API:
    - [`api.setCurrency`](#apiReference-setCurrency)
    - [`api.getCurrency`](#apiReference-getCurrency)
    - [`api.deleteCurrency`](#apiReference-deleteCurrency)
    - [`api.getCurrencyAmount`](#apiReference-getCurrencyAmount)
    - [`api.setCurrencyAmount`](#apiReference-setCurrencyAmount)
    - [`api.giveCurrencyAmount`](#apiReference-giveCurrencyAmount)
    - These currencies can be used in the `currency` field in the Shop to automatically deduct currency on purchase.
    - These can also be persisted across sessions.

### 10th August 2026
- Added to the UI Request System:
    - [`api.addUiRequestPopup`](#apiReference-addUiRequestPopup)

### 7th August 2026
- Added UI Request System:
    - [`api.addUiRequest`](#apiReference-addUiRequest)
    - [`api.deleteUiRequest`](#apiReference-deleteUiRequest)
    - [`onUiRequestResponded`](#callbacks-onUiRequestResponded)

### 20th July 2026
- New mob settings:
    - [`runningJumpInfo`](#mobSettings-runningJumpInfo)
    - [`runningRandomFacingInfo`](#mobSettings-runningRandomFacingInfo)
    - [`runningSlideInfo`](#mobSettings-runningSlideInfo)
    - [`walkingJumpInfo`](#mobSettings-walkingJumpInfo)
    - [`walkingRandomFacingInfo`](#mobSettings-walkingRandomFacingInfo)
    - [`walkingSlideInfo`](#mobSettings-walkingSlideInfo)

### 13th July 2026
- Added new [`bridgeInfo`](#mobSettings-bridgeInfo) mob setting to make mobs build bridges or leave trails, e.g.:
```js
bridgeInfo: {
    blockToPlace: "Bricks",
    mustBeGrounded: false,
    yOffset: -1,
},
```

### 26th June 2026
- Added new client options for custom game UI:
    - [`middleTextTop`](#clientOptions-middleTextTop)
    - [`headerChips`](#clientOptions-headerChips)
- Added ProgressBar support to options that accept a CustomTextStyling
    - e.g. `["Loading", { type: "ProgressBar", progress: 0.5 }]`

### 15th June 2026
- Added new client options for adjusting the third person camera:
    - [`cameraRotationOffset`](#clientOptions-cameraRotationOffset)
    - [`cameraPositionOffset`](#clientOptions-cameraPositionOffset)

### 11th June 2026
- Added the ability to get and set scale for lifeforms (players, mobs)
    - [`api.getLifeformScale`](#apiReference-getLifeformScale)
    - [`api.setLifeformScale`](#apiReference-setLifeformScale)

- Added new block standing callbacks:
    - This is to prevent having to use `onBlockStand`, which eats into your runtime limit significantly due to its high frequency of being called
    - [`onBlockStandStart`](#callbacks-onBlockStandStart)
    - [`onBlockStandStop`](#callbacks-onBlockStandStop)

- Added `isReceiveDamageCooldownGlobal` [client option](#clientOptions-isReceiveDamageCooldownGlobal) and [mob setting](#mobSettings-isReceiveDamageCooldownGlobal)

### 10th June 2026
- Added [`showChatBubbles`](#clientOptions-showChatBubbles) client option
- Added [`useRespawnButton`](#clientOptions-useRespawnButton) client option
- Added [`multilineTextBox`](#entitySettings-multilineTextBox) entity setting
- Added [`lobbyLeaderboardTags`](#entitySettings-lobbyLeaderboardTags) entity setting

### 18th May 2026
- Added the ability to change the gamemode of a player
    - [`api.setPlayerGamemode`](#apiReference-setPlayerGamemode)
    - [`api.getPlayerGamemode`](#apiReference-getPlayerGamemode)
- Added the ability to queue text to be displayed in different places on the screen
    - [`api.queueMiddleTextLower`](#apiReference-queueMiddleTextLower)
    - [`api.queueMiddleTextUpper`](#apiReference-queueMiddleTextUpper)
    - [`api.queueCrosshairText`](#apiReference-queueCrosshairText)
    - [`api.getQueuedStatus`](#apiReference-getQueuedStatus)
    - [`api.removeFromQueue`](#apiReference-removeFromQueue)

### 15th May 2026
- Significantly increased runtime limit - interrupts should be less frequent
- Added [`api.isNearInterrupt`](#apiReference-isNearInterrupt)

### 12th May 2026
- Added new client option [`groundArrowPath`](#clientOptions-groundArrowPath)

### 11th May 2026
- Added Changelog
- Added a bunch of new API Methods:
    - [`api.addCustomKillfeedMessage`](#apiReference-addCustomKillfeedMessage)
    - [`api.deleteAllItems`](#apiReference-deleteAllItems)
    - [`api.findItem`](#apiReference-findItem)
    - [`api.findStandardChestItem`](#apiReference-findStandardChestItem)
    - [`api.getEffectLevel`](#apiReference-getEffectLevel)
    - [`api.getItemDropName`](#apiReference-getItemDropName)
    - [`api.getItemIDsOverlappingWithPlayer`](#apiReference-getItemIDsOverlappingWithPlayer)
    - [`api.getMobDbId`](#apiReference-getMobDbId)
    - [`api.hasEffect`](#apiReference-hasEffect)
    - [`api.preventFallDamageNextGrounding`](#apiReference-preventFallDamageNextGrounding)
    - [`api.removeItemNameFromStandardChest`](#apiReference-removeItemNameFromStandardChest)
    - [`api.resetCanChangeBlock`](#apiReference-resetCanChangeBlock)
    - [`api.resetCanPickUpItem`](#apiReference-resetCanPickUpItem)
    - [`api.setOtherEntitySettingToDefault`](#apiReference-setOtherEntitySettingToDefault)
    - [`api.updateMeshParticleSystems`](#apiReference-updateMeshParticleSystems)

### 7th May 2026
- Added [`api.copyChunk`](#apiReference-copyChunk)
- Added [`onPlayerToggledShopMenu`](#callbacks-onPlayerToggledShopMenu)

### 28th April 2026
- Doubled code block size (16000 -> 32000)
- 'Hide World Code' option renamed to 'Hide Code' and also hides code of Code Blocks
- New [docs page!](https://bloxd.io/docs)

### 23rd April 2026
- Added [`api.matchmakePlayer`](#apiReference-matchmakePlayer)
- Added per-item gun stats in `customAttributes.gunStats`

### 21st April 2026
- Added database methods to write persisted data to be saved between sessions.
- There are two sections of database values:
    - Lobby
        - [`api.getLobbyDBValue`](#apiReference-getLobbyDBValue)
        - [`api.setLobbyDBValue`](#apiReference-setLobbyDBValue)
        - [`api.deleteLobbyDBValue`](#apiReference-deleteLobbyDBValue)
        - [`api.deleteAllLobbyDBValues`](#apiReference-deleteAllLobbyDBValues)
        - Writes data that is persisted to the *lobby*, whether it's a worlds lobby or a lobby within a custom game
    - Player
        - [`api.getDBValue`](#apiReference-getDBValue)
        - [`api.setDBValue`](#apiReference-setDBValue)
        - [`api.deletePlayerDBValue`](#apiReference-deletePlayerDBValue)
        - [`api.deleteAllPlayerDBValues`](#apiReference-deleteAllPlayerDBValues)
        - Writes data that is persisted to the player. For custom games, this data persists *between* lobbies

### 16th April 2026
- Added [`api.setItemStat`](#apiReference-setItemStat)

### 11th April 2026
- The Code Editor now supports **collaborative editing**
    - You can now simultaneously code with your other fellow coders at the same time! Hop into a code block or World Code together to simultaneously create the best World or the best Custom Game ever!!

### 7th April 2026
- New Code Editor UI! Syntax Highlighting, Autocomplete, Error Checking, and more!

### 25th March 2026
- Increased world code size from 16000 to 64000 characters

### 24th March 2026
- Added mesh entities!
    - [`api.attemptCreateMeshEntity`](#apiReference-attemptCreateMeshEntity)
    - [`api.updateMeshEntity`](#apiReference-updateMeshEntity)
    - [`api.deleteMeshEntity`](#apiReference-deleteMeshEntity)
- Added throwables!
    - [`api.attemptCreateThrowable`](#apiReference-attemptCreateThrowable)
    - [`api.deleteThrowable`](#apiReference-deleteThrowable)

### 23rd March 2026
- Added the ability to open and close chests for players, going beyond reach distance to open chests from far away
    - [`api.openChestForPlayer`](#apiReference-openChestForPlayer)
    - [`api.closeChestForPlayer`](#apiReference-closeChestForPlayer)

### Previous Changes
- For previous changes, see the [code-api](https://github.com/Bloxdy/code-api) repository or #dev-log in the Bloxd Discord server