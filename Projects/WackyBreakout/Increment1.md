# Move the Paddle

For this step, you're letting the player move the paddle.

### Rigidbody2d Kinematic
- Changing Rigidbody 2d Body type to Kinematic means we will be changing the position of rigidbody ourselves rather than adding forces to move it. 

### Static Class
- ConfigurationUtils class is static because we don't want to attach the class to game object to instantiate it, we want to access the class directly. ( Example Math class )

### Rigidbody2d Field
- Add a Rigidbody2D field and add code to Start method for efficiency.
```
# region Fields 
    Rigidbody2D rb2d;
# endregion 

# region Unity methods
    void Start()
    {
        rb2d = GetComponent<Rigidbody2D>();
    }
# endregion
```

### Horizontal Input
- Add code to body of the FixedUpdate method to move rigidbody of paddle based on input on the Horizontal input axis 
```
#region Unity methods
    float horizontalInput = Input.GetAxis("Horizontal");
    if(horizontalInput != 0)
    {
        Vector2 position = rb2d.position;
        position.x += horizontalInput * ConfigurationUtils.PaddleMoveUnitsPerSecond * 
            Time.deltaTime;
        rb2d.MovePosition(position);
    }
#endregion
```
---

# Keep the Paddle in the PlayField



