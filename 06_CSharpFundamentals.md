---
layout: page
title: C# Fundamentals
permalink: /06_CSharpFundamentals/
---
# C# Fundamentals

## What is C# 

Scripts that run within the Unity Editor are written in the programming language C# (pronounced C Sharp). C# is an object-oriented programming language and makes use of classes.

## Classes

Classes are software entities that encapsulate date and other functions.

For example the class `Asteroid` might contain data for the for the asteroid's speed as well as a function for moving.

```cs
public class Asteroid : MonoBehaviour
{
    float speed = 1f;

    void Move()
    {

    }
}
```

Notice that C# is an explicit language, meaning we must declare the data types for our variables, as well as declare whether our methods return data types. We can see the the `speed` of our asteroid is a `float`(decimal number) and we can see that the method `Move()` returns no data and we must explicitly type the keyword `void`. Additionally, as we can see in this example, `float` values must be followed by the letter `f`.

## Creating and Instance of a Class

Classes are only blueprints, to create an instance of a class, we need to use the keyword `new`. For example, if I wanted to use the Unity class [Vector2](https://docs.unity3d.com/6000.3/Documentation/ScriptReference/Vector2.html) which is a data type that can store and manipulate `(x,y)` data such as the position or direction of objects, I would need to use the key word `new` to create a new instance of that class.

```cs
Vector3 direction = new Vector3 (0f, 1f, 0f);
```

```cs
public class Asteroid : MonoBehaviour
{
    float speed = 1f;
    Vector3 direction = new Vector3 (0f, 1f, 0f);

    void Move()
    {

    }
}
```

## Accessibility levels

[C# Reference](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/accessibility-levels)

Within C# variables and variables can have different level of accessibility. By default variables and methods have a `protected` status, meaning that these variables and methods can only be accessed by the contained class or classes derived from the base class.

When programming within Unity, we often want our different scrips to have access to the variables and methods contained within the classes we create. To do this, we need to use the keyword `public` before our class or variable declaration.

For example, if we add `public` before our speed and direction variable, these variables will be accessible to other scripts. Importantly, for Unity, making a variable public, allows that variable to be accessible within the Inspector window.
```cs
public float speed = 1f;
public Vector3 direction = new Vector3 (0f, 1f, 0f);
```

![Screen shot showing speed and direction available within the inspector](/Attachments/Pasted%20image%2020260731130235.png)

We can then edit the variable directly within our editor without having to return to our script. This is also helpful is we have multiple asteroids and we all want them to have different speeds and directions.

