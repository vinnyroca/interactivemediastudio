---
layout: page
title: Inventory
permalink: /Tutorials/29_Inventory/
---
# Inventory

In this tutorial, we will create an Inventory Manager script that will control the logic of what is added or removed from our inventory.

## Creating Script

Begin by creating an `InventoryManager` script within your Scripts folder and implementing the singleton pattern within the script:

```cs
using UnityEngine;

public class InventoryManager : MonoBehaviour
{
    public static InventoryManager instance;
    void Awake()
    {
        if (instance != null && instance != this)
        {
            Destroy(gameObject);
        }
        else
        {
            instance = this;
        }
    }
}

```

## Adding to Prefab

Next, create and InventoryManager object that has your `InventoryManager` script and add it to your `GameManager` prefab. Remember, if you are adding it to your scene prefab, make sure to apply all the overrides.

## Inventory Variables

Let's create some variables to hold items within our inventory as well as limit the amount of items within our Inventory:

```cs
public List<InventoryItem> inventoryItems = new();
public int inventoryLimit = 6;
```

Notice that our inventory list will store objects of type `InventoryItem`.

## Inventory Methods

Within our script let's make two methods to add and remove items from our inventory. These methods can take in an `InventoryItem` name `item`

1. `AddToInventory(InventoryItem item)`
2. `RemoveFromInventory(InventoryItem item)`

```cs
public void AddToInventory(InventoryItem item)
{
    inventoryItems.Add(item);
}

public void RemoveFromInventory(InventoryItem item)
{
    inventoryItems.Remove(item);
}
```

Next, let's make two methods that return a boolean value. One will check if our inventory is full. The other will check if an item is currently within our inventory:

1. `bool CheckInventorySpaceAvailable()`
2. `bool CheckIfItemIsInInventory(InventoryItem item)`

```cs
public bool CheckInventorySpaceAvailable()
{
    if (inventoryItems.Count == inventoryLimit)
    {
        return false;
    }
    else
    {
        return true;
    }
}

public bool CheckIfItemIsInInventory(InventoryItem item)
{
    if (inventoryItems.Contains(item))
    {
        return true;
    }
    else
    {
        return false;
    }
}
```

## Inventory UI

Since, we are using existing UI spaces to hold our inventory images. Let's create some variables to be able to refresh our UI.

First, let's create a list of type `Image` to hold our UI images that we can use to show what is in our inventory. You will need to import the `UnityEngine.UI` namespace to get access to the `Image` class:

```cs
public List<Image> inventoryImages = new();
```

Within our project, let's drag all our inventory images into this list and make sure to override our prefab after we make our changes:

![Adding Inventory images to list](/Attachments/29_1.gif)

## Pickup Object

Before updating our user interface, let's make sure our Inventory Manager is working. To do this, we will need an item in our scene that we can pickup and add to our inventory.

Let's begin by creating a new script named `PickupInteractable`. Within this script, let's implement the `IInteractable` interface:

```cs
using UnityEngine;
using UnityEngine.SceneManagement;

public class PickupInteractable : MonoBehaviour, IInteractable
{
    public string InteractText()
    {
        
    }

    public void Interact()
    {

    }
}
```

Additionally, let's create a variable to store our Inventory Item definition:

```cs
public InventoryItem itemType;
```

## Interact

For our interaction, let's see if we have space available in our inventory, if we do, let's add the item to our inventory. If we can add the item to our inventory, let's destroy our game object:

```cs
public void Interact()
{
    if (InventoryManager.instance.CheckInventorySpaceAvailable())
    {
        InventoryManager.instance.AddToInventory(itemType);
        Destroy(gameObject);
    }
}
```

For our InteractText , let's check if we have space in our inventory. If we have space, let's let the player know they can pick up the item. If they do not have space, let's let them know their inventory is full:

```cs
public string InteractText()
{
    if (InventoryManager.instance.CheckInventorySpaceAvailable())
    {
        return "Pickup " + itemType.itemName;
    }
    else
    {
        return "Full Inventory: Cannot pick up " + itemType.itemName;
    }
}
```

In the end, our pickup script should look like this:

```cs
using UnityEngine;
using UnityEngine.SceneManagement;

public class PickupInteractable : MonoBehaviour, IInteractable
{
    public InventoryItem itemType;
    public string InteractText()
    {
        if (InventoryManager.instance.CheckInventorySpaceAvailable())
        {
            return "Pickup " + itemType.itemName;
        }
        else
        {
            return "Full Inventory: Cannot pick up " + itemType.itemName;
        }
    }

    public void Interact()
    {
        if (InventoryManager.instance.CheckInventorySpaceAvailable())
        {
            InventoryManager.instance.AddToInventory(itemType);
            Destroy(gameObject);
        }
    }
}
```

## Pickup Prefab

Let's add this `PickupInteractable` script to our Key prefab. Let's also make sure the `itemType` is set to our Key scriptable object. When completed, drag a key into your scene:

![Image showing creating key prefab](/Attachments/29_2.gif)

Now we can test that when we click on our Key, it is destroyed and added to the Inventory Manager list:

![image showing character picking up key](/Attachments/29_3.gif)

## Refreshing UI

Now that we know we can pick up our key and store it in our inventory, let's write some code to update our UI to show our key.

To do this, we will need to refresh our UI every time we add or remove items from our inventory. Let's begin by creating a method in our InventoryManager that refreshes our UI. In this script, let's iterate through our Inventory Items and see if there is anything in the list. If there is, lets assign the corresponding image in the image list the sprite from our inventory item. If there is no item, we can set the sprite to null:

```cs
void RefreshInventory()
{
    for (int i = 0; i < inventoryLimit; i++)
    {
        if (i < inventoryItems.Count)
        {
            inventoryImages[i].sprite = inventoryItems[i].uiIcon;
        }
        else
        {
            inventoryImages[i].sprite = null;
        }
    }
}
```

Finally, let's call refresh when we add or remove items from our inventory:

```cs
public void AddToInventory(InventoryItem item)
{
    inventoryItems.Add(item);
    RefreshInventory();
}

public void RemoveFromInventory(InventoryItem item)
{
    inventoryItems.Remove(item);
    RefreshInventory();
}
```

When picking up an item, we'll notice that our sprite does not show up. To correct this, let's create a variable that holds some colors. We can create a variable that holds a completely opaque white image for when we want to show an icon, as well as a completely transparent color for when there is no image in the inventory slot:

```cs
Color emptyColor = new Color(0, 0, 0, 0);
Color itemColor = new Color(1, 1, 1, 1);
```

Next, let's assign the colors when we refresh our UI:

```cs
void RefreshInventory()
{
    for (int i = 0; i < inventoryLimit; i++)
    {
        if (i < inventoryItems.Count)
        {
            inventoryImages[i].color = itemColor;
            inventoryImages[i].sprite = inventoryItems[i].uiIcon;
        }
        else
        {
            inventoryImages[i].sprite = null;
            inventoryImages[i].color = emptyColor;
        }
    }
}
```

![Image showing key pickup](/Attachments/29_4.gif)

## Scripts

Our Scripts at the end of this tutorial:

```cs
using UnityEngine;
using UnityEngine.SceneManagement;

public class PickupInteractable : MonoBehaviour, IInteractable
{
    public InventoryItem itemType;
    public string InteractText()
    {
        if (InventoryManager.instance.CheckInventorySpaceAvailable())
        {
            return "Pickup " + itemType.itemName;
        }
        else
        {
            return "Full Inventory: Cannot pick up " + itemType.itemName;
        }
    }

    public void Interact()
    {
        if (InventoryManager.instance.CheckInventorySpaceAvailable())
        {
            InventoryManager.instance.AddToInventory(itemType);
            Destroy(gameObject);
        }
    }
}

```

```cs
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class InventoryManager : MonoBehaviour
{
    public static InventoryManager instance;
    public List<InventoryItem> inventoryItems = new();
    public int inventoryLimit = 6;

    Color emptyColor = new Color(0, 0, 0, 0);
    Color itemColor = new Color(1, 1, 1, 1);

    public List<Image> inventoryImages = new();
    void Awake()
    {
        if (instance != null && instance != this)
        {
            Destroy(gameObject);
        }
        else
        {
            instance = this;
        }
    }

    public void AddToInventory(InventoryItem item)
    {
        inventoryItems.Add(item);
        RefreshInventory();
    }

    public void RemoveFromInventory(InventoryItem item)
    {
        inventoryItems.Remove(item);
        RefreshInventory();
    }

    public bool CheckInventorySpaceAvailable()
    {
        if (inventoryItems.Count == inventoryLimit)
        {
            return false;
        }
        else
        {
            return true;
        }
    }

    public bool CheckIfItemIsInInventory(InventoryItem item)
    {
        if (inventoryItems.Contains(item))
        {
            return true;
        }
        else
        {
            return false;
        }
    }

    void RefreshInventory()
    {
        for (int i = 0; i < inventoryLimit; i++)
        {
            if (i < inventoryItems.Count)
            {
                inventoryImages[i].color = itemColor;
                inventoryImages[i].sprite = inventoryItems[i].uiIcon;
            }
            else
            {
                inventoryImages[i].sprite = null;
                inventoryImages[i].color = emptyColor;
            }
        }
    }
}
```










