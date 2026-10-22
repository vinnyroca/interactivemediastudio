---
layout: page
title: Scriptable Objects
permalink: /Tutorials/28_ScriptableObjects/
---
# Scriptable Objects

In this tutorial, we will learn how to create scriptable objects and why to use them within our project.

## What is a scriptable object?

Similar to `Monobehaviour`, scriptable objects or `ScriptableObjects` is a Unity class that let's us store data within our project folder. In other words, while all other classes in Unity have to be attached to some type of game object in our scene to exist within our project, Scriptable Objects live within our project folder without having to have an "in game" instance.

For example, we could create a scriptable object class that stores a name and an image. We could store this name image pairing within our asset folder and use it within our scene to pass data to various objects.

### Why Use Scritptable Objects

While it is not always necessary to use scriptable objects within Unity projects, their largest benefit is the ability to decouple logic from visuals. In this tutorial, we will be using scriptable objects to create inventory item definitions within our project folder. We are then able to attach these object definitions to collectibles within our scene, and then pass that information to some form of manager when an item is picked up. In this way, even when our object is destroyed on pickup, the data that is past persists because it lives within our asset folder and not our scene. Like many concepts within programming, scriptable objects seem fairly abstract until we put them into practice, so let's lay out our a plan for this tutorial: 

### Inventory Manager, Inventory Items, Prefab Pickups

In this multi-part tutorial, we are going to make an inventory system within our project that is able to store items, and then use those items at specific moments. For example, we might be able to pick up a key and use that key to open a door.

To produce this system, we need three parts:

1. Pickup Interactables
2. Inventory Manager
3. Inventory Item Definitions 

Considering in the last tutorial we focused on interactables, it is easy to imagine how the pick up interactable will function. to create this item, we'll need some form of pickup script that implements the `IInteractable` interface. 

From there, we need to consider what happens with this object when we interact with it. One option we have is to create and Inventory Manager that keeps track of everything in our inventory and handles adding and removing items. This will be a similar form of singleton architecture that we have used before.

The question then becomes, what are we actually storing within our inventory. If we consider a real inventory, or a backpack that holds items, the backpack, or inventory, holds the items themselves. One might imagine in a 3D game such as *Peak* or *Big Walk*, one's inventory is actually the game object that one picks up and holds. In 2D games, and in most game inventories, we do not actually store the physics-based items, but instead store an abstraction of those items as data. In other words, our inventory is a list of data.

![Screenshot of the game Big Walk](/Attachments/Pasted%20image%2020261021150655.png)

Holding Items in *Big Walk*

In the simplest version, an inventory can be a list of strings that describe what is in the inventory by name `['apple', "hat", "key" ]`. However, in most cases, our Inventory items need more data than just a name. Often times we need additional data such as images or stats.

Since these items need to contain more than just name data, we need to consider how can we store and pass complex data within our inventory. With this question comes many considerations. 

If, for example, we want each item in our inventory to be a unique object, such as weapons within *Legend of Zelda: Breath of the Wild* that decay over time, our Inventory may need to contain particular instances of objects whose properties change over time.

![Screenshot of the Inventory in Legend of Zelda: Breath of the Wild](/Attachments/Pasted%20image%2020261021150328.png)

Inventory in *Legend of Zelda: Breath of the Wild*

However, if our inventory just needs to store a generic amount of items, instead of storing particular instances of those items, we can instead just store a generic set of data for each item as well as the amount of that item within our inventory. We can see a version of this generic inventory system within *Minecraft*:

![Image of Inventory in Minecraft](/Attachments/Pasted%20image%2020261021151159.png)

Inventory in *Minecraft*

In this tutorial, since we are going to focus on using scriptable objects, which represent data types as opposed to unique instances of collections of data, we will create a simple inventory that holds a list of generic items. Given the preexisting UI within our scene, we will plan to design some form of inventory system that can hold six items (not stacked), and we can add items to our inventory by picking up those items within our scene. 

Let's begin with creating scriptable object to hold our inventory item data.

## Creating a Scriptable Object

To create scriptable object, navigate to your project folder and right click. Navigate to **Create > Scripting > ScriptableObject Script**. You can create this script within your scriptable objects folder, and name the script `InventoryItem`.

This script is essentially our Scriptable Object template. We can notice that at the top of our script, if we want to create an inventory item scriptable object, we can navigate to **Scriptable Objects/InventoryItem** within the right click menu of our project folder.

Before we create and Inventory Item scriptable object, let's first define the data this scriptable object will contain. We need this scriptable object to contain both an image and an object name, so lets make two public variables to hold a `string` and a `Sprite`:

```cs
using UnityEngine;

[CreateAssetMenu(fileName = "InventoryItem", menuName = "Scriptable Objects/InventoryItem")]
public class InventoryItem : ScriptableObject
{
    public string itemName;
    public Sprite uiIcon;
}
```

Next, after saving our script, let's right click in our ScriptableObjects folder to create a new InventoryItem scriptable object. We can name this object `Key`:

![Animated video showing a key scriptable object being created](/Attachments/28_1.gif)

Next, let's provide our scriptable object with the item name of "Key" and provide a UI icon of our "Key Fork" image from our Art folder:

![Animated image showing add the name and image to our scriptable object](/Attachments/28_2.gif)

In the next tutorial, we will focus on creating an Inventory Manager to hold a list of our Inventory Items.










