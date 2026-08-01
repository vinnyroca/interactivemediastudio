---
layout: page
title: Physics Motion
permalink: /09_Physics/
---
# Physics Motion

## Physics versus Transform

So far within our project, we have moving our object using the Transform component. While this method of moving objects is helpful when we want to move objects to precise locations, it can be difficult to work with collisions and object interaction. There are many methods for handling object collision within Unity, however, the built in physics engine is the most approachable and contains all the code we need for manipulating objects based on a shared physics system.

Since most of *Asteroids* involves objects colliding with each other, it is a good idea to shift the movement of our objects to use the physics system.

## RigidBody2D

To tell Unity we want an object to be affected by the physics system, we must make that object a rigid body. A rigid body in a solid, non-deformable object that can be simulated with various forces such as gravity. To make objects rigid bodies with 2D Unity, we need to add a Rigidbody2D component to our game object.

Let's begin by adding a rigid body to our asteroid.

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

You'll notice that when we play our scene, our asteroid falls as though affected by gravity. This is because, by default, Unity simulates gravity along the Y axis. Since asteroids is top down, we can turn off gravity on our rigid body by setting our **Gravity Scale** property to **0**.

## Fixed Update and MovePosition









