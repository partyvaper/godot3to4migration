# Godot 3 to 4 Migration

Godot 3.x to Godot 4.x gdscript code migration guide

Everyone is free to add pull requests and improvements!

## Useful resources

* [Godot News - Godot 4 Multiplayer changes 2 (GDScript changes)](https://godotengine.org/article/multiplayer-changes-godot-4-0-report-2)
* [Godot News - Godot 4 Multiplayer changes 1](https://godotengine.org/article/multiplayer-changes-godot-4-0-report-1)
* [Godot News - Godot 4 GDScript updates](https://godotengine.org/article/gdscript-progress-report-feature-complete-40)

* [godot4migrator.py](https://github.com/V-Sekai/gd3to4/blob/main/godot4migrator.py) - Python script that automatically does code renames from Godot 3 to Godot 4.
* [convert-to-godot4.sh](https://gist.github.com/aaronfranke/79b424226475d277d0035b7835b09c5f) - Shell script that does the same thing.

## GDScript

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

### const Dictionary

Godot 3
```gdscript
const meshes: Dictionary = {
	rocks = [
		'res://assets/Rock1.tres',
		'res://assets/Rock2.tres',
	],
	bushes = [
		'res://assets/Bush1.tres',
		'res://assets/Bush2.tres',
	],
}
```

Godot 4
```gdscript
var meshes: Dictionary = {
	rocks = [
		'res://assets/Rock1.tres',
		'res://assets/Rock2.tres',
	],
	bushes = [
		'res://assets/Bush1.tres',
		'res://assets/Bush2.tres',
	],
}
```

### Timer Signal TODO

Godot 3
```gdscript
var timer = Timer.new()
timer.wait_time = 5.0
timer.autostart = true
timer.connect('timeout', $AnimationPlayer, 'play', ['despawn'])
```

Godot 4
```gdscript
var timer = Timer.new()
timer.wait_time = 5.0
timer.autostart = true
timer.connect('timeout', Callable($AnimationPlayer, 'play'), ['despawn'])
```

### export string dropdown TODO

Godot 3
```gdscript
export(String, 'backpack', 'book', 'radio', 'camera') var object_type: String = 'backpack'
```

Godot 4
```gdscript
@export ???
```

## Find bound key by action name

Godot 3
```gdscript
var name = 'key_attack'
var key = ''

for action in InputMap.get_action_list(name):
	if action is InputEventKey:
		key = OS.get_scancode_string(action.scancode)
	elif action is InputEventJoypadButton:
		key = Input.get_joy_button_string(action.button_index)
	elif action is InputEventMouseButton:
		key = 'Left Click' if action.button_index == BUTTON_LEFT else 'Right Click'
	else:
		key = '???'

print(key) # Will print "F" or "Left Click" or "Xbox A, PS X, etc."
```

Godot 4
```gdscript
var name = 'key_attack'
var key = ''

for action in InputMap.get_action_list(name):
	key = action.as_text()

print(key) # Will print "F" or "Left Click" or "Xbox A, PS X, etc."
```

### Global methods

Godot 3
```gdscript
rand_range(1, 5)
rand_range(1.0, 5.0)
Engine.editor_hint
OS.get_ticks_msec()
OS.get_scancode_string()
InputMap.get_action_list('action')
(action as InputEventKey).scancode
($Panel as Panel).get_stylebox('panel')
get_viewport().get_camera()
```

Godot 4:
```gdscript
randi_range(1, 5)
randf_range(1.0, 5.0)
Engine.is_editor_hint()
Time.get_ticks_msec()
OS.get_keycode_string()
InputMap.action_get_events('action')
(action as InputEventKey).keycode
($Panel as Panel).get_theme_stylebox('panel')
get_viewport().get_camera_3d() # or get_camera_2d()
```

## Credits

* [V-Sekai](https://github.com/V-Sekai)
* [aaronfranke](https://gist.github.com/aaronfranke)
* [godotengine/godot.git](https://github.com/godotengine/godot)
* [godotengine.org](https://godotengine.org)
