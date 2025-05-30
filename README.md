-- Th1xDevMenu completo - loadstring ready
-- Autor: Tr1nX_777 + ChatGPT
-- Menu estilo macabro, roxo/preto/cinza escuro, som e animações clean
-- Loadstring-friendly, tudo client-side

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

-- Sons básicos do Roblox (embed simples)
local function createSound(parent, soundId)
    local sound = Instance.new("Sound", parent)
    sound.SoundId = "rbxassetid://"..soundId
    sound.Volume = 0.3
    sound.PlayOnRemove = false
    return sound
end

-- Sons para clicks ON/OFF e toggles
local soundClickOn = createSound(PlayerGui, 12222216)   -- click sound 1
local soundClickOff = createSound(PlayerGui, 12222190)  -- click sound 2

-- Criar GUI principal
local gui = Instance.new("ScreenGui")
gui.Name = "Th1xDevMenu"
gui.ResetOnSpawn = false
gui.Parent = PlayerGui

-- Fundo principal
local Frame = Instance.new("Frame", gui)
Frame.Size = UDim2.new(0, 420, 0, 360)
Frame.Position = UDim2.new(0.3, 0, 0.25, 0)
Frame.BackgroundColor3 = Color3.fromRGB(30, 0, 60)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0, 15)

-- Barra lateral das abas
local TabHolder = Instance.new("Frame", Frame)
TabHolder.Size = UDim2.new(0, 110, 1, -40)
TabHolder.Position = UDim2.new(0, 0, 0, 40)
TabHolder.BackgroundColor3 = Color3.fromRGB(15, 0, 30)
TabHolder.BorderSizePixel = 0

local TabLayout = Instance.new("UIListLayout", TabHolder)
TabLayout.SortOrder = Enum.SortOrder.LayoutOrder
TabLayout.Padding = UDim.new(0, 5)

-- Título do menu no topo
local TitleLabel = Instance.new("TextLabel", Frame)
TitleLabel.Size = UDim2.new(1, 0, 0, 40)
TitleLabel.Position = UDim2.new(0, 0, 0, 0)
TitleLabel.BackgroundColor3 = Color3.fromRGB(45, 0, 90)
TitleLabel.BorderSizePixel = 0
TitleLabel.Text = "Th1x Dev Menu"
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextSize = 22
TitleLabel.TextColor3 = Color3.fromRGB(200, 170, 255)

local UICornerTitle = Instance.new("UICorner", TitleLabel)
UICornerTitle.CornerRadius = UDim.new(0, 15)

-- Variáveis para abas e frames
local TabList = {}
local CurrentTab = nil

local function createTab(name)
    local btn = Instance.new("TextButton", TabHolder)
    btn.Size = UDim2.new(1, 0, 0, 30)
    btn.BackgroundColor3 = Color3.fromRGB(60, 0, 100)
    btn.TextColor3 = Color3.fromRGB(220, 200, 255)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.Text = name
    btn.AutoButtonColor = false

    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 8)

    -- Hover effect
    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(90, 0, 130)}):Play()
    end)
    btn.MouseLeave:Connect(function()
        if CurrentTab ~= btn then
            TweenService:Create(btn, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(60, 0, 100)}):Play()
        end
    end)

    local tabFrame = Instance.new("Frame", Frame)
    tabFrame.Size = UDim2.new(1, -120, 1, -40)
    tabFrame.Position = UDim2.new(0, 110, 0, 40)
    tabFrame.BackgroundColor3 = Color3.fromRGB(20, 10, 40)
    tabFrame.Visible = false
    tabFrame.BorderSizePixel = 0
    local tabCorner = Instance.new("UICorner", tabFrame)
    tabCorner.CornerRadius = UDim.new(0, 12)

    -- Ativa aba e desativa outras
    btn.MouseButton1Click:Connect(function()
        if CurrentTab and CurrentTab ~= btn then
            -- Desativa aba antiga
            for _, t in pairs(TabList) do
                t.Frame.Visible = false
                TweenService:Create(t.Button, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(60, 0, 100)}):Play()
            end
        end
        tabFrame.Visible = true
        CurrentTab = btn
        TweenService:Create(btn, TweenInfo.new(0.3), {BackgroundColor3 = Color3.fromRGB(120, 20, 180)}):Play()
        soundClickOn:Play()
    end)

    table.insert(TabList, {Button = btn, Frame = tabFrame})
    return tabFrame, btn
end

-- Criar as abas e guardar
local HomeTab, homeBtn = createTab("Home")
local OutfitTab, outfitBtn = createTab("Outfit")
local RiskTab, riskBtn = createTab("Risk")

-- Ativar HomeTab inicialmente
homeBtn.BackgroundColor3 = Color3.fromRGB(120, 20, 180)
HomeTab.Visible = true
CurrentTab = homeBtn

-- =======================
-- Funções gerais e helpers
-- =======================

-- Função para criar toggle buttons estilizados
local function createToggleButton(parent, text, position)
    local btn = Instance.new("TextButton", parent)
    btn.Size = UDim2.new(0, 200, 0, 35)
    btn.Position = position
    btn.BackgroundColor3 = Color3.fromRGB(50, 0, 80)
    btn.TextColor3 = Color3.fromRGB(220, 210, 255)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.Text = text
    btn.AutoButtonColor = false

    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 10)

    -- Indicador ON/OFF
    local indicator = Instance.new("Frame", btn)
    indicator.Size = UDim2.new(0, 35, 0, 30)
    indicator.Position = UDim2.new(1, -40, 0, 2)
    indicator.BackgroundColor3 = Color3.fromRGB(120, 0, 180)
    indicator.BorderSizePixel = 0
    indicator.Visible = false
    local indicatorCorner = Instance.new("UICorner", indicator)
    indicatorCorner.CornerRadius = UDim.new(0, 5)

    return btn, indicator
end

-- Função para tocar som ao clicar, passando toggle state true/false
local function playToggleSound(state)
    if state then
        soundClickOn:Play()
    else
        soundClickOff:Play()
    end
end

-- =========================
-- IMPLEMENTAÇÃO HOME TAB
-- =========================

-- ESP Toggle
local espToggle, espIndicator = createToggleButton(HomeTab, "ESP ON/OFF", UDim2.new(0, 15, 0, 10))
local espEnabled = false

espToggle.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    espIndicator.Visible = espEnabled
    playToggleSound(espEnabled)

    -- Aqui você colocaria a implementação real de ESP (contornos nos players)
    if espEnabled then
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                -- Criar Highlight
                if not plr.Character:FindFirstChild("Th1xHighlight") then
                    local hl = Instance.new("Highlight")
                    hl.Name = "Th1xHighlight"
                    hl.Adornee = plr.Character
                    hl.FillColor = Color3.fromRGB(150, 50, 200)
                    hl.OutlineColor = Color3.fromRGB(100, 0, 150)
                    hl.Parent = plr.Character
                end
            end
        end
    else
        -- Remover todos Highlights
        for _, plr in pairs(Players:GetPlayers()) do
            if plr.Character and plr.Character:FindFirstChild("Th1xHighlight") then
                plr.Character.Th1xHighlight:Destroy()
            end
        end
    end
end)

-- INVISIBILIDADE
local invisToggle, invisIndicator = createToggleButton(HomeTab, "Invisível", UDim2.new(0, 15, 0, 60))
local invisEnabled = false

local function setInvisible(state)
    local char = player.Character
    if not char then return end
    for _, part in pairs(char:GetChildren()) do
        if part:IsA("BasePart") then
            part.Transparency = state and 1 or 0
            part.CanCollide = not state
        elseif part:IsA("Decal") then
            part.Transparency = state and 1 or 0
        elseif part:IsA("ParticleEmitter") then
            part.Enabled = not state
        end
    end
end

invisToggle.MouseButton1Click:Connect(function()
    invisEnabled = not invisEnabled
    invisIndicator.Visible = invisEnabled
    setInvisible(invisEnabled)
    playToggleSound(invisEnabled)
end)

-- TELEPORTAR
local tpBox = Instance.new("TextBox", HomeTab)
tpBox.PlaceholderText = "Nome do Jogador"
tpBox.Size = UDim2.new(0, 200, 0, 30)
tpBox.Position = UDim2.new(0, 15, 0, 110)
tpBox.BackgroundColor3 = Color3.fromRGB(45, 0, 80)
tpBox.TextColor3 = Color3.fromRGB(220, 210, 255)
tpBox.Font = Enum.Font.GothamBold
tpBox.TextSize = 16
local tpCorner = Instance.new("UICorner", tpBox)
tpCorner.CornerRadius = UDim.new(0, 8)

local tpBtn = Instance.new("TextButton", HomeTab)
tpBtn.Text = "Teleportar"
tpBtn.Size = UDim2.new(0, 200, 0, 35)
tpBtn.Position = UDim2.new(0, 15, 0, 150)
tpBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 150)
tpBtn.TextColor3 = Color3.fromRGB(230, 220, 255)
tpBtn.Font = Enum.Font.GothamBold
tpBtn.TextSize = 16
tpBtn.AutoButtonColor = false
local tpBtnCorner = Instance.new("UICorner", tpBtn)
tpBtnCorner.CornerRadius = UDim.new(0, 10)

tpBtn.MouseButton1Click:Connect(function()
    local targetName = tpBox.Text:lower():gsub("%s+", "")
    if targetName == "" then
        tpBtn.Text = "Digite um nome!"
        wait(1.5)
        tpBtn.Text = "Teleportar"
        return
    end

    local targetPlayer = nil
    for _, plr in pairs(Players:GetPlayers()) do
        if plr.Name:lower():gsub("%s+", "") == targetName then
            targetPlayer = plr
            break
        end
    end

    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        player.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0, 3, 0)
        tpBtn.Text = "Teleportado!"
        wait(1.5)
        tpBtn.Text = "Teleportar"
    else
        tpBtn.Text = "Jogador não encontrado!"
        wait(1.5)
        tpBtn.Text = "Teleportar"
    end
end)

-- VOO (WASD + SPACE + CTRL) clean e macabra
local flyToggle, flyIndicator = createToggleButton(HomeTab, "Fly ON/OFF", UDim2.new(0, 15, 0, 200))
local flying = false
local speed = 60
local flyForce = nil
local bodyGyro = nil
local bodyVelocity = nil

local UserInput = UserInputService

local function startFly()
    if flying then return end
    local char = player.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return end

    flying = true

    -- Criar BodyVelocity e BodyGyro para controlar o voo
    local root = char.HumanoidRootPart

    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.Velocity = Vector3.new(0,0,0)
    bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    bodyVelocity.Parent = root

    bodyGyro = Instance.new("BodyGyro")
    bodyGyro.D = 10
    bodyGyro.P = 10000
    bodyGyro.MaxTorque = Vector3.new(4e5, 4e5, 4e5)
    bodyGyro.Parent = root

    -- Efeito visual macabro (aura roxa)
    if not root:FindFirstChild("FlyAura") then
        local aura = Instance.new("ParticleEmitter")
        aura.Name = "FlyAura"
        aura.Color = ColorSequence.new(Color3.fromRGB(150, 0, 180))
        aura.LightEmission = 0.9
        aura.Rate = 40
        aura.Lifetime = NumberRange.new(0.3)
        aura.Speed = NumberRange.new(1)
        aura.Parent = root
    end
end

local function stopFly()
    if not flying then return end
    flying = false

    local char = player.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return end

    if bodyVelocity then bodyVelocity:Destroy() end
    if bodyGyro then bodyGyro:Destroy() end

    -- Remover aura
    if char.HumanoidRootPart:FindFirstChild("FlyAura") then
        char.HumanoidRootPart.FlyAura.Enabled = false
        wait(0.5)
        char.HumanoidRootPart.FlyAura:Destroy()
    end
end

-- Controle do voo
local moveVector = Vector3.new(0,0,0)

UserInput.InputBegan:Connect(function(input, gameProcessed)
    if flying then
        if input.KeyCode == Enum.KeyCode.W then moveVector = moveVector + Vector3.new(0,0,-1) end
        if input.KeyCode == Enum.KeyCode.S then moveVector = moveVector + Vector3.new(0,0,1) end
        if input.KeyCode == Enum.KeyCode.A then moveVector = moveVector + Vector3.new(-1,0,0) end
        if input.KeyCode == Enum.KeyCode.D then moveVector = moveVector + Vector3.new(1,0,0) end
        if input.KeyCode == Enum.KeyCode.Space then moveVector = moveVector + Vector3.new(0,1,0) end
        if input.KeyCode == Enum.KeyCode.LeftControl then moveVector = moveVector + Vector3.new(0,-1,0) end
    end
end)

UserInput.InputEnded:Connect(function(input, gameProcessed)
    if flying then
        if input.KeyCode == Enum.KeyCode.W then moveVector = moveVector - Vector3.new(0,0,-1) end
        if input.KeyCode == Enum.KeyCode.S then moveVector = moveVector - Vector3.new(0,0,1) end
        if input.KeyCode == Enum.KeyCode.A then moveVector = moveVector - Vector3.new(-1,0,0) end
        if input.KeyCode == Enum.KeyCode.D then moveVector = moveVector - Vector3.new(1,0,0) end
        if input.KeyCode == Enum.KeyCode.Space then moveVector = moveVector - Vector3.new(0,1,0) end

--// CONTROLE COMPLETO DO VOO
local flySpeed = 3
local isFlying = false
local velocity = Instance.new("BodyVelocity")
velocity.MaxForce = Vector3.new(1e9, 1e9, 1e9)
velocity.Name = "Th1xFlight"

function enableFlight()
	isFlying = true
	local char = getChar()
	local root = getRoot()
	velocity.Parent = root
	Starter.Sound:Play()

	game:GetService("RunService").RenderStepped:Connect(function()
		if isFlying then
			local moveDir = Vector3.new()
			if UIS:IsKeyDown(Enum.KeyCode.W) then moveDir += cam.CFrame.LookVector end
			if UIS:IsKeyDown(Enum.KeyCode.S) then moveDir -= cam.CFrame.LookVector end
			if UIS:IsKeyDown(Enum.KeyCode.A) then moveDir -= cam.CFrame.RightVector end
			if UIS:IsKeyDown(Enum.KeyCode.D) then moveDir += cam.CFrame.RightVector end
			if UIS:IsKeyDown(Enum.KeyCode.Space) then moveDir += Vector3.new(0, 1, 0) end
			if UIS:IsKeyDown(Enum.KeyCode.LeftShift) then moveDir -= Vector3.new(0, 1, 0) end
			velocity.Velocity = moveDir.Unit * flySpeed
		end
	end)
end

function disableFlight()
	isFlying = false
	velocity.Parent = nil
end

FlyBtn.MouseButton1Click:Connect(function()
	if isFlying then
		disableFlight()
	else
		enableFlight()
	end
end)

--// ABA: OUTFIT
local outfitText = createText("Outfit", TabBtn("Outfit"))
outfitText.TextColor3 = Color3.fromRGB(255, 105, 180)

local specialOutfit = createBtn("Roupa Demoníaca", outfitTab)
specialOutfit.MouseButton1Click:Connect(function()
	local hum = getChar():FindFirstChild("Humanoid")
	if hum then
		for _, v in ipairs(getChar():GetChildren()) do
			if v:IsA("Accessory") or v:IsA("Shirt") or v:IsA("Pants") then v:Destroy() end
		end
		local shirt = Instance.new("Shirt", getChar())
		shirt.ShirtTemplate = "rbxassetid://9421963654"
		local pants = Instance.new("Pants", getChar())
		pants.PantsTemplate = "rbxassetid://9421963030"
		local horn = game:GetService("InsertService"):LoadAsset(6773662752)
		horn.Parent = getChar()
		horn:FindFirstChildWhichIsA("Accessory").Parent = getChar()
	end
end)

--// ABA: RISK
local riskText = createText("Risk", TabBtn("Risk"))
riskText.TextColor3 = Color3.fromRGB(255, 0, 0)

local invisBtn = createBtn("Ficar Invisível", riskTab)
invisBtn.MouseButton1Click:Connect(function()
	local root = getRoot()
	if root then
		root.Transparency = 1
		for _, v in ipairs(getChar():GetDescendants()) do
			if v:IsA("BasePart") then v.Transparency = 1 end
		end
	end
end)

local moneyBtn = createBtn("Gerar Dinheiro 💸", riskTab)
moneyBtn.MouseButton1Click:Connect(function()
	local money = Instance.new("BillboardGui", getChar().Head)
	money.Size = UDim2.new(0, 200, 0, 50)
	money.StudsOffset = Vector3.new(0, 2, 0)
	money.AlwaysOnTop = true

	local label = Instance.new("TextLabel", money)
	label.Size = UDim2.new(1, 0, 1, 0)
	label.BackgroundTransparency = 1
	label.Text = "+$9999"
	label.Font = Enum.Font.GothamBlack
	label.TextSize = 24
	label.TextColor3 = Color3.fromRGB(0, 255, 0)

	game:GetService("Debris"):AddItem(money, 3)
end)

--// MINIMIZAR / RESTAURAR MENU
local isMinimized = false
local minimizeBtn = createBtn("Minimizar", homeTab)
minimizeBtn.Text = "-"
minimizeBtn.Size = UDim2.new(0, 20, 0, 20)
minimizeBtn.Position = UDim2.new(1, -30, 0, 5)

minimizeBtn.MouseButton1Click:Connect(function()
	if isMinimized then
		main.Position = UDim2.new(0.5, -200, 0.5, -150)
		main.Visible = true
		isMinimized = false
		local restoreSound = Instance.new("Sound", main)
		restoreSound.SoundId = "rbxassetid://9118823106"
		restoreSound.Volume = 0.6
		restoreSound:Play()
	else
		main.Visible = false
		isMinimized = true
	end
end)

--// ATALHO PARA ESCONDER / RESTAURAR MENU
UIS.InputBegan:Connect(function(input, gpe)
	if not gpe and input.KeyCode == Enum.KeyCode.M then
		main.Visible = not main.Visible
		local toggleSound = Instance.new("Sound", main)
		toggleSound.SoundId = "rbxassetid://12222216"
		toggleSound.Volume = 0.3
		toggleSound:Play()
	end
end)
