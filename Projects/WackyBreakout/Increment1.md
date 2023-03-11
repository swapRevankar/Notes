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

### Paddle Half Width 
- Retrieve paddle Box Collider 2d component, calculate half the width, and store it in a field. We do it for optimization.
```
# region Fields
    float halfColliderWidth;
# endregion 

# region Unity methods
    void Start()
    {
        BoxCollider2d bc2d = GetComponent<BoxCollider2D>();
        halfColliderWidth = bc2d.size.x / 2;
    }
# endregion 
```

### CalculateClampedX Method
- Calculate a valid new x position, including the clamping, before you call the method to move the rigid body
```
# region Public method
    float CalculateClampedX(float x)
    {
        if(x - halfColliderWidth < ScreenUtils.ScreenLeft)
        {
            x = ScreenUtils.ScreenLeft + halfColliderWidth;
        }
        else if (x + halfColliderWidth > ScreenUtils.ScreenRight)
        {
            x = ScreenUtils.ScreenRight - halfColliderWidth;
        }
        return x;
    }
# endregion    
```
---

# Move Ball

### BallImpulseForce Method
Add a public static method to ConfigurationUtils class to provide BallImpulseForce value. Return 200 from that property 
```
# region Properties
    public static float BallImpulseForce
    {
        get {return 200;}
    }
# endregion
``` 

### Get Ball Moving
Add code to Start method of Ball class to get the ball moving.
```
# region Unity method
    void Start()
    {
        float angle = -90 * Mathf.Deg2Rad;
        Vector2 force = new Vector(
                ConfigurationUtils.BallImpulseForce * Mathf.Cos(angle),
                ConfigurationUtils.BallImpulseForce * Mathf.Sin(angle));
        GetComponent<RigidBody2d>().AddForce(force);
    }
#endregion

```




