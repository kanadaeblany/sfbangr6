local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

game.StarterGui:SetCore("ChatMakeSystemMessage", {
    Text = "";
    Color = Color3.fromRGB(255, 0, 0);
    Font = Enum.Font.SourceSansBold;
    TextSize = 24;
})

local gui = Instance.new("ScreenGui")
gui.Name = "Shadow Fiend BackShots"
gui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 350, 0, 250)
frame.Position = UDim2.new(0.5, -175, 0.5, -125)
frame.BackgroundColor3 = Color3.new(0.15, 0.15, 0.15)
frame.BorderColor3 = Color3.new(1, 0, 0)
frame.BorderSizePixel = 3
frame.Draggable = true
frame.Active = true

local title = Instance.new("TextLabel", frame)
title.Text = "Shadow Fiend BackShots"
title.Size = UDim2.new(1, 0, 0, 40)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
title.TextColor3 = Color3.new(1, 0, 0)
title.Font = Enum.Font.SourceSansBold
title.TextScaled = true

local textBox = Instance.new("TextBox", frame)
textBox.PlaceholderText = "Enter username/display name"
textBox.Size = UDim2.new(1, -20, 0, 30)
textBox.Position = UDim2.new(0, 10, 0, 50)
textBox.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
textBox.TextColor3 = Color3.new(1, 1, 1)
textBox.Font = Enum.Font.SourceSans
textBox.TextScaled = true
textBox.ClearTextOnFocus = false

local toggleButton = Instance.new("TextButton", frame)
toggleButton.Text = "Toggle On"
toggleButton.Size = UDim2.new(1, -20, 0, 30)
toggleButton.Position = UDim2.new(0, 10, 0, 100)
toggleButton.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
toggleButton.TextColor3 = Color3.new(1, 1, 1)
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.TextScaled = true

local statusLabel = Instance.new("TextLabel", frame)
statusLabel.Text = "Status: Off"
statusLabel.Size = UDim2.new(1, -20, 0, 30)
statusLabel.Position = UDim2.new(0, 10, 0, 150)
statusLabel.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
statusLabel.TextColor3 = Color3.new(1, 1, 1)
statusLabel.Font = Enum.Font.SourceSans
statusLabel.TextScaled = true

local messageLabel = Instance.new("TextLabel", frame)
messageLabel.Text = "Made by z5_2r"
messageLabel.Size = UDim2.new(1, -20, 0, 30)
messageLabel.Position = UDim2.new(0, 10, 0, 200)
messageLabel.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
messageLabel.TextColor3 = Color3.new(1, 0, 0)
messageLabel.Font = Enum.Font.SourceSansBold
messageLabel.TextScaled = true

local isFollowing = false
local targetPlayer = nil
local followConnection = nil
local animationTrack = nil
local animationId = "rbxassetid://148840371"

local function findPlayerByName(name)
    for _, player in ipairs(Players:GetPlayers()) do
        if string.match(player.Name:lower(), name:lower()) or string.match(player.DisplayName:lower(), name:lower()) then
            return player
        end
    end
    return nil
end

local function followPlayer()
    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local targetRoot = targetPlayer.Character.HumanoidRootPart
        local character = Players.LocalPlayer.Character or Players.LocalPlayer.CharacterAdded:Wait()
        local rootPart = character:WaitForChild("HumanoidRootPart")
        
        local humanoid = character:WaitForChild("Humanoid")
        local animation = Instance.new("Animation")
        animation.AnimationId = animationId
        animationTrack = humanoid:LoadAnimation(animation)
        animationTrack:Play()
        animationTrack:AdjustSpeed(100)
        
        followConnection = RunService.Stepped:Connect(function()
            rootPart.CFrame = targetRoot.CFrame * CFrame.new(0, 0, 2)
        end)
    end
end

local function stopFollowing()
    if followConnection then
        followConnection:Disconnect()
        followConnection = nil
    end
    if animationTrack then
        animationTrack:Stop()
        animationTrack = nil
    end
end

toggleButton.MouseButton1Click:Connect(function()
    isFollowing = not isFollowing
    
    if isFollowing then
        targetPlayer = findPlayerByName(textBox.Text)
        if targetPlayer then
            followPlayer()
            statusLabel.Text = "Status: On"
            toggleButton.Text = "Toggle Off"
        else
            statusLabel.Text = "Status: Player not found"
            isFollowing = false
        end
    else
        stopFollowing()
        statusLabel.Text = "Status: Off"
        toggleButton.Text = "Toggle On"
    end
end)
