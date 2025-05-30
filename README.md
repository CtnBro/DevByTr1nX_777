loadstring([[
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local InsertService = game:GetService("InsertService")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Remove GUI antiga
local oldGui = playerGui:FindFirstChild("Tr1nX_Macabro_GUI")
if oldGui then oldGui:Destroy() end

local function addUICorner(obj, radius)
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, radius)
    corner.Parent = obj
end

-- Criar GUI base
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "Tr1nX_Macabro_GUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 520, 0, 620)
frame.Position = UDim2.new(0.25, 0, 0.15, 0)
frame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Parent = screenGui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(0.9, 0, 0.1, 0)
title.Position = UDim2.new(0.05, 0, 0.02, 0)
title.BackgroundTransparency = 0.5
title.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
title.TextColor3 = Color3.fromRGB(255, 0, 0)
title.Font = Enum.Font.GothamBlack
title.TextSize = 30
title.Text = "DEV BY Tr1nX_777 | MACABRO"
title.BorderSizePixel = 0
title.Parent = frame
addUICorner(title, 15)

-- Botão Minimizar
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(1, -35, 0.02, 0)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
minimizeBtn.TextColor3 = Color3.new(1, 1, 1)
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 24
minimizeBtn.Text = "—"
minimizeBtn.BorderSizePixel = 0
addUICorner(minimizeBtn, 7)
minimizeBtn.Parent = frame

-- GUI Minimizada (quadradinho)
local miniFrame = Instance.new("Frame")
miniFrame.Size = UDim2.new(0, 110, 0, 40)
miniFrame.Position = UDim2.new(0.01, 0, 0.9, 0)
miniFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
miniFrame.BorderSizePixel = 0
miniFrame.Visible = false
miniFrame.Parent = screenGui
addUICorner(miniFrame, 10)

local miniLabel = Instance.new("TextLabel")
miniLabel.Size = UDim2.new(1, 0, 1, 0)
miniLabel.BackgroundTransparency = 1
miniLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
miniLabel.Font = Enum.Font.GothamBlack
miniLabel.TextSize = 24
miniLabel.Text = "Th1x"
miniLabel.Parent = miniFrame

minimizeBtn.MouseButton1Click:Connect(function()
    frame.Visible = false
    miniFrame.Visible = true
end)

miniFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        frame.Visible = true
        miniFrame.Visible = false
    end
end)

-- Abas
local tabHomeBtn = Instance.new("TextButton")
tabHomeBtn.Size = UDim2.new(0, 120, 0, 40)
tabHomeBtn.Position = UDim2.new(0.05, 0, 0.13, 0)
tabHomeBtn.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
tabHomeBtn.TextColor3 = Color3.fromRGB(255, 0, 0)
tabHomeBtn.Font = Enum.Font.GothamBold
tabHomeBtn.TextSize = 22
tabHomeBtn.Text = "HOME"
tabHomeBtn.BorderSizePixel = 0
addUICorner(tabHomeBtn, 10)
tabHomeBtn.Parent = frame

local tabOutfitBtn = Instance.new("TextButton")
tabOutfitBtn.Size = UDim2.new(0, 120, 0, 40)
tabOutfitBtn.Position = UDim2.new(0.32, 0, 0.13, 0)
tabOutfitBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
tabOutfitBtn.TextColor3 = Color3.fromRGB(120, 120, 120)
tabOutfitBtn.Font = Enum.Font.GothamBold
tabOutfitBtn.TextSize = 22
tabOutfitBtn.Text = "OUTFIT"
tabOutfitBtn.BorderSizePixel = 0
addUICorner(tabOutfitBtn, 10)
tabOutfitBtn.Parent = frame

local homeContainer = Instance.new("Frame")
homeContainer.Size = UDim2.new(0.9, 0, 0.75, 0)
homeContainer.Position = UDim2.new(0.05, 0, 0.21, 0)
homeContainer.BackgroundTransparency = 1
homeContainer.Parent = frame

local outfitContainer = Instance.new("Frame")
outfitContainer.Size = homeContainer.Size
outfitContainer.Position = homeContainer.Position
outfitContainer.BackgroundTransparency = 1
outfitContainer.Visible = false
outfitContainer.Parent = frame

local function switchTab(tabName)
    if tabName == "HOME" then
        homeContainer.Visible = true
        outfitContainer.Visible = false
        tabHomeBtn.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
        tabHomeBtn.TextColor3 = Color3.fromRGB(255, 0, 0)
        tabOutfitBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        tabOutfitBtn.TextColor3 = Color3.fromRGB(120, 120, 120)
    elseif tabName == "OUTFIT" then
        homeContainer.Visible = false
        outfitContainer.Visible = true
        tabHomeBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        tabHomeBtn.TextColor3 = Color3.fromRGB(120, 120, 120)
        tabOutfitBtn.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
        tabOutfitBtn.TextColor3 = Color3.fromRGB(255, 0, 0)
    end
end

tabHomeBtn.MouseButton1Click:Connect(function()
    switchTab("HOME")
end)

tabOutfitBtn.MouseButton1Click:Connect(function()
    switchTab("OUTFIT")
end)

switchTab("HOME") -- inicia na home

-- Função para criar botões na homeContainer
local function createHomeButton(text, posY)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.9, 0, 0, 50)
    btn.Position = UDim2.new(0, 0, 0, posY)
    btn.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
    btn.TextColor3 = Color3.fromRGB(255, 0, 0)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 22
    btn.Text = text
    btn.BorderSizePixel = 0
    addUICorner(btn, 10)
    btn.Parent = homeContainer
    return btn
end

-- DUPLICAR (exemplo simples)
local dupeBtn = createHomeButton("Dupe Item na Mão", 0)
dupeBtn.MouseButton1Click:Connect(function()
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    local heldTool = character:FindFirstChildOfClass("Tool")
    if heldTool and humanoid then
        local clone = heldTool:Clone()
        clone.Parent = character
        humanoid:EquipTool(clone)
        dupeBtn.Text = "✔️ Duped!"
        dupeBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
        task.wait(1.2)
        dupeBtn.Text = "Dupe Item na Mão"
        dupeBtn.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
    else
        dupeBtn.Text = "Sem item na mão!"
        dupeBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
        task.wait(1.2)
        dupeBtn.Text = "Dupe Item na Mão"
        dupeBtn.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
    end
end)

-- ESP toggle
local espEnabled = false
local espBoxes = {}

local function createESP()
    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= player then
            local char = plr.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                if not espBoxes[plr] then
                    local box = Drawing.new("Square")
                    box.Color = Color3.fromRGB(255, 0, 0)
                    box.Thickness = 2
                    box.Filled = false
                    espBoxes[plr] = box
                end
            end
        end
    end
end

local function removeESP()
    for _, box in pairs(espBoxes) do
        box:Remove()
    end
    espBoxes = {}
end

local espBtn = createHomeButton("Toggle ESP", 60)
espBtn.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    if espEnabled then
        espBtn.Text = "ESP ON"
        createESP()
    else
        espBtn.Text = "Toggle ESP"
        removeESP()
    end
end)

RunService.RenderStepped:Connect(function()
    if espEnabled then
        for plr, box in pairs(espBoxes) do
            local char = plr.Character
            if char and char:FindFirstChild("HumanoidRootPart") and char:FindFirstChildOfClass("Humanoid") and char.Humanoid.Health > 0 then
                local hrp = char.HumanoidRootPart
                local vector, onScreen = workspace.CurrentCamera:WorldToViewportPoint(hrp.Position)
                if onScreen then
                    box.Position = Vector2.new(vector.X - 20, vector.Y - 20)
                    box.Size = Vector2.new(40, 40)
                    box.Visible = true
                else
                    box.Visible = false
                end
            else
                if box then box.Visible = false end
            end
        end
    end
end)

-- Voar toggle
local flying = false
local flySpeed = 50
local bodyVelocity

local flyBtn = createHomeButton("Toggle Voar", 120)
flyBtn.MouseButton1Click:Connect(function()
    local character = player.Character
    local humanoid = character and character:FindFirstChildOfClass("Humanoid")
    if not humanoid then return end

    flying = not flying
    if flying then
        flyBtn.Text = "Voar ON"
        bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Velocity = Vector3.new(0,0,0)
        bodyVelocity.MaxForce = Vector3.new(1e5,1e5,1e5)
        bodyVelocity.Parent = character.HumanoidRootPart

        -- Controle simples de voo com teclas
        local connection
        connection = RunService.RenderStepped:Connect(function()
            if flying then
                local direction = Vector3.new()
                if UserInputService:IsKeyDown(Enum.KeyCode.W) then direction = direction + workspace.CurrentCamera.CFrame.LookVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.S) then direction = direction - workspace.CurrentCamera.CFrame.LookVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.A) then direction = direction - workspace.CurrentCamera.CFrame.RightVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.D) then direction = direction + workspace.CurrentCamera.CFrame.RightVector end
                if UserInputService:IsKeyDown(Enum.KeyCode.Space) then direction = direction + Vector3.new(0,1,0) end
                if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then direction = direction - Vector3.new(0,1,0) end

                bodyVelocity.Velocity = direction.Unit * flySpeed
            else
                if connection then
                    connection:Disconnect()
                    connection = nil
                end
            end
        end)
    else
        flyBtn.Text = "Toggle Voar"
        if bodyVelocity then
            bodyVelocity:Destroy()
            bodyVelocity = nil
        end
    end
end)

-- Invisível toggle
local invisible = false

local invisBtn = createHomeButton("Toggle Invisível", 180)
invisBtn.MouseButton1Click:Connect(function()
    local character = player.Character
    if not character then return end
    invisible = not invisible

    for _, part in pairs(character:GetChildren()) do
        if part:IsA("BasePart") or part:IsA("Decal") then
            if invisible then
                part.Transparency = 1
            else
                part.Transparency = 0
            end
        elseif part:IsA("ParticleEmitter") or part:IsA("Trail") then
            part.Enabled = not invisible
        end
    end
    invisBtn.Text = invisible and "Invisível ON" or "Toggle Invisível"
end)

-- Teleport to player
local teleportFrame = Instance.new("Frame")
teleportFrame.Size = UDim2.new(0.9, 0, 0, 60)
teleportFrame.Position = UDim2.new(0, 0, 0, 240)
teleportFrame.BackgroundTransparency = 0
teleportFrame.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
teleportFrame.BorderSizePixel = 0
addUICorner(teleportFrame, 10)
teleportFrame.Parent = homeContainer

local tpInput = Instance.new("TextBox")
tpInput.Size = UDim2.new(0.65, 0, 0.6, 0)
tpInput.Position = UDim2.new(0.03, 0, 0.2, 0)
tpInput.BackgroundColor3 = Color3.fromRGB(25, 0, 0)
tpInput.TextColor3 = Color3.fromRGB(255, 0, 0)
tpInput.PlaceholderText = "Nome do player para teleportar"
tpInput.Font = Enum.Font.Gotham
tpInput.TextSize = 20
tpInput.ClearTextOnFocus = false
addUICorner(tpInput, 10)
tpInput.Parent = teleportFrame

local tpBtn = Instance.new("TextButton")
tpBtn.Size = UDim2.new(0.3, 0, 0.6, 0)
tpBtn.Position = UDim2.new(0.69, 0, 0.2, 0)
tpBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
tpBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
tpBtn.Font = Enum.Font.GothamBold
tpBtn.TextSize = 20
tpBtn.Text = "Teleportar"
addUICorner(tpBtn, 10)
tpBtn.Parent = teleportFrame

tpBtn.MouseButton1Click:Connect(function()
    local targetName = tpInput.Text:lower()
    if targetName == "" then return end

    local targetPlayer
    for _, plr in pairs(Players:GetPlayers()) do
        if plr.Name:lower():find(targetName) then
            targetPlayer = plr
            break
        end
    end

    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = targetPlayer.Character.HumanoidRootPart
        local char = player.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            char.HumanoidRootPart.CFrame = hrp.CFrame + Vector3.new(0, 3, 0)
            tpBtn.Text = "Teleportado!"
            task.wait(1.5)
            tpBtn.Text = "Teleportar"
        end
    else
        tpBtn.Text = "Jogador não encontrado"
        task.wait(1.5)
        tpBtn.Text = "Teleportar"
    end
end)

-- Outfit - gerenciamento das roupas temporárias
local tempAccessories = {}

local function clearTempAccessories()
    for _, acc in pairs(tempAccessories) do
        if acc and acc.Parent then
            acc:Destroy()
        end
    end
    tempAccessories = {}
end

-- GRG Store - vira cenoura
local function applyCarrotOutfit()
    clearTempAccessories()
    local char = player.Character
    if not char then return end

    -- Model da cenoura simples: usar MeshPart com MeshId de cenoura (exemplo)
    local carrot = Instance.new("Part")
    carrot.Name = "CarrotOutfitPart"
    carrot.Size = Vector3.new(1, 2, 1)
    carrot.Color = Color3.fromRGB(255, 110, 0)
    carrot.Material = Enum.Material.SmoothPlastic
    carrot.Anchored = false
    carrot.CanCollide = false
    carrot.Parent = char

    local weld = Instance.new("WeldConstraint")
    weld.Part0 = char.HumanoidRootPart
    weld.Part1 = carrot
    weld.Parent = carrot

    carrot.CFrame = char.HumanoidRootPart.CFrame * CFrame.new(0, 2, 0)

    table.insert(tempAccessories, carrot)
end

-- Tr1nx_777 - roupa macabra (usar acessórios)
local function applyMacabreOutfit()
    clearTempAccessories()
    local char = player.Character
    if not char then return end

    -- IDs de acessórios macabros (exemplo - pode trocar pra acessórios reais)
    local accessoryIds = {
        13390258, -- Máscara
        15212264, -- Capa
        14447057, -- Ombreiras
    }

    for _, assetId in pairs(accessoryIds) do
        local success, accessory = pcall(function()
            return InsertService:LoadAsset(assetId)
        end)
        if success and accessory then
            local acc = accessory:GetChildren()[1]
            if acc and acc:IsA("Accessory") then
                acc.Parent = char
                table.insert(tempAccessories, acc)
            else
                accessory:Destroy()
            end
        end
    end
end

local grgBtn = Instance.new("TextButton")
grgBtn.Size = UDim2.new(0.9, 0, 0, 50)
grgBtn.Position = UDim2.new(0.05, 0, 0, 300)
grgBtn.BackgroundColor3 = Color3.fromRGB(255, 170, 0)
grgBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
grgBtn.Font = Enum.Font.GothamBold
grgBtn.TextSize = 24
grgBtn.Text = "GRG STORE - Vira Cenoura"
addUICorner(grgBtn, 10)
grgBtn.Parent = homeContainer

grgBtn.MouseButton1Click:Connect(function()
    applyCarrotOutfit()
end)

local tr1nxBtn = Instance.new("TextButton")
tr1nxBtn.Size = UDim2.new(0.9, 0, 0, 50)
tr1nxBtn.Position = UDim2.new(0.05, 0, 0, 360)
tr1nxBtn.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
tr1nxBtn.TextColor3 = Color3.fromRGB(255, 0, 0)
tr1nxBtn.Font = Enum.Font.GothamBold
tr1nxBtn.TextSize = 24
tr1nxBtn.Text = "TR1NX_777 - Roupa Macabra"
addUICorner(tr1nxBtn, 10)
tr1nxBtn.Parent = homeContainer

tr1nxBtn.MouseButton1Click:Connect(function()
    applyMacabreOutfit()
end)
