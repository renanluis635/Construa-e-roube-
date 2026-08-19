-- ========================================================
-- SISTEMA DE KEY PERSONALIZADO (ROXO)
-- ========================================================

local correctKey = "roube"
local unlocked = false

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ConstruaUmaBaseERoubeKey"
screenGui.Parent = game:GetService("CoreGui")

local frame = Instance.new("Frame")
frame.Parent = screenGui
frame.Size = UDim2.fromScale(0.35, 0.32)
frame.Position = UDim2.fromScale(0.5, 0.5)
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.BackgroundColor3 = Color3.fromRGB(45, 20, 75)
frame.BorderSizePixel = 0
Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 14)

local stroke = Instance.new("UIStroke")
stroke.Parent = frame
stroke.Color = Color3.fromRGB(170, 0, 255)
stroke.Thickness = 1.5

local title = Instance.new("TextLabel")
title.Parent = frame
title.Size = UDim2.fromScale(1, 0.2)
title.BackgroundTransparency = 1
title.Text = "Construa uma base e roube"
title.Font = Enum.Font.GothamBold
title.TextScaled = true
title.TextColor3 = Color3.new(1, 1, 1)

local line = Instance.new("Frame")
line.Parent = frame
line.Size = UDim2.fromScale(0.8, 0.006)
line.Position = UDim2.fromScale(0.1, 0.22)
line.BackgroundColor3 = Color3.fromRGB(190, 80, 255)
line.BorderSizePixel = 0

local mainText = Instance.new("TextLabel")
mainText.Parent = frame
mainText.Position = UDim2.fromScale(0, 0.26)
mainText.Size = UDim2.fromScale(1, 0.15)
mainText.BackgroundTransparency = 1
mainText.Text = "Sistema de Key"
mainText.Font = Enum.Font.Gotham
mainText.TextScaled = true
mainText.TextColor3 = Color3.new(1, 1, 1)

local subText = Instance.new("TextLabel")
subText.Parent = frame
subText.Position = UDim2.fromScale(0, 0.38)
subText.Size = UDim2.fromScale(1, 0.12)
subText.BackgroundTransparency = 1
subText.Text = "Insira a sua chave de acesso abaixo para continuar.\nby Renan"
subText.Font = Enum.Font.Gotham
subText.TextScaled = true
subText.TextColor3 = Color3.fromRGB(230, 230, 230)

local textBox = Instance.new("TextBox")
textBox.Parent = frame
textBox.Position = UDim2.fromScale(0.15, 0.54)
textBox.Size = UDim2.fromScale(0.7, 0.13)
textBox.PlaceholderText = "Coloque sua chave…"
textBox.Text = ""
textBox.BackgroundColor3 = Color3.fromRGB(25, 10, 45)
textBox.TextColor3 = Color3.new(1, 1, 1)
textBox.Font = Enum.Font.Gotham
textBox.TextScaled = true
Instance.new("UICorner", textBox).CornerRadius = UDim.new(0, 10)

local confirm = Instance.new("TextButton")
confirm.Parent = frame
confirm.Position = UDim2.fromScale(0.3, 0.72)
confirm.Size = UDim2.fromScale(0.4, 0.15)
confirm.Text = "CONFIRMAR"
confirm.Font = Enum.Font.GothamBold
confirm.TextScaled = true
confirm.TextColor3 = Color3.fromRGB(255, 255, 255)
confirm.BackgroundColor3 = Color3.fromRGB(150, 40, 220)
Instance.new("UICorner", confirm).CornerRadius = UDim.new(0, 10)

confirm.MouseButton1Click:Connect(function()
	if textBox.Text == correctKey then
		unlocked = true
		screenGui:Destroy()
	end
end)

repeat
	task.wait()
until unlocked


-- ========================================================
-- SCRIPT PRINCIPAL
-- ========================================================

local Rayfield = loadstring(game:HttpGet(
	'https://sirius.menu/rayfield'
))()

local Window = Rayfield:CreateWindow({
	Name = "Construa uma base e roube",
	LoadingTitle = "Carregando...",
	LoadingSubtitle = "Delta Mobile Compatible",
	ConfigurationSaving = { Enabled = false },
	Discord = { Enabled = false },
	KeySystem = false
})

local Tab = Window:CreateTab("Principal", 4483362458)


-- ========================================================
-- SERVIÇOS E VARIÁVEIS LOCAIS
-- ========================================================

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local isOneBlockActive = false
local isEspActive = false
local espConnections = {}


-- ========================================================
-- ONEBLOCK
-- ========================================================

local function ApplyScale(character, scaleFactor)
	if not character then return end
	local hum = character:FindFirstChildOfClass("Humanoid")

	pcall(function()
		character:ScaleTo(scaleFactor)
	end)

	if hum then
		local heightScale = hum:FindFirstChild("BodyHeightScale")
		local widthScale = hum:FindFirstChild("BodyWidthScale")
		local depthScale = hum:FindFirstChild("BodyDepthScale")
		local headScale = hum:FindFirstChild("HeadScale")

		pcall(function()
			if heightScale then heightScale.Value = scaleFactor end
			if widthScale then widthScale.Value = scaleFactor end
			if depthScale then depthScale.Value = scaleFactor end
			if headScale then headScale.Value = scaleFactor end
		end)

		hum.HipHeight = (scaleFactor == 1) and 2 or 0.85

		for _, part in pairs(character:GetChildren()) do
			if part:IsA("BasePart") then
				if scaleFactor < 1 then
					part.CanCollide = true
					part.CustomPhysicalProperties = PhysicalProperties.new(0.7, 0.0, 0.0, 1, 1)
				else
					part.CanCollide = true
					part.CustomPhysicalProperties = nil
				end
			end
		end
	end
end

local function OneBlock(state)
	isOneBlockActive = state
	local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
	if isOneBlockActive then
		ApplyScale(character, 0.3)
	else
		ApplyScale(character, 1)
	end
end


-- ========================================================
-- ESP
-- ========================================================

local function CreateESP(player)
	if player == LocalPlayer then return end

	local function ApplyNameTag(character)
		if not character then return end
		local head = character:WaitForChild("Head", 5)
		if not head then return end

		if head:FindFirstChild("NameESP") then
			head.NameESP:Destroy()
		end

		local billboard = Instance.new("BillboardGui")
		billboard.Name = "NameESP"
		billboard.Adornee = head
		billboard.Size = UDim2.new(0, 100, 0, 50)
		billboard.StudsOffset = Vector3.new(0, 2.5, 0)
		billboard.AlwaysOnTop = true

		local textLabel = Instance.new("TextLabel")
		textLabel.Parent = billboard
		textLabel.Size = UDim2.new(1, 0, 1, 0)
		textLabel.BackgroundTransparency = 1
		textLabel.Text = player.Name
		textLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
		textLabel.TextStrokeTransparency = 0.2
		textLabel.TextSize = 16
		textLabel.Font = Enum.Font.SourceSansBold

		billboard.Parent = head
	end

	if player.Character then
		ApplyNameTag(player.Character)
	end

	local connection = player.CharacterAdded:Connect(function(character)
		if isEspActive then
			ApplyNameTag(character)
		end
	end)

	table.insert(espConnections, connection)
end

local function ToggleESP(state)
	isEspActive = state
	if isEspActive then
		for _, player in pairs(Players:GetPlayers()) do
			CreateESP(player)
		end
		local playerJoinedConnection = Players.PlayerAdded:Connect(function(player)
			if isEspActive then
				CreateESP(player)
			end
		end)
		table.insert(espConnections, playerJoinedConnection)
	else
		for _, connection in pairs(espConnections) do
			pcall(function() connection:Disconnect() end)
		end
		espConnections = {}
		for _, player in pairs(Players:GetPlayers()) do
			if player.Character and player.Character:FindFirstChild("Head") then
				local esp = player.Character.Head:FindFirstChild("NameESP")
				if esp then esp:Destroy() end
			end
		end
	end
end


-- ========================================================
-- VELOCIDADE (WALKSPEED)
-- ========================================================

local function SetWalkSpeed(speed)
	local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
	local humanoid = character:FindFirstChildOfClass("Humanoid")
	if humanoid then
		humanoid.WalkSpeed = speed
	end
end


-- ========================================================
-- INTERFACE (RAYFIELD UI)
-- ========================================================

Tab:CreateToggle({
	Name = "Modo 1 Bloco (OneBlock)",
	CurrentValue = false,
	Flag = "OneBlockToggle",
	Callback = function(Value)
		OneBlock(Value)
	end,
})

Tab:CreateToggle({
	Name = "ESP Nomes em Vermelho",
	CurrentValue = false,
	Flag = "EspToggle",
	Callback = function(Value)
		ToggleESP(Value)
	end,
})

Tab:CreateSlider({
	Name = "Velocidade (WalkSpeed)",
	Range = {16, 100},
	Increment = 1,
	Suffix = " Studs",
	CurrentValue = 16,
	Flag = "SpeedSlider",
	Callback = function(Value)
		SetWalkSpeed(Value)
	end,
})
