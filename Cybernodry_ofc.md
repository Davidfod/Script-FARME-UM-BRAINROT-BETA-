local UIS = game:GetService("UserInputService")
local RepS = game:GetService("ReplicatedStorage")
local LP = game:GetService("Players").LocalPlayer
local Char = LP.Character or LP.CharacterAdded:Wait()

local ui = Instance.new("ScreenGui")
ui.Name = "Cyber_v2"
ui.Parent = LP.PlayerGui
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

local main = Instance.new("Frame")
main.Size = UDim2.new(0, 280, 0, 240)
main.Position = UDim2.new(0.5, -140, 0.4, -120)
main.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
main.BorderSizePixel = 0
main.Parent = ui
drag(main)

Instance.new("UICorner", main).CornerRadius = UDim.new(0, 8)
local grad = Instance.new("UIGradient", main)
grad.Color = ColorSequence.new{ColorSequenceKeypoint.new(0, Color3.fromRGB(30, 30, 40)), ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 20, 25))}

local line = Instance.new("Frame", main)
line.Size = UDim2.new(1, 0, 0, 2)
line.Position = UDim2.new(0, 0, 0, 40)
line.BackgroundColor3 = Color3.fromRGB(180, 50, 255)
line.BorderSizePixel = 0

local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1, -80, 0, 40)
title.Position = UDim2.new(0, 15, 0, 0)
title.Text = "PAINEL CYBER"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextXAlignment = "Left"
title.Font = "GothamBold"
title.TextSize = 16
title.BackgroundTransparency = 1

local close = Instance.new("TextButton", main)
close.Size = UDim2.new(0, 24, 0, 24)
close.Position = UDim2.new(1, -30, 0, 8)
close.Text = "×"
close.TextColor3 = Color3.new(1, 1, 1)
close.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
close.Font = "GothamBold"
Instance.new("UICorner", close).CornerRadius = UDim.new(1, 0)
close.MouseButton1Click:Connect(function() ui:Destroy() end)

local min = Instance.new("TextButton", main)
min.Size = UDim2.new(0, 12, 0, 12)
min.Position = UDim2.new(1, -50, 0, 14)
min.Text = ""
min.BackgroundColor3 = Color3.fromRGB(255, 200, 50)
Instance.new("UICorner", min).CornerRadius = UDim.new(1, 0)

-- Menu de TPs
local tpF = Instance.new("Frame", ui)
tpF.Size = UDim2.new(0, 220, 0, 320)
tpF.Position = UDim2.new(0.5, 150, 0.4, -160)
tpF.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
tpF.Visible = false
Instance.new("UICorner", tpF)
drag(tpF)

local tpList = Instance.new("ScrollingFrame", tpF)
tpList.Size = UDim2.new(1, -20, 1, -50)
tpList.Position = UDim2.new(0, 10, 0, 45)
tpList.BackgroundTransparency = 1
tpList.ScrollBarThickness = 2

local function createBtn(txt, parent, pos, color)
	local b = Instance.new("TextButton", parent)
	b.Size = UDim2.new(0.9, 0, 0, 38)
	b.Position = pos
	b.Text = txt
	b.BackgroundColor3 = color or Color3.fromRGB(35, 35, 45)
	b.TextColor3 = Color3.new(1, 1, 1)
	b.Font = "Gotham"
	b.TextSize = 13
	Instance.new("UICorner", b).CornerRadius = UDim.new(0, 5)
	return b
end

local btnTp = createBtn("TELEPORTES", main, UDim2.new(0.05, 0, 0, 60), Color3.fromRGB(45, 40, 60))
local btnCash = createBtn("CASH INJECTOR", main, UDim2.new(0.05, 0, 0, 110), Color3.fromRGB(45, 40, 60))

-- Sistema Cash
local cF = Instance.new("Frame", ui)
cF.Size = UDim2.new(0, 220, 0, 200)
cF.Position = UDim2.new(0.5, -110, 0.4, -100)
cF.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
cF.Visible = false
Instance.new("UICorner", cF)
drag(cF)

local nIn = Instance.new("TextBox", cF)
nIn.Size = UDim2.new(0.8, 0, 0, 30)
nIn.Position = UDim2.new(0.1, 0, 0.2, 0)
nIn.Text = LP.Name
nIn.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
nIn.TextColor3 = Color3.new(1,1,1)

local vIn = Instance.new("TextBox", cF)
vIn.Size = UDim2.new(0.8, 0, 0, 30)
vIn.Position = UDim2.new(0.1, 0, 0.45, 0)
vIn.PlaceholderText = "Valor..."
vIn.Text = ""
vIn.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
vIn.TextColor3 = Color3.new(1,1,1)

local go = createBtn("INJETAR", cF, UDim2.new(0.1, 0, 0.7, 0), Color3.fromRGB(0, 120, 70))

btnTp.MouseButton1Click:Connect(function() tpF.Visible = not tpF.Visible end)
btnCash.MouseButton1Click:Connect(function() cF.Visible = not cF.Visible end)

local isMin = false
min.MouseButton1Click:Connect(function()
	isMin = not isMin
	main:TweenSize(isMin and UDim2.new(0, 200, 0, 40) or UDim2.new(0, 280, 0, 240), "Out", "Quart", 0.3, true)
	btnTp.Visible = not isMin
	btnCash.Visible = not isMin
end)

-- TPs
local function getRemote()
	local r = RepS:FindFirstChild("DebugMoney", true)
	if r and r:IsA("RemoteEvent") then return r end
	return RepS:FindFirstChild("DecrementMoney", true)
end

go.MouseButton1Click:Connect(function()
	local r = getRemote()
	local v = tonumber(vIn.Text)
	if r and v then
		pcall(function() r:FireServer(-v, nIn.Text) r:FireServer(-v) end)
		go.Text = "OK!" task.wait(1) go.Text = "INJETAR"
	end
end)

for i = 1, 6 do
	local b = createBtn("Area "..i, tpList, UDim2.new(0.05, 0, 0, (i-1)*45))
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
