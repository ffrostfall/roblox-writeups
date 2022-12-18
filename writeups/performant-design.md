*this post is targeted towards intermediate/experienced scripters, not beginners*

A big problem with the common Roblox's developer on performance is "you shouldn't optimize something unless you need to", and while this is somewhat okay for less ambitious games, you really should focus on designing your code to not require optimization in the first place.

For example, let's design a small physics-networking system similar to Roblox's. No code yet, so let's just lay down some design:

Step 1: Each physics object checks if players are near it
Step 2: If there is a player nearby, transfer network ownership
Step 3: Client receives that, and starts doing physics calculations for the object.
Step 4: The client sends updates every frame.
Step 5: The server runs checks to make sure the client is accurate- if it's inaccurate, send an update to the client.

If we're trying to design an efficient system, there's a lot of issues with this model of network ownership. 

---

Step 1 has an issue: why is each physics object getting the closest players? Assuming there's more physics objects than players, it would make more sense for the server to check if there's players in a certain radius. 

**Takeaway:** Design your code to do less- if there's a route to overall do less, then take it. Sometimes you don't need to do certain operations when you can design your code to work less.

---

Step 4 has an issue: why do we need to calculate it every frame? Shouldn't we just update it when it changes, instead of wasting bandwith on the same value?

**Takeaway:** Try to take away redundant code. If something is the same, then don't update it. Don't put out needless updates with things you already know.

---
Step 5 has an issue: Why are we sending an update to the client when the client is either 1. exploiting or 2. incorrect- in which case the calculations will instantly realize it's wrong, and adjust accordingly?

**Takeaway:** You don't need the full context to do something. You can rely on systems you wrote to do the work for you- you don't need to be perfect with 100% accuracy every time.

---

Overall, you shouldn't need to traditionally "optimize" your code. You should have *already done it* by thinking "hey, this probably isn't the best idea, I should try something more efficient" before you even write it. This means you don't need to gut out systems and you'll have less issues with performance later on, because you thought about performance before even writing the code. Just a simple "this is probably bad, I should reduce the load" is enough to save time later rewriting and optimizing code, with zero effort spent on *actually* optimizing it.