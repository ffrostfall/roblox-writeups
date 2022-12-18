Any competitive, or hardcore, shooter on Roblox will do very poorly. I don't think you should make one. Here's why:

1. Roblox has no reliability types on RemoteEvents- you cannot do 60hz character movement without *massively* cutting out your players (anyone on wifi, anyone with a slightly subpar connection)
2. Roblox's default humanoids move at 20hz- this is too low for competitive shooters.

Alright, that can be a lot of information for someone who's not too familiar with networking. Let's dive into it:
# Reliability Types
When a packet gets sent from your computer to the Roblox server, it has two different *reliability types*: Reliable, and unreliable. Because packets aren't guaranteed to go through, it will try to re-send a packet if it doesn't go through. **All networking traffic is halted during this time**. And there's also unreliable: If it doesn't go through, nothing happens.

So, this means that people on subpar connections will be hurt if you send a packet every frame. If you have 0.5% packet loss (this is fine to play almost every game, and most people will have something similar when playing on wireless), that means every 2 seconds someone will experience a ping spike when you fire a remote event every frame. That is not playable.

# Why isn't 20hz movement acceptable?

Because competitive shooters require *much* more precise movement refresh rates, as the timings are *a lot* more precise. You cannot have a competitive tactical shooter with 20hz movement.

# Why make this thread?
This information really needs to be spread out more- I see many trying to make competitive shooters on Roblox, it is just a massive time waste. I want to put this in an area where it will be seen by many people, so no one tries to make a mistake and do this.

---

If you wish to see competitive shooters on Roblox, please show your support this feature request:
https://devforum.roblox.com/t/reliability-types-for-remoteevent/308510