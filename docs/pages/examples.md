## DialogueList Dialogues

### Creating the Dialogue
You can create an unlimited number of dialogues belonging to one entry, where you may have multiple entries with their own individual dialogues. These are stored in a dictionary.

!!! info "Formatting"
    Dialogue entries follow a specific format as displayed below. Just like you would be able to in a script, you can override any settings and also set certain dialogues to be locked.

```lua
local DialogueList = {
	["Steve"] = {
		["FirstDialogue"] = {
			Settings = false;
			Locked = false;
			Prerequisites = false;
			Dialogue = {
				[1] = {Message = "Hello!", Next = "Second"}; -- The first message or dialogue to be played in the dialogue always has a key of 1
				["Second"] = {Message = "This is a converted dialogue!", Next = "Third"}; -- The Next key points towards the next message that will be played once this message is finished
				["Third"] = {Message = "Time to end!", Next = false}; -- If next is set to false, this will mark the end of the dialogue
			};
		};
        ["SecondDialogue"] = {
			Settings = {
                TextFont = Enum.Font.Cartoon; -- Override the font of the dialogue and display name text
                NameFont = Enum.Font.Cartoon;
            };
			Locked = false;
			Prerequisites = false;
			Dialogue = {
				[1] = {Message = "Hello!", Next = "Second"};
				["Second"] = {Message = "Second message!", Next = "Third"};
				["Third"] = {Message = "Third and final message!", Next = false};
			};
		};
	};
}

return DialogueList
```

!!! info
    The key of the first message of the dialogue should always be set to [1]. Each message node has a pointer Next that points to the key of the next message to be played once the current message is finished playing. These keys can be customized and unique, although some sort of orderly naming convention is recommended for readability purposes.

### Fetching & Using the Dialogue

One of the ways to initiate a dialogue is to have it associated with an NPC. We can fetch all the dialogues for the NPC and load whichever dialogue we want by referencing the dialogue ID/key.

When creating a new NPC object, we **do not need** to have a `ProximityPrompt` in the NPC model. It will automatically be created for you. We can detect when the player initiates a dialogue with the NPC's `PromptTriggered` event, where we can play the dialogue and add in any additional code or functionality.

```lua
local steveModel = NPCs.Steve -- the name of the NPC is set to 'Steve', which matches the dialogues listed under the key 'Steve'
local steve = EzNpc.new(steveModel)

local dialogues = steve:FetchDialogues() -- fetches all the dialogues under the key 'Steve'

local steveId = steve.GetId
local steveFirstDialogue = steve:LoadDialogue(steveId, "FirstDialogue")

steve.PromptTriggered:Connect(function(data, dialogueId) -- returns the npc data and the current loaded dialogue (id)
	steveFirstDialogue:Play() -- plays the dialogue
end)

steveFirstDialogue.DialogueBegun:Connect(function()
	print("Dialogue ended") -- prints when the dialogue starts
end)

steveFirstDialogue.DialogueEnded:Connect(function()
	print("Dialogue ended") -- prints when the dialogue ends
end)
```

![type:video](./videos/stevefirstdialogue-example.mp4)

## Custom Dialogues

### Creating & Using the Dialogue

When creating a dialogue from scratch in a script, you also have the option to either associate it with an NPC or keep it independent. For this example, we will keep it similar with the previous example and use an NPC.

```lua
local rogerModel = NPCs.Roger
local roger = EzNpc.new(rogerModel)
local rogerId = roger.GetId -- NPC ID

local rogerDialogue = EzDialogue.new("Mr. Roger", "Custom") -- (name to be displayed, dialogueId)
rogerDialogue.Id = rogerId -- Sets the NPC ID
rogerDialogue.Prompt = roger.GetPrompt -- set the prompt to detect the trigger to the NPC's prompt

roger.PromptTriggered:Connect(function(data, dialogueId)
	rogerDialogue:StartDialogue("Hello")
    rogerDialogue:Dialogue("Goodbye!")
	rogerDialogue:EndDialogue()
end)
```

!!! info
    When creating a new custom dialogue, `npcId` is set to the `name` by default. To change `npcId` to suit your needs, you can use the provided setter method. 

    You will also need to declare the `ProximityPrompt` to be able to detect prompt triggers. Since the NPC automatically creates one for you, you can set the dialogue's prompt to the NPC's prompt.

![type:video](./videos/rogercustomdialogue-example.mp4)

!!! warning
    To create a dialogue without association to an NPC, you can set `npcId` and `dialogueId` however you want. However, you will need to declare your own prompt and write the triggers yourself. 