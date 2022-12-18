"Performance" means a lot. It can mean call time optimization, it can mean low memory usage. It could mean lowering bandwith usage, or maybe all of the above. Performance could be about something per-frame, or something that gets called once. It could be about reducing start-up times, or reducing load times. Or it could be all of those things!

I'm going to attempt to explain when performance matters and why it matters. There's a lot of misinterpretations and people saying "don't use xx because it is 100 microseconds slower". Microseconds can matter when you're dealing with things like frame times, but does it really matter when you're running the code once per second?

# Frame Time / Frame Budget
So first off: let's define what fps is. A frame, or a "tick", or how long it takes to run things like rendering and server logic. For example, it takes 16 milliseconds to render a single frame and display it on your screen. This means you have 60 frames per second- which is what Roblox runs at.

One important thing to note is that **16 milliseconds is a lot of time**. 1 millisecond is 1000 microseconds- a difference of 80 microseconds is pointless. It's a micro-optimization. One common example of needless performance worries is with object-oriented programming- the difference between regular function calling, and method calling, is negligible (within ~80 microseconds), and most of the time you won't be calling something per frame with object-oriented anyways. However something that saves ~800 microseconds is worth it- that's almost 1/16th your frame budget.

![image|686x62](upload://akcwuJw7LganqDr6iYoFaDhLj7t.png)
(Benchmarking method vs. passing self)

**One important thing to note is almost no optimization that sacrifices readability for speed is not worth it 90% of the time.**
Making your code unreadable for a ~150 microsecond gain is not worth it.

**Ping is directly impacted by frame time.** Due to the way Roblox networking works (packets are sent at the end of the frame, packets are received at the start of the frame), your frame time directly impacts ping. Going under 60fps will increase your user's ping.

It's important to note that the more remotes you call, the longer it'll take to send those out. This literally increases frame time- which increases ping. Connections are also atrociously inefficient.

### The Microprofiler
Since Roblox's fps-cap is at 60 (for both the client and server), that means each frame should take under 16 milliseconds- since 1/60 is 0.016, which is 16 milliseconds. So this is the *maximum* amount of time it takes before user experience starts dropping. The microprofiler lets us see what exactly is eating up the frame budget, and it's a very useful tool for debugging performance issues.

![image|690x278](upload://9QoOukVXZ71V0eeIEs9s3MvGFiO.jpeg)
(A screenshot of the client-sided microprofiler)

# Networking
Networking is arguably the most important area to optimize. Optimizing your networking can significantly improve user experience by the frame time on the server (which subsequently reduces ping), ping spikes, and in some extreme cases prevent disconnecting from the server. In other extreme cases, not optimizing bandwith can result in long freezes on the server. However I've already talked about this, and so have other developers- I'll just link the posts below.

https://devforum.roblox.com/t/in-depth-explanation-of-robloxs-remoteevents-instance-replication-and-physics-replication-w-sources/1847340

https://devforum.roblox.com/t/network-optimization-best-practices-how-to-keep-your-games-ping-low/1913475

# Client FPS
This is arguably one of the most important factors of optimization. Your client's FPS should be top priority, as a sub-60 FPS experience may significantly diminish the quality of time spent on your game. There's a lot of *great* ways to optimize the client though:
- Implement a chunkloading system
- Turn off Shadows and CastShadow on things that don't need them
- Delete textures the client will never see
- Don't run heavy code on the client- optimize all your code on the client. It directly impacts FPS.
- Don't overlap textures- this significantly increases render time.
- Transparent blocks / textures are substantially worse for performance.

# Conclusion
You should focus on optimizing client fps, server fps and bandwith- you shouldn't be focused on things like reducing call time for functions that get called once per second. **Your frame time is what matters.**

I don't recommend using attributes, valueobjects and instances for things like state. This isn't good, but I have an entire post about that.
https://devforum.roblox.com/t/lets-talk-about-attributes-valueobjects-and-instances/1918761