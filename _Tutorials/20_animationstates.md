
# Animation States

In this tutorial, we will learn how to use animation states with Unity. To begin, we can think of animation states as different moments of animation that our characters or objects move between. For example, within *Asteroids* there are two animation states for the player. In one state the thrusters for the spacecraft are off, in the other, the thrusters are on. (There is also a destruction state, but we will touch on that later.) Below, you can see an animated gif of these two states in action. In this tutorial, we will implement these two animation states within our Unity project.

![Animated image of spacecraft changing animation states](/Attachments/20_1.gif)

## Importing Animations

To begin, we need to import the two following animations. One is the *idle* pose for our player and the other is the thrust pose:


![Gif of spacecraft](/Attachments/SpaceShip.gif)
<a href="/Attachments/spacecraftanimation_idle.png" download="spacecraftanimation_idle.png">Download Idle Sprite Sheet</a>

![Animation of space craft thrust](/Attachments/spacecraftanimation_thrustgif.gif)

<a href="/Attachments/spacecraftanimation_thrust.png" download="spacecraftanimation_idle.png">Download Thrust Sprite Sheet</a>



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