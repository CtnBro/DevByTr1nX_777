-- GUI Th1x Menu Script by Tr1nX_777

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

local gui = Instance.new("ScreenGui", PlayerGui)
gui.Name = "Th1xDevMenu"
gui.ResetOnSpawn = false

local Frame = Instance.new("Frame", gui)
Frame.Size = UDim2.new(0, 400, 0, 320)
Frame.Position = UDim2.new(0.35, 0, 0.3, 0)
Frame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0, 12)

local TabList = {}
local CurrentTab

function CreateTab(name)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 30)
    btn.Text = name
    btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 14

    local tabFrame = Instance.new("Frame", Frame)
    tabFrame.Size = UDim2.new(1, 0, 1, -30)
    tabFrame.Position = UDim2.new(0, 0, 0, 30)
    tabFrame.BackgroundTransparency = 1
    tabFrame.Visible = false

    btn.MouseButton1Click:Connect(function()
        if CurrentTab then CurrentTab.Visible = false end
        tabFrame.Visible = true
        CurrentTab = tabFrame
    end)

    table.insert(TabList, {Button = btn, Frame = tabFrame})
    return tabFrame, btn
end

local TabHolder = Instance.new("Frame", Frame)
TabHolder.Size = UDim2.new(0, 100, 0, 290)
TabHolder.Position = UDim2.new(0, 0, 0, 30)
TabHolder.BackgroundColor3 = Color3.fromRGB(20, 20, 20)

local Layout = Instance.new("UIListLayout", TabHolder)
Layout.SortOrder = Enum.SortOrder.LayoutOrder

-- HOME TAB
local HomeTab, homeBtn = CreateTab("Home")
homeBtn.Parent = TabHolder

local espToggle = Instance.new("TextButton", HomeTab)
espToggle.Text = "ESP ON/OFF"
espToggle.Size = UDim2.new(0, 200, 0, 30)
espToggle.Position = UDim2.new(0, 110, 0, 10)

local flyToggle = Instance.new("TextButton", HomeTab)
flyToggle.Text = "Fly ON/OFF"
flyToggle.Size = UDim2.new(0, 200, 0, 30)
flyToggle.Position = UDim2.new(0, 110, 0, 50)

local invisToggle = Instance.new("TextButton", HomeTab)
invisToggle.Text = "Invisível"
invisToggle.Size = UDim2.new(0, 200, 0, 30)
invisToggle.Position = UDim2.new(0, 110, 0, 90)

local tpBox = Instance.new("TextBox", HomeTab)
tpBox.PlaceholderText = "Nome do Jogador"
tpBox.Size = UDim2.new(0, 200, 0, 30)
tpBox.Position = UDim2.new(0, 110, 0, 130)

local tpBtn = Instance.new("TextButton", HomeTab)
tpBtn.Text = "Teleportar"
tpBtn.Size = UDim2.new(0, 200, 0, 30)
tpBtn.Position = UDim2.new(0, 110, 0, 170)

-- OUTFIT TAB
local OutfitTab, outfitBtn = CreateTab("Outfit")
outfitBtn.Parent = TabHolder

local carrotBtn = Instance.new("TextButton", OutfitTab)
carrotBtn.Text = "GRG STORE - Cenoura"
carrotBtn.Size = UDim2.new(0, 200, 0, 30)
carrotBtn.Position = UDim2.new(0, 110, 0, 10)

local darkBtn = Instance.new("TextButton", OutfitTab)
darkBtn.Text = "tr1nx_777 - Roupa Macabra"
darkBtn.Size = UDim2.new(0, 200, 0, 30)
darkBtn.Position = UDim2.new(0, 110, 0, 50)

-- RISK TAB
local RiskTab, riskBtn = CreateTab("Risk")
riskBtn.Parent = TabHolder

local desc = Instance.new("TextLabel", RiskTab)
desc.Text = "Tr1nX_777 que domina"
desc.Size = UDim2.new(0, 260, 0, 30)
desc.Position = UDim2.new(0, 110, 0, 10)
desc.TextColor3 = Color3.new(1, 0, 0)
desc.Font = Enum.Font.GothamBold
desc.TextSize = 14
desc.BackgroundTransparency = 1

local moneyBtn = Instance.new("TextButton", RiskTab)
moneyBtn.Text = "💸 Adicionar Dinheiro"
moneyBtn.Size = UDim2.new(0, 200, 0, 30)
moneyBtn.Position = UDim2.new(0, 110, 0, 50)

-- Minimizar botão
local MiniBtn = Instance.new("TextButton", Frame)
MiniBtn.Text = "-"
MiniBtn.Size = UDim2.new(0, 30, 0, 30)
MiniBtn.Position = UDim2.new(1, -30, 0, 0)

local miniIcon = Instance.new("TextButton", gui)
miniIcon.Text = "Th1x"
miniIcon.Size = UDim2.new(0, 100, 0, 40)
miniIcon.Position = UDim2.new(0, 10, 1, -50)
miniIcon.Visible = false

MiniBtn.MouseButton1Click:Connect(function()
    Frame.Visible = false
    miniIcon.Visible = true
end)

miniIcon.MouseButton1Click:Connect(function()
    Frame.Visible = true
    miniIcon.Visible = false
end)
