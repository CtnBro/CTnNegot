-- INSTÂNCIA INICIAL
local plr = game.Players.LocalPlayer
local rs = game:GetService("ReplicatedStorage")

-- FUNÇÃO: SUGESTÃO DE NOME PARECIDO
local function getClosestName(input, list)
	local inputLower = input:lower()
	local closest, shortest = nil, math.huge
	for _, obj in pairs(list) do
		local name = obj.Name:lower()
		local distance = math.abs(#name - #inputLower)
		if distance < shortest then
			shortest = distance
			closest = obj.Name
		end
	end
	return closest
end

-- CRIA GUI
local gui = Instance.new("ScreenGui", plr:WaitForChild("PlayerGui"))
gui.Name = "1nstaHuntersHub"

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0, 350, 0, 250)
main.Position = UDim2.new(0.3, 0, 0.3, 0)
main.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
main.Active, main.Draggable = true, true
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

-- INVENTÁRIO LOCAL DO PLAYER
local inv = plr:FindFirstChild("Inventory") or Instance.new("Folder", plr)
inv.Name = "Inventory"

local seedItems = inv:FindFirstChild("Seed Items") or Instance.new("Folder", inv)
seedItems.Name = "Seed Items"

-- BOTÃO DE SPAWN
spawnBtn.MouseButton1Click:Connect(function()
	local input = itemInput.Text
	if input == "" then
		feedbackLabel.Text = "❌ Digita o nome da semente."
		return
	end

	local seedFolder = rs:FindFirstChild("Seeds")
	if not seedFolder then
		feedbackLabel.Text = "❌ Pasta 'Seeds' não encontrada no jogo!"
		return
	end

	local seed = seedFolder:FindFirstChild(input)
	if seed then
		if seedItems:FindFirstChild(seed.Name) then
			feedbackLabel.Text = "✅ Você já tem a semente '" .. input .. "'!"
			return
		end
		local clone = seed:Clone()
		clone.Parent = seedItems
		feedbackLabel.Text = "✅ '" .. input .. "' adicionada à sua Seed Items!"
	else
		local similar = getClosestName(input, seedFolder:GetChildren())
		feedbackLabel.Text = "❌ '" .. input .. "' não existe. Você quis dizer: '" .. similar .. "'?"
	end
end)
