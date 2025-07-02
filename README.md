
# Roblox Earthquake Script

This is a Roblox server-side script to simulate an earthquake with customizable intensity, liquefaction, and duration. It uses a RemoteEvent named `StartEarthquake` to trigger the earthquake.

## How it works
- Place a Folder or Model named `EarthquakeZone` in Workspace with the parts you want to shake/collapse.
- Place a RemoteEvent named `StartEarthquake` in ReplicatedStorage.
- Add the script from `ServerScriptService/EarthquakeServer.lua` into ServerScriptService.
- Call the RemoteEvent with:
  - `intensity` (number): how much the ground shakes.
  - `liquefaction` (boolean): whether parts should sink straight down.
  - `duration` (number): how long the shaking lasts.

## Example RemoteEvent call
```lua
local rs = game:GetService("ReplicatedStorage")
local startEvent = rs:WaitForChild("StartEarthquake")
startEvent:FireServer(0.5, true, 10)


---

Created for Roblox projects needing a dynamic disaster simulation.
(I hope it works)