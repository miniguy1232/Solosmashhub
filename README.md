local Settings = {
    ESPEnabled = true
}

-- UI
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Solosmash Hub", "Ocean")
local MainTab = Window:NewTab("Main")
local ESPSection = MainTab:NewSection("Fruit ESP")

-- Clear old ESP boxes
local function ClearFruitESP()
    for _, v in pairs(game:GetService("CoreGui"):GetChildren()) do
        if v:IsA("BoxHandleAdornment") and v.Name == "FruitESP" then
            v:Destroy()
        end
    end
end

-- Create new ESP
local function CreateFruitESP()
    ClearFruitESP()
    for _, v in pairs(workspace:GetDescendants()) do
        if v:IsA("Part") and v.Name:lower():find("fruit") then
            local esp = Instance.new("BoxHandleAdornment")
            esp.Name = "FruitESP"
            esp.Adornee = v
            esp.Size = v.Size
            esp.Color3 = Color3.fromRGB(255, 0, 0)
            esp.AlwaysOnTop = true
            esp.ZIndex = 5
            esp.Transparency = 0.4
            esp.Parent = game:GetService("CoreGui")
        end
    end
end

-- Toggle logic
ESPSection:NewToggle("ESP Fruits", "Toggle Fruit ESP", function(state)
    Settings.ESPEnabled = state
    if state then
        task.spawn(function()
            while Settings.ESPEnabled do
                pcall(CreateFruitESP)
                task.wait(2)
            end
        end)
    else
        ClearFruitESP()
    end
end)
