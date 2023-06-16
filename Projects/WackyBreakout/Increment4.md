# Add Bonus blocks
For this step, you're adding the bonus blocks. 
### Bonus block points
- Add a value in the configuration data CSV file for the Bonus block points (I used 2 for this) and add the appropriate fields and properties to the ConfigurationData and ConfigurationUtils
class so the BonusBlock class will be able to access this value. 
```
# region Fields
int bonusBlockPoints = 2
#endregion

#region Properties 
public int BonusBlockPoints 
{
    get {return bonusBlockPoints;}
}
#end region
```

### BonusBlock script
- Add a sprite for the Bonus block and create a new BonusBlock script as a child class of the 
Block class. Add code to the BonusBlock Start method to set the points the block is worth to 
the Bonus block value from the ConfigurationUtils property. 
```
#region Unity methods
void Start()
    {
        Points = ConfigurationUtils.BonusBlockPoints;
    }
#endregion
```

### LevelBuilder script
- Make a BonusBlock prefab using the Bonus block sprite and the BonusBlock script. Add a field 
for the BonusBlock prefab to the LevelBuilder script and populate that field in the Inspector. 
```
#region Fields
[SerializeField]
GameObject prefabBonusBlock;
#endregion
```

- Change the LevelBuilder script to only use Bonus blocks so you can make sure the scoring 
works properly. 
```
void PlaceBlock(Vector2 position)
{
    else if (randomBlockType <
            (ConfigurationUtils.StandardBlockProbability + ConfigurationUtils.BonusBlockProbability))
        {
            Instantiate(prefabBonusBlock, position, Quaternion.identity);
        }
}
```

# Add Freezer and Speedup blocks
For this step, you're adding the effect (freezer and speedup) blocks. You won’t be adding the 
actual effects until the next increment, but you’re adding the effect blocks to the level now. 

### Effect block points
- Add a value in the configuration data CSV file for the Effect block points (I used 5 for this) and add the appropriate fields and properties to the ConfigurationData and ConfigurationUtils
class so the EffectBlock class (see next paragraph) will be able to access this value. 
```
public class ConfigurationData
{
#region Fields
int effectBlockPoints = 5;
#endregion    

#region Properties
public int EffectBlockPoints
{
    get { return effectBlockPoints; }
}
#endregion

public static class ConfigurationUtils
{
#region Properties
public static int BonusBlockPoints
{
    get { return configurationData.BonusBlockPoints; }
}
#endregion
}

}    
```

### EffectBlock script
- Add sprites for the Freezer and Speedup blocks and create a new EffectBlock script as a child 
class of the Block class. Add code to the EffectBlock Start method to set the points the block 
is worth to the Effect block value. Add fields to the script to store the two effect sprites and 
populate them in the Inspector.
```
public class EffectBlock : Block
{
    #region Fields

    [SerializeField]
    Sprite freezerSprite;
    [SerializeField]
    Sprite speedupSprite;

    // effect-specific values
    EffectName effect;

    #endregion

    #region Properties

    /// <summary>
    /// Sets the effect for the pickup
    /// </summary>
    /// <value>pickup effect</value>
    public EffectName Effect
    {
        set
        {
            effect = value;

            // set sprite
            SpriteRenderer spriteRenderer = GetComponent<SpriteRenderer>();
            if (effect == EffectName.Freezer)
            {
                spriteRenderer.sprite = freezerSprite;
            }
            else
            {
                spriteRenderer.sprite = speedupSprite;
            }
        }
    }

    #endregion

    #region Unity methods

    /// <summary>
    /// Start is called before the first frame update
    /// </summary>	
    void Start()
    {
        Points = ConfigurationUtils.EffectBlockPoints;
    }
}
```

### EffectBlock prefab using the Freezer block sprite and the EffectBlock script
 - Make a EffectBlock prefab using the Freezer block sprite and the EffectBlock script. Add a field for the EffectBlock prefab to the LevelBuilder script and populate the field in the Inspector.
```

public class LevelBuilder : MonoBehaviour
{
    #region Fields
    [SerializeField]
    GameObject prefabEffectBlock
    #endregion
}

```

### EffectName enum with two values: Freezer and SpeedUp.
- Copy the EffectName.cs file from the zip to the Scripts/Gameplay folder. That file defines a EffectName enum with two values: Freezer and SpeedUp. Add a filed to the EffectBlock script to hold which kind of effect the block applies and a public property for setting the field. 

- And a public property for setting the field. Set the sprite for the block appropriately within the set accessor for the property 


```
public class EffectBlock : Block
{
    #region Fields
    // effect specific values
    EffectName effect
    #endregion

    #region 
    public EffectName Effect
    {
        set
        {
            effect = value

            // set sprite 
            SpriteRenderer spriteRenderer = GetComponent<SpriteRenderer>();
            if (effect == EffectName.Freezer)
            {
                spriteRenderer.sprite = freezerSprite;
            }
            else
            {
                spriteRenderer.sprite = speedupSprite;
            } 
        }
    }
    #endregion
}
```

### Make sure scoring and sprite setting works correctly 
- Change the LevelBuilder script to only use Freezer blocks so you can make sure the scoring and 
sprite setting works properly. Do the same for Speedup blocks. Hint: You'll need to save the 
block game object you instantiate as you add each block to the level so you can get the 
EffectBlock component attached to that game object so you can access the property to set the 
effect for the block. 
- Note: You do NOT have to actually implement the freezer and speedup effects; that happens in 
the next increment. The points should be correct when you hit a effect block, though. 

# Randomizing the blocks
For this step, you're randomly adding the four types of blocks to the scene based on probabilities.
### Appropriate fields and properties to ConfigurationData and ConfigurationUtils class 

- Add values in the configuration data CSV file for the different block probabilities (I used 70 for standard blocks, 20 for bonus blocks, 5, for freezer blocks, and 5 for speedup blocks). Add the appropriate fields and properties to the ConfigurationData and ConfigurationUtils class so 
the LevelBuilder class will be able to access those values. 
```
public class ConfigurationData
{
    #region Fields

    float standardBlockProbability = 0.7f;
    float bonusBlockProbability = 0.2f;
    float freezerBlockProbability = 0.05f;
    float speedupBlockProbability = 0.05f; 
    
    #endregion

    #region Properties
    /// <summary>
    /// Gets the probability that a standard block
    /// will be added to the level
    /// </summary>
    /// <value>standard block probability</value>
    public float StandardBlockProbability
    {
        get { return standardBlockProbability; }
    }

    /// <summary>
    /// Gets the probability that a bonus block
    /// will be added to the level
    /// </summary>
    /// <value>bonus block probability</value>
    public float BonusBlockProbability
    {
        get { return bonusBlockProbability; }
    }

    /// <summary>
    /// Gets the probability that a freezer block
    /// will be added to the level
    /// </summary>
    /// <value>freezer block probability</value>
    public float FreezerBlockProbability
    {
        get { return freezerBlockProbability; }
    }

    /// <summary>
    /// Gets the probability that a speedup block
    /// will be added to the level
    /// </summary>
    /// <value>speedup block probability</value>
    public float SpeedupBlockProbability
    {
        get { return speedupBlockProbability; }
    } 
    #endregion

}

```

### LevelBuilder script to randomly decide 
- Add code to the LevelBuilder script to randomly decide which block to place each time a block 
is added to the level based on the probabilities for each block. My Start method was getting 
somewhat large at this point, so I added a PlaceBlock method that randomly picks a block to 
place at a given location.

```
#region Private methods

    /// <summary>
    /// Places a randomly-selected block at the given position
    /// </summary>
    /// <param name="position">position of the block</param>
    void PlaceBlock(Vector2 position)
    {
        float randomBlockType = Random.value;
        if (randomBlockType < ConfigurationUtils.StandardBlockProbability)
        {
            Instantiate(prefabStandardBlock, position, Quaternion.identity);
        }
        else if (randomBlockType <
            (ConfigurationUtils.StandardBlockProbability + ConfigurationUtils.BonusBlockProbability))
        {
            Instantiate(prefabBonusBlock, position, Quaternion.identity);
        }
        else
        {
            // effect block selected
            GameObject effectBlock = Instantiate(prefabEffectBlock, position, Quaternion.identity);
            EffectBlock effectBlockScript = effectBlock.GetComponent<EffectBlock>();

            // set effect
            float freezerThreshold = ConfigurationUtils.StandardBlockProbability +
                ConfigurationUtils.BonusBlockProbability +
                ConfigurationUtils.FreezerBlockProbability;
            if (randomBlockType < freezerThreshold)
            {
                effectBlockScript.Effect = EffectName.Freezer;
            }
            else
            {
                effectBlockScript.Effect = EffectName.Speedup;
            }
        }
    }

    #endregion

     void Start()
    {
        Instantiate(prefabPaddle);

        // retrieve block size
        GameObject tempBlock = Instantiate<GameObject>(prefabStandardBlock);
        BoxCollider2D collider = tempBlock.GetComponent<BoxCollider2D>();
        float blockWidth = collider.size.x;
        float blockHeight = collider.size.y;
        Destroy(tempBlock);

        // calculate blocks per row and make sure left block position centers row
        float screenWidth = ScreenUtils.ScreenRight - ScreenUtils.ScreenLeft;
        int blocksPerRow = (int)(screenWidth / blockWidth);
        float totalBlockWidth = blocksPerRow * blockWidth;
        float leftBlockOffset = ScreenUtils.ScreenLeft +
            (screenWidth - totalBlockWidth) / 2 +
            blockWidth / 2;

        float topRowOffset = ScreenUtils.ScreenTop -
            (ScreenUtils.ScreenTop - ScreenUtils.ScreenBottom) / 5 -
            blockHeight / 2;

        // add rows of blocks
        Vector2 currentPosition = new Vector2(leftBlockOffset, topRowOffset);
        for (int row = 0; row < 3; row++)
        {
            for (int column = 0; column < blocksPerRow; column++)
            {
                PlaceBlock(currentPosition);
                currentPosition.x += blockWidth;
             }

            // move to next row
            currentPosition.x = leftBlockOffset;
            currentPosition.y += blockHeight;
        }
    }

```