local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local ScreenGui = Instance.new("ScreenGui", playerGui)
ScreenGui.Name = "Th1xDevMenu"
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 400, 0, 300)
MainFrame.Position = UDim2.new(0.35, 0, 0.3, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 0, 25)
MainFrame.Visible = true
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true

local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 12)

-- Aba lateral
local SideBar = Instance.new("Frame", MainFrame)
SideBar.Size = UDim2.new(0, 100, 1, 0)
SideBar.Position = UDim2.new(0, 0, 0, 0)
SideBar.BackgroundColor3 = Color3.fromRGB(15, 0, 15)
local sideCorner = Instance.new("UICorner", SideBar)
sideCorner.CornerRadius = UDim.new(0, 12)

local tabs = {
    "Home",
    "Outfit",
    "Risk"
}

local contentFrames = {}

for i, tabName in ipairs(tabs) do
	local btn = Instance.new("TextButton", SideBar)
	btn.Size = UDim2.new(1, 0, 0, 40)
	btn.Position = UDim2.new(0, 0, 0, (i - 1) * 45 + 10)
	btn.Text = tabName
	btn.BackgroundColor3 = Color3.fromRGB(35, 0, 35)
	btn.TextColor3 = Color3.fromRGB(255, 0, 255)
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 14
	local btnCorner = Instance.new("UICorner", btn)
	btnCorner.CornerRadius = UDim.new(0, 8)

	local frame = Instance.new("Frame", MainFrame)
	frame.Size = UDim2.new(1, -110, 1, -20)
	frame.Position = UDim2.new(0, 110, 0, 10)
	frame.BackgroundColor3 = Color3.fromRGB(20, 0, 20)
	frame.Visible = (i == 1)
	contentFrames[tabName] = frame

	btn.MouseButton1Click:Connect(function()
		for _, f in pairs(contentFrames) do f.Visible = false end
		frame.Visible = true
	end)
end

-- HOME TAB CONTENT
do
	local home = contentFrames["Home"]

	local function createButton(text, y, callback)
		local btn = Instance.new("TextButton", home)
		btn.Size = UDim2.new(0, 220, 0, 35)
		btn.Position = UDim2.new(0, 20, 0, y)
		btn.Text = text
		btn.BackgroundColor3 = Color3.fromRGB(60, 0, 60)
		btn.TextColor3 = Color3.fromRGB(255, 0, 255)
		btn.Font = Enum.Font.GothamBold
		btn.TextSize = 14
		local corner = Instance.new("UICorner", btn)
		corner.CornerRadius = UDim.new(0, 8)
		btn.MouseButton1Click:Connect(callback)
	end

	createButton("🔍 ESP", 10, function()
		for _, v in pairs(Players:GetPlayers()) do
			if v ~= player then
				local tag = Instance.new("BillboardGui", v.Character:FindFirstChild("Head") or v.Character:FindFirstChildOfClass("Part"))
				tag.Size = UDim2.new(0, 100, 0, 40)
				tag.AlwaysOnTop = true
				local txt = Instance.new("TextLabel", tag)
				txt.Size = UDim2.new(1, 0, 1, 0)
				txt.BackgroundTransparency = 1
				txt.Text = v.Name
				txt.TextColor3 = Color3.new(1, 0, 0)
				txt.Font = Enum.Font.GothamBold
				txt.TextScaled = true
			end
		end
	end)

	createButton("🦅 Voar", 55, function()
		local flySpeed = 50
		local UIS = game:GetService("UserInputService")
		local flying = true
		local cam = workspace.CurrentCamera
		local bodyVel = Instance.new("BodyVelocity", player.Character:WaitForChild("HumanoidRootPart"))
		bodyVel.Velocity = Vector3.zero
		bodyVel.MaxForce = Vector3.new(9999, 9999, 9999)

		UIS.InputBegan:Connect(function(input)
			if flying then
				if input.KeyCode == Enum.KeyCode.W then
					bodyVel.Velocity = cam.CFrame.LookVector * flySpeed
				elseif input.KeyCode == Enum.KeyCode.S then
					bodyVel.Velocity = -cam.CFrame.LookVector * flySpeed
				elseif input.KeyCode == Enum.KeyCode.Space then
					bodyVel.Velocity = Vector3.new(0, flySpeed, 0)
				end
			end
		end)

		UIS.InputEnded:Connect(function(input)
			if flying then
				bodyVel.Velocity = Vector3.zero
			end
		end)
	end)

	createButton("🫥 Invisível", 100, function()
		for _, v in pairs(player.Character:GetDescendants()) do
			if v:IsA("BasePart") then v.Transparency = 1 end
			if v:IsA("Decal") then v.Transparency = 1 end
		end
	end)

	createButton("🧍 Teleport até Player", 145, function()
		local name = game:GetService("Players"):FindFirstChild(game:GetService("StarterGui"):PromptInput("Digite o nome do player"))
		if name and name.Character then
			player.Character:MoveTo(name.Character:FindFirstChild("HumanoidRootPart").Position)
		end
	end)
end

-- OUTFIT TAB CONTENT
do
	local outfit = contentFrames["Outfit"]

	local function createButton(text, y, applyFunc)
		local btn = Instance.new("TextButton", outfit)
		btn.Size = UDim2.new(0, 220, 0, 35)
		btn.Position = UDim2.new(0, 20, 0, y)
		btn.Text = text
		btn.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
		btn.TextColor3 = Color3.new(1, 1, 1)
		btn.Font = Enum.Font.GothamBold
		btn.TextSize = 14
		local corner = Instance.new("UICorner", btn)
		corner.CornerRadius = UDim.new(0, 8)
		btn.MouseButton1Click:Connect(applyFunc)
	end

	createButton("GRG Store (Cenoura)", 10, function()
		for _, v in pairs(player.Character:GetDescendants()) do
			if v:IsA("BasePart") then v.BrickColor = BrickColor.new("Bright orange") end
		end
	end)

	createButton("Tr1nX_777 (Macabro)", 55, function()
		for _, v in pairs(player.Character:GetDescendants()) do
			if v:IsA("BasePart") then v.Color = Color3.fromRGB(30, 0, 30) end
		end
	end)
end

-- RISK TAB
do
	local risk = contentFrames["Risk"]

	local title = Instance.new("TextLabel", risk)
	title.Size = UDim2.new(1, -20, 0, 40)
	title.Position = UDim2.new(0, 10, 0, 10)
	title.Text = "⚠️ Tr1nX_777 que domina"
	title.BackgroundTransparency = 1
	title.TextColor3 = Color3.fromRGB(255, 0, 0)
	title.Font = Enum.Font.GothamBlack
	title.TextSize = 20

	local moneyInput = Instance.new("TextBox", risk)
	moneyInput.Size = UDim2.new(0, 200, 0, 35)
	moneyInput.Position = UDim2.new(0, 20, 0, 60)
	moneyInput.PlaceholderText = "Quanto dinheiro deseja gerar?"
	moneyInput.Text = ""
	moneyInput.TextColor3 = Color3.new(1,1,1)
	moneyInput.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
	moneyInput.Font = Enum.Font.GothamBold
	moneyInput.TextSize = 14
	local moneyCorner = Instance.new("UICorner", moneyInput)
	moneyCorner.CornerRadius = UDim.new(0, 8)

	local confirmBtn = Instance.new("TextButton", risk)
	confirmBtn.Size = UDim2.new(0, 220, 0, 35)
	confirmBtn.Position = UDim2.new(0, 20, 0, 110)
	confirmBtn.Text = "💸 Sim, por conta em risco"
	confirmBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
	confirmBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
	confirmBtn.Font = Enum.Font.GothamBlack
	confirmBtn.TextSize = 14
	local confirmCorner = Instance.new("UICorner", confirmBtn)
	confirmCorner.CornerRadius = UDim.new(0, 8)

	confirmBtn.MouseButton1Click:Connect(function()
		local val = tonumber(moneyInput.Text)
		if val then
			print("Gerando dinheiro fake: R$"..val)
			-- Aqui você adicionaria lógica real se o jogo deixasse
		end
	end)
end
