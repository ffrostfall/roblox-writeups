```lua
local freeThread

local function functionPasser(fn, ...)
	fn(...)
end

local function yielder()
	while true do
		functionPasser(coroutine.yield())
	end
end

local function SpawnWithReuse(fn, ...)
	if not freeThread then
		freeThread = coroutine.create(yielder)
		coroutine.resume(freeThread)
	end
	local acquiredThread = freeThread
	freeThread = nil
	task.spawn(acquiredThread, fn, ...)
	freeThread = acquiredThread
end
```
*(putting credit where it's due, I learned this pattern from @Stravant's GoodSignal)*

---
## What does this do?

This snippet of code allows us to *re-use* threads instead of constantly creating new ones. This helps with performance. While it may look intimidating, it's not that complex.

---
## How does this work?

These lines of code may seem complex and complicated, but I can assure you this process is very simple.

All we really need to know are a few things:
- Threads can only contain one function
-`` task.spawn`` can **resume** a thread, aka make it stop yielding
- We can pass arguments into ``coroutine.yield()`` through ``task.spawn(function, argsPassedIntoYield)``

So let's go over this process real quickly:
1. When we want to spawn a thread, detect if there's already a ``freeThread``
2. If not, create a thread with the function ``yielder``.
3. Resume the thread we just created- this runs the ``yielder`` function.
4. After that, grab the ``freeThread`` reference, and set the freeThread variable to nil. We keep the value this way, but any reference to it is cleared, so nothing else can detect it or interfere.
5. Resume the thread **again**- except this time, it's yielding. Remember how I said we can pass arguments into ``coroutine.yield``? We pass the function through. That runs a function which.. literally just runs the function you put into it. This means that ``yielder``:
    1. Waits until a function gets passed through
    2. When a function does pass through, run it.
    3. Continues waiting
6. After we run ``yielder``, put freeThread back, because it's done- it's not running any code.

---

## Why does this work?

Because in this scenario, we are only creating a thread if we cannot access the existing thread. This means we don't need to consistently spawn threads- which increases the speed ever so slightly. However, this is important at an incredibly large scale when you're spawning threads up to hundreds of times a frame, resuming a thread instead of creating a thread can save a lot of time. This optimization method particularly helped my project BridgeNet- thread reuse is not implemented yet, but it helped speed it up *by over double*.

Hope this explanation helped!

note: I'm aware the example is bugged, I don't care enough to fix it.