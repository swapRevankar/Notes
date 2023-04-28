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