# Break the block
### OnCollisionEnter2D 
- Add block script and attach it to block prefab
- This method should destroy the block if the collision is with ball game object
```
#region Unity method
    void OnCollisionEnter2D(Collision2D coll)
    {
        if(coll.gameObject.CompareTag("Ball"))
        {
            Destroy(gameObject);
        }
    }
#endregion
```
---
# Automatically build 3 rows of standard blocks
### BallSpawner 
- Spawns a new ball when it's added to the scene.
```
#region Fields
    [SerializedField]
    GameObject prefabBall;
#endregion

#region Unity method
    void Start()
    {
        GameObject tempBall = Instantiate<GameObject>(prefabBall);
    }
#endregion
```

### LevelBuilder
- Create a new LevelBuilder script
- Add a field to the script for the Paddle prefab, Add code to Start method to instantiate paddle
```
#region Fields
    [SerializedField]
    GameObject prefabPaddle;
#endregion 

#region Unity method
    void Start()
    {
        Instantiate(prefabPaddle);
    }
#endregion
```

### StandardBlock
- Add code to build 3 rows of standard blocks. The row should start around 1/5 of screen height down from the top of the screen and the rows should be centered horizontally. You'll need to create a temporary block to retrieve the block weight and height, destroy the block once you have done that.
```
# region Fields
    [SerializedField]
    GameObject prefabStandardBlock;
#endregion

# region Unity method
    void Start()
    {
        // retrieve block size 
        GameObject tempBlock = Instantiate<GameObject>(prefabStandardBlock);
        BoxCollider2D collider = tempBlock.GetComponent<BoxCollider2D>();
        float blockWidth = collider.size.x;
        float blockHeight = collider.size.y;
        Destroy(tempBlock);

        // calculate blocks per row and make sure left block positions center row 
        float screenWidth = ScreenUtils.ScreenRight - ScreenUtils.ScreenLeft;
        int blocksPerRow = (int)(screenWidth / blockWidth);
        float totalBlockWidth = blocksPerRow * blockWidth;
        float leftBlockOffset = ScreenUtils.ScreenLeft + (screenWidth - totalBlockWidth) / 2 + blockWidth / 2;

        float topRowOffset = ScreenUtils.ScreenTop - (ScreenUtils.ScreenTop - ScreenUtils.ScreenBottom ) / 5 - blockHeight / 2;

        // add rows of block 
        Vector2 currentPosition = new Vector2(leftBlockOffset, topRowOffset);
        for(int row = 0; row < 3; row++)
        {
            for(int column = 0; column < blocksPerRow; column++)
            {
                Instantiate(prefabStandardBlock, currentPosition, Quaternion.identity);
                currentPosition.x += blockWidth;
            }

            // move to next row 
            currentPosition.x = leftBlockOffset;
            currentPosition.y += blockHeight;
        }
    }
#endregion
```
---
# Add a death timer to the ball 
Destroy the ball after a set period

### Ball lifetime
- Add a property to the ConfigurationUtils class for the ball lifetime in seconds, making the property public so the Ball class will be able to access it.
```
#region Properties
public static float BallLifeSeconds
{
    get{ return 10;}
}
#endregion
```

### Timer component
- Copy Timer script into Gameplay folder
- Add the timer component to the Ball and run the timer with the ball lifetime as the timer duration when the Ball is added to the scene.
```
#region Unity method
    void Start()
    {
        // start death time 
        deathTimer = gameObject.addComponent<Timer>();
        deathTimer.Duration = ConfigurationUtils.BallLifeSeconds;
        deathTimer.Run();
    }
#endregion 
```

## Timer finished
- Have the ball destroy itself when the timer is finished 
```
#region Unity method 
    void Update()
    {
        if(deathTimer.Finished)
        {
            Destroy(gameObject);
        }
    }
#endregion
```
---
# Spawn new ball into a collision free location
### For this step you are making sure ball are spawned into collision free location. The idea behind this approach is that if I'd be spawning into a collision, I set a flag to say I need to retry to spawn and I try the spawn again on the next frame of the game if that flag is true.
```
#region Fields
    bool retrySpawn = false;
    Vector2 spawnLocationMin;
    Vector2 spawnLocationMax;
#endregion

#region Unity methods
    void Start()
    {
        // spawn and destroy ball to calculate spawn location min and max
        GameObject tempBall = Instantiate<GameObject>(prefabBall);
        BoxCollider2D collider = tempBall.GetComponent<BoxCollider2D>();
        float ballColliderHalfWidth = collider.size.x / 2;
        float ballColliderHalfHeight = collider.size.y / 2;

        spawnLocationMin = new Vector2(
            tempBall.transform.position.x - ballColliderHalfWidth,
            tempBall.transform.position.y - ballColliderHalfHeight);
        
        spawnLocationMax = new Vector2(
            tempBall.transform.position.x + ballColliderHalfWidth,
            tempBall.transform.position.y + ballColliderHalfHeight);
        Destroy(tempBall);

        // spawn first ball in game 
        SpawnBall();
    }
#endregion

#region Public methods
    public void SpawnBall()
    {
        // make sure we don't spawn into a collision
        if(Physics2D.OverlapArea(spawnLocationMin, spawnLocationMax) == null)
        {
            retrySpawn = false;
            Instantiate(prefabBall);
        }
        else
        {
            retrySpawn = true;
        }
    }
#endregion
```

# Ball script call BallSpawner
Have the Ball script call the BallSpawner method just before it destroys itself. You need to get access to the BallSpawner script using GetComponent method.
```
#region Unity method
    void Update()
    {
        // die when time is up
        if(deathTimer.Finished)
        {
            Camera.main.GetComponent<BallSpawner>().SpawnBall();
            Destroy(gameObject);
        }
    }
#endregion
```
---
# Fix ball spawning unfairness
For this step, you are making the ball wait for a second before they start moving. This actually gives player a chance to "get set" at the start of the game, and also make it more fair when a new ball is spawned into scene. 
```
#region Unity method
    void Start()
    {
        // start move timer
        moveTimer = gameObject.AddComponent<Timer>();
        moveTimer.Duration = 1;
        moveTimer.Run();
    }

    void Update()
    {
        // move when time is uup
        if(moveTimer.Finished)
        {
            moveTimer.Stop();
            StartMoving();
        }
    }
#endregion

#region Private methods
    void StartMoving()
    {
        // get the ball moving
        float angle = -90* Mathf.Deg2Rad;
        Vector2 force = new Vector2(
            ConfigurationUtils.BallImpulseForce * Mathf.Cos(angle), 
            ConfigurationUtils.BallImpulseForce * Mathf.Sin(angle));
        GetComponent<Rigidbody2D>().AddForce(force);
    }
#endregion
```

