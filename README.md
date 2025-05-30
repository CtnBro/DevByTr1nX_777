local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local InsertService = game:GetService("InsertService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Criar GUI principal
local gui = Instance.new("ScreenGui", playerGui)
gui.Name = "Th1xDevMenu"
gui.ResetOnSpawn = false

local Frame = Instance.new("Frame", gui)
Frame.Size = UDim2.new(0, 400, 0, 320)
Frame.Position = UDim2.new(0.35, 0, 0.3, 0)
Frame.BackgroundColor3 = Color3.fromRGB(35, 0, 60) -- roxo escuro
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0, 12)

-- Botão minimizar/restaurar
local minimizeBtn = Instance.new("TextButton", Frame)
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(1, -35, 0, 5)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(50, 0, 90)
minimizeBtn.Text = "-"
minimizeBtn.TextColor3 = Color3.new(1,1,1)
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 20
minimizeBtn.ZIndex = 5

local isMinimized = false
minimizeBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    for _, v in pairs(Frame:GetChildren()) do
        if v ~= minimizeBtn and v.ClassName ~= "UICorner" then
            v.Visible = not isMinimized
        end
    end
    if isMinimized then
        Frame.Size = UDim2.new(0, 100, 0, 40)
    else
        Frame.Size = UDim2.new(0, 400, 0, 320)
    end
end)

-- Container das abas (botões)
local TabHolder = Instance.new("Frame", Frame)
TabHolder.Size = UDim2.new(0, 100, 0, 290)
TabHolder.Position = UDim2.new(0, 0, 0, 30)
TabHolder.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
TabHolder.BorderSizePixel = 0

local Layout = Instance.new("UIListLayout", TabHolder)
Layout.SortOrder = Enum.SortOrder.LayoutOrder
Layout.Padding = UDim.new(0, 5)

-- Armazenar abas e botões
local Tabs = {}
local CurrentTab

local function CreateTab(name)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 30)
    btn.BackgroundColor3 = Color3.fromRGB(50, 0, 90)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 14
    btn.Text = name
    btn.BorderSizePixel = 0

    local tabFrame = Instance.new("Frame", Frame)
    tabFrame.Size = UDim2.new(1, -100, 1, -30)
    tabFrame.Position = UDim2.new(0, 100, 0, 30)
    tabFrame.BackgroundColor3 = Color3.fromRGB(25, 0, 50)
    tabFrame.Visible = false
    local corner = Instance.new("UICorner", tabFrame)
    corner.CornerRadius = UDim.new(0, 10)

    btn.MouseButton1Click:Connect(function()
        if CurrentTab then
            CurrentTab.Visible = false
        end
        tabFrame.Visible = true
        CurrentTab = tabFrame
    end)

    btn.Parent = TabHolder

    Tabs[name] = tabFrame
    return tabFrame, btn
end

-- Criar abas
local HomeTab, homeBtn = CreateTab("Home")
local OutfitTab, outfitBtn = CreateTab("Outfit")
local RiskTab, riskBtn = CreateTab("Risk")

-- Mostrar aba Home por padrão
homeBtn:CaptureFocus()
homeBtn.AutoButtonColor = false
homeBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 120)
CurrentTab = HomeTab
HomeTab.Visible = true

-- === ANIMAÇÃO E CONTROLE DE VOO ===

local flying = false
local speed = 60
local flyBodyVelocity
local flyBodyGyro
local char = player.Character or player.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")
local humanoid = char:WaitForChild("Humanoid")

local moveVector = Vector3.new(0,0,0)

-- Função para ativar voo
local function startFly()
    flying = true
    humanoid.PlatformStand = true
    flyBodyVelocity = Instance.new("BodyVelocity", hrp)
    flyBodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    flyBodyVelocity.Velocity = Vector3.new(0,0,0)

    flyBodyGyro = Instance.new("BodyGyro", hrp)
    flyBodyGyro.MaxTorque = Vector3.new(1e5, 1e5, 1e5)
    flyBodyGyro.CFrame = hrp.CFrame
end

-- Função para parar voo
local function stopFly()
    flying = false
    humanoid.PlatformStand = false
    if flyBodyVelocity then flyBodyVelocity:Destroy() flyBodyVelocity = nil end
    if flyBodyGyro then flyBodyGyro:Destroy() flyBodyGyro = nil end
end

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.LeftControl then
        if flying then
            stopFly()
        else
            startFly()
        end
    end
end)

-- Atualiza vetor de movimento
local function updateMoveVector()
    local camCFrame = workspace.CurrentCamera.CFrame
    local forward = camCFrame.LookVector
    local right = camCFrame.RightVector

    local move = Vector3.new(0,0,0)
    if UserInputService:IsKeyDown(Enum.KeyCode.W) then
        move = move + forward
    end
    if UserInputService:IsKeyDown(Enum.KeyCode.S) then
        move = move - forward
    end
    if UserInputService:IsKeyDown(Enum.KeyCode.A) then
        move = move - right
    end
    if UserInputService:IsKeyDown(Enum.KeyCode.D) then
        move = move + right
    end
    if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
        move = move + Vector3.new(0,1,0)
    end
    if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
        move = move - Vector3.new(0,1,0)
    end

    moveVector = move.Unit
end

RunService.RenderStepped:Connect(function()
    if flying and flyBodyVelocity and flyBodyGyro then
        updateMoveVector()
        if moveVector ~= moveVector then moveVector = Vector3.new(0,0,0) end -- NaN check
        flyBodyVelocity.Velocity = moveVector * speed
        flyBodyGyro.CFrame = workspace.CurrentCamera.CFrame
    end
end)

-- === INVISIBILIDADE FUNCIONAL ===

local invisible = false

local function setInvisible(state)
    local character = player.Character
    if not character then return end

    invisible = state
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") or part:IsA("Decal") or part:IsA("Accessory") then
            if state then
                if part:IsA("BasePart") then
                    part.Transparency = 1
                    if part:IsA("Decal") then
                        part.Transparency = 1
                    end
                elseif part:IsA("Decal") then
                    part.Transparency = 1
                elseif part:IsA("Accessory") then
                    local handle = part:FindFirstChild("Handle")
                    if handle then
                        handle.Transparency = 1
                    end
                end
            else
                if part:IsA("BasePart") then
                    part.Transparency = 0
                elseif part:IsA("Decal") then
                    part.Transparency = 0
                elseif part:IsA("Accessory") then
                    local handle = part:FindFirstChild("Handle")
                    if handle then
                        handle.Transparency = 0
                    end
                end
            end
        end
    end
end

local invisToggle = Instance.new("TextButton", HomeTab)
invisToggle.Text = "Invisível ON/OFF"
invisToggle.Size = UDim2.new(0, 200, 0, 30)
invisToggle.Position = UDim2.new(0, 110, 0, 90)
invisToggle.BackgroundColor3 = Color3.fromRGB(70, 0, 110)
invisToggle.TextColor3 = Color3.new(1, 1, 1)
invisToggle.Font = Enum.Font.GothamBold

invisToggle.MouseButton1Click:Connect(function()
    setInvisible(not invisible)
end)

-- === DINHEIRO ABA RISK ===

local moneyInput = Instance.new("TextBox", RiskTab)
moneyInput.PlaceholderText = "Quantidade de dinheiro"
moneyInput.Size = UDim2.new(0, 130, 0, 30)
moneyInput.Position = UDim2.new(0, 30, 0, 10)
moneyInput.BackgroundColor3 = Color3.fromRGB(40, 0, 70)
moneyInput.TextColor3 = Color3.new(1, 1, 1)
moneyInput.Font = Enum.Font.GothamBold
moneyInput.ClearTextOnFocus = false

local addMoneyBtn = Instance.new("TextButton", RiskTab)
addMoneyBtn.Text = "Adicionar Dinheiro"
addMoneyBtn.Size = UDim2.new(0, 120, 0, 30)
addMoneyBtn.Position = UDim2.new(0, 170, 0, 10)
addMoneyBtn.BackgroundColor3 = Color3.fromRGB(70, 0, 110)
addMoneyBtn.TextColor3 = Color3.new(1, 1, 1)
addMoneyBtn.Font = Enum.Font.GothamBold

addMoneyBtn.MouseButton1Click:Connect(function()
    local amount = tonumber(moneyInput.Text)
    if amount and amount > 0 then
        print("Adicionar dinheiro: "..amount)
        -- Aqui adiciona o dinheiro no sistema real
    else
        warn("Quantidade inválida")
    end
end)

-- === OUTFIT (ROUPAS PERSONALIZADAS) ===

local function applyOutfit(modelId)
    local char = player.Character
    if char then
        local humanoid = char:FindFirstChildOfClass("Humanoid")
        if humanoid then
            local success, asset = pcall(function()
                return InsertService:LoadAsset(modelId)
            end)
            if success and asset then
                asset.Parent = char
            else
                warn("Falha ao carregar roupa do ID "..tostring(modelId))
            end
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
    applyOutfit(163977769609584)
end)

-- Fim do trecho principal
