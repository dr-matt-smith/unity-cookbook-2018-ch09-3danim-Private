# 2D Animation

In this chapter, we will cover:

1. Flipping a sprite horizontally: the Do-It-Yourself approach

1. Flipping a sprite horizontally: using Animator State Chart and Transitions

2. Animating body parts for character movement events

3. Creating a 3-frame animation clip to make a platform continually animate

4. Making a platform start falling once stepped-on using a Trigger to move animation from one state to another

5. Creating animation clips from sprite sheet sequences

6. Creating a platform game with Tiles and Tilemaps

7. Creating a game with the 2D Gamekit

<!-- ******************************** -->
<!-- ******************************** -->

# Introduction

Since Unity 4.6 in 2014 Unity has shipped with dedicated 2D features, and Unity 2018 continues to build on these. In this chapter, we present a range of recipes to introduce the basics of 2D animation in Unity 2018, and help you understand the relationships between the different animation elements.


<!-- ******************************** -->
<!-- ******************************** -->

# The big picture

In Unity 2D animations can be created in several different ways – one way is to create many images, each slightly different, which frame-by-frame give the appearance of movement. A second way to create animations is by defining keyframe positions for individual parts of an object (for example, the arms, legs, feet, head, eyes, and so on), and getting Unity to calculate all the in-between positions when the game in running.

![Insert Image B08775_08_21.png](./08_figures/B08775_08_21.png)

![Insert Image B08775_08_33.png](./08_figures/B08775_08_33.png)


Both sources of animations become Animation Clips in the Animation panel. Each Animation Clip then becomes a State in the Animator Controller State Machine. We then define under what conditions a GameObject will Transition from one animation state (clip) to another.

## Resources

In this chapter, we introduce recipes demonstrating the animation system for 2D game elements. The potato-man2D character is from the Unity 2D Platformer, which you can download yourself from the Unity asset store. That project is a good place to see lots more examples of 2D game and animation techniques (www.assetstore.unity3d.com/en/#!/content/11228).

![Insert Image B08775_08_03.png](./08_figures/B08775_08_03.png)


Here are some links for useful resources and sources of information to explore these  topics further:

- Overview of 2D features in Unity:

    - https://unity.com/solutions/2d

- Unity's beginners walkthrough guide to 2D game development:

    - https://unity3d.com/learn/tutorials/topics/2d-game-creation/2d-game-development-walkthrough

- Unity's 2D Rogue-like tutorial series:

    - https://unity3d.com/learn/tutorials/s/2d-roguelike-tutorial

- Unity 2D Platformer (where the PotatoMan character came from):

    - https://www.assetstore.unity3d.com/en/#!/content/11228

- The platform sprites are from Daniel Cook's Planet Cute game resources:

    - http://www.lostgarden.com/2007/05/dancs-miraculously-flexible-game.html

- Creating a basic 2D platformer game:

    - https://www.unity3d.com/learn/tutorials/modules/beginner/live-training-archive/creating-a-basic-platformer-game

- Hat Catch 2D game tutorial:

    - https://www.unity3d.com/learn/tutorials/modules/beginner/live-training-archive/2d-catch-game-pt1

- Unity games from a 2D perspective video:

    - https://www.unity3d.com/learn/tutorials/modules/beginner/live-training-archive/introduction-to-unity-via-2d

- A fantastic set of modular 2D characters with a free Creative Commons license from 'Kenny'. These assets would be perfect for animating body parts in a similar way to the potato-manexample in this chapter and in the Unity 2D platformer demo:

    - http://kenney.nl/assets/modular-characters

- Joe Strout's illuminating Gamasutra article on 3 approaches to 2D character animation with Unity's scriping and animation states:

    - https://www.gamasutra.com/blogs/JoeStrout/20150807/250646/2D_Animation_Methods_in_Unity.php



<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

# Flipping a sprite horizontally: the Do-It-Yourself approach

Perhaps the simplest 2D animation is a simple flip, from facing left to facing right, or facing up to facing down, and so on. In this recipe we'll add a cute bug sprite to the scene, and write a short script to flip its horizontal direction when the Left and Right arrow keys are pressed.

![Insert Image B08775_08_04.png](./08_figures/B08775_08_04.png)

<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

For this recipe, we have prepared the image you need in a folder named Sprites in folder 1362_03_01.

<!-- ******************************* -->
<!-- ******************************* -->

## How to do it...

To flip an object horizontally with arrow key presses, follow these steps:

1. Create a new Unity 2D project.

    NOTE: If you are working in a project that was originally created 3D, you can change the default project behaviour (e.g. Sprite Textures and Scene mode) to 2D via menu: Edit | Project Settings | Editor, then choosing 2D for the Default Behavior Mode in the Inspector.

    ![Insert Image B08775_08_34.png](./08_figures/B08775_08_34.png)

1. Import the provided image EnemyBug.png.

1.Drag an instance of the red Enemy Bug image from the Project | Sprites folder  into the Scene. Position this GameObject at (0, 0, 0) and scale to (2, 2, 2).

1. Create a C# script-class named BugFlip, and add an instance-object as a component to GameObject Enemy  Bug:

    ```csharp
        using UnityEngine;
        using System.Collections;

        public class BugFlip : MonoBehaviour {
          private bool facingRight = true;

          void Update() {
            if (Input.GetKeyDown(KeyCode.LeftArrow) && facingRight)
              Flip ();
            if (Input.GetKeyDown(KeyCode.RightArrow) && !facingRight)
              Flip();
          }

          void Flip (){
            // Switch the way the player is labelled as facing.
            facingRight = !facingRight;

            // Multiply the player's x local scale by -1.
            Vector3 theScale = transform.localScale;
            theScale.x *= -1;
            transform.localScale = theScale;
          }
        }
    ```

1. When you run your scene, pressing the Left and Right arrow keys should make the bug face left or right correspondingly.

<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

The C# class defines a Boolean variable facingRight, which stores a true/false value corresponding to whether or not the bug is facing right or not. Since our bug sprite is initially facing right, then we set the initial value of facingRight to true to match this.

Method Update(), every frame, checks to see if the Left or Right arrow keys have been pressed. If the Left arrow key is pressed and the bug is facing right, then method Flip() is called, likewise if the Right arrow key is pressed and the bug is facing left (that is, facing right is false), again method Flip() is called.

Method Flip() performs two actions, the first simply reverses the true/false value in variable facingRight. The second action changes the +/- sign of the X-value of the localScale property of the transform. Reversing the sign of the localScale results in the 2D flip that  we desire. Look inside the PlayerControl script for the PotatoMan character in the next recipe – you'll see exactly the same Flip() method being used.





to create a single-frame animation:
To do this first make sure that you have the Animation window open under > Window > Animation. Then select your player gameobject and hit create. That will create an animator component, and a animation that you can save in the appropriate folder (/Animations). Make sure your player has a sprite renderer component, and click on Add Property > Toggle the Sprite Renderer drop down > Hit the plus sign on the right of "Sprite". The diamond in the animation view represents the initial frame of the animation. If you leave it just one frame it will be a single frame animation which is what you are looking for. Simply drag your desired sprite into the animator and on top of the diamond. Or change it in editor under your sprite renderer, but make sure when you are done to uncheck the record button. Else it will save any changes you make to the game object in it's animation. To make the other 7 sprite animations look at the top left of the window, find the drop down that is currently selecting your current animation, and hit that > create new clip and continue the process



<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

# Flipping a sprite horizontally: using Animator State Chart and Transitions

In this recipe we'll use (in a simple way) the Unity animation system, to create 2 states corresponding to 2 animation clips, and a script that changes the localScale according to which animation state is active. We'll use a second script, that will map the arrow keys press Horizontal input axis values to a parameter in the state chart, and which will drive the transition from one state to the other.

While it may see a lot of work, compared to the previous recipe, such an approach illustrates how we can map from input events (such as key presses or touch inputs), to paramters and triggers in a state chart.

<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

For this recipe, we have prepared the image you need in a folder named Sprites in folder 1362_03_01.

<!-- ******************************* -->
<!-- ******************************* -->

## How to do it...

To flip an object horizontally using Animator State Chart and Transitions, follow these steps:

1. Create a new Unity 2D project.

1. Import the provided image EnemyBug.png.

1. Drag an instance of the red Enemy Bug image from the Project | Sprites folder  into the scene. Position this GameObject at (0, 0, 0) and scale to (2, 2, 2).

1. With GameObject Enemy Bug selected in Hierarchy, open the Animation panel (menu: Window | Animation | Animation), and click the Create button to create a new Animation Clip asset. Save the new Animation Clip asset as beetle-right. You will also see that an Animator component has also been added to the Enemy Bug GameObject:

    ![Insert Image B08775_08_35.png](./08_figures/B08775_08_35.png)

1. If you look at the Project panel, you'll see 2 new asset files have been created: the beetle-right Animation Clip, and also an Animator Controller named Enemy Bug:

    ![Insert Image B08775_08_36.png](./08_figures/B08775_08_36.png)

1. Close the Animation panel, and double click the Enemy Bug Animator Controller to start editing it - it should appear in a new Animator panel. You should see four *states*, Any State and Exit are unlinked, and state Entry has a Transition arrow connecting to Animation Clip beetle-right. This means that as soon as the Animator Controller starts to play, it will enter the beetle-right state. State beetle-right is tinted orange, to indicate that it is the Default state.

    NOTE: If there is only one Animation Clip state, that will be the Default state automatically. Once you have other states added to the state chart, you can right-click a different state and use the context menu to change which state is first entered.

    ![Insert Image B08775_08_38.png](./08_figures/B08775_08_38.png)

1. Select state beetle-right, and make a copy of it, renaming the copy beetle-left (use can use the right-mouse menu, or the CTRL-C/V keyboard shortcuts). It makes sense to position beetle-left to the **left** of beetle-right.

    ![Insert Image B08775_08_39.png](./08_figures/B08775_08_39.png)

1. Move your mouse pointer over state beetle-right, and then the mouse right-click context menu, choose Make Transition, and drag the white arrow that appears into state beetle-left.

    ![Insert Image B08775_08_40.png](./08_figures/B08775_08_40.png)

1. Repeat this step with beetle-left, to create a Transition back from beetle-left to beetle-right.

    ![Insert Image B08775_08_41.png](./08_figures/B08775_08_41.png)

1. We want an instance Transition between left and right facing. So for **each** Transition uncheck the Has Exit Time option: click the Transition arrow to select it (it should turn blue), then uncheck this option in the Inspector:

    ![Insert Image B08775_08_42.png](./08_figures/B08775_08_42.png)

    NOTE: To delete a Transition, first select it, then use the Delete key (Windows) or press Fn+Backspace (Mac OS).

1. For our condition to decide when to change the active state we now need to create a parameter indicating if the Left/Right arrow keys have been clicked. Left/Right keys presses are indicated by the Unity input system's Horizontal axis value. Create a state chart float parameter named axisHorizontal by selecting Parameters (rather than Layers) in the top-left of the Animator panel, clikcing the plus-symbol "+" button, and choosing Float. Name your new parameter axisHorizontal.

    ![Insert Image B08775_08_40.png](./08_figures/B08775_08_40.png)

1. Now we have our Parameter, we can define the conditions for changing between the left- and right- facing states. When the Left array key is pressed, the Unity input system's Horizontal axis value is negative, so select the Transition from beetle-right to beetle-left, and in the Inspector click the plus-symbol in the Conditions section of the Transition properties. Since there is only one Parameter, this will automatically be suggested, with defaults of Greater than zero. Change the Greater to Less, and we have our desired condition:

    ![Insert Image B08775_08_43.png](./08_figures/B08775_08_43.png)

1. Now select the Transition from beetle-left to beetle-right, and add a Condition. In this case the defaults, axisHorizontal Greater than zero are just what we want (since a positive value is returned by Unity's input system Horizontal axis when the Right arrow key is pressed).

1. We need a method to actually map from the Unity input system's Horizontal axis value to our Animator state chart Parameter axisHorizontal. This we can do with short script-class, which we'll create in the next steps.

1. Now we need to map from the Left/Right-array keys to values for the axisHoriztonal parameter in the Animator Controller. Create a C# script-class named InputMapper, and add an instance-object as a component to GameObject Enemy Bug:

    ```csharp
        using UnityEngine;

        public class InputMapper : MonoBehaviour
        {
        	Animator animator;

        	void Start() {
        		animator = GetComponent<Animator>();
        	}

        	void Update() {
        		animator.SetFloat("axisHorizontal", Input.GetAxisRaw("Horizontal"));
        	}
        }
    ```


1. Now we need to actually change the local scale property of the GameObject when we switch to the left or right facing state. Create a C# script-class named LocalScaleSetter:

    ```csharp
        using UnityEngine;

        public class LocalScaleSetter : StateMachineBehaviour  {
        	public Vector3 scale = Vector3.one;

        	override public void OnStateEnter(Animator animator, AnimatorStateInfo stateInfo, int layerIndex) {
        		animator.transform.localScale = scale;
        	}

        }
    ```

1. In the Animator panel select state beetle-right. Now in the Inspector cluck the Add Behaviour button, and select LocalScaleSetter. The default public Vector 3 scale value of (1,1,1) is fine for this state.

1. In the Animator panel select state beetle-left. Now in the Inspector cluck the Add Behaviour button, and select LocalScaleSetter. Change the public Vector 3 scale to a value of (-1,1,1) - i.e. we need to swap the X-scaling to make our Sprite face to the left.

    ![Insert Image B08775_08_46.png](./08_figures/B08775_08_46.png)

    NOTE: Adding instance-objects of C# script-classes to Animator states is a great way to link the logic for actions when entering/in/exiting a state, with the Animator states themselves.

1. In the Animator panel select state beetle-right. Now in the Inspector cluck the Add Behaviour button, and select InputMapper.

1. When you run your Scene, pressing the Left and Right arrow keys should make the bug face left or right correspondingly.

<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

C# script-class InputMapper reads the Unity input system's Horizontal axis value each frame, and set the Animator state chart Parameter axisHorizontal to this value. If the value is less than (left arrow) or greater than (right arrow) zero, then if appropriate the Animator state system will switch to the other state.

C# script-class LocalScaleSetter actually changes the localScale property (normal 1,1,1, or left -1,1,1). The public Vector3 variable for this script-class can be customized the instance on each state. The method OnStateEnter(...) is involved each time the state that an instance-object of this C# class is attacehd to is entered. You can read about the various event messages for the StateMachineBehaviour class at:

    - https://docs.unity3d.com/ScriptReference/StateMachineBehaviour.html

When we press the Left-arrow key, the value of Unity input system's Horizontal axis value is negative, and this is mapped to the Animator state chart Parameter axisHorizontal, causing the system to Transition to state beetle-left, and the OnStateEnter(...) of script-class LocalScaleSetter instance to be executed, setting the local scale to (-1, 1, 1), making the Texture flip Horiztonally, so the beetle faces left.

<!-- ******************************** -->
<!-- ******************************** -->

## There's more...

Here are some suggestions for enhancing this recipe.

<!-- ******************************** -->
<!-- ******************************** -->

## Instantanous swapping

You may have noticed a delay, even though we set Exit Time to zero. This is because there is a default blending when Transitioning from one state to anther. However, this can be set to zero, so that the state machine switches instantaneously from one state to the next.

Do the following:

1. Select each Transition in the Animator panel.

1. Expand the Settings properties.

1. Zero both the Transition Duration and the Transition Offset.

    ![Insert Image B08775_08_45.png](./08_figures/B08775_08_45.png)

Now when you run the Scene, the bug should immediately swith left and right as you press the corresponding arrow keys.

<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->



#  Animating body parts for character movement events

In this recipe, we'll learn to animate the hat of the Unity potato-man character in response to a jump event.

<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

For this recipe, we have prepared the files you need in folder 08_03.

<!-- ******************************* -->
<!-- ******************************* -->

## How to do it...

To animate body parts for character movement events, follow these steps:

1. Create a new Unity 2D project.

1. Import the provided package PotatoManAssets into your project.

1. Increase the size of the Main Camera to 10.

1. Let's setup the 2D gravity setting for this project – we'll use the same setting as from Unity's 2D platform tutorial, a setting of Y= -30. Set 2D gravity to this value by choosing menu: Edit | Project Settings | Physics 2D, and then at the top change the Y value to -30.

    ![Insert Image B08775_08_47.png](./08_figures/B08775_08_47.png)

1. Drag an instance of the PotatoMan hero character2D from the Project | Prefabs folder into the Scene. Position this GameObject at (0, 3, 0).

1. Drag an instance of the sprite platformWallBlocks from the Project | Sprites folder into the Scene. Position this GameObject at (0, -4, 0).

1. Add a Box Collider 2D component to GameObject platformWallBlocks by choosing menu: Add Component | Physics 2D | Box Collider 2D.

1. We now have a stationary platform that the player can land upon, and walk left and right on. Create a new Layer named Ground, and assign GameObject platformWallBlocks to this new layer, as shown in the following screenshot. Pressing the Space key when the character is on the platform will now make him jump.

    ![Insert Image B08775_08_05.png](./08_figures/B08775_08_05.png)

1. Currently the PotatoMan character is animated (arms and legs moving) when we make him jump. Let's remove the Animation clips and Animator controller and create our own from scratch. Delete folders Clips and Controllers from Project | Assets | PotatoMan2DAssets | Character2D | Animation, as shown:

    ![Insert Image B08775_08_22.png](./08_figures/B08775_08_22.png)

1. Let's create an Animation clip (and its associated Animator controller) for our hero character. In the Hierarchy select GameObject hero. Ensuring GameObject hero character2D is selected in the Hierarchy, open the Animation panel, and ensure it  is in Dope Sheet view (this is the default).

1. Click the Animation panel Create button, and save the new clip in the Character2D | Animation folder, naming it as character-potatoman-idle. You've now created an Animation clip for the 'idle' character state (which is not animated).

    ![Insert Image B08775_08_48.png](./08_figures/B08775_08_48.png)

    - NOTE:Your final game may end up with tens, or even hundreds, of animation clips. Make things easy to search by prefixing the names of clips with object type, name, and then description of the animation clip.

1. Looking at the Character2D | Animation folder in the Project panel you should  now see both the Animation clip you have just created (character-potatoman-idle)  and also a new Animator controller, which has defaulted to the name of your GameObject hero character2D:

    ![Insert Image B08775_08_23.png](./08_figures/B08775_08_23.png)

1. Ensuring GameObject hero is selected in the Hierarchy, open the Animator panel and you'll see the State Machine for controlling the animation of our character. Since we only have one Animation clip (character-potatoman-idle) then upon entry the State Machine immediately enters this state.

    ![Insert Image B08775_08_24.png](./08_figures/B08775_08_24.png)

1. Run your scene – since the character is always in the 'idle' state, we see no animation yet when we make it jump.

1. Now we'll create a 'jump' Animation clip which animates the hat. First, ensure that GameObject hero is still selected the Hierarchy. Next, click the empty dropdown menu in the Animation panel (next to the word 'Samples'), and create a new clip in your Animation folder, naming it character-potatoman-jump.

    ![Insert Image B08775_08_49.png](./08_figures/B08775_08_49.png)

1. Click button Add Property, and chose Transform | Position of the hat child object, by clicking its '+' plus-sign button. We are now ready to record changes to the (X, Y, Z) position of GameObject hat in this animation clip:

    ![Insert Image B08775_08_25.png](./08_figures/B08775_08_25.png)

1. You should now see 2 'keyframes' at 0.0 and at 1.0. These are indicated by diamonds in the Timeline area in the right-hand-section of the Animation panel.

1. Click to select the first keyframe (at time 0.0) - the diamond should turn blue to indicate it is selected.

1. Let's record a new position for the hat for this first frame. Click the red Record circle button once to start recording in the Animation panel. Now in the Scene panel move the hat up and left a little, away from the head. You should see that all three X, Y, Z values have a red background in the Inspector – this is to inform you that the values of the Transform component are being recorded in the animation clip:

    ![Insert Image B08775_08_26.png](./08_figures/B08775_08_26.png)

1. Click the red Record circle button again to stop recording in the Animation panel.

1. Since 1 second is perhaps too long for our jump animation, drag the second keyframe diamond to the left to a time of 0.5.

    ![Insert Image B08775_08_50.png](./08_figures/B08775_08_50.png)

1. We now need to define when the character should Transition from the 'idle' state to the 'jump' state. In the Animator panel select state character-potatoman-idle, and create a transition to the state character-potatoman-jump by right-mouse-clicking and choosing menu Make Transition, then drag the transition arrow to state character-potatoman-jump, as shown:

    ![Insert Image B08775_08_27.png](./08_figures/B08775_08_27.png)

1. Now let's add a Trigger parameter named 'Jump', by clicking on the add parameter plus-sign "+" button at the top-left of the Animator panel, choosing Trigger, and typing the name Jump:

    ![Insert Image B08775_08_28.png](./08_figures/B08775_08_28.png)

1. We can now define the properties for when our character should Transition from idle to jump. Click the Transition arrow to select it, and set the following 2 properties and add 1 condition in the Inspector panel:

    - Has Exit Time: uncheck this option

    - Transition Duration (s): set to 0.01

    - Conditions: Add Jump (click plus-sign '+' button at bottom)

    ![Insert Image B08775_08_29.png](./08_figures/B08775_08_29.png)


1. Save and run your scene. Once the character has landed on the platform and you press the SPACE key to jump, you'll now see the character's hat jump away from his head, and slowly move back. Since we haven't added any transition to ever leave the Jump state, this Animation clip will loop, so the hat keeps on moving even when the jump is completed.

1. In the Animator panel select state character-potatoman-jump and add a new Transition back to state character-potatoman-idle. Select this Transition arrow  and in the Inspector panel sets its properties as follows:

    - Has Exit Time: (leave as checked)

    - Exit time: 0.5 (this needs to be the same time value as the second keyfame of our Jump animation clip)

1. Save and run your scene. Now when you jump the hat should animate once, after which the character immediately returns to its Idle state.

<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

You have added an Animation controller State Machine to GameObject hero. The two Animation clips you created (idle and jump) appear as States in the Animator panel. You created a Transition from Idle to Jump when the 'Jump' Trigger parameter is received by the State Machine. You created a second Transition, which transitions back to the Idle state after waiting 0.5 seconds (the same duration between the 2 keyframes in our Jump Animation clip).

Note that the key to everything working for the potato-man character is that when we make  the character jump with the SPACE key, then code in the PlayerControl C# scripted component of GameObject hero, as well as making the sprite move upwards on screen, also sends a SetTrigger(…) message to the Animator controller component,  for the Trigger named Jump.

The difference between a Boolean Parameter and a Trigger is that a Trigger is temporality set to True and once the SetTrigger(...) event has been 'consumed' by a state transition it automatically returns to being False. So Triggers are useful for actions we wish to do once and then revert to a previous state. A Boolean Parameter is a variable, which can have its value set to true/or False at different times during the game, and so different Transitions can be created to fire depending on the value of the variable at any time. Note that Boolean parameters have to have their values explicitly set back to False with a SetBool(…).

The following screenshot highlights the line of code that sends the SetTrigger(...) message:

![Insert Image B08775_08_30.png](./08_figures/B08775_08_30.png)

State Machines for animations of a range of motions (running/walking/jumping/falling/dying and so on.) will have more states and transitions. The Unity-provided potato-man character has a more complex State Machine, and more complex animations (of hands and feet, and eyes and hat and so on, for each Animation clip), which you may find useful to explore.

Learn more about the Animation view on the Unity Manual web pages at http://docs.unity3d.com/Manual/AnimationEditorGuide.html.

<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

# Creating a 3-frame animation clip to make  a platform continually animate

In this recipe, we'll make a wooden-looking platform continually animate, moving upwards and downwards. This can be achieved with a single, 3-frame, animation clip (starting at top, position at bottom, top position again).

![Insert Image B08775_08_02.png](./08_figures/B08775_08_02.png)

<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

This recipe builds on the previous one, so make a copy of that project, and work on the copy for this recipe.

<!-- ******************************* -->
<!-- ******************************* -->

## How to do it...

To create a continually moving animated platform, follow these steps:

1. Drag an instance of the sprite platformWoodBlocks from the Project | Sprites folder into the scene. Position this GameObject at (-4, -5, 0), so that these wood blocks are neatly to left, and slightly below, the wall blocks platform.

1. Add a Box Collider 2D component to GameObject platformWoodBlocks so that the player's character can stand on this platform too. Choose menu: Add Component | Physics 2D | Box Collider 2D.

1. Create a new folder named Animations, in which to store the animation clip and controller we'll create next.

1. Ensuring GameObject platformWoodBlocks is still selected in the Hierarchy, open an Animation panel, and ensure it is in Dope Sheet view (this is the default).

1. Click the Animation panel Create button, and save the new clip in your new Animation folder, naming it as platform-wood-moving-up-down.

1. Click button Add Property, and choose Transform and the click the '+' plus-sign by Position. We are now ready to record changes to the (X, Y, Z) position of GameObject platformWoodBlocks in this animation clip.

    ![Insert Image B08775_08_51.png](./08_figures/B08775_08_51.png)

1. You should now see 2 'keyframes' at 0.0 and at 1.0. These are indicated by diamonds in the Timeline area in the right-hand-section of the Animation panel.

1. We need 3 keyframes, with the new one at 2:00 seconds. Click at 2:00 in the Timeline along the top of the Animation panel, so that the red line for the current playhead time is at time 2:00. Then click diamond+ button to create a new keyframe at the current playhead time:

    ![Insert Image B08775_08_08.png](./08_figures/B08775_08_08.png)

1. The first and third keyframes are fine – they record the current height of the wood platform at Y= -5. We need to make the middle keyframe record the height of the platform at the top of its motion, and Unity in-betweening will do all the rest of the animation work for us.

1. Select the middle keyframe (at time 1:00), by clicking on either diamond at time 1:00 (they should both turn blue, and the red playhead vertical line should move to 1:00, to indicate the middle keyframe is being edited).

1. Click the red Record cirle button to start recording changes.

1. Now in the Inspector change the Y position of the platform to 0. You should see that all three X, Y, Z values have a red background in the Inspector – this is to inform you that the values of the Transform component are being recorded in the animation clip.

1. Click the red Record cirle button again, to end recording changes.

1. Save and run your scene. The wooden platform should now be animating continuously, moving smoothly up and down the positions we setup.

NOTE: If you want the potatoman character to be able to jump when on the moving wooden block, you'll need to select the block GameObject and set its layer to Ground.

<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

You have added an animation to GameObject platformWoodBlocks. This animation contains three keyframes. A keyframe represents the values of properties of the object at a point in time. The first keyframe stores a Y-value of -4, the second keyframe a Y-value of 0, and the final keyframe -4 again. Unity calculates all the in-between values for us, and the result is a smooth animation of the Y-position of the platform.


<!-- ******************************** -->
<!-- ******************************** -->

## There's more...

Here are some suggestions for enhancing this recipe.

<!-- ******************************** -->
<!-- ******************************** -->

## Copy animation relative to a new parent GameObject

If we wanted to duplicate the moving platform, simply duplicating the platformWoodBlocks GameObject in the Hierarchy and moving the copy won't work - since when you run the Scene each duplicate would be animated back to the location of the original animation frames (that is, all copies would be positioned and moving in the original location).

The solution is first to create a new, empty GameObject (e.g. named movingBlockParent), and then parent platformWoodBlocks to this GameObject. Now we can duplicate GameObject movingBlockParent (and its child platformWoodBlocks), to create more moving blocks in our scene, that each move relative to where the parent GameObject is located at Design-Time.


<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

#  Making a platform start falling once stepped-on using a Trigger to move animation from one state to another

In many cases we don't wish an animation to begin until some condition has been met, or some event occurred. In these cases a good way to organize the Animator Controller is to have two animation states (clips) and a Trigger on the Transition between the clips. We use code to detect when we wish the animation to start playing, and at that time we send the Trigger message to the Animation Controller, causing the transition to start.

In this recipe we'll create a water platform block in our 2D platform game; such blocks will begin to slowly fall down the screen as soon as they have been stepped on, and so the player must keep on moving otherwise they'll fall down the screen with the blocks too! It looks as shown in the following screenshot:

![Insert Image B08775_08_01.png](./08_figures/B08775_08_01.png)

<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

This recipe builds on the previous one, so make a copy of that project, and work on the copy for this recipe.

<!-- ******************************* -->
<!-- ******************************* -->

## How to do it...

To construct an animation that only plays once a Trigger has been received, follow these steps:

1. In the Hierarchy create an Empty GameObject named water-block-container, positioned at (2.5, -4, 0). This empty GameObject will allow us to make  duplicates of animated Water Blocks that will animate relative to their parent GameObject position.

1. Drag an instance of the sprite Water Block from the Project | Sprites folder into the scene and child it to GameObject water-block-container. Ensure the position of your new child GameObject Water Block is (0, 0, 0), so that it appears neatly to right of the wall blocks platform, as shown in the following screenshot:

    ![Insert Image B08775_08_09.png](./08_figures/B08775_08_09.png)

1. Add a Box Collider 2D component to child GameObject Water Block, and set the layer of this GameObject to Ground, so that the player's character can stand and jump on this water block platform.

1. Ensuring child GameObject Water Block is selected in the Hierarchy, open an Animation panel, then create a new clip named platform-water-up. saving it in  your Animations folder.

<!--
1. Click button Add Curve, and chose Transform and Position.

1. Delete the second keyframe at time 1:00. You have now completed the creation of the water block up animation clip.
-->

1. Create a second Animation clip, named platform-water-down. Again, click button  Add Property, and chose Transform and Position, and delete the second keyframe at time 1:00.

1. With the first keyframe at time 0:00 selected, click the red Record button once to start recording changes, and set the Y-value of the GameObjects Transform Position to -5. Now the red Record button again to end recording chagnes. You have now completed the creation of the water block down animation clip, so you can click the red record button to stop recording.

1. You may have noticed that as well as the up/down Animation Clips that you created, another file was created in your Animations folder, an Animator Controller named Water Block. Select this file and open the Animator panel, to see and edit the State Machine diagram:

    ![Insert Image B08775_08_10.png](./08_figures/B08775_08_10.png)

1. Currently, although we created 2 animation clips (states), only the Up state is ever active. This is because when the scene begins (Entry) the object will immediately go in state platform-water-up, but since there are no transition arrows from this state to platform-water-down, then at present the Water Block GameObject will always be in its Up state.

1. Ensure state platform-water-up is selected (it will have a blue border around it), and create a Transition (arrow) to state platform-water-down, by choosing Make Transition from the mouse-right-click menu.

1. If you run the scene now, the default Transition settings are that after 0.75 seconds (default Exit Time) the Water Block will transition into their Down state. We don't want this – we only want them to animate downwards after the player has walked onto them.

1. So create a Trigger named Fall, by choosing the Parameters tab in the Animator panel, clicking the plus '+' button and selecting Trigger, and then selecting Fall.

1. Do the following to create our the transition to wait for our Trigger:

- In the Animator panel select the Transition

- In the Inspector panel uncheck the Has Exit Time option

- Set Transition Duration to 3.0 (so the Water Block slowly Transitions to its Down state over a period of 2 seconds)

- In the Inspector panel click the plus '+' button to add a Condition, which should automatically suggest the only possible condition parameter, which is our Trigger Fall.

    ![Insert Image B08775_08_12.png](./08_figures/B08775_08_12.png)


    NOTE: - An alternative to setting the Transition Duration numerically, is to drag the Transition end time to 3:00 seconds in the Animation Timeline offered below the Transition Settings in the Inspector

1. We now need to add a Collider trigger just above the Water Block, and a C# script-class behavior to send the Animator Controller Trigger when the player enters the collider.

1. First, ensuring child GameObject Water Block is selected, add a (second) 2D Box Collider, with a Y-Offset of 1, and tick its Is Trigger checkbox:

    ![Insert Image B08775_08_13.png](./08_figures/B08775_08_13.png)

1. Create a C# script-class named WaterBlock, and add an instance-object as a component to the Water Block child GameObject:

    ```csharp
        using UnityEngine;
        using System.Collections;

        public class WaterBlock : MonoBehaviour {
          private Animator animatorController;

          void Start(){
            animatorController = GetComponent<Animator>();
          }

          void OnTriggerEnter2D(Collider2D hit){
            if(hit.CompareTag("Player")){
              animatorController.SetTrigger("Fall");
            }
          }
        }
    ```

1. Make 6 more copies of GameObject water-block-container, with X positions increasing by 1 each time, that is, 3.5, 4.5, 5.5, and so on.

1. Run the scene, and as the player's character runs across each water block they will start falling down, so he had better keep running!

<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

You created a two-state Animator Controller state machine. Each state was an Animation Clip. You created a Transition from the Water Block Up state to its Down state that will take place when the Animator Controller received a Fall Trigger message. You created a Box Collider 2D with a Trigger, so that scripted component WaterBlock could be detected when the player (tagged Player) enters its collider, and at that point send the Fall Trigger message to make the Water Block GameObject start gently Transitioning into its Down state further down the screen.

Learn more about the Animator Controllers on the Unity Manual web pages at:

- http://docs.unity3d.com/Manual/class-AnimatorController.html.

<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

#  Creating animation clips from sprite sheet sequences

The traditional method of animation involved hand-drawing many images, each slightly different, which displayed quickly frame-by-frame to give the appearance of movement. For computer game animation, the term Sprite Sheet is given to the image file that contains one or more sequences of sprite frames. Unity provides tools to breakup individual sprite images in large sprite sheet files, so that individual frames, or sub-sequences of frames can be used to create Animation Clips that can become States in Animator Controller State Machines. In this recipe, we'll import and break up an open source monster sprite sheet into three animation clips for Idle, Attack, and Death that looks as shown:

![Insert Image B08775_08_17.png](./08_figures/B08775_08_17.png)


<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

For all the recipes in this chapter, we have prepared the sprite images you need in folder 1362_03_05. Many thanks to Rosswet Mobile for making these sprites available as Open Source at:

- http://www.rosswet.com/wp/?p=156.

<!-- ******************************* -->
<!-- ******************************* -->

## How to do it...

To create an animation from a sprite sheet of frame-by-frame animation images, follow  these steps:

1. Create a new Unity 2D project.

1. Import the provided image monster1.

1. With image monster1 selected in the Project panel, change its Sprite mode to Multiple in the Inspector, the click the Apply button at the bottom of the panel.

    ![Insert Image B08775_08_52.png](./08_figures/B08775_08_52.png)

1. Now in the Inspector open the Sprite Editor panel by clicking button  Sprite Editor.

1. In the Sprite Editor open the Slice dropdown dialog, set the Type to Grid, set the grid Pixel Size to 64x64, and then click the Slice button. For Type choose dropdown option Grig by Cell Size, and set X and Y to 64. Finally, click the Slice button, and then Apply button in the bar at the top right of the Sprite Editor panel:

    ![Insert Image B08775_08_18.png](./08_figures/B08775_08_18.png)

1. In the Project panel you can now click the expand triangle button center-right on the sprite, and you'll see all the different child frames for this sprite.

    ![Insert Image B08775_08_19.png](./08_figures/B08775_08_19.png)

1. Create a folder named Animations.

1. In your new folder, create an Animator Controller asset file named monster-animator, choose Project panel menu: Create | Animator Controller.

1. In the scene create a new Empty GameObject named monster1 (at position 0, 0, 0), and drag your monster-animator into this GameObject.

1. With GameObject monster1 selected in the Hierarchy, open up the Animation panel, and create a new Animation Clip named monster1-idle.

1. Select image monster1 in the Project panel (in its expanded view), and select and drag the first 5 frames (frames monster1_0-4) into the Animation panel. Change the sample rate to 12 (since this animation was created to run at 12-frames per second).

    ![Insert Image B08775_08_20.png](./08_figures/B08775_08_20.png)

1. If you look at the State Chart for monster-animator, you'll see it has a default state (clip) named monster-idle.

1. When you run your scene you should now see the monster1 GameObject animating in its monster-idle state. You may wish to make the Main Camera size a bit smaller (e.g. size 1), since these are quite small sprites:

    ![Insert Image B08775_08_53.png](./08_figures/B08775_08_53.png)


<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

Unity's Sprite Editor knows about sprite sheets, and once the correct grid size has been entered it treats the items in each grid square inside the sprite sheet image as an individual image, or frame, of the animation. You selected sub-sequences of sprite animation frames and added them into several Animation Clips. You had added an Animation Controller to your GameObject, and so each Animation Clip appears as a state in the Animation Controller State Machine.

You can now repeat the process, creating an Animation Clip monster-attack with frames 8-12, and a third clip monster-death with frames 15-21. You would then create Triggers and Transitions to make the monster GameObject transition into the appropriate states as the game is played.

Learn more about the Unity Sprite Editor from the Unity video tutorials at:

    - https://unity3d.com/learn/tutorials/modules/beginner/2d/sprite-editor.

Learn more about 2D animation with Sprite Sheets in an article by John Horton on GameCodeOldSchool.com:

    - http://gamecodeschool.com/unity/simple-2d-sprite-sheet-animations-in-unity/

<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

# TileMapper

basics - before GameKit ??

Tile
 Assets
Grid GameObjects
The Tilemap Palette
Custom Brushes


<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

# Creating a platform game with Tiles and Tilemaps

One of the powerful 2D tools introduced by Unity is the Tilemapper. In this recipe we'll create a simple 2D platformer, building a grid-based Scene using some free tile sprite images.

![Insert Image B08775_08_59.png](./08_figures/B08775_08_59.png)


<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

For this recipe, we have prepared the Unity pacakge and images you need in folder 08_07.

Special thanks to  GameArt2D.com for publishing the Desert image Sprites with the Creative Commons Zero licence:

    - https://www.gameart2d.com/free-desert-platformer-tileset.html



<!-- ******************************* -->
<!-- ******************************* -->

## How to do it...

To create a platform game with Tiles and Tilemaps, follow these steps:

1. Create a new Unity 2D project.

1. Import the provide images.

1. The tile sprites we've using for this recipe are 128x128 pixels. It's important to ensure that we set the pixels-per-unit to 128, so that our sprite images will map to a grid of 1x1 Unity units. Select all the sprites in folder Project | DesertTilePack | Tile, and in the Inspector set Pixels per Unit to 128.

    ![Insert Image B08775_08_54.png](./08_figures/B08775_08_54.png)

1. Display the Tile Palette panel, by choosing menu: Window | 2D | Tile Palette.

1. In the Project panel, create a new folder named Palettes (this is where to save your Tile Palette assets).

1. Click the Create New Palette button in the Tile Palette panel, and create a new Tile Palette named DesertPalette.

    ![Insert Image B08775_08_55.png](./08_figures/B08775_08_55.png)

1. In the Project panel, create a new folder named Tiles (this is where to save your Tile assets).

1. Ensure Tile Palette DesertPalette is selected in the Tile Palette panel, and then select all the sprites in folder Project | DesertTilePack | Tile, and drag them into the Tile Palette. When asked where to save these new Tile asset files, select your new Assets | Tiles folder. You should now have 16 Tile assets in your Tiles folder, and these Tiles should be available to work with in your DesertPalette in the Tile Palette panel:

    ![Insert Image B08775_08_56.png](./08_figures/B08775_08_56.png)

1. Drag Sprite BG from the DesertTilePack in the Project panel into the Scene. Resize the Main Camera (it should be Orthographic since this is a 2D project), so that the desert background fills the entire game panel.

1. Add a Tilemap GameObject to the Scene, choose menu: Create | 2D Object | Tilemap. You'll see a Grid GameObject added, and as a child of that you'll see a Tilemap GameObject. Rename the Tilemap GameObject to Tilemap-platforms.

    ![Insert Image B08775_08_57.png](./08_figures/B08775_08_57.png)

    NOTE: Just as UI GameObjects are children of a Canvas, Tilemap GameObjects are children of a Grid.

1. We can now start 'painting' Tiles onto our Tilemap. Ensure Tilemap-platforms is selected in the Hierarchy, and that you can see the Tile Palette panel. In the Tile Palette panel select the Paint with active brush tool (the paintbrush icon). Now click on a Tile in the Tile Palette panel, and then in the Scene panel each time you click the mouse button you'll be adding a Tile to Tilemap-platforms, automatically aligned with the grid:

    ![Insert Image B08775_08_58.png](./08_figures/B08775_08_58.png)

1. If you want to delete a tile, use Shift-Click over that grid position.

1. Use the Tile Palette brush to paint 2 or 3 platforms.

1. Now let's add a suitable Collider to GameObject Tilemap-platforms. Select GameObject Tilemap-platforms in the Hierarchy, and in the Inspector add a Tilemap Collider 2D: click Add Component then choose Tilemap | Tilemap Collider 2D.

1. Also create a new Layer named Ground, and set GameObject Tilemap-platforms to be on this Layer (this will allow characters to jump when standing on a platform).

1. Let's test our platform Scene with a 2D character - we can re-use the potatoman character from Unity's free tutorials. Import the provided package PotatoManAssets into your project.

1. Let's setup the 2D gravity setting for this project since the size of the potatoman character is big with respect to the platforms, we'll make the character move slowly by having a heavy gravity etting of Y= -60. Set 2D gravity to this value by choosing menu: Edit | Project Settings | Physics 2D, and then at the top change the Y value to -60.

1. Drag an instance of the PotatoMan hero character2D from the Project | Prefabs folder into the Scene. Position him somewhere above one of your platforms.

1. Play the Scene. The 2D character hero should fall down and land on the platform. You should be able to move the character left and right, and jump using the Space key.

1. You may wish to decorate the scene by dragging some of the Objects Sprites onto the Scene (in Project folder Project | DesertTilePack | Object).

<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

By having a set of platform Sprites that are all a regular size (e.g. 128x128), it is straighforward to create a Tile Palette from those Sprites, and the to add a Grid and Tilemap to the Scene, allowing Tile Palette brush paint of Tiles into the Scene.

You had to set the Sprite pixels-per-unit to 128, so each Tile maps to a 1x1 Unity grid unit.

You added a Tilemap Collider 2D to the Tilemap GameObject, so that characters (like the potatoman) can interact with the platforms. By adding a Layer Ground, and setting the Tilemap GameObject to this Layer, jumping code in the potatoman character controller Script can test the Layer of the object being stood above, so that the jump action will only be possible when standing on a platform Tile.

<!-- ******************************** -->
<!-- ******************************** -->

## There's more...

Here are some suggestions for enhancing this recipe.

<!-- ******************************** -->
<!-- ******************************** -->

## Tile Palettes for objects and walls etc..

The object Sprites in the Desert free pack are all different sizes, and certainly not consistent with the 128x128 Sprite size for the platform Tiles.

However, if the Sprites for the objects and walls etc. in your game _are_ the same size as your plaform Sprites, then you can create a Tile Palette for your objects, and paint them into the Scene using the Tile Palette brush.

<!-- ******************************** -->
<!-- ******************************** -->

## Rule Tiles for intelligent Tile selection

If you explore the 2D Extras Pack (see next), or the 2D GameKit (see next recipe), you'll learn about Rule Tiles. These allow you to define rules about the choice of a Tile based on its neighbors, for example you wouldn't put a platform top Tile immediately on top of another one, so Rule Tiles would place some kind of ground tile to be the Tile under the platform top tile. Rules can ensure the left-most and right-most tiles in a group select the Tiles with artwork for the edges, and so on.

A good introduction to Rule Times can be found in the follwing Unity live training video session:

    - https://unity3d.com/learn/tutorials/topics/2d-game-creation/using-rule-tiles-tilemap

<!-- ******************************** -->
<!-- ******************************** -->

## Learning more

Some learning resources about Tilemapping include:

- Unity TileMap tutorial:

    - https://unity3d.com/learn/tutorials/topics/2d-game-creation/intro-2d-world-building-w-tilemap

- Unity tutoral tilemap assets:

    - https://oc.unity3d.com/index.php/s/VzImolXrvp3K2Q5/download

- Lots of 2D Extra resources, free from Unity Technologies:

    - https://github.com/Unity-Technologies/2d-extras

- Sean Duffy's great tutoral on Tilemapping on the Ray Wenderlick site:

    - https://www.raywenderlich.com/188105/introduction-to-the-new-unity-2d-tilemap-system






<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

# Creating a game with the 2D Gamekit

A collection of Unity 2D tools has been combined to become the Unity is the 2D GameKit. In this recipe we'll create a simple 2D platformer to explore some of the features offere by the 2D GameKit, including pressure plates, doors, falling object damaging enemies etc.

![Insert Image B08775_08_62.png](./08_figures/B08775_08_62.png)

<!-- ******************************* -->
<!-- ******************************* -->

## Getting ready

This recipe uses the free Unity Asset Store and Package Manager packages.

<!-- ******************************* -->
<!-- ******************************* -->

## How to do it...

To create a platform game with Tiles and Tilemaps, follow these steps:

1. Create a new Unity 2D project.

1. Use the Package Manager to install the Cinemachine and Post Processing packages (if these are installed you'll get errors when downloading the 2D GameKit).

1. Import 2D GameKit (free from Unity Technologies) from the Asset Store.

1. Close and then re-open the Unity Editor.

1. Create a new 2D GameKit Scene, by choosing menu: Kit Tools | Create New Scene. You'll then be asked to name the Scene, and a new Scene asset file will be created in your Project | Assets folder. You'll see there are quite a few special GameObjects in the Hierarchy of your new Scene:

    ![Insert Image B08775_08_60.png](./08_figures/B08775_08_60.png)

1. As you can see, the new Scene starts off containing an animated 2D character (Ellen), and a small platform.

1. In the Inspector select the Tilemap child of GameObject TilemapGrid - we are getting ready to paint some Tiles onto this Tilemap GameObject.

1. Display the Tile Palette, choose menu: Window | 2D | Tile Palette. Select TilesetGameKit, and clikc on the green topped grass platform tile. Select the Paint with active Brush tool (the paintbrush icon).

1. Now you can start painting grass topped platforms onto the Scene. This is a Rule Tile, so it cleverly ensures that only the top Tiles in a touching group are painted with the grass topped Tile. The other touching tiles (left/right/below) are painted with a brown, earthy Tile.

1. Create a wide, flat area, and then to the right of where Ellen starts, create a very tall wall of earth, too tall for Ellen to jump over.

1. Now between Ellen and the earth wall, add 4 Spikes, so she would get damage trying to jump over them. Drag instances of the Spikes Prefab from Project folder 2DGameKit | Prefabs | Environment.

1. Now, to make things even harder, add a Chomper enemy between the Spikes and the earth wall! Drag an instance of the Chomper Prefab from Project folder 2DGameKit | Prefabs | Enemies.

    ![Insert Image B08775_08_63.png](./08_figures/B08775_08_63.png)

1. We have to give Ellen some way to get past the earth wall, that avoids the Spikes and Chomper obstacles. Let's add a Teleporter, to the left of where Ellen starts. Drag an instance of the Teleporter Prefab from Project folder 2DGameKit | Prefabs | Interactables.

1. Let's create a destination point for the Teleporter using a custom Sprite. Import the Enemy Bug Sprite into this project, and drag an instance from the Project panel into the Scene - somewhere to the right of the earth wall.

1. Teleporters require a Transition Point component in the GameObject that is to be the destination of the teleportation. First add a Collider 2D to Enemy Bug, choose Add Component | Physics 2D | Box Collider 2D. Check its Is Trigger option.

1. Now add a Transition Point component to Enemy Bug, choose Add Component, then search for Transition, then add Transition Point.

1. We can now set up the Teleporter. With the Teleporter selected in the Hierarchy, in the Inspector for the Transition Point (Script) component do the following:

    - Transitioning Game Object: drag Ellen into this slot

    - Transition Type: choose Same Scene from the dropdown menu

    - Destination Transform: drag Enemy Bug into this Transition Point slot

    - Transition When: choose On Trigger Enter from the dropdown menu

    ![Insert Image B08775_08_66.png](./08_figures/B08775_08_66.png)

1. Run the Scene. Ellen can safetly avoid the Spikes and Chomper by using the Teleporter.

1. Let's make it a bit more interesting - having the Teleporter GameObject initially in active (not visible or interactable with), and adding a switch that Ellen has to hit to make the Teleporter active.

1. Select the Teleporter GameObject in the Hierachy, and uncheck its active box at the top-left of the Inpsector - the GameObejct should be invisible, and appear greyed out in the Hierarchy.

1. Add a single use switch to the game, to the left of where Ellen starts. Drag an instance of the Single Use Switch from Project folder 2DGameKit | Prefabs | Interactables.

1. With the Single Use Switch selected in the Hierarchy, in the Inspector set the following:

    - Layers: add Layer Player to the interactble Layers (so swithc can be enabled by Player colliding or firing a bullet)

    - On Enter: Drag Teleporter into a free RunTime Only GameObject Slot, and change the action dropdown menu from No Function to GameObject | Set Active (bool), and then check the checkbox that appears.

1. Run the Scene. Ellen now has to travel over to the switch, to reveal the Teleporter, that then leads her to safetly transport to the Enemy Bug location, beyond the earth wall and away from danger.

<!-- ******************************** -->
<!-- ******************************** -->

## How it works...

We have dipped our toes into the wide range of features of the 2D GameKit. Hopefully this recipe gives an idea of how to work with the provided Prefabs, and also how to explore how custom artwork can be used, with appropriately added components, to create your own GameObjects using the features of the 2D GameKit.

If you look at the Ellen 2D character, you'll see some scripted components that manage the characters interaction with the 2D GameKit. These include:

- CharacterController 2D - movement and physics interactions

- Player Input - keyboard/input control mapping, so you can change which keys/controller buttons control movement, jumping, etc.

- Player Character - how character interactives with the 2D GameKit, including fighting (melee), damage, bullet pool etc.

Learn more about Ellen and her component in the reference guide:

- https://unity3d.com/learn/tutorials/projects/2d-game-kit/ellen?playlist=49633


<!-- ******************************** -->
<!-- ******************************** -->

## There's more...

Some learning resources about the Unity 2D GameKit:

- Unity's 2D GameKit online Tutorials / Reference Guide / Advanced Topics:

    - https://unity3d.com/learn/tutorials/s/2d-game-kit

- Unity 's offical 2D GameKit forum:

    - https://forum.unity.com/threads/2d-game-kit-official-thread.517249/

- Asset Store 2D GameKit tutorial project:

    - https://assetstore.unity.com/packages/essentials/tutorial-projects/2d-game-kit-107098

- Series of YouTube video tutorials from Unity Technologies entitled Getting Started With 2D Game Kits:

    - Overview and Goals [1/8]: https://www.youtube.com/watch?v=cgqIOWu8W1c

    - Ellen and Placing Hazards [2/8] Live 2018/2/21: https://www.youtube.com/watch?v=V2_vj_bbB4M

    - Adding Moving Platforms [3/8]: ttps://www.youtube.com/watch?v=SfC3qYz4gAI

    - Doors and Destructible Objects [4/8]: https://www.youtube.com/watch?v=-hj6HnbI7PE

    - Adding and Squishing Enemies [5/8]: https://www.youtube.com/watch?v=WRKG_DDlUnQ

    - Using The Inventory System [7/8]: https://youtu.be/LYQz-mtr90U

    - Teleporting and Dialog Boxes [8/8]: https://www.youtube.com/watch?v=gZ_OZL57c0g