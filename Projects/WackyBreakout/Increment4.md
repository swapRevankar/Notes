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