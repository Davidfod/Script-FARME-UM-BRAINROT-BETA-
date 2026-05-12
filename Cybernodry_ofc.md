local UIS = game:GetService("UserInputService")
local RepS = game:GetService("ReplicatedStorage")
local RunS = game:GetService("RunService")
local LP = game:GetService("Players").LocalPlayer
local Char = LP.Character or LP.CharacterAdded:Wait()

local ui = Instance.new("ScreenGui", LP.PlayerGui)
ui.Name = "CyberV3_Retail"
ui.ResetOnSpawn = false

local function drag(f)
    local d, di, ds, sp
    f.InputBegan:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 then
            d = true ds = i.Position sp = f.Position
        end
    end)
    UIS.InputChanged:Connect(function(i)
        if d and i.UserInputType == Enum.UserInputType.MouseMovement then
            local dt = i.Position - ds
            f.Position = UDim2.new(sp.X.Scale, sp.X.Offset + dt.X, sp.Y.Scale, sp.Y.Offset + dt.Y)
        end
    end)
    UIS.InputEnded:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 then d = false end
    end)
end

local main = Instance.new("Frame", ui)
main.Size = UDim2.new(0, 280, 0, 280)
main.Position = UDim2.new(0.5, -140, 0.4, -140)
main.BackgroundColor3 = Color3.fromRGB(18, 18, 22)
main.BorderSizePixel = 0
main.ClipsDescendants = true
drag(main)

Instance.new("UICorner", main).CornerRadius = UDim.new(0, 6)
local line = Instance.new("Frame", main)
line.Size = UDim2.new(1, 0, 0, 2)
line.Position = UDim2.new(0, 0, 0, 38)
line.BackgroundColor3 = Color3.fromRGB(160, 40, 255)
line.BorderSizePixel = 0

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1, -20, 0, 38)
title.Position = UDim2.new(0, 12, 0, 0)
title.Text = "CYBER HUB | V3"
title.TextColor3 = Color3.new(1,1,1)
title.Font = "GothamBold"
title.TextSize = 14
title.BackgroundTransparency = 1
title.TextXAlignment = "Left"

local close = Instance.new("TextButton", main)
close.Size = UDim2.new(0, 22, 0, 22)
close.Position = UDim2.new(1, -28, 0, 8)
close.Text = "x"
close.BackgroundColor3 = Color3.fromRGB(200, 40, 40)
close.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", close).CornerRadius = UDim.new(1,0)
close.MouseButton1Click:Connect(function() ui:Destroy() end)

local mini = Instance.new("TextButton", main)
mini.Size = UDim2.new(0, 12, 0, 12)
mini.Position = UDim2.new(1, -48, 0, 13)
mini.Text = ""
mini.BackgroundColor3 = Color3.fromRGB(255, 200, 50)
Instance.new("UICorner", mini).CornerRadius = UDim.new(1,0)

local function btn(txt, p, pos, c)
    local b = Instance.new("TextButton", p)
    b.Size = UDim2.new(0.9, 0, 0, 35)
    b.Position = pos
    b.Text = txt
    b.BackgroundColor3 = c or Color3.fromRGB(30, 30, 38)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = "GothamMedium"
    b.TextSize = 12
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 4)
    return b
end

local tpsBtn = btn("MENU TELEPORTES", main, UDim2.new(0.05, 0, 0, 50))
local cashBtn = btn("CASH PANEL", main, UDim2.new(0.05, 0, 0, 95))

local isMini = false
mini.MouseButton1Click:Connect(function()
    isMini = not isMini
    if isMini then
        tpsBtn.Visible = false
        cashBtn.Visible = false
        main:TweenSize(UDim2.new(0, 280, 0, 38), "Out", "Quart", 0.3, true)
    else
        main:TweenSize(UDim2.new(0, 280, 0, 280), "Out", "Quart", 0.3, true)
        task.wait(0.2)
        tpsBtn.Visible = true
        cashBtn.Visible = true
    end
end)

local tpF = Instance.new("Frame", ui)
tpF.Size = UDim2.new(0, 200, 0, 300)
tpF.Position = UDim2.new(0.5, 150, 0.4, -150)
tpF.BackgroundColor3 = Color3.fromRGB(18, 18, 22)
tpF.Visible = false
Instance.new("UICorner", tpF)
drag(tpF)

local cF = Instance.new("Frame", ui)
cF.Size = UDim2.new(0, 220, 0, 240)
cF.Position = UDim2.new(0.5, -110, 0.4, -120)
cF.BackgroundColor3 = Color3.fromRGB(15, 15, 18)
cF.Visible = false
Instance.new("UICorner", cF)
drag(cF)

local nIn = Instance.new("TextBox", cF)
nIn.Size = UDim2.new(0.8, 0, 0, 30)
nIn.Position = UDim2.new(0.1, 0, 0.15, 0)
nIn.Text = LP.Name
nIn.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
nIn.TextColor3 = Color3.new(1,1,1)

local vIn = Instance.new("TextBox", cF)
vIn.Size = UDim2.new(0.8, 0, 0, 30)
vIn.Position = UDim2.new(0.1, 0, 0.35, 0)
vIn.PlaceholderText = "Valor..."
vIn.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
vIn.TextColor3 = Color3.new(1,1,1)

local inject = btn("INJETAR AGORA", cF, UDim2.new(0.1, 0, 0.6, 0), Color3.fromRGB(0, 100, 50))
local autoBtn = btn("AUTO CASH: OFF", cF, UDim2.new(0.1, 0, 0.8, 0), Color3.fromRGB(60, 60, 70))

-- LOGICA SUPER FAST
local loopConn
autoBtn.MouseButton1Click:Connect(function()
    if loopConn then
        loopConn:Disconnect()
        loopConn = nil
        autoBtn.Text = "AUTO CASH: OFF"
        autoBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
    else
        autoBtn.Text = "AUTO CASH: ON"
        autoBtn.BackgroundColor3 = Color3.fromRGB(160, 40, 255)
        
        local r = RepS:FindFirstChild("DebugMoney", true) or RepS:FindFirstChild("DecrementMoney", true)
        
        loopConn = RunS.RenderStepped:Connect(function()
            local v = tonumber(vIn.Text)
            if r and v then
                pcall(function() 
                    r:FireServer(-v, nIn.Text) 
                    r:FireServer(-v) 
                end)
            end
        end)
    end
end)

inject.MouseButton1Click:Connect(function()
    local r = RepS:FindFirstChild("DebugMoney", true) or RepS:FindFirstChild("DecrementMoney", true)
    local v = tonumber(vIn.Text)
    if r and v then
        pcall(function() r:FireServer(-v, nIn.Text) r:FireServer(-v) end)
    end
end)

tpsBtn.MouseButton1Click:Connect(function() tpF.Visible = not tpF.Visible end)
cashBtn.MouseButton1Click:Connect(function() cF.Visible = not cF.Visible end)

for i = 1, 6 do
    local b = btn("Area "..i, tpF, UDim2.new(0.05, 0, 0, (i-1)*45 + 40))
    b.MouseButton1Click:Connect(function()
        local a = workspace:FindFirstChild("Areas")
        if a and a:FindFirstChild("Area"..i) then
            local p = a["Area"..i]:FindFirstChild("Boundary") or a["Area"..i]:FindFirstChild("EventZone")
            if p and Char:FindFirstChild("HumanoidRootPart") then
                local cf = p.CFrame
                if i == 2 then cf = cf * CFrame.new(0, 3, (p.Size.Z/2)+3) else cf = cf * CFrame.new(0, 3, 0) end
                Char.HumanoidRootPart.CFrame = cf
            end
        end
    end)
end
