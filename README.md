# Paulo Henrique de Lima

### Unreal Engine Programmer | Gameplay, Systems & Multiplayer

I'm a game programmer with 6+ years of experience developing games with Unreal Engine. As Co-Founder and Game Programmer at South Blue Moon, I was responsible for programming two commercially released PC games: **Automate It: Factory Puzzle** and **Chased by Darkness**.

My experience includes C++, Blueprints, gameplay systems, multiplayer networking, AI, UI implementation, and service integrations. I particularly enjoy building interconnected gameplay systems and solving the technical problems behind them.

I also work as a freelance Unreal Engine programmer for independent developers and international clients.

[LinkedIn](https://www.linkedin.com/in/paulo-henrique-de-lima-7150261a0/)

## Automate It: Factory Puzzle

A factory-building puzzle game where players construct production layouts to complete level objectives.
</details><img width="1920" height="1080" alt="ss_7ab256ee69b0026d7edee93075426f2e4895c178 1920x1080" src="https://github.com/user-attachments/assets/76055857-6de7-4350-8364-ed3b4d922197" />

**Role:** Game Programmer & Co-Founder at South Blue Moon  
**Development began:** February 2024  
**Released:** May 27, 2025  
**Technology:** Unreal Engine, C++, Blueprints

[View on Steam](https://store.steampowered.com/app/2472770/Automate_It_Factory_Puzzle/)

I was responsible for the game's programming, including the factory simulation, construction system, camera, UI, level editor, and in-game service integrations. The core systems were implemented in C++, with Blueprints used primarily for UI and selected integrations.

### Key contributions

- Developed a centralized, fixed-step factory simulation in C++ designed for consistent puzzle outcomes across different frame rates.
- Built a custom grid-based construction system.
- Implemented machine configuration interfaces and other UI from designs provided by the team's artist.
- Developed an in-game level editor with JSON serialization, validation, and mod.io publishing and browsing.
- Integrated leaderboards, player profiles, achievements, and save functionality.

<details>
<summary><strong>Technical highlight: fixed-step factory simulation</strong></summary>

### Problem and approach

The earlier timer-based production logic produced different results depending on frame rate, particularly at lower FPS. I replaced it with a centralized simulation advancing in fixed 0.05-second steps.

The manager calculates how many simulation steps are due from the game clock and processes outstanding steps. This allows multiple simulation updates within one rendered frame rather than tying production progress to a single update per frame.

### Processing order and item flow

Each simulation step processes items, splitter storage, spawners, and production machines before evaluating objectives.

I implemented spatial ordering for spawners and production machines. Ordering them by grid position reduced dependence on the sequence in which the player constructed the layout.

Items move through a network of connected points with occupancy checks, filters, and throughput limits. When checking a destination, the system can process its occupying item first and then check for space again. This allows movement dependencies to be resolved within the same simulation step. Per-tick markers control repeated processing, while mergers use input-cycling rules to coordinate incoming items.

### Separating simulation from visuals

I separated fixed-step item simulation from visual movement. Each visual item references its simulation counterpart by ID and moves smoothly toward a target derived from the conveyor points, using delta time, conveyor speed, and the simulation time multiplier. This keeps movement smooth while the production logic advances in discrete steps.

I replaced Async Conveyor Plugin's production and simulation logic while retaining and adapting its instanced-mesh rendering pipeline, which passes item positions and rotations to materials through transform textures. My changes connected this visual layer to the custom simulation and handled presentation details such as delivery animations and elevator visuals.

</details>

<details>
<summary><strong>Construction, camera, and UI</strong></summary>

I created a custom C++ construction system tailored to the game's requirements, adapted the top-down camera for movement across uneven terrain, and implemented foliage fading to prevent vegetation from obstructing the camera's view.

Much of the player's interaction with the factory happens through UI, including machine configuration. I implemented machine configuration interfaces and a data-driven recipe viewer based on designs from the team's artist. The viewer builds an expandable diagram from the recipe Data Table, showing inputs, outputs, and the machines that connect them. C++ creates the item and connection widgets and recalculates the layout as branches expand, while Blueprint widgets populate the labels, icons, and quantities.

</details>

<details>
<summary><strong>Level editor and mod.io integration</strong></summary>

I developed an in-game editor that lets players create levels using existing map environments. Players can subdivide building areas, place decorations, configure spawners and receivers, and define objectives and available mechanics.

I implemented serialization of level objects and settings to JSON, along with the loading logic that reconstructs actors in the map.

The mod.io integration supports:

- Level validation before publishing through the game.
- Uploading player-created levels.
- Browsing and downloading community levels.
- Loading downloaded level data into the game.

</details>

<details>
<summary><strong>Services, persistence, and post-release support</strong></summary>

I implemented the client integration with our backend for leaderboards, player gallery and player profiles, including statistics such as levels played and total stars earned.

I integrated EasyMultiSave to persist actor positions and configuration variables, and SteamCore for achievements.

After release, I addressed bugs and implemented quality-of-life improvements over the following months.

</details>

## Chased by Darkness

A survival horror game playable solo or in online co-op with up to four players.
<img width="1920" height="1080" alt="chased" src="https://github.com/user-attachments/assets/4a9695f4-d17c-4fae-8a4a-551634fbf0e8" />
**Role:** Sole Game Programmer & Co-Founder at South Blue Moon  
**Development began:** January 2021  
**Released:** June 1, 2021  
**Major map updates:** January 2022 and May 2023  
**Technology:** Unreal Engine, primarily Blueprints, with limited C++ for menu settings

[View on Steam](https://store.steampowered.com/app/1595710/Chased_by_Darkness/)

I was responsible for gameplay programming, networking, enemy AI, UI implementation, and platform integrations, from the prototype through release and subsequent content updates. Post-release work included multiplayer bug fixes, quality-of-life improvements, and two additional maps.

### Key contributions

- Implemented cooperative gameplay replication, server-authoritative projectile damage, and shared objective tracking.
- Developed enemy AI using Behavior Trees, sight and hearing perception, and state-based pursuit.
- Adapted a community-replicated ALS locomotion base, including custom camera behavior and first-person weapon presentation.
- Implemented menus, lobby interfaces, HUD, and controller support.
- Built map-specific objectives, boss phases, and configurable difficulty systems.

<details>
<summary><strong>Multiplayer and platform integration</strong></summary>

I implemented replication for the game's interactions and combat on top of the existing replicated locomotion base.

Projectiles were spawned locally and on the server, but only server-side impacts applied damage. The host followed a separate path to avoid duplicate projectile creation. Local interfaces sent gameplay actions to the server, such as entering door codes, while shared objective progress was tracked through a replicated GameState variable.

I integrated session creation and joining using AdvancedSessions, with Steam services for the Steam version and EOS for the Epic Games Store version. I also implemented EOS-based Steam–Epic crossplay, which was later removed following player feedback about the account-linking requirement.

</details>

<details>
<summary><strong>Enemy AI</strong></summary>

I implemented enemy behavior using Behavior Trees with sight and hearing perception, including transitions between navigation and active pursuit.

Outside visual pursuit, the main enemy navigated toward sampled player positions to encourage encounters without continuously tracking players. Visual contact enabled continuous target updates; losing sight for a defined period returned the enemy to navigation toward a sampled position.

Gunfire could redirect navigation through hearing perception, with distance and priority rules preventing sound stimuli from overriding an active visual pursuit.

I also implemented difficulty scaling based on objective progress, player count, and configurable gameplay parameters.

</details>

<details>
<summary><strong>Camera, animation, and UI</strong></summary>

I adapted a community-replicated version of Advanced Locomotion System, modifying Animation Blueprints for weapon use and replacing its camera approach with a project-specific implementation.

First-person camera adjustments used view pitch and movement speed to avoid clipping into the character body. A camera-attached mesh handled first-person item and weapon presentation alongside the third-person character mesh, with arm visibility adjusted as needed.

I implemented the interaction and combat logic, integrating Ballistics FX for projectile visuals and connecting its hit handling to the game's damage logic.

I also implemented menus, lobby interfaces, HUD, and other gameplay UI following designs provided by the team's artist, and added controller support.
