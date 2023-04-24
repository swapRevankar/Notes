# Add configuration data file processing
### Copy ConfigurationData script
- Copy the ConfigutationData script into your Scripts/Configuration folder

### static field to hold ConfigurationData Object
- Open the ConfigurationUtils script in Visual Studio and add a static field to hold a 
ConfigurationData object.  
```
public static class ConfigurationUtils
{
    #region Fields
    static ConfigurationData configurationData;
    #endregion
}
```
### 

### Initialize method
- Add code to the ConfigurationUtils Initialize method to call the ConfigurationData constructor to populate the new field. 
```
public static void Initialize()
{
    configurationData = new ConfigurationData();
}
```
- Change all the 
ConfigurationUtils properties to return the appropriate properties from your new field 
instead of the hard-coded values they currently return. 
```
  public static int Example
{
    get { return configurationData.Example; }
}
```

### GameInitializer Awake
- Add code to the GameInitializer Awake method to call the ConfigurationUtils
Initialize method
```
public class GameInitializer: MonoBehaviour
{
    void Awake()
    {
        ConfigurationUtils.Initialize();
    }
}
```
### Comma-separated value
- Create a csv (comma-separated value) file containing the five pieces(PaddleMoveUnitsPerSecond,BallImpulseForce,BallLifeSeconds,MinSpawnSeconds,MaxSpawnSeconds,BallsPerGame,StandardBlockPoints) of configuration data 
that are currently exposed by the ConfigurationData class; you can name the file 
anything you want, but be sure to put it in your StreamingAssets folder. The first line in 
the file should be a comma-separated list of the value names and the second line in the 
file should be a comma-separated list of the values



