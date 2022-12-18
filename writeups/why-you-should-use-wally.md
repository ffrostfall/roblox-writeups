For some context: Wally is a package manager for Roblox. Link is here: https://wally.run/

So firstly, lets talk about what a package manager does, as I'm sure a lot of people don't know what a package manager does- and that's okay! I'll explain it right now. Let's take a few projects: [TopbarPlus by ForeverHD](https://devforum.roblox.com/t/zoneplus-v320-construct-dynamic-zones-and-effectively-determine-players-and-parts-within-their-boundaries/1017701), [ZonePlus by ForeverHD](https://devforum.roblox.com/t/zoneplus-v320-construct-dynamic-zones-and-effectively-determine-players-and-parts-within-their-boundaries/1017701), [ClientCast by Pyseph](https://devforum.roblox.com/t/clientcast-a-client-based-idiosyncratic-hitbox-system/895217/252), [PacketProfiler by Pyseph](https://devforum.roblox.com/t/packet-profiler-accurately-measure-remote-packet-bandwidth/1890924) and [BridgeNet by ffrostflame](https://devforum.roblox.com/t/bridgenet-insanely-optimized-easy-to-use-networking-library-full-of-utilities-now-with-roblox-ts-v199-beta/1909935).  Every single one of these projects- which are **resources** made by the community, have one thing in common: they all use external dependencies (relies on other resources, created by other scripters).

TopbarPlus relies on [Maid by Quenty](https://devforum.roblox.com/t/how-to-use-a-maid-class-on-roblox-to-manage-state/340061)
ZonePlus relies on [Janitor by HowManySmall](https://github.com/howmanysmall/Janitor)
BridgeNet relies on [Promise by evaera](https://github.com/evaera/roblox-lua-promise), and [GoodSignal by Stravant](https://devforum.roblox.com/t/lua-signal-class-comparison-optimal-goodsignal-class/1387063)
ClientCast relies on [GoodSignal by Stravant](https://devforum.roblox.com/t/lua-signal-class-comparison-optimal-goodsignal-class/1387063)
PacketProfiler relies on [Roact by Roblox](https://github.com/Roblox/roact/), and  [GoodSignal by Stravant](https://devforum.roblox.com/t/lua-signal-class-comparison-optimal-goodsignal-class/1387063)
*Fun fact: in that list alone, three of those projects use Wally*

A package is essentially an open-source resource for others to use. A package manager manages your packages and dependencies (some packages rely on other packages!). What exactly does that mean?

The first thing a package manager does is make things incredibly easy to install. You don't need to manually download anything, manually update anything by dragging files, moving stuff around. You just edit a file a little bit, and the package manager updates it for you.

The second thing a package manager does is manage dependencies. As I mentioned, a lot of packages use at least one dependency- so by using a package manager, it automatically updates **those dependencies for you as well**. Awesome! Never worry about drag-n-drop file nightmares again.

So onto the third thing a project manager does, and this gets a bit in-depth, so I'll start this simple. One thing you might've noticed with the list of packages I gave, is that ClientCast, PacketProfiler, and BridgeNet both rely on GoodSignal. So what does this mean if you're using BridgeNet, ClientCast, and PacketProfiler?

It means you have **three** duplicate scripts in your game which does literally the same thing, with slightly less performance because GoodSignal benefits from coroutine re-usage. Same thing issue with Promise, same issue with Janitor. Package managers alleviate this issue by having **one** main package that every dependency points to.

Oh, yeah, and what happens when both you and a project use GoodSignal? If you use GoodSignal too, then you have FOUR duplicate scripts in your game by using these packages! And there's *no way* that's a reason to not use those packages.

Wally, as a package manager, fixes this. On the surface, without looking inside, it looks like these are duplicates- but they're not. Every ModuleScript you see inside of a package- for example, ``ffrostflame_bridgenet@.0.0-rc2/Promise``, is actually a 1-line script that requires the real promise. Same thing with all the packages in the top Packages folder!
![image|387x307](upload://u0WIkMtpMa0y9HKEb49Oo9kxmj7.png)

(promise in ffrostflame_bridgenet@2.0.0-rc2)
```lua
return require(script.Parent.Parent["evaera_promise@4.0.0"]["promise"])
```

So using Wally, to get a package, you would just do ``require(ReplicatedStorage.Packages.GoodSignal)``! Very easy, you don't need to change parents or anything like that.

But there's a problem with this. Sometimes packages aren't on Wally, and sometimes dependencies of projects on Wally are just files within the projects source code- with the issues as mentioned :thinking:. So, with this, we can reasonably come to the conclusion that the *more developers that use Wally*, the better, and richer, Wally's database of packages will be.

In fact, one of my main pain-points when developing my package BridgeNet was adding support for non-wally users. It's why the Roblox marketplace version is ever so slightly out of date! Every single time I finish a new version, I need to edit it to not use Wally, and then upload it to a few different places. It's annoying to do. [Knit, a popular framework made by sleitnick](https://github.com/Sleitnick/Knit), has a complex process for uploading non-wally versions. [Look at this script that sleitnick made to do it!](https://github.com/Sleitnick/Knit/blob/main/.github/workflows/release.yaml#L32) It's needlessly complex, as he should be able to just upload it to wally without worry.

That's why I'm asking *you* to use Wally, for the benefit of both yourself, other developers, and for the sake of package creators. Wally is maintained by some *really* great people, and it's criminally underused. I'm extremely grateful to Wally's developers. All your favorite packages **are most likely on wally**. And if they aren't, you can upload them yourself! If you're worried about using others uploaded projects- **any downloaded packages are visible to you as a developer as well.** It's a public registry! Anyone can submit a project for usage.
![image|630x159](upload://aLWAirZA7Ng0a59WoESPXF6cMaV.png)
![image|323x182](upload://Aatq1LbnapaiPzNQY6OJUwDxH2k.png)
*screenshots of wally users uploading projects not on wally, to wally*

Updating your packages is as *simple* as running a single command: ``wally install``
![image|627x145](upload://cxULaj054c4CCqXMywzcsu7lPjF.png)

Downloading packages is as simple as dropping a few quick lines into a file.
![image|690x139](upload://l9OU9RStCHPbwQnkL4glWwpiQhf.png)