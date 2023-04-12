# MastersProject

For my Masters project at university I interested in tools development for unreal engine. I wanted to explore one could extend the editor through use of plugins. 

I came upon the idea of prototyping a toolkit for assisiting development of scripted dialogue scenes in games. More specifically, replicating some of the functionality used in the creation of visual novel games.

You can watch the presentation [here](https://onedrive.live.com/View.aspx?resid=3244DF4F4684711B!56469&wdSlideId=256&wdModeSwitchTime=1681334403447&authkey=!AEusOIz0NUngYlo) or read below for a comprehensive breakdown of the project.

## Research

[Ren'Py](https://www.renpy.org/) is one of the more popular tools for creating visual novel games. Built using Python, it is more of a complete game engine rather than a toolkit. It allows for quick development of 2D games using a script|tag system. The developer only needs to write their story and add some keywords from the engine. Ren'Py will then read through the script. Upon reaching a known keyword it will format the following text appropriately.

An example from the [Ren'Py Documentation](https://www.renpy.org/doc/html/quickstart.html#a-simple-game)

    label start:

        "Sylvie" "Hi there! How was class?"

        "Me" "Good..."

        "I can't bring myself to admit that it all went in one ear and out the other."

        "Me" "Are you going home now? Wanna walk back with me?"

        "Sylvie" "Sure!"

This approach does work well and there are many reasons to use a data driven development approach like this. It affords quick iteration and development. It is also really approachable to new developers; you need little programming experience, just an idea and some determination to write your story.

It is not without it's limitations. Firstly, editors utilising this type of data driven development are typically restricted to 2D viewports. Restricted may be the wrong word. More so, the complexity of creating keywords to support 3D viewport composition and expecting the developer to use the tags is unrealistic. It would rely on the developer to maticulously specify the locations of characters and camera movements in a 3D environment which is not a great user experience.

On a related note, a secondary tedium of data driven development in this fashion is the maintenance required on large complex projects. Ren'Py relys on its keywords to generate the scene during the game. The developer is able to expand the engine by adding their own keywords to create new scenes specific for their story. However as a writer may want to compose a number of unique scenes, the programmers will need to create a new keyword to support the unique scene. This scene may only be used once or a couple times leading to a lot of development overhead. 

## My Approach

My goal was to develop a functional toolset, employing novel methods of development, influenced by modern game design paradigms. I wanted to utilise the advance 3D features of Unreal Engine to assist in developing visual novel features.

My implementation centers around the Level Sequencer tool built into the engine. This tool is an NLE (None-Linear Editor) like Premier Pro from Adobe, but with the ability to call scripted functions specified in Blueprints. As this tool is analagous to tradional NLE's used in cinema to tell stories, it was the obvious first choice to extend the editor with. Using this tool the designer can more visually compose 3D scenes or 2D scenes using built in cameras, cuts and scriptable actors. This tool is already used in this way to create in game cut scenes.

![The Level Sequence Tool](media\VNTK_EditorComposition.png)

I attempted to expand the capabilities of the level sequencer by modifying the DirectorBP that exists behind any sequence made using the Level Sequencer tool. When you add a triggerable event to a sequence, its corresponding event node is created in the DirectorBP. These events are connected to custom functions for pausing and resuming the sequence, displaying text boxes and choices, and triggering which sequence to open at the end of the current one. 

![The DirectorBP behind every sequence asset](media\VNTK_DirectorBP.png)

The designer composing the scene only needs to add breakpoints to the relevant tracks and pass through the required information. The DirectorBP passes that to the relevant actors to control them. Adding to the breakpoint track triggers an instruction to pause the sequence. Typically to display a diaglogue box or a choice. Adding a breakpoint to the Dialogue or Choices tracks trigger text to appear onscreen in the form of dialogue or a clickable choice respectively.

![The breakpoints on the sequence tracks](media\VNTK_Track_Breakpoints.png)
![The breakpoints for triggering the dialogue and choices](media\VNTK_Track_DialoguesAndChoices.png)

When adding items to the tracks, the designer can pass through data. This could be in the form of a line of dialogue and the character asset that is speaking. Or it can pass through a list of choices and their linked action. When a choice is clicked it can trigger the linked action. In the demonstration, this is in the form of opening a new sequence based on what the player selected. 
