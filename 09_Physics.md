---
layout: page
title: Physics Motion
permalink: /09_Physics/
---
# Physics Motion

## Physics versus Transform

So far within our project, we have moving our object using the Transform component. While this method of moving objects is helpful precise locations, it can be difficult to work with collisions and object interactions. There are many methods for handling object collision within Unity, however, the built in physics engine is the most approachable and contains all the code we need for manipulating objects based on a shared physics system.

Since many of the interactions within *Asteroids* involves objects colliding with each other, it is a good idea to shift the movement of our asteroids to use the physics system.

## RigidBody2D

[Unity Reference](https://docs.unity3d.com/6000.3/Documentation/Manual/2d-physics/rigidbody/introduction-to-rigidbody-2d.html)

To tell Unity we want an object to be affected by the physics system, we must make that object a rigid body. A rigid body in a solid, non-deformable object that can be simulated with various forces such as gravity. To make objects rigid bodies with 2D Unity, we need to add a Rigidbody2D component to our game object.

Let's begin by deleting any duplicated asteroids and adding a rigid body to our one remaining asteroid.

![Animated gif of a screen recording of unity in which a user is adding a rigidbody2d component to a game object named asteroid](../Attachments/9_1.gif)

Next lets edit our `Asteroid` code and remove our movement code. Your code should look like this to start:

```cs
using UnityEngine;

public class Asteroid : MonoBehaviour
{
    public float speed = 1f;

    void Start()
    {

    }

    void Update()
    {

    }
}
```

You'll notice that when we play our scene, our asteroid falls as though affected by gravity. 

![Gif recording of an white circle asteroid following down the y axis of the screen as a result of gravity](../Attachments/9_2.gif)

By default, Unity simulates gravity along the Y axis. Since Asteroids is from a top down, we can turn off gravity on our rigid body by setting our **Gravity Scale** property to **0**.

![Screen shot of the unity user interface showing that the gravity scale of a rigidbody2d component is set to 0](/Attachments/Pasted%20image%2020260803143210.png)

## Moving Physics Objects

Now that we have converted our asteroid into a physics objects, we need to be able to move our object around the screen. There are many ways to move rigid body objects using the unity physics system. Two such examples are the [AddForce()](https://docs.unity3d.com/6000.3/Documentation/ScriptReference/Rigidbody2D.AddForce.html) and [MovePosition()](https://docs.unity3d.com/6000.3/Documentation/ScriptReference/Rigidbody2D.MovePosition.html) methods. To move our asteroid rigid body, we are going to to use the `MovePosition()` method which moves our rigid body to a new position by approximating the linear velocity require to move the rigid body to the new position. 

To move our asteroid we will need the following information:

- The asteroid's Rigidbody2D
- The direction we want to move the asteroid
- The speed we want the asteroid to move

We already have a variable for speed, so we will need to create two variables to hold the asteroid's Rigidbody2D and direction. Just like with our Transform component, we can create a `Rigidbody2D` variable to get and store our component.

``` cs
public class Asteroid : MonoBehaviour
{
    Rigidbody2D rigidbody;  
...
```

Next, we can make sure to get our Rigidbody2D component in our `Start()` function.

```cs
private void Start()
{
    rigidbody = GetComponent<Rigidbody2D>();
}
```

Next, we'll need to create a variable for the direction we want our asteroid to move. Unlike the `Translate()` method, `MovePosition()`, when using the 2D rigid body, moves our object using a `Vector2` variable.  Let's also assign our variable a default starting direction of right by using `Vector2.right` to test the movement of our object.

```cs
public class Asteroid : MonoBehaviour
{
    Rigidbody2D rigidbody;
    Vector2 direction = Vector2.right; //equals (1,0);
...
```








