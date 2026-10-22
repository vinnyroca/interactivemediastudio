---
layout: page
title: Door
permalink: /Tutorials/30_Door/
---
# Door

Now that we can add a key to our inventory, let's modify our `SceneChangeInteractable` script to be able to take a key. In this way, we can create a locked door that only opens with a key.

## Modifying Scene Change script

Let's first modify our `SceneChangeInteractable` script by adding two variables. One variable will hold a boolean value that determines if our door requires an item to enter . The second variable will store the type of item required to enter. 

```cs
public bool requiresItem;
public InventoryItem requiredItem;
```

Next, let's modify both our Interact Text and Interact methods to have two paths; one path will be for Scene Change objects that require items and the second will be for objects that do not require items:

```cs
public string InteractText()
{
    if (requiresItem)
    {

    }
    else
    {
        return "Enter " + nameOfPlaceToEnter;
    }
}

public void Interact()
{
    if (requiresItem)
    {

    }
    else
    {
        SceneManager.LoadScene(sceneToLoad);
    }
        
}
```

Within our Interact Text, let's check if our inventory has our required item. If it does, let's let the player know that they will use that item if they enter the door. If they do not have the item, let's let the player know they need that item to enter:

```cs
public string InteractText()
{
    if (requiresItem)
    {
        if (InventoryManager.instance.CheckIfItemIsInInventory(requiredItem))
        {
            return "Entering " + sceneToLoad + " will use one " + requiredItem.itemName;
        }
        else
        {
            return "You need " + requiredItem.itemName + " to enter " + sceneToLoad;
        }
    }
    else
    {
        return "Enter " + nameOfPlaceToEnter;
    }
}
```

Within our Interact method, let's also check if our inventory contains our required item. If it does, let's remove that object from our inventory and load the next scene:

```cs
public void Interact()
{
    if (requiresItem)
    {
        if (InventoryManager.instance.CheckIfItemIsInInventory(requiredItem))
        {
            InventoryManager.instance.RemoveFromInventory(requiredItem);
            SceneManager.LoadScene(sceneToLoad);
        }
    }
    else
    {
        SceneManager.LoadScene(sceneToLoad);
    }     
}
```

## Door Object

Finally, let's set our House object to require a Key:

![Image showing House object set to contain key](/Attachments/Pasted%20image%2020261021224237.png)

![Testing key with Unity](/Attachments/29_5.gif)
