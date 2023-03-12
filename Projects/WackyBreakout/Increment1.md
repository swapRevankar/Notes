# Move the Paddle

For this step, you're letting the player move the paddle.

### Rigidbody2d Kinematic
- Changing Rigidbody 2d Body type to Kinematic means we will be changing the position of rigidbody ourselves rather than adding forces to move it. 

### Static Class
- ConfigurationUtils class is static because we don't want to attach the class to game object to instantiate it, we want to access the class directly. ( Example Math class )

### Rigidbody2d Field
- Add a Rigidbody2D field and add code to Start method for efficiency.
```
#region Fields 
    Rigidbody2D rb2d;
#endregion 

#region Unity methods
    void Start()
    {
        rb2d = GetComponent<Rigidbody2D>();
    }
#endregion
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
#region Fields
    float halfColliderWidth;
#endregion 

#region Unity methods
    void Start()
    {
        BoxCollider2d bc2d = GetComponent<BoxCollider2D>();
        halfColliderWidth = bc2d.size.x / 2;
    }
#endregion 
```

### CalculateClampedX Method
- Calculate a valid new x position, including the clamping, before you call the method to move the rigid body
```
#region Public method
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
#endregion    
```
---

# Move Ball

### BallImpulseForce Method
Add a public static method to ConfigurationUtils class to provide BallImpulseForce value. Return 200 from that property 
```
#region Properties
    public static float BallImpulseForce
    {
        get {return 200;}
    }
# endregion
``` 

### Get Ball Moving
Add code to Start method of Ball class to get the ball moving.
- This code initializes a float variable named "angle" with a value of negative 90 degrees converted to radians using Math.Deg2Rad constant. It then creates a new Vector2 variable named "force" with x and y components equal to cosine and sine of teh angle variable. 
- The cosine and sine functions return the ratio of the adjacent and hypotenuse sides, and opposite and hypotenuse sides, of a right angle triangle with given angle. In this case, since the angle is -90 (-Pi/2 radians) the cosine of the angle is 0 and the sine of the angle is -1.
- Therefore, the "force" variable is initialized with an x component for 0 and a y component of -1, which means it points straight down in the negative y direction. This could be used, for example, to apply downward force to a physics object in game. 
```
#region Unity method
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
---
# Curve The Paddle
### BounceAngleHalfRange 
In Paddle script add a BounceAngleHalfRange constant field. Set constant value to 60, converted to radians.
```
#region Fields
    const float BounceAngleHalfRange = 60 * Mathf.Deg2Rad;
#endregion
```
### Paddle OnCollisionEnter2D
Calculates a new vector with magnitude 1 for the new direction the ball should move in based on where the ball hit the paddle.
```
#region Unity method
void OnCollisionEnter2D(Collision2D coll)
{
    if(coll.gameObject.CompareTag("Ball"))
    {
        // calculate new ball direction
        float ballOffsetFromPaddleCenter = transform.position.x - coll.transform.position.x;
        float normalizedBallOffset = ballOffsetFromPaddleCenter / halfColliderWidth;
        float angleOffset = normalizedBallOffset * BounceAngleHalfRange;
        // add angle offset to PI/2(90 degrees) to get new angle
        float angle = Mathf.PI / 2 + angleOffset;
        Vector2 direction = new Vector2(Mathf.Cos(angle), Mathf.Sin(angle));

        // tell ball to set direction to new direction
        Ball ballScript = coll.gameObject.GetComponent<Ball>();
        ballScript.SetDirection(direction);
    }
}
#endregion


```
### Ball SetDirection
This method should change the direction the call is moving while keeping the speed the same as it was. Current ball speed is just the magnitude of the rigidbody velocity, so setting the velocity to current speed * new direction is what you need.
```
#region Public methods
public void SetDirection(Vector2 direction)
{
    // get current rigid body speed
    RigidBody2D rb2d = GetComponent<RigidBody2D>();
    float speed = rb2d.velocity.magnitude;
    rb2d.velocity = direction.speed;
}
#endregion
```
---
# Fix Side Collision Bug
### TopCollision
- The Collider2D object passed in as a parameter for the OnCollisionEnter2D method provides some useful information, especially the contact points for the collision. 
- Get 2 points for the collision and compare them. When comparing floats you don't really wanna use == for your comparison, you'll want to check if the two floats are within some tolerance of each other 
```
#region Private method
bool TopCollision(Collision2D coll)
{
    const float tolerance = 0.05f;

    // on top collision, both points are at the same y location
    ContactPoint2D[] contacts = new ContactPoint2D[2];
    coll.GetContacts(contacts);
    return Mathf.Abs(contacts[0].point.y - contacts[1].point.y) < tolerance;
}
#endregion
```
### Change Boolean expression
Change the boolean expression for the if statement in the OnCollisionEnter2D method to check for both the ball tag and a top collision
```
if(coll.gameObject.CompareTag("Ball") && TopCollision(coll))
{
    
}

```





