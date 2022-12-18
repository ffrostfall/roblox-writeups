Odds are, if you have Rojo, you have at least 2 of these installed. I like using all of them, and I'll explain why they're really cool here, and how they fit into my workflow- because there's not much information on this tooling out there!

# First extension: Selene
[Selene by Kampfkarren](https://kampfkarren.github.io/selene/roblox.html) is an opinionated linter designed for Roblox- but can be used outside of Roblox. It's a great tool- it helps catch bugs, and keeps your code nice and idiomatic.

One of the biggest parts about development in general is keeping your code clean and readable, which is exactly why selene does. Your code being idiomatic is important, because the majority of people will be able to understand and read it. Selene helps keep your code idiomatic- it catches things like multi-line statements, duplicate keys in tables, empty if statements, unused variables, etc. etc. One really cool thing Selene also does is automatically flag deprecated functions, which means you'll instantly know when something you use is deprecated.

![image|580x85](upload://l8IA6h53X5mnt8jLOfZe2BscLwe.png)


# Second extension: StyLua
[StyLua by JohnnyMorganz](https://marketplace.visualstudio.com/items?itemName=JohnnyMorganz.stylua) is Lua formatter that goes hand-in-hand with Selene because a lot of the formatting lints are fixed by StyLua. StyLua follows the [ Roblox style guide](https://roblox.github.io/lua-style-guide/)- which I recommend you follow. This means your code will always be formatted in a way that's readable, idiomatic, and follows a style guide that **Roblox themselves follows**. Since StyLua is an auto-formatter, it can be as simple as pressing Ctrl + S to auto-format your code, which is very helpful. 
![image|549x73](upload://8QiJK9D77xmqoxbg9C9pRuvEUGX.png)
![image|690x290](upload://vcRvttC7spcgLwJovhO43KCJWyQ.png)
*(my personal formatting settings)*

# Third extension: Roblox LSP
[Roblox LSP by Nightrains](https://devforum.roblox.com/t/roblox-lsp-full-intellisense-for-roblox-and-luau/717745) really is the glue here- it provides highlighting, typechecking, all that for Roblox. It even provides autocomplete inside vscode for things like folders in workspace, which is very nice. I don't have very much to say about it myself, but it's impressive. I use it *alongside* selene, with both Roblox LSP diagnostics, and selene's lints as well. It's what works for me.

---

While a lot of what I've said is personalized towards me, I encourage you to find your own balance- don't shy away from Rojo because it feels awkward for you, because there's a lot you can personalize. Look at the documentation for selene, Roblox LSP, look at the settings, and try figuring it out! You can go to an extension's settings through here: 
![image|468x369](upload://oPf0MOtBbb2wVkqEMNVsqGlzLjP.png)