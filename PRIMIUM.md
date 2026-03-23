--// 🗿 primium HUB FULL

local player = game.Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local char = player.Character or player.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")
local hum = char:WaitForChild("Humanoid")

local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "SangHubPro"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 220, 0, 320)
frame.Position = UDim2.new(0, 20, 0, 100)
frame.BackgroundColor3 = Color3.fromRGB(15,15,15)
frame.Active = true
frame.Draggable = true

local function makeBtn(text, y)
    local b = Instance.new("TextButton", frame)
    b.Size = UDim2.new(0, 200, 0, 35)
    b.Position = UDim2.new(0,10,0,y)
    b.Text = text
    b.BackgroundColor3 = Color3.fromRGB(40,40,40)
    b.TextColor3 = Color3.new(1,1,1)
    return b
end

local flyBtn = makeBtn("Fly: OFF",10)
local noclipBtn = makeBtn("Noclip: OFF",50)
local espBtn = makeBtn("ESP: OFF",90)
local speedBtn = makeBtn("Speed x2",130)
local jumpBtn = makeBtn("Infinite Jump: OFF",170)
local tpBtn = makeBtn("Click TP: OFF",210)

local flying, noclip, esp, infJump, clickTP = false,false,false,false,false

local bv = Instance.new("BodyVelocity")
bv.MaxForce = Vector3.new(1e5,1e5,1e5)

flyBtn.MouseButton1Click:Connect(function()
    flying = not flying
    flyBtn.Text = "Fly: "..(flying and "ON" or "OFF")
    bv.Parent = flying and hrp or nil
end)

noclipBtn.MouseButton1Click:Connect(function()
    noclip = not noclip
    noclipBtn.Text = "Noclip: "..(noclip and "ON" or "OFF")
end)

RunService.Stepped:Connect(function()
    if noclip then
        for _,v in pairs(char:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide = false end
        end
    end
end)

espBtn.MouseButton1Click:Connect(function()
    esp = not esp
    espBtn.Text = "ESP: "..(esp and "ON" or "OFF")
    
    for _,plr in pairs(game.Players:GetPlayers()) do
        if plr ~= player and plr.Character then
            if esp then
                local h = Instance.new("Highlight", plr.Character)
                h.FillColor = Color3.fromRGB(255,0,0)
            else
                for _,v in pairs(plr.Character:GetChildren()) do
                    if v:IsA("Highlight") then v:Destroy() end
                end
            end
        end
    end
end)

speedBtn.MouseButton1Click:Connect(function()
    hum.WalkSpeed = hum.WalkSpeed == 16 and 50 or 16
end)

jumpBtn.MouseButton1Click:Connect(function()
    infJump = not infJump
    jumpBtn.Text = "Infinite Jump: "..(infJump and "ON" or "OFF")
end)

UIS.JumpRequest:Connect(function()
    if infJump then
        hum:ChangeState("Jumping")
    end
end)

tpBtn.MouseButton1Click:Connect(function()
    clickTP = not clickTP
    tpBtn.Text = "Click TP: "..(clickTP and "ON" or "OFF")
end)

UIS.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 and clickTP then
        local mouse = player:GetMouse()
        if mouse.Hit then
            hrp.CFrame = mouse.Hit + Vector3.new(0,3,0)
        end
    end
end)

RunService.RenderStepped:Connect(function()
    if flying then
        local cam = workspace.CurrentCamera
        local move = Vector3.zero
        
        if UIS:IsKeyDown(Enum.KeyCode.W) then move += cam.CFrame.LookVector end
        if UIS:IsKeyDown(Enum.KeyCode.S) then move -= cam.CFrame.LookVector end
        if UIS:IsKeyDown(Enum.KeyCode.A) then move -= cam.CFrame.RightVector end
        if UIS:IsKeyDown(Enum.KeyCode.D) then move += cam.CFrame.RightVector end
        
        bv.Velocity = move * 70
    end
end)
