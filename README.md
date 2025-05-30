local Players = game:GetService("Players")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

local gui = Instance.new("ScreenGui", PlayerGui)
gui.Name = "Th1xDevMenu"
gui.ResetOnSpawn = false

local Frame = Instance.new("Frame", gui)
Frame.Size = UDim2.new(0, 400, 0, 320)
Frame.Position = UDim2.new(0.35, 0, 0.3, 0)
Frame.BackgroundColor3 = Color3.fromRGB(35, 0, 60) -- Roxo escuro
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0, 12)

local TabHolder = Instance.new("Frame", Frame)
TabHolder.Size = UDim2.new(0, 100, 0, 290)
TabHolder.Position = UDim2.new(0, 0, 0, 30)
TabHolder.BackgroundColor3 = Color3.fromRGB(20, 20, 20) -- Cinza escuro

local Layout = Instance.new("UIListLayout", TabHolder)
Layout.SortOrder = Enum.SortOrder.LayoutOrder

local TabList = {}
local CurrentTab

function CreateTab(name)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 30)
    btn.Text = name
    btn.BackgroundColor3 = Color3.fromRGB(50, 0, 90) -- Roxo mais vibrante para botões
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 14
    btn.BorderSizePixel = 0

    local tabFrame = Instance.new("Frame", Frame)
    tabFrame.Size = UDim2.new(1, 0, 1, -30)
    tabFrame.Position = UDim2.new(0, 0, 0, 30)
    tabFrame.BackgroundColor3 = Color3.fromRGB(25, 0, 50) -- Fundo roxo escuro
    tabFrame.BackgroundTransparency = 0
    tabFrame.Visible = false
    local corner = Instance.new("UICorner", tabFrame)
    corner.CornerRadius = UDim.new(0, 10)

    btn.MouseButton1Click:Connect(function()
        if CurrentTab then CurrentTab.Visible = false end
        tabFrame.Visible = true
        CurrentTab = tabFrame
    end)

    table.insert(TabList, {Button = btn, Frame = tabFrame})
    return tabFrame, btn
end

-- HOME TAB
local HomeTab, homeBtn = CreateTab("Home")
homeBtn.Parent = TabHolder

local espToggle = Instance.new("TextButton", HomeTab)
espToggle.Text = "ESP ON/OFF"
espToggle.Size = UDim2.new(0, 200, 0, 30)
espToggle.Position = UDim2.new(0, 110, 0, 10)
espToggle.BackgroundColor3 = Color3.fromRGB(70, 0, 110)
espToggle.TextColor3 = Color3.new(1, 1, 1)
espToggle.Font = Enum.Font.GothamBold

local flyToggle = Instance.new("TextButton", HomeTab)
flyToggle.Text = "Fly ON/OFF"
flyToggle.Size = UDim2.new(0, 200, 0, 30)
flyToggle.Position = UDim2.new(0, 110, 0, 50)
flyToggle.BackgroundColor3 = Color3.fromRGB(70, 0, 110)
flyToggle.TextColor3 = Color3.new(1, 1, 1)
flyToggle.Font = Enum.Font.GothamBold

local invisToggle = Instance.new("TextButton", HomeTab)
invisToggle.Text = "Invisível ON/OFF"
invisToggle.Size = UDim2.new(0, 200, 0, 30)
invisToggle.Position = UDim2.new(0, 110, 0, 90)
invisToggle.BackgroundColor3 = Color3.fromRGB(70, 0, 110)
invisToggle.TextColor3 = Color3.new(1, 1, 1)
invisToggle.Font = Enum.Font.GothamBold

local tpBox = Instance.new("TextBox", HomeTab)
tpBox.PlaceholderText = "Nome do Jogador"
tpBox.Size = UDim2.new(0, 200, 0, 30)
tpBox.Position = UDim2.new(0, 110, 0, 130)
tpBox.BackgroundColor3 = Color3.fromRGB(40, 0, 70)
tpBox.TextColor3 = Color3.new(1, 1, 1)
tpBox.Font = Enum.Font.GothamBold
tpBox.ClearTextOnFocus = false
tpBox.TextStrokeTransparency = 0.8

local tpBtn = Instance.new("TextButton", HomeTab)
tpBtn.Text = "Teleportar"
tpBtn.Size = UDim2.new(0, 200, 0, 30)
tpBtn.Position = UDim2.new(0, 110, 0, 170)
tpBtn.BackgroundColor3 = Color3.fromRGB(70, 0, 110)
tpBtn.TextColor3 = Color3.new(1, 1, 1)
tpBtn.Font = Enum.Font.GothamBold

-- FUNÇÃO DE VOAR MELHORADA
local flying = false
local flyingSpeed = 50
local flyBodyVelocity, flyBodyGyro

local soundFlyToggle = Instance.new("Sound", Frame)
soundFlyToggle.SoundId = "rbxassetid://9118825981" -- som curto, macabro (pode trocar o id)
soundFlyToggle.Volume = 0.5

flyToggle.MouseButton1Click:Connect(function()
    flying = not flying
    soundFlyToggle:Play()
    if flying then
        local char = player.Character
        if char then
            local hrp = char:FindFirstChild("HumanoidRootPart")
            local humanoid = char:FindFirstChildOfClass("Humanoid")
            if hrp and humanoid then
                humanoid.PlatformStand = true
                flyBodyVelocity = Instance.new("BodyVelocity", hrp)
                flyBodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
                flyBodyVelocity.Velocity = Vector3.new(0, 0, 0)

                flyBodyGyro = Instance.new("BodyGyro", hrp)
                flyBodyGyro.MaxTorque = Vector3.new(1e5, 1e5, 1e5)
                flyBodyGyro.CFrame = hrp.CFrame

                -- Partículas roxas macabras
                local particle = Instance.new("ParticleEmitter", hrp)
                particle.Color = ColorSequence.new(Color3.fromRGB(150, 0, 255))
                particle.LightEmission = 0.8
                particle.Size = NumberSequence.new(0.5)
                particle.Rate = 20
                particle.Speed = NumberRange.new(1)
                particle.Lifetime = NumberRange.new(0.5)
                particle.Name = "FlyParticles"

                -- Controle de voo
                spawn(function()
                    while flying and hrp.Parent do
                        flyBodyVelocity.Velocity = hrp.CFrame.LookVector * flyingSpeed + Vector3.new(0, 10 * math.sin(tick()*5), 0) -- flutuação
                        flyBodyGyro.CFrame = workspace.CurrentCamera.CFrame * CFrame.Angles(math.rad(5*math.sin(tick()*4)), 0, 0)
                        wait()
                    end
                    if flyBodyVelocity then flyBodyVelocity:Destroy() end
                    if flyBodyGyro then flyBodyGyro:Destroy() end
                    if hrp:FindFirstChild("FlyParticles") then hrp.FlyParticles:Destroy() end
                    if humanoid then humanoid.PlatformStand = false end
                end)
            end
        end
    else
        -- Para voar
        local char = player.Character
        if char then
            local hrp = char:FindFirstChild("HumanoidRootPart")
            local humanoid = char:FindFirstChildOfClass("Humanoid")
            if hrp then
                if flyBodyVelocity then flyBodyVelocity:Destroy() end
                if flyBodyGyro then flyBodyGyro:Destroy() end
                if hrp:FindFirstChild("FlyParticles") then hrp.FlyParticles:Destroy() end
                if humanoid then humanoid.PlatformStand = false end
            end
        end
    end
end)

-- ADICIONAR DINHEIRO COM INPUT
local moneyInput = Instance.new("TextBox", HomeTab)
moneyInput.PlaceholderText = "Quantidade de dinheiro"
moneyInput.Size = UDim2.new(0, 130, 0, 30)
moneyInput.Position = UDim2.new(0, 110, 0, 210)
moneyInput.BackgroundColor3 = Color3.fromRGB(40, 0, 70)
moneyInput.TextColor3 = Color3.new(1, 1, 1)
moneyInput.Font = Enum.Font.GothamBold
moneyInput.ClearTextOnFocus = false

local addMoneyBtn = Instance.new("TextButton", HomeTab)
addMoneyBtn.Text = "Adicionar Dinheiro"
addMoneyBtn.Size = UDim2.new(0, 70, 0, 30)
addMoneyBtn.Position = UDim2.new(0, 250, 0, 210)
addMoneyBtn.BackgroundColor3 = Color3.fromRGB(70, 0, 110)
addMoneyBtn.TextColor3 = Color3.new(1, 1, 1)
addMoneyBtn.Font = Enum.Font.GothamBold

addMoneyBtn.MouseButton1Click:Connect(function()
    local amount = tonumber(moneyInput.Text)
    if amount and amount > 0 then
        -- Aqui você coloca a função de adicionar dinheiro do jogo
        print("Adicionar dinheiro: "..amount)
        -- Exemplo: player.leaderstats.Cash.Value += amount (ajuste conforme seu sistema)
    else
        warn("Quantidade inválida")
    end
end)

-- OUTFIT TAB - aplicar roupas personalizadas
local OutfitTab, outfitBtn = CreateTab("Outfit")
outfitBtn.Parent = TabHolder

local function applyOutfit(modelId)
    local char = player.Character
    if char then
        -- Tenta usar Humanoid:ApplyDescription (se usar R15)
        local humanoid = char:FindFirstChildOfClass("Humanoid")
        if humanoid then
            local success, err = pcall(function()
                local asset = game:GetService("InsertService"):LoadAsset(modelId)
                asset.Parent = char
            end)
            if not success then warn("Erro ao aplicar roupa: "..err) end
        end
    end
end

local carrotBtn = Instance.new("TextButton", OutfitTab)
carrotBtn.Text = "GRG STORE - Cenoura"
carrotBtn.Size = UDim2.new(0, 200, 0, 30)
carrotBtn.Position = UDim2.new(0, 110, 0, 10)
carrotBtn.BackgroundColor3 = Color3.fromRGB(70, 0, 110)
carrotBtn.TextColor3 = Color3.new(1, 1, 1)
carrotBtn.Font = Enum.Font.GothamBold
carrotBtn.MouseButton1Click:Connect(function()
    applyOutfit(163977769609584) -- troque pelo ID real do modelo da cenoura
end)

local darkBtn = Instance.new("TextButton", OutfitTab)
darkBtn.Text = "tr1nx_777 - Roupa Macabra"
darkBtn.Size = UDim2.new(0, 200, 0, 30)
darkBtn.Position = UDim2.new(0, 110, 0, 50)
darkBtn.BackgroundColor3 = Color3.fromRGB(70, 0, 110)
darkBtn.TextColor3 = Color3.new(1, 1, 1)
darkBtn.Font = Enum.Font.GothamBold
darkBtn.MouseButton1Click:Connect(function()
    applyOutfit(87654321) -- troque pelo ID real do modelo da roupa macabra
end)

-- ... Resto do código (abas, minimização, etc) permanece o mesmo
