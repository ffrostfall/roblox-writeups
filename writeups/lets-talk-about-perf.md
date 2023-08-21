# Performance
I have seen a lot of toxicity and differing opinions on performance in the Roblox community, within all different places. Some people are biased against opting for performance, some people are biased for it, some people want only performance, etc. etc. I am going to try to make this to resolve some of this toxicity and arguing. While that's, kind of absurd to attempt, I really do not like the misconceptions being spread around performance.

## My background + my goal for this "article"
I have been scripting on Roblox for 5 years now. I would say I'm fairly successful, and I know a fair thing or two about this platform. I have been on both sides of this argument; I have argued against trading things for performance, and I have argued for trading things for performance. I would say that back-end work is my specialty, with performance being something I excel at. I am trying to keep this as neutral as possible, because I do not want to argue. As such I won't be replying to anything even slightly inflammatory; I don't want to argue.

## Topic #1: "Premature optimization is the root of all evil"
The full quote is "We should forget about small efficiencies, say about 97% of the time: premature optimization is the root of all evil.". What this means is, if code could be 10 microseconds faster, but it only runs every second, you should not touch it. You should not touch things that will not ultimately impact any form of user experience. There is no reason to.

I see this used as an argument against micro-optimizations for serious things that actually should be prematurely optimized. I understand that it's difficult to tell what should be optimized and what shouldn't be for most coders, but I think there is an easy way to tell if something should be optimized or not: Is this going to affect the user if it does impact performance? If so, how much will it impact performance?

You should be prematurely optimizing things like character replication, head movement, things that can crash the client or overload bandwith, or something that can cause freezes! In fact most of the time, you will wish you prematurely optimized these things! I have had many times where something I put off optimizing became a problem during testing and I wish I didn't put it off. Usually the optimization is pretty simple, and there isn't much trade-off. This is why you *usually* never see triple A games having massive performance issues that crash clients on release! Because.. they prematurely optimize the 3% of cases, because usually that 3% is clear.

You should always be careful and do any easy optimizations on code that runs per-frame. There is almost never a loss to doing that. Most dramatic optimizations I do on games I become a developer for, take me an hour maximum, and improve UX drastically. Just do it.

## Topic #2: Performance vs. logging/stack traces/etc.
If you are discarding error/logging info for the sake of performance, you are doing something wrong.

## Topic #3: Time is not the only performance factor
Time, memory, and bandwidth. Those are what you need to optimize. Just because your server runs at 20 frames per second for an hour, doesn't mean it will for 6 hours. I think every Roblox dev needs to focus more on memory leaks; I plan on making a whole separate article for memory managemenet later.

Remember that you only need to stay below ~12 milliseconds per frame. Memory and bandwidth are also important performance factors, and usually most possible optimizations that save time aren't really going to help you.

## Topic #4: Speed over everything
- Do not fork a library just to micro-optimize it and then call it better.
- Do not remake a library just to make it micro-optimized.
- Do not switch libraries for the sole purpose of a speed gain.

I will maybe add more to this later.
