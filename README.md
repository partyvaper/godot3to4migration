# Godot 3 to 4 Migration
Godot 3.x to Godot 4.x migration guide

## Useful resources

* [Godot News - Godot 4 Multiplayer changes 2 (GDScript changes)](https://godotengine.org/article/multiplayer-changes-godot-4-0-report-2)
* [Godot News - Godot 4 Multiplayer changes 1](https://godotengine.org/article/multiplayer-changes-godot-4-0-report-1)
* [Godot News - Godot 4 GDScript updates](https://godotengine.org/article/gdscript-progress-report-feature-complete-40)

* [godot4migrator.py](https://github.com/V-Sekai/gd3to4/blob/main/godot4migrator.py) - Python script that automatically does code renames from Godot 3 to Godot 4.
* [convert-to-godot4.sh](https://gist.github.com/aaronfranke/79b424226475d277d0035b7835b09c5f) - Shell script that does the same thing.

## GDScript

TODO: Add more code examples.

### Tool

Godot 3:
```gdscript
tool
extends Spatial
```

Godot 4:
```gdscript
@tool
extends Node3D
```

### Exports, Setters/Getters (TODO: Untested)

Godot 3:
```gdscript
export(float, 0.1, 60.0, 0.1) var wait_time: float = 5.0
export(float, 0.1, 50.0, 0.1) var spawn_radius: float = 20.0 setget set_spawn_radius, get_spawn_radius

func set_spawn_radius(_spawn_radius: float) -> void:
	if (spawn_radius == _spawn_radius):
		return
	spawn_radius = _spawn_radius

func get_spawn_radius() -> float:
	return spawn_radius
```

Godot 4:
```gdscript
@export_range(0.1, 60.0, 0.1) var wait_time: float = 5.0

var _spawn_radius: float = 20.0
@export_range(0.1, 50.0, 0.1) var spawn_radius: float:
	set(val): set_spawn_radius(val)
	get: return _spawn_radius

func set_spawn_radius(val: float) -> void:
	if (_spawn_radius == val):
		return
	_spawn_radius = val
```

### Kinematic body motion velocity (TODO?)

Godot 3:
```gdscript
extends KinematicBody

var velocity: Vector3 = Vector3(12, 15)

func _process(_delta: float) -> void:
  var moving = move_and_slide_with_snap(velocity, Vector3.DOWN, Vector3.UP, true, 3, deg2rad(45.0), false)
```

Godot 4:
```gdscript
extends CharacterBody3D

func _process(_delta: float) -> void:
  motion_velocity = Vector3(12, 15)
  move_and_slide()
```

### Global methods

Godot 3
```gdscript
rand_range(1, 5)
rand_range(1.0, 5.0)
```

Godot 4:
```gdscript
randi_range(1, 5)
randf_range(1.0, 5.0)
```

## Credits

* [V-Sekai](https://github.com/V-Sekai)
* [aaronfranke](https://gist.github.com/aaronfranke)
* [godotengine/godot.git](https://github.com/godotengine/godot)
* [godotengine.org](https://godotengine.org)
