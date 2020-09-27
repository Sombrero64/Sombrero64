[Blog](https://sombrero64.github.io/Sombrero64/blog): [Roblox](https://sombrero64.github.io/Sombrero64/blog/roblox)
# ReplicatedScriptService Might Be a Bad Idea
Roblox has plans introducting a new service called **ReplicatedScriptService**, a service that runs scripts to both cilents and the server. 
It allows you to run game logic for players and their server, which is useful at certain cases, but I think this can be a bad idea later on it's lifetime.

The main issue is that malicious exploiters (children who exploits thier client to instail cheats) can use this a gateway to run exploits as they can inject thier code.
Either they inject it directly to ReplicatedScriptService, or inject code that would introduce scripts to the service that would ruin servers.

For example, let's take a local script and add the following code.

```lua
local Storage = game:FindService("ReplicatedStorage")
local Scripts = game:FindService("ReplicatedScriptService")

local Event = Instance.new("BindableEvent", Storage)

local Script = script:WaitForChild("Script"):Clone()
Script.Parent = Scripts

Event:Fire()
```

Let's go the nested script and add the following code.

```lua
repeat wait() until script.Parent == game:FindService("ReplicatedScriptService")

local Event = game.ReplicatedStorage:WaitForChild("BindableEvent")
Event.event:Connect(function()
	-- do this
end)
```

This is just a concept, as ReplicatedScriptService seems nonfunctional. But this could work, and that's a big no-no.

## The Problem
As you can see, it's stupid easy to make exploits with this. For example, you can write code that can wipe content to brick servers.
For example, I can go on some popular game, inject some code that would delete storages (including Workspace, ReplicatedStorage, ServerStorage & ServerScriptService), and I see that people would no longer able to play the server. 

So, imagine that pently of exploiters went on multiple servers on a game, and brick them, therefore, you might not able to play your favorite game.
The only way to prevent such as a thing is to play the game in a private server with select people, but there are people out there who cannot afford private servers.

I hope Roblox and developers were aware about this problem, and would try their best to patch these exploits. 
However, I don't guarantee that Roblox wouldn't do a thing about it, or atleast fail when trying, 
as with the inforcement of Replicated Filtering (FilteringEnabled) knocks down pently of exploits, 
but places who couldn't work with FE is pretty much broken as they aren't designed to work with it.

However, patching out exploits in a game is like whack-a-mole; while you patch out an exploit, there will still be another one.
And you are pretty much screwed if the exploiters made a copy of your place and leak it, as everyone would know how your game works.

That's not fun! The fun part is building your dreams, and the serious part is programming, including patching bugs and problems.
I shouldn't take my time patching out exploits if I know their still be a new, stronger one coming out. So, why bother?

Now, I don't have a problem with exploiters who wants do mirror harmless things, such as changing the color of their gun to only themselfs, patching exploits, and just want to see then improve the security of the place or even Roblox.
As long you aren't hurting anyone and getting an unfair advantage, I won't mind. What I have a problem with is that pently of exploiters are going to cheat, break servers, and steal assets.

## Solutions
Luckly, you can include an anti-exploit messure that removes scripts from the service.

```lua
local RSS = game:FindService("ReplicatedScriptService")
local Whitelist = {} -- include important scripts you created here

local function contains(items, item)
	for _, i in pairs(items) do
		if i == item then return true end
	end
	return false
end

RSS.ChildAdded:Connect(function(obj)
	if not contains(Whitelist, obj) then
		obj:Destroy()
	end
end)
```

You do not have plans using the service, you can shorten the script like this.

```lua
local RSS = game:FindService("ReplicatedScriptService")

RSS.ChildAdded:Connect(function(obj)
	obj:Destroy()
end)
```

It's best to place this in a service that only the server can access, such as ServerScriptService. 
Therefore, even clients can edit these scripts, it would not be replicated back to another players.
