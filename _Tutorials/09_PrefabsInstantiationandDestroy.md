---
layout: page
title: Prefabs, Instantiation, and Destroy
permalink: /Tutorials/09_PrefabsInstantiationandDestroy/
---
# Prefabs, Instantiation, and Destroy

In this tutorial we will learn how to fire projectiles from our spacecraft. In doing so, we will learn about Prefabs as well as how to create and destroy objects during runtime.

## Creating Projectile 

To create a projectile begin by creating a Square game object. 

![animated gif showing a user in the unity editor creating a square sprite](/Attachments/prefab_1.gif)

Then rename it to **Projectile** and scale it down to around `(.05, .3, 1)`

![animated gif of a user in the unity editor renaming a square sprite to projectile and scaling it down](/Attachments/prefab_2.gif)

## Moving Projectile

To move our projectile, let's use the `AddForce()` method with a rigid body.

Begin by adding a Rigidbody2D component to your projectile and setting the Gravity Scale to 0

![animated gif of a user adding a rigidbody2d component to the projectile game object in the unity editor](/Attachments/prefab_3.gif)

Create a **Projectile** MonoBehavior script and add it to our Projectile game object

![animated gif of a user creating a projectile script and adding it to a projectile game object within the unity edit](/Attachments/prefab_4.gif)

Open the script and add the following variables:

```cs
Rigidbody2D rBody;
public float speed = 15f;
bool forcedAdded = false;
```



To move our rigid body, let's first get the component in `Start()`

```cs
void Start()
{
    rBody = GetComponent<Rigidbody2D>();
}
```

Then add a force to our rigid body in the up direction of the projectile's transform

```cs
void FixedUpdate()
{
    rBody.AddForce(transform.up * speed, ForceMode2D.Impulse);
}
```

In order to trigger the `AddForce()` method only once, we will use a `Boolean` flag during the loop of `FixedUpdate()`. After adding an initial force we will set the `bool` variable `forceAdded` to `true` so no additional force will be added to our projectile.

```cs
void FixedUpdate()
{
    if(forcedAdded == false)
    {
        rBody.AddForce(transform.up * speed, ForceMode2D.Impulse);
        forcedAdded = true;
    }
}
```

Lastly, add the ScreenWrap component to the Projectile.

The Projectile game object and Projectile.cs file should look like this:

![Screen shot of the unity interface showing the projectile game object in the inspector](/Attachments/Pasted%20image%2020260806123313.png)

```cs
using UnityEngine;

public class Projectile : MonoBehaviour
{
    Rigidbody2D rBody;
    public float speed = 15f;
    bool forcedAdded = false;

    void Start()
    {
        rBody = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        
    }

    void FixedUpdate()
    {
        if (forcedAdded == false)
        {
            rBody.AddForce(transform.up * speed, ForceMode2D.Impulse);
            forcedAdded = true;
        }
    }
}
```

Playing our game, we notice our projectile moves indefinitely:

![Animated gif showing a projectile moving in the Unity editor](/Attachments/prefab_5.gif)

## Lifetime Timer and Destroy

 Currently, our projectile continues to move across our screen. Ideally, our projectile would move for a certain amount of time and then disappear. To accomplish this we can combine a timer with Unity's `Destroy()` method ([Unity Reference](https://docs.unity3d.com/6000.3/Documentation/ScriptReference/Object.Destroy.html)).

To create a timer in Unity, we can use a variable that holds the time we have remaining, and then subtract from this time each frame. We can use `Time.deltaTime` to subtract the time from the update loop.

```cs
public float lifetime = .5f;

void Update()
{
    lifetime -= Time.deltaTime; 
}
```

Next, we can check when our `lifetime` is less than or equal to 0.

```cs
void Update()
{
    lifetime -= Time.deltaTime; 

    if( lifetime <= 0)
    {

    }
}
```

If it is less than 0, we can use the `Destroy()` method. We can give a number of objects to `Destroy()` including components and game objects. In our case, we want to destroy the entire projectile, so we can give the method `gameObject` which will destroy the game object the component is attached to.

```cs
void Update()
{
    lifetime -= Time.deltaTime; 

    if( lifetime <= 0)
    {
            Destroy(gameObject);
    }
}
```

Now the projectile will be destroyed after half a second

![animated gif showing a launched projectile being destroyed after half a second](/Attachments/prefab_6.gif)