# Rhythm-Game
A DDR based rhythm game made in Godot 4.3.

# Description
This is a rhythm game inspired by Dance-Dance Revolution, Friday Night Funkin and Fortnite Festival.
It has two songs for the game for the players to try, however, it can be hard for players to play.
The main objective is to get a highscore and hit as many notes you can.
It has a grade performance on the notes and a score function.

# Assets
<img width="234" height="216" alt="arrows" src="https://github.com/user-attachments/assets/4c951db8-fc1e-4ca4-89e4-95cf57909ce3" />
Notes.

Backgrounds.
![141c66b99732dd2b2ca688139c42f92a 500x500x1](https://github.com/user-attachments/assets/6f8d90e6-bc4d-4f1e-b32c-bbc81644d60c)
![images](https://github.com/user-attachments/assets/b44f89ad-2393-4c90-b157-6fac33680e39)

Songs (Youtube)
Level 1 - (https://youtu.be/F8NfHAe5RM4?feature=shared)
Level 2 - (https://youtu.be/SSbBvKaM6sk?feature=shared)

# Codes 

for Key Falling.

 '''extends Sprite2D

@export var fall_speed: float = 4.0

var init_y_pos: float = -360


var has_passed: bool = false
var pass_threshold = 300.0
func _init():
	set_process(false)

# Called every frame. 'delta' is the elapsed time since the previous frame.
func _process(delta):
	global_position += Vector2(0, fall_speed)
	
	
	
	if global_position.y > pass_threshold and not $Timer.is_stopped():
		print($Timer.wait_time - $Timer.time_left)
		$Timer.stop()
		has_passed = true

func Setup(target_x: float, target_frame: int ):
	global_position = Vector2(target_x, init_y_pos)
	frame = target_frame
	
	set_process(true)



func _on_destroy_timer_timeout():
	queue_free()'''

