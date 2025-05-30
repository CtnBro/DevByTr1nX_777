-- Tr1X Menu Macabro - Script Atualizado até o final
-- Interface sombria com funções como ESP, Voo, Invisibilidade, Teleporte, Roupas, Dinheiro, etc.

-- Serviços
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local TeleportService = game:GetService("TeleportService")
local Debris = game:GetService("Debris")

-- Player
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- GUI Principal
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "Tr1XMenu"

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 400, 0, 300)
MainFrame.Position = UDim2.new(0.5, -200, 0.5, -150)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MainFrame.Visible = true

-- Arrastar
local dragging, dragInput, dragStart, startPos

MainFrame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = MainFrame.Position
	end
end)

MainFrame.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement then
		dragInput = input
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		local delta = input.Position - dragStart
		MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

UserInputService.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = false
	end
end)

-- Tabs
local currentTab = "Home"

local function createTabButton(name, posX)
	local btn = Instance.new("TextButton", MainFrame)
	btn.Size = UDim2.new(0, 100, 0, 30)
	btn.Position = UDim2.new(0, posX, 0, 0)
	btn.Text = name
	btn.BackgroundColor3 = Color3.fromRGB(50, 0, 80)
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.BorderSizePixel = 0
	btn.MouseButton1Click:Connect(function()
		currentTab = name
		for _, child in ipairs(MainFrame:GetChildren()) do
			if child:IsA("Frame") and child.Name ~= "TabButtons" then
				child.Visible = false
			end
		end
		MainFrame:FindFirstChild(name).Visible = true
	end)
end

createTabButton("Home", 0)
createTabButton("Outfit", 100)
createTabButton("Risk", 200)

-- Gui de Tabs
local function createTabFrame(name)
	local frame = Instance.new("Frame", MainFrame)
	frame.Name = name
	frame.Size = UDim2.new(1, 0, 1, -30)
	frame.Position = UDim2.new(0, 0, 0, 30)
	frame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
	frame.BorderSizePixel = 0
	frame.Visible = name == "Home"
	return frame
end

local homeTab = createTabFrame("Home")
local outfitTab = createTabFrame("Outfit")
local riskTab = createTabFrame("Risk")

-- Efeito Sonoro
local function playClickSound()
	local sound = Instance.new("Sound", MainFrame)
	sound.SoundId = "rbxassetid://12222030"
	sound.Volume = 1
	sound:Play()
	Debris:AddItem(sound, 2)
end

-- Invisibilidade
local invisible = false
local function toggleInvisibility()
	if not character then return end
	invisible = not invisible
	for _, part in ipairs(character:GetDescendants()) do
		if part:IsA("BasePart") or part:IsA("Decal") then
			if invisible then
				part.Transparency = 1
			else
				part.Transparency = 0
			end
		end
	end
end

-- Voo
local flying = false
local flightConnection = nil
local speed = 5

local function startFlying()
	local bodyGyro = Instance.new("BodyGyro")
	bodyGyro.P = 9e4
	bodyGyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
	bodyGyro.cframe = humanoidRootPart.CFrame
	bodyGyro.Parent = humanoidRootPart

	local bodyVelocity = Instance.new("BodyVelocity")
	bodyVelocity.velocity = Vector3.new(0, 0, 0)
	bodyVelocity.maxForce = Vector3.new(9e9, 9e9, 9e9)
	bodyVelocity.Parent = humanoidRootPart

	flightConnection = RunService.RenderStepped:Connect(function()
		local direction = Vector3.new()
		if UserInputService:IsKeyDown(Enum.KeyCode.W) then direction += workspace.CurrentCamera.CFrame.LookVector end
		if UserInputService:IsKeyDown(Enum.KeyCode.S) then direction -= workspace.CurrentCamera.CFrame.LookVector end
		if UserInputService:IsKeyDown(Enum.KeyCode.A) then direction -= workspace.CurrentCamera.CFrame.RightVector end
		if UserInputService:IsKeyDown(Enum.KeyCode.D) then direction += workspace.CurrentCamera.CFrame.RightVector end
		if UserInputService:IsKeyDown(Enum.KeyCode.Space) then direction += workspace.CurrentCamera.CFrame.UpVector end

		bodyGyro.CFrame = workspace.CurrentCamera.CFrame
		bodyVelocity.velocity = direction * speed
	end)

	character.Humanoid.PlatformStand = true
end

local function stopFlying()
	if flightConnection then flightConnection:Disconnect() flightConnection = nil end
	if humanoidRootPart:FindFirstChild("BodyGyro") then humanoidRootPart.BodyGyro:Destroy() end
	if humanoidRootPart:FindFirstChild("BodyVelocity") then humanoidRootPart.BodyVelocity:Destroy() end
	character.Humanoid.PlatformStand = false
end

UserInputService.InputBegan:Connect(function(input, processed)
	if processed then return end
	if input.KeyCode == Enum.KeyCode.LeftControl then
		flying = not flying
		if flying then
			startFlying()
		else
			stopFlying()
		end
	end
end)

-- Botão Invisibility
local invisBtn = Instance.new("TextButton", homeTab)

invisBtn.Size = UDim2.new(0, 120, 0, 30)
invisBtn.Position = UDim2.new(0, 10, 0, 10)
invisBtn.Text = "Invisibilidade"
invisBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 80)
invisBtn.TextColor3 = Color3.new(1, 1, 1)
invisBtn.BorderSizePixel = 0
invisBtn.MouseButton1Click:Connect(function()
	playClickSound()
	toggleInvisibility()
end)

-- Botão Voo
local flyBtn = Instance.new("TextButton", homeTab)
flyBtn.Size = UDim2.new(0, 120, 0, 30)
flyBtn.Position = UDim2.new(0, 10, 0, 50)
flyBtn.Text = "Ativar Voo"
flyBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 80)
flyBtn.TextColor3 = Color3.new(1, 1, 1)
flyBtn.BorderSizePixel = 0
flyBtn.MouseButton1Click:Connect(function()
	playClickSound()
	flying = not flying
	if flying then
		startFlying()
		flyBtn.Text = "Desativar Voo"
	else
		stopFlying()
		flyBtn.Text = "Ativar Voo"
	end
end)

-- ESP (visualizar jogadores através das paredes)
local function applyESP(target)
	if target:FindFirstChild("Head") then
		local adorn = Instance.new("BoxHandleAdornment")
		adorn.Adornee = target:FindFirstChild("Head")
		adorn.AlwaysOnTop = true
		adorn.ZIndex = 5
		adorn.Size = Vector3.new(2, 2, 1)
		adorn.Color3 = Color3.fromRGB(255, 0, 255)
		adorn.Transparency = 0.3
		adorn.Parent = target:FindFirstChild("Head")
	end
end

local function toggleESP()
	for _, p in ipairs(Players:GetPlayers()) do
		if p ~= player and p.Character then
			applyESP(p.Character)
		end
	end
end

local espBtn = Instance.new("TextButton", homeTab)
espBtn.Size = UDim2.new(0, 120, 0, 30)
espBtn.Position = UDim2.new(0, 10, 0, 90)
espBtn.Text = "Ativar ESP"
espBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 80)
espBtn.TextColor3 = Color3.new(1, 1, 1)
espBtn.BorderSizePixel = 0
espBtn.MouseButton1Click:Connect(function()
	playClickSound()
	toggleESP()
end)

-- === FINALIZAÇÃO ===
-- Definir aba inicial visível
CurrentTab = HomeTab
CurrentTab.Visible = true
homeBtn.BackgroundColor3 = Color3.fromRGB(90, 0, 130)

-- Estilizar botões das abas para feedback visual
for _, tab in pairs(TabList) do
    tab.Button.MouseButton1Click:Connect(function()
        for _, t in pairs(TabList) do
            t.Button.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        end
        tab.Button.BackgroundColor3 = Color3.fromRGB(90, 0, 130)
    end)
end
-- Linha 412 em diante -- continuação do menu macabro roxo

-- === FUNÇÕES AUXILIARES ===
local function createSound(parent, soundId, volume)
    local s = Instance.new("Sound", parent)
    s.SoundId = soundId
    s.Volume = volume or 0.5
    s:Play()
    game.Debris:AddItem(s, 2)
    return s
end

-- === EVENTO DE FECHAR E MINIMIZAR ===
minimizeBtn.MouseButton1Click:Connect(function()
    if MainFrame.Visible then
        MainFrame.Visible = false
        -- Mostrar botão para restaurar
        restoreBtn.Visible = true
    end
end)

restoreBtn.MouseButton1Click:Connect(function()
    if not MainFrame.Visible then
        MainFrame.Visible = true
        restoreBtn.Visible = false
    end
end)

-- === CRÉDITOS ===
local creditsLabel = Instance.new("TextLabel", MainFrame)
creditsLabel.Text = "Tr1X Menu © 2025"
creditsLabel.Size = UDim2.new(0, 180, 0, 20)
creditsLabel.Position = UDim2.new(0, 10, 1, -30)
creditsLabel.BackgroundTransparency = 1
creditsLabel.TextColor3 = Color3.fromRGB(180, 0, 220)
creditsLabel.Font = Enum.Font.GothamItalic
creditsLabel.TextSize = 14
creditsLabel.TextXAlignment = Enum.TextXAlignment.Left

-- === REFORÇANDO A ESTÉTICA DO MENU ===
MainFrame.BackgroundColor3 = Color3.fromRGB(35, 0, 55)
MainFrame.BorderColor3 = Color3.fromRGB(90, 0, 130)

for _, btn in pairs({homeBtn, outfitBtn, riskBtn, minimizeBtn}) do
    btn.BackgroundColor3 = Color3.fromRGB(60, 0, 100)
    btn.TextColor3 = Color3.fromRGB(220, 170, 255)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.AutoButtonColor = false
    btn.MouseEnter:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(100, 0, 160)
        createSound(btn, "rbxassetid://9118823109", 0.3)
    end)
    btn.MouseLeave:Connect(function()
        btn.BackgroundColor3 = Color3.fromRGB(60, 0, 100)
    end)
end

-- === OUTRO AJUSTE: FEEDBACK DOS BOTÕES DAS ABAS ===
local function selectTab(selectedButton)
    for _, tab in pairs(TabList) do
        if tab.Button == selectedButton then
            tab.Frame.Visible = true
            tab.Button.BackgroundColor3 = Color3.fromRGB(100, 0, 160)
        else
            tab.Frame.Visible = false
            tab.Button.BackgroundColor3 = Color3.fromRGB(60, 0, 100)
        end
    end
end

homeBtn.MouseButton1Click:Connect(function() selectTab(homeBtn) end)
outfitBtn.MouseButton1Click:Connect(function() selectTab(outfitBtn) end)
riskBtn.MouseButton1Click:Connect(function() selectTab(riskBtn) end)

-- === AJUSTAR TAMANHO E POSIÇÃO DO RESTAURAR ===
restoreBtn.Position = UDim2.new(0, 10, 0, 10)
restoreBtn.Size = UDim2.new(0, 80, 0, 30)
restoreBtn.BackgroundColor3 = Color3.fromRGB(60, 0, 100)
restoreBtn.TextColor3 = Color3.fromRGB(220, 170, 255)
restoreBtn.Font = Enum.Font.GothamBold
restoreBtn.TextSize = 14
restoreBtn.Visible = false

restoreBtn.MouseEnter:Connect(function()
    restoreBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 160)
    createSound(restoreBtn, "rbxassetid://9118823109", 0.3)
end)

restoreBtn.MouseLeave:Connect(function()
    restoreBtn.BackgroundColor3 = Color3.fromRGB(60, 0, 100)
end)

-- === LIMPAR INPUTS AO ABRIR ABA RISK ===
riskBtn.MouseButton1Click:Connect(function()
    moneyBox.Text = ""
end)

-- === FEEDBACK SONORO AO TROCAR DE ABA ===
for _, tab in pairs(TabList) do
    tab.Button.MouseButton1Click:Connect(function()
        createSound(tab.Button, "rbxassetid://9118823109", 0.4)
    end)
end

-- === FINALIZAÇÃO - GARANTIR ABA HOME ATIVA AO INICIAR ===
selectTab(homeBtn)

-- === LIMPEZA AO SAIR DO JOGO ===
player.AncestryChanged:Connect(function(_, parent)
    if not parent then
        if flying then
            flying = false
            destroyFlight()
            if flightConnection then
                flightConnection:Disconnect()
                flightConnection = nil
            end
        end
    end
end)
-- Linha 534 em diante -- continuação do menu Tr1X roxo/macabro

-- === CONTINUAÇÃO DA FUNCIONALIDADE DE VOAR LIMPO E CONTROLADO ===

local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local flying = false
local flightSpeed = 50
local flightVelocity = nil
local flightConnection = nil

local function enableFlight()
    if flying then return end
    flying = true

    local character = player.Character
    if not character then flying = false return end
    local hrp = character:FindFirstChild("HumanoidRootPart")
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if not hrp or not humanoid then flying = false return end

    humanoid.PlatformStand = true

    flightVelocity = Instance.new("BodyVelocity")
    flightVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    flightVelocity.Velocity = Vector3.new(0,0,0)
    flightVelocity.Parent = hrp

    local direction = Vector3.new(0,0,0)
    local moveVector = Vector3.new(0,0,0)

    flightConnection = RunService.Heartbeat:Connect(function()
        direction = Vector3.new(0,0,0)

        if UserInputService:IsKeyDown(Enum.KeyCode.W) then
            direction = direction + workspace.CurrentCamera.CFrame.LookVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then
            direction = direction - workspace.CurrentCamera.CFrame.LookVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then
            direction = direction - workspace.CurrentCamera.CFrame.RightVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then
            direction = direction + workspace.CurrentCamera.CFrame.RightVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
            direction = direction + Vector3.new(0,1,0)
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) or UserInputService:IsKeyDown(Enum.KeyCode.RightControl) then
            direction = direction - Vector3.new(0,1,0)
        end

        if direction.Magnitude > 0 then
            direction = direction.Unit * flightSpeed
        end

        flightVelocity.Velocity = direction
    end)

    createSound(MainFrame, "rbxassetid://9118823115", 0.5) -- som ativar voo
end

local function disableFlight()
    if not flying then return end
    flying = false

    local character = player.Character
    if not character then return end
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.PlatformStand = false
    end

    if flightVelocity then
        flightVelocity:Destroy()
        flightVelocity = nil
    end

    if flightConnection then
        flightConnection:Disconnect()
        flightConnection = nil
    end

    createSound(MainFrame, "rbxassetid://9118823116", 0.5) -- som desativar voo
end

flyToggle.MouseButton1Click:Connect(function()
    if flying then
        disableFlight()
        flyToggle.Text = "Fly OFF"
        flyToggle.BackgroundColor3 = Color3.fromRGB(90, 0, 140)
    else
        enableFlight()
        flyToggle.Text = "Fly ON"
        flyToggle.BackgroundColor3 = Color3.fromRGB(180, 0, 220)
    end
end)

flyToggle.BackgroundColor3 = Color3.fromRGB(90, 0, 140)
flyToggle.TextColor3 = Color3.fromRGB(220, 170, 255)
flyToggle.Font = Enum.Font.GothamBold
flyToggle.TextSize = 16

-- === ESP SIMPLES COM CONTORNOS E ATIVA/DESATIVA ===

local espEnabled = false
local espTags = {}

local function enableESP()
    espEnabled = true
    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = plr.Character.HumanoidRootPart
            if not espTags[plr] then
                local adorn = Instance.new("BoxHandleAdornment")
                adorn.Adornee = hrp
                adorn.Size = Vector3.new(2, 5, 1)
                adorn.AlwaysOnTop = true
                adorn.ZIndex = 10
                adorn.Transparency = 0.6
                adorn.Color3 = Color3.fromRGB(180, 0, 220)
                adorn.Parent = hrp
                espTags[plr] = adorn
            end
        end
    end
end

local function disableESP()
    espEnabled = false
    for _, adorn in pairs(espTags) do
        adorn:Destroy()
    end
    espTags = {}
end

espToggle.MouseButton1Click:Connect(function()
    if espEnabled then
        disableESP()
        espToggle.Text = "ESP OFF"
        espToggle.BackgroundColor3 = Color3.fromRGB(90, 0, 140)
    else
        enableESP()
        espToggle.Text = "ESP ON"
        espToggle.BackgroundColor3 = Color3.fromRGB(180, 0, 220)
    end
end)

espToggle.BackgroundColor3 = Color3.fromRGB(90, 0, 140)
espToggle.TextColor3 = Color3.fromRGB(220, 170, 255)
espToggle.Font = Enum.Font.GothamBold
espToggle.TextSize = 16

-- === INVISIBILIDADE FUNCIONAL ===

local invisEnabled = false
local originalTransparency = {}

local function enableInvisibility()
    invisEnabled = true
    local character = player.Character
    if character then
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") and part.Transparency < 1 then
                originalTransparency[part] = part.Transparency
                part.Transparency = 0.8
                if part:FindFirstChildOfClass("Decal") then
                    for _, decal in pairs(part:GetChildren()) do
                        if decal:IsA("Decal") then
                            decal.Transparency = 0.8
                        end
                    end
                end
            end
        end
    end
end

local function disableInvisibility()
    invisEnabled = false
    for part, trans in pairs(originalTransparency) do
        if part and part.Parent then
            part.Transparency = trans
            if part:FindFirstChildOfClass("Decal") then
                for _, decal in pairs(part:GetChildren()) do
                    if decal:IsA("Decal") then
                        decal.Transparency = trans
                    end
                end
            end
        end
    end
    originalTransparency = {}
end

invisToggle.MouseButton1Click:Connect(function()
    if invisEnabled then
        disableInvisibility()
        invisToggle.Text = "Invisível OFF"
        invisToggle.BackgroundColor3 = Color3.fromRGB(90, 0, 140)
    else
        enableInvisibility()
        invisToggle.Text = "Invisível ON"
        invisToggle.BackgroundColor3 = Color3.fromRGB(180, 0, 220)
    end
end)

invisToggle.BackgroundColor3 = Color3.fromRGB(90, 0, 140)
invisToggle.TextColor3 = Color3.fromRGB(220, 170, 255)
invisToggle.Font = Enum.Font.GothamBold
invisToggle.TextSize = 16

-- === TELEPORTAR PARA JOGADOR ===

tpBtn.MouseButton1Click:Connect(function()
    local targetName = tpBox.Text
    local targetPlayer = Players:FindFirstChild(targetName)
    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = targetPlayer.Character.HumanoidRootPart
        local character = player.Character
        if character and character:FindFirstChild("HumanoidRootPart") then
            character.HumanoidRootPart.CFrame = hrp.CFrame + Vector3.new(0, 5, 0)
            createSound(tpBtn, "rbxassetid://9118823109", 0.5)
        end
    else
        tpBox.Text = "Jogador não encontrado"
        createSound(tpBtn, "rbxassetid://9118823135", 0.6)
    end
end)

tpBtn.BackgroundColor3 = Color3.fromRGB(90, 0, 140)
tpBtn.TextColor3 = Color3.fromRGB(220, 170, 255)
tpBtn.Font = Enum.Font.GothamBold
tpBtn.TextSize = 16

tpBox.BackgroundColor3 = Color3.fromRGB(20, 0, 40)
tpBox.TextColor3 = Color3.fromRGB(220, 170, 255)
tpBox.PlaceholderColor3 = Color3.fromRGB(150, 100, 200)
tpBox.Font = Enum.Font.GothamBold
tpBox.TextSize = 16
tpBox.ClearTextOnFocus = false

-- === ADICIONAR DINHEIRO NA ABA RISK ===

moneyBtn.MouseButton1Click:Connect(function()
    local amount = tonumber(moneyBox.Text)
    if amount and amount > 0 then
        local leaderstats = player:FindFirstChild("leaderstats")
        if leaderstats and leaderstats:FindFirstChild("Money") then
            leaderstats.Money.Value = leaderstats.Money.Value + amount
            moneyBox.Text = ""
            createSound(moneyBtn, "rbxassetid://9118823109", 0.5)
        else
            moneyBox.Text = "No leaderstats/Money"
            createSound(moneyBtn, "rbxassetid://9118823135", 0.6)
        end
    else
        moneyBox.Text = "Valor inválido"
        createSound(moneyBtn, "rbxassetid://9118823135", 0.6)
    end
end)

moneyBtn.BackgroundColor3 = Color3.fromRGB(90, 0, 140)
moneyBtn.TextColor3 = Color3.fromRGB(220, 170, 255)
moneyBtn.Font = Enum.Font.GothamBold
moneyBtn.TextSize = 16

moneyBox.BackgroundColor3 = Color3.fromRGB(20, 0, 40)
moneyBox.TextColor3 = Color3.fromRGB(220, 170, 255)
moneyBox.PlaceholderColor3 = Color3.fromRGB(150, 100, 200)
moneyBox.Font = Enum.Font.GothamBold
moneyBox.TextSize = 16
moneyBox.ClearTextOnFocus = false

-- === Roupas personalizadas no Outfit Tab ===

carrotBtn.MouseButton1Click:Connect(function()
    local char = player.Character
    if not char then return end
    -- Remove previous carrot if exists
    if char:FindFirstChild("CarrotAccessory") then
        char.CarrotAccessory:Destroy()
    end
    local carrotAssetId = 163977769609584
    local carrotAccessory = Instance.new("Accessory")
    carrotAccessory.Name = "CarrotAccessory"

    local handle = Instance.new("Part")
    handle.Name = "Handle"
    handle.Size = Vector3.new(1,1,1)
    handle.Transparency = 0
    handle.CanCollide = false
    handle.Parent = carrotAccessory

    local mesh = Instance.new("SpecialMesh", handle)
    mesh.MeshId = "rbxassetid://1538901915"
    mesh.TextureId = "rbxassetid://1538901843"
    mesh.Scale = Vector3.new(1.5, 1.5, 1.5)

    carrotAccessory.Parent = char
    player.Character.Humanoid:AddAccessory(carrotAccessory)

    createSound(carrotBtn, "rbxassetid://9118823109", 0.5)
end)

carrotBtn.BackgroundColor3 = Color3.fromRGB(90, 0, 140)
carrotBtn.TextColor3 = Color3.fromRGB(220, 170, 255)
carrotBtn.Font = Enum.Font.GothamBold
carrotBtn.TextSize = 16

-- === Finaliza aqui ===

print("Tr1X Menu carregado com sucesso. Divirta-se! 👻")
