local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local player = Players.LocalPlayer
local gui = player:WaitForChild("PlayerGui"):WaitForChild("InventoryGui") -- Ajuste se seu GUI tem outro nome
local seedInput = gui:WaitForChild("SeedInput")
local spawnButton = gui:WaitForChild("SpawnButton")
local seedItemsFrame = gui:WaitForChild("SeedItems")

local seedsFolder = ReplicatedStorage:WaitForChild("Seeds")

-- Função para calcular similaridade básica de strings (Levenshtein Distance simplificada)
local function stringSimilarity(s1, s2)
    s1 = s1:lower()
    s2 = s2:lower()
    local len1 = #s1
    local len2 = #s2

    local matrix = {}
    for i = 0, len1 do
        matrix[i] = {[0] = i}
    end
    for j = 0, len2 do
        matrix[0][j] = j
    end

    for i = 1, len1 do
        for j = 1, len2 do
            local cost = (s1:sub(i,i) == s2:sub(j,j)) and 0 or 1
            matrix[i][j] = math.min(
                matrix[i-1][j] + 1,
                matrix[i][j-1] + 1,
                matrix[i-1][j-1] + cost
            )
        end
    end

    local maxLen = math.max(len1, len2)
    if maxLen == 0 then
        return 1
    end
    -- Similaridade entre 0 e 1 (1 = igual, 0 = totalmente diferente)
    return 1 - (matrix[len1][len2] / maxLen)
end

-- Função para sugerir o nome mais parecido
local function suggestSeedName(name)
    local bestMatch = nil
    local bestScore = 0

    for _, seed in pairs(seedsFolder:GetChildren()) do
        local score = stringSimilarity(name, seed.Name)
        if score > bestScore then
            bestScore = score
            bestMatch = seed.Name
        end
    end

    -- Sugere só se for razoavelmente parecido (>0.5)
    if bestScore > 0.5 then
        return bestMatch
    else
        return nil
    end
end

local function addSeedToInventory(seedName)
    -- Verifica se já existe no inventário (evita duplicatas)
    for _, child in pairs(seedItemsFrame:GetChildren()) do
        if child.Name == seedName then
            warn("Semente já está no inventário!")
            return
        end
    end

    local seedTemplate = seedsFolder:FindFirstChild(seedName)
    if not seedTemplate then
        warn("Semente não encontrada para adicionar!")
        return
    end

    -- Clonar e colocar no inventário (assumindo que as sementes são GuiObjects)
    local clone = seedTemplate:Clone()
    clone.Parent = seedItemsFrame
    clone.Name = seedName
    print("Semente "..seedName.." adicionada ao inventário!")
end

spawnButton.MouseButton1Click:Connect(function()
    local inputName = seedInput.Text
    if inputName == "" then
        warn("Digite o nome da semente!")
        return
    end

    local seed = seedsFolder:FindFirstChild(inputName)

    if seed then
        -- Semente existe, adiciona direto
        addSeedToInventory(inputName)
    else
        -- Semente não existe, sugere nome parecido
        local suggestion = suggestSeedName(inputName)
        if suggestion then
            print("Semente não encontrada. Você quis dizer: "..suggestion.." ?")
            -- Você pode mostrar essa sugestão para o usuário via GUI, aqui só print pra teste
            -- Se quiser já adicionar a sugestão automaticamente, pode chamar:
            -- addSeedToInventory(suggestion)
        else
            warn("Nenhuma semente parecida encontrada!")
        end
    end
end)
