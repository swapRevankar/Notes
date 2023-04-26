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

### ConfigurationData Constructor 
- Add code to the ConfigurationData constructor to open the configuration data file 
using Application.streamingAssetsPath and your file name as the full file name.You’ll need to add a System.IO using directive to get access to the StreamReader class and a System using directive to get access to the Exception class.
```
#region Constructor
public ConfigurationData()
    {
        // read and save configuration data from file
        StreamReader input = null;
        try
        {
            // create stream reader object
            input = File.OpenText(Path.Combine(
                Application.streamingAssetsPath, ConfigurationDataFileName));

            // read in names and values
            string names = input.ReadLine();
            string values = input.ReadLine();

            // set configuration data fields
            SetConfigurationDataFields(values);
        }
        catch (Exception e)
        {
        }
        finally
        {
            // always close input file
            if (input != null)
            {
                input.Close();
            }
        }
    }
#endregion
``` 

### SetConfigurationDataFields
- Add code to extract the values from the second line in the file and populate the fields 
with them
```
#region Private method
void SetConfigurationDataFields(string csvValues)
    {
        // the code below assumes we know the order in which the
        // values appear in the string. We could do something more
        // complicated with the names and values, but that's not
        // necessary here
        string[] values = csvValues.Split(',');
        paddleMoveUnitsPerSecond = float.Parse(values[0]);
        ballImpulseForce = float.Parse(values[1]);
        ballLifeSeconds = float.Parse(values[2]);
        minSpawnSeconds = float.Parse(values[3]);
        maxSpawnSeconds = float.Parse(values[4]);
        ballsPerGame = int.Parse(values[5]);
        standardBlockPoints = int.Parse(values[6]);
    }
#endregion
```

# Add HUD with score and balls remaining
### Add HUD with score and ball remaining
- Add a new Canvas to the scene and rename it to HUD. Add balls left and score Text objects as 
children of that game object. Put reasonable default text for those Text objects.
Be sure to select the Canvas in the HUD game object and change the UI Scale Mode in the 
Canvas Scaler component to Scale With Screen Size. This makes it so everything is placed and sized reasonably no matter what resolution the game runs at.

# Add accurate ball remaining
For this step, we are making the balls remaining display in the HUD accurate
- Add a value in the configuration data CSV file for the number of balls per game and add the 
appropriate fields and properties to the ConfigurationData and ConfigurationUtils class so 
the HUD class will be able to access this value. 

### HUD script
- Create a HUD script and attach it as a component to the HUD canvas. In the HUD script, add a 
field (marked with [SerializeField]) to hold the balls left Text object (as a GameObject). Populate the field in the Inspector. 
```
#region Fields
[SerializeField]
GameObject ballsLeftTextGameObject; 
#endregion 
```

### Balls left text support
- Add static fields to hold the balls left Text object and the actual balls left.
```
#region Fields
    const string BallsLeftPrefix = "Balls Left: ";
    static int ballsLeft = 0;
    static Text ballsLeftText;
#endregion
```

### LooseBall()
- Add a static public 
method that a ball can call to tell the HUD it was lost (left the bottom of the screen). In the body 
of that method, update both the balls left and the balls left text that's displayed. 
```
#region Public method
public static void LoseBall()
    {
        ballsLeft--;
        ballsLeftText.text = BallsLeftPrefix + ballsLeft.ToString();
    }
#endregion
```

### Start method to populate the ball left value
- Add code to the Start method to populate the balls left value and Text field. The balls left value 
starts as the number of balls per game. We can initialize the Text field by getting the Text 
component attached to the balls left Text game object field we populated in the Inspector. You 
should also set the text to reflect the correct number of balls left here. 
```
#region Unity method
void Start()
    {
        ballsLeft = ConfigurationUtils.BallsPerGame;
        ballsLeftText = ballsLeftTextGameObject.GetComponent<Text>();
        ballsLeftText.text = BallsLeftPrefix + ballsLeft.ToString();
    }
#endregion
```

### Ball OnBallBecameInvisible
- Add code to the Ball OnBecameInvisible method to call the new HUD method as appropriate. 
Don't worry that the balls left can go negative; we won't see that when we add functionality to 
end the game when that value gets to 0. 
```
#region Unity methods
void OnBecameInvisible()
    {
        // death timer destruction is in Update
        if (!deathTimer.Finished)
        {
            // only spawn a new ball if below screen
            float halfColliderHeight =
                gameObject.GetComponent<BoxCollider2D>().size.y / 2;
            if (transform.position.y - halfColliderHeight < ScreenUtils.ScreenBottom)
            {
                HUD.LoseBall();
                Camera.main.GetComponent<BallSpawner>().SpawnBall();
            }
            Destroy(gameObject);
        }
    }
#endregion
```

# Add accurate scoring 
For this step, you are adding scoring functionality to the game
### Score text object
- In the HUD script, add a field (marked with [SerializeField]) to hold the score Text object (as a 
GameObject). Populate the field in the Inspector. 
```
#region Fields
[SerializeField]
GameObject scoreTextGameObject;
#endregion
```
- Add static fields to the HUD script to hold the score Text object and the actual score. 
```
#region Fields
const string ScorePrefix = "Score: ";
static int score = 0;
static Text scoreText;
#endregion
```

- Add a static public method that a block can call to tell the HUD to add points to the score. In the body of that method, update both the score and the score text that's displayed.
```
#region public method
public static void AddPoints(int points)
    {
        score += points;
        scoreText.text = ScorePrefix + score.ToString();
    }
#endregion
``` 

### Populate score in start method
- Add code to the Start method to populate the score value and Text field. The score should start at 0. We can initialize the Text field by getting the Text component attached to the score Text game object field we populated in the Inspector. You should also set the text to reflect the correct score here.
```
# region Unity methods
score = 0;
scoreText = scoreTextGameObject.GetComponent<Text>();
scoreText.text = ScorePrefix + score.ToString();
#endregion
``` 
### Block script
- Add a private field to the Block script to hold how many points the block is worth 
```
# region Private Field
    int points;
#endregion
```
- Add a protected property with a set accessor so child classes can set the value of the field.
```
#region Protected properties
protected int Points
{
    set{points = value;}
}
#endregion 
```

### Call new HUD method
- Add code to the Block script to call the new HUD method just before the block is destroyed. 
This isn't an optimal object-oriented solution because Block objects shouldn't really have to 
know about the existence of the HUD or the methods it exposes, but it's a reasonable solution 
given the C# knowledge we have at this point. Don't worry, we'll make this much better once we 
know about delegates and event handling. 
```
void OnCollisionEnter2D(Collision2D coll)
    {
        if (coll.gameObject.CompareTag("Ball"))
        {
            HUD.AddPoints(points);
            Destroy(gameObject);
        }
    }
```

### Standard block points 
- Add a value in the configuration data CSV file for the Standard block points (I used 1 for this value) and add the appropriate fields and properties to the ConfigurationData and 
ConfigurationUtils class so the StandardBlock class (see next paragraph) will be able to 
access this value. 
```
int standardBlockPoints = 1;
public int StandardBlockPoints 
{
    get {return standardBlockPoints;}
}
```

### StandardBlock script
- Create a new StandardBlock script as a child class of the Block class. Add fields for the standard block sprites, marking each field with [SerializeField] so you can populate it in the 
Inspector.
```
#region Fields

    [SerializeField]
    Sprite sprite0;
    [SerializeField]
    Sprite sprite1;
    [SerializeField]
    Sprite sprite2;

 #endregion
```
- Add code to the StandardBlock Start method to set the points the block is worth to 
the Standard block value using the appropriate ConfigurationUtils property
```
Points = ConfigurationUtils.StandardBlockPoints;
```

- Also add code to this method to randomly select one of the standard block sprites for this block. 
```
SpriteRenderer spriteRenderer = GetComponent<SpriteRenderer>();
int spriteNumber = Random.Range(0, 3);
if (spriteNumber == 0)
{
    spriteRenderer.sprite = sprite0;
}
else if (spriteNumber == 1)
{
    spriteRenderer.sprite = sprite1;
}
else
{
    spriteRenderer.sprite = sprite2;
}
```



