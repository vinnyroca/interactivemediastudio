---
layout: page
title: Persistent Data
permalink: /Tutorials/31_PersistentData/
---
## Persistent Data

One thing that we will notice as we change rooms with our key, when we leave and then reenter the main room, our key will respawn. Let's implement a system that tracks whether or not our key has been destroyed.

# Game Manager List

One way to keep track of the items we have picked up, is to store all of our interactable objects within a list in our game manager. This list, for example, could contain all of the destructible objects and pickups we have interacted with. We can then use this list to determine whether or not we want to destroy those objects when our scene loads.

Let's begin by modifying our Game Manager to contain a list of strings:

```cs
public List<string> interactables = new();
```


## Pickup Interactable

From here, we can edit our Pickup Interactable. When we pick up our game object, let's add that object to the interactable list within our game manager. To distinguish this object from other objects within our list, let's use the active scene name combined with our object name as the value we provide to the list. As a note, this will require that every object we want to add to this list has a different name:

```cs
public void Interact()
{
    if (InventoryManager.instance.CheckInventorySpaceAvailable())
    {
        InventoryManager.instance.AddToInventory(itemType);
        GameManager.instance.interactables.Add(SceneManager.GetActiveScene().name + gameObject.name);
        Destroy(gameObject);
    }
}
```

Finally, in a `Start()` method, let's check if our object is contained within this list. If it is, we have previously picked it up and can destroy the game object:

```cs
private void Start()
{
    if (GameManager.instance.interactables.Contains(SceneManager.GetActiveScene().name + gameObject.name))
    {
        Destroy(gameObject);
    }
}
```

![Image showing key being picked up](/Attachments/30_1.gif)