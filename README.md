--[[
Tr1X Menu Macabro - Script Completo
Versão: 1.0
--]]

-- Serviços
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local StarterGui = game:GetService("StarterGui")

-- Player local
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Remote Event
local spawnRemote = ReplicatedStorage:WaitForChild("Spawn")

-- Sons
local clickSound = Instance.new("Sound")
clickSound.SoundId = "rbxassetid://12222030"
clickSound.Volume = 1
clickSound.Parent = game.SoundService

-- Função de clique com som
local function playClickSound()
	clickSound:Play()
end

-- Anti-Invisibilidade após respawn
player.CharacterAdded:Connect(function(char)
	character = char
end)

-- GUI Principal
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "Tr1X_Menu"

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 400, 0, 300)
MainFrame.Position = UDim2.new(0.5, -200, 0.5, -150)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 0, 50)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

-- Estética Macabra: Stroke e Gradiente
local stroke = Instance.new("UIStroke")
stroke.Color = Color3.fromRGB(130, 0, 200)
stroke.Thickness = 1.5
stroke.Parent = MainFrame

local gradient = Instance.new("UIGradient")
gradient.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0, Color3.fromRGB(50, 0, 80)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(100, 0, 150))
})
gradient.Parent = MainFrame

-- Minimizar botão
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(1, -35, 0, 5)
minimizeBtn.Text = "-"
minimizeBtn.Font = Enum.Font.Antique
minimizeBtn.TextSize = 20
minimizeBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 100)
minimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeBtn.Parent = MainFrame

-- Minimizar lógica
local minimized = false
minimizeBtn.MouseButton1Click:Connect(function()
	playClickSound()
	minimized = not minimized
	for _, v in pairs(MainFrame:GetChildren()) do
		if v:IsA("TextButton") or v:IsA("Frame") then
			if v ~= minimizeBtn then
				v.Visible = not minimized
			end
		end
	end
end)

-- Título
local Title = Instance.new("TextLabel")
Title.Text = "Tr1X Macabro Menu"
Title.Size = UDim2.new(1, 0, 0, 40)
Title.Position = UDim2.new(0, 0, 0, 0)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.Antique
Title.TextScaled = true
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Parent = MainFrame

-- Abas
local Tabs = {
	"Home", "Outfit", "Risk"
}
local TabFrames = {}
local currentTab = ""

-- Criar botões de aba
for i, name in ipairs(Tabs) do
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(0, 120, 0, 30)
	btn.Position = UDim2.new(0, 10 + ((i - 1) * 130), 0, 50)
	btn.Text = name
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 14
	btn.BackgroundColor3 = Color3.fromRGB(60, 0, 80)
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.Parent = MainFrame

	local frame = Instance.new("Frame")
	frame.Size = UDim2.new(1, -20, 1, -90)
	frame.Position = UDim2.new(0, 10, 0, 90)
	frame.BackgroundColor3 = Color3.fromRGB(40, 0, 60)
	frame.Visible = false
	frame.Parent = MainFrame
	TabFrames[name] = frame

	btn.MouseButton1Click:Connect(function()
		playClickSound()
		for t, f in pairs(TabFrames) do
			f.Visible = false
		end
		frame.Visible = true
		currentTab = name
	end)
end

-- Função para criar botão dentro da aba
local function createFunctionButton(parent, text, callback)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(1, -20, 0, 30)
	btn.Position = UDim2.new(0, 10, 0, #parent:GetChildren() * 35)
	btn.Text = text
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 14
	btn.BackgroundColor3 = Color3.fromRGB(90, 0, 130)
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.Parent = parent
	btn.MouseButton1Click:Connect(function()
		playClickSound()
		callback()
	end)
end

-- Funções da aba Home
createFunctionButton(TabFrames["Home"], "Ativar ESP", function()
	for _, plr in pairs(Players:GetPlayers()) do
		if plr ~= player and plr.Character then
			local highlight = Instance.new("Highlight")
			highlight.FillColor = Color3.fromRGB(255, 0, 255)
			highlight.OutlineColor = Color3.fromRGB(0, 0, 0)
			highlight.Parent = plr.Character
		end
	end
end)

-- Funções da aba Outfit
createFunctionButton(TabFrames["Outfit"], "Aplicar Roupas", function()
	local shirt = Instance.new("Shirt", character)
	shirt.ShirtTemplate = "rbxassetid://14407676002"
	local pants = Instance.new("Pants", character)
	pants.PantsTemplate = "rbxassetid://14407676835"
end)

-- Funções da aba Risk
createFunctionButton(TabFrames["Risk"], "Autodestruição", function()
	character:BreakJoints()
end)

-- Voo com CTRL/WASD/SPACE
local flying = false
local UIS = game:GetService("UserInputService")

local function setupFlight()
	local bodyGyro = Instance.new("BodyGyro")
	bodyGyro.P = 9e4
	bodyGyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
	bodyGyro.cframe = character.HumanoidRootPart.CFrame
	bodyGyro.Parent = character.HumanoidRootPart

	local bodyVel = Instance.new("BodyVelocity")
	bodyVel.velocity = Vector3.new(0,0,0)
	bodyVel.maxForce = Vector3.new(9e9, 9e9, 9e9)
	bodyVel.Parent = character.HumanoidRootPart

	local flyDir = Vector3.new()

	UIS.InputBegan:Connect(function(input)
		if input.KeyCode == Enum.KeyCode.W then
			flyDir = flyDir + Vector3.new(0,0,-1)
		elseif input.KeyCode == Enum.KeyCode.S then
			flyDir = flyDir + Vector3.new(0,0,1)
		elseif input.KeyCode == Enum.KeyCode.A then
			flyDir = flyDir + Vector3.new(-1,0,0)
		elseif input.KeyCode == Enum.KeyCode.D then
			flyDir = flyDir + Vector3.new(1,0,0)
		elseif input.KeyCode == Enum.KeyCode.Space then
			flyDir = flyDir + Vector3.new(0,1,0)
		end
	end)

	UIS.InputEnded:Connect(function(input)
		if input.KeyCode == Enum.KeyCode.W then
			flyDir = flyDir - Vector3.new(0,0,-1)
		elseif input.KeyCode == Enum.KeyCode.S then
			flyDir = flyDir - Vector3.new(0,0,1)
		elseif input.KeyCode == Enum.KeyCode.A then
			flyDir = flyDir - Vector3.new(-1,0,0)
		elseif input.KeyCode == Enum.KeyCode.D then
			flyDir = flyDir - Vector3.new(1,0,0)
		elseif input.KeyCode == Enum.KeyCode.Space then
			flyDir = flyDir - Vector3.new(0,1,0)
		end
	end)

	while flying do
		bodyGyro.CFrame = workspace.CurrentCamera.CFrame
		bodyVel.velocity = workspace.CurrentCamera.CFrame:vectorToWorldSpace(flyDir * 50)
		task.wait()
	end
	bodyGyro:Destroy()
	bodyVel:Destroy()
end

UIS.InputBegan:Connect(function(input)
	if input.KeyCode == Enum.KeyCode.LeftControl then
		flying = not flying
		if flying then
			setupFlight()
		end
	end
end)

-- Inicializa aba Home
TabFrames["Home"].Visible = true


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
