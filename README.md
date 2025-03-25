# Bao
Giúp bạn đẹp trai hơn
local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local root = character:WaitForChild("HumanoidRootPart")

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = player:WaitForChild("PlayerGui")

local BrownSquare = Instance.new("Frame")
BrownSquare.Size = UDim2.new(0, 50, 0, 50)
BrownSquare.Position = UDim2.new(0.5, -25, 0.5, -25)
BrownSquare.BackgroundColor3 = Color3.fromRGB(139, 69, 19)
BrownSquare.Parent = ScreenGui
BrownSquare.Draggable = true
BrownSquare.Active = true

local FunctionUI = Instance.new("Frame")
FunctionUI.Size = UDim2.new(0, 300, 0, 400)
FunctionUI.Position = UDim2.new(0.5, -150, 0.5, -200)
FunctionUI.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
FunctionUI.Visible = false
FunctionUI.Parent = ScreenGui

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 50)
Title.BackgroundColor3 = Color3.fromRGB(75, 75, 75)
Title.Text = "Blox Fruits Script"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 20
Title.Parent = FunctionUI

local function createButton(name, position, callback)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(0, 280, 0, 50)
    Button.Position = position
    Button.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    Button.Text = name
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.TextSize = 18
    Button.Parent = FunctionUI
    Button.MouseButton1Click:Connect(callback)
end

local farmLevelActive = false
local farmEnemiesActive = false
local farmMaterialsActive = false
local farmSeaEventActive = false

local function getNearestEnemy()
    local enemies = Workspace:WaitForChild("Enemies"):GetChildren()
    local nearest = nil
    local minDist = math.huge
    for _, enemy in pairs(enemies) do
        local enemyRoot = enemy:FindFirstChild("HumanoidRootPart")
        if enemyRoot and enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 then
            local dist = (root.Position - enemyRoot.Position).Magnitude
            if dist < minDist then
                minDist = dist
                nearest = enemy
            end
        end
    end
    return nearest
end

local function farmLevel()
    spawn(function()
        while farmLevelActive do
            local target = getNearestEnemy()
            if target then
                root.CFrame = target.HumanoidRootPart.CFrame * CFrame.new(0, 0, -5)
                local tool = character:FindFirstChildOfClass("Tool")
                if tool then tool:Activate() end
            end
            wait(0.1)
        end
    end)
end

local function farmNearbyEnemies()
    spawn(function()
        while farmEnemiesActive do
            local target = getNearestEnemy()
            if target then
                root.CFrame = target.HumanoidRootPart.CFrame * CFrame.new(0, 0, -5)
                local tool = character:FindFirstChildOfClass("Tool")
                if tool then tool:Activate() end
            end
            wait(0.1)
        end
    end)
end

local function farmMaterials()
    spawn(function()
        while farmMaterialsActive do
            for _, item in pairs(Workspace:GetChildren()) do
                if item.Name:find("Material") or item.Name:find("Chest") then
                    root.CFrame = item.CFrame
                    wait(0.5)
                end
            end
            wait(0.1)
        end
    end)
end

local function farmSeaEvent()
    spawn(function()
        while farmSeaEventActive do
            for _, event in pairs(Workspace:GetChildren()) do
                if event.Name:find("SeaEvent") or event.Name:find("Leviathan") then
                    root.CFrame = event.CFrame * CFrame.new(0, 0, -10)
                    local tool = character:FindFirstChildOfClass("Tool")
                    if tool then tool:Activate() end
                    wait(0.5)
                end
            end
            wait(0.1)
        end
    end)
end

createButton("Farm Level", UDim2.new(0, 10, 0, 60), function()
    farmLevelActive = not farmLevelActive
    if farmLevelActive then farmLevel() end
end)

createButton("Farm Nearby Enemies", UDim2.new(0, 10, 0, 120), function()
    farmEnemiesActive = not farmEnemiesActive
    if farmEnemiesActive then farmNearbyEnemies() end
end)

createButton("Farm Materials", UDim2.new(0, 10, 0, 180), function()
    farmMaterialsActive = not farmMaterialsActive
    if farmMaterialsActive then farmMaterials() end
end)

createButton("Farm Sea Event", UDim2.new(0, 10, 0, 240), function()
    farmSeaEventActive = not farmSeaEventActive
    if farmSeaEventActive then farmSeaEvent() end
end)

BrownSquare.MouseButton1Click:Connect(function()
    FunctionUI.Visible = not FunctionUI.Visible
end)
