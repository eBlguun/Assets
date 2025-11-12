# Assets
ServerStorage/
└── Classes/
    │
    ├── Core/
    │   ├── GameManager.lua           [ModuleScript] -- Singleton service
    │   ├── DataManager.lua            [ModuleScript] -- Singleton service (DataStore handling)
    │   └── ServerConfig.lua           [ModuleScript] -- Configuration table
    │
    ├── Player/
    │   ├── PlayerData.lua             [ModuleScript] -- OOP Class
    │   ├── Inventory.lua              [ModuleScript] -- OOP Class
    │   ├── Deck.lua                   [ModuleScript] -- OOP Class
    │   ├── Collection.lua             [ModuleScript] -- OOP Class
    │   └── PassiveBuffManager.lua     [ModuleScript] -- OOP Class (5 slot system)
    │
    ├── Card/
    │   ├── Card.lua                   [ModuleScript] -- Base OOP Class
    │   ├── WeaponCard.lua             [ModuleScript] -- OOP Class (inherits Card)
    │   ├── ToolCard.lua               [ModuleScript] -- OOP Class (inherits Card)
    │   ├── SpellCard.lua              [ModuleScript] -- OOP Class (inherits Card)
    │   ├── CharacterCard.lua          [ModuleScript] -- OOP Class (Legendary Cards)
    │   └── CardFactory.lua            [ModuleScript] -- Factory pattern service
    │
    ├── Combat/
    │   ├── CombatManager.lua          [ModuleScript] -- Singleton service
    │   ├── DamageCalculator.lua       [ModuleScript] -- Utility module
    │   ├── StatusEffect.lua           [ModuleScript] -- OOP Class (buffs/debuffs)
    │   └── SkillSystem.lua            [ModuleScript] -- Singleton service
    │
    ├── Systems/
    │   ├── DurabilitySystem.lua       [ModuleScript] -- Singleton service
    │   ├── EnhancementSystem.lua      [ModuleScript] -- Singleton service (fusion)
    │   ├── CardForge.lua              [ModuleScript] -- Singleton service (repair/upgrade)
    │   ├── PackCraftingSystem.lua     [ModuleScript] -- Singleton service (10 item packs)
    │   └── ShopSystem.lua             [ModuleScript] -- Singleton service
    │
    └── World/
        ├── ZoneManager.lua            [ModuleScript] -- Singleton service
        ├── LootSpawner.lua            [ModuleScript] -- Singleton service
        └── BossManager.lua            [ModuleScript] -- Singleton service (basic bosses)

ReplicatedStorage/
├── Modules/
│   ├── CardData.lua               [ModuleScript] -- Data table (all card definitions)
│   ├── GameConstants.lua          [ModuleScript] -- Constants (rarities, drop rates)
│   ├── Enums.lua                  [ModuleScript] -- Enums (CardType, Rarity, Enhancement)
│   └── Utilities.lua              [ModuleScript] -- Helper functions
│
├── Remotes/
│   ├── CardRemotes/               [Folder]
│   │   ├── MaterializeCard        [RemoteFunction]
│   │   ├── EquipCard              [RemoteEvent]
│   │   ├── UseCardSkill           [RemoteEvent]
│   │   └── RepairCard             [RemoteFunction]
│   │
│   ├── CombatRemotes/             [Folder]
│   │   ├── StartBattle            [RemoteEvent]
│   │   ├── DealDamage             [RemoteEvent]
│   │   └── EndBattle              [RemoteEvent]
│   │
│   ├── ShopRemotes/               [Folder]
│   │   ├── BuyPack                [RemoteFunction]
│   │   ├── BuyItem                [RemoteFunction]
│   │   └── SellCard               [RemoteFunction]
│   │
│   └── SystemRemotes/             [Folder]
│       ├── FuseCards              [RemoteFunction]
│       ├── CraftPack              [RemoteFunction]
│       ├── ActivateBuff           [RemoteEvent]
│       └── UpgradeSkillLevel      [RemoteFunction]
│
└── Assets/
    └── CardModels/                [Folder] -- Card tool models

ServerScriptService/
├── Server.lua                     [Script] -- Main server initializer
│
├── Handlers/
│   ├── PlayerJoinHandler.lua      [Script] -- PlayerAdded event
│   ├── PlayerLeaveHandler.lua     [Script] -- PlayerRemoving event
│   ├── CombatHandler.lua          [Script] -- Combat remote handlers
│   ├── CardHandler.lua            [Script] -- Card remote handlers
│   ├── ShopHandler.lua            [Script] -- Shop remote handlers
│   └── SystemHandler.lua          [Script] -- System remote handlers (fusion, craft, etc.)
│
├── Systems/
│   ├── AutoSaveLoop.lua           [Script] -- Periodic data saving (every 5 mins)
│   ├── LootSpawnLoop.lua          [Script] -- Respawn loot chests/cards
│   └── AntiExploit.lua            [Script] -- Security checks
│
└── Tests/
    └── TestCardSystem.lua         [Script] -- Testing (disable in production)


StarterPlayer/
└── StarterPlayerScripts/
    ├── ClientLoader.lua           [LocalScript] -- Main client initializer
    │
    ├── Controllers/
    │   ├── UIController.lua        [ModuleScript] -- UI management
    │   ├── CardController.lua      [ModuleScript] -- Card UI & materialization
    │   ├── InventoryController.lua [ModuleScript] -- Inventory UI
    │   ├── DeckController.lua      [ModuleScript] -- Deck management UI
    │   ├── CombatController.lua    [ModuleScript] -- Combat UI & effects
    │   ├── ShopController.lua      [ModuleScript] -- Shop UI
    │   └── ForgeController.lua     [ModuleScript] -- Card forge/fusion UI
    │
    ├── UI/
    │   ├── InventoryUI.lua         [LocalScript] -- Inventory screen handler
    │   ├── DeckBuilderUI.lua       [LocalScript] -- Deck builder screen
    │   ├── CombatUI.lua            [LocalScript] -- Combat HUD
    │   ├── ShopUI.lua              [LocalScript] -- Shop interface
    │   ├── ForgeUI.lua             [LocalScript] -- Forge/upgrade interface
    │   └── CollectionUI.lua        [LocalScript] -- Collection museum display
    │
    └── Effects/
        ├── CardVFX.lua             [ModuleScript] -- Card effect manager
        └── CombatVFX.lua           [ModuleScript] -- Combat visual effects


StarterGui/
├── MainUI/                        [ScreenGui]
│   ├── InventoryFrame             [Frame]
│   ├── DeckBuilderFrame           [Frame]
│   ├── ShopFrame                  [Frame]
│   ├── ForgeFrame                 [Frame]
│   └── CollectionFrame            [Frame]
│
└── CombatUI/                      [ScreenGui]
    ├── HealthBar                  [Frame]
    ├── StaminaBar                 [Frame]
    ├── CardHand                   [Frame]
    ├── SkillButtons               [Frame]
    └── DurabilityDisplay          [Frame]

Workspace/
├── Zones/                         [Folder]
│   ├── StarterZone                [Model]
│   ├── ForestZone                 [Model]
│   └── MountainZone               [Model]
│
├── LootSpawns/                    [Folder] -- Invisible parts for spawn locations
│   ├── CommonSpawn1               [Part]
│   ├── RareSpawn1                 [Part]
│   └── BossChest1                 [Part]
│
├── BossArenas/                    [Folder]
│   └── BasicBossArena             [Model]
│
└── NPCs/                          [Folder]
    ├── ShopKeeper                 [Model + Script]
    ├── CardForge                  [Model + Script]
    └── CollectionNPC              [Model + Script]
