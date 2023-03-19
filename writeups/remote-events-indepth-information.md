*Updated 10/27/2022*

I hate the lack of centralized knowledge when it comes to things like how Roblox does networking, so I am going to do my best to share my knowledge with others when it comes to this.

**Prerequisite Knowledge**
- Low-level networking knowledge (packets, ping)
- networking reliability and ordering.
- Roblox's RemoteEvents
- Throughput, bandwidth.

Just a bit of information to start off with: You can press Shift F3 and then Shift 1 to view more networking data.

Roblox does networking per frame- this means that networking is not handled *instantly*, but rather when a packet arrives, it's deferred to the next frame. Most games use end-to-end latency when they show ping (csgo, Minecraft, etc.). It's **not** RTT (round trip time).

Shift F3:

![image](https://user-images.githubusercontent.com/80861876/226212846-f609e6c8-4866-444c-9ab5-7fd8de62b6c3.png)

The ping you see in this image (62.56ms) is after frame desync- it factors in your framerate, pretty much. And yes, this means when the client/server drops below 60fps, ping will increase. It also means that using an FPS unlocker will decrease ping.

Shift F3 + Shift 1:

![image](https://user-images.githubusercontent.com/80861876/226212851-debdf7af-a973-4b5c-95af-c6acc9009bf8.png)

The ping you see in this image (25ms) is without frame desync in mind.

---

# **RemoteEvents**
When you fire a RemoteEvent, the data you sent into the remote event is added to a queue. This queue gets emptied per frame, and all the data gets sent out. The Roblox client receives this information with a ~9-byte overhead. It is important to keep in mind that remote events are reliable, meaning that there are mechanisms in place to prevent packet loss.

It is also important to keep in mind that RemoteEvents, alongside a lot of other things, are ordered, meaning extra steps are taken to make sure that *all important data* (property changes too) is ordered and fired in the same order they were fired on the server.

Another important thing to keep in mind, *that Roblox will throttle networking if you fire too many remote events too fast*. I don't know the metrics, but I have had it happen to me.

There's a queue for RemoteEvents when a RemoteEvent is fired, but there is no listener. This is a *small* queue, and it should not be relied upon as you can lose important data. It *does* drop remote events, and fast-firing events will devastate this queue.

Sources:

![image](https://user-images.githubusercontent.com/80861876/226212865-16ae9193-1fab-450c-98c7-e452e242dcb5.png)

*(quick note, while it does say it "doesn't happen every frame" this is not my experience. I've taken microprofiler logs a lot, and it has happened every frame. so please keep that in mind. However at the same time, I may be wrong. )*

![image](https://user-images.githubusercontent.com/80861876/226212868-099ebda0-fd2f-4d94-a6c8-225802e26213.png)

---

# **Physics**
Physics networking is unreliable and unordered. This means that it, under normal circumstances, has less latency than RemoteEvents. This also means that physics will not care about packet loss or the order of events that happen. It doesn't have to. Part positions can come out of order, and packet loss doesn't affect positioning much as the position is replicated per frame.

Another thing to keep in mind is that **CFrames are expensive to set**. You should avoid CFraming a lot of parts at once.

Physics data is also replicated: if you have a visual moving part on the client, not only will this be visually affected by unstable ping and whatnot, but it will also take up needless bandwidth. 

As a quick footnote for this category, humanoids are expensive to replicate/use. You should always try to reduce the number of humanoids in your projects.

# **Instance Replication**
**Properties are not replicated from client/server network ownership, only physics data**. This is extremely important to note. Only positional/directional data is transferred,

I haven't found that much information on how instances are replicated, how much it impacts networking, and whatnot, but I would imagine that it takes a notable amount of data to replicate instances. So I would *not* recommend relying on Roblox instance replication for entire instances, as it could impact your game and make things worse on low-throughput players. I would recommend creating said parts on the client to save all the wasted data. For example:
```
-- Server
EffectsRemote:FireAllClients(position, "fireballEffect")

-- Client
EffectsRemote.OnClientEvent:Connect(function(position, effect)
     local Clone = EffectsFolder[effect]:Clone()
     Clone.Position = position
     Clone.Parent = workspace
end)
```
*purely an example, I would not recommend taking from this.*

Another noteworthy behavior of instance replication is the fact that **only the most recent property value is replicated.** This basically means that if you change a property 3 times in one frame, only the last value will be replicated.

---

A table that describes the amount of data it takes to replicate values (rough, potentially inaccurate):
```
Blank remote call: ~9 bytes

string: length + 2 bytes

boolean: 2 bytes
number: 9 bytes (IEEE-754 signed doubles)

table: 2 bytes -- (There is no overhead for table indexes)

EnumItem: 4 bytes

Instance: 4 bytes

Vector3: 13 bytes

CFrame (axis-aligned): 14 bytes
CFrame (random rotation, non-axis aligned): 20 bytes
```
(by Tomarty: https://devforum.roblox.com/t/ore-one-remote-event/569721/33)

Roblox uses RakNet (http://www.raknet.com/, https://github.com/facebookarchive/RakNet) to manage networking, which implements reliability and ordering.

While your target for bandwidth should be completely relative to your player cap, I always try to get the client *receive* below 100 kbps and the client *send* below 20 kbps. Although, it completely depends on the game and the player cap for said game.

Thanks for reading! I hope this has helped you in some way.
