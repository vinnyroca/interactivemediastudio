---
layout: page
title: Duplication, Random, and Camera
permalink: /08_DuplicationRandomandCamera/
---
# Random and Camera

## Random

A better approach to give each asteroid a different direction is to assign them a random speed at the start of the game. To do this we can use the `Random` class to generate random numbers.

### Random. Range()

For our `speed` variable, we can assign a random value by using `Random.Range()`. This function returns a value based on a minimum and maximum value.

To set our speed to a random value between .75 and 2.5 we can call  `Random.Range(.75f, 2.5f)` and set it equal to our speed variable.

```cs
void Start()
{
    speed = Random.Range(.75f, 2.5f);
    asteroidTransform = gameObject.GetComponent<Transform>();
}
```

### Random.insideUnitCircle

To generate a random `Vector2` for our `direction` we can use `Random.insideUnitCircle`. This returns a random `Vector2` with the x and y values ranging from -1 to 1.

However, since our direction is a `Vector3` we will need to use the values from a local variable and then assign those to our `direction` using a new Vector3. We can get access to the x and y values of our random `Vector2` by using `.x` and `.y`

```cs
void Start()
{
    Vector2 randomVector = Random.insideUnitCircle;
    direction = new Vector3(randomVector.x, randomVector.y, 0);
    
...
```

Playing our game, we will not see that our asteroids move in a random direction every time we play the game:

![Screen recording showing a user playing and restarting a unity scene with 5 circles moving in different direction at different speeds each time the game is played](/Attachments/8_2.gif)

## Camera

Currently, our asteroids fly off the screen, never to return. In the original version of the game, these asteroids wrap around once they exit an edge of the screen. We can detect when an asteroid passes over the edge of our screen by looking the the bounds of the Camera game objects in our screen.

Begin by making a new script called `ScreenWrap`. Add this script to all of your asteroids.

Within the script, add global `float` variables for `screenWidth` and `screenHeight`.
   
```cs
public class ScreenWrap : MonoBehaviour
{
    float screenWidth;
    float screenHeight;

...
```

To get the width and height of our game screen we need to look at the size of our Camera. Since we are using an orthographic camera, meaning that there is no distortion as a result of perspective we can look at the orthographic size of our camera which is equal to half our camera's height.

![screen shot show the size of the orthographic camera in the unity editor equal to 5](/Attachments/Pasted%20image%2020260731163130.png)
   
Since our Camera has a tag of MainCamera, we can get access to our camera in our scripts by using the keyword `Camera.main`. Within `Update()` we can set the `screenHeight variable to be equal to the `Camera.main.orthographicSize * 2`.

```cs
public class ScreenWrap : MonoBehaviour
{
    float screenWidth;
    float screenHeight;
    void Update()
    {
        screenHeight = Camera.main.orthographicSize * 2;
    }
}
```

The width of our camera will be equal to double our orthographic size our our camera multiplied by the aspect ratio of our camera. The aspect ratio of your camera is determined by the resolution of your camera with your game view. With Full HD your camera has an aspect ration of 16:9.
  
![Screen shot showing the the resolution of the game is set to Full HD](/Attachments/Pasted%20image%2020260731163758.png)

```cs
public class ScreenWrap : MonoBehaviour
{
    float screenWidth;
    float screenHeight;
    void Update()
    {
        screenHeight = Camera.main.orthographicSize * 2;
        screenWidth = Camera.main.orthographicSize * 2 * Camera.main.aspect;
    }
}
```

Lastly, we need to check if the position of our game object is beyond the edge of the screen, or in other words, half the value of the width or height. To do this, we can use  four if statements to see if our game object if beyond the four edges of our screen.

```cs
public class ScreenWrap : MonoBehaviour
{
    float screenWidth;
    float screenHeight;
    void Update()
    {
        screenHeight = Camera.main.orthographicSize * 2;
        screenWidth = Camera.main.orthographicSize * 2 * Camera.main.aspect;

        Vector2 pos = transform.position;
        float y = screenHeight / 2;
        float x = screenWidth / 2;

        if(pos.y > y)
        {
            pos.y = -y;
        }
        if (pos.y < -y)
        {
            pos.y = y;
        }
        if (pos.x > x)
        {
            pos.x = -x;
        }
        if (pos.x < -x)
        {
            pos.x = x;
        }

        transform.position = pos;
    }
}
```

![animated gif showing the circles popping as they move on and off the screen](/Attachments/8_3.gif)
Playing the game, we notice that circles pop in and out of the frame instead of smoothly exiting and entering. To fix this, we can add a small margin to the edges of our screen so the asteroids go beyond the edge of our screen before move to the other side. To do this we will create a  `float` variable called `wrapMargin` and set it to a small value such as .5. We'll make it public so that we can change the value based on the size of the object.

```cs
public float wrapMargin = .5f;
```

Next, we can add this `wrapMargin` to our `screenWidth` and `screenHeight`. Are final script will look like this:

```cs
using UnityEngine;

public class ScreenWrap : MonoBehaviour
{
    float screenWidth;
    float screenHeight;
    float wrapMargin = .5f;
    void Update()
    {
        screenHeight = Camera.main.orthographicSize * 2 + wrapMargin;
        screenWidth = Camera.main.orthographicSize * 2 * Camera.main.aspect + wrapMargin;

        Vector2 pos = transform.position;
        float y = screenHeight / 2;
        float x = screenWidth / 2;

        if(pos.y > y)
        {
            pos.y = -y;
        }
        if (pos.y < -y)
        {
            pos.y = y;
        }
        if (pos.x > x)
        {
            pos.x = -x;
        }
        if (pos.x < -x)
        {
            pos.x = x;
        }

        transform.position = pos;
    }
}

```

![animated gif showing the circles moving more smoothly as the pass one side of the screen and enter another](/Attachments/8_4.gif)