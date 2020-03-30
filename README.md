# E09a-First-Person-Character

The purpose of this exercise is for you to create a 3D first-person character in Godot. As this is your first 3D project in Godot, we will start slowly, giving you an opportunity to become accustomed to the environment and some of the concepts associate with building a 3D game.

This exercise is loosely based on the Godot 101: Intro to 3D tutorial by KidsCanCode. The YouTube video is [available here](https://www.youtube.com/watch?v=_55ktNdarxY) and a similar tutorial is [written out here](http://kidscancode.org/godot_recipes/basics/3d/101_3d_07/). Please follow the directions I provide in this exercise, but if you get stuck or need additional context, feel free to consult the KidsCanCode tutorial.

Please Fork and Clone this repository to your computer. In the folder that is created, locate the project.godot file and open it in Godot.

In Game.tscn, I have provided a starting place for the exercise: the scene contains a parent Spatial node (named Game) and a StaticBody Ground node (containing a MeshInstance Plane and a CollisionShape)

Right-click on the Game node, and Add Child Node. Choose KinematicBody (not KinematicBody2D!). Name the new node Player

Right-click on the Player node, and Add Child Node. Choose CollisionShape. Name the new node Body

Right-click on the Player node again, and Add Child Node. Choose CollisionShape. Name the new node Feet

Right-click on the Player node again, and Add Child Node. Choose Spatial. Name the new node Pivot

Right-click on the Pivot node, and Add Child Node. Choose Camera.

Select the Body node. In the Inspector, select a new shape: New CapsuleShape. Edit the Shape; set the Radius to 0.5 and make sure the Height is 1. Go back to the Inspector for the Body node, and open Spatial->Transform. Set Translation->y to 1.1 and Rotation Degrees->x to 90

Select the Feet node. In the Inspector, select a new shape: New BoxShape. Edit the Shape: set the extents to x:0.4, y:0.1, z:0.4. Go back to the Inspector for the Foot node, and open Spatial->Transform. Set Translation->y to 0.2

Select the Pivot node. In the Inspector, Spatial->Transform, set Translation->y to 1.6

Select the Camera node. In the Inspector, set Current to On

Run the project, and make sure everything looks correct. Your perspective should be about two meters above a (relatively) small blue rectangle.

In the Project menu->Project Settings, select the Input Map tab. As previously, set up WASD keyboard controls to the following mappings: forward, left, back, right. Close Project Settings

Right-click on the Player node, and Attach Script. Change the Template to Empty, and set the Path to res://Scripts/Player.gd

In the resulting Player.gd script, type the following:

```
extends KinematicBody

onready var Camera = $Pivot/Camera

var gravity = -30
var max_speed = 8
var mouse_sensitivity = 0.002
var mouse_range = 1.2

var velocity = Vector3()

func get_input():
	var input_dir = Vector3()
	if Input.is_action_pressed("forward"):
		input_dir += -Camera.global_transform.basis.z
	if Input.is_action_pressed("back"):
		input_dir += Camera.global_transform.basis.z
	if Input.is_action_pressed("left"):
		input_dir += -Camera.global_transform.basis.x
	if Input.is_action_pressed("right"):
		input_dir += Camera.global_transform.basis.x
	input_dir = input_dir.normalized()
	return input_dir

func _unhandled_input(event):
	if event is InputEventMouseMotion:
		$Pivot.rotate_x(-event.relative.y * mouse_sensitivity)
		rotate_y(-event.relative.x * mouse_sensitivity)
		$Pivot.rotation.x = clamp($Pivot.rotation.x, -mouse_range, mouse_range)

func _physics_process(delta):
	velocity.y += gravity * delta
	var desired_velocity = get_input() * max_speed
	
	velocity.x = desired_velocity.x
	velocity.z = desired_velocity.z
	velocity = move_and_slide(velocity, Vector3.UP, true)

```
(make sure to indent using tabs instead of spaces)

If you run the game again, you should now be able to move forward, back, strafe left and right, and look around using the mouse. If you leave the platform, you should fall.

Finally, we will add a weapon. Right click on the Pivot node, and Add Child Node. Choose MeshInstance. Name the new node Gun. Select the Gun node.

In the Assets folder (in the FileSystem panel), you should see shotgun.obj. Drag that file to the Mesh property in the Inspector

Under Spatial->Transform, set Translation->x: 0.2, y: -0.15, z: -0.3. Set Rotation Degrees->x: 180. Set Scale->x: 8, y: 8, z: 8

If you run the game again, you should now be holding a blue gun.

Save and commit your changes, Push the repository back to GitHub, and turn in a link to your repository on Canvas.
