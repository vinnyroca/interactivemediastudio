---
layout: page
title: Dialog with Ink
permalink: /Tutorials/32_dialog/
---

# Dialog with Ink

In this tutorial, we will learn how to add dialog to our project. Through this process, we will learn how to create dialog using Ink and Inky, as well as how to connect this dialog to our user interface.

## What is Ink

Ink is a scripting language that we can use to write branching dialog. Ink allows us to easily write narratives, and then process those narratives within Unity to add interactive dialog to our game. 

More information on Ink and Basic Tutorials, can be found [here](https://www.inklestudios.com/ink/web-tutorial/). To begin working with Ink, we need to download the Inky editor, this will allow us to write stories using Ink.

You can download the Inky editor [here](https://www.inklestudios.com/ink/). I would recommend moving your Inky editor to your hard drive so you can access it while moving your project around. 

While our template currently has ink-Unity integration installed, in future project, to use ink with Unity, you'll need to download this [Unity Package](https://github.com/inkle/ink-unity-integration) and bring it into your Unity project.

## Inky

Copy and Paste this brief story into the Inky editor to see how ink works:

```
->frog

===frog===

Frog: Do not be alarmed, I am a talking frog.

*[Judgemental Response] You: This is against nature and not normal.
*[Happy Response] You: I am not alarmed, this is totally normal.

-Frog: It's not normal! I've been bewitched! God, the horrors of conciousness!


* [Offer to Help]-> help
* Well, good luck with that... -> END

= help
You: Well perhaps there is something I can do to help?

Frog: hmm, now that you mention it you could try to bargin with the wizard that bewtiched me.

* [NO] You: No, I'm alright -> END
* [YES] You: Okay, where can I find them?

- Frog: Amazing! Head to the woods.

* [okay] You: Okay, I hope to save you soon -> crossroad

= crossroad

* Head to the Woods ->woods
* Head to the River ->river

= river

There is nothing here -> crossroad

=woods

* knock on Wizard's door

- You: Hello, I am here to see the Wizard.
Wizard: What are you here about?

* I am here about my friend, the frog

- Ah, I see, I am not really a Wizard. In fact, I am the ever changing invisble hand of the market.

* huh...

- You see, it was time for that Frog to have language, the market demanded it. Sorry, there is nothing I can do..

* [Leave] You: Fine, whatever -> END
* [Banish the Hand of the Market] You: Hah! You are no longer invisible, I see you "Hand of the Market" BE GONE!

- The invisible hand vanishes and everyone is happy!

->END
```

Within Inky we can make branching stories. To write these stories, we need to learn some basic ink syntax.

- ` ->END`: Required in every story, points to story end
- `=== name ===` : A  knot, this marks a different dialog within are larger story
- `= subName`: Marks a stitch, a part in a story you can navigate to (navigate to a stich using `-> stichName`)
- `*Normal option` or `*[Normal Option]` or `*[Normal Option] Option Text`: A normal option can only be chosen once and will then disappear from the story
- `+[Sticky Option]`: A sticky option will persist even if it is chosen
- `->name`: A divert, Use arrows to navigate to knot or stitch
- `VAR`: use to create global variables `VAR KEY = false`
- `- Some Text`: Use the dash line to navigate to text after a choice has been made

## Inky in Unity

Using the ink package in, the only thing we need to do to get our ink project in Unity is to save our ink story within our Unity project. Once the ink project is saved, we can create a simple Ink manager.

Ink Story Manager

Let's use this simple script to create a manager that controls our ink story:

```cs
using Ink.Runtime;
using Ink.UnityIntegration;
using TMPro;
using UnityEngine;
using UnityEngine.Events;
using UnityEngine.UI;


public class InkyStoryManager : MonoBehaviour
{
    public static InkyStoryManager instance;

    public InkFile inkFileAsset = null;
    public Story currentStory;

    public bool isStoryActive = false;

    public GameObject storyPanel;
    public TextMeshProUGUI storyText;
    public GameObject choicesPanel;
    public Button choicesButton;

    public Interactor playerInteractor;
    public Player player;

    void Awake()
    {
        if (instance != null && instance != this)
        {
            Destroy(gameObject);
        }
        else
        {
            instance = this;
        }

        storyPanel.SetActive(false);
    }

    private void Start()
    {
        currentStory = new Story(inkFileAsset.storyJson);
    }

    public void StartDialogKnot(string knotName)
    {
        storyPanel.SetActive(true);
        currentStory.ChoosePathString(knotName);
        Display();
        player.enabled = false;
        playerInteractor.enabled = false;
    }

    void Display()
    {
        ClearChoices();
        string tempText = currentStory.Continue();

        storyText.text = tempText.Trim();

        if (currentStory.currentChoices.Count > 0)
        {
            foreach (Choice choice in currentStory.currentChoices)
            {
                Button button = Instantiate(choicesButton, choicesPanel.transform);

                TextMeshProUGUI buttonText = button.GetComponentInChildren<TextMeshProUGUI>();
                buttonText.text = choice.text;

                int index = choice.index;
                button.onClick.AddListener(() => SelectChoice(index));
            }
        }
        else if (currentStory.canContinue)
        {
            Button button = Instantiate(choicesButton, choicesPanel.transform);

            TextMeshProUGUI buttonText = button.GetComponentInChildren<TextMeshProUGUI>();
            buttonText.text = "...";

            button.onClick.AddListener(() => Display());
        }
        else
        {
            Button button = Instantiate(choicesButton, choicesPanel.transform);

            TextMeshProUGUI buttonText = button.GetComponentInChildren<TextMeshProUGUI>();
            buttonText.text = "exit";

            button.onClick.AddListener(() => ExitStory());
        }
    }

    void ExitStory()
    {
        isStoryActive = false;
        storyPanel.SetActive(false);
        ClearChoices();
        player.enabled = true;
        playerInteractor.enabled = true;
    }

    void SelectChoice(int index)
    {
        currentStory.ChooseChoiceIndex(index);
        Display();

    }

    void ClearChoices()
    {
        foreach (Transform child in choicesPanel.transform)
        {
            Destroy(child.gameObject);
        }
    }
}
```

This manager script using the Ink namespace to create a story from our Ink project. Additionally, it looks at our ink file and allows us to play a project from a particular knot using the method `StartDialogKnot()`. This script also displays our text using the UI created within our template project.

To set this dialog up, we need to drag in our story, UI elements and prefabs. Additionally, since this Manager disables the player when we enter into a knot, we also need to connect this to our Player and Interactor.

Let's make sure we add this manager to our Game Manager and update our prefab:

![Animated gif of creating inky dialog manager](/Attachments/31_1.gif)

## Dialog Interactable

Now that we have a way to manage our story. Let's create an interactable that we can use to start our story. Let's begin by making a `DialogInteractable` script and implement the `IInteractable` interface:

```cs
using UnityEngine;
using UnityEngine.Events;
using System.Collections.Generic;

public class DialogInteractable : MonoBehaviour, IInteractable
{
    public void Start()
    {

    }

    public string InteractText()
    {

    }

    public void Interact()
    {

    }
}
```

Then, let's have our `InteractText()` return a dialog name variable. We can also have our `Interact` method call the `StartDialogKnot()` method on our Ink manager. We can provide this method with a variable that we can assign in the inspector.

```cs
using UnityEngine;
using UnityEngine.Events;
using System.Collections.Generic;

public class DialogInteractable : MonoBehaviour, IInteractable
{
    public string dialogName;
    public string knotName;

    public void Start()
    {

    }

    public string InteractText()
    {
        return "Speak with " + dialogName;
    }

    public void Interact()
    {
        InkyStoryManager.instance.StartDialogKnot(knotName);
    }
}
```

It is recommended that you add least one sticky option `+` if you want players to return to a dialog over and over again. If you want players to only speak with an NPC once, you might implement a boolean that checks if an NPC has been spoken with, if so, you could play another ink knot.

To use this script, place it on an object with a collider and provide it with a knot name. Make sure that your knot name has a corresponding name within your ink story.

In this example, let's add this to our Cake object within our Interior Scene. Let's also make sure our Adventure Story ink story looks like the following:

```
===cake===

Cake: Hey, is today your birthday?

*[Lie] -> YesBirthday
*[Truth] -> NoBirthday
+[EXIT] ->END

= YesBirthday
You: Yes! Today is my birthday! 
Cake: Amazing!
Cake: Oh, oh god! I forgot...
Cake: I begin to feel when it is someone's birthday!
Cake: Please tell me you were lying!

*[Truth] You: Yes, I was lying...

Cake: oh, good...
Cake: I am better now.
->END

= NoBirthday
You: No, today is not my birthday 
Cake: Oh, okay, bye.
->END
```

Our cake Dialog Interactable should look like this:

![image of cake dialog interactable ](/Attachments/Pasted%20image%2020261028200755.png)

![Animated image of cake dialog](/Attachments/31_2.gif)

## External Methods

Within ink stories, we can trigger external methods within Unity. In this example, we will create a list of events on our Dialog Interactable. We will then write some code to trigger those events in our text.

Let's set up a system so that our Ink Manager keeps track of a `currentDialog` variable. We can pass our dialog to our manager when we start a knot:

```cs
DialogInteractable currentDialog;
```

```cs
public void StartDialogKnot(string knotName, DialogInteractable dialog)
{
    currentDialog = dialog;
    storyPanel.SetActive(true);
    currentStory.ChoosePathString(knotName);
    Display();
    player.enabled = false;
    playerInteractor.enabled = false;
}
```

Next, let's edit our Dialog Interactable script to contain a list of Unity Events, as well as send over our script when we call `StartDialogKnot`:

```cs
using UnityEngine;
using UnityEngine.Events;
using System.Collections.Generic;

public class DialogInteractable : MonoBehaviour, IInteractable
{
    public string dialogName;
    public string knotName;

    public List<UnityEvent> dialogEvents = new List<UnityEvent>();

    public void Start()
    {

    }

    public string InteractText()
    {
        return "Speak with " + dialogName;
    }

    public void Interact()
    {
        InkyStoryManager.instance.StartDialogKnot(knotName, this);
    }
}
```


Next, let's add a method to our Ink Manager that can call a dialog event based on a given index value:

```cs
public void StoryEvent(int index)
{
    currentDialog.dialogEvents[index].Invoke();
}
```

Finally, we need to bind this `StoryEvent` method to a function within our ink story:

First, let's add the following line to the top of our ink story:

```cs
EXTERNAL event(index)
```

This creates a function (`event()`) that we can call anytime within our story. To bind this to our `StoryEvent()` script in our code, we need to add the following line to our `Start` method after we create our story:

```cs
currentStory.BindExternalFunction("event", (int index) => { StoryEvent(index); });
```

This line looks at the function with the name "event" and also makes sure it has a parameter named "index", which we are declaring as an integer value. We are then using a lamda expression (`=>`). We can think of this expression as the words "go to", meaning that when "event" is called **go to** the StoryEvent method.

## Using External Methods

On our cake object, there is an animator that can switch to a stressed animation by using the trigger "stress".

Let's trigger this animation using an event on our dialog interactable script on our cake:

![Showing event on cake interactable](/Attachments/Pasted%20image%2020261028203228.png)

Then, let's edit our story so that this animator toggles on and off depending on our choices. To call an external method within ink, we use the tilde `~` key before the methods name:

```
EXTERNAL event(index)

===cake===

Cake: Hey, is today your birthday?

*[Lie] -> YesBirthday
*[Truth] -> NoBirthday
+[EXIT] ->END

= YesBirthday
You: Yes! Today is my birthday! 
Cake: Amazing!
Cake: Oh, oh god! I forgot...
~ event(0)
Cake: I begin to feel when it is someone's birthday!
Cake: Please tell me you were lying!

*[Truth] You: Yes, I was lying...

Cake: oh, good...
~ event(0)
Cake: I am better now.
->END

= NoBirthday
You: No, today is not my birthday 
Cake: Oh, okay, bye.
->END
```

![Animated gif of cake changing animation based on dialog choices](Attachments/31_3.gif)