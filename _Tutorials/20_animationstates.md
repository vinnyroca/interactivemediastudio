---
layout: page
title: Animation States
permalink: /Tutorials/20_animationstates/
---

# Animation States

In this tutorial, we will learn how to use animation states with Unity. To begin, we can think of animation states as different moments of animation that our characters or objects move between. For example, within *Asteroids* there are two animation states for the player. In one state the thrusters for the spacecraft are off, in the other, the thrusters are on. (There is also a destruction state, but we will touch on that later.) Below, you can see an animated gif of these two states in action. In this tutorial, we will implement these two animation states within our Unity project.

![Animated image of spacecraft changing animation states](/Attachments/20_1.gif)

## Importing Animations

To begin, we need to import the two following animations. One is the *idle* pose for our player and the other is the thrust pose:


![Gif of spacecraft](/Attachments/SpaceShip.gif)
<a href="/Attachments/spacecraftanimation_idle.png" download="spacecraftanimation_idle.png">Download Idle Sprite Sheet</a>

![Animation of space craft thrust](/Attachments/spacecraftanimation_thrustgif.gif)

<a href="/Attachments/spacecraftanimation_thrust.png" download="spacecraftanimation_idle.png">Download Thrust Sprite Sheet</a>

Following the import setting from the previous tutorial, download these two sprite sheets, slice them up in the sprite editor, and drag and drop the frames into your scene to create two animations. You should create two animations, one name *spacecraft_idle* and the other labeled *spacecraft_thrust*

You can import these animations using the following settings:

![Image showing import settings](/Attachments/Pasted%20image%2020260916174927.png)

After importing and creating the animations, our hierarchy and art folder should look like the below image. You can see we now have a sprite, animator and animation for both the idle and thrust animations:

![Image showing art folder](/Attachments/Pasted%20image%2020260916175104.png)

![Image showing hierarchy with two animations](/Attachments/Pasted%20image%2020260916175143.png)

Let's begin by deleting a few items we don't need. We won't need the two animations within our hierarchy, so let's delete those. 

![gif of deleting animations](/Attachments/20_2.gif)

Next, we only need one animator controller for our player, so let's delete our *spacecrafteanimation_thrust* animator controller from our project folder:
 
![gif of deleting our animator](/Attachments/20_3.gif)

## Assigning Sprite to Player

The next step we want to do is to assign one of our idle sprites to our player prefab. To do this, we need to select our player prefab, and drag in one of our idle spacecraft sprites into the sprite property of the Sprite Renderer component. Note that you might have changed the scale of your original game object and we may need to set its scale to be 1 on the X, Y, and Z axes:

![Adding idle sprite to player](/Attachments/20_4.gif)


If we now play our game, we should see our player sprite on our player game object during play:

![Image showing new sprite on player](/Attachments/Pasted%20image%2020260916175952.png)

## Adding Animation to Player

To play our idle animation on our player, we first need to allow our player to play animations. To do this, we need to add an Animator component to our Player prefab.

![image of animator component highlighted on player prefab](/Attachments/Pasted%20image%2020260916180150.png)

Next, we need to make sure that our Animator has an Animator controller assigned to it. For our player, we can use the *spacecraftanimation_idle_0* animator controller we created when we made our idle animation:

![image of animator component with animator controller highlighted ](/Attachments/Pasted%20image%2020260916180448.png)

If we now play our project, we will see that our idle animation now plays.

![animated image showing the player animation playing during runtime](/Attachments/20_5.gif)

## Animators

So why is our animation playing? To understand this question, we first need to understand how animators and animations work within Unity. 

To do this, we first need to open the Animator window.

With your player prefab selected, **Navigate to Window > Animation > Animator:**

![animate image showing the opening of the animator window](/Attachments/20_6.gif)

Within our animator, we can notice a variety of boxes, each labeled with a different word. Importantly, we see a box labeled *spacecraft_idle* which matches the name of the animation we created earlier in this tutorial. What we are looking at is called *animation states*.

The animator controller, the object that was created when we made our animation, is a map that tells our Animator component on our game object which animation to play based on different conditions. 

If I play my game and look at the animator widow with the instantiated player in my scene selected, I will see a blue line below my *spacecraft_idle* animation, indicating that this animation is playing:

![image showing blue line below idle animation in animator](/Attachments/Pasted%20image%2020260916182618.png)

What is happening in this situation is that since my idle animation is directly connected to *Entry*, my idle animation is going to begin playing as soon as my game begins.

## Animation States

Using the Animator window to edit my animation controller, I can get different animations to play based on certain conditions. In our case, we want our thrust animation to play when our player is holding down the thrust button.

To achieve this, we first need to bring our thrust animation into our animator controller. There are multiple ways to do this, such as making a new state, however, most commonly, we can drag our animation into our controller. Let's drag our thrust animation into the animator controller on our player prefab:

![animated image of dragging in thrust animation into animator controller](/Attachments/20_7.gif)


## Transitions

Currently, we have the animation within our Animator controller, however, this animation will not play as it is not connected to any other state or animation. There is an option to access animations that exist within an animator controller through code, but, often times, we make to transition back and forth between different animations.

For our example, we want our player to transition from the idle animation to the thrust animation when the thrust button is pressed. To do this, there is a multi-step process:

1. Set the transitions in the animator
2. Create Parameters 
3. Set the conditions within the animator
4. Access the animator in code
5. Access the condition in code
6. Set conditions so that animations transition how we want

### Create Transitions

To create transitions within the Animator, we first need to select the state we want to transition from.

From there, we can press right click and choose **Make Transition** and then click on the state we want to transition to. This will create a line with an arrow indicating the direction the transition is moving. We can do this to make a transition from our idle state to our thrust state:

![Animated gif of making a transition.](/Attachments/20_8.gif)

We can also do this to set a transition back to our idle state when our thrust button is not pressed:

![animated image of creating transition between thrust state and idle state](/Attachments/20_9.gif)

If we were to play our game now, we would see that our thrust is constantly turning on and off. This is because our animator controller is continuously transitioning between the two states. To prevent this behavior, we need to add some conditions that control when our animation transitions.

![animated gif of thrust turning on and off](/Attachments/20_10.gif)

## Creating Animation Parameters

[Unity Reference](https://docs.unity3d.com/6000.3/Documentation/Manual/AnimationParameters.html)

To create conditions which we can use to decide when our animations transition, we first need to create parameters that control those conditions. Parameters can be made within the Animator window and can consist of the following types of parameters:

- _Integer_ - a whole number
- _Float_ - a number with a fractional part
- _Bool_ - true or false value (represented by a checkbox)
- _Trigger_ - a boolean parameter that is reset by the controller when consumed by a transition (represented by a circle button)

With these parameters, we can detect the value of each created parameter and then use that value to decide how to control our animation.

For example, with our transition from idle to thrust, we could use a variety of parameters. We could, for example, use a float or int parameter to detect the value of our input and then use this value to decide which animation will play. Or, we could use a Bool parameter that is set to true when the player is moving their character and false when the thrust button is not pressed.

In this tutorial, let's use a bool parameter that is either true or false depending on if the player is pressing the thrust button.

To do this, we first need to create a bool parameter. 
To create a parameter
- Navigate to the parameter tab in the Animator window
- Press the plus button to add a new parameter
- Select bool
- Name the parameter "thrust"

![animated image of creating a thrust parameter](/Attachments/20_11.gif)

## Setting Conditions

Once we have a parameter, we can then use this parameter to set conditions on our transitions. For this tutorial, let's use a set of conditions that allow the idle animation to transition to the thrust animation if "thrust" is true and also allow the thrust animation to transition to the idle animation if "thrust" is false.

To create these conditions, we need to select on the transition line and then navigate to the inspector panel. From there, we can press the plus button in the conditions section to add our condition.

![Animated gif of setting animation conditions](/Attachments/20_12.gif)

## Programming Animation States

Once we have our parameters and or conditions, we can then access these parameters in code to change our animation state during runtime.  

Let's write this code within our Spacecraft.cs file (or Player.cs file).

### Getting Animator

The first step in this process is getting our Animator. We can begin by creating a variable for our Animator component named `anim`:

```cs
Animator anim;
```

Next, we can get our Animator either in `Start` or `Awake`

```cs
void Start()
{
    rBody = GetComponent<Rigidbody2D>();
    sRenderer = GetComponent<SpriteRenderer>();
    //new code
    anim = GetComponent<Animator>();
}
```

## Setting Parameters

With our `anim` value now assigned. We can now get access to the parameters on our animator controller.

Within this tutorial, we want to change our bool value of "thrust" to true if our player is pressing down the thrust button. To do this, we can check if our `moveInput` value is greater than 0.

Let's first create the if and else statement that will check if our player is pressing the move button. We can do this in our `FixedUpdate` method after assigning the values of `moveInput` and `rotateInput`:

```cs
void FixedUpdate()
{
    moveInput = moveAction.ReadValue<float>();
    rotateInput = rotateAction.ReadValue<float>();

    if (moveInput > 0)
    {
        
    }
    else
    {
        
    }
```

### Setting Values

To set the value of parameter on an Animator, we can use a variety of methods

- `SetBool()` allows us to set boolean parameters
- `SetFloat()` allows us to set a float parameter
- `SetInteger()` allows us to set an integer parameter
- `SetTrigger()` allows us to set a trigger

Within our example, we want to use the `SetBool()` method on our Animator component to set the value of thrust. As a starting point, we can first call `anim.SetBool();` in both our `if` and `else` statements:

```cs
if (moveInput > 0)
{
    anim.SetBool();
}
else
{
    anim.SetBool();
}
```

With these methods we need to provide two parameters:

1. The name of the bool we want to set as a string
2. The value we want to set our bool to (either true or false)

In our case, when a player is the move button, we can set our bool parameter by using `anim.SetBool("thrust", true)`, and when the player is not pressing the move button we can transition back to our idle state by using `anim.SetBool("thrust", false)`:

```cs
if (moveInput > 0)
{
    anim.SetBool("thrust", true);
}
else
{
    anim.SetBool("thrust", false);
}
```

### Testing

![Animated gif of project showing a slight delay in transition to thrust animation when the player mvoes](/Attachments/20_13.gif)

If we play our project, we will notice that there is slight delay when we transition to our thrust animation and back. The reason for this is that Unity, by default, will transition to a new animation based on time value set with our Animator controller.

## Exit Time

By selecting one of our transitions and looking at our inspector, we can see that our transition has an exit time of .25 seconds. This exit time is giving our transition the slight delay we are seeing during play:

![Image showing exit time of a quarter second](/Attachments/Pasted%20image%2020260916193231.png)

To correct this, let's uncheck the box *Has Exit Time* as well as set our *Transition Duration* to be 0. This will give us an instant transition. Let's do this for both transitions. You can find the *Transition Duration* under the setting drop down:

![Image of inspector showing that there is no longer any exit time](/Attachments/Pasted%20image%2020260916193513.png)

As a result, now when we play our project, we will see an instant transition from idle to thrust:

![animated image of game being played showing the instant transition between idle and thrust](/Attachments/20_14.gif)


## Challange

Customize the animations within your scene: using software of your choice, create an animation for player destruction and add it to your scene. You may need to add a timer to delay the time by which the player spawns
