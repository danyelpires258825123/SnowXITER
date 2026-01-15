--// SnowXIT UI | LocalScript
--// Aimbot REAL (HEAD LOCK) + ESP PRO (TIME FIX) + HealthBar + Tracer ENEMY ONLY
--// Tudo UNIFICADO

repeat task.wait() until game:IsLoaded()

--------------------------------------------------
-- SERVICES
--------------------------------------------------
local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

local Player = Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

--------------------------------------------------
-- CONFIG
--------------------------------------------------
local BUTTON_IMAGE = "rbxassetid://113108067983330"
local UI_NAME = "Painel SnowXIT | by SnowXIT Team"

--------------------------------------------------
-- STATE
--------------------------------------------------
local State = {
	Aimbot = false,
	WallCheck = true,
	ShowFOV = true,
	FOVSize = 200,
	ESP = false,
	Tracer = false
}

--------------------------------------------------
-- GUI BASE
--------------------------------------------------
local ScreenGui = Instance.new("ScreenGui", PlayerGui)
ScreenGui.Name = "SnowXIT_GUI"
ScreenGui.ResetOnSpawn = false

--------------------------------------------------
-- FLOAT BUTTON
--------------------------------------------------
local ToggleButton = Instance.new("ImageButton", ScreenGui)
ToggleButton.Size = UDim2.fromOffset(60,60)
ToggleButton.Position = UDim2.fromScale(0.08,0.4)
ToggleButton.Image = BUTTON_IMAGE
ToggleButton.BackgroundColor3 = Color3.fromRGB(25,25,25)
ToggleButton.Active = true
ToggleButton.Draggable = true
ToggleButton.AutoButtonColor = false
ToggleButton.ClipsDescendants = true
Instance.new("UICorner", ToggleButton).CornerRadius = UDim.new(1,0)

--------------------------------------------------
-- PANEL
--------------------------------------------------
local Panel = Instance.new("Frame", ScreenGui)
Panel.Size = UDim2.fromOffset(330,440)
Panel.Position = UDim2.fromScale(0.35,0.25)
Panel.BackgroundColor3 = Color3.fromRGB(18,18,18)
Panel.Visible = false
Panel.Active = true
Panel.Draggable = true
Instance.new("UICorner", Panel).CornerRadius = UDim.new(0,12)

--------------------------------------------------
-- TITLE
--------------------------------------------------
local Title = Instance.new("TextLabel", Panel)
Title.Size = UDim2.new(1,0,0,40)
Title.BackgroundColor3 = Color3.fromRGB(180,0,0)
Title.Text = UI_NAME
Title.TextColor3 = Color3.new(1,1,1)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 14

--------------------------------------------------
-- TABS
--------------------------------------------------
local Tabs = Instance.new("Frame", Panel)
Tabs.Position = UDim2.fromOffset(0,40)
Tabs.Size = UDim2.new(1,0,0,35)
Tabs.BackgroundTransparency = 1

local function CreateTab(name,x)
	local b = Instance.new("TextButton", Tabs)
	b.Size = UDim2.fromOffset(100,30)
	b.Position = UDim2.fromOffset(x,2)
	b.Text = name
	b.Font = Enum.Font.GothamBold
	b.TextSize = 13
	b.BackgroundColor3 = Color3.fromRGB(30,30,30)
	b.TextColor3 = Color3.new(1,1,1)
	Instance.new("UICorner", b)
	return b
end

local TabAimbot = CreateTab("Aimbot",10)
local TabVisual = CreateTab("Visual",120)
local TabPlayer = CreateTab("Player",230)

--------------------------------------------------
-- PAGES
--------------------------------------------------
local AimbotPage = Instance.new("Frame", Panel)
AimbotPage.Position = UDim2.fromOffset(0,80)
AimbotPage.Size = UDim2.new(1,0,1,-80)
AimbotPage.BackgroundTransparency = 1

local VisualPage = AimbotPage:Clone()
VisualPage.Parent = Panel
VisualPage.Visible = false

local PlayerPage = AimbotPage:Clone()
PlayerPage.Parent = Panel
PlayerPage.Visible = false

--------------------------------------------------
-- TOGGLE
--------------------------------------------------
local function CreateToggle(parent,text,y,callback)
	local btn = Instance.new("TextButton", parent)
	btn.Size = UDim2.new(0.9,0,0,32)
	btn.Position = UDim2.fromOffset(15,y)
	btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
	btn.Font = Enum.Font.Gotham
	btn.TextSize = 13
	btn.TextColor3 = Color3.new(1,1,1)
	Instance.new("UICorner", btn)

	local state = false
	btn.Text = text.." : OFF"

	btn.MouseButton1Click:Connect(function()
		state = not state
		btn.Text = text.." : "..(state and "ON" or "OFF")
		if callback then callback(state) end
	end)
end

--------------------------------------------------
-- SLIDER
--------------------------------------------------
local function CreateSlider(parent,text,y,min,max,default,callback)
	local holder = Instance.new("Frame", parent)
	holder.Size = UDim2.new(0.9,0,0,45)
	holder.Position = UDim2.fromOffset(15,y)
	holder.BackgroundTransparency = 1

	local label = Instance.new("TextLabel", holder)
	label.Size = UDim2.new(1,0,0,20)
	label.BackgroundTransparency = 1
	label.Text = text.." : "..default
	label.TextColor3 = Color3.new(1,1,1)
	label.Font = Enum.Font.Gotham
	label.TextSize = 12

	local bar = Instance.new("Frame", holder)
	bar.Position = UDim2.fromOffset(0,25)
	bar.Size = UDim2.new(1,0,0,6)
	bar.BackgroundColor3 = Color3.fromRGB(60,60,60)
	Instance.new("UICorner", bar)

	local fill = Instance.new("Frame", bar)
	fill.Size = UDim2.fromScale((default-min)/(max-min),1)
	fill.BackgroundColor3 = Color3.fromRGB(200,0,0)
	Instance.new("UICorner", fill)

	local dragging = false

	bar.InputBegan:Connect(function(i)
		if i.UserInputType == Enum.UserInputType.MouseButton1
		or i.UserInputType == Enum.UserInputType.Touch then
			dragging = true
		end
	end)

	UIS.InputEnded:Connect(function(i)
		if i.UserInputType == Enum.UserInputType.MouseButton1
		or i.UserInputType == Enum.UserInputType.Touch then
			dragging = false
		end
	end)

	RunService.RenderStepped:Connect(function()
		if dragging then
			local x = math.clamp((UIS:GetMouseLocation().X - bar.AbsolutePosition.X)/bar.AbsoluteSize.X,0,1)
			fill.Size = UDim2.fromScale(x,1)
			local value = math.floor(min + (max-min)*x)
			label.Text = text.." : "..value
			if callback then callback(value) end
		end
	end)
end

--------------------------------------------------
-- FOV
--------------------------------------------------
local FOV = Drawing.new("Circle")
FOV.Color = Color3.fromRGB(255,0,0)
FOV.Thickness = 2
FOV.Filled = false
FOV.NumSides = 64

RunService.RenderStepped:Connect(function()
	FOV.Position = Camera.ViewportSize/2
	FOV.Radius = State.FOVSize
	FOV.Visible = State.ShowFOV
end)

--------------------------------------------------
-- AIMBOT
--------------------------------------------------
local function GetClosestEnemyHead()
	local closestHead
	local shortest = State.FOVSize

	for _,plr in pairs(Players:GetPlayers()) do
		if plr ~= Player and plr.Team ~= Player.Team then
			local char = plr.Character
			local hum = char and char:FindFirstChildOfClass("Humanoid")
			local head = char and char:FindFirstChild("Head")

			if hum and hum.Health > 0 and head then
				local pos,vis = Camera:WorldToViewportPoint(head.Position)
				if vis then
					local dist = (Vector2.new(pos.X,pos.Y) - Camera.ViewportSize/2).Magnitude
					if dist < shortest then
						if State.WallCheck then
							local params = RaycastParams.new()
							params.FilterType = Enum.RaycastFilterType.Blacklist
							params.FilterDescendantsInstances = {Player.Character}

							local ray = workspace:Raycast(Camera.CFrame.Position, head.Position - Camera.CFrame.Position, params)
							if ray and ray.Instance:IsDescendantOf(char) then
								closestHead = head
								shortest = dist
							end
						else
							closestHead = head
							shortest = dist
						end
					end
				end
			end
		end
	end
	return closestHead
end

RunService.RenderStepped:Connect(function()
	if State.Aimbot and UIS:IsMouseButtonPressed(Enum.UserInputType.MouseButton1) then
		local head = GetClosestEnemyHead()
		if head then
			Camera.CFrame = CFrame.new(Camera.CFrame.Position, head.Position)
		end
	end
end)

--------------------------------------------------
-- ESP PRO (TIME FIXADO)
--------------------------------------------------
local ESP = {}

local function CreateESP(plr)
	if plr == Player then return end
	local box = Drawing.new("Square")
	box.Thickness = 1
	box.Filled = false

	local hp = Drawing.new("Line")
	hp.Thickness = 2

	local tracer = Drawing.new("Line")
	tracer.Thickness = 1

	ESP[plr] = {box, hp, tracer}
end

for _,p in pairs(Players:GetPlayers()) do CreateESP(p) end
Players.PlayerAdded:Connect(CreateESP)

RunService.RenderStepped:Connect(function()
	for plr,objs in pairs(ESP) do
		local char = plr.Character
		local hum = char and char:FindFirstChild("Humanoid")
		local hrp = char and char:FindFirstChild("HumanoidRootPart")

		if char and hum and hum.Health > 0 and hrp then
			local pos,vis = Camera:WorldToViewportPoint(hrp.Position)
			if vis and State.ESP then
				local isEnemy = plr.Team ~= Player.Team
				local size = Vector2.new(40,60)

				objs[1].Size = size
				objs[1].Position = Vector2.new(pos.X-size.X/2,pos.Y-size.Y/2)
				objs[1].Color = isEnemy and Color3.fromRGB(255,0,0) or Color3.fromRGB(0,170,255)
				objs[1].Visible = true

				local hpPercent = hum.Health / hum.MaxHealth
				local bottom = objs[1].Position.Y + size.Y
				objs[2].From = Vector2.new(objs[1].Position.X-4, bottom)
				objs[2].To = Vector2.new(objs[1].Position.X-4, bottom - (size.Y * hpPercent))
				objs[2].Color = Color3.fromRGB(0,255,0)
				objs[2].Visible = true

				if State.Tracer and isEnemy then
					objs[3].From = Camera.ViewportSize/2
					objs[3].To = Vector2.new(pos.X,pos.Y)
					objs[3].Color = Color3.fromRGB(255,0,0)
					objs[3].Visible = true
				else
					objs[3].Visible = false
				end
			else
				for _,o in pairs(objs) do o.Visible = false end
			end
		else
			for _,o in pairs(objs) do o.Visible = false end
		end
	end
end)

--------------------------------------------------
-- UI BUTTONS
--------------------------------------------------
CreateToggle(AimbotPage,"Aimbot",10,function(v) State.Aimbot = v end)
CreateToggle(AimbotPage,"WallCheck",50,function(v) State.WallCheck = v end)
CreateToggle(AimbotPage,"Mostrar FOV",90,function(v) State.ShowFOV = v end)

CreateSlider(AimbotPage,"FOV Size",140,50,400,200,function(v)
	State.FOVSize = v
end)

CreateToggle(VisualPage,"ESP Box Pro",10,function(v) State.ESP = v end)
CreateToggle(PlayerPage,"Tracer",10,function(v) State.Tracer = v end)

--------------------------------------------------
-- TAB SWITCH
--------------------------------------------------
TabAimbot.MouseButton1Click:Connect(function()
	AimbotPage.Visible = true
	VisualPage.Visible = false
	PlayerPage.Visible = false
end)

TabVisual.MouseButton1Click:Connect(function()
	AimbotPage.Visible = false
	VisualPage.Visible = true
	PlayerPage.Visible = false
end)

TabPlayer.MouseButton1Click:Connect(function()
	AimbotPage.Visible = false
	VisualPage.Visible = false
	PlayerPage.Visible = true
end)

--------------------------------------------------
-- PANEL TOGGLE
--------------------------------------------------
ToggleButton.MouseButton1Click:Connect(function()
	Panel.Visible = not Panel.Visible
end)
