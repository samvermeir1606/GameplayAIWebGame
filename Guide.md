# Zombie Survival AI Sandbox API Guide [v1.3.0]

Welcome to the Sandbox code interface. When accessing the runtime panel via the **"Edit Agent AI Logic"** overlay window, your JavaScript code runs continuously inside the main simulation loop. The environment automatically hands you an orchestration handle named `api`.

Use this guide to inspect available properties, fetch vision parameters, and build your custom Behavior Tree leaf evaluations.

---

## 👁️ 1. Environmental Perception (Read-only)

Perception structures pass mathematical data wrappers as safe, read-only structures.

### `api.getAgentStats()`
Returns the vital vectors and condition trackers of your hero agent.
* **Return Format:**
  ```json
  {
    "health": 10.00,
    "energy": 10.00,
    "stamina": 10.00,
    "pos": { "x": 325.0, "y": 300.0 } // Vector object copy
  }
api.getInventory()
Returns an array containing exactly 5 items tracking stored items. Empty positions return null.

Return Format: [Item, null, Item, null, null]

Item Properties Object:

name: String label representation (e.g., "PISTOL (DPS: 2.4, AMMO: 6)" or "FOOD (7)").

type: Categorized tag string: "gun", "food", or "medkit".

api.getCurrentlyVisibleHouses()
Returns a structural map list containing geometry coordinates of houses crossing your raycasted field-of-view cone.

Return Format:

JavaScript
[
  {
    "id": 2,
    "center": { "x": 950.0, "y": 275.0 }, // Vector2 format
    "entrances": [
      { "x": 800.0, "y": 272.5 }          // Vector2 doorway nodes
    ]
  }
]
api.getVisibleZombies() & api.getVisibleItems()
Returns an array of entities currently crossing inside your raycasted sight boundaries.

Zombie Entity Properties: { health: 35, pos: Vector2 }

Item Entity Properties: { pos: Vector2, meta: { name: String, type: String } }

🎯 2. Actuator Controls (Commands)
Invoking an actuator action modifies your hero's acceleration forces or asset structures for the current frame iteration.

api.wander()

Applies organic pathfinding exploration steering forces.

api.seek(targetVector)

Decelerates towards a target position using steering arrivals to eliminate overshooting.

api.flee(targetVector)

Toggles sprinting speed modifications and directs movement vectors cleanly away from a target coordinate.

api.shootAt(zombieEntity)

Halts movement speed temporarily, aims heading parameters at the zombie vector, and spawns bullet intersection vectors.

api.useItem(slotIndex)

Consumes food or medical supplies at the index position to restore vital properties.

api.dropItem(slotIndex)

Clears an item asset container allocation slot.
