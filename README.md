-- Coloque em ServerScriptService

local Players = game:GetService("Players")

Players.PlayerAdded:Connect(function(player)

    -- Leaderstats
    local leaderstats = Instance.new("Folder")
    leaderstats.Name = "leaderstats"
    leaderstats.Parent = player

    local Level = Instance.new("IntValue")
    Level.Name = "Level"
    Level.Value = 1
    Level.Parent = leaderstats

    local XP = Instance.new("IntValue")
    XP.Name = "XP"
    XP.Value = 0
    XP.Parent = leaderstats

    -- Sistema automático
    while player.Parent do
        wait(3)

        XP.Value += 10

        -- Up de level
        if XP.Value >= 100 then
            XP.Value = 0
            Level.Value += 1
            print(player.Name .. " subiu para o level " .. Level.Value)
        end
    end
end)
-- ServerScriptService

local Players = game:GetService("Players")

Players.PlayerAdded:Connect(function(player)

    -- Leaderstats
    local leaderstats = Instance.new("Folder")
    leaderstats.Name = "leaderstats"
    leaderstats.Parent = player

    local Level = Instance.new("IntValue")
    Level.Name = "Level"
    Level.Value = 1
    Level.Parent = leaderstats

    local Coins = Instance.new("IntValue")
    Coins.Name = "Coins"
    Coins.Value = 0
    Coins.Parent = leaderstats

    -- Missão
    local Quest
    EnemyKilled
    -- Script dentro do NPC

local humanoid = script.Parent.Humanoid

humanoid.Died:Connect(function()

    local killer = humanoid:FindFirstChild("creator")

    if killer and killer.Value then
        game.ReplicatedStorage.EnemyKilled:FireServer(killer.Value)
    end
end)-- INVENTÁRIO COMPLETO
-- Coloque em ServerScriptService

local Players = game:GetService("Players")

Players.PlayerAdded:Connect(function(player)

    -- Pasta do inventário
    local Inventory = Instance.new("Folder")
    Inventory.Name = "Inventory"
    Inventory.Parent = player

    -- Espaços do inventário
    local Slots = Instance.new("IntValue")
    Slots.Name = "Slots"
    Slots.Value = 20
    Slots.Parent = Inventory

    -- Ouro
    local Gold = Instance.new("IntValue")
    Gold.Name = "Gold"
    Gold.Value = 0
    Gold.Parent = Inventory

end)

--------------------------------------------------
-- FUNÇÃO PARA ADICIONAR ITEM
--------------------------------------------------

function AddItem(player, itemName, rarity)

    local inventory = player:FindFirstChild("Inventory")

    if inventory then

        local item = Instance.new("StringValue")
        item.Name = itemName
        item.Value = rarity
        item.Parent = inventory

        print(itemName.." foi adicionado!")
    end
end

--------------------------------------------------
-- EXEMPLOS
--------------------------------------------------

game.Players.PlayerAdded:Connect(function(player)

    wait(3)

    AddItem(player,"Espada Comum","Comum")
    AddItem(player,"Armadura Rara","Raro")
    AddItem(player,"Fruta Mítica","Mítico")

end)-- LocalScript
-- StarterGui > ScreenGui

local player = game.Players.LocalPlayer
local inventory = player:WaitForChild("Inventory")

for _,item in pairs(inventory:GetChildren()) do

    if item:IsA("StringValue") then

        print("ITEM:", item.Name)
        print("RARIDADE:", item.Value)

    end
end-- SALVAR INVENTÁRIO COM DATASTORE
-- Coloque em ServerScriptService

local DataStoreService = game:GetService("DataStoreService")
local Players = game:GetService("Players")

local InventoryStore = DataStoreService:GetDataStore("InventoryData")

--------------------------------------------------
-- CRIAR INVENTÁRIO
---------------------------------------------------- SALVAR INVENTÁRIO COM DATASTORE
-- Coloque em ServerScriptService

local DataStoreService = game:GetService("DataStoreService")
local Players = game:GetService("Players")

local InventoryStore = DataStoreService:GetDataStore("InventoryData")

--------------------------------------------------
-- CRIAR INVENTÁRIO
--------------------------------------------------

Players.PlayerAdded:Connect(function(player)

    local Inventory = Instance.new("Folder")
    Inventory.Name = "Inventory"
    Inventory.Parent = player

    --------------------------------------------------
    -- CARREGAR DADOS
    --------------------------------------------------

    local success, data = pcall(function()
        return InventoryStore:GetAsync(player.UserId)
    end)

    if success and data then

        for _, itemData in pairs(data) do

            local item = Instance.new("StringValue")
            item.Name = itemData.Name
            item.Value = itemData.Rarity
            item.Parent = Inventory

        end

        print("Inventário carregado!")

    else
        print("Novo jogador ou erro ao carregar.")
    end
end)

--------------------------------------------------
-- FUNÇÃO ADICIONAR ITEM
--------------------------------------------------

function AddItem(player, itemName, rarity)

    local inventory = player:FindFirstChild("Inventory")

    if inventory then

        local item = Instance.new("StringValue")
        item.Name = itemName
        item.Value = rarity
        item.Parent = inventory

        print(itemName.." adicionado ao inventário!")
    end
end

--------------------------------------------------
-- SALVAR DADOS
--------------------------------------------------

Players.PlayerRemoving:Connect(function(player)

    local inventory = player:FindFirstChild("Inventory")

    if inventory then

        local itemsToSave = {}

        for _, item in pairs(inventory:GetChildren()) do

            if item:IsA("StringValue") then

                table.insert(itemsToSave,{
                    Name = item.Name,
                    Rarity = item.Value
                })

            end
        end

        local success, errorMessage = pcall(function()

            InventoryStore:SetAsync(player.UserId, itemsToSave)

        end)

        if success then
            print("Inventário salvo!")
        else
            warn("Erro ao salvar: "..errorMessage)
        end
    end
end)

EquipItem

-- LocalScript
-- Dentro de um TextButton

local player = game.Players.LocalPlayer

script.Parent.MouseButton1Click:Connect(function()

    print("Equipando item!")

    -- Nome do item
    local itemName = "Espada Lendária"

    game.ReplicatedStorage.EquipItem:FireServer(itemName)

end)game.ReplicatedStorage.EquipItem.OnServerEvent:Connect(function(player,itemName)

    local inventory = player:FindFirstChild("Inventory")

    if inventory and inventory:FindFirstChild(itemName) then

        player.EquippedItem.Value = itemName

        print(player.Name.." equipou "..itemName)

    end
end)
efeito de espada 

-- EFEITOS ESPECIAIS NOS ITENS
-- Coloque em ServerScriptService

local Players = game:GetService("Players")

--------------------------------------------------
-- FUNÇÃO DE EFEITO
--------------------------------------------------

function AddEffects(tool, rarity)

    local handle = tool:FindFirstChild("Handle")

    if handle then

        --------------------------------------------------
        -- PARTÍCULAS
        --------------------------------------------------

        local particle = Instance.new("ParticleEmitter")
        particle.Parent = handle

        particle.Rate = 25
        particle.Speed = NumberRange.new(2)

        --------------------------------------------------
        -- CORES POR RARIDADE
        --------------------------------------------------

        if rarity == "Raro" then

            handle.BrickColor = BrickColor.Blue()

            particle.Color = ColorSequence.new(Color3.fromRGB(0,170,255))

        elseif rarity == "Épico" then

            handle.BrickColor = BrickColor.Magenta()

            particle.Color = ColorSequence.new(Color3.fromRGB(170,0,255))

        elseif rarity == "Lendário" then

            handle.BrickColor = BrickColor.new("Bright yellow")

            particle.Color = ColorSequence.new(Color3.fromRGB(255,200,0))

            --------------------------------------------------
            -- LUZ
            --------------------------------------------------

            local light = Instance.new("PointLight")
            light.Parent = handle
            light.Brightness = 3
            light.Range = 10
            light.Color = Color3.fromRGB(255,200,0)

        elseif rarity == "Mítico" then

            handle.BrickColor = BrickColor.Red()

            particle.Color = ColorSequence.new(Color3.fromRGB(255,0,0))

            --------------------------------------------------
            -- FOGO
            --------------------------------------------------

            local fire = Instance.new("Fire")
            fire.Parent = handle
            fire.Size = 6

        end
    end
end

--------------------------------------------------
-- CRIAR ITEM COM EFEITO
--------------------------------------------------

function CreateWeapon(player, itemName, rarity)

    local tool = Instance.new("Tool")
    tool.Name = itemName

    local handle = Instance.new("Part")
    handle.Name = "Handle"
    handle.Size = Vector3.new(1,1,4)
    handle.Parent = tool

    -- Aplicar efeitos
    AddEffects(tool, rarity)

    tool.Parent = player.Backpack
end

--------------------------------------------------
-- TESTE
--------------------------------------------------

Players.PlayerAdded:Connect(function(player)

    wait(3)

    CreateWeapon(player,"Espada Mítica","Mítico")

end)-- ANIMAÇÃO DA ESPADA
-- Coloque dentro da Tool (Espada)

local tool = script.Parent
local Players = game:GetService("Players")

--------------------------------------------------
-- ID DA ANIMAÇÃO
--------------------------------------------------

local AnimationId = "rbxassetid://522635514"
-- Você pode trocar pelo ID da sua animação

--------------------------------------------------
-- EQUIPAR
--------------------------------------------------

tool.Equipped:Connect(function()

    local character = tool.Parent
    local humanoid = character:FindFirstChild("Humanoid")

    if humanoid then

        local animation = Instance.new("Animation")
        animation.AnimationId = AnimationId

        local animator = humanoid:FindFirstChildOfClass("Animator")

        if animator then

            local track = animator:LoadAnimation(animation)

            --------------------------------------------------
            -- ATAQUE
            --------------------------------------------------

            tool.Activated:Connect(function()

                track:Play()

            end)
        end
    end
end)
-- EFEITO AO ATACAR

tool.Activated:Connect(function()

    local handle = tool:FindFirstChild("Handle")

    if handle then

        --------------------------------------------------
        -- SOM
        --------------------------------------------------

        local sound = Instance.new("Sound")
        sound.Parent = handle
        sound.SoundId = "rbxassetid://12222216"
        sound.Volume = 1

        sound:Play()

        --------------------------------------------------
        -- RASTRO
        --------------------------------------------------

        local trail = Instance.new("Trail")
        trail.Parent = handle

        local att0 = Instance.new("Attachment")
        att0.Parent = handle

        local att1 = Instance.new("Attachment")
        att1.Position = Vector3.new(0,4,0)
        att1.Parent = handle

        trail.Attachment0 = att0
        trail.Attachment1 = att1

        wait(0.5)

        trail:Destroy()
    end
end)
-- EFEITO AO ATACAR

tool.Activated:Connect(function()

    local handle = tool:FindFirstChild("Handle")

    if handle then

        --------------------------------------------------
        -- SOM
        --------------------------------------------------

        local sound = Instance.new("Sound")
        sound.Parent = handle
        sound.SoundId = "rbxassetid://12222216"
        sound.Volume = 1

        sound:Play()

        --------------------------------------------------
        -- RASTRO
        --------------------------------------------------

        local trail = Instance.new("Trail")
        trail.Parent = handle

        local att0 = Instance.new("Attachment")
        att0.Parent = handle

        local att1 = Instance.new("Attachment")
        att1.Position = Vector3.new(0,4,0)
        att1.Parent = handle

        trail.Attachment0 = att0
        trail.Attachment1 = att1

        wait(0.5)

        trail:Destroy()
    end
    -- SISTEMA DE EVOLUÇÃO
-- Coloque em ServerScriptService

local Players = game:GetService("Players")

--------------------------------------------------
-- CONFIGURAÇÕES
--------------------------------------------------

local Evolutions = {

    ["Espada Comum"] = {
        Next = "Espada Rara",
        NeedLevel = 5
    },

    ["Espada Rara"] = {
        Next = "Espada Épica",
        NeedLevel = 10
    },

    ["Espada Épica"] = {
        Next = "Espada Lendária",
        NeedLevel = 20
    },

    ["Espada Lendária"] = {
        Next = "Espada Mítica",
        NeedLevel = 35
    }
}

--------------------------------------------------
-- EVOLUIR ITEM
--------------------------------------------------

function EvolveItem(player,itemName)

    local inventory = player:FindFirstChild("Inventory")

    if inventory then

        local item = inventory:FindFirstChild(itemName)

        if item and Evolutions[itemName] then

            local evolutionData = Evolutions[itemName]

            local playerLevel = player.leaderstats.Level.Value

            --------------------------------------------------
            -- VERIFICAR LEVEL
            --------------------------------------------------

            if playerLevel >= evolutionData.NeedLevel then

                --------------------------------------------------
                -- REMOVER ITEM ANTIGO
                --------------------------------------------------

                item:Destroy()

                --------------------------------------------------
                -- NOVO ITEM
                --------------------------------------------------

                local newItem = Instance.new("StringValue")
                newItem.Name = evolutionData.Next
                newItem.Value = "Evoluído"
                newItem.Parent = inventory

                print(player.Name.." evoluiu para "..evolutionData.Next)

            else

                print("Level insuficiente!")

            end
        end
    end
end

--------------------------------------------------
-- TESTE
--------------------------------------------------

Players.PlayerAdded:Connect(function(player)

    --------------------------------------------------
    -- LEADERSTATS
    --------------------------------------------------

    local leaderstats = Instance.new("Folder")
    leaderstats.Name = "leaderstats"
    leaderstats.Parent = player

    local Level = Instance.new("IntValue")
    Level.Name = "Level"
    Level.Value = 25
    Level.Parent = leaderstats

    --------------------------------------------------
    -- INVENTÁRIO
    --------------------------------------------------

    local Inventory = Instance.new("Folder")
    Inventory.Name = "Inventory"
    Inventory.Parent = player

    local Sword = Instance.new("StringValue")
    Sword.Name = "Espada Épica"
    Sword.Value = "Épico"
    Sword.Parent = Inventory

    wait(5)

    --------------------------------------------------
    -- EVOLUIR
    --------------------------------------------------

    EvolveItem(player,"Espada Épica")

end)
end)
