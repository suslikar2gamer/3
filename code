-- Ensure necessary services are properly defined
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

local Players = game:GetService("Players")
local players = game:GetService("Players")
local ReplicatedFirst = game:GetService("ReplicatedFirst")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Camera = Workspace.CurrentCamera



local LocalPlayer = Players.LocalPlayer
local PlayerService = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Mannequin = ReplicatedStorage.Assets.Mannequin
local LootBins = Workspace.Map.Shared.LootBins
local Randoms = Workspace.Map.Shared.Randoms
local Vehicles = Workspace.Vehicles.Spawned
local Characters = Workspace.Characters
local Corpses = Workspace.Corpses
local Zombies = Workspace.Zombies
local Loot = Workspace.Loot
local last_hitbox_update = 0


local Framework = require(ReplicatedFirst:WaitForChild("Framework"))
Framework:WaitForLoaded()

repeat task.wait() until Framework.Classes.Players.get()
local PlayerClass = Framework.Classes.Players.get()

local Globals = Framework.Configs.Globals
local World = Framework.Libraries.World
local Network = Framework.Libraries.Network
local Cameras = Framework.Libraries.Cameras
local Bullets = Framework.Libraries.Bullets
local Lighting = Framework.Libraries.Lighting
local Interface = Framework.Libraries.Interface
local Resources = Framework.Libraries.Resources
local Raycasting = Framework.Libraries.Raycasting
local Characters = Workspace.Characters
local Corpses = Workspace.Corpses
local Zombies = Workspace.Zombies
local Maids = Framework.Classes.Maids
local Animators = Framework.Classes.Animators
local VehicleController = Framework.Classes.VehicleControler

local Firearm = nil
task.spawn(function() 
    setthreadidentity(2)
    Firearm = require(ReplicatedStorage.Client.Abstracts.ItemInitializers.Firearm)
end)

local ItemData = Framework.Configs.ItemData
local Tick = tick()
local InstanceNew = Instance.new
local last_shot_time = 0
local Events = getupvalue(Network.Add, 1)
local GetFireImpulse = getupvalue(Bullets.Fire, 6)
local GetSpreadAngle = getupvalue(Bullets.Fire, 1)
local GetSpreadVector = getupvalue(Bullets.Fire, 3)
local CastLocalBullet = getupvalue(Bullets.Fire, 4)
local LightingState = getupvalue(Lighting.GetState, 1)
local AnimatedReload = getupvalue(Firearm, 7)

for i,v in pairs(getconnections(game:GetService("ScriptContext").Error)) do
    v:Disconnect()
end
coroutine.wrap(function()
    while task.wait(1) do
        for i,v in pairs(getconnections(game:GetService("ScriptContext").Error)) do
            v:Disconnect()
        end
    end
end)()

local Framework = require(game.ReplicatedFirst.Framework)

-- Credits To The Original Devs @xz, @goof
getgenv().Config = {
	Invite = "Firearm.xyz | AR2",
	Version = "0.0",
}

getgenv().luaguardvars = {
	DiscordName = "User#69",
}

local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Owen301313/Metal.xyz-Scripts/refs/heads/main/customUI.lua"))()

library:init() -- Initalizes Library Do Not Delete This

local Window = library.NewWindow({
	title = "Firearm.xyz x Antihook.xyz - Swift",
	size = UDim2.new(0, 530, 0, 655)
})

local tabs = {
    Tab1 = Window:AddTab("  Main  "),
    Tab2 = Window:AddTab("  Visuals  "),
    RageTab = Window:AddTab("  Rage  "),
    Tab3 = Window:AddTab("  Misc  "),
	Settings = library:CreateSettingsTab(Window),
}

-- 1 = Set Section Box To The Left
-- 2 = Set Section Box To The Right

local sections = {
	Section1 = tabs.Tab1:AddSection("Aimbot", 1),
	Section2 = tabs.RageTab:AddSection("Head Expander", 1),
	Section5 = tabs.Tab1:AddSection("Gun Mods", 2),
	Section4 = tabs.RageTab:AddSection("Rage Bot", 2),
    WallBangSection = tabs.RageTab:AddSection("Wallbang", 1),
	loadoutSection = tabs.Tab3:AddSection("Clothing", 1),
    VehicleSection = tabs.Tab3:AddSection("Vehicles", 1),
	miscMovementSection = tabs.Tab3:AddSection("Fly", 2),
    ZombiesTab = tabs.Tab3:AddSection("Anti Zombie", 2),
    MeleeAura = tabs.Tab3:AddSection("Fast Knife", 2),
	Visuals = tabs.Tab2:AddSection("Players", 1),
	WorldVisuals = tabs.Tab2:AddSection("World", 2),
    localPlayer = tabs.Tab2:AddSection("Local Player", 2)
}

local AimbotSettings = {
    Enabled = false,
    MaxDistance = 10000,
    TargetFOV = 200,
    TargetPart = "Head",
    ShowFOV = false
}

local mt = getrawmetatable(game)
setreadonly(mt, false)
local oldIndex = mt.__index
local last_hitbox_update = 0
local HitboxSettings = {
    Enabled = false,
    Size = 15
}


mt.__index = newcclosure(function(self, k)
    if k == "Size" and self:IsA("BasePart") and self.Name == "Head" then
        return Vector3.new(1.15, 1.15, 1.15)
    end
    return oldIndex(self, k)
end)


local function updateHitboxSize(size)
    if not HitboxSettings.Enabled then return end
    
    pcall(function()
        for _, player in pairs(players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character then
                local head = player.Character:FindFirstChild("Head")
                if head and typeof(head) == "Instance" then
                    head.Size = Vector3.new(size, size, size)
                end
            end
        end
    end)
end


RunService.RenderStepped:Connect(function()

    local current_time = tick()
    
    if HitboxSettings.Enabled and current_time - last_hitbox_update >= 0.1 then
        updateHitboxSize(HitboxSettings.Size)
        last_hitbox_update = current_time
    end
end)











-- FOV Circle Setup
local FOVCircle = Drawing.new("Circle")
FOVCircle.Visible = false
FOVCircle.Color = Color3.fromRGB(255, 0, 0)  -- Default to red
FOVCircle.Thickness = 2
FOVCircle.Filled = false

local function UpdateFOVCircle()
    if AimbotSettings.ShowFOV then
        local MousePos = UserInputService:GetMouseLocation()
        FOVCircle.Position = MousePos
        FOVCircle.Radius = AimbotSettings.TargetFOV
        FOVCircle.Visible = true
    else
        FOVCircle.Visible = false
    end
end

local function GetCharacter(Player)
    return Player.Character or workspace:FindFirstChild(Player.Name)
end

local function UpdateAimbot()
    if not AimbotSettings.Enabled then return end
    if not UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then return end

    local ClosestPlayer, ClosestAngle = nil, AimbotSettings.TargetFOV
    
    for _, Player in ipairs(Players:GetPlayers()) do
        if Player == LocalPlayer then continue end
        local Character = GetCharacter(Player)
        if not Character then continue end

        local TargetPart = Character:FindFirstChild(AimbotSettings.TargetPart) or Character:FindFirstChild("HumanoidRootPart")
        if not TargetPart then continue end

        local Humanoid = Character:FindFirstChildOfClass("Humanoid")
        if not Humanoid or Humanoid.Health <= 0 then continue end

        local ScreenPos, OnScreen = Camera:WorldToViewportPoint(TargetPart.Position)
        if not OnScreen then continue end

        local Distance = (TargetPart.Position - Camera.CFrame.Position).Magnitude
        if Distance > AimbotSettings.MaxDistance then continue end

        local MousePos = UserInputService:GetMouseLocation()
        local AngleFromMouse = (Vector2.new(ScreenPos.X, ScreenPos.Y) - MousePos).Magnitude

        if AngleFromMouse < ClosestAngle then
            ClosestPlayer, ClosestAngle = Player, AngleFromMouse
        end
    end

    if ClosestPlayer then
        local Character = GetCharacter(ClosestPlayer)
        if Character then
            local TargetPart = Character:FindFirstChild(AimbotSettings.TargetPart)
            if TargetPart then
                local ScreenPosition, OnScreen = Camera:WorldToViewportPoint(TargetPart.Position)
                if OnScreen then
                    local MousePos = UserInputService:GetMouseLocation()
                    mousemoverel((ScreenPosition.X - MousePos.X) / 2, (ScreenPosition.Y - MousePos.Y) / 2)
                end
            end
        end
    end
end

-- Ensure RunService is valid before connecting
RunService.RenderStepped:Connect(UpdateAimbot)
RunService.RenderStepped:Connect(UpdateFOVCircle)

-- Aimbot UI Toggles and Sliders
local AimbotToggle = sections.Section1:AddToggle({
    enabled = true,
    text = "Aimbot",
    flag = "Enable_AimbotToggle",
    risky = true,
    callback = function(value)
        AimbotSettings.Enabled = value
    end
})

local FOVToggle = sections.Section1:AddToggle({
    enabled = true,
    text = "Show FOV Circle",
    flag = "ShowFOV_Toggle",
    callback = function(value)
        AimbotSettings.ShowFOV = value
    end
})



sections.Section1:AddSlider({
    text = "FOV Size", 
    flag = 'AimFOV_Slider', 
    value = AimbotSettings.TargetFOV,
    min = 10, 
    max = 1000,
    increment = 1,
    callback = function(value) 
        AimbotSettings.TargetFOV = value
    end
})




sections.Section2:AddToggle({
    enabled = true,
    text = "Hitbox Expander",
    flag = "hitboxEnabled",
    risky = true,
    callback = function(Value)
        HitboxSettings.Enabled = Value
        if not Value then
            pcall(function()
                for _, player in pairs(players:GetPlayers()) do
                    if player ~= LocalPlayer and player.Character then
                        local head = player.Character:FindFirstChild("Head")
                        if head and typeof(head) == "Instance" then
                            head.Size = Vector3.new(1, 1, 1)
                        end
                    end
                end
            end)
        end
    end,
})


sections.Section2:AddSlider({
	text = "Hitbox Size", 
	flag = 'hitboxSize', 
	suffix = "", 
	value = HitboxSettings.Size,
	min = 0.1, 
	max = 30,
	increment = 0.001,
	risky = false,
	callback = function(v) 
		print("Slider Value Is Now : ".. v)
	end
})


local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local Camera = workspace.CurrentCamera

local tracer = Drawing.new("Line")
tracer.Thickness = 2
tracer.Color = Color3.new(1, 0, 0) -- Red
tracer.Transparency = 1
tracer.Visible = false

local tracerEnabled = false

-- Define getNearestPlayer function first
local function getNearestPlayer()
    local nearestPlayer = nil
    local shortestDistance = math.huge
    
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local screenPoint, onScreen = Camera:WorldToViewportPoint(player.Character.HumanoidRootPart.Position)
            
            if onScreen then
                local distance = (Vector2.new(Mouse.X, Mouse.Y) - Vector2.new(screenPoint.X, screenPoint.Y)).Magnitude
                
                if distance < shortestDistance then
                    shortestDistance = distance
                    nearestPlayer = player.Character.HumanoidRootPart
                end
            end
        end
    end
    return nearestPlayer
end

-- Add the toggle inside the section
sections.Section1:AddToggle({
    enabled = true,
    text = "Target Indicator",
    flag = "Target_Indicator",
    callback = function(value)
        tracerEnabled = value
        if not tracerEnabled then
            tracer.Visible = false
        end
    end
})

RunService.RenderStepped:Connect(function()
    if not tracerEnabled then return end
    
    local nearestPart = getNearestPlayer()  -- This will work now since the function is defined
    
    if nearestPart then
        local startPos = Vector2.new(Mouse.X, Mouse.Y)
        local endPos, onScreen = Camera:WorldToViewportPoint(nearestPart.Position)
        
        if onScreen then
            tracer.From = startPos
            tracer.To = Vector2.new(endPos.X, endPos.Y)
            tracer.Visible = true
        else
            tracer.Visible = false
        end
    else
        tracer.Visible = false
    end
end)

sections.Section1:AddSeparator({
	text = "Silent Aim"
})



local mt = getrawmetatable(game)
setreadonly(mt, false)
local oldIndex = mt.__index
local last_hitbox_update = 0
local HitboxSettings = {
    Enabled = false,
    Size = 15
}


mt.__index = newcclosure(function(self, k)
    if k == "Size" and self:IsA("BasePart") and self.Name == "Head" then
        return Vector3.new(1.15, 1.15, 1.15)
    end
    return oldIndex(self, k)
end)


local function updateHitboxSize(size)
    if not HitboxSettings.Enabled then return end
    
    pcall(function()
        for _, player in pairs(players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character then
                local head = player.Character:FindFirstChild("Head")
                if head and typeof(head) == "Instance" then
                    head.Size = Vector3.new(size, size, size)
                end
            end
        end
    end)
end


RunService.RenderStepped:Connect(function()

    local current_time = tick()
    
    if HitboxSettings.Enabled and current_time - last_hitbox_update >= 0.1 then
        updateHitboxSize(HitboxSettings.Size)
        last_hitbox_update = current_time
    end
end)

local function NewDrawing(Type, Properties)
    local Object = Drawing.new(Type)
    for Property, Value in pairs(Properties) do
        Object[Property] = Value
    end
    return Object
end



local ESPSettings = {
    Enabled = false,
    ShowNames = false,
    ShowDistance = false,
    ShowHealth = false,
    ShowBoxes = false,
    ShowTracers = false,
    ShowChams = false,
    MaxDistance = 10000
}


local ESPObjects = {}
local ChamsObjects = {}

local function RemoveChams(Player)
    if ChamsObjects[Player] then
        ChamsObjects[Player]:Destroy()
        ChamsObjects[Player] = nil
    end
end

local function RemoveESP(Player)
    if ESPObjects[Player] then
        for _, Obj in pairs(ESPObjects[Player]) do
            Obj:Remove()
        end
        ESPObjects[Player] = nil
    end
    RemoveChams(Player)
end

local function AddChams(Player)
    if Player == LocalPlayer or ChamsObjects[Player] then return end

    local Character = Player.Character
    if not Character then return end

    local Highlight = Instance.new("Highlight")
    Highlight.Adornee = Character
    Highlight.FillColor = Color3.new(1, 0, 0)
    Highlight.OutlineColor = Color3.new(0, 0, 0)
    Highlight.FillTransparency = 0.5
    Highlight.OutlineTransparency = 0
    Highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
    Highlight.Parent = Character

    ChamsObjects[Player] = Highlight
end

local function AddESP(Player)
    if Player == LocalPlayer or ESPObjects[Player] then return end



    if ESPSettings.ShowChams then
        AddChams(Player)
    end
end

local function UpdateESP(Player)
    local ESP = ESPObjects[Player]
    if not ESP then return end

    local Character = Player.Character
    local RootPart = Character and Character:FindFirstChild(AimbotSettings.TargetPart)

    if not RootPart then
        for _, Obj in pairs(ESP) do
            Obj.Visible = false
        end
        RemoveChams(Player)
        return
    end

    local Pos, OnScreen = Camera:WorldToViewportPoint(RootPart.Position)
    local Distance = (Camera.CFrame.Position - RootPart.Position).Magnitude

    if OnScreen and ESPSettings.Enabled and Distance <= ESPSettings.MaxDistance then
        local ScaleFactor = 1 / (Distance / 100)
        local BoxSize = Vector2.new(40, 60) * ScaleFactor
        local TopLeft = Vector2.new(Pos.X - BoxSize.X / 2, Pos.Y - BoxSize.Y / 2)

      
    else
        for _, Obj in pairs(ESP) do
            Obj.Visible = false
        end
    end
end


Players.PlayerAdded:Connect(function(Player)
    AddESP(Player)
    Player.CharacterAdded:Connect(function()
        AddESP(Player)
    end)
end)

Players.PlayerRemoving:Connect(function(Player)
    RemoveESP(Player)
end)

for _, Player in ipairs(Players:GetPlayers()) do
    AddESP(Player)
end


RunService.RenderStepped:Connect(function()

    UpdateAimbot()






    for _, Player in ipairs(Players:GetPlayers()) do
        if Player ~= LocalPlayer then
            UpdateESP(Player)
        end
    end
end)

local function GetHealth(Player)
    if Player and Player:FindFirstChild("Stats") then
        local Health = Player.Stats:FindFirstChild("Health")
        if Health and Health:IsA("NumberValue") then
            return Health.Value, 100
        end
    end
    return 0, 100
end

local function AddChams(Player)
    if Player == LocalPlayer or ChamsObjects[Player] then return end

    local Character = Player.Character
    if not Character then return end

    local Highlight = Instance.new("Highlight")
    Highlight.Adornee = Character
    Highlight.FillColor = Color3.new(1, 0, 0) 
    Highlight.OutlineColor = Color3.new(0, 0, 0)
    Highlight.FillTransparency = 0.5
    Highlight.OutlineTransparency = 0
    Highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
    Highlight.Parent = Character

    ChamsObjects[Player] = Highlight
end


local function RemoveChams(Player)
    if ChamsObjects[Player] then
        ChamsObjects[Player]:Destroy()
        ChamsObjects[Player] = nil
    end
end


local function AddESP(Player)
    if Player == LocalPlayer or ESPObjects[Player] then return end

    ESPObjects[Player] = {
        BoxOutline = NewDrawing("Square", {Thickness = 3, Color = Color3.new(0, 0, 0), Transparency = 0.75, Visible = false}),
        Box = NewDrawing("Square", {Thickness = 1, Color = Color3.new(1, 1, 1), Transparency = 1, Visible = false}),
        TracerOutline = NewDrawing("Line", {Thickness = 3, Color = Color3.new(0, 0, 0), Transparency = 0.75, Visible = false}),
        Tracer = NewDrawing("Line", {Thickness = 1, Color = Color3.new(255, 255, 255), Transparency = 1, Visible = false}),
        NameTag = NewDrawing("Text", {Size = 13, Center = true, Outline = true, Color = Color3.new(1, 1, 1), Visible = false}),
        DistanceTag = NewDrawing("Text", {Size = 12, Center = true, Outline = true, Color = Color3.new(1, 1, 1), Visible = false}),
        HealthBarOutline = NewDrawing("Line", {Thickness = 3, Color = Color3.new(0, 0, 0), Transparency = 1, Visible = false}),
        HealthBar = NewDrawing("Line", {Thickness = 2, Color = Color3.new(0, 1, 0), Transparency = 1, Visible = false}),
    }

    if ESPSettings.ShowChams then
        AddChams(Player)
    end
end


RunService.RenderStepped:Connect(function()
    for _, Player in pairs(Players:GetPlayers()) do
        if Player == LocalPlayer then continue end

        if not ESPObjects[Player] then
            AddESP(Player)
        end

        local ESP = ESPObjects[Player]
        local Character = Player.Character
        local RootPart = Character and Character:FindFirstChild("HumanoidRootPart")

        if not RootPart then
            for _, Obj in pairs(ESP) do
                Obj.Visible = false
            end
            RemoveChams(Player)
            continue
        end

        local Pos, OnScreen = Camera:WorldToViewportPoint(RootPart.Position)
        local Distance = (Camera.CFrame.Position - RootPart.Position).Magnitude

        if OnScreen and ESPSettings.Enabled and Distance <= ESPSettings.MaxDistance then
            local Health, MaxHealth = GetHealth(Player)
            local HealthPercent = Health / MaxHealth

           
            if ESPSettings.ShowChams and ChamsObjects[Player] then
                ChamsObjects[Player].FillColor = Color3.fromRGB(255 - (HealthPercent * 255), HealthPercent * 255, 0)
            elseif ChamsObjects[Player] then
                RemoveChams(Player)
            end

            local ScaleFactor = 1 / (Distance / 100)
            local BoxSize = Vector2.new(40, 60) * ScaleFactor
            local TopLeft = Vector2.new(Pos.X - BoxSize.X / 2, Pos.Y - BoxSize.Y / 2)
            ESP.BoxOutline.Visible = ESPSettings.ShowBoxes
            ESP.Box.Visible = ESPSettings.ShowBoxes
            if ESPSettings.ShowBoxes then
                ESP.BoxOutline.Position = TopLeft
                ESP.BoxOutline.Size = BoxSize

                ESP.Box.Position = TopLeft
                ESP.Box.Size = BoxSize
            end

            ESP.TracerOutline.Visible = ESPSettings.ShowTracers
            ESP.Tracer.Visible = ESPSettings.ShowTracers
            if ESPSettings.ShowTracers then
                ESP.TracerOutline.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                ESP.TracerOutline.To = Vector2.new(Pos.X, Pos.Y)

                ESP.Tracer.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                ESP.Tracer.To = Vector2.new(Pos.X, Pos.Y)
            end


            ESP.NameTag.Visible = ESPSettings.ShowNames
            if ESPSettings.ShowNames then
                ESP.NameTag.Text = Player.Name
                ESP.NameTag.Position = Vector2.new(Pos.X, TopLeft.Y - 15)
            end

            ESP.DistanceTag.Visible = ESPSettings.ShowDistance
            if ESPSettings.ShowDistance then
                ESP.DistanceTag.Text = string.format("[%d]", Distance)
                ESP.DistanceTag.Position = Vector2.new(Pos.X, TopLeft.Y - 30)
            end

            ESP.HealthBarOutline.Visible = ESPSettings.ShowHealth
            ESP.HealthBar.Visible = ESPSettings.ShowHealth
            if ESPSettings.ShowHealth then
                local BarHeight = BoxSize.Y
                local HealthBarHeight = BarHeight * HealthPercent

                ESP.HealthBarOutline.From = Vector2.new(TopLeft.X - 6, TopLeft.Y)
                ESP.HealthBarOutline.To = Vector2.new(TopLeft.X - 6, TopLeft.Y + BarHeight)

                ESP.HealthBar.From = Vector2.new(TopLeft.X - 6, TopLeft.Y + BarHeight)
                ESP.HealthBar.To = Vector2.new(TopLeft.X - 6, TopLeft.Y + BarHeight - HealthBarHeight)
                ESP.HealthBar.Color = Color3.fromRGB(255 - (HealthPercent * 255), HealthPercent * 255, 0)
            end
        else
            for _, Obj in pairs(ESP) do
                Obj.Visible = false
            end
        end
    end
end)

for _, Player in pairs(Players:GetPlayers()) do
    AddESP(Player)
end

sections.Visuals:AddToggle({
    enabled = true,
   text = "ESP Enabled",
   flag = "espEnabled", 
   callback = function(Value)
   ESPSettings.Enabled = Value
   end,
  
})

sections.Visuals:AddToggle({
    enabled = true,
   text = "Show Names",
   flag = "showNames", 
     callback = function(Value)
   ESPSettings.ShowNames = Value
  end
})


sections.Visuals:AddToggle({
    enabled = true,
   text = "Show Distance",
   flag = "showDistance", 
   callback = function(Value)
   ESPSettings.ShowDistance = Value
  end,
})

sections.Visuals:AddToggle({
   text = "Show Boxes",
   enabled = true,
   flag = "showBoxes", 
     callback = function(Value)
   ESPSettings.ShowBoxes = Value

  end,
})

sections.Visuals:AddToggle({
  enabled = true,
	text = "Show Tracers",
	flag = "showTracers",
   callback = function(Value)
   ESPSettings.ShowTracers = Value
  end,
})

sections.Visuals:AddToggle({
  enabled = true,
	text = "Show Chams",
	flag = "showChams",
   callback = function(Value)
   ESPSettings.ShowChams = Value
  end,
})

sections.Visuals:AddToggle({
  enabled = true,
	text = "Show Health",
	flag = "showHealth",
   callback = function(Value)
   ESPSettings.ShowHealth = Value
  end,
})

sections.Visuals:AddSlider({
	text = "Max Distance", 
	flag = 'Slider_1', 
	suffix = "", 
	value = ESPSettings.MaxDistance,
	min = 1000, 
	max = 10000,
	increment = 0.001,
	tooltip = false,
	risky = false,
	callback = function(v) 
		ESPSettings.MaxDistance = v
	end
})


-- Add Color Picker to change FOV circle color
FOVToggle:AddColor({
    enabled = true,
    text = "FOV Circle Color",  -- Label for the color picker
    flag = "Color_1",  -- Flag to store the color value
    tooltip = "Pick a color for the FOV Circle",  -- Tooltip
    color = FOVCircle.Color,  -- Default color for FOV circle
    trans = 0,  -- Transparency
    open = false,  -- If the picker is open by default
    callback = function(color)
        FOVCircle.Color = color  -- Update the FOV circle color when picked
    end
})

local function trim(s)
    return s:match("^%s*(.-)%s*$")
end

local function getMappedOptions(folderName, filterEquipSlot)
    local folder = game:GetService("ReplicatedStorage").ItemData[folderName]
    if not folder then
        warn("Folder not found: " .. folderName)
        return {}, {}
    end

    local displayList = {}
    local mapping = {}
    for _, moduleScript in ipairs(folder:GetChildren()) do
        if moduleScript:IsA("ModuleScript") then
            local success, data = pcall(require, moduleScript)
            if success and data then
                local isSpecial = data.EventItem or data.LegacyItem or data.RareItem or data.SpecialItem
                if isSpecial then
                    if filterEquipSlot then
                        if data.EquipSlot == filterEquipSlot and data.DisplayName then
                            local disp = trim(data.DisplayName)
                            mapping[disp] = trim(moduleScript.Name)
                            table.insert(displayList, disp)
                            print("Found [" .. filterEquipSlot .. "] option (DisplayName):", disp)
                        end
                    else
                        if data.DisplayName then
                            local disp = trim(data.DisplayName)
                            mapping[disp] = trim(moduleScript.Name)
                            table.insert(displayList, disp)
                            print("Found option in folder " .. folderName .. " (DisplayName):", disp)
                        end
                    end
                end
            else
                warn("ModuleScript " .. moduleScript.Name .. " failed to load or has invalid data.")
            end
        end
    end
    table.sort(displayList)
    return displayList, mapping
end

-- Get clothing options (same as original)
local topDisplay, topMapping = getMappedOptions("Clothing\013", "Top")
local bottomDisplay, bottomMapping = getMappedOptions("Clothing\013", "Bottom")
local hatDisplay, hatMapping = getMappedOptions("Hats\013")
local vestDisplay, vestMapping = getMappedOptions("Vests\013")
local beltDisplay, beltMapping = getMappedOptions("Belts\013")

-- Initialize selected clothing variables
local selectedClothes = {
    Top = (#topDisplay > 0 and topMapping[topDisplay[1]]) or "None",
    Bottom = (#bottomDisplay > 0 and bottomMapping[bottomDisplay[1]]) or "None",
    Hat = (#hatDisplay > 0 and hatMapping[hatDisplay[1]]) or "None",
    Vest = (#vestDisplay > 0 and vestMapping[vestDisplay[1]]) or "None",
    Belt = (#beltDisplay > 0 and beltMapping[beltDisplay[1]]) or "None",
}

-- Create the loadout tab and section

-- Add dropdowns for each clothing type (top, bottom, hat, vest, belt)
sections.loadoutSection:AddList({
    text = "Select Top",
    flag = "selectTop",
    values = topDisplay,
    callback = function(value)
        selectedClothes.Top = topMapping[value]
        print("Selected Top (DisplayName):", value, "-> Module Name:", selectedClothes.Top)
    end
})

sections.loadoutSection:AddList({
    text = "Select Bottom",
    flag = "selectBottom",
    values = bottomDisplay,
    callback = function(value)
        selectedClothes.Bottom = bottomMapping[value]
        print("Selected Bottom (DisplayName):", value, "-> Module Name:", selectedClothes.Bottom)
    end
})

sections.loadoutSection:AddList({
    text = "Select Hat",
    flag = "selectHat",
    multi = false,
    open = true,
    values = hatDisplay,
    callback = function(value)
        selectedClothes.Hat = hatMapping[value]
        print("Selected Hat (DisplayName):", value, "-> Module Name:", selectedClothes.Hat)
    end
})

sections.loadoutSection:AddList({
    text = "Select Vest",
    flag = "selectVest",
    values = vestDisplay,
    callback = function(value)
        selectedClothes.Vest = vestMapping[value]
        print("Selected Vest (DisplayName):", value, "-> Module Name:", selectedClothes.Vest)
    end
})

sections.loadoutSection:AddList({
    text = "Select Belt",
    flag = "selectBelt",
    values = beltDisplay,
    multi = false,
    open = true,
    callback = function(value)
        selectedClothes.Belt = beltMapping[value]
        print("Selected Belt (DisplayName):", value, "-> Module Name:", selectedClothes.Belt)
    end
})

-- Add the button to equip the selected clothing loadout
sections.loadoutSection:AddButton({
    text = "Equip Clothing Loadout",
    callback = function()
        local Creator = require(game:GetService("ReplicatedStorage").Client.Abstracts.Interface.MainMenuClasses.TabClasses.Creator)
        local setEquipSlot = getupvalue(Creator, 34)
        
        print("Equipping with:")
        print(" Top: " .. selectedClothes.Top)
        print(" Bottom: " .. selectedClothes.Bottom)
        print(" Hat: " .. selectedClothes.Hat)
        print(" Vest: " .. selectedClothes.Vest)
        print(" Belt: " .. selectedClothes.Belt)
        
        setEquipSlot("Top", { Status = "Unlocked", Name = selectedClothes.Top }, true)
        setEquipSlot("Bottom", { Status = "Unlocked", Name = selectedClothes.Bottom }, true)
        setEquipSlot("Hat", { Status = "Unlocked", Name = selectedClothes.Hat }, true)
        setEquipSlot("Vest", { Status = "Unlocked", Name = selectedClothes.Vest }, true)
        setEquipSlot("Belt", { Status = "Unlocked", Name = selectedClothes.Belt }, true)
        
        print("Selected clothing loadout equipped!")
    end
})

-- Fly Settings and State Setup
-- Fly Settings and State Setup
local FlySettings = {
    Enabled = false,
    Speed = 0.15,
    LastTick = tick(),
    StateIndex = 1,  
    LastStateUpdate = tick(),
    LastGroundUpdate = tick()
}

local FlyToggle = sections.miscMovementSection:AddToggle({
    enabled = true,
    text = "Fly",
    flag = "flyEnabled",
    risky = true,
    callback = function(value)
        FlySettings.Enabled = value
    end
})

FlyToggle:AddBind({
	text = "Fly Keybind",
	flag = "flyEnabledB",
	nomouse = true,
	noindicator = true,
	tooltip = "Enables fly with a keybind.",
	mode = "toggle",
	bind = Enum.KeyCode.Q,
	risky = false,
	keycallback = function(v)
	    FlySettings.Enabled = value
	end
})

sections.miscMovementSection:AddSlider({
    text = "Fly Speed",
    flag = "flySpeed",
    suffix = " studs",
    value = 0.09,
    min = 0.05,
    max = 0.65,
    increment = 0.01,
    callback = function(v)
        FlySettings.Speed = v
    end
})

local states = {
    {state = "Climbing", flag = "Climbing"},
    {state = "Vaulting", flag = "Vaulting"},
    {state = "SprintSwimming", flag = "Swimming"}
}

local function updateState(char)
    char.Climbing = false
    char.Vaulting = false
    char.Swimming = false
    
    FlySettings.StateIndex = (FlySettings.StateIndex % #states) + 1
    local currentState = states[FlySettings.StateIndex]
    
    char.MoveState = currentState.state
    char[currentState.flag] = true
    
    char.RubberBandReset = 0
    char.LastMoveTime = workspace:GetServerTimeNow()
    char.LastGroundTime = workspace:GetServerTimeNow()
    char.LastGroundPos = char.RootPart.Position
end

RunService.Heartbeat:Connect(function()
    if not FlySettings.Enabled then return end
    
    local char = PlayerClass.Character
    if not char or not char.RootPart then return end
    
    if tick() - FlySettings.LastTick > 0.1 then
        updateState(char)
        FlySettings.LastTick = tick()
    end
    
    local direction = Vector3.zero
    local speed = FlySettings.Speed
    
    if UserInputService:IsKeyDown(Enum.KeyCode.W) then
        direction += Workspace.CurrentCamera.CFrame.LookVector * speed
    end
    if UserInputService:IsKeyDown(Enum.KeyCode.S) then
        direction -= Workspace.CurrentCamera.CFrame.LookVector * speed
    end
    if UserInputService:IsKeyDown(Enum.KeyCode.A) then
        direction -= Workspace.CurrentCamera.CFrame.RightVector * speed
    end
    if UserInputService:IsKeyDown(Enum.KeyCode.D) then
        direction += Workspace.CurrentCamera.CFrame.RightVector * speed
    end
    if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
        direction += Vector3.new(0, 1, 0) * speed
    end
    if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
        direction -= Vector3.new(0, 1, 0) * speed
    end
    
    if direction.Magnitude > 0 then
        for i = 1, 3 do
            local increment = direction * (1/3)
            

            local randomOffset = Vector3.new(
                math.random(-5, 5) / 100,
                math.random(-5, 5) / 100,
                math.random(-5, 5) / 100
            )
            
            char.RootPart.CFrame += increment + (randomOffset * 0.01)
            char.RootPart.AssemblyLinearVelocity = (increment.Unit + randomOffset) * 0.1
            
            task.wait(math.random(1, 2) / 100)
        end
    end
end)

local VehicleESPSettings = {
    Enabled = false, 
    MaxDistance = 5000 
}

local VehicleESPObjects = {}

local function CreateVehicleESP(vehicle)
    local nameTag = NewDrawing("Text", {
        Visible = false,
        Center = true,
        Outline = true,
        Size = 16,
        Color = Color3.new(0, 1, 0),
        Transparency = 1,
    })

    VehicleESPObjects[vehicle] = {
        Model = vehicle,
        NameTag = nameTag
    }
end

local function RemoveVehicleESP(vehicle)
    if VehicleESPObjects[vehicle] then
        VehicleESPObjects[vehicle].NameTag:Remove()
        VehicleESPObjects[vehicle] = nil
    end
end

local function UpdateVehicleESP()
    if not VehicleESPSettings.Enabled then
        for _, esp in pairs(VehicleESPObjects) do
            esp.NameTag.Visible = false
        end
        return
    end

    for vehicle, esp in pairs(VehicleESPObjects) do
        if vehicle and vehicle:IsA("Model") and vehicle.PrimaryPart then
            local primaryPart = vehicle.PrimaryPart
            local screenPosition, onScreen = Camera:WorldToViewportPoint(primaryPart.Position)

            if onScreen then
                local distance = (primaryPart.Position - Camera.CFrame.Position).Magnitude
                if distance <= VehicleESPSettings.MaxDistance then
                    esp.NameTag.Text = string.format("%s [%d]", vehicle.Name, math.floor(distance))
                    esp.NameTag.Position = Vector2.new(screenPosition.X, screenPosition.Y)
                    esp.NameTag.Visible = true
                else
                    esp.NameTag.Visible = false
                end
            else
                esp.NameTag.Visible = false
            end
        else
            esp.NameTag.Visible = false
        end
    end
end

for _, vehicle in ipairs(Vehicles:GetChildren()) do
    if vehicle:IsA("Model") then
        CreateVehicleESP(vehicle)
    end
end

Vehicles.ChildAdded:Connect(function(vehicle)
    if vehicle:IsA("Model") then
        CreateVehicleESP(vehicle)
    end
end)

Vehicles.ChildRemoved:Connect(function(vehicle)
    RemoveVehicleESP(vehicle)
end)

RunService.RenderStepped:Connect(UpdateVehicleESP)




sections.WorldVisuals:AddToggle({
    enabled = true,
    text = "Vehicle ESP Enabled",
    flag = "vehicleESPEnabled",
    callback = function(value)
        VehicleESPSettings.Enabled = value
    end,
})

sections.WorldVisuals:AddSlider({
    text = "Vehicle ESP Distance",
    flag = "vehicleESPDistance",
    suffix = " studs",
    value = VehicleESPSettings.MaxDistance,
    min = 1000,
    max = 10000,
    increment = 100,
    callback = function(value)
        VehicleESPSettings.MaxDistance = value
    end,
})

local CorpseESPSettings = {
    Enabled = false, 
    MaxDistance = 5000 
}

local CorpseESPObjects = {}



local function CreateCorpseESP(corpse)
    if corpse.Name == "Zombie" then return end 
    local nameTag = NewDrawing("Text", {
        Visible = false,
        Center = true,
        Outline = true,
        Size = 16,
        Color = Color3.new(1, 0, 0), 
        Transparency = 1,
    })

    CorpseESPObjects[corpse] = {
        Model = corpse,
        NameTag = nameTag
    }
end

local function RemoveCorpseESP(corpse)
    if CorpseESPObjects[corpse] then
        CorpseESPObjects[corpse].NameTag:Remove()
        CorpseESPObjects[corpse] = nil
    end
end

local function UpdateCorpseESP()
    if not CorpseESPSettings.Enabled then
        for _, esp in pairs(CorpseESPObjects) do
            esp.NameTag.Visible = false
        end
        return
    end

    for corpse, esp in pairs(CorpseESPObjects) do
        if corpse and corpse:IsA("Model") and corpse.PrimaryPart then
            local primaryPart = corpse.PrimaryPart
            local screenPosition, onScreen = Camera:WorldToViewportPoint(primaryPart.Position)

            if onScreen then
                local distance = (primaryPart.Position - Camera.CFrame.Position).Magnitude
                if distance <= CorpseESPSettings.MaxDistance then
                    esp.NameTag.Text = string.format("%s [%d studs]", corpse.Name, math.floor(distance))
                    esp.NameTag.Position = Vector2.new(screenPosition.X, screenPosition.Y)
                    esp.NameTag.Visible = true
                else
                    esp.NameTag.Visible = false
                end
            else
                esp.NameTag.Visible = false
            end
        else
            esp.NameTag.Visible = false
        end
    end
end

for _, corpse in ipairs(Corpses:GetChildren()) do
    if corpse:IsA("Model") and corpse.Name ~= "Zombie" then
        CreateCorpseESP(corpse)
    end
end

Corpses.ChildAdded:Connect(function(corpse)
    if corpse:IsA("Model") and corpse.Name ~= "Zombie" then
        CreateCorpseESP(corpse)
    end
end)

Corpses.ChildRemoved:Connect(function(corpse)
    RemoveCorpseESP(corpse)
end)

sections.WorldVisuals:AddSeparator({
    enabled = true,
    text = "Corpse ESP"
})

sections.WorldVisuals:AddToggle({
    enabled = true,
    text = "Corpse ESP Enabled",
    flag = "corpseESPEnabled",
    callback = function(value)
        CorpseESPSettings.Enabled = value
    end,
})

sections.WorldVisuals:AddSlider({
    text = "Corpse ESP Distance",
    flag = "corpseESPDistance",
    suffix = " studs",
    value = CorpseESPSettings.MaxDistance,
    min = 1000,
    max = 10000,
    increment = 100,
    callback = function(value)
        CorpseESPSettings.MaxDistance = value
    end,
})

local ZombieESPSettings = {
    Enabled = false,
    MaxDistance = 5000,
    RareOnly = false,
    RarityThreshold = 1
}

local ZombieESPObjects = {}

local function GetZombieRarity(zombieName)
    local config = workspace.Zombies.Configs:FindFirstChild(zombieName)
    if config and config:IsA("ModuleScript") then
        local success, zombieData = pcall(require, config)
        if success and zombieData and zombieData.SpawnRarity then
            return tonumber(zombieData.SpawnRarity) or 0
        end
    end
    return 0
end

local function CreateZombieESP(zombie)
    if not zombie or not zombie.Name then return end
    if not zombie:IsA("Model") then return end
    
    local rarity = GetZombieRarity(zombie.Name)
    
    local nameTag = Drawing.new("Text")
    nameTag.Visible = false
    nameTag.Center = true
    nameTag.Outline = true
    nameTag.Size = 16
    nameTag.Color = (rarity and rarity >= ZombieESPSettings.RarityThreshold) and Color3.new(1, 0, 0) or Color3.new(1, 0.5, 0)
    nameTag.Transparency = 1

    
    if not ZombieESPSettings.RareOnly or (rarity and rarity >= ZombieESPSettings.RarityThreshold) then
        if typeof(zombie) == "Instance" then
            ZombieESPObjects[zombie.Name] = {
                Model = zombie,
                NameTag = nameTag,
                Rarity = rarity
            }
        end
    else
        nameTag:Remove()
    end
end

local function RemoveZombieESP(zombie)
    if zombie and ZombieESPObjects[zombie.Name] then
        ZombieESPObjects[zombie.Name].NameTag:Remove()
        ZombieESPObjects[zombie.Name] = nil
    end
end

local function UpdateZombieESP()
    if not ZombieESPSettings.Enabled then
        for _, esp in pairs(ZombieESPObjects) do
            if esp and esp.NameTag then
                esp.NameTag.Visible = false
            end
        end
        return
    end

    for zombieName, esp in pairs(ZombieESPObjects) do
        if not esp or not esp.Model or not esp.Model.Parent then
            if esp and esp.NameTag then
                esp.NameTag:Remove()
            end
            ZombieESPObjects[zombieName] = nil
            continue
        end

        if esp.Model and esp.Model.PrimaryPart then
            local primaryPart = esp.Model.PrimaryPart
            local screenPosition, onScreen = Camera:WorldToViewportPoint(primaryPart.Position)

            if onScreen then
                local distance = (primaryPart.Position - Camera.CFrame.Position).Magnitude
                if distance <= ZombieESPSettings.MaxDistance then
                    esp.NameTag.Text = string.format("%s [%d studs] [%.2f]", 
                        esp.Model.Name, 
                        math.floor(distance),
                        esp.Rarity
                    )
                    esp.NameTag.Position = Vector2.new(screenPosition.X, screenPosition.Y)
                    esp.NameTag.Visible = true
                else
                    esp.NameTag.Visible = false
                end
            else
                esp.NameTag.Visible = false
            end
        else
            esp.NameTag.Visible = false
        end
    end
end

for _, zombie in ipairs(workspace.Zombies.Mobs:GetChildren()) do
    if zombie:IsA("Model") then
        CreateZombieESP(zombie)
    end
end

workspace.Zombies.Mobs.ChildAdded:Connect(function(zombie)
    if zombie:IsA("Model") then
        CreateZombieESP(zombie)
    end
end)

workspace.Zombies.Mobs.ChildRemoved:Connect(function(zombie)
    RemoveZombieESP(zombie)
end)

RunService.RenderStepped:Connect(UpdateZombieESP)


sections.WorldVisuals:AddSeparator({
    enabled = true,
    text = "Zombie ESP"
})

sections.WorldVisuals:AddToggle({
    enabled = true,
    text = "Zombie ESP Enabled",
    flag = "zombieESPEnabled",
    callback = function(value)
        ZombieESPSettings.Enabled = value
    end,
})

sections.WorldVisuals:AddToggle({
    enabled = true,
    text = "Rare Zombies Only",
    flag = "rareZombiesOnly",
    callback = function(value)
        ZombieESPSettings.RareOnly = value
        for _, zombie in ipairs(workspace.Zombies.Mobs:GetChildren()) do
            RemoveZombieESP(zombie)
            if zombie:IsA("Model") then
                CreateZombieESP(zombie)
            end
        end
    end,
})

sections.WorldVisuals:AddSlider({
    text = "Zombie ESP Distance",
    flag = "zombieESPDistance",
    suffix = " studs",
    value = ZombieESPSettings.MaxDistance,
    min = 1000,
    max = 10000,
    increment = 100,
    callback = function(value)
        ZombieESPSettings.MaxDistance = value
    end,
})

local GunModSettings = {
    NoRecoilEnabled = false,
    RecoilValue = 1.0,
    NoReloadEnabled = false,
    NoSpreadEnabled = false,
    InstantSearchEnabled = false, 
}
local function ModifyGunRecoil(enabled, value)
    if not Firearm then return end
    
    local function modifyRecoilData(gun, property, multiplier)
        if gun.RecoilData then
            setreadonly(gun.RecoilData, false)
            gun.RecoilData[property] = gun.RecoilData[property] * multiplier
            setreadonly(gun.RecoilData, true)
        end
    end

    for _, gunData in pairs(ItemData) do
        if gunData.Type == "Firearm" then
            setreadonly(gunData, false)
            
            if enabled then
                modifyRecoilData(gunData, "KickUpBounce", value)
                modifyRecoilData(gunData, "KickUpForce", value)
                modifyRecoilData(gunData, "KickUpSpeed", value)
                modifyRecoilData(gunData, "KickUpGunInfluence", value)
                modifyRecoilData(gunData, "KickUpCameraInfluence", value)
                modifyRecoilData(gunData, "RaiseInfluence", value)
                modifyRecoilData(gunData, "RaiseBounce", value)
                modifyRecoilData(gunData, "RaiseSpeed", value)
                modifyRecoilData(gunData, "RaiseForce", value)
                modifyRecoilData(gunData, "ShiftGunInfluence", value)
                modifyRecoilData(gunData, "ShiftBounce", value)
                modifyRecoilData(gunData, "ShiftCameraInfluence", value)
                modifyRecoilData(gunData, "ShiftForce", value)
            else
                modifyRecoilData(gunData, "KickUpBounce", 1)
                modifyRecoilData(gunData, "KickUpForce", 1)
                modifyRecoilData(gunData, "KickUpSpeed", 1)
                modifyRecoilData(gunData, "KickUpGunInfluence", 1)
                modifyRecoilData(gunData, "KickUpCameraInfluence", 1)
                modifyRecoilData(gunData, "RaiseInfluence", 1)
                modifyRecoilData(gunData, "RaiseBounce", 1)
                modifyRecoilData(gunData, "RaiseSpeed", 1)
                modifyRecoilData(gunData, "RaiseForce", 1)
                modifyRecoilData(gunData, "ShiftGunInfluence", 1)
                modifyRecoilData(gunData, "ShiftBounce", 1)
                modifyRecoilData(gunData, "ShiftCameraInfluence", 1)
                modifyRecoilData(gunData, "ShiftForce", 1)
            end
            
            setreadonly(gunData, true)
        end
    end
end

sections.Section5:AddToggle({
    enabled = true,
    text = "No Recoil",
    flag = "noRecoilEnabled",
    callback = function(Value)
        GunModSettings.NoRecoilEnabled = Value
        ModifyGunRecoil(Value, GunModSettings.RecoilValue)
    end,
})

sections.Section5:AddSlider({
    text = "Recoil Multiplier",
    flag = "recoilMultiplier",
    suffix = "x",
    value = 1.0,
    min = 0,
    max = 1,
    increment = 0.01,
    tooltip = "0 = No Recoil, 1 = Full Recoil",
    callback = function(v)
        GunModSettings.RecoilValue = v
        if GunModSettings.NoRecoilEnabled then
            ModifyGunRecoil(true, v)
        end
    end
})

local OldGetFireImpulse = GetFireImpulse
GetFireImpulse = function(...)
    if GunModSettings.NoRecoilEnabled then
        local ReturnArgs = {OldGetFireImpulse(...)}
        for Index = 1, #ReturnArgs do
            ReturnArgs[Index] = ReturnArgs[Index] * (1 - GunModSettings.RecoilValue)
        end
        
        return unpack(ReturnArgs)
    end
    
    return OldGetFireImpulse(...)
end

setupvalue(Firearm, 7, function(...)
    if GunModSettings.NoReloadEnabled then
        local Args = {...}
        for Index = 0, Args[3].LoopCount do
            Args[4]("Commit", "Load")
        end
        
        Args[4]("Commit", "End")
        return true
    end
    return AnimatedReload(...)
end)

sections.Section5:AddToggle({
    enabled = true,
    text = "No Spread",
    flag = "noSpreadEnabled",
    callback = function(Value)
        GunModSettings.NoSpreadEnabled = Value
    end,
})

sections.Section5:AddToggle({
    enabled = true,
    text = "No Reload",
    flag = "noReloadEnabled",
    callback = function(Value)
        GunModSettings.NoReloadEnabled = Value
    end,
})

local InteractHeartbeat, FindItemData
for Index, Table in pairs(getgc(true)) do
    if type(Table) == "table" and rawget(Table, "Rate") == 0.05 then
        InteractHeartbeat = Table.Action
        FindItemData = getupvalue(InteractHeartbeat, 11)
        if InteractHeartbeat and FindItemData then break end
    end
end

if InteractHeartbeat and FindItemData then
    setupvalue(InteractHeartbeat, 11, function(...)
        if GunModSettings.InstantSearchEnabled then 
            local ReturnArgs = {FindItemData(...)}
            if ReturnArgs[4] then ReturnArgs[4] = 0 end
            return unpack(ReturnArgs)
        end
        return FindItemData(...)
    end)
end

sections.Section5:AddToggle({
    enabled = true,
    text = "Instant Search",
    flag = "instantSearchEnabled",
    callback = function(Value)
        GunModSettings.InstantSearchEnabled = Value
    end
})

local ZombieCircleSettings = {
	Enabled = false,
	Radius = 10,     
	Speed = 500      
}

sections.ZombiesTab:AddToggle({
	enabled = true,
	text = "Zombie Circle",
	flag = "zombieCircleEnabled",
	callback = function(value)
		ZombieCircleSettings.Enabled = value
	end,
})

sections.ZombiesTab:AddSlider({
	text = "Zombie Circle Radius",
	flag = "zombieCircleRadius",
	suffix = " studs",
	value = ZombieCircleSettings.Radius,
	min = 7,
	max = 100,
	increment = 1,
	callback = function(v)
		ZombieCircleSettings.Radius = v
	end,
})

sections.ZombiesTab:AddSlider({
	text = "Zombie Circle Speed",
	flag = "zombieCircleSpeed",
	suffix = " rpm",
	value = ZombieCircleSettings.Speed,
	min = 1,
	max = 10000,
	increment = 1,
	callback = function(v)
		ZombieCircleSettings.Speed = v
	end,
})

local MeleeAuraSettings = {
	Enabled = false,
	Radius = 10,     
	Speed = 10      
}

sections.MeleeAura:AddToggle({
	enabled = true,
	text = "Fast Knife",
	flag = "FastKnifeEnabled",
	callback = function(value)
		MeleeAuraSettings.Enabled = value
	end,
})

sections.MeleeAura:AddSlider({
	text = "Fast Knife Speed",
	flag = "FastKnifeSpeed",
	suffix = " rpm",
	value = MeleeAuraSettings.Speed,
	min = 1,
	max = 10,
	increment = 1,
	callback = function(v)
		MeleeAuraSettings.Speed = v
	end,
})

sections.MeleeAura:AddSlider({
	text = "Fast Knife Radius",
	flag = "FastKnifeRadius",
	suffix = "",
	value = MeleeAuraSettings.Radius,
	min = 1,
	max = 10,
	increment = 1,
	callback = function(v)
		MeleeAuraSettings.Radius = v
	end,
})

local VehicleModsSettings = {
	VehicleKillaura = false,
    NoImpact = false,
    AlwaysDriveable = false,
    InfinteFuel = false,
    NoVehicleExplosion = false,
    NoVehicleTipOver = false,
    VehicleFly = false,
    VehicleSwim = false,
	KillauraRadius = 100,     
    VFlySpeed = 0.65,    
	CarSpeed = 5      
}

sections.VehicleSection:AddToggle({
	enabled = true,
	text = "Vehicle Kill Aura",
	flag = "VehKaEnabled",
    risky = true,
	callback = function(value)
		VehicleModsSettings.VehicleKillaura = value
	end,
})

sections.VehicleSection:AddToggle({
	enabled = true,
	text = "No Vehicle Impact",
	flag = "NoVehImpactEnabled",
    risky = true,
	callback = function(value)
		VehicleModsSettings.NoImpact = value
	end,
})

sections.VehicleSection:AddToggle({
	enabled = true,
	text = "Always Driveable",
	flag = "AlwaysDriveable",
    risky = true,
	callback = function(value)
		VehicleModsSettings.AlwaysDriveable = value
	end,
})

sections.VehicleSection:AddToggle({
	enabled = true,
	text = "Infinite Fuel",
	flag = "AlwaysHaveFuel",
    risky = true,
	callback = function(value)
		VehicleModsSettings.InfinteFuel = value
	end,
})

sections.VehicleSection:AddToggle({
	enabled = true,
	text = "No Vehicle Explosion",
	flag = "NoVehicleExplosionT",
    risky = true,
	callback = function(value)
		VehicleModsSettings.NoVehicleExplosion = value
	end,
})

sections.VehicleSection:AddToggle({
	enabled = true,
	text = "No Vehicle Tip Over",
	flag = "NoVehicleTipOverT",
    risky = true,
	callback = function(value)
		VehicleModsSettings.NoVehicleTipOver = value
	end,
})

sections.VehicleSection:AddToggle({
	enabled = true,
	text = "Vehicle Fly",
	flag = "VehicleFlyT",
    risky = true,
	callback = function(value)
		VehicleModsSettings.VehicleFly = value
	end,
})

sections.VehicleSection:AddToggle({
	enabled = true,
	text = "Vehicle Jesus",
	flag = "VehicleJesus",
    risky = true,
	callback = function(value)
		VehicleModsSettings.VehicleSwim = value
	end,
})

sections.VehicleSection:AddSlider({
	text = "Vehicle Kill Aura Radius",
	flag = "VehKaRadius",
	suffix = "",
	value = VehicleModsSettings.KillauraRadius,
	min = 1,
	max = 10,
	increment = 1,
	callback = function(v)
		VehicleModsSettings.KillauraRadius = v
	end,
})

sections.VehicleSection:AddSlider({
	text = "Vehicle Fly Speed",
	flag = "VehFlyRadius",
	suffix = "",
	value = VehicleModsSettings.VFlySpeed,
	min = 0.01,
	max = 1,
	increment = 1,
	callback = function(v)
		VehicleModsSettings.VFlySpeed = v
	end,
})

sections.VehicleSection:AddSlider({
	text = "Vehicle Car Speed",
	flag = "VehSpeedRadius",
	suffix = "",
	value = VehicleModsSettings.CarSpeed,
	min = 0.01,
	max = 5,
	increment = 1,
	callback = function(v)
		VehicleModsSettings.CarSpeed = v
	end,
})

local WallBangSettings = {
	WallbangToggle = false, 
    WallbangDepth = 5,    
}

sections.WallBangSection:AddToggle({
	enabled = true,
	text = "Wallbang",
	flag = "WallbangT",
    risky = true,
	callback = function(value)
		WallBangSettings.WallbangToggle = value
	end,
})

sections.WallBangSection:AddSlider({
	text = "Wallbang Depth",
	flag = "WallbangDepths",
	suffix = "",
	value = WallBangSettings.WallbangDepth,
	min = 0.01,
	max = 5,
	increment = 1,
	callback = function(v)
		WallBangSettings.WallbangDepth = v
	end,
})

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer

-- Constants for materials
local FORCEFIELD_MATERIAL = Enum.Material.ForceField
local DEFAULT_MATERIAL = Enum.Material.Plastic

-- Variables
local currentColor = Color3.fromRGB(255, 255, 255)
local forcefieldEnabled = false
local originalColors = {}

-- Customize Character Parts (including Head)
local function customizeCharacter(character, newColor)
	for _, part in ipairs(character:GetDescendants()) do
		if part:IsA("BasePart") or part:IsA("MeshPart") then
			-- Always include Head & weird Head parts
			if string.lower(part.Name):find("head") then
				-- Store original color if not already stored
				if not originalColors[part] then
					originalColors[part] = part.Color
				end
				if forcefieldEnabled then
					part.Color = newColor
					part.Material = FORCEFIELD_MATERIAL
				else
					part.Color = originalColors[part] or part.Color
					part.Material = DEFAULT_MATERIAL
				end
			else
				-- For other body parts
				if not originalColors[part] then
					originalColors[part] = part.Color
				end
				if forcefieldEnabled then
					part.Color = newColor
					part.Material = FORCEFIELD_MATERIAL
				else
					part.Color = originalColors[part] or part.Color
					part.Material = DEFAULT_MATERIAL
				end
			end
		end
	end
end

-- On Respawn
local function onCharacterAdded(character)
	originalColors = {} -- reset saved colors
	if forcefieldEnabled then
		customizeCharacter(character, currentColor)
	end
end

player.CharacterAdded:Connect(onCharacterAdded)

-- Toggle
local SelfChamsToggle = sections.localPlayer:AddToggle({
	enabled = true,
	text = "Enables Self Chams",
	flag = "SelfChamsEnable",
	callback = function(value)
		forcefieldEnabled = value
		print("[cb] Forcefield toggled:", value)

		local character = player.Character
		if character then
			customizeCharacter(character, currentColor)
		end
	end,
})

-- Color Picker
SelfChamsToggle:AddColor({
	enabled = true,
	text = "Self Chams Color",
	flag = "Color_1",
	tooltip = "Self Chams Color",
	color = currentColor,
	trans = 0,
	open = false,
	callback = function(value)
		currentColor = value
		local character = player.Character
		if forcefieldEnabled and character then
			customizeCharacter(character, currentColor)
		end
	end,
})

-- Heartbeat update
RunService.Heartbeat:Connect(function()
	if forcefieldEnabled then
		local character = player.Character
		if character then
			customizeCharacter(character, currentColor)
		end
	end
end)

-- Connection to handle new character spawns
player.CharacterAdded:Connect(onCharacterAdded)

RunService.RenderStepped:Connect(UpdateCorpseESP)






LocalPlayer.CharacterAdded:Connect(hookAnimations)




Players.PlayerAdded:Connect(AddESP)
Players.PlayerRemoving:Connect(RemoveChams)

library:SendNotification("firearm.xyz loaded", 5, Color3.new(255, 0, 0))

-- Window:SetOpen(true) -- Either Close Or Open Window
