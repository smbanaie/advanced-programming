# Homework 4: Game Development Framework

**Section**: 4 - Design Patterns
**Estimated Time**: 4-6 hours
**Difficulty**: Advanced
**Prerequisites**: Section 4 Tutorial & Workshop

---

## 📋 Assignment Overview

Create a comprehensive game development framework that demonstrates advanced design pattern implementation. You'll build a character system with strategy patterns, event management with observer patterns, resource handling with singletons, and level generation with factory patterns. The framework will showcase how multiple design patterns work together in complex, interactive systems.

**Learning Focus**: Real-world application of design patterns in game development

---

## 🎯 Learning Objectives

By completing this assignment, you will demonstrate the ability to:

1. **Implement Singleton Pattern**: Create resource managers and game state handlers
2. **Apply Factory Pattern**: Build flexible character and level creation systems
3. **Use Observer Pattern**: Implement event-driven game mechanics
4. **Utilize Strategy Pattern**: Create interchangeable AI and ability systems
5. **Combine Design Patterns**: Build cohesive game architecture
6. **Design Framework Architecture**: Create extensible game development tools

---

## 📋 Requirements

### Core Requirements

#### 1. Game Entity System with Factory Pattern
Create a flexible entity creation system:

**Abstract GameEntity Base Class:**
- Common properties: id, name, position, health, level
- Abstract methods: update(), render(), take_damage(), heal()
- Observer pattern integration for entity events

**Concrete Entity Classes:**
- `Player` - Player-controlled character
- `Enemy` - AI-controlled opponents
- `NPC` - Non-player characters with dialogue
- `Item` - Collectible game objects

**EntityFactory Class:**
- `create_player(name, class_type)` - Create player with different classes
- `create_enemy(enemy_type, difficulty)` - Create enemies of different types
- `create_npc(npc_type, dialogue_tree)` - Create NPCs with different behaviors
- `create_random_item(rarity)` - Create items with random properties

#### 2. AI System with Strategy Pattern
Implement interchangeable AI behaviors:

**Abstract AIStrategy Base Class:**
- `execute(entity, game_state)` - Execute AI behavior
- Strategy switching at runtime

**Concrete AI Strategies:**
- `AggressiveAI` - Attacks nearby enemies
- `DefensiveAI` - Focuses on survival and healing
- `PatrolAI` - Moves between waypoints
- `FleeAI` - Runs from threats
- `GuardAI` - Protects specific locations

**AI Context Class:**
- `set_strategy(strategy)` - Change AI behavior
- `execute_current_strategy()` - Run current AI logic

#### 3. Event System with Observer Pattern
Create a comprehensive game event system:

**Abstract GameEvent Base Class:**
- Event properties: type, timestamp, source, data
- Observer notification system

**Event Types:**
- `PlayerDeathEvent` - Player character died
- `EnemyDefeatedEvent` - Enemy was defeated
- `ItemCollectedEvent` - Player collected an item
- `LevelCompleteEvent` - Player finished a level
- `GameStateChangeEvent` - Game state transitions

**EventManager Singleton:**
- Register event listeners
- Fire events to all subscribers
- Event history and replay functionality

#### 4. Resource Management with Singleton Pattern
Implement shared resource management:

**ResourceManager Singleton:**
- Load and cache game assets (images, sounds, data)
- Provide access to shared resources
- Memory management and cleanup

**Asset Types:**
- Textures and sprites
- Sound effects and music
- Game configuration data
- Localization strings

#### 5. Level Generation with Factory Pattern
Create procedural level generation:

**Abstract LevelGenerator Base Class:**
- `generate_level(difficulty, theme)` - Generate complete level
- Different generation strategies

**Concrete Generators:**
- `DungeonGenerator` - Create dungeon layouts
- `ForestGenerator` - Generate forest environments
- `CaveGenerator` - Build cave systems
- `CityGenerator` - Construct urban environments

**LevelFactory Class:**
- `create_level(level_type, **params)` - Factory method for level creation
- Level caching and reuse

---

## 🏗️ Implementation Guide

### Step 1: Core Game Classes and Singletons

```python
from abc import ABC, abstractmethod
from dataclasses import dataclass, field
from datetime import datetime
from typing import List, Dict, Optional, Any, Tuple
from enum import Enum
import random
import threading

class GameState(Enum):
    MENU = "menu"
    PLAYING = "playing"
    PAUSED = "paused"
    GAME_OVER = "game_over"

class Rarity(Enum):
    COMMON = "common"
    UNCOMMON = "uncommon"
    RARE = "rare"
    EPIC = "epic"
    LEGENDARY = "legendary"

@dataclass
class Position:
    x: float
    y: float

    def distance_to(self, other: 'Position') -> float:
        return ((self.x - other.x) ** 2 + (self.y - other.y) ** 2) ** 0.5

@dataclass
class GameEvent:
    event_type: str
    timestamp: datetime = field(default_factory=datetime.now)
    source: Any = None
    data: Dict[str, Any] = field(default_factory=dict)

class EventManager(metaclass=SingletonMeta):
    """Singleton event manager using observer pattern"""

    def __init__(self):
        if not hasattr(self, '_initialized'):
            self._listeners = {}
            self._event_history = []
            self._initialized = True

    def subscribe(self, event_type: str, listener: callable):
        """Subscribe to an event type"""
        if event_type not in self._listeners:
            self._listeners[event_type] = []
        self._listeners[event_type].append(listener)

    def unsubscribe(self, event_type: str, listener: callable):
        """Unsubscribe from an event type"""
        if event_type in self._listeners:
            self._listeners[event_type].remove(listener)

    def emit(self, event: GameEvent):
        """Emit an event to all subscribers"""
        self._event_history.append(event)

        if event.event_type in self._listeners:
            for listener in self._listeners[event.event_type]:
                try:
                    listener(event)
                except Exception as e:
                    print(f"Event listener error: {e}")

    def get_event_history(self, event_type: str = None, limit: int = None) -> List[GameEvent]:
        """Get event history, optionally filtered"""
        history = self._event_history
        if event_type:
            history = [e for e in history if e.event_type == event_type]
        if limit:
            history = history[-limit:]
        return history

class ResourceManager(metaclass=SingletonMeta):
    """Singleton resource manager"""

    def __init__(self):
        if not hasattr(self, '_initialized'):
            self._resources = {}
            self._loaded_files = set()
            self._initialized = True

    def load_texture(self, texture_name: str) -> str:
        """Load texture (simplified - would load actual image)"""
        if texture_name not in self._loaded_files:
            # Simulate loading
            self._resources[f"texture_{texture_name}"] = f"Loaded texture: {texture_name}"
            self._loaded_files.add(texture_name)

        return self._resources[f"texture_{texture_name}"]

    def load_sound(self, sound_name: str) -> str:
        """Load sound effect"""
        if sound_name not in self._loaded_files:
            self._resources[f"sound_{sound_name}"] = f"Loaded sound: {sound_name}"
            self._loaded_files.add(sound_name)

        return self._resources[f"sound_{sound_name}"]

    def get_config(self, config_name: str) -> Dict[str, Any]:
        """Get game configuration"""
        # Mock configuration data
        configs = {
            "game_settings": {"max_health": 100, "difficulty_multiplier": 1.0},
            "enemy_stats": {"basic_enemy": {"health": 50, "damage": 10}},
            "player_classes": ["warrior", "mage", "rogue"]
        }
        return configs.get(config_name, {})

    def cleanup(self):
        """Clean up unused resources"""
        # In real implementation, would unload unused assets
        pass
```

### Step 2: Game Entity System with Factory

```python
class GameEntity(ABC):
    """Abstract base class for all game entities"""

    def __init__(self, entity_id: int, name: str, position: Position):
        self.id = entity_id
        self.name = name
        self.position = position
        self.health = 100
        self.max_health = 100
        self.level = 1
        self.is_alive = True
        self.event_manager = EventManager()

    @abstractmethod
    def update(self, delta_time: float):
        """Update entity state"""
        pass

    @abstractmethod
    def render(self) -> str:
        """Render entity representation"""
        pass

    def take_damage(self, damage: int):
        """Take damage and check for death"""
        self.health -= damage
        if self.health <= 0:
            self.health = 0
            self.is_alive = False
            self.event_manager.emit(GameEvent(
                "entity_death",
                source=self,
                data={"entity": self, "cause": "damage"}
            ))

    def heal(self, amount: int):
        """Heal the entity"""
        old_health = self.health
        self.health = min(self.max_health, self.health + amount)

        if self.health > old_health:
            self.event_manager.emit(GameEvent(
                "entity_healed",
                source=self,
                data={"entity": self, "healed": self.health - old_health}
            ))

class Player(GameEntity):
    """Player character"""

    def __init__(self, entity_id: int, name: str, position: Position, player_class: str):
        super().__init__(entity_id, name, position)
        self.player_class = player_class
        self.experience = 0
        self.inventory = []

    def update(self, delta_time: float):
        # Player-specific update logic
        pass

    def render(self) -> str:
        return f"Player: {self.name} ({self.player_class}) - HP: {self.health}/{self.max_health}"

    def gain_experience(self, exp: int):
        """Gain experience and potentially level up"""
        self.experience += exp
        if self.experience >= self.level * 100:  # Simple leveling
            self.level_up()

    def level_up(self):
        """Level up the player"""
        self.level += 1
        self.max_health += 20
        self.health = self.max_health

        self.event_manager.emit(GameEvent(
            "player_level_up",
            source=self,
            data={"new_level": self.level}
        ))

class Enemy(GameEntity):
    """Enemy character with AI"""

    def __init__(self, entity_id: int, name: str, position: Position, enemy_type: str, ai_strategy=None):
        super().__init__(entity_id, name, position)
        self.enemy_type = enemy_type
        self.ai_context = AIContext(ai_strategy or AggressiveAI())

    def update(self, delta_time: float):
        if self.is_alive:
            self.ai_context.execute_strategy(self, {})

    def render(self) -> str:
        return f"Enemy: {self.name} ({self.enemy_type}) - HP: {self.health}/{self.max_health}"

class EntityFactory:
    """Factory for creating game entities"""

    _entity_counter = 1

    @classmethod
    def _get_next_id(cls) -> int:
        entity_id = cls._entity_counter
        cls._entity_counter += 1
        return entity_id

    @staticmethod
    def create_player(name: str, player_class: str, position: Position = None) -> Player:
        """Create a player character"""
        if position is None:
            position = Position(0, 0)

        entity_id = EntityFactory._get_next_id()
        player = Player(entity_id, name, position, player_class)

        # Set class-specific stats
        if player_class == "warrior":
            player.max_health = 120
            player.health = 120
        elif player_class == "mage":
            player.max_health = 80

        return player

    @staticmethod
    def create_enemy(enemy_type: str, difficulty: int, position: Position = None) -> Enemy:
        """Create an enemy"""
        if position is None:
            position = Position(random.randint(0, 100), random.randint(0, 100))

        entity_id = EntityFactory._get_next_id()
        name = f"{enemy_type.title()} {entity_id}"

        enemy = Enemy(entity_id, name, position, enemy_type)

        # Scale difficulty
        enemy.max_health = 50 * difficulty
        enemy.health = enemy.max_health

        return enemy

    @staticmethod
    def create_random_item(rarity: Rarity) -> 'Item':
        """Create a random item based on rarity"""
        entity_id = EntityFactory._get_next_id()

        # Item names based on rarity
        names = {
            Rarity.COMMON: ["Health Potion", "Bread", "Rusty Sword"],
            Rarity.UNCOMMON: ["Mana Potion", "Leather Armor", "Iron Sword"],
            Rarity.RARE: ["Greater Health Potion", "Chain Mail", "Steel Sword"],
            Rarity.EPIC: ["Elixir of Life", "Plate Armor", "Magic Sword"],
            Rarity.LEGENDARY: ["Phoenix Potion", "Dragon Scale Armor", "Excalibur"]
        }

        name = random.choice(names[rarity])
        position = Position(random.randint(0, 100), random.randint(0, 100))

        return Item(entity_id, name, position, rarity)
```

### Step 3: AI System with Strategy Pattern

```python
class AIStrategy(ABC):
    """Abstract AI strategy"""

    @abstractmethod
    def execute(self, entity: GameEntity, game_state: Dict[str, Any]):
        """Execute AI behavior"""
        pass

class AggressiveAI(AIStrategy):
    """Aggressive AI - attacks nearby enemies"""

    def execute(self, entity: GameEntity, game_state: Dict[str, Any]):
        # Find nearby enemies and attack
        nearby_entities = game_state.get('nearby_entities', [])
        enemies = [e for e in nearby_entities if isinstance(e, Enemy) and e != entity]

        if enemies:
            target = min(enemies, key=lambda e: entity.position.distance_to(e.position))
            # Simulate attack
            damage = random.randint(5, 15)
            target.take_damage(damage)
            print(f"{entity.name} attacks {target.name} for {damage} damage")

class DefensiveAI(AIStrategy):
    """Defensive AI - heals when damaged"""

    def execute(self, entity: GameEntity, game_state: Dict[str, Any]):
        if entity.health < entity.max_health * 0.5:  # Below 50% health
            heal_amount = random.randint(10, 20)
            entity.heal(heal_amount)
            print(f"{entity.name} heals for {heal_amount} health")
        else:
            print(f"{entity.name} waits defensively")

class PatrolAI(AIStrategy):
    """Patrol AI - moves between waypoints"""

    def __init__(self):
        self.waypoints = [
            Position(0, 0),
            Position(50, 0),
            Position(50, 50),
            Position(0, 50)
        ]
        self.current_waypoint = 0

    def execute(self, entity: GameEntity, game_state: Dict[str, Any]):
        target = self.waypoints[self.current_waypoint]

        # Simple movement towards waypoint
        dx = target.x - entity.position.x
        dy = target.y - entity.position.y
        distance = (dx**2 + dy**2)**0.5

        if distance < 5:  # Close enough to waypoint
            self.current_waypoint = (self.current_waypoint + 1) % len(self.waypoints)
            print(f"{entity.name} reached waypoint, moving to next")
        else:
            # Move towards waypoint
            entity.position.x += dx / distance * 2
            entity.position.y += dy / distance * 2
            print(f"{entity.name} patrolling towards waypoint")

class AIContext:
    """Context for AI strategy pattern"""

    def __init__(self, strategy: AIStrategy):
        self._strategy = strategy

    def set_strategy(self, strategy: AIStrategy):
        """Change AI strategy"""
        self._strategy = strategy

    def execute_strategy(self, entity: GameEntity, game_state: Dict[str, Any]):
        """Execute current strategy"""
        self._strategy.execute(entity, game_state)
```

### Step 4: Item System and Level Generation

```python
class Item(GameEntity):
    """Collectible game item"""

    def __init__(self, entity_id: int, name: str, position: Position, rarity: Rarity):
        super().__init__(entity_id, name, position)
        self.rarity = rarity
        self.is_collected = False

    def update(self, delta_time: float):
        # Items don't need updates unless animated
        pass

    def render(self) -> str:
        status = "collected" if self.is_collected else "available"
        return f"Item: {self.name} ({self.rarity.value}) - {status}"

    def collect(self, collector: GameEntity):
        """Collect this item"""
        if not self.is_collected:
            self.is_collected = True
            self.event_manager.emit(GameEvent(
                "item_collected",
                source=self,
                data={"item": self, "collector": collector}
            ))

class LevelGenerator(ABC):
    """Abstract level generator"""

    @abstractmethod
    def generate_level(self, difficulty: int, theme: str) -> Dict[str, Any]:
        """Generate a complete level"""
        pass

class DungeonGenerator(LevelGenerator):
    """Generate dungeon levels"""

    def generate_level(self, difficulty: int, theme: str) -> Dict[str, Any]:
        # Generate dungeon layout
        width, height = 20 + difficulty * 5, 20 + difficulty * 5

        # Create rooms, corridors, enemies, items
        enemies = []
        items = []

        # Generate enemies based on difficulty
        num_enemies = difficulty * 2
        for _ in range(num_enemies):
            enemy = EntityFactory.create_enemy("goblin", difficulty)
            enemies.append(enemy)

        # Generate items
        num_items = difficulty
        for _ in range(num_items):
            rarity = random.choice(list(Rarity))
            item = EntityFactory.create_random_item(rarity)
            items.append(item)

        return {
            "type": "dungeon",
            "theme": theme,
            "width": width,
            "height": height,
            "enemies": enemies,
            "items": items,
            "difficulty": difficulty
        }

class LevelFactory:
    """Factory for creating levels"""

    _generators = {
        "dungeon": DungeonGenerator(),
        "forest": None,  # Would implement ForestGenerator
        "cave": None,    # Would implement CaveGenerator
    }

    @classmethod
    def create_level(cls, level_type: str, difficulty: int, theme: str = "default") -> Dict[str, Any]:
        """Create a level using appropriate generator"""
        if level_type not in cls._generators:
            raise ValueError(f"Unknown level type: {level_type}")

        generator = cls._generators[level_type]
        if generator is None:
            raise ValueError(f"Generator for {level_type} not implemented")

        return generator.generate_level(difficulty, theme)
```

### Step 5: Game Engine Integration

```python
class GameEngine:
    """Main game engine integrating all systems"""

    def __init__(self):
        self.event_manager = EventManager()
        self.resource_manager = ResourceManager()
        self.entities = []
        self.current_level = None
        self.game_state = GameState.MENU

        # Set up event listeners
        self._setup_event_listeners()

    def _setup_event_listeners(self):
        """Set up event listeners for game mechanics"""

        # Player death listener
        self.event_manager.subscribe("entity_death", self._handle_entity_death)

        # Item collection listener
        self.event_manager.subscribe("item_collected", self._handle_item_collection)

        # Level completion listener
        self.event_manager.subscribe("level_complete", self._handle_level_complete)

    def _handle_entity_death(self, event: GameEvent):
        """Handle entity death events"""
        entity = event.data["entity"]
        if isinstance(entity, Player):
            self.game_state = GameState.GAME_OVER
            print(f"Game Over! {entity.name} has died.")
        elif isinstance(entity, Enemy):
            print(f"Enemy {entity.name} defeated!")

    def _handle_item_collection(self, event: GameEvent):
        """Handle item collection events"""
        item = event.data["item"]
        collector = event.data["collector"]
        print(f"{collector.name} collected {item.name}!")

    def _handle_level_complete(self, event: GameEvent):
        """Handle level completion"""
        print("Level complete! Congratulations!")

    def start_new_game(self, player_name: str, player_class: str):
        """Start a new game"""
        self.game_state = GameState.PLAYING

        # Create player
        player = EntityFactory.create_player(player_name, player_class)
        self.entities.append(player)

        # Generate first level
        self.current_level = LevelFactory.create_level("dungeon", 1, "tutorial")
        self.entities.extend(self.current_level["enemies"])
        self.entities.extend(self.current_level["items"])

        print(f"Started new game with {player_name} the {player_class}")

    def update(self, delta_time: float):
        """Update game state"""
        if self.game_state != GameState.PLAYING:
            return

        # Update all entities
        for entity in self.entities[:]:  # Copy list to avoid modification during iteration
            if entity.is_alive:
                entity.update(delta_time)
            else:
                self.entities.remove(entity)

    def render(self) -> str:
        """Render current game state"""
        if self.game_state == GameState.MENU:
            return "Game Menu - Press Start to begin"
        elif self.game_state == GameState.GAME_OVER:
            return "Game Over - Press Restart to try again"

        # Render all entities
        output = []
        for entity in self.entities:
            if entity.is_alive:
                output.append(entity.render())

        return "\n".join(output)
```

---

## 🧪 Testing Requirements

Create a comprehensive test script that demonstrates:

```python
# test_game_framework.py
from game_framework import *

def test_game_framework():
    print("🎮 Testing Game Development Framework")
    print("=" * 50)

    # Initialize game engine
    engine = GameEngine()

    # Test singleton resource manager
    resource_manager = ResourceManager()
    texture = resource_manager.load_texture("player_sprite")
    sound = resource_manager.load_sound("sword_swing")
    config = resource_manager.get_config("game_settings")

    print(f"Loaded texture: {texture}")
    print(f"Loaded sound: {sound}")
    print(f"Game config: {config}")

    # Test entity factory
    print(f"\n🏭 Testing Entity Factory:")
    player = EntityFactory.create_player("Hero", "warrior")
    enemy = EntityFactory.create_enemy("orc", 2)
    item = EntityFactory.create_random_item(Rarity.RARE)

    print(f"Created player: {player.render()}")
    print(f"Created enemy: {enemy.render()}")
    print(f"Created item: {item.render()}")

    # Test AI strategies
    print(f"\n🤖 Testing AI Strategies:")
    enemy.ai_context.set_strategy(AggressiveAI())
    print("Enemy with Aggressive AI:")
    enemy.ai_context.execute_strategy(enemy, {"nearby_entities": [player]})

    enemy.ai_context.set_strategy(DefensiveAI())
    enemy.take_damage(40)  # Damage enemy to trigger defensive behavior
    print("Enemy with Defensive AI (after taking damage):")
    enemy.ai_context.execute_strategy(enemy, {})

    # Test level generation
    print(f"\n🏰 Testing Level Generation:")
    level = LevelFactory.create_level("dungeon", 2, "dark_crypt")
    print(f"Generated level: {level['type']} ({level['difficulty']} difficulty)")
    print(f"Enemies in level: {len(level['enemies'])}")
    print(f"Items in level: {len(level['items'])}")

    # Test event system
    print(f"\n📢 Testing Event System:")
    event_manager = EventManager()

    def test_listener(event):
        print(f"Event received: {event.event_type} - {event.data}")

    event_manager.subscribe("test_event", test_listener)
    event_manager.emit(GameEvent("test_event", data={"message": "Hello, events!"}))

    # Test game engine
    print(f"\n🎮 Testing Game Engine:")
    engine.start_new_game("TestPlayer", "mage")
    print("Initial game state:")
    print(engine.render())

    # Simulate some game updates
    print(f"\nGame updates:")
    for i in range(3):
        engine.update(1.0)
        print(f"Update {i+1}:")
        print(engine.render())

if __name__ == "__main__":
    test_game_framework()
```

---

## 📋 Submission Requirements

### Code Files
- `game_framework.py` - Main game framework implementation
- `test_game_framework.py` - Comprehensive test script
- `README.md` - Documentation with architecture diagrams and usage examples

### Documentation Requirements
- Class hierarchy diagram showing relationships between entities and patterns
- Design pattern usage explanation for each major component
- Game state flow diagram
- Performance considerations for real-time updates

### Test Results
Your test script should demonstrate:
- All design patterns working correctly together
- Entity creation and management through factories
- AI behavior switching with strategy pattern
- Event system broadcasting notifications
- Level generation creating varied content
- Game engine coordinating all systems

---

## 🎯 Evaluation Criteria

### Code Quality (40%)
- Proper design pattern implementation and integration
- Clean separation of concerns between systems
- Effective use of inheritance and polymorphism
- Comprehensive error handling

### Functionality (40%)
- All required design patterns implemented correctly
- Entity system supports different types flexibly
- AI system allows runtime behavior changes
- Event system enables loose coupling
- Level generation creates varied content

### Testing (20%)
- Comprehensive test coverage of all systems
- Pattern interactions tested thoroughly
- Edge cases and error conditions handled
- Clear test output demonstrating functionality

### Documentation (10%)
- Clear code comments and docstrings
- Architecture documentation
- Design decision explanations
- Usage examples and API documentation

---

## 🚀 Extension Challenges

### Advanced AI Systems
1. **Composite AI** - Combine multiple strategies (e.g., patrol + attack when threatened)
2. **Learning AI** - AI that adapts behavior based on player actions
3. **Group AI** - Coordinated behavior between multiple enemies
4. **Dynamic Difficulty** - AI that adjusts based on player skill level

### Enhanced Event Systems
1. **Event Filtering** - Subscribe to events with conditions
2. **Event Chaining** - Events that trigger other events
3. **Asynchronous Events** - Handle events in background threads
4. **Event Persistence** - Save/load event history

### Advanced Level Generation
1. **Biome Blending** - Smooth transitions between different level types
2. **Procedural Stories** - Generate narrative elements along with layout
3. **Dynamic Scaling** - Levels that adapt to player count
4. **Replayability** - Seed-based generation for consistent recreation

---

## 🆘 Getting Help

### Common Issues
1. **Singleton Thread Safety** - Use the provided SingletonMeta metaclass
2. **Observer Memory Leaks** - Always unsubscribe listeners when objects are destroyed
3. **Strategy State Management** - Strategies should be stateless or manage their own state
4. **Factory Complexity** - Keep factory methods focused on creation, not business logic

### Resources
- Game Programming Patterns book by Robert Nystrom
- Entity Component System (ECS) architecture
- Real-time game loop implementations
- Observer pattern in game event systems

### Debug Tips
- Add logging to event emissions to trace event flow
- Use debug rendering to visualize AI decision making
- Profile entity updates to identify performance bottlenecks
- Test each pattern in isolation before integration

---

## ✅ Success Criteria

Your game development framework is complete when:

- ✅ Singleton patterns manage shared resources effectively
- ✅ Factory patterns create entities and levels flexibly
- ✅ Observer pattern enables event-driven game mechanics
- ✅ Strategy pattern allows interchangeable AI behaviors
- ✅ All patterns work together in cohesive game architecture
- ✅ Framework is extensible for additional game features
- ✅ Test script demonstrates all functionality with expected output

---

**Estimated Completion Time**: 4-6 hours
**Difficulty Level**: Advanced
**Submission Deadline**: [Set by instructor]

Build a flexible, extensible game development framework that demonstrates mastery of design patterns in complex systems!