-- GUI Th1x Menu Script by Tr1nX_777 | Versão Expandida Sombria

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

-- Sons usados para feedback
local function createSound(parent, soundId)
    local sound = Instance.new("Sound")
    sound.SoundId = soundId
    sound.Volume = 0.5
    sound.Parent = parent
    return sound
end

-- Som de ativar/desativar
local soundOn = createSound(PlayerGui, "rbxassetid://9118822020")  -- som de clique positivo
local soundOff = createSound(PlayerGui, "rbxassetid://9118822050") -- som de clique negativo

-- Função para criar botão com borda e canto arredondado
local function createButton(parent, text, position)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 200, 0, 30)
    btn.Position = position
    btn.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    btn.TextColor3 = Color3.fromRGB(230, 230, 230)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.Text = text
    btn.AutoButtonColor = false
    btn.Parent = parent

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = btn

    local stroke = Instance.new("UIStroke")
    stroke.Color = Color3.fromRGB(255, 0, 0)
    stroke.Thickness = 1.5
    stroke.Parent = btn

    -- Efeito hover dark glow
    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(70, 0, 0)}):Play()
    end)
    btn.MouseLeave:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(35, 35, 35)}):Play()
    end)

    return btn
end

-- Criando GUI base
local gui = Instance.new("ScreenGui")
gui.Name = "Th1xDevMenu"
gui.ResetOnSpawn = false
gui.Parent = PlayerGui

local Frame = Instance.new("Frame", gui)
Frame.Size = UDim2.new(0, 420, 0, 360)
Frame.Position = UDim2.new(0.3, 0, 0.2, 0)
Frame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0, 16)

local TabHolder = Instance.new("Frame", Frame)
TabHolder.Size = UDim2.new(0, 120, 0, 320)
TabHolder.Position = UDim2.new(0, 0, 0, 40)
TabHolder.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
local cornerTab = Instance.new("UICorner", TabHolder)
cornerTab.CornerRadius = UDim.new(0, 14)

local Layout = Instance.new("UIListLayout", TabHolder)
Layout.SortOrder = Enum.SortOrder.LayoutOrder
Layout.Padding = UDim.new(0, 8)

-- Área conteúdo da aba
local ContentFrame = Instance.new("Frame", Frame)
ContentFrame.Size = UDim2.new(1, -130, 1, -40)
ContentFrame.Position = UDim2.new(0, 130, 0, 40)
ContentFrame.BackgroundTransparency = 1

local tabs = {}
local currentTab

local function createTab(name)
    local button = createButton(TabHolder, name, UDim2.new(0, 0, 0, 0))
    button.Size = UDim2.new(1, 0, 0, 40)
    button.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
    
    local tabFrame = Instance.new("Frame", ContentFrame)
    tabFrame.Size = UDim2.new(1, 0, 1, 0)
    tabFrame.BackgroundTransparency = 1
    tabFrame.Visible = false
    
    button.MouseButton1Click:Connect(function()
        if currentTab then currentTab.Visible = false end
        tabFrame.Visible = true
        currentTab = tabFrame
    end)

    table.insert(tabs, {button = button, frame = tabFrame})
    return tabFrame, button
end

-- Função para toggle com som e mudança visual
local function createToggleButton(parent, text, position)
    local btn = createButton(parent, text, position)
    local toggled = false

    local function updateVisual()
        if toggled then
            btn.BackgroundColor3 = Color3.fromRGB(180, 20, 20)
            btn.Text = text .. " [ON]"
            soundOn:Play()
        else
            btn.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
            btn.Text = text .. " [OFF]"
            soundOff:Play()
        end
    end

    btn.MouseButton1Click:Connect(function()
        toggled = not toggled
        updateVisual()
        -- Aqui você pode disparar eventos ou funções para ligar/desligar funcionalidade
    end)

    updateVisual()
    return btn, function() return toggled end
end

-- Criar abas
local homeTab, homeBtn = createTab("Home")
local outfitTab, outfitBtn = createTab("Outfit")
local riskTab, riskBtn = createTab("Risk")

-- Ajustar pai dos botões da aba
homeBtn.Parent = TabHolder
outfitBtn.Parent = TabHolder
riskBtn.Parent = TabHolder

-- -- -- -- -- HOME TAB -- -- -- -- --

-- Toggle ESP
local espToggle, isEspOn = createToggleButton(homeTab, "ESP", UDim2.new(0, 20, 0, 20))

-- Toggle Fly (com animação macabra)
local flyToggle, isFlyOn = createToggleButton(homeTab, "Fly", UDim2.new(0, 20, 0, 70))

-- Função para animação de voo sombria
local flying = false
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")

local flyTweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)

local flyTweenUp = TweenService:Create(rootPart, flyTweenInfo, {Position = rootPart.Position + Vector3.new(0, 5, 0)})
local flyTweenDown = TweenService:Create(rootPart, flyTweenInfo, {Position = rootPart.Position - Vector3.new(0, 5, 0)})

local flyingConnection

local function startFlying()
    flying = true

    -- Som do voo
    local flySound = createSound(rootPart, "rbxassetid://9118822200")
    flySound.Looped = true
    flySound.Volume = 0.3
    flySound:Play()

    flyingConnection = RunService.Heartbeat:Connect(function(step)
        local newPos = rootPart.Position + Vector3.new(0, math.sin(tick() * 5) * 0.5, 0)
        rootPart.CFrame = CFrame.new(newPos) * CFrame.Angles(0, tick(), 0)
    end)

    -- Visual macabro no personagem (exemplo: cor vermelha nos braços)
    for _, part in pairs(character:GetChildren()) do
        if part:IsA("BasePart") then
            part.Material = Enum.Material.Neon
            part.Color = Color3.fromRGB(180, 0, 0)
        end
    end
end

local function stopFlying()
    flying = false
    if flyingConnection then
        flyingConnection:Disconnect()
        flyingConnection = nil
    end
    -- Resetar material e cor para padrão
    for _, part in pairs(character:GetChildren()) do
        if part:IsA("BasePart") then
            part.Material = Enum.Material.SmoothPlastic
            part.Color = Color3.fromRGB(255, 255, 255)
        end
    end
end

flyToggle.MouseButton1Click:Connect(function()
    if flying then
        stopFlying()
        soundOff:Play()
    else
        startFlying()
        soundOn:Play()
    end
end)

-- Teleporte simples
local tpBox = Instance.new("TextBox", homeTab)
tpBox.PlaceholderText = "Nome do Jogador"
tpBox.Size = UDim2.new(0, 200, 0, 30)
tpBox.Position = UDim2.new(0, 20, 0, 120)
tpBox.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
tpBox.TextColor3 = Color3.new(1, 1, 1)
tpBox.ClearTextOnFocus = false
local cornerTp = Instance.new("UICorner", tpBox)
cornerTp.CornerRadius = UDim.new(0, 6)

local tpBtn = createButton(homeTab, "Teleportar", UDim2.new(0, 20, 0, 160))

tpBtn.MouseButton1Click:Connect(function()
    local targetName = tpBox.Text
    local targetPlayer = Players:FindFirstChild(targetName)
    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        rootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame + Vector3.new(0, 3, 0)
        soundOn:Play()
    else
        soundOff:Play()
    end
end)

-- -- -- -- -- OUTFIT TAB -- -- -- -- --

local carrotBtn = createButton(outfitTab, "GRG STORE - Cenoura", UDim2.new(0, 20, 0, 20))
local darkBtn = createButton(outfitTab, "tr1nx_777 - Roupa Macabra", UDim2.new(0, 20, 0, 70))

carrotBtn.MouseButton1Click:Connect(function()
    soundOn:Play()
    -- Aqui insira código para aplicar roupa cenoura
end)

darkBtn.MouseButton1Click:Connect(function()
    soundOn:Play()
    -- Aqui insira código para aplicar roupa macabra
end)

-- -- -- -- -- RISK TAB -- -- -- -- --

local desc = Instance.new("TextLabel", riskTab)
desc.Text = "Tr1nX_777 que domina"
desc.Size = UDim2.new(0, 260, 0, 30)
desc.Position = UDim2.new(0, 20, 0, 20)
desc.TextColor3 = Color3.fromRGB(255, 0, 0)
desc.Font = Enum.Font.GothamBold
desc.TextSize = 16
desc.BackgroundTransparency = 1

local moneyBtn = createButton(riskTab, "💸 Adicionar Dinheiro", UDim2.new(0, 20, 0, 70))
moneyBtn.MouseButton1Click:Connect(function()
    soundOn:Play()
    -- Código para adicionar dinheiro aqui
end)

-- -- -- -- -- BOTÃO MINIMIZAR -- -- -- -- --

local MiniBtn = createButton(Frame, "-", UDim2.new(1, -40, 0, 5))
MiniBtn.Size = UDim2.new(0, 30, 0, 30)
MiniBtn.TextSize = 24
MiniBtn.BackgroundColor3 = Color3.fromRGB(40, 0, 0)

local miniIcon = createButton(gui, "Th1x", UDim2.new(0, 20, 1, -60))
miniIcon.Visible = false

MiniBtn.MouseButton1Click:Connect(function()
    Frame.Visible = false
    miniIcon.Visible = true
    soundOn:Play()
end)

miniIcon.MouseButton1Click:Connect(function()
    Frame.Visible = true
    miniIcon.Visible = false
    soundOn:Play()
end)

-- Ativa a primeira aba automaticamente
homeBtn.MouseButton1Click:Invoke()

