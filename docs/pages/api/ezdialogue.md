## Constructors

#### **new**

```lua
local dialogue = EzDialogue.new(name, dialogueId, settings)
```

Creates and returns a new dialogue object. The `npcId` property is set to `name` by default. `settings` is a table of settings or nil, where the table will reconcile with the default settings if any are missing. 

---

#### **newFromTable**

```lua
local dialogue = EzDialogue.newFromTable(identifier, dialogueId)
```

Creates and returns a new dialogue object by reading a dialogue entry that has already been prepared in a `DialogueList` dictionary. It searches through `DialogueList` through the `identifier` key, then finding the specific dialogue using `dialogueId`.

---

## Methods

### Methods for `DialogueList` Dialogues

#### **Play**

```lua
dialogue:Play()
```

Plays the dialogue. 

---

#### **UnlockChoice**

```lua
dialogue:UnlockChoice(nodeId, nextNode)
```

Unlocks a choice that was previously locked. Expects the `nodeId` of the node and the `Next` pointer of the choice/option to unlock.

---

#### **LockChoice**

```lua
dialogue:LockChoice(nodeId, nextNode)
```

Locks a choice that was previously unlocked. Expects the `nodeId` of the node and the `Next` pointer of the choice/option to lock.

---

### Methods for Custom Dialogues

#### **StartDialogue**

```lua
dialogue:StartDialogue(message)
```

Starts a dialogue with an initial `message` that displays first. Cannot start the same dialogue again while it has not ended.

---

#### **Dialogue**

```lua
dialogue:Dialogue(message)
```

Adds new text to be displayed in the dialogue.

!!! info
    For more information on RichText support, see the RichText section in [examples](../examples.md).

---

#### **PromptChoices**

```lua
local option = dialogue:PromptChoices(optionTable, optionKey)
```

!!! info
    Usage example that shows the `optionTable` format:
    ```lua
    local choices = {
		{Id = 1, Choice = "First choice", Reply = "You chose the first choice"};
		{Id = 2, Choice = "Second choice", Reply = "You chose the second choice"};
	}

    local promptChosen = rogerDialogue:PromptChoices(choices, "first_set") -- "first_set" is the id for this options event
	local chosenPrompt = promptChosen:Wait() -- returns the choice Id
    ```

Prompts the player with choices that supports up to 3 choices. Yields the dialogue until a choice is chosen. Returns an event that can be used to wait for the chosen option to create custom dialogue trees. 

---

#### **EndDialogue**

```lua
dialogue:EndDialogue()
```

Ends the dialogue.

---

### Setter Methods

#### Prompt

`dialogue.Prompt : boolean` _[default: false]_

Accepts a `ProximityPrompt` and sets the prompt for handling triggers.

---

#### ToggleMovement

`dialogue.ToggleMovement : boolean` _[default: false]_

Set to `true` if you want the player to be able to move during the dialogue.

!!! warning
    Setting this value to `true` is risky as it allows the player to move freely during a dialogue. Use only when required and in a specific case scenario.

---

#### ReEnableMovement

`dialogue.ReEnableMovement : boolean` _[default: true]_

Set to `false` if you do not want the player to be able to move after the dialogue.

!!! info
    This module uses PlayerControls to enable and disable the player controls, which works across all platforms. If you set this value to `false`, you will have to manually enable the controls on your own.

---

#### CanSkip

`dialogue.CanSkip : boolean` _[default: true]_

Set to `false` if you do not want the player to be able to skip the current message being played. If `true`, a blinking button will appear that will skip to the end of the current message and plays the next message after a cooldown.

!!! note
    You can change the cooldown length with the provided setter method. The default is `3 seconds`.

---

#### ShowName

`dialogue.ShowName : boolean` _[default: true]_

Set to `false` if you do not want the name to be displayed during the dialogue. The name displayed can be changed using the provided setter method.

---

#### ReEnablePrompt

`dialogue.ReEnablePrompt : boolean` _[default: true]_

Set to `false` if you do not want the prompt that triggers the dialogue to be enabled again after the dialogue ends.

!!! warning
    If `ReEnablePrompt` is set to `false`, you will have to enable it again on your own. You can retrieve the prompt using the provided getter method and toggle it yourself for your own needs.

---

#### ResetCamera

`dialogue.ResetCamera : boolean` _[default: true]_

Set to `false` if you do not want the player's camera to reset after the dialogue ends.

!!! info
    If set to `false`, you will have to manipulate the player's camera on your own after the dialogue has ended.

---

#### ReEnableUI

`dialogue.ReEnableUI : boolean` _[default: true]_

Set to `false` if you do not want game UI to be enabled again after the dialogue ends.

!!! info
    You can configure which UI, including your own, will be hidden and shown during a dialogue. For more information, see the UI section in [examples](../examples.md).

---

#### TextSpeed

`dialogue.TextSpeed : EzDialogue.enum.TextSpeed.ITEM_NAME` _[default: EzDialogue.enum.TextSpeed.Normal]_

You can set the text speed of the dialogue.

!!! info
    List of available enums:
    ```lua
    EzDialogue.enum.TextSpeed.Slow; -- each character is played after 0.1 seconds
    EzDialogue.enum.TextSpeed.Normal; -- each character is played after 0.05 seconds [DEFAULT]
    EzDialogue.enum.TextSpeed.Fast; -- each character is played after 0.01 seconds
    ```

---

#### TextFont

`dialogue.TextFont : Enum.Font.ITEM_NAME` _[default: Enum.Font.Arial]_

You can set the font of the text.

---

#### NameFont

`dialogue.NameFont : Enum.Font.ITEM_NAME` _[default: Enum.Font.Arial]_

You can set the font of the display name text.

---

#### NameFontItalic

`dialogue.NameFontItalic : boolean` _[default: false]_

You can set the display name to an italic style by setting the value to `true`.

---

## Events

#### **DialogueBegun**

`<RBXScriptSignal> dialogue.DialogueBegun(id)`

```lua
dialogue.DialogueBegun:Connect(function(id)
    print(id)
end)
```

Fires when the dialogue starts. Passes the `id` as an argument.

---

#### **DialogueEnded**

`<RBXScriptSignal> dialogue.DialogueEnded(id)`

```lua
dialogue.DialogueEnded:Connect(function(id)
    print(id)
end)
```

Fires when the dialogue ends. Passes the `id` as an argument.

!!! warning
    If you need to create additional functions or events that triggers when a dialogue ends, it is better to connect it to the `DialogueEnded` event as it is the only accurate way of determining when a dialogue interaction has ended. 
    ```lua
    dialogue:StartDialogue("Hello")
    dialogue:EndDialogue()
    -- any code written after will be run regardless if the dialogue has ended or not
    -- the script in which you are calling the dialogue from does not yield when a dialogue is started
    ```

---

#### **OnChoiceUnlocked**

`<RBXScriptSignal> dialogue.OnChoiceUnlocked(nodeId)`

```lua
dialogue.OnChoiceUnlocked:Connect(function(nodeId)
    print(nodeId)
end)
```

Fires when a choice is unlocked. Passes the `nodeId` as an argument.

---

## Properties
