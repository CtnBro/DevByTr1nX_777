local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RS = game:GetService("RunService")
local cam = workspace.CurrentCamera
local lp = Players.LocalPlayer
local char = lp.Character or lp.CharacterAdded:Wait()
local hum = char:WaitForChild("Humanoid")
local root = char:WaitForChild("HumanoidRootPart")
local gui = Instance.new("ScreenGui", lp:WaitForChild("PlayerGui"))
gui.Name = "Th1xDevMenu"
gui.ResetOnSpawn = false

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 400, 0, 300)
main.Position = UDim2.new(0.5, -200, 0.5, -150)
main.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
main.BorderSizePixel = 0
main.Active = true
main.Draggable = true

local uiCorner = Instance.new("UICorner", main)
uiCorner.CornerRadius = UDim.new(0, 10)

local tabBtns = Instance.new("Frame", main)
tabBtns.Size = UDim2.new(0, 100, 1, 0)
tabBtns.Position = UDim2.new(0, 0, 0, 0)
tabBtns.BackgroundColor3 = Color3.fromRGB(15, 15, 25)

local UIList = Instance.new("UIListLayout", tabBtns)
UIList.SortOrder = Enum.SortOrder.LayoutOrder

local content = Instance.new("Frame", main)
content.Size = UDim2.new(1, -100, 1, 0)
content.Position = UDim2.new(0, 100, 0, 0)
content.BackgroundColor3 = Color3.fromRGB(20, 20, 30)

local function TabBtn(name)
	local btn = Instance.new("TextButton", tabBtns)
	btn.Size = UDim2.new(1, 0, 0, 40)
	btn.Text = name
	btn.Font = Enum.Font.GothamBold
	btn.TextColor3 = Color3.new(1, 1, 1)
	btn.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
	btn.BorderSizePixel = 0
	return btn
end

local function createPage()
	local page = Instance.new("ScrollingFrame", content)
	page.Size = UDim2.new(1, 0, 1, 0)
	page.BackgroundTransparency = 1
	page.Visible = false
	page.ScrollBarThickness = 4
	local layout = Instance.new("UIListLayout", page)
	layout.SortOrder = Enum.SortOrder.LayoutOrder
	layout.Padding = UDim.new(0, 6)
	return page
end

local function createBtn(text, parent)
	local btn = Instance.new("TextButton", parent)
	btn.Size = UDim2.new(1, -20, 0, 30)
	btn.Text = text
	btn.Font = Enum.Font.GothamBold
	btn.TextColor3 = Color3.new(1,1,1)
	btn.BackgroundColor3 = Color3.fromRGB(40,40,50)
	btn.BorderSizePixel = 0
	btn.Position = UDim2.new(0, 10, 0, 0)
	local corner = Instance.new("UICorner", btn)
	corner.CornerRadius = UDim.new(0, 6)
	return btn
end

local function createText(text, parent)
	local lbl = Instance.new("TextLabel", parent)
	lbl.Size = UDim2.new(1, -20, 0, 30)
	lbl.Text = text
	lbl.Font = Enum.Font.GothamBold
	lbl.TextColor3 = Color3.fromRGB(180, 0, 255)
	lbl.BackgroundTransparency = 1
	lbl.Position = UDim2.new(0, 10, 0, 0)
	return lbl
end

local homeTab = createPage()
local outfitTab = createPage()
local riskTab = createPage()
local tabs = {Home = homeTab, Outfit = outfitTab, Risk = riskTab}
local CurrentTab = homeTab
CurrentTab.Visible = true

for name, page in pairs(tabs) do
	local btn = TabBtn(name)
	btn.MouseButton1Click:Connect(function()
		for _, p in pairs(tabs) do p.Visible = false end
		page.Visible = true
		CurrentTab = page
	end)
end

-- ESP (placeholder)
createBtn("ESP ON/OFF", homeTab)

-- Fly
local isFlying = false
local flyBtn = createBtn("Ativar Voo (CTRL)", homeTab)
local flySpeed = 3
local velocity = Instance.new("BodyVelocity")
velocity.MaxForce = Vector3.new(1e9, 1e9, 1e9)
velocity.Name = "Th1xFlight"

function getChar() return lp.Character or lp.CharacterAdded:Wait() end
function getRoot() return getChar():WaitForChild("HumanoidRootPart") end

UIS.InputBegan:Connect(function(input, gpe)
	if not gpe and input.KeyCode == Enum.KeyCode.LeftControl then
		if isFlying then
			isFlying = false
			velocity.Parent = nil
		else
			isFlying = true
			velocity.Parent = getRoot()
		end
	end
end)

RS.RenderStepped:Connect(function()
	if isFlying then
		local move = Vector3.new()
		if UIS:IsKeyDown(Enum.KeyCode.W) then move += cam.CFrame.LookVector end
		if UIS:IsKeyDown(Enum.KeyCode.S) then move -= cam.CFrame.LookVector end
		if UIS:IsKeyDown(Enum.KeyCode.A) then move -= cam.CFrame.RightVector end
		if UIS:IsKeyDown(Enum.KeyCode.D) then move += cam.CFrame.RightVector end
		if UIS:IsKeyDown(Enum.KeyCode.Space) then move += Vector3.new(0, 1, 0) end
		velocity.Velocity = move.Unit * flySpeed
	end
end)

-- Invisível
local invisBtn = createBtn("Ficar Invisível", homeTab)
invisBtn.MouseButton1Click:Connect(function()
	for _, v in pairs(getChar():GetDescendants()) do
		if v:IsA("BasePart") then v.Transparency = 1 end
	end
end)

-- Outfit
local outfitBtn = createBtn("Usar Roupa GRG STORE", outfitTab)
outfitBtn.MouseButton1Click:Connect(function()
	local hum = getChar():FindFirstChild("Humanoid")
	if hum then
		for _, v in ipairs(getChar():GetChildren()) do
			if v:IsA("Accessory") or v:IsA("Shirt") or v:IsA("Pants") then v:Destroy() end
		end
		local shirt = Instance.new("Shirt", getChar())
		shirt.ShirtTemplate = "rbxassetid://163977769609584"
	end
end)

-- Risk - dinheiro
local moneyBtn = createBtn("💸 Adicionar Dinheiro", riskTab)
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
