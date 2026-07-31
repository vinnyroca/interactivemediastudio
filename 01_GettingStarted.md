---
layout: page
title: 01. Getting Started with Unity
permalink: /01_GettingStarted.md/
---
# Getting Started with Unity


```cs

using UnityEngine;
using UnityEngine.InputSystem;

public class PaddleMover : MonoBehaviour
{
    InputSystem_Actions inputActions;
    Rigidbody2D rb;
    [SerializeField]
    float speed =3;
    Vector2 direction;

    private void OnEnable()
    {
        inputActions.Enable();
        inputActions.Player.Move.performed += OnMove;
        inputActions.Player.Move.canceled += OnMove;
    }

    private void OnDisable()
    {
        inputActions.Disable();
    }

    // Start is called once before the first execution of Update after the MonoBehaviour is created
    void Awake()
    {
        inputActions = new InputSystem_Actions();
        rb = GetComponent<Rigidbody2D>();
    }
    
    void OnMove(InputAction.CallbackContext ctx)
    {
        Vector2 moveVector = ctx.ReadValue<Vector2>();
        Debug.Log(moveVector);
        Vector2 paddleDirection = new Vector2(moveVector.x, 0);
        direction = paddleDirection.normalized;
    }

    private void FixedUpdate()
    {
        rb.MovePosition(rb.position + direction * speed * Time.fixedDeltaTime);
    }

}

```