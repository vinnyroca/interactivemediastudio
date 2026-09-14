

If we drag our asteroid image into our scene, we will notice that image does not look quite right. The image is a bit too large and it is also blurry along the pixel edges. To fix this, we need to adjust a few setting within our import settings.



Importing Art Assets
Sprites
Explination of what sprites are
Explination of import size
explination of single sprite
how to swap sprites

Different import setting for different types of images
Show result of compression with pixel art/blurry pixels

Animation

Animation

How to create an animation for 2d

Some Tools

Photoshop
Aesprite
Clip Studio
Procreate
Piskel

Show example of sprite sheet in piskel
Show example of spirte sheet in photo shop

Within Unity show multiple sprites and how to split sprite sheet.
Check box for animation


Playing a one animation using instationation and destory
Drag animation frames into scene
This creates an animation as well an animator
explain difference between the two.
play scene to show animation
turn off looping to show animation stops

Make a prefab then, within code, instantiate the destroy the game object.

## Animation States

Could write some code to swap the sprite, but lets instead use an idle boiling animation and a fire animation.

slice the sprite sheet and bring it into unity.
Create two animation.

Add boil animation to the player. by adding an animator and assigning the spacecraft animator

Change animation state, look at animator
Show transitions

transition too and transition back

show that we can set conditions

need to create conditions

explain the different conditions

make a bool condition

set to true and false

within code get the animator component, then, we can use the setbool method to set to true and false

Play notice delay

that is becuase of exit time, we dont want a transtion we want instance, we can check off then set to 0

Watch as animation plays normally.

Challange, customize the animations within your sceen, using software of your choice, create an animation for player destruction and add it to your scene. You may need to add a timer to delay the time by which the player spawns