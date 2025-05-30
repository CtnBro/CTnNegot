--[[  
1NSTA HUNTERS V1 💀  
PAINEL DE SPAWN + GIFT DE PET 🔥🚩  
by: @1nsta  
]]

-- services
local plr = game.Players.LocalPlayer
local uis = game:GetService("UserInputService")
local rs = game:GetService("ReplicatedStorage")
local tween = game:GetService("TweenService")

-- instancia GUI
local gui = Instance.new("ScreenGui", plr.PlayerGui)
gui.Name = "1nstaHuntersHub"

-- main frame
local main = Instance.new("Frame", gui)
main.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
main.BorderSizePixel = 0
main.Size = UDim2.new(0, 350, 0, 250)
main.Position = UDim2.new(0.3, 0, 0.3, 0)
main.Active = true
main.Draggable = true

local UICorner = Instance.new("UICorner", main)
UICorner.CornerRadius = UDim.new(0, 10)

local title = Instance.new("TextLabel", main)
title.Text = "1NSTA HUNTERS HUB 🔥"
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 20
local UICorner2 = Instance.new("UICorner", title)
UICorner2.CornerRadius = UDim.new(0, 10)

-- input item
local itemLabel = Instance.new("TextLabel", main)
itemLabel.Text = "Item/Pet pra Spawn"
itemLabel.Position = UDim2.new(0, 10, 0, 50)
itemLabel.Size = UDim2.new(0, 150, 0, 25)
itemLabel.BackgroundTransparency = 1
itemLabel.TextColor3 = Color3.new(1, 1, 1)
itemLabel.Font = Enum.Font.GothamBold
itemLabel.TextSize = 14

local itemInput = Instance.new("TextBox", main)
itemInput.PlaceholderText = "ex: Tomato ou Dog"
itemInput.Position = UDim2.new(0, 10, 0, 80)
itemInput.Size = UDim2.new(0, 150, 0, 30)
itemInput.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
itemInput.TextColor3 = Color3.new(1, 1, 1)
itemInput.Font = Enum.Font.GothamBold
itemInput.TextSize = 14
Instance.new("UICorner", itemInput)

-- input player
local playerLabel = Instance.new("TextLabel", main)
playerLabel.Text = "Player pra Gift"
playerLabel.Position = UDim2.new(0, 190, 0, 50)
playerLabel.Size = UDim2.new(0, 150, 0, 25)
playerLabel.BackgroundTransparency = 1
playerLabel.TextColor3 = Color3.new(1, 1, 1)
playerLabel.Font = Enum.Font.GothamBold
playerLabel.TextSize = 14

local playerInput = Instance.new("TextBox", main)
playerInput.PlaceholderText = "Nick do mlk"
playerInput.Position = UDim2.new(0, 190, 0, 80)
playerInput.Size = UDim2.new(0, 150, 0, 30)
playerInput.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
playerInput.TextColor3 = Color3.new(1, 1, 1)
playerInput.Font = Enum.Font.GothamBold
playerInput.TextSize = 14
Instance.new("UICorner", playerInput)

-- botao spawn
local spawnBtn = Instance.new("TextButton", main)
spawnBtn.Text = "SPAWN ITEM 🌱"
spawnBtn.Position = UDim2.new(0, 10, 0, 130)
spawnBtn.Size = UDim2.new(0, 150, 0, 40)
spawnBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
spawnBtn.TextColor3 = Color3.new(1, 1, 1)
spawnBtn.Font = Enum.Font.GothamBold
spawnBtn.TextSize = 16
Instance.new("UICorner", spawnBtn)

-- botao gift
local giftBtn = Instance.new("TextButton", main)
giftBtn.Text = "GIFT PET 🎁"
giftBtn.Position = UDim2.new(0, 190, 0, 130)
giftBtn.Size = UDim2.new(0, 150, 0, 40)
giftBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
giftBtn.TextColor3 = Color3.new(1, 1, 1)
giftBtn.Font = Enum.Font.GothamBold
giftBtn.TextSize = 16
Instance.new("UICorner", giftBtn)

-- creditos
local cred = Instance.new("TextLabel", main)
cred.Text = "by 1NSTA HUNTERS 🚩"
cred.Size = UDim2.new(1, 0, 0, 25)
cred.Position = UDim2.new(0, 0, 1, -25)
cred.BackgroundTransparency = 1
cred.TextColor3 = Color3.fromRGB(255, 255, 255)
cred.Font = Enum.Font.GothamBold
cred.TextSize = 12

-----------------------------------------
-- funções brabas

spawnBtn.MouseButton1Click:Connect(function()
    local item = itemInput.Text
    if item == "" then
        warn("digita o nome do item burrão")
        return
    end
    --// firesignal no botão do jogo
    local gui = plr.PlayerGui:WaitForChild("MainUI")
    local inputBox = gui:WaitForChild("SeedInput")
    local checkerButton = gui:WaitForChild("CheckerButton")
    local spawnButton = gui:WaitForChild("SpawnButton")

    inputBox.Text = item
    firesignal(checkerButton.MouseButton1Click)
    wait(0.5)
    firesignal(spawnButton.MouseButton1Click)

    print("🌱 SPAWN DO ITEM '" .. item .. "' FEITO MLK 🔥")
end)

giftBtn.MouseButton1Click:Connect(function()
    local item = itemInput.Text
    local alvo = playerInput.Text
    if item == "" or alvo == "" then
        warn("digita o item e o player burrão")
        return
    end

    local args = {
        [1] = alvo,
        [2] = item
    }

    rs.RemoteEvent.Gift:FireServer(unpack(args))

    print("🎁 PET '" .. item .. "' FOI PRO MLK " .. alvo .. " COM SUCESSO KKKK 💀")
end)
