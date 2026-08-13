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

## Instantiating Prefabs

### Creating a Prefab

Now that we have a properly functioning projectile, lets rework our program so that we can spawn an infinite number of projectiles using a fire input button.

In order to create more projectiles, we need to continuously duplicate the single projectile we currently have. To do this, we need to turn our projectile into a Prefab. In Unity, a Prefab is essentially a game object blueprint that we can use to create exact copies from a template.

Creating a prefab is incredibly easy, the only thing we need to do is drag our game object from our Hierarchy to our Project window. For best practice, we can drag our game object in a folder named **Prefabs** to keep things organized.

![animated gif of user dragging a projectile game object to the project window to create a prefab](/Attachments/prefab_7.gif)

We'll know when our object has become a prefab when the name of the game object becomes blue and the cube outline is replaced with a solid blue cube. Once we create the prefab object, we can delete the original.

![animated gif of user delete the original projectile game object from the hierarchy](/Attachments/prefab_8.gif)

### Creating Fire Input Action

To fire our projectile we need to create and Input Action within our Spacecraft.cs file. To do this we will create a button.

Let's begin by creating a `public` `InputAction` variable called `fireAction` in our Spacecraft.cs file.

```cs
public InputAction fireAction
```

Enable the disable the fireAction

```cs
private void OnEnable()
{
    moveAction.Enable();
    rotateAction.Enable();
    fireAction.Enable();
}

private void OnDisable()
{
    moveAction.Disable();
    rotateAction.Disable();
    fireAction.Disable();
}
```

Within the Unity Editor, let's create a binding for our Fire Action. We need to set this binding to be a button. To do that, we can press the gear icon and change the **Action Type** to **Button**.

![animated gif showing adding a biding and changing it to a button](/Attachments/prefab_9.gif)

We can then set our binding to be our **Left Mouse Button**.

![Animated gif of setting Fire Action to the Left Mouse Button](/Attachments/prefab_10.gif)

### Prefab Variable

To fire our projectile from our Spacecraft script we need to create a public `GameObject` variable to store our Prefab.

```cs
public GameObject projectilePrefab;
```

We can then drag our Prefab from our Project window to the open slot in the  Spacecraft component on our Player.

![Animated gif showing a user dragging the Projectile prefab into the spacecraft component ](/Attachments/prefab_11.gif)

### triggered and Instantiate 

Lastly, to fire our projectile, within our `Update()` method, we can use `bool` value `triggered` on our `fireAction` to see if our Fire input has been pressed.

```cs
void Update()
{
    if (fireAction.triggered) 
    {
        //Fire Projectile
    }
}
```

To create our projectile we can use the `Instantiate()` method which create a new clone of the object we provide it. We can also set the position of the projectile to fired from slightly in front of our Player by using an offset variable. Additionally, we will set the rotation of our projectile to match the rotation of our Spacecraft.

```cs
public float projectileOffset = .8f;

///...

void Update()
{

    if (fireAction.triggered) 
    {
        Vector3 projectilePosition = transform.position + transform.up * projectileOffset;
        Instantiate(projectilePrefab, projectilePosition, transform.rotation);
    }
}
```

![Animated gif of projectile firing in the unity editor](/Attachments/prefab_12.gif)

Our code for our Projectile.cs and our Spacecraft.cs should look like the following

```cs
using UnityEngine;
using UnityEngine.InputSystem;

public class Spacecraft : MonoBehaviour
{
    public InputAction moveAction;
    public InputAction rotateAction;
    public InputAction fireAction;
    float moveInput;
    float rotateInput;
    Rigidbody2D rBody;
    public float movePower = 15f;
    public float rotatePower = 200f;
    public float projectileOffset = .8f;

    public GameObject projectilePrefab;

    private void OnEnable()
    {
        moveAction.Enable();
        rotateAction.Enable();
        fireAction.Enable();
    }

    private void OnDisable()
    {
        moveAction.Disable();
        rotateAction.Disable();
        fireAction.Disable();
    }
   
    void Start()
    {
        rBody = GetComponent<Rigidbody2D>();
    }
   
    void Update()
    {
        if (fireAction.triggered) 
        {
            Vector3 projectilePosition = transform.position + transform.up * projectileOffset;
            Instantiate(projectilePrefab, projectilePosition, transform.rotation);
        }
    }

    void FixedUpdate()
    {
        moveInput = moveAction.ReadValue<float>();
        rotateInput = rotateAction.ReadValue<float>();

        Vector2 moveForce = transform.up * moveInput * movePower * Time.fixedDeltaTime;
        rBody.AddForce(moveForce, ForceMode2D.Impulse);

        float newRotation = rBody.rotation + rotateInput * rotatePower * Time.fixedDeltaTime;
        rBody.MoveRotation(newRotation);
    }
}

```

```cs
using UnityEngine;

public class Projectile : MonoBehaviour
{
    Rigidbody2D rBody;
    public float speed = 15f;
    bool forcedAdded = false;
    public float lifetime = .5f;

    void Start()
    {
        rBody = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        lifetime -= Time.deltaTime; 

        if( lifetime <= 0)
        {
            Destroy(gameObject);
        }
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

