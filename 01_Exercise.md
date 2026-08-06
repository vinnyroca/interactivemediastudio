---
layout: page
title: MoveRotation Exercise
permalink: /01_Exercise/
---
# MoveRotation Exercise

## Description

For this exercise, get your spacecraft to rotate using [`MoveRotation()`](https://docs.unity3d.com/6000.3/Documentation/ScriptReference/Rigidbody2D.MoveRotation.html)

To do this, you will need to create a new `InputAction` and store and read the value from that input action. Since we are rotating in either direction, you will need to use a **Postive\Negative Binding**. Rotate your spacecraft using the `A` and `D` key. Additionally, use a `float` variable named `rotatePower` to control the speed of your rotation.

As a hint, notice that [Rigidbody2D.rotation](https://docs.unity3d.com/6000.3/Documentation/ScriptReference/Rigidbody2D-rotation.html) is a `float` value, and [MoveRotation](https://docs.unity3d.com/6000.3/Documentation/ScriptReference/Rigidbody2D.MoveRotation.html) takes in a `float` value.

![animate gif showing a white triangle moving around a black screen](/Attachments/input_7.gif)