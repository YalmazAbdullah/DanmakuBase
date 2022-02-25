## Documentation
The scripts are available in the "Scripts" folder feel free to edit them to suit your needs, below I provide basic documentation. Note the GUI editor uses degrees but all the angles under the hood are in radians.
Classes:
Main:

Player:
  A basic player class to serve as a test dummy for aiming at. Its very rudamentary.
  methods:
    _process(delta):
      param:delta
      return:none
      Called every frame. Maps the player position to the mouse position

Default Bullet:
  A basic bullet class. It has rudamentary point on circle collision detection. I deliberatly left this very bare_bones so that you can set it as u see if in order to optamize performance and balance you needs. I would suggest inhereting and overriding if you want to expand on this class but it should be fine to make one from scratch as long as you include set_PlayerPosition() function in ur new class. Also note; collisions will not work outside addon enviorment since the player position is updated via group calls in main scene for the sake of efficency(atleast i think its better?).
  
  | Attribute     | Description |
  | ---               | --- |
  | bullet_speed      | speed at which the bullet moves|
  | bullet_life_span  | total lifespan in seconds of the bullet|
  | life_time         | current age of bullet|
  | bullet_radius     | the size of the bullet hitbox|
  | collided          | turns true when collision is detected|
  | player_position   | vector used to store player position|
 
  methods:
    _process(delta):
      param:delta between frames
      return:none
      Called every frame. Calls move,age and collision detection functions.
    move(delta):
      param:delta between frames
      return:none
      moves position of bullet
    age(delta):
      increases the age of the bullet, if the lifespan is reached it frees the bullet.
    collision_Detection(playerVec:Vector2)
      params:player position vector
      return:none
      simple point on circle collsion detection method.
     set_PlayerPosition(playerVec:Vector2)
      params:player position vector
      return:none
      updates the stored player location.
   
Abstract Emitter:
  An abstract class for emitter that the editable emitter and prefab emitter inherit from. Godot does not let me enforce setters and getters via private virable but I would strongly encourage you to use them for all attributes in the class.

  | Attribute     | Description |
  | ---               | --- |
  | bullet_adress         | stores the adress of the bullet scene this  emitter is using|
  | Bullet                | used to load and store bullet scene for quick instancing|
  | fire_rate             | fire rate of the emitter in seconds per bullet, ie: how many seconds between each shot fire. It makes it easier to write code and is easier to understand then bullets per second imo|
  | clip_size             | number of bullets fired before a reload is required| 
  | reload_time           | time in seconds that it takes to reload|
  | angular_veloctiy      | speed at which the emitter rotates|
  | angular_acceleration  | rate at which the angular velocity changes|
  | max_angular_velocity  | maximum angular velocity achived by emitter, when achived the direction flips|
  | volley_size           | number of bullets in a single volley|
  | spread_angle          | angle of spread in a single volley|
  | spread_width          | distance between each individual bullet in a volley|
  | array_count           | number of bullet arrays|
  | array_angle           | angle between each bullet array|
  | aim_enabled           | when true, disables rotation and targets emitter at player|
  | aim_pause             | wait in seconds before retargetting aim at player|
  | aim_offset            | angle offset between when aiming at player|
  | player                | stores player object for aiming |
  | controler             | stores the controller class that this emitter is child to|
  | aim_timer             | timer to keep track of pauses between aim|
  | shot_timer            | timer to keep track of wait between shots|
  | reload_timer          | timer to keep track of wait between reload|
  | shot_count            | tracks number of shots fired,resets after reload|
  
  methods:
    _process(delta):
      calls movement and bound handling functions. Also calls the aim or rotate functions based on enabled flags. When not reloading or on shot cooldown it calls the shoot function and checks magizine via clip management fucntion.
     
     _move():
      virtual function for emitter movement
     
     _bound_Handler():
      virtual function of handling going out of bounds for emitter
     
     aim(delta,player_position):
      param:delta between frames, the position of the player
      return:none
      rotates the emitter to look at player when the aim referesh is off cooldown
     
     rotate(delta):
      param:delta between frames
      return:none
      Updates the angular veloctiy with the accerate_Rotation() function. Rotates the emitter every frame based on the angular velocity.
     
     accelerate_Rotaion(delta):
      param:delta between frames
      return:float
      Increases the angular velocity based on the angular acceleration and returns the new angular velocity
     
     cooldown(delta):
      param:delta between frames
      return:boolean
      Checks to see if the cooldown between shots has been completed
     
     reload(delta):
      param:delta between frames
      return:boolean
      Checks to see if the reload wait between shots has been completed
     
     shoot():
      param:none
      return:none
      for each array the function instances a volley of bullets and adjusts their position and rotaion accordingly
      
     instance_Bullet(childBullets,angle):
      param:empty array to store a single volley of bullets,starting angle of array
      return: childBullets array
      instances a volley of bullets with the starting angle of the array and stores them childBullets array
      
     position_Bullet(childBullets,angle):
      param:empty array containing a single volley of bullets,starting angle of array
      return:childBullets array
      adjusts the starting position of a volley of bullets according the array angle and returns updated childBullets container array
     
     rotate_Bullet(childBullets,angle):
      param:empty array containing a single volley of bullets,starting angle of array
      return: childBullets array
      adjusts the starting angle of a volley of bullets according the array angle and returns updated childBullets container array
     
     clip_Managment():
      param:none
      return:none
      checks to see if clip has been emptied; forces reload when needed
     
     load_Emitter(filepath):
      param:path to file in string form
      return:none
      loads params from a saved emitter

Editable Emitter:

Prefab Emitter:

Tab: