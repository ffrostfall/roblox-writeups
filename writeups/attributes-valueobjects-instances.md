# Introduction
I'm making this post because I see a lot of games using things like the below code snippet in competitive fighting games. This, to be put quite simply, isn't good at all. I will explain why in this post.
```lua
local Stunned = Instance.new("Folder")
Stunned.Name = "Stunned"
Stunned.Parent = Character
```

---
## The demon that is StarterCharacterScripts

The first common pattern I see is value objects / folders being in StarterCharacterScripts. It is EXTREMELY expensive to put things in StarterCharacterScripts- and if you have resetting enabled, exploiters can mass lag the server by constantly respawning with alts. Not only does respawning with >10 instances in StarterCharacterScripts lag the server, it also leaks memory out of the sides due to a Roblox bug where [character instances don't get cleaned up](https://devforum.roblox.com/t/every-time-your-character-dies-it-leaks-memory-up-to-500-mb-probably-infinite/1889683). Also, any time the character respawns and you have non-scripts in there, it'll cause significant lag spikes in your game.


---

## ValueObjects & Instances as Temporary State
(by temporary state, I mean for things like character states such as Running, Crouching, Stunned)
ValueObjects and Instances are overused. You should never be depending on ValueObjects or instances for short-term state- attributes are acceptable, but still not optimal. So first off, let's identify a few things:
- The client never calculates other clients state, speed, or anything of the such.
- The client, as such, doesn't need to know anyone else's state, speed, or anything apart from appearance, position, and things like that.
- The client doesn't need to know the state, cooldowns, or saved data of other clients or mobs.


Let's identify a few things with ValueObjects, instances, and attributes:
- ValueObjects, instances, and attributes replicate to every single client assuming they are a descendant of ReplicatedStorage or Workspace.
- Instances are costly to create / destroy rapidly, and it has a lot of wasted data. The same with attributes, just to a lesser extent.
- Functions like :FindFirstChild() take time to execute, so relying on chains of :FindFirstChild() or .Value can result in animation or state weirdness. Keep in mind, Roblox does all inbound connections in one frame. The same is said about attributes, but to a lesser extent.
- Creating ValueObjects on character spawn is costly and will cost a significant amount of resources.

I think it's pretty easy to identify the issues with this. Any form of using attributes, instances, or valueobjects for temporary state (characters) results in a lot of wasted bandwith. Here, let's do some math! Assuming you're *creating instances*: setting the name (16chars, let's assume, so 22 bytes), class (2 byte cost..? no documentation.), parent (5 byte cost). These are reasonable guesses for 1 instance creation- now if we have 8 of these updates in a single frame, that's 6 (Overhead assumption)+2+22+5 = 35 bytes, for a single instance creation. 8*35=280 bytes, multiply that by 60 that's 16.8k, aka 16.8 kilobytes. Now here's the catch- that's multiplied by however many players there are. So if you have a 25 player server, those 8 state updates per frame result in **420,000** bytes per second. That's 420 kilobytes. Now, here's a shocker: speedtest.net reports your internet speed in *bits*. 8 bits is one byte, so if you wanna know how that scales with your internet: 3,360 kilobits, which is 3.36 megabits (3.36 Mbps) for 8 state updates per frame. Just 8.

Mind blown. That's so much wasted data, that most likely **isn't even used by any of your scripts.** This doesn't even mention the issues with memory, creation time, update speed, :FindFirstChild(), all of that. I'm not going into detail about that.

Keep in mind: **these issues still persist with attributes.** You do NOT need to replicate data that *has zero usage anywhere in your codebase (at least most likely)*.

Main takeaway: Don't use things that replicate to every player when that's not needed at all. If really necessary, don't create instances- make a ValueObject on character spawn instead.

---

# Why should I care about these things?
Because if your game remotely relies on ping, **the server's FPS directly impacts ping**. If your server is taking 20 milliseconds for physics and .Heartbeat, you are going to have 20 milliseconds of added ping.

You're also going to notice client frame time being higher- all these instance changes need to be applied and executed. Remember, **16ms per frame is 60fps**. That's an incredibly low budget- 14ms per frame is 70 fps, and 16ms is 60. That's 10 gained fps- for 2 milliseconds of saved time.

Also keep in mind that *player to player interactions* are most of the time beneficial for a game- having a higher player cap can make your game more enjoyable.

Remember that the above math I just showed you is a more conservative assumption **considering you're making an instance**.

---

# What about readability and ease of access?
It's possible to make readable and good code without needing to use things like instances. I *personally* suggest using OOP for character control- it's efficient, looks good, expandable. However, there's people who would tell you to use ECS too- it's your decision. I'm not going to say "this specific way is better than X", I'm not trying to advertise OOP, this is just a suggestion. It's your decision on how you structure your code, I'm just telling you that *this specific way* is very bad. You can use RemoteEvents for replicating state from the server to the client, and back, as well. Just don't use normal instances / value objects / attributes.