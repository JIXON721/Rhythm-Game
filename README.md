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

    extends Sprite2D

    @export var fall_speed: float = 4.0

    var init_y_pos: float = -360


    var has_passed: bool = false
    var pass_threshold = 300.0
    func _init():
	set_process(false)

    Called every frame. 'delta' is the elapsed time since the previous frame.
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
	queue_free()

 Code for Key Listening

    extends Sprite2D

    @onready var falling_key = preload("res://objects/falling_key.tscn")
    @onready var score_text = preload("res://objects/score_press_text.tscn")
    @export var key_name: String = ""

    var falling_key_queue = []


    var perfect_press_threshold: float = 30
    var great_press_threshold: float = 50
    var good_press_threshold: float = 60
    var ok_press_threshold: float = 80


    var perfect_press_score: float  = 350
    var great_press_score: float  = 200
    var good_press_score: float  = 100
    var ok_press_score: float = 50



    Called every frame. 'delta' is the elapsed time since the previous frame.
    func _process(delta):
	
	if falling_key_queue.size() > 0:
		if falling_key_queue.front().has_passed:
			falling_key_queue.pop_front()
			
			
			
		if Input.is_action_just_pressed(key_name):
			var key_to_pop = falling_key_queue.pop_front()
			
			var distance_from_pass = abs(key_to_pop.pass_threshold - key_to_pop.global_position.y)
			
			var press_score_text: String = ""
			if distance_from_pass < perfect_press_threshold:
				Signals.IncrementScore.emit(perfect_press_score)
			elif distance_from_pass < great_press_threshold:
				Signals.IncrementScore.emit(great_press_score)
			elif distance_from_pass < good_press_threshold:
				Signals.IncrementScore.emit(good_press_score)
			elif distance_from_pass < ok_press_threshold:
				Signals.IncrementScore.emit(ok_press_score)
			else:
				# MISS
				pass
			
			key_to_pop.queue_free()
			
			var st_inst = score_text.instantiate()
			get_tree().get_root().call_deferred("add_child", st_inst)
			st_inst.global_position = global_position
	
	
	
	
	
	
    func CreateFallingKey():
		var fk_inst = falling_key.instantiate()
		get_tree().get_root().call_deferred("add_child", fk_inst)
		fk_inst.Setup(position.x, frame + 4)
		
		falling_key_queue.push_back(fk_inst)
	


    func _on_random_spawn_timer_timeout():
	CreateFallingKey()
	$RandomSpawnTimer.wait_time = randf_range(0.4, 3)
	$RandomSpawnTimer.start()

 Code for GameUI

    extends Control

    var score: int = 0

    Called when the node enters the scene tree for the first time.
    func _ready():
	Signals.IncrementScore.connect(IncrementScore)

    func IncrementScore(incr: int):
	score += incr
	%ScoreLabel.text = str(score) + " pts"

# Videos of the Gameplay
![image](https://github.com/user-attachments/assets/ff595044-fb32-44b1-8a22-5d12c52e46d5)


