Assuming the following is true:
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

This means 9 bytes per "remote" call can be reduced using a specific data structure, like the following:
```lua
-- ID being a 1-character length string
-- ID2 being another 1-character length string
-- Create IDs by keeping track of the number of IDs created, then using string.pack to convert it into "binary" (theres a useless byte in there unfortunately)
{
  [ID] = {
    {A, B, C} -- An individual call: Remote:Fire({ A, B, C })
  } -- Array, indexes are free with no cost.
  [ID2] = {
    {A, B, C} - :Fire({A, B, C})
    {D, E, F} - :Fire({D, E, F})
  }
} -- Dictionary
```

Using this format results in the following benchmark:
Roblox:
![image](https://user-images.githubusercontent.com/80861876/228321062-76bedd2a-12e4-4a71-b986-de123280fc80.png)


BridgeNet2:
![image](https://user-images.githubusercontent.com/80861876/228321084-5d9d1f88-1bea-4c4a-ae3b-941218d7170d.png)
