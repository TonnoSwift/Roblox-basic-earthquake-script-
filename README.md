---

## 2) ServerScriptService/EarthquakeServer.lua

```lua
local rs = game:GetService("ReplicatedStorage")
local startEvent = rs:WaitForChild("StartEarthquake")
local earthquakeZone = workspace:WaitForChild("EarthquakeZone")
local RunService = game:GetService("RunService")
local debris = game:GetService("Debris")

local function shakeAndCollapse(intensity, liquefaction, duration)
    print(string.format("[EARTHQUAKE]: Starting with intensity=%.2f, liquefaction=%s, duration=%.1fs", intensity, tostring(liquefaction), duration))
    local startTime = tick()
    while tick() - startTime < duration do
        for _, part in ipairs(earthquakeZone:GetChildren()) do
            if part:IsA("BasePart") then
                local originalPos = part.Position
                local offset = Vector3.new(
                    math.random(-100, 100)/100 * intensity,
                    0,
                    math.random(-100, 100)/100 * intensity
                )
                part.Position = originalPos + offset
            end
        end
        RunService.Heartbeat:Wait()
    end
    
    for _, part in ipairs(earthquakeZone:GetChildren()) do
        if part:IsA("BasePart") then
            part.Anchored = false
            if liquefaction then
                local downForce = Instance.new("BodyVelocity")
                downForce.Velocity = Vector3.new(0, -200, 0)
                downForce.MaxForce = Vector3.new(1e6, 1e6, 1e6)
                downForce.P = 1e4
                downForce.Parent = part
                debris:AddItem(downForce, 1)
            else
                local randomForce = Instance.new("BodyVelocity")
                randomForce.Velocity = Vector3.new(
                    math.random(-50, 50),
                    math.random(0, 100),
                    math.random(-50, 50)
                )
                randomForce.MaxForce = Vector3.new(1e6, 1e6, 1e6)
                randomForce.P = 1e4
                randomForce.Parent = part
                debris:AddItem(randomForce, 0.5)
            end
        end
    end
    print("[EARTHQUAKE]: Complete!")
end

startEvent.OnServerEvent:Connect(function(player, intensity, liquefaction, duration)
    shakeAndCollapse(intensity, liquefaction, duration)
end)
