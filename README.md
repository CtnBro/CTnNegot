--[[  
1NSTA HUNTERS V1 💀  
PAINEL DE SPAWN + GIFT DE PET 🔥🚩  
by: @1nsta  
]]

-- serviços
local plr = game.Players.LocalPlayer
local rs = game:GetService("ReplicatedStorage")

-- onde estão as sementes
local seedFolder = rs:WaitForChild("Seeds")

-- função pra buscar nome parecido
local function getClosestName(input)
	local inputLower = input:lower()
	local closest = nil
	local shortest = math.huge
	for _, seed in pairs(seedFolder:GetChildren()) do
		local name = seed.Name:lower()
		local distance = math.abs(#name - #inputLower)
		if distance < shortest then
			shortest = distance
			closest = seed.Name
		end
	end
	return closest
end

-- cria GUI
local gui = Instance.new("ScreenGui", plr.PlayerGui)
gui.Name = "1nstaHuntersHub"

local main = Instance.new("Frame", gui)
main.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
main.BorderSizePixel = 0
main.Size = UDim2.new(0, 350, 0, 250)
main.Position = UDim2.new(0.3, 0, 0.3, 0)
main.Active = true
main.Draggable = true
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)

local title = Instance.new("TextLabel", main)
title.Text = "1NSTA HUNTERS HUB 🔥"
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
title.TextColor3 = Color3.new(1, 1, 1)
title.Font = Enum.Font.GothamBold
title.TextSize = 20
Instance.new("UICorner", title).CornerRadius = UDim.new(0, 10)

local itemLabel = Instance.new("TextLabel", main)
itemLabel.Text = "Semente pra Spawn"
itemLabel.Position = UDim2.new(0, 10, 0, 50)
itemLabel.Size = UDim2.new(0, 150, 0, 25)
itemLabel.BackgroundTransparency = 1
itemLabel.TextColor3 = Color3.new(1, 1, 1)
itemLabel.Font = Enum.Font.GothamBold
itemLabel.TextSize = 14

local itemInput = Instance.new("TextBox", main)
itemInput.PlaceholderText = "ex: Candy Blossom"
itemInput.Position = UDim2.new(0, 10, 0, 80)
itemInput.Size = UDim2.new(0, 150, 0, 30)
itemInput.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
itemInput.TextColor3 = Color3.new(1, 1, 1)
itemInput.Font = Enum.Font.GothamBold
itemInput.TextSize = 14
Instance.new("UICorner", itemInput)

local feedbackLabel = Instance.new("TextLabel", main)
feedbackLabel.Text = ""
feedbackLabel.Position = UDim2.new(0, 10, 0, 115)
feedbackLabel.Size = UDim2.new(1, -20, 0, 20)
feedbackLabel.BackgroundTransparency = 1
feedbackLabel.TextColor3 = Color3.new(1, 1, 1)
feedbackLabel.Font = Enum.Font.Gotham
feedbackLabel.TextSize = 14
feedbackLabel.TextWrapped = true

local spawnBtn = Instance.new("TextButton", main)
spawnBtn.Text = "SPAWN SEMENTE 🌱"
spawnBtn.Position = UDim2.new(0, 10, 0, 140)
spawnBtn.Size = UDim2.new(0, 150, 0, 40)
spawnBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
spawnBtn.TextColor3 = Color3.new(1, 1, 1)
spawnBtn.Font = Enum.Font.GothamBold
spawnBtn.TextSize = 16
Instance.new("UICorner", spawnBtn)

-- INVENTÁRIO LOCAL (cliente)
local inventory = plr:FindFirstChild("Inventory") or Instance.new("Folder", plr)
inventory.Name = "Inventory"

local seedItems = inventory:FindFirstChild("Seed Items") or Instance.new("Folder", inventory)
seedItems.Name = "Seed Items"

-- Ação do botão
spawnBtn.MouseButton1Click:Connect(function()
	local input = itemInput.Text
	if input == "" then
		feedbackLabel.Text = "❌ Digita o nome da semente."
		return
	end

	local foundSeed = seedFolder:FindFirstChild(input)
	if foundSeed then
		local alreadyOwned = seedItems:FindFirstChild(foundSeed.Name)
		if alreadyOwned then
			feedbackLabel.Text = "✅ Você já tem a semente '" .. input .. "'!"
			return
		end

		local clone = foundSeed:Clone()
		clone.Parent = seedItems
		feedbackLabel.Text = "✅ '" .. input .. "' adicionada em 'Seed Items'!"
	else
		local suggestion = getClosestName(input)
		feedbackLabel.Text = "❌ Não achei '" .. input .. "'. Você quis dizer: '" .. suggestion .. "'?"
	end
end)
