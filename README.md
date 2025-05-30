-- Tr1X Menu Sombrio com Estilo Macabro
-- Criado por Tr1nX_777 - Versão revisada e melhorada

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

-- Criação da GUI principal
local gui = Instance.new("ScreenGui")
gui.Name = "Tr1xMacabroMenu"
gui.ResetOnSpawn = false
gui.Parent = PlayerGui

-- Main Frame
local Frame = Instance.new("Frame")
Frame.Name = "MainFrame"
Frame.Size = UDim2.new(0, 450, 0, 360)
Frame.Position = UDim2.new(0.3, 0, 0.3, 0)
Frame.BackgroundColor3 = Color3.fromRGB(40, 0, 70)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.Parent = gui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 12)
UICorner.Parent = Frame

-- TopBar
local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(1, 0, 0, 30)
TopBar.BackgroundColor3 = Color3.fromRGB(70, 0, 100)
TopBar.BorderSizePixel = 0
TopBar.Parent = Frame

local Title = Instance.new("TextLabel")
Title.Text = "Tr1X Menu Macabro"
Title.Font = Enum.Font.GothamBlack
Title.TextColor3 = Color3.fromRGB(230, 200, 255)
Title.TextSize = 16
Title.BackgroundTransparency = 1
Title.Size = UDim2.new(1, -40, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = TopBar

local Close = Instance.new("TextButton")
Close.Text = "-"
Close.Font = Enum.Font.GothamBold
Close.TextColor3 = Color3.fromRGB(230, 200, 255)
Close.TextSize = 20
Close.BackgroundColor3 = Color3.fromRGB(60, 0, 90)
Close.Size = UDim2.new(0, 30, 1, 0)
Close.Position = UDim2.new(1, -30, 0, 0)
Close.Parent = TopBar

-- MiniIcon para reabrir o menu
local MiniIcon = Instance.new("TextButton")
MiniIcon.Text = "Tr1X"
MiniIcon.Font = Enum.Font.GothamBold
MiniIcon.TextSize = 14
MiniIcon.TextColor3 = Color3.fromRGB(230, 200, 255)
MiniIcon.Size = UDim2.new(0, 100, 0, 30)
MiniIcon.Position = UDim2.new(0, 10, 1, -40)
MiniIcon.BackgroundColor3 = Color3.fromRGB(30, 0, 50)
MiniIcon.Visible = false
MiniIcon.Parent = gui

Close.MouseButton1Click:Connect(function()
    Frame.Visible = false
    MiniIcon.Visible = true
end)

MiniIcon.MouseButton1Click:Connect(function()
    Frame.Visible = true
    MiniIcon.Visible = false
end)

-- Tab Holder
local TabHolder = Instance.new("Frame")
TabHolder.Size = UDim2.new(0, 110, 1, -30)
TabHolder.Position = UDim2.new(0, 0, 0, 30)
TabHolder.BackgroundColor3 = Color3.fromRGB(30, 0, 50)
TabHolder.BorderSizePixel = 0
TabHolder.Parent = Frame

local Layout = Instance.new("UIListLayout")
Layout.SortOrder = Enum.SortOrder.LayoutOrder
Layout.Padding = UDim.new(0, 6)
Layout.Parent = TabHolder

local TabList = {}
local CurrentTab

local function CreateTab(name)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 30)
    btn.Text = name
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 14
    btn.TextColor3 = Color3.fromRGB(230, 200, 255)
    btn.BackgroundColor3 = Color3.fromRGB(60, 0, 90)
    btn.AutoButtonColor = false

    local UICornerBtn = Instance.new("UICorner")
    UICornerBtn.CornerRadius = UDim.new(0, 6)
    UICornerBtn.Parent = btn

    local tabFrame = Instance.new("Frame")
    tabFrame.Size = UDim2.new(1, -110, 1, -30)
    tabFrame.Position = UDim2.new(0, 110, 0, 30)
    tabFrame.BackgroundColor3 = Color3.fromRGB(20, 0, 40)
    tabFrame.BorderSizePixel = 0
    tabFrame.Visible = false
    tabFrame.Parent = Frame

    btn.MouseButton1Click:Connect(function()
        for _, tab in ipairs(TabList) do
            tab.Frame.Visible = false
            tab.Button.BackgroundColor3 = Color3.fromRGB(60, 0, 90)
        end
        tabFrame.Visible = true
        btn.BackgroundColor3 = Color3.fromRGB(90, 0, 130)
        CurrentTab = tabFrame
    end)

    btn.Parent = TabHolder
    table.insert(TabList, {Button = btn, Frame = tabFrame})
    return tabFrame
end

-- Criar abas principais
local HomeTab = CreateTab("Home")
local OutfitTab = CreateTab("Outfit")
local RiskTab = CreateTab("Risk")

-- Inicializar aba visível
HomeTab.Visible = true
CurrentTab = HomeTab

for _, tab in ipairs(TabList) do
    if tab.Frame == CurrentTab then
        tab.Button.BackgroundColor3 = Color3.fromRGB(90, 0, 130)
    else
        tab.Button.BackgroundColor3 = Color3.fromRGB(60, 0, 90)
    end
end

-- Função para tocar som ao clicar nos botões
local function playClickSound(parent, soundId)
    local sound = Instance.new("Sound", parent)
    sound.SoundId = soundId
    sound.Volume = 0.5
    sound:Play()
    game.Debris:AddItem(sound, 2)
end

-- ===== Conteúdo Aba Home =====

-- ESP Toggle
local espToggle = Instance.new("TextButton")
espToggle.Text = "ESP ON/OFF"
espToggle.Size = UDim2.new(0, 200, 0, 30)
espToggle.Position = UDim2.new(0, 10, 0, 10)
espToggle.BackgroundColor3 = Color3.fromRGB(60, 0, 90)
espToggle.TextColor3 = Color3.new(1, 1, 1)
espToggle.Font = Enum.Font.GothamBold
espToggle.TextSize = 14
espToggle.Parent = HomeTab

local espEnabled = false
espToggle.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    playClickSound(espToggle, "rbxassetid://9118823109")
    print("ESP toggled:", espEnabled)
    -- TODO: colocar função ESP real aqui (se quiser)
end)

-- Fly Toggle
local flyToggle = Instance.new("TextButton")
flyToggle.Text = "Fly ON/OFF (CTRL)"
flyToggle.Size = UDim2.new(0, 200, 0, 30)
flyToggle.Position = UDim2.new(0, 10, 0, 50)
flyToggle.BackgroundColor3 = Color3.fromRGB(60, 0, 90)
flyToggle.TextColor3 = Color3.new(1, 1, 1)
flyToggle.Font = Enum.Font.GothamBold
flyToggle.TextSize = 14
flyToggle.Parent = HomeTab

local flying = false
local flightSpeed = 50
local bodyVelocity, bodyGyro
local flightConnection

local function createFlight()
    local character = player.Character
    if not character then return end
    local hrp = character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end

    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.Velocity = Vector3.new(0, 0, 0)
    bodyVelocity.MaxForce = Vector3.new(0, 0, 0)
    bodyVelocity.Parent = hrp

    bodyGyro = Instance.new("BodyGyro")
    bodyGyro.MaxTorque = Vector3.new(0, 0, 0)
    bodyGyro.Parent = hrp
end

local function destroyFlight()
    if bodyVelocity then
        bodyVelocity:Destroy()
        bodyVelocity = nil
    end
    if bodyGyro then
        bodyGyro:Destroy()
        bodyGyro = nil
    end
end

local moveVector = Vector3.new()

UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.LeftControl then
        flying = not flying
        if flying then
            local character = player.Character
            if not character then return end
            local hrp = character:FindFirstChild("HumanoidRootPart")
            if not hrp then return end

            createFlight()

            bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
            bodyGyro.MaxTorque = Vector3.new(4e5, 4e5, 4e5)

            flightConnection = RunService.RenderStepped:Connect(function()
                local camCFrame = workspace.CurrentCamera.CFrame
                local dir = Vector3.new(0, 0, 0)

                if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                    dir = dir + camCFrame.LookVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                    dir = dir - camCFrame.LookVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                    dir = dir - camCFrame.RightVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                    dir = dir + camCFrame.RightVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                    dir = dir + Vector3.new(0, 1, 0)
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
                    dir = dir - Vector3.new(0, 1, 0)
                end

                if dir.Magnitude > 0 then
                    dir = dir.Unit * flightSpeed
                end

                bodyVelocity.Velocity = dir
                bodyGyro.CFrame = workspace.CurrentCamera.CFrame
            end)
        else
            if flightConnection then
                flightConnection:Disconnect()
                flightConnection = nil
            end
            destroyFlight()
        end
        playClickSound(flyToggle, "rbxassetid://9118823109")
    end
end)

flyToggle.MouseButton1Click:Connect(function()
    -- Ativa ou desativa voar via botão também
    flying = not flying
    if flying then
        local character = player.Character
        if not character then return end
        local hrp = character:FindFirstChild("HumanoidRootPart")
        if not hrp then return end

        createFlight()

        bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
        bodyGyro.MaxTorque = Vector3.new(4e5, 4e5, 4e5)

        flightConnection = RunService.RenderStepped:Connect(function()
            local camCFrame = workspace.CurrentCamera.CFrame
            local dir = Vector3.new(0, 0, 0)

            if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                dir = dir + camCFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                dir = dir - camCFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                dir = dir - camCFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                dir = dir + camCFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                dir = dir + Vector3.new(0, 1, 0)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
                dir = dir - Vector3.new(0, 1, 0)
            end

            if dir.Magnitude > 0 then
                dir = dir.Unit * flightSpeed
            end

            bodyVelocity.Velocity = dir
            bodyGyro.CFrame = workspace.CurrentCamera.CFrame
        end)
    else
        if flightConnection then
            flightConnection:Disconnect()
            flightConnection = nil
        end
        destroyFlight()
    end
    playClickSound(flyToggle, "rbxassetid://9118823109")
end)

-- Invisibilidade Toggle
local invisToggle = Instance.new("TextButton")
invisToggle.Text = "Invisibilidade ON/OFF"
invisToggle.Size = UDim2.new(0, 200, 0, 30)
invisToggle.Position = UDim2.new(0, 10, 0, 90)
invisToggle.BackgroundColor3 = Color3.fromRGB(60, 0, 90)
invisToggle.TextColor3 = Color3.new(1, 1, 1)
invisToggle.Font = Enum.Font.GothamBold
invisToggle.TextSize = 14
invisToggle.Parent = HomeTab

local invisEnabled = false
local invisPartsCache = {}

local function saveOriginalState(instance)
    if invisPartsCache[instance] then return end
    invisPartsCache[instance] = {
        Transparency = instance.Transparency,
        CanCollide = instance:IsA("BasePart") and instance.CanCollide or nil
    }
end

local function setTransparencyRecursive(instance, transparency)
    if instance:IsA("BasePart") then
        if invisEnabled then
            saveOriginalState(instance)
            instance.Transparency = transparency
            instance.CanCollide = false
        else
            local original = invisPartsCache[instance]
            if original then
                instance.Transparency = original.Transparency
                if original.CanCollide ~= nil then
                    instance.CanCollide = original.CanCollide
                end
            else
                instance.Transparency = 0
                if instance:IsA("BasePart") then
                    instance.CanCollide = true
                end
            end
        end
    elseif instance:IsA("Decal") or instance:IsA("Texture") or instance:IsA("SpecialMesh") then
        instance.Transparency = transparency
    end

    for _, child in ipairs(instance:GetChildren()) do
        setTransparencyRecursive(child, transparency)
    end
end

invisToggle.MouseButton1Click:Connect(function()
    invisEnabled = not invisEnabled

    local soundId = invisEnabled and "rbxassetid://9118823109" or "rbxassetid://9118823111"
    playClickSound(invisToggle, soundId)

    local character = player.Character
    if character then
        if invisEnabled then
            invisPartsCache = {}
            setTransparencyRecursive(character, 1)
        else
            setTransparencyRecursive(character, 0)
            invisPartsCache = {}
        end
    end
end)

-- ===== Conteúdo Aba Outfit =====
-- Exemplo de botão para colocar roupa especial
local outfitBtn = Instance.new("TextButton")
outfitBtn.Text = "Aplicar Roupa Sombria"
outfitBtn.Size = UDim2.new(0, 200, 0, 30)
outfitBtn.Position = UDim2.new(0, 10, 0, 10)
outfitBtn.BackgroundColor3 = Color3.fromRGB(60, 0, 90)
outfitBtn.TextColor3 = Color3.new(1, 1, 1)
outfitBtn.Font = Enum.Font.GothamBold
outfitBtn.TextSize = 14
outfitBtn.Parent = OutfitTab

outfitBtn.MouseButton1Click:Connect(function()
    playClickSound(outfitBtn, "rbxassetid://9118823109")
    -- Aqui você adicionaria o código para equipar a roupa
    print("Roupa sombria aplicada!")
end)

-- ===== Conteúdo Aba Risk =====
local riskBtn = Instance.new("TextButton")
riskBtn.Text = "Gerar Dinheiro Macabro"
riskBtn.Size = UDim2.new(0, 200, 0, 30)
riskBtn.Position = UDim2.new(0, 10, 0, 10)
riskBtn.BackgroundColor3 = Color3.fromRGB(60, 0, 90)
riskBtn.TextColor3 = Color3.new(1, 1, 1)
riskBtn.Font = Enum.Font.GothamBold
riskBtn.TextSize = 14
riskBtn.Parent = RiskTab

riskBtn.MouseButton1Click:Connect(function()
    playClickSound(riskBtn, "rbxassetid://9118823109")
    -- Código para gerar dinheiro ou função especial
    print("Dinheiro macabro gerado!")
end)

-- === RISK TAB ===
local moneyBox = Instance.new("TextBox", RiskTab)
moneyBox.PlaceholderText = "Quantidade de dinheiro"
moneyBox.Size = UDim2.new(0, 200, 0, 30)
moneyBox.Position = UDim2.new(0, 110, 0, 90)
moneyBox.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
moneyBox.TextColor3 = Color3.new(1, 1, 1)
moneyBox.Font = Enum.Font.Gotham

local moneyBtn = Instance.new("TextButton", RiskTab)
moneyBtn.Text = "💸 Adicionar Dinheiro"
moneyBtn.Size = UDim2.new(0, 200, 0, 30)
moneyBtn.Position = UDim2.new(0, 110, 0, 130)
moneyBtn.BackgroundColor3 = Color3.fromRGB(60, 0, 90)
moneyBtn.TextColor3 = Color3.new(1, 1, 1)
moneyBtn.Font = Enum.Font.GothamBold
moneyBtn.TextSize = 14

moneyBtn.MouseButton1Click:Connect(function()
    local amount = tonumber(moneyBox.Text)
    if amount and amount > 0 then
        -- Exemplo: adicionar dinheiro - aqui você conecta ao sistema do jogo
        print("Adicionando dinheiro: " .. amount)
        -- Som ao adicionar dinheiro
        local sound = Instance.new("Sound", moneyBtn)
        sound.SoundId = "rbxassetid://9118823109"
        sound.Volume = 0.7
        sound:Play()
        game.Debris:AddItem(sound, 2)
    else
        warn("Valor inválido para dinheiro")
    end
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
