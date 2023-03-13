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

