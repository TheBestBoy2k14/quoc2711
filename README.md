local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")

local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local flying = false
local noclip = false
local esp = false

local speed = 60

local frame = script.Parent
frame.BackgroundColor3 = Color3.fromRGB(20,20,20)
frame.Size = UDim2.new(0, 200, 0, 180)
frame.Position = UDim2.new(0.05, 0, 0.3, 0)

local corner = Instance.new("UICorner", frame)

local function createButton(name, posY)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -20, 0, 40)
    btn.Position = UDim2.new(0, 10, 0, posY)
    btn.Text = name
    btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Parent = frame

    Instance.new("UICorner", btn)
    return btn
end

local flyBtn = createButton("Fly: OFF", 10)
local noclipBtn = createButton("Noclip: OFF", 60)
local espBtn = createButton("ESP: OFF", 110)

flyBtn.MouseButton1Click:Connect(function()
    flying = not flying
    flyBtn.Text = "Fly: " .. (flying and "ON" or "OFF")
end)

RunService.RenderStepped:Connect(function()
    if flying then
        hrp.Velocity = workspace.CurrentCamera.CFrame.LookVector * speed
    end
end)

noclipBtn.MouseButton1Click:Connect(function()
    noclip = not noclip
    noclipBtn.Text = "Noclip: " .. (noclip and "ON" or "OFF")
end)

RunService.Stepped:Connect(function()
    if noclip and char then
        for _, v in pairs(char:GetDescendants()) do
            if v:IsA("BasePart") then
                v.CanCollide = false
            end
        end
    end
end)

local highlights = {}

local function enableESP()
    for _, p in pairs(game.Players:GetPlayers()) do
        if p ~= player and p.Character then
            local h = Instance.new("Highlight")
            h.FillColor = Color3.fromRGB(255,0,0)
            h.Parent = p.Character
            highlights[p] = h
        end
    end
end

local function disableESP()
    for _, h in pairs(highlights) do
        if h then h:Destroy() end
    end
    highlights = {}
end

espBtn.MouseButton1Click:Connect(function()
    esp = not esp
    espBtn.Text = "ESP: " .. (esp and "ON" or "OFF")

    if esp then
        enableESP()
    else
        disableESP()
    end
end)

-- Ẩn/hiện menu bằng phím M
UIS.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.M then
        frame.Visible = not frame.Visible
    end
end)
