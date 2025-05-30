-- Th1x Dev Menu - Versão aprimorada by ChatGPT

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

-- Sons simples para ativar/desativar (beep)
local soundOn = Instance.new("Sound")
soundOn.SoundId = "rbxassetid://9118820353" -- som beep on
soundOn.Volume = 0.3
soundOn.Parent = PlayerGui

local soundOff = Instance.new("Sound")
soundOff.SoundId = "rbxassetid://9118820180" -- som beep off
soundOff.Volume = 0.3
soundOff.Parent = PlayerGui

-- Criar GUI principal
local gui = Instance.new("ScreenGui")
gui.Name = "Th1xDevMenu"
gui.ResetOnSpawn = false
gui.Parent = PlayerGui

-- Frame principal
local Frame = Instance.new("Frame", gui)
Frame.Size = UDim2.new(0, 420, 0, 350)
Frame.Position = UDim2.new(0.3, 0, 0.25, 0)
Frame.BackgroundColor3 = Color3.fromRGB(45, 0, 75) -- roxo escuro
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.Name = "MainFrame"

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0, 16)

-- Cabeçalho
local Header = Instance.new("TextLabel", Frame)
Header.Size = UDim2.new(1, 0, 0, 40)
Header.BackgroundColor3 = Color3.fromRGB(35, 0, 60)
Header.Text = "Th1x Dev Menu"
Header.TextColor3 = Color3.fromRGB(220, 220, 220)
Header.Font = Enum.Font.GothamBold
Header.TextSize = 22
Header.BorderSizePixel = 0
Header.Position = UDim2.new(0, 0, 0, 0)
local headerCorner = Instance.new("UICorner", Header)
headerCorner.CornerRadius = UDim.new(0, 16)

-- Minimizar botão
local MiniBtn = Instance.new("TextButton", Header)
MiniBtn.Text = "–"
MiniBtn.Size = UDim2.new(0, 40, 0, 40)
MiniBtn.Position = UDim2.new(1, -40, 0, 0)
MiniBtn.BackgroundColor3 = Color3.fromRGB(25, 0, 50)
MiniBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
MiniBtn.Font = Enum.Font.GothamBold
MiniBtn.TextSize = 24
MiniBtn.BorderSizePixel = 0
MiniBtn.Name = "MinimizeBtn"
local miniBtnCorner = Instance.new("UICorner", MiniBtn)
miniBtnCorner.CornerRadius = UDim.new(0, 16)

-- Botão para restaurar após minimizar
local RestoreBtn = Instance.new("TextButton", gui)
RestoreBtn.Text = "Th1x"
RestoreBtn.Size = UDim2.new(0, 100, 0, 40)
RestoreBtn.Position = UDim2.new(0, 20, 1, -60)
RestoreBtn.BackgroundColor3 = Color3.fromRGB(50, 0, 80)
RestoreBtn.TextColor3 = Color3.fromRGB(230, 230, 230)
RestoreBtn.Font = Enum.Font.GothamBold
RestoreBtn.TextSize = 22
RestoreBtn.Visible = false
RestoreBtn.BorderSizePixel = 0
local restoreCorner = Instance.new("UICorner", RestoreBtn)
restoreCorner.CornerRadius = UDim.new(0, 16)

MiniBtn.MouseButton1Click:Connect(function()
    Frame.Visible = false
    RestoreBtn.Visible = true
    soundOff:Play()
end)

RestoreBtn.MouseButton1Click:Connect(function()
    Frame.Visible = true
    RestoreBtn.Visible = false
    soundOn:Play()
end)

-- Frame lateral para botões das abas
local TabHolder = Instance.new("Frame", Frame)
TabHolder.Size = UDim2.new(0, 120, 0, 300)
TabHolder.Position = UDim2.new(0, 0, 0, 40)
TabHolder.BackgroundColor3 = Color3.fromRGB(35, 5, 55)
TabHolder.BorderSizePixel = 0
local tabHolderCorner = Instance.new("UICorner", TabHolder)
tabHolderCorner.CornerRadius = UDim.new(0, 12)

local Layout = Instance.new("UIListLayout", TabHolder)
Layout.SortOrder = Enum.SortOrder.LayoutOrder
Layout.Padding = UDim.new(0, 10)

-- Container das abas para trocar visibilidade
local TabContainer = Instance.new("Frame", Frame)
TabContainer.Size = UDim2.new(1, -120, 0, 300)
TabContainer.Position = UDim2.new(0, 120, 0, 40)
TabContainer.BackgroundColor3 = Color3.fromRGB(20, 15, 30)
TabContainer.BorderSizePixel = 0
local tabContainerCorner = Instance.new("UICorner", TabContainer)
tabContainerCorner.CornerRadius = UDim.new(0, 12)

local TabList = {}
local CurrentTab = nil

-- Função para criar abas e botões laterais
local function CreateTab(name)
    local btn = Instance.new("TextButton", TabHolder)
    btn.Text = name
    btn.Size = UDim2.new(1, 0, 0, 40)
    btn.BackgroundColor3 = Color3.fromRGB(50, 10, 80)
    btn.TextColor3 = Color3.fromRGB(220, 220, 220)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 18
    btn.AutoButtonColor = false
    btn.BorderSizePixel = 0
    btn.Name = name .. "Btn"

    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 10)

    local tabFrame = Instance.new("Frame", TabContainer)
    tabFrame.Size = UDim2.new(1, 0, 1, 0)
    tabFrame.Position = UDim2.new(0, 0, 0, 0)
    tabFrame.BackgroundTransparency = 1
    tabFrame.Visible = false
    tabFrame.Name = name .. "Tab"

    btn.MouseButton1Click:Connect(function()
        if CurrentTab then
            CurrentTab.Visible = false
        end
        tabFrame.Visible = true
        CurrentTab = tabFrame

        -- Anima botão selecionado
        for _, v in pairs(TabHolder:GetChildren()) do
            if v:IsA("TextButton") then
                v.BackgroundColor3 = Color3.fromRGB(50, 10, 80)
            end
        end
        btn.BackgroundColor3 = Color3.fromRGB(100, 30, 150)
        soundOn:Play()
    end)

    table.insert(TabList, {Button = btn, Frame = tabFrame})
    return tabFrame, btn
end

-- Criar as abas
local HomeTab, homeBtn = CreateTab("Home")
local OutfitTab, outfitBtn = CreateTab("Outfit")
local RiskTab, riskBtn = CreateTab("Risk")

-- Ativar a Home por padrão
homeBtn.BackgroundColor3 = Color3.fromRGB(100, 30, 150)
HomeTab.Visible = true
CurrentTab = HomeTab

-- === HOME TAB CONTENT ===

-- Função helper para criar botões estilizados
local function CreateButton(parent, text, posY)
    local btn = Instance.new("TextButton", parent)
    btn.Text = text
    btn.Size = UDim2.new(0, 260, 0, 40)
    btn.Position = UDim2.new(0, 20, 0, posY)
    btn.BackgroundColor3 = Color3.fromRGB(65, 15, 110)
    btn.TextColor3 = Color3.fromRGB(230, 230, 230)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 18
    btn.BorderSizePixel = 0
    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 14)
    return btn
end

-- Função helper para criar TextBox estilizado
local function CreateTextBox(parent, placeholder, posY)
    local box = Instance.new("TextBox", parent)
    box.PlaceholderText = placeholder
    box.Size = UDim2.new(0, 260, 0, 35)
    box.Position = UDim2.new(0, 20, 0, posY)
    box.BackgroundColor3 = Color3.fromRGB(50, 10, 90)
    box.TextColor3 = Color3.fromRGB(220, 220, 220)
    box.Font = Enum.Font.Gotham
    box.TextSize = 16
    box.ClearTextOnFocus = false
    box.BorderSizePixel = 0
    local corner = Instance.new("UICorner", box)
    corner.CornerRadius = UDim.new(0, 12)
    return box
end

-- Variables to track toggles
local espEnabled = false
local flyEnabled = false
local invisEnabled = false

-- ESP system basic implementation (highlight players)
local Highlights = {}

local function ToggleESP(state)
    espEnabled = state
    if espEnabled then
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                if not Highlights[plr.Name] then
                    local hl = Instance.new("Highlight")
                    hl.Name = "ESPHighlight"
                    hl.Adornee = plr.Character
                    hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                    hl.FillColor = Color3.fromRGB(180, 0, 180) -- roxo forte
                    hl.OutlineColor = Color3.fromRGB(255, 255, 255)
                    hl.Parent = player.PlayerGui
                    Highlights[plr.Name] = hl
                end
            end
        end
    else
        for _, hl in pairs(Highlights) do
            hl:Destroy()
        end
        Highlights = {}
    end
end

-- Botão ESP
local espBtn = CreateButton(HomeTab, "ESP ON/OFF", 20)
espBtn.BackgroundColor3 = Color3.fromRGB(80, 10, 130)
espBtn.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    ToggleESP(espEnabled)
    if espEnabled then
        espBtn.BackgroundColor3 = Color3.fromRGB(140, 40, 180)
        soundOn:Play()
    else
        espBtn.BackgroundColor3 = Color3.fromRGB(80, 10, 130)
        soundOff:Play()
    end
end)

-- === FLY SYSTEM ===
local flying = false
local flySpeed = 50
local flyDirection = Vector3.new(0, 0, 0)
local bodyVelocity = nil
local bodyGyro = nil

local function startFly()
    local character = player.Character
    if not character or not character:FindFirstChild("HumanoidRootPart") or not character:FindFirstChild("Humanoid") then return end

    local hrp = character.HumanoidRootPart

    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    bodyVelocity.Velocity = Vector3.new(0,0,0)
    bodyVelocity.Parent = hrp

    bodyGyro = Instance.new("BodyGyro")
    bodyGyro.MaxTorque = Vector3.new(4e5, 4e5, 4e5)
    bodyGyro.P = 1e4
    bodyGyro.CFrame = hrp.CFrame
    bodyGyro.Parent = hrp

    flying = true
end

local function stopFly()
    if bodyVelocity then bodyVelocity:Destroy() end
    if bodyGyro then bodyGyro:Destroy() end
    flying = false
end

-- Atualização do voo com controles WASD + Space + Ctrl
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if not flying then return end

    if input.KeyCode == Enum.KeyCode.W then
        flyDirection = flyDirection + Vector3.new(0, 0, -1)
    elseif input.KeyCode == Enum.KeyCode.S then
        flyDirection = flyDirection + Vector3.new(0, 0, 1)
    elseif input.KeyCode == Enum.KeyCode.A then
        flyDirection = flyDirection + Vector3.new(-1, 0, 0)
    elseif input.KeyCode == Enum.KeyCode.D then
        flyDirection = flyDirection + Vector3.new(1, 0, 0)
    elseif input.KeyCode == Enum.KeyCode.Space then
        flyDirection = flyDirection + Vector3.new(0, 1, 0)
    elseif input.KeyCode == Enum.KeyCode.LeftControl then
        flyDirection = flyDirection + Vector3.new(0, -1, 0)
    end
end)

UserInputService.InputEnded:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if not flying then return end

    if input.KeyCode == Enum.KeyCode.W then
        flyDirection = flyDirection - Vector3.new(0, 0, -1)
    elseif input.KeyCode == Enum.KeyCode.S then
        flyDirection = flyDirection - Vector3.new(0, 0, 1)
    elseif input.KeyCode == Enum.KeyCode.A then
        flyDirection = flyDirection - Vector3.new(-1, 0, 0)
    elseif input.KeyCode == Enum.KeyCode.D then
        flyDirection = flyDirection - Vector3.new(1, 0, 0)
    elseif input.KeyCode == Enum.KeyCode.Space then
        flyDirection = flyDirection - Vector3.new(0, 1, 0)
    elseif input.KeyCode == Enum.KeyCode.LeftControl then
        flyDirection = flyDirection - Vector3.new(0, -1, 0)
    end
end)

RunService.Heartbeat:Connect(function()
    if flying and bodyVelocity and bodyGyro then
        local character = player.Character
        if character and character:FindFirstChild("HumanoidRootPart") then
            local hrp = character.HumanoidRootPart
            local camCFrame = workspace.CurrentCamera.CFrame
            local moveVector = flyDirection.Unit
            if moveVector.Magnitude == 0 then
                bodyVelocity.Velocity = Vector3.new(0, 0, 0)
            else
                bodyVelocity.Velocity = camCFrame:VectorToWorldSpace(moveVector * flySpeed)
            end
            bodyGyro.CFrame = camCFrame
        end
    end
end)

-- Botão Fly
local flyBtn = CreateButton(HomeTab, "Fly ON/OFF (WASD + Space + Ctrl)", 75)
flyBtn.BackgroundColor3 = Color3.fromRGB(80, 10, 130)
flyBtn.MouseButton1Click:Connect(function()
    flyEnabled = not flyEnabled
    if flyEnabled then
        startFly()
        flyBtn.BackgroundColor3 = Color3.fromRGB(140, 40, 180)
        soundOn:Play()
    else
        stopFly()
        flyBtn.BackgroundColor3 = Color3.fromRGB(80, 10, 130)
        soundOff:Play()
    end
end)

-- === INVISIBILITY SYSTEM ===

local function ToggleInvisibility(state)
    invisEnabled = state
    local character = player.Character
    if not character then return end
    for _, part in pairs(character:GetChildren()) do
        if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
            if invisEnabled then
                part.Transparency = 0.7
                if part:FindFirstChild("Decal") then
                    part.Decal.Transparency = 0.7
                end
            else
                part.Transparency = 0
                if part:FindFirstChild("Decal") then
                    part.Decal.Transparency = 0
                end
            end
        end
    end
    -- Humanoid transparency (head/face)
    local head = character:FindFirstChild("Head")
    if head then
        if invisEnabled then
            for _, decal in pairs(head:GetChildren()) do
                if decal:IsA("Decal") then
                    decal.Transparency = 0.7
                end
            end
        else
            for _, decal in pairs(head:GetChildren()) do
                if decal:IsA("Decal") then
                    decal.Transparency = 0
                end
            end
        end
    end
end

local invisBtn = CreateButton(HomeTab, "Invisível ON/OFF", 130)
invisBtn.BackgroundColor3 = Color3.fromRGB(80, 10, 130)
invisBtn.MouseButton1Click:Connect(function()
    invisEnabled = not invisEnabled
    ToggleInvisibility(invisEnabled)
    if invisEnabled then
        invisBtn.BackgroundColor3 = Color3.fromRGB(140, 40, 180)
        soundOn:Play()
    else
        invisBtn.BackgroundColor3 = Color3.fromRGB(80, 10, 130)
        soundOff:Play()
    end
end)

-- === TELEPORT SYSTEM ===

local tpBox = CreateTextBox(HomeTab, "Nome do Jogador para Teleportar", 180)
local tpBtn = CreateButton(HomeTab, "Teleportar", 225)

tpBtn.MouseButton1Click:Connect(function()
    local targetName = tpBox.Text
    if targetName == "" then
        tpBtn.Text = "Digite um nome válido"
        task.delay(2, function() tpBtn.Text = "Teleportar" end)
        return
    end
    local targetPlayer = Players:FindFirstChild(targetName)
    if not targetPlayer or not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        tpBtn.Text = "Jogador não encontrado"
        task.delay(2, function() tpBtn.Text = "Teleportar" end)
        return
    end
    local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if hrp then
        hrp.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
        soundTeleport:Play()
    end
end)

-- === OUTFIT TAB CONTENT ===

local OutfitLabel = Instance.new("TextLabel", OutfitTab)
OutfitLabel.Text = "Escolha sua roupa especial"
OutfitLabel.Size = UDim2.new(1, 0, 0, 40)
OutfitLabel.Position = UDim2.new(0, 0, 0, 10)
OutfitLabel.TextColor3 = Color3.fromRGB(230, 230, 230)
OutfitLabel.BackgroundTransparency = 1
OutfitLabel.Font = Enum.Font.GothamBold
OutfitLabel.TextSize = 22

local outfits = {
    {Name = "Roupa Sombria", ShirtId = "1234567", PantsId = "7654321"},
    {Name = "Roupa Macabra", ShirtId = "2345678", PantsId = "8765432"},
    {Name = "Roupa Futurista", ShirtId = "3456789", PantsId = "9876543"},
}

local yPos = 60
for _, outfit in pairs(outfits) do
    local btn = CreateButton(OutfitTab, outfit.Name, yPos)
    btn.MouseButton1Click:Connect(function()
        local character = player.Character
        if character then
            local shirt = character:FindFirstChildOfClass("Shirt") or Instance.new("Shirt", character)
            shirt.ShirtTemplate = "rbxassetid://" .. outfit.ShirtId
            local pants = character:FindFirstChildOfClass("Pants") or Instance.new("Pants", character)
            pants.PantsTemplate = "rbxassetid://" .. outfit.PantsId
            soundOn:Play()
        end
    end)
    yPos = yPos + 50
end

-- === RISK TAB CONTENT ===

local moneyAmountBox = CreateTextBox(RiskTab, "Digite a quantia de dinheiro para adicionar", 20)
local addMoneyBtn = CreateButton(RiskTab, "Adicionar Dinheiro", 70)

addMoneyBtn.MouseButton1Click:Connect(function()
    local amount = tonumber(moneyAmountBox.Text)
    if not amount or amount <= 0 then
        addMoneyBtn.Text = "Digite um valor válido"
        task.delay(2, function() addMoneyBtn.Text = "Adicionar Dinheiro" end)
        return
    end
    -- Exemplo: adicionar dinheiro - adaptável conforme seu sistema
    local leaderstats = player:FindFirstChild("leaderstats")
    if leaderstats and leaderstats:FindFirstChild("Money") then
        leaderstats.Money.Value = leaderstats.Money.Value + amount
        addMoneyBtn.Text = "Dinheiro adicionado!"
        soundOn:Play()
        task.delay(2, function() addMoneyBtn.Text = "Adicionar Dinheiro" end)
    else
        addMoneyBtn.Text = "Erro ao encontrar leaderstats"
        soundOff:Play()
        task.delay(2, function() addMoneyBtn.Text = "Adicionar Dinheiro" end)
    end
end)

-- === MINIMIZE / RESTORE BUTTON ===

local minimizeBtn = Instance.new("TextButton", MainFrame)
minimizeBtn.Text = "_"
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 28
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(1, -35, 0, 5)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(50, 0, 80)
minimizeBtn.TextColor3 = Color3.fromRGB(230, 230, 230)
minimizeBtn.BorderSizePixel = 0
local corner = Instance.new("UICorner", minimizeBtn)
corner.CornerRadius = UDim.new(0, 6)

local minimized = false

minimizeBtn.MouseButton1Click:Connect(function()
    if minimized then
        MainFrame.Size = UDim2.new(0, 320, 0, 420)
        for _, child in pairs(MainFrame:GetChildren()) do
            if child ~= minimizeBtn then
                child.Visible = true
            end
        end
        minimized = false
        minimizeBtn.Text = "_"
    else
        for _, child in pairs(MainFrame:GetChildren()) do
            if child ~= minimizeBtn then
                child.Visible = false
            end
        end
        MainFrame.Size = UDim2.new(0, 60, 0, 40)
        minimized = true
        minimizeBtn.Text = "◣"
    end
end)

-- === Inicializar aba HOME ao abrir ===
CurrentTab = HomeTab
HomeTab.Visible = true
OutfitTab.Visible = false
RiskTab.Visible = false

-- Sombras e efeitos macabros podem ser adicionados via sombras das UIs, gradientes etc, mas por simplicidade, mantive o básico.

-- Variáveis de controle
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local flySpeed = 50
local flying = false
local flyVelocity = Vector3.new(0, 0, 0)

-- Função pra ativar/desativar ESP (exemplo simples)
local function ToggleESP(state)
    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = plr.Character.HumanoidRootPart
            local box = hrp:FindFirstChild("ESPBox")
            if state then
                if not box then
                    box = Instance.new("BoxHandleAdornment")
                    box.Name = "ESPBox"
                    box.Adornee = hrp
                    box.AlwaysOnTop = true
                    box.ZIndex = 10
                    box.Size = Vector3.new(4, 6, 2)
                    box.Color3 = Color3.fromRGB(160, 60, 180)
                    box.Transparency = 0.5
                    box.Parent = hrp
                end
            else
                if box then
                    box:Destroy()
                end
            end
        end
    end
end

-- Função para toggle invisibilidade
local function ToggleInvisibility(state)
    local character = player.Character
    if character then
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                part.Transparency = state and 0.8 or 0
                if part:FindFirstChildOfClass("Decal") then
                    for _, decal in pairs(part:GetChildren()) do
                        if decal:IsA("Decal") then
                            decal.Transparency = state and 1 or 0
                        end
                    end
                end
            elseif part:IsA("Accessory") then
                if part.Handle then
                    part.Handle.Transparency = state and 0.8 or 0
                end
            end
        end
    end
end

-- Controle de voo: ativar/desativar fly
local function FlyToggle(state)
    flying = state
    local character = player.Character
    if flying and character and character:FindFirstChild("HumanoidRootPart") and character:FindFirstChild("Humanoid") then
        local hrp = character.HumanoidRootPart
        local humanoid = character.Humanoid

        -- Desativar gravidade para humanoid
        humanoid.PlatformStand = true

        -- Criar BodyVelocity para movimento
        local bodyVel = hrp:FindFirstChild("FlyVelocity")
        if not bodyVel then
            bodyVel = Instance.new("BodyVelocity")
            bodyVel.Name = "FlyVelocity"
            bodyVel.MaxForce = Vector3.new(1e5, 1e5, 1e5)
            bodyVel.Velocity = Vector3.new(0, 0, 0)
            bodyVel.Parent = hrp
        end

        -- Controlador de direção
        local moveDir = Vector3.new()
        local speed = flySpeed

        -- Input tracking
        local forward = false
        local backward = false
        local left = false
        local right = false
        local up = false
        local down = false

        -- Conexões para Input
        local inputBeganConn, inputEndedConn

        inputBeganConn = UserInputService.InputBegan:Connect(function(input, gameProcessed)
            if gameProcessed then return end
            if input.KeyCode == Enum.KeyCode.W then forward = true end
            if input.KeyCode == Enum.KeyCode.S then backward = true end
            if input.KeyCode == Enum.KeyCode.A then left = true end
            if input.KeyCode == Enum.KeyCode.D then right = true end
            if input.KeyCode == Enum.KeyCode.Space then up = true end
            if input.KeyCode == Enum.KeyCode.LeftControl then down = true end
        end)

        inputEndedConn = UserInputService.InputEnded:Connect(function(input, gameProcessed)
            if gameProcessed then return end
            if input.KeyCode == Enum.KeyCode.W then forward = false end
            if input.KeyCode == Enum.KeyCode.S then backward = false end
            if input.KeyCode == Enum.KeyCode.A then left = false end
            if input.KeyCode == Enum.KeyCode.D then right = false end
            if input.KeyCode == Enum.KeyCode.Space then up = false end
            if input.KeyCode == Enum.KeyCode.LeftControl then down = false end
        end)

        -- Atualizar movimento a cada frame
        local flyConnection
        flyConnection = RunService.Heartbeat:Connect(function()
            if not flying then
                flyConnection:Disconnect()
                if bodyVel then bodyVel:Destroy() end
                humanoid.PlatformStand = false
                inputBeganConn:Disconnect()
                inputEndedConn:Disconnect()
                return
            end

            local camCFrame = workspace.CurrentCamera.CFrame
            local moveVec = Vector3.new()

            if forward then moveVec = moveVec + camCFrame.LookVector end
            if backward then moveVec = moveVec - camCFrame.LookVector end
            if left then moveVec = moveVec - camCFrame.RightVector end
            if right then moveVec = moveVec + camCFrame.RightVector end
            if up then moveVec = moveVec + Vector3.new(0, 1, 0) end
            if down then moveVec = moveVec - Vector3.new(0, 1, 0) end

            if moveVec.Magnitude > 0 then
                moveVec = moveVec.Unit * speed
            end

            bodyVel.Velocity = moveVec
        end)

    elseif not flying then
        local character = player.Character
        if character and character:FindFirstChild("Humanoid") then
            character.Humanoid.PlatformStand = false
            local bodyVel = character.HumanoidRootPart:FindFirstChild("FlyVelocity")
            if bodyVel then bodyVel:Destroy() end
        end
    end
end

-- Configurar toggles com feedback visual e sonoro

SetupToggleButton(espBtn, "espEnabled", function(state)
    ToggleESP(state)
end)

SetupToggleButton(flyBtn, "flyEnabled", function(state)
    FlyToggle(state)
end)

SetupToggleButton(invisBtn, "invisEnabled", function(state)
    ToggleInvisibility(state)
end)

-- Teleporte com som e feedback
tpBtn.MouseButton1Click:Connect(function()
    AnimateButton(tpBtn)
    local targetName = tpBox.Text
    if targetName == "" then
        tpBtn.Text = "Digite um nome válido"
        task.delay(2, function() tpBtn.Text = "Teleportar" end)
        return
    end
    local targetPlayer = Players:FindFirstChild(targetName)
    if not targetPlayer or not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        tpBtn.Text = "Jogador não encontrado"
        soundOff:Play()
        task.delay(2, function() tpBtn.Text = "Teleportar" end)
        return
    end
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        player.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
        soundTeleport:Play()
    end
end)
