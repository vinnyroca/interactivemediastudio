# Scenes

So far in Unity we have learned about the Component and Game Object relationship. Components are small bits of code that are stored on a game object. But where are game objects store? Where is information stored on how our game objects are laid out in our project.

Although it has not been discussed so far, every time we have been saving our project in Unity, we have been saving a Scene. A scene in unity is a collection of game objects and other assets. Thus far in this project, we have been working within the default scene in Unity. This scene is often called SampleScene.

You can see this scene name at the top of your Unity project:

![](Attachments/Pasted%20image%2020260921205407.png)

You can also see within our Project we have a Scenes folder:

![](Attachments/Pasted%20image%2020260921205433.png)

What are scenes used for? In Unity, scenes are used to structure our projects. They allow us to collect game objects and assets in the scene editor and then save the position and relationship between these objects. Within a Unity project, you technically only need one scene. As long as you have one scene, you can create a finished application.

In this tutorial, however, we are going to focus on how you can have multiple scenes within your Unity project and how can we transition in between different scenes. 

## Creating Scenes

To create a scene, we can **right click** in our Project window and select **Create > Scene > Scene**. It is recommended that you create this Scene in your scene folder. Let's name this scene `MainMenu`:

![Animated image of creating a new main menu scene](/Attachments/22_1.gif)

**Note:** Before opening another  scene make sure you save the state of the current scene. Switching to scene within out saving will result in losing data.

We can then select our MainMenu scene and double click to open it.

Editing scenes
Script to Transition into other scene
Build Index
Attach public method to button
Edit existing scene to go back to main menu on loss.

