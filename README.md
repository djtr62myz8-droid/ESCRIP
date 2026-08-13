--========================================================
-- P A N E L   M 4 T 1
-- LOCAL SCRIPT
-- StarterPlayer > StarterPlayerScripts
--========================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local GUI_NAME = "M4T1_MENU"
local PURPLE = Color3.fromRGB(125,75,255)

--========================================================
-- ESTADOS
--========================================================

local AimEnabled = false
local ESPEnabled = false
local XRayEnabled = false
local BodyBoxEnabled = false
local NameEnabled = false
local DistanceEnabled = false
local CrosshairEnabled = true
local NoClipEnabled = false
local AntiLagEnabled = false

local FOV = 120
local Smoothness = 0.15
local XRayTransparency = 0.75

local visuals = {}
local xrayParts = {}
local disabledEffects = {}

--========================================================
-- LIMPIAR GUI ANTERIOR
--========================================================

local old = playerGui:FindFirstChild(GUI_NAME)

if old then
	old:Destroy()
end

--========================================================
-- SCREEN GUI
--========================================================

local gui = Instance.new("ScreenGui")
gui.Name = GUI_NAME
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.DisplayOrder = 999
gui.Parent = playerGui

--========================================================
-- PANEL 650 x 430
--========================================================

local main = Instance.new("Frame")
main.Name = "Main"
main.Size = UDim2.fromOffset(650,430)
main.Position = UDim2.new(0.5,-325,0.5,-215)
main.BackgroundColor3 = Color3.fromRGB(10,14,24)
main.BorderSizePixel = 0
main.Parent = gui

local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0,15)
mainCorner.Parent = main

local mainStroke = Instance.new("UIStroke")
mainStroke.Thickness = 2
mainStroke.Color = PURPLE
mainStroke.Parent = main

--========================================================
-- FONDO
--========================================================

local background = Instance.new("Frame")
background.Size = UDim2.fromScale(1,1)
background.BackgroundTransparency = 0.88
background.BorderSizePixel = 0
background.Parent = main

local backgroundCorner = Instance.new("UICorner")
backgroundCorner.CornerRadius = UDim.new(0,15)
backgroundCorner.Parent = background

local gradient = Instance.new("UIGradient")
gradient.Rotation = 35
gradient.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0,Color3.fromRGB(75,30,180)),
	ColorSequenceKeypoint.new(0.5,Color3.fromRGB(30,70,180)),
	ColorSequenceKeypoint.new(1,Color3.fromRGB(150,30,220))
})
gradient.Parent = background

--========================================================
-- HEADER
--========================================================

local header = Instance.new("Frame")
header.Size = UDim2.new(1,0,0,55)
header.BackgroundTransparency = 1
header.ZIndex = 5
header.Parent = main

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1,-110,0,28)
title.Position = UDim2.fromOffset(18,5)
title.BackgroundTransparency = 1
title.Text = "P A N E L   M 4 T 1"
title.TextColor3 = Color3.fromRGB(255,255,255)
title.TextSize = 18
title.Font = Enum.Font.GothamBlack
title.TextXAlignment = Enum.TextXAlignment.Left
title.ZIndex = 6
title.Parent = header

local subtitle = Instance.new("TextLabel")
subtitle.Size = UDim2.new(1,-110,0,18)
subtitle.Position = UDim2.fromOffset(18,31)
subtitle.BackgroundTransparency = 1
subtitle.Text = "C O N T R O L   P A N E L  •  P R U E B A S"
subtitle.TextColor3 = Color3.fromRGB(160,165,180)
subtitle.TextSize = 8
subtitle.Font = Enum.Font.Gotham
subtitle.TextXAlignment = Enum.TextXAlignment.Left
subtitle.ZIndex = 6
subtitle.Parent = header

--========================================================
-- CERRAR
--========================================================

local close = Instance.new("TextButton")
close.Size = UDim2.fromOffset(38,38)
close.Position = UDim2.new(1,-48,0,8)
close.BackgroundColor3 = Color3.fromRGB(45,20,30)
close.Text = "X"
close.TextColor3 = Color3.fromRGB(255,70,80)
close.TextSize = 16
close.Font = Enum.Font.GothamBold
close.ZIndex = 10
close.Parent = header

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0,9)
closeCorner.Parent = close

--========================================================
-- REABRIR
--========================================================

local reopen = Instance.new("TextButton")
reopen.Size = UDim2.fromOffset(230,42)
reopen.Position = UDim2.new(0.5,-115,0,10)
reopen.BackgroundColor3 = Color3.fromRGB(15,18,30)
reopen.Text = "P A N E L   M 4 T 1"
reopen.TextColor3 = Color3.fromRGB(255,255,255)
reopen.TextSize = 13
reopen.Font = Enum.Font.GothamBlack
reopen.Visible = false
reopen.Parent = gui

local reopenCorner = Instance.new("UICorner")
reopenCorner.CornerRadius = UDim.new(0,10)
reopenCorner.Parent = reopen

local reopenStroke = Instance.new("UIStroke")
reopenStroke.Thickness = 2
reopenStroke.Color = PURPLE
reopenStroke.Parent = reopen

close.MouseButton1Click:Connect(function()
	main.Visible = false
	reopen.Visible = true
end)

reopen.MouseButton1Click:Connect(function()
	main.Visible = true
	reopen.Visible = false
end)

--========================================================
-- SIDEBAR
--========================================================

local sidebar = Instance.new("Frame")
sidebar.Size = UDim2.new(0,150,1,-65)
sidebar.Position = UDim2.fromOffset(10,58)
sidebar.BackgroundTransparency = 1
sidebar.ZIndex = 3
sidebar.Parent = main

--========================================================
-- CONTENT
--========================================================

local content = Instance.new("Frame")
content.Size = UDim2.new(1,-175,1,-65)
content.Position = UDim2.fromOffset(165,58)
content.BackgroundColor3 = Color3.fromRGB(19,23,37)
content.BorderSizePixel = 0
content.ZIndex = 3
content.Parent = main

local contentCorner = Instance.new("UICorner")
contentCorner.CornerRadius = UDim.new(0,11)
contentCorner.Parent = content

local pages = {}
local tabs = {}

--========================================================
-- PÁGINAS
--========================================================

local function createPage(name,text,y)

	local tab = Instance.new("TextButton")
	tab.Size = UDim2.new(1,0,0,38)
	tab.Position = UDim2.fromOffset(0,y)
	tab.BackgroundTransparency = 1
	tab.Text = text
	tab.TextColor3 = Color3.fromRGB(150,155,175)
	tab.TextSize = 10
	tab.Font = Enum.Font.GothamBold
	tab.TextXAlignment = Enum.TextXAlignment.Left
	tab.AutoButtonColor = false
	tab.ZIndex = 5
	tab.Parent = sidebar

	local padding = Instance.new("UIPadding")
	padding.PaddingLeft = UDim.new(0,12)
	padding.Parent = tab

	local page = Instance.new("ScrollingFrame")
	page.Size = UDim2.new(1,-20,1,-20)
	page.Position = UDim2.fromOffset(10,10)
	page.BackgroundTransparency = 1
	page.BorderSizePixel = 0
	page.ScrollBarThickness = 4
	page.CanvasSize = UDim2.new(0,0,0,700)
	page.Visible = false
	page.ZIndex = 4
	page.Parent = content

	pages[name] = page
	tabs[name] = tab

	tab.MouseButton1Click:Connect(function()

		for _,p in pairs(pages) do
			p.Visible = false
		end

		for _,t in pairs(tabs) do
			t.BackgroundTransparency = 1
			t.TextColor3 = Color3.fromRGB(150,155,175)
		end

		page.Visible = true
		tab.BackgroundTransparency = 0.1
		tab.BackgroundColor3 = PURPLE
		tab.TextColor3 = Color3.new(1,1,1)
	end)

	return page
end

local combatPage = createPage(
	"Combat",
	"◉   C O M B A T",
	0
)

local visualsPage = createPage(
	"Visuals",
	"◈   V I S U A L S",
	44
)

local crossPage = createPage(
	"Crosshair",
	"⊕   C R O S S H A I R",
	88
)

local mainPage = createPage(
	"Main",
	"⌂   M A I N",
	132
)

--========================================================
-- TOGGLE
--========================================================

local function createToggle(
	parent,
	name,
	description,
	y,
	callback
)

	local button = Instance.new("TextButton")
	button.Size = UDim2.new(1,-10,0,52)
	button.Position = UDim2.fromOffset(0,y)
	button.BackgroundColor3 = Color3.fromRGB(28,33,49)
	button.Text = ""
	button.AutoButtonColor = false
	button.ZIndex = 5
	button.Parent = parent

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0,8)
	corner.Parent = button

	local label = Instance.new("TextLabel")
	label.Size = UDim2.new(1,-70,0,20)
	label.Position = UDim2.fromOffset(11,5)
	label.BackgroundTransparency = 1
	label.Text = name
	label.TextColor3 = Color3.fromRGB(240,242,250)
	label.TextSize = 11
	label.Font = Enum.Font.GothamBold
	label.TextXAlignment = Enum.TextXAlignment.Left
	label.ZIndex = 6
	label.Parent = button

	local desc = Instance.new("TextLabel")
	desc.Size = UDim2.new(1,-70,0,17)
	desc.Position = UDim2.fromOffset(11,27)
	desc.BackgroundTransparency = 1
	desc.Text = description
	desc.TextColor3 = Color3.fromRGB(130,135,150)
	desc.TextSize = 8
	desc.Font = Enum.Font.Gotham
	desc.TextXAlignment = Enum.TextXAlignment.Left
	desc.ZIndex = 6
	desc.Parent = button

	local indicator = Instance.new("Frame")
	indicator.Size = UDim2.fromOffset(36,20)
	indicator.Position = UDim2.new(1,-47,0.5,-10)
	indicator.BackgroundColor3 = Color3.fromRGB(65,68,80)
	indicator.ZIndex = 6
	indicator.Parent = button

	local indicatorCorner = Instance.new("UICorner")
	indicatorCorner.CornerRadius = UDim.new(1,0)
	indicatorCorner.Parent = indicator

	local knob = Instance.new("Frame")
	knob.Size = UDim2.fromOffset(14,14)
	knob.Position = UDim2.fromOffset(3,3)
	knob.BackgroundColor3 = Color3.fromRGB(240,240,245)
	knob.ZIndex = 7
	knob.Parent = indicator

	local knobCorner = Instance.new("UICorner")
	knobCorner.CornerRadius = UDim.new(1,0)
	knobCorner.Parent = knob

	local state = false

	button.MouseButton1Click:Connect(function()

		state = not state

		if state then
			indicator.BackgroundColor3 = PURPLE
			knob.Position = UDim2.new(1,-17,0,3)
		else
			indicator.BackgroundColor3 = Color3.fromRGB(65,68,80)
			knob.Position = UDim2.fromOffset(3,3)
		end

		callback(state)
	end)
end

--========================================================
-- SLIDER
--========================================================

local function createSlider(
	parent,
	text,
	y,
	minValue,
	maxValue,
	current,
	callback
)

	local box = Instance.new("Frame")
	box.Size = UDim2.new(1,-10,0,60)
	box.Position = UDim2.fromOffset(0,y)
	box.BackgroundColor3 = Color3.fromRGB(28,33,49)
	box.BorderSizePixel = 0
	box.ZIndex = 5
	box.Parent = parent

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0,8)
	corner.Parent = box

	local label = Instance.new("TextLabel")
	label.Size = UDim2.new(1,0,0,20)
	label.Position = UDim2.fromOffset(11,4)
	label.BackgroundTransparency = 1
	label.TextColor3 = Color3.new(1,1,1)
	label.TextSize = 10
	label.Font = Enum.Font.GothamBold
	label.TextXAlignment = Enum.TextXAlignment.Left
	label.ZIndex = 6
	label.Parent = box

	local bar = Instance.new("Frame")
	bar.Size = UDim2.new(1,-22,0,6)
	bar.Position = UDim2.fromOffset(11,42)
	bar.BackgroundColor3 = Color3.fromRGB(60,64,76)
	bar.BorderSizePixel = 0
	bar.ZIndex = 6
	bar.Parent = box

	local fill = Instance.new("Frame")
	fill.BackgroundColor3 = PURPLE
	fill.BorderSizePixel = 0
	fill.ZIndex = 7
	fill.Parent = bar

	local fillCorner = Instance.new("UICorner")
	fillCorner.CornerRadius = UDim.new(1,0)
	fillCorner.Parent = fill

	local dragging = false

	local function update(value)

		value = math.clamp(
			value,
			minValue,
			maxValue
		)

		local alpha =
			(value-minValue) /
			(maxValue-minValue)

		fill.Size =
			UDim2.new(alpha,0,1,0)

		label.Text =
			text..": "..string.format("%.2f",value)

		callback(value)
	end

	update(current)

	bar.InputBegan:Connect(function(input)

		if input.UserInputType ==
			Enum.UserInputType.MouseButton1
			or input.UserInputType ==
			Enum.UserInputType.Touch then

			dragging = true

			local alpha =
				math.clamp(
					(input.Position.X -
					bar.AbsolutePosition.X) /
					bar.AbsoluteSize.X,
					0,1
				)

			update(
				minValue +
				alpha *
				(maxValue-minValue)
			)
		end
	end)

	UserInputService.InputChanged:Connect(function(input)

		if not dragging then
			return
		end

		if input.UserInputType ==
			Enum.UserInputType.MouseMovement
			or input.UserInputType ==
			Enum.UserInputType.Touch then

			local alpha =
				math.clamp(
					(input.Position.X -
					bar.AbsolutePosition.X) /
					bar.AbsoluteSize.X,
					0,1
				)

			update(
				minValue +
				alpha *
				(maxValue-minValue)
			)
		end
	end)

	UserInputService.InputEnded:Connect(function(input)

		if input.UserInputType ==
			Enum.UserInputType.MouseButton1
			or input.UserInputType ==
			Enum.UserInputType.Touch then

			dragging = false
		end
	end)
end

--========================================================
-- FOV
--========================================================

local fovCircle = Instance.new("Frame")
fovCircle.AnchorPoint = Vector2.new(0.5,0.5)
fovCircle.Position = UDim2.fromScale(0.5,0.5)
fovCircle.Size = UDim2.fromOffset(FOV*2,FOV*2)
fovCircle.BackgroundTransparency = 1
fovCircle.Visible = false
fovCircle.ZIndex = 2
fovCircle.Parent = gui

local fovCorner = Instance.new("UICorner")
fovCorner.CornerRadius = UDim.new(1,0)
fovCorner.Parent = fovCircle

local fovStroke = Instance.new("UIStroke")
fovStroke.Thickness = 2
fovStroke.Color = PURPLE
fovStroke.Parent = fovCircle

--========================================================
-- CROSSHAIR
--========================================================

local crosshair = Instance.new("TextLabel")
crosshair.Size = UDim2.fromOffset(60,60)
crosshair.AnchorPoint = Vector2.new(0.5,0.5)
crosshair.Position = UDim2.fromScale(0.5,0.5)
crosshair.BackgroundTransparency = 1
crosshair.Text = "+"
crosshair.TextColor3 = PURPLE
crosshair.TextSize = 32
crosshair.Font = Enum.Font.GothamBlack
crosshair.Visible = true
crosshair.ZIndex = 50
crosshair.Parent = gui

--========================================================
-- X-RAY
--========================================================

local function isCharacterPart(part)

	for _,plr in ipairs(Players:GetPlayers()) do

		if plr.Character
			and part:IsDescendantOf(plr.Character) then
			return true
		end
	end

	return false
end

local function applyXRay(part)

	if not XRayEnabled then
		return
	end

	if not part:IsA("BasePart") then
		return
	end

	if isCharacterPart(part) then
		return
	end

	if part.Transparency >= 1 then
		return
	end

	if xrayParts[part] == nil then
		xrayParts[part] =
			part.LocalTransparencyModifier
	end

	part.LocalTransparencyModifier =
		XRayTransparency
end

local function enableXRay()

	XRayEnabled = true

	for _,object in ipairs(
		workspace:GetDescendants()
	) do
		applyXRay(object)
	end
end

local function disableXRay()

	XRayEnabled = false

	for part,oldTransparency in pairs(
		xrayParts
	) do

		if part and part.Parent then
			part.LocalTransparencyModifier =
				oldTransparency
		end
	end

	table.clear(xrayParts)
end

workspace.DescendantAdded:Connect(function(object)

	if XRayEnabled
		and object:IsA("BasePart") then

		task.defer(function()
			applyXRay(object)
		end)
	end
end)

--========================================================
-- ESP
--========================================================

local function removeVisual(plr)

	local data = visuals[plr]

	if not data then
		return
	end

	for _,object in pairs(data) do

		if typeof(object) == "Instance"
			and object.Parent then

			object:Destroy()
		end
	end

	visuals[plr] = nil
end

local function createVisual(plr)

	if plr == player then
		return
	end

	if not plr.Character then
		return
	end

	removeVisual(plr)

	local character = plr.Character
	local root =
		character:FindFirstChild(
			"HumanoidRootPart"
		)

	if not root then
		return
	end

	local data = {}
	visuals[plr] = data

	local highlight = Instance.new("Highlight")
	highlight.Name = "M4T1_ESP"
	highlight.Adornee = character
	highlight.FillColor = PURPLE
	highlight.OutlineColor = PURPLE
	highlight.FillTransparency = 0.55
	highlight.OutlineTransparency = 0
	highlight.DepthMode =
		Enum.HighlightDepthMode.AlwaysOnTop
	highlight.Enabled = ESPEnabled
	highlight.Parent = character

	data.highlight = highlight

	local box = Instance.new("BoxHandleAdornment")
	box.Name = "M4T1_BODY_BOX"
	box.Adornee = root
	box.Size = Vector3.new(4.5,7,3)
	box.Color3 = PURPLE
	box.Transparency = 0.4
	box.AlwaysOnTop = true
	box.ZIndex = 10
	box.Visible = BodyBoxEnabled
	box.Parent = character

	data.box = box

	local billboard = Instance.new("BillboardGui")
	billboard.Name = "M4T1_INFO"
	billboard.Adornee = root
	billboard.Size = UDim2.fromOffset(220,55)
	billboard.StudsOffset = Vector3.new(0,4.5,0)
	billboard.AlwaysOnTop = true
	billboard.Enabled =
		NameEnabled or DistanceEnabled
	billboard.Parent = character

	data.billboard = billboard

	local info = Instance.new("TextLabel")
	info.Size = UDim2.fromScale(1,1)
	info.BackgroundTransparency = 1
	info.TextColor3 = PURPLE
	info.TextStrokeTransparency = 0.25
	info.TextSize = 12
	info.Font = Enum.Font.GothamBold
	info.Parent = billboard

	data.info = info
end

local function updateVisuals()

	for _,plr in ipairs(
		Players:GetPlayers()
	) do

		if plr ~= player then

			if plr.Character then

				if not visuals[plr] then
					createVisual(plr)
				end

				local data = visuals[plr]

				if data then

					local root =
						plr.Character:
						FindFirstChild(
							"HumanoidRootPart"
						)

					if root then

						data.highlight.Adornee =
							plr.Character

						data.highlight.Enabled =
							ESPEnabled

						data.box.Adornee =
							root

						data.box.Visible =
							BodyBoxEnabled

						data.billboard.Adornee =
							root

						data.billboard.Enabled =
							NameEnabled or
							DistanceEnabled

						local text = ""

						if NameEnabled then
							text = plr.Name
						end

						if DistanceEnabled then

							local myCharacter =
								player.Character

							local myRoot =
								myCharacter and
								myCharacter:
								FindFirstChild(
									"HumanoidRootPart"
								)

							if myRoot then

								local distance =
									(root.Position -
									myRoot.Position).Magnitude

								if text ~= "" then
									text = text.."\n"
								end

								text =
									text..
									math.floor(distance)..
									" studs"
							end
						end

						data.info.Text = text
					end
				end
			else
				removeVisual(plr)
			end
		end
	end
end

Players.PlayerAdded:Connect(function(plr)

	plr.CharacterAdded:Connect(function()

		task.wait(0.5)
		updateVisuals()

	end)
end)

Players.PlayerRemoving:Connect(function(plr)
	removeVisual(plr)
end)

--========================================================
-- AIM
--========================================================

local function getClosestTarget()

	local camera =
		workspace.CurrentCamera

	if not camera then
		return nil
	end

	local center =
		camera.ViewportSize / 2

	local closest
	local closestDistance = math.huge

	for _,plr in ipairs(
		Players:GetPlayers()
	) do

		if plr ~= player
			and plr.Character then

			local humanoid =
				plr.Character:
				FindFirstChildOfClass(
					"Humanoid"
				)

			local head =
				plr.Character:
				FindFirstChild("Head")

			if humanoid
				and humanoid.Health > 0
				and head then

				local pos,onScreen =
					camera:WorldToViewportPoint(
						head.Position
					)

				if onScreen and pos.Z > 0 then

					local dx =
						pos.X-center.X

					local dy =
						pos.Y-center.Y

					local distance =
						math.sqrt(dx*dx+dy*dy)

					if distance <= FOV
						and distance <
							closestDistance then

						closestDistance =
							distance

						closest = head
					end
				end
			end
		end
	end

	return closest
end

--========================================================
-- NO CLIP
--========================================================

local function updateNoClip()

	if not NoClipEnabled then
		return
	end

	local character =
		player.Character

	if not character then
		return
	end

	for _,part in ipairs(
		character:GetDescendants()
	) do

		if part:IsA("BasePart") then
			part.CanCollide = false
		end
	end
end

--========================================================
-- ANTI LAG
--========================================================

local function enableAntiLag()

	AntiLagEnabled = true

	table.clear(disabledEffects)

	for _,object in ipairs(
		workspace:GetDescendants()
	) do

		if object:IsA("ParticleEmitter")
			or object:IsA("Trail")
			or object:IsA("Beam") then

			if object.Enabled then

				disabledEffects[object] = true
				object.Enabled = false

			end
		end
	end
end

local function disableAntiLag()

	AntiLagEnabled = false

	for object in pairs(
		disabledEffects
	) do

		if object and object.Parent then
			object.Enabled = true
		end
	end

	table.clear(disabledEffects)
end

workspace.DescendantAdded:Connect(function(object)

	if AntiLagEnabled then

		if object:IsA("ParticleEmitter")
			or object:IsA("Trail")
			or object:IsA("Beam") then

			object.Enabled = false
			disabledEffects[object] = true

		end
	end
end)

--========================================================
-- COMBAT
--========================================================

createToggle(
	combatPage,
	"A I M",
	"Aim de prueba para tu experiencia",
	0,
	function(value)

		AimEnabled = value
		fovCircle.Visible = value

	end
)

createSlider(
	combatPage,
	"F O V",
	57,
	40,
	400,
	FOV,
	function(value)

		FOV = math.floor(value)

		fovCircle.Size =
			UDim2.fromOffset(
				FOV*2,
				FOV*2
			)

	end
)

createSlider(
	combatPage,
	"S M O O T H N E S S",
	124,
	0.01,
	1,
	Smoothness,
	function(value)

		Smoothness = value

	end
)

--========================================================
-- VISUALS
--========================================================

createToggle(
	visualsPage,
	"E S P",
	"Resalta jugadores del mismo servidor",
	0,
	function(value)

		ESPEnabled = value
		updateVisuals()

	end
)

createToggle(
	visualsPage,
	"X - R A Y",
	"Transparencia local de las paredes",
	57,
	function(value)

		if value then
			enableXRay()
		else
			disableXRay()
		end

	end
)

createToggle(
	visualsPage,
	"B O D Y   B O X",
	"Caja alrededor del jugador",
	114,
	function(value)

		BodyBoxEnabled = value
		updateVisuals()

	end
)

createToggle(
	visualsPage,
	"N O M B R E",
	"Mostrar nombre",
	171,
	function(value)

		NameEnabled = value
		updateVisuals()

	end
)

createToggle(
	visualsPage,
	"D I S T A N C I A",
	"Mostrar distancia",
	228,
	function(value)

		DistanceEnabled = value
		updateVisuals()

	end
)

--========================================================
-- CROSSHAIR
--========================================================

createToggle(
	crossPage,
	"C R O S S H A I R",
	"Mostrar u ocultar la mira",
	0,
	function(value)

		CrosshairEnabled = value
		crosshair.Visible = value

	end
)

--========================================================
-- MAIN
--========================================================

createToggle(
	mainPage,
	"N O   C L I P",
	"Desactiva las colisiones de tu personaje",
	0,
	function(value)

		NoClipEnabled = value

		if not value then

			local character =
				player.Character

			if character then

				for _,part in ipairs(
					character:GetDescendants()
				) do

					if part:IsA("BasePart") then
						part.CanCollide = true
					end

				end
			end
		end
	end
)

createToggle(
	mainPage,
	"A N T I   L A G",
	"Reduce partículas y efectos locales",
	57,
	function(value)

		if value then
			enableAntiLag()
		else
			disableAntiLag()
		end

	end
)

--========================================================
-- LOOP PRINCIPAL
--========================================================

local visualTimer = 0

RunService.RenderStepped:Connect(function(delta)

	updateNoClip()

	if AimEnabled then

		local camera =
			workspace.CurrentCamera

		local target =
			getClosestTarget()

		if camera and target then

			local desired =
				CFrame.lookAt(
					camera.CFrame.Position,
					target.Position
				)

			camera.CFrame =
				camera.CFrame:Lerp(
					desired,
					math.clamp(
						Smoothness,
						0.01,
						1
					)
				)
		end
	end

	visualTimer += delta

	if visualTimer >= 0.15 then

		visualTimer = 0
		updateVisuals()

	end
end)

--========================================================
-- DRAG
--========================================================

local dragging = false
local dragStart
local startPosition

header.InputBegan:Connect(function(input)

	if input.UserInputType ==
		Enum.UserInputType.MouseButton1
		or input.UserInputType ==
		Enum.UserInputType.Touch then

		dragging = true
		dragStart = input.Position
		startPosition = main.Position
	end
end)

UserInputService.InputChanged:Connect(function(input)

	if not dragging then
		return
	end

	if input.UserInputType ==
		Enum.UserInputType.MouseMovement
		or input.UserInputType ==
		Enum.UserInputType.Touch then

		local delta =
			input.Position -
			dragStart

		main.Position =
			UDim2.new(
				startPosition.X.Scale,
				startPosition.X.Offset + delta.X,
				startPosition.Y.Scale,
				startPosition.Y.Offset + delta.Y
			)
	end
end)

UserInputService.InputEnded:Connect(function(input)

	if input.UserInputType ==
		Enum.UserInputType.MouseButton1
		or input.UserInputType ==
		Enum.UserInputType.Touch then

		dragging = false
	end
end)

--========================================================
-- RIGHT SHIFT
--========================================================

UserInputService.InputBegan:Connect(function(
	input,
	processed
)

	if processed then
		return
	end

	if input.KeyCode ==
		Enum.KeyCode.RightShift then

		main.Visible =
			not main.Visible

		reopen.Visible =
			not main.Visible
	end
end)

--========================================================
-- PÁGINA INICIAL
--========================================================

combatPage.Visible = true

tabs["Combat"].BackgroundTransparency = 0.1
tabs["Combat"].BackgroundColor3 = PURPLE
tabs["Combat"].TextColor3 = Color3.new(1,1,1)

updateVisuals()

print("================================")
print("P A N E L   M 4 T 1")
print("650 x 430")
print("COMBAT / VISUALS / CROSSHAIR / MAIN")
print("X-RAY / ESP / AIM / FOV")
print("NO CLIP / ANTI LAG")
print("MENU CARGADO")
print("================================")
