local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local Button = Instance.new("TextButton")

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(30,30,30)
Frame.Position = UDim2.new(0.1,0,0.3,0)
Frame.Size = UDim2.new(0,200,0,100)

Button.Parent = Frame
Button.Size = UDim2.new(1,0,1,0)
Button.Text = "Fly: OFF"
Button.BackgroundColor3 = Color3.fromRGB(50,50,50)
Button.TextColor3 = Color3.new(1,1,1)

local flying = false
local speed = 50
local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")

local bv, bg

function startFly()
    bv = Instance.new("BodyVelocity")
    bg = Instance.new("BodyGyro")

    bv.MaxForce = Vector3.new(1e5,1e5,1e5)
    bg.MaxTorque = Vector3.new(1e5,1e5,1e5)

    bv.Parent = hrp
    bg.Parent = hrp

    game:GetService("RunService").RenderStepped:Connect(function()
        if flying then
            local cam = workspace.CurrentCamera
            bv.Velocity = cam.CFrame.LookVector * speed
            bg.CFrame = cam.CFrame
        end
    end)
end

function stopFly()
    if bv then bv:Destroy() end
    if bg then bg:Destroy() end
end

Button.MouseButton1Click:Connect(function()
    flying = not flying

    if flying then
        Button.Text = "Fly: ON"
        startFly()
    else
        Button.Text = "Fly: OFF"
        stopFly()
    end
end)
