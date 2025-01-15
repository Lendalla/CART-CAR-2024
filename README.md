-- ScreenGui for toggle teleport
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer
local teleportEnabled = false
local mode = "Teleport"

local TeleportScreenGui = player:WaitForChild("PlayerGui"):FindFirstChild("TeleportScreenGui")
if not TeleportScreenGui then
    TeleportScreenGui = Instance.new("ScreenGui")
    TeleportScreenGui.Name = "TeleportScreenGui"
    TeleportScreenGui.ResetOnSpawn = false
    TeleportScreenGui.Parent = player:WaitForChild("PlayerGui")
end

local function createButton(name, text, position, parent)
    local button = Instance.new("TextButton")
    button.Name = name
    button.Parent = parent
    button.Size = UDim2.new(0, 120, 0, 70)
    button.Position = position
    button.Text = text
    button.BackgroundColor3 = Color3.new(1, 0, 0)
    button.BorderSizePixel = 0
    button.AutoButtonColor = false

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = button

    button.MouseButton1Click:Connect(function()
        local sizeGoal = {Size = UDim2.new(0, 130, 0, 80)}
        local sizeTweenInfo = TweenInfo.new(0.1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
        local sizeTween = TweenService:Create(button, sizeTweenInfo, sizeGoal)
        sizeTween:Play()
        sizeTween.Completed:Connect(function()
            local sizeResetGoal = {Size = UDim2.new(0, 120, 0, 70)}
            local sizeResetTween = TweenService:Create(button, sizeTweenInfo, sizeResetGoal)
            sizeResetTween:Play()
        end)
    end)

    return button
end

local ToggleButton = TeleportScreenGui:FindFirstChild("ToggleButton")
if not ToggleButton then
    ToggleButton = createButton("ToggleButton", "เฆี่ยน Bacon", UDim2.new(0, 10, 0, 10), TeleportScreenGui)
end

local TeleportButton = TeleportScreenGui:FindFirstChild("TeleportButton")
if not TeleportButton then
    TeleportButton = createButton("TeleportButton", "Teleport Mode", UDim2.new(0, 140, 0, 10), TeleportScreenGui)
end

local TweenServiceButton = TeleportScreenGui:FindFirstChild("TweenServiceButton")
if not TweenServiceButton then
    TweenServiceButton = createButton("TweenServiceButton", "Tween Mode", UDim2.new(0, 270, 0, 10), TeleportScreenGui)
end

local function updateModeButtons()
    if mode == "Teleport" then
        TeleportButton.BackgroundColor3 = Color3.new(0, 1, 0)
        TweenServiceButton.BackgroundColor3 = Color3.new(1, 0, 0)
    elseif mode == "TweenService" then
        TeleportButton.BackgroundColor3 = Color3.new(1, 0, 0)
        TweenServiceButton.BackgroundColor3 = Color3.new(0, 1, 0)
    end
end

ToggleButton.MouseButton1Click:Connect(function()
    teleportEnabled = not teleportEnabled
    local goal = {}
    if teleportEnabled then
        goal.BackgroundColor3 = Color3.new(0, 1, 0)
    else
        goal.BackgroundColor3 = Color3.new(1, 0, 0)
    end
    local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    local tween = TweenService:Create(ToggleButton, tweenInfo, goal)
    tween:Play()
end)

TeleportButton.MouseButton1Click:Connect(function()
    mode = "Teleport"
    updateModeButtons()
end)

TweenServiceButton.MouseButton1Click:Connect(function()
    mode = "TweenService"
    updateModeButtons()
end)

updateModeButtons()

function teleportToNearestPlayer()
    local nearestPlayer = nil
    local shortestDistance = math.huge

    for _, otherPlayer in pairs(Players:GetPlayers()) do
        if otherPlayer ~= player and otherPlayer.Character and otherPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local distance = (player.Character.HumanoidRootPart.Position - otherPlayer.Character.HumanoidRootPart.Position).magnitude
            if distance < shortestDistance then
                shortestDistance = distance
                nearestPlayer = otherPlayer
            end
        end
    end

    if nearestPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        player.Character:SetPrimaryPartCFrame(nearestPlayer.Character.HumanoidRootPart.CFrame)
    end
end

function tweenToNearestPlayer()
    local nearestPlayer = nil
    local shortestDistance = math.huge

    for _, otherPlayer in pairs(Players:GetPlayers()) do
        if otherPlayer ~= player and otherPlayer.Character and otherPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local distance = (player.Character.HumanoidRootPart.Position - otherPlayer.Character.HumanoidRootPart.Position).magnitude
            if distance < shortestDistance then
                shortestDistance = distance
                nearestPlayer = otherPlayer
            end
        end
    end

    if nearestPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
        local goal = {CFrame = nearestPlayer.Character.HumanoidRootPart.CFrame}
        local tween = TweenService:Create(player.Character.HumanoidRootPart, tweenInfo, goal)
        tween:Play()
    end
end

spawn(function()
    while true do
        if teleportEnabled then
            if mode == "Teleport" then
                teleportToNearestPlayer()
            elseif mode == "TweenService" then
                tweenToNearestPlayer()
            end
        end
        wait(0.5)
    end
end)

-- ScreenGui for auto 
local GUI1 = Instance.new("ScreenGui")
local U5 = Instance.new("Frame")
local Frame = Instance.new("Frame")
local RJ = Instance.new("TextLabel")
local RejoinServer = Instance.new("TextButton")
local AutoCollectAllItem = Instance.new("TextButton")
local UIScale = Instance.new("UIScale")
local Btn = Instance.new("ImageButton")
local UIScale_2 = Instance.new("UIScale")

--Properties:

GUI1.Name = "GUI-1"
GUI1.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
GUI1.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

U5.Name = "U5"
U5.Parent = GUI1
U5.BackgroundColor3 = Color3.fromRGB(68, 68, 68)
U5.BorderColor3 = Color3.fromRGB(0, 0, 0)
U5.BorderSizePixel = 0
U5.Position = UDim2.new(0.347766697, 0, 0.10126403, 0)
U5.Size = UDim2.new(0, 222, 0, 276)
U5.Style = Enum.FrameStyle.DropShadow

Frame.Parent = U5
Frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Frame.BorderColor3 = Color3.fromRGB(0, 0, 0)
Frame.BorderSizePixel = 0
Frame.Position = UDim2.new(0.113348886, 0, 0.0551189482, 0)
Frame.Size = UDim2.new(0, 176, 0, 35)
Frame.Style = Enum.FrameStyle.DropShadow

RJ.Name = "RJ"
RJ.Parent = Frame
RJ.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
RJ.BackgroundTransparency = 1.000
RJ.BorderColor3 = Color3.fromRGB(0, 0, 0)
RJ.BorderSizePixel = 0
RJ.Position = UDim2.new(-0.18078509, 0, -0.848163307, 0)
RJ.Size = UDim2.new(0, 200, 0, 50)
RJ.SizeConstraint = Enum.SizeConstraint.RelativeYY
RJ.Font = Enum.Font.Jura
RJ.Text = "เมืองทอง HUB"
RJ.TextColor3 = Color3.fromRGB(214, 214, 214)
RJ.TextSize = 23.000

RejoinServer.Name = "Rejoin Server"
RejoinServer.Parent = Frame
RejoinServer.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
RejoinServer.BackgroundTransparency = 0.500
RejoinServer.BorderColor3 = Color3.fromRGB(0, 0, 0)
RejoinServer.BorderSizePixel = 0
RejoinServer.Position = UDim2.new(-0.120210409, 0, 2.34461594, 0)
RejoinServer.Size = UDim2.new(0, 182, 0, 38)
RejoinServer.Modal = true
RejoinServer.Font = Enum.Font.Gotham
RejoinServer.Text = "Rejoin Server"
RejoinServer.TextColor3 = Color3.fromRGB(255, 255, 255)
RejoinServer.TextSize = 14.000

AutoCollectAllItem.Name = "Auto Collect All Item"
AutoCollectAllItem.Parent = Frame
AutoCollectAllItem.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
AutoCollectAllItem.BackgroundTransparency = 0.500
AutoCollectAllItem.BorderColor3 = Color3.fromRGB(0, 0, 0)
AutoCollectAllItem.BorderSizePixel = 0
AutoCollectAllItem.Position = UDim2.new(-0.120646104, 0, 5.00785637, 0)
AutoCollectAllItem.Size = UDim2.new(0, 182, 0, 38)
AutoCollectAllItem.Modal = true
AutoCollectAllItem.Font = Enum.Font.Gotham
AutoCollectAllItem.Text = "Auto Collect All Item"
AutoCollectAllItem.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoCollectAllItem.TextSize = 14.000

UIScale.Parent = U5
UIScale.Scale = 1.100

Btn.Name = "Btn"
Btn.Parent = GUI1
Btn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Btn.BorderColor3 = Color3.fromRGB(0, 0, 0)
Btn.BorderSizePixel = 0
Btn.Position = UDim2.new(0.184487954, 0, 0.186022252, 0)
Btn.Size = UDim2.new(0, 79, 0, 72)
Btn.Image = "rbxasset://textures/ui/GuiImagePlaceholder.png"

UIScale_2.Parent = Btn
UIScale_2.Scale = 0.760

-- Scripts:

local function PRJR_fake_script() -- RejoinServer.RJ 
    local script = Instance.new('LocalScript', RejoinServer)

    local Players = game:GetService("Players")
    local TeleportService = game:GetService("TeleportService")
    
    local button = script.Parent
    
    button.MouseButton1Click:Connect(function()
        local player = Players.LocalPlayer
        TeleportService:Teleport(game.PlaceId, player)
    end)
    
    
end
coroutine.wrap(PRJR_fake_script)()
local function VRZG_fake_script() -- AutoCollectAllItem.AutoCollectAllItemScript 
    local script = Instance.new('LocalScript', AutoCollectAllItem)

    local TweenService = game:GetService("TweenService")
    local Players = game:GetService("Players")
    local LocalPlayer = Players.LocalPlayer
    local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
    
    local button = script.Parent
    
    local function teleportToPositions(positions)
        for _, position in positions do
            humanoidRootPart.CFrame = CFrame.new(position)
            task.wait(0.5)
        end
    end
    
    button.MouseButton1Click:Connect(function()
        game.StarterGui:SetCore("SendNotification", {
            Title = "Notification",
            Text = "Waiting 2 seconds",
            Duration = 3
        })
        task.wait(3)
    
        local teleportPositions = {
            Vector3.new(274, 348, 473),
            Vector3.new(268, 348, 473),
            Vector3.new(166, 780, 122),
            Vector3.new(165, 760, -1015),
            Vector3.new(131, 760, -975),
            Vector3.new(131, 760, -984),
            Vector3.new(131, 760, -991),
            Vector3.new(131, 760, -1000),
            Vector3.new(131, 760, -1008),
            Vector3.new(131, 760, -1015),
            Vector3.new(131, 760, -1024),
            Vector3.new(131, 760, -1031),
            Vector3.new(131, 760, -1040),
            Vector3.new(131, 760, -1047),
            Vector3.new(131, 760, -1055),
            Vector3.new(198, 760, -1055),
            Vector3.new(198, 760, -1048),
            Vector3.new(198, 760, -1040),
            Vector3.new(198, 760, -1031),
            Vector3.new(198, 760, -1023),
            Vector3.new(198, 760, -1015),
            Vector3.new(198, 760, -1008),
            Vector3.new(198, 760, -999),
            Vector3.new(198, 760, -991),
            Vector3.new(198, 760, -983),
            Vector3.new(198, 760, -976),
            Vector3.new(105, 3, 81),
            Vector3.new(84, 5, 34)
        }
    
        teleportToPositions(teleportPositions)
    end)
    
end
coroutine.wrap(VRZG_fake_script)()
local function KCJEV_fake_script() -- U5.Drag_UI 
    local script = Instance.new('LocalScript', U5)

    local userInputService = game:GetService("UserInputService")
    local gui = script.Parent -- Assuming the script is a child of the GUI element you want to drag
    
    local dragging
    local dragInput
    local dragStart
    local startPos
    
    local function update(input)
        local delta = input.Position - dragStart
        gui.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
    
    gui.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = gui.Position
    
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    
    gui.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType.Touch then
            dragInput = input
        end
    end)
    
    userInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            update(input)
        end
    end)
    
end
coroutine.wrap(KCJEV_fake_script)()
local function AMDOYO_fake_script() -- Btn.Open/Close 
    local script = Instance.new('LocalScript', Btn)

    local button = script.Parent
    local gui = button.Parent:FindFirstChild("U5")
    
    if button and gui then
        button.MouseButton1Click:Connect(function()
            gui.Visible = not gui.Visible
        end)
    else
        warn("Button or GUI not found")
    end
    
end
coroutine.wrap(AMDOYO_fake_script)()
local function NIGWB_fake_script() -- Btn.Drag_BTN 
    local script = Instance.new('LocalScript', Btn)

    local userInputService = game:GetService("UserInputService")
    local gui = script.Parent -- Assuming the script is a child of the GUI element you want to drag
    
    local dragging
    local dragInput
    local dragStart
    local startPos
    
    local function update(input)
        local delta = input.Position - dragStart
        gui.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
    
    gui.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = gui.Position
    
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    
    gui.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType.Touch then
            dragInput = input
        end
    end)
    
    userInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            update(input)
        end
    end)
    
end
coroutine.wrap(NIGWB_fake_script)()

-- Add animation to buttons in ScreenGui for auto
local function addButtonAnimation(button)
    button.MouseButton1Click:Connect(function()
        local originalSize = button.Size
        local shrinkGoal = {Size = UDim2.new(button.Size.X.Scale, button.Size.X.Offset - 10, button.Size.Y.Scale, button.Size.Y.Offset - 10)}
        local growGoal = {Size = originalSize}
        local tweenInfo = TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
        
        local shrinkTween = TweenService:Create(button, tweenInfo, shrinkGoal)
        shrinkTween:Play()
        shrinkTween.Completed:Connect(function()
            local growTween = TweenService:Create(button, tweenInfo, growGoal)
            growTween:Play()
        end)
    end)
end

addButtonAnimation(RejoinServer)
addButtonAnimation(AutoCollectAllItem)
addButtonAnimation(Btn)
