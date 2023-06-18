# Refactor ball death processing
For this step, you are refactoring how we handle a ball being destroyed because its death timer has finished to use an event invoker and an event listener instead.

### EventManager script
- Create a new Events folder in the Scripts folder in the Project window. Create a new EventManager script in your new folder. Open the EventManager script in Visual Studio and change it to a static class that doesn’t inherit from MonoBehaviour. Add a documentation 
comment for the class and add a using directive for the UnityEngine.Events namespace. Delete the Start and Update methods from the class. 

```
Using UnityEngine.Events;

public static class EventManager
{

}
```

### BallDiedEvent script
- Create a BallDiedEvent script in the Scripts/Events folder in the Project window and make it a UnityEvent with no parameters.
```
using UnityEngine.Event
public class BallDiedEvent: UnityEvent
{

}
``` 

### Ball class
- We know the Ball class is the class that will invoke this event, so add the appropriate field to the Ball class, making sure you assign a new BallDiedEvent object to the field to initialize it
```
public class Ball : MonoBehaviour 
{
    #region Fields
    // death support 
    Timer deathTImer
    BallDiedEvent ballDiedEvent = new BallDiedEvent()
    #endregion
}
```
- Add an AddBallDiedListener method to the class that adds a listener to the event. 
```
public void AddBallDiedListener(UnityAction listener)
{
    ballDiedEvent.AddListener(listener);
}
```

### EventManager
- Add support to the EventManager for lists of listeners and invokers for the new event. In addition to the fields you need to add, be sure to include the following methods: 
AddBallDiedInvoker, AddBallDiedListener, and RemoveBallDiedInvoker.
```
    #region Ball died support

    /// <summary>
    /// Adds the given script as a ball died invoker
    /// </summary>
    /// <param name="invoker">invoker</param>
    public static void AddBallDiedInvoker(Ball invoker)
    {
        // add invoker to list and add all listeners to invoker
        ballDiedInvokers.Add(invoker);
        foreach (UnityAction listener in ballDiedListeners)
        {
            invoker.AddBallDiedListener(listener);
        }
    }

    /// <summary>
    /// Adds the given method as a ball died listener
    /// </summary>
    /// <param name="listener">listener</param>
    public static void AddBallDiedListener(UnityAction listener)
    {
        // add listener to list and to all invokers
        ballDiedListeners.Add(listener);
        foreach (Ball invoker in ballDiedInvokers)
        {
            invoker.AddBallDiedListener(listener);
        }
    }

    /// <summary>
    /// Remove the given script as a ball died invoker
    /// </summary>
    /// <param name="invoker">invoker</param>
    public static void RemoveBallDiedInvoker(Ball invoker)
    {
        // remove invoker from list
        ballDiedInvokers.Remove(invoker);
    }

    #endregion

``` 

### Ball class
- Go back to the Ball class and add code to add it to the EventManager as an invoker of the event. 
```
void Start()
{
    // add as invoker of events
    EventManager.AddBallDiedInvoker(this);
    EventManager.AddBallLostInvoker(this);
}
```




