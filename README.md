--[[
    Blox Fruits Teleport Script
    Este script permite teleportar para locais importantes no jogo Blox Fruits
]]

-- Configurações
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Interface do usuário
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "TeleportGUI"
ScreenGui.Parent = player.PlayerGui

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 200, 0, 300)
Frame.Position = UDim2.new(0.85, 0, 0.5, -150)
Frame.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.Parent = ScreenGui

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Text = "Blox Fruits Teleport"
Title.TextSize = 18
Title.Font = Enum.Font.SourceSansBold
Title.Parent = Frame

local ScrollingFrame = Instance.new("ScrollingFrame")
ScrollingFrame.Position = UDim2.new(0, 0, 0, 35)
ScrollingFrame.Size = UDim2.new(1, 0, 1, -40)
ScrollingFrame.BackgroundTransparency = 1
ScrollingFrame.ScrollBarThickness = 6
ScrollingFrame.Parent = Frame

-- Lista de locais importantes no Blox Fruits
local locations = {
    {name = "Starter Island", position = Vector3.new(1071.2832, 16.3085976, 1426.86792)},
    {name = "Marine Start", position = Vector3.new(-2573.3374, 6.88881969, 2046.99817)},
    {name = "Middle Town", position = Vector3.new(-655.824158, 7.88708115, 1436.67908)},
    {name = "Jungle", position = Vector3.new(-1249.77222, 11.8870859, 341.356476)},
    {name = "Pirate Village", position = Vector3.new(-1122.34998, 4.78708982, 3855.91992)},
    {name = "Desert", position = Vector3.new(1094.14587, 6.5, 4192.88721)},
    {name = "Frozen Village", position = Vector3.new(1198.00928, 27.0074959, -1211.73376)},
    {name = "MarineFord", position = Vector3.new(-4505.375, 20.687294, 4260.55908)},
    {name = "Colosseum", position = Vector3.new(-1428.35474, 7.38933945, -3014.37305)},
    {name = "Sky Island 1", position = Vector3.new(-4970.21875, 717.707275, -2622.35449)},
    {name = "Sky Island 2", position = Vector3.new(-4813.0249, 903.708557, -1912.69055)},
    {name = "Sky Island 3", position = Vector3.new(-7952.31006, 5545.52832, -320.704956)},
    {name = "Prison", position = Vector3.new(4854.16455, 5.68742752, 740.194641)},
    {name = "Magma Village", position = Vector3.new(-5231.75879, 8.61593437, 8467.87695)},
    {name = "Underwater City", position = Vector3.new(61163.8516, 11.7796879, 1819.78418)},
    {name = "Fountain City", position = Vector3.new(5132.93506, 4.53632832, 4037.83252)},
    {name = "House of Davy Jones", position = Vector3.new(-9508.00977, 142.104858, 5737.0293)}
}

-- Criar botões para cada local
for i, location in ipairs(locations) do
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(0.9, 0, 0, 30)
    Button.Position = UDim2.new(0.05, 0, 0, (i-1) * 35)
    Button.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Text = location.name
    Button.TextSize = 14
    Button.Font = Enum.Font.SourceSans
    Button.Parent = ScrollingFrame
    
    -- Adicionar efeito de hover
    Button.MouseEnter:Connect(function()
        Button.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
    end)
    
    Button.MouseLeave:Connect(function()
        Button.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    end)
    
    -- Função de teleporte
    Button.MouseButton1Click:Connect(function()
        -- Verificar se o jogador está no jogo
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            -- Efeito visual de teleporte
            local effect = Instance.new("Part")
            effect.Shape = Enum.PartType.Ball
            effect.Size = Vector3.new(5, 5, 5)
            effect.Material = Enum.Material.Neon
            effect.Color = Color3.fromRGB(0, 255, 255)
            effect.CFrame = humanoidRootPart.CFrame
            effect.Anchored = true
            effect.CanCollide = false
            effect.Transparency = 0.5
            effect.Parent = workspace
            
            -- Animação do efeito
            game:GetService("TweenService"):Create(
                effect,
                TweenInfo.new(1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
                {Size = Vector3.new(0, 0, 0), Transparency = 1}
            ):Play()
            
            -- Remover o efeito após a animação
            game:GetService("Debris"):AddItem(effect, 1)
            
            -- Teleportar o jogador
            humanoidRootPart.CFrame = CFrame.new(location.position)
            
            -- Notificação
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "Teleporte",
                Text = "Teleportado para " .. location.name,
                Duration = 3
            })
        end
    end)
end

-- Ajustar o tamanho do ScrollingFrame
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, (#locations * 35))

-- Botão para fechar/abrir a interface
local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0, 40, 0, 40)
ToggleButton.Position = UDim2.new(0, 10, 0.5, -20)
ToggleButton.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.Text = "TP"
ToggleButton.TextSize = 16
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.Parent = ScreenGui

-- Arredondar os cantos
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = Frame

local UICorner2 = Instance.new("UICorner")
UICorner2.CornerRadius = UDim.new(0, 8)
UICorner2.Parent = ToggleButton

-- Função para alternar a visibilidade da interface
local guiVisible = true
ToggleButton.MouseButton1Click:Connect(function()
    guiVisible = not guiVisible
    Frame.Visible = guiVisible
end)

-- Mensagem de inicialização
print("Script de teleporte do Blox Fruits carregado com sucesso!")
game:GetService("StarterGui"):SetCore("SendNotification", {
    Title = "Script Ativado",
    Text = "Teleporte do Blox Fruits está pronto para uso!",
    Duration = 5
})
--[[
    Blox Fruits Finder
    Este script detecta frutas próximas no jogo Blox Fruits e notifica o jogador
]]

-- Configurações
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Configurações do detector
local SCAN_RADIUS = 300 -- Raio de detecção em studs
local SCAN_INTERVAL = 3 -- Intervalo entre verificações em segundos
local NOTIFICATION_SOUND_ID = "rbxassetid://6895079853" -- Som de alerta

-- Interface do usuário
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "FruitFinderGUI"
ScreenGui.Parent = player.PlayerGui

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 250, 0, 150)
Frame.Position = UDim2.new(0.85, -250, 0.2, 0)
Frame.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.Parent = ScreenGui

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Text = "Detector de Frutas"
Title.TextSize = 18
Title.Font = Enum.Font.SourceSansBold
Title.Parent = Frame

local StatusLabel = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 30)
StatusLabel.Position = UDim2.new(0, 0, 0, 35)
StatusLabel.Size = UDim2.new(1, 0, 0, 25)
StatusLabel.BackgroundTransparency = 1
StatusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
StatusLabel.Text = "Procurando frutas..."
StatusLabel.TextSize = 14
StatusLabel.Font = Enum.Font.SourceSans
StatusLabel.Parent = Frame

local FruitsList = Instance.new("TextLabel")
FruitsList.Position = UDim2.new(0, 10, 0, 65)
FruitsList.Size = UDim2.new(1, -20, 1, -70)
FruitsList.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
FruitsList.TextColor3 = Color3.fromRGB(255, 255, 255)
FruitsList.Text = "Nenhuma fruta encontrada"
FruitsList.TextSize = 14
FruitsList.Font = Enum.Font.SourceSans
FruitsList.TextXAlignment = Enum.TextXAlignment.Left
FruitsList.TextYAlignment = Enum.TextYAlignment.Top
FruitsList.TextWrapped = true
FruitsList.Parent = Frame

-- Arredondar os cantos
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = Frame

local UICorner2 = Instance.new("UICorner")
UICorner2.CornerRadius = UDim.new(0, 4)
UICorner2.Parent = FruitsList

-- Botão para fechar/abrir a interface
local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0, 40, 0, 40)
ToggleButton.Position = UDim2.new(0, 10, 0.2, 0)
ToggleButton.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.Text = "🍎"
ToggleButton.TextSize = 20
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.Parent = ScreenGui

local UICorner3 = Instance.new("UICorner")
UICorner3.CornerRadius = UDim.new(0, 8)
UICorner3.Parent = ToggleButton

-- Função para alternar a visibilidade da interface
local guiVisible = true
ToggleButton.MouseButton1Click:Connect(function()
    guiVisible = not guiVisible
    Frame.Visible = guiVisible
end)

-- Função para tocar som de alerta
local function playAlertSound()
    local sound = Instance.new("Sound")
    sound.SoundId = NOTIFICATION_SOUND_ID
    sound.Volume = 1
    sound.Parent = game:GetService("SoundService")
    sound:Play()
    game:GetService("Debris"):AddItem(sound, 3)
end

-- Função para verificar se um objeto é uma fruta
local function isFruit(obj)
    -- Verificar se o objeto tem características de uma fruta do Blox Fruits
    if obj:IsA("Tool") or obj:IsA("Model") then
        -- Verificar nome (frutas geralmente têm "Fruit" no nome)
        local name = obj.Name:lower()
        if name:find("fruit") or name:match("quake") or name:match("human") or name:match("string") or
           name:match("bird") or name:match("rumble") or name:match("paw") or name:match("gravity") or
           name:match("dough") or name:match("shadow") or name:match("venom") or name:match("control") or
           name:match("soul") or name:match("dragon") then
            return true
        end
        
        -- Verificar por outras características comuns de frutas
        if obj:IsA("Model") and obj:FindFirstChild("Handle") then
            return true
        end
    end
    return false
end

-- Função para calcular a distância entre dois pontos
local function getDistance(pos1, pos2)
    return (pos1 - pos2).Magnitude
end

-- Função para escanear frutas próximas
local foundFruits = {}
local function scanForFruits()
    -- Limpar lista anterior
    foundFruits = {}
    
    -- Verificar todos os objetos no workspace
    for _, obj in pairs(workspace:GetDescendants()) do
        if isFruit(obj) then
            local fruitPos
            if obj:IsA("Tool") and obj:FindFirstChild("Handle") then
                fruitPos = obj.Handle.Position
            elseif obj:IsA("Model") and obj:FindFirstChild("Handle") then
                fruitPos = obj.Handle.Position
            elseif obj:FindFirstChild("HumanoidRootPart") then
                fruitPos = obj.HumanoidRootPart.Position
            end
            
            if fruitPos then
                local distance = getDistance(humanoidRootPart.Position, fruitPos)
                if distance <= SCAN_RADIUS then
                    table.insert(foundFruits, {
                        name = obj.Name,
                        distance = math.floor(distance),
                        position = fruitPos
                    })
                end
            end
        end
    end
    
    -- Ordenar por distância
    table.sort(foundFruits, function(a, b)
        return a.distance < b.distance
    end)
    
    -- Atualizar interface
    updateFruitsList()
    
    -- Notificar se encontrou frutas
    if #foundFruits > 0 and not lastNotified then
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Fruta Encontrada!",
            Text = foundFruits[1].name .. " a " .. foundFruits[1].distance .. " studs",
            Duration = 5
        })
        playAlertSound()
        lastNotified = true
    elseif #foundFruits == 0 then
        lastNotified = false
    end
end

-- Função para atualizar a lista de frutas na interface
function updateFruitsList()
    if #foundFruits == 0 then
        FruitsList.Text = "Nenhuma fruta encontrada"
        StatusLabel.Text = "Procurando frutas..."
        StatusLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    else
        StatusLabel.Text = "Frutas encontradas: " .. #foundFruits
        StatusLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
        
        local listText = ""
        for i, fruit in ipairs(foundFruits) do
            listText = listText .. fruit.name .. " - " .. fruit.distance .. " studs\n"
            
            -- Criar marcador visual para a fruta
            createFruitMarker(fruit)
        end
        FruitsList.Text = listText
    end
end

-- Função para criar marcadores visuais para as frutas
local markers = {}
function createFruitMarker(fruit)
    -- Remover marcador antigo se existir
    for i, marker in ipairs(markers) do
        if marker.fruit.name == fruit.name and marker.fruit.position == fruit.position then
            marker.billboard:Destroy()
            table.remove(markers, i)
            break
        end
    end
    
    -- Criar novo marcador
    local billboard = Instance.new("BillboardGui")
    billboard.Size = UDim2.new(0, 100, 0, 40)
    billboard.StudsOffset = Vector3.new(0, 2, 0)
    billboard.Adornee = workspace:FindFirstChild(fruit.name)
    billboard.AlwaysOnTop = true
    
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 1, 0)
    frame.BackgroundTransparency = 0.5
    frame.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    frame.Parent = billboard
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = frame
    
    local nameLabel = Instance.new("TextLabel")
    nameLabel.Size = UDim2.new(1, 0, 0.5, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    nameLabel.TextStrokeTransparency = 0
    nameLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    nameLabel.Text = fruit.name
    nameLabel.TextSize = 14
    nameLabel.Font = Enum.Font.SourceSansBold
    nameLabel.Parent = billboard
    
    local distanceLabel = Instance.new("TextLabel")
    distanceLabel.Size = UDim2.new(1, 0, 0.5, 0)
    distanceLabel.Position = UDim2.new(0, 0, 0.5, 0)
    distanceLabel.BackgroundTransparency = 1
    distanceLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    distanceLabel.TextStrokeTransparency = 0
    distanceLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    distanceLabel.Text = fruit.distance .. " studs"
    distanceLabel.TextSize = 12
    distanceLabel.Font = Enum.Font.SourceSans
    distanceLabel.Parent = billboard
    
    billboard.Parent = player.PlayerGui
    
    -- Adicionar à lista de marcadores
    table.insert(markers, {
        billboard = billboard,
        fruit = fruit
    })
    
    -- Remover após 10 segundos se não for atualizado
    game:GetService("Debris"):AddItem(billboard, 10)
end

-- Função para teleportar para a fruta mais próxima
local function teleportToNearestFruit()
    if #foundFruits > 0 then
        local nearestFruit = foundFruits[1]
        humanoidRootPart.CFrame = CFrame.new(nearestFruit.position) + Vector3.new(0, 5, 0)
        
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Teleporte",
            Text = "Teleportado para " .. nearestFruit.name,
            Duration = 3
        })
    else
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Erro",
            Text = "Nenhuma fruta encontrada para teleportar",
            Duration = 3
        })
    end
end

-- Adicionar botão de teleporte
local TeleportButton = Instance.new("TextButton")
TeleportButton.Position = UDim2.new(0, 10, 1, -35)
TeleportButton.Size = UDim2.new(1, -20, 0, 25)
TeleportButton.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
TeleportButton.TextColor3 = Color3.fromRGB(255, 255, 255)
TeleportButton.Text = "Teleportar para Fruta Mais Próxima"
TeleportButton.TextSize = 14
TeleportButton.Font = Enum.Font.SourceSansBold
TeleportButton.Parent = Frame
TeleportButton.Visible = false -- Inicialmente oculto

local UICorner4 = Instance.new("UICorner")
UICorner4.CornerRadius = UDim.new(0, 4)
UICorner4.Parent = TeleportButton

-- Mostrar botão de teleporte quando frutas forem encontradas
game:GetService("RunService").Heartbeat:Connect(function()
    TeleportButton.Visible = #foundFruits > 0
end)

-- Conectar função de teleporte ao botão
TeleportButton.MouseButton1Click:Connect(teleportToNearestFruit)

-- Iniciar escaneamento periódico
while wait(SCAN_INTERVAL) do
    pcall(function()
        scanForFruits()
    end)
end
--[[
    Blox Fruits Auto Farm
    Este script permite auto-farm de mobs no jogo Blox Fruits
]]

-- Configurações
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")

-- Configurações do auto-farm
local AUTO_FARM_ENABLED = false
local TARGET_MOB_NAME = "Bandit" -- Mob padrão para iniciantes
local FARM_RADIUS = 100 -- Raio para procurar mobs
local ATTACK_DISTANCE = 10 -- Distância para atacar
local ATTACK_DELAY = 0.1 -- Delay entre ataques

-- Interface do usuário
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AutoFarmGUI"
ScreenGui.Parent = player.PlayerGui

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 250, 0, 300)
Frame.Position = UDim2.new(0.15, 0, 0.5, -150)
Frame.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.Parent = ScreenGui

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Text = "Auto Farm Blox Fruits"
Title.TextSize = 18
Title.Font = Enum.Font.SourceSansBold
Title.Parent = Frame

local StatusLabel = Instance.new("TextLabel")
StatusLabel.Position = UDim2.new(0, 0, 0, 35)
StatusLabel.Size = UDim2.new(1, 0, 0, 25)
StatusLabel.BackgroundTransparency = 1
StatusLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
StatusLabel.Text = "Status: Desativado"
StatusLabel.TextSize = 14
StatusLabel.Font = Enum.Font.SourceSans
StatusLabel.Parent = Frame

local TargetLabel = Instance.new("TextLabel")
TargetLabel.Position = UDim2.new(0, 10, 0, 65)
TargetLabel.Size = UDim2.new(1, -20, 0, 25)
TargetLabel.BackgroundTransparency = 1
TargetLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TargetLabel.Text = "Alvo: " .. TARGET_MOB_NAME
TargetLabel.TextSize = 14
TargetLabel.Font = Enum.Font.SourceSans
TargetLabel.TextXAlignment = Enum.TextXAlignment.Left
TargetLabel.Parent = Frame

local StatsFrame = Instance.new("Frame")
StatsFrame.Position = UDim2.new(0, 10, 0, 95)
StatsFrame.Size = UDim2.new(1, -20, 0, 100)
StatsFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
StatsFrame.BorderSizePixel = 0
StatsFrame.Parent = Frame

local StatsTitle = Instance.new("TextLabel")
StatsTitle.Position = UDim2.new(0, 0, 0, 5)
StatsTitle.Size = UDim2.new(1, 0, 0, 20)
StatsTitle.BackgroundTransparency = 1
StatsTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
StatsTitle.Text = "Estatísticas de Farm"
StatsTitle.TextSize = 14
StatsTitle.Font = Enum.Font.SourceSansBold
StatsTitle.Parent = StatsFrame

local KillsLabel = Instance.new("TextLabel")
KillsLabel.Position = UDim2.new(0, 10, 0, 30)
KillsLabel.Size = UDim2.new(1, -20, 0, 20)
KillsLabel.BackgroundTransparency = 1
KillsLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
KillsLabel.Text = "Inimigos derrotados: 0"
KillsLabel.TextSize = 14
KillsLabel.Font = Enum.Font.SourceSans
KillsLabel.TextXAlignment = Enum.TextXAlignment.Left
KillsLabel.Parent = StatsFrame

local TimeLabel = Instance.new("TextLabel")
TimeLabel.Position = UDim2.new(0, 10, 0, 50)
TimeLabel.Size = UDim2.new(1, -20, 0, 20)
TimeLabel.BackgroundTransparency = 1
TimeLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TimeLabel.Text = "Tempo de farm: 00:00:00"
TimeLabel.TextSize = 14
TimeLabel.Font = Enum.Font.SourceSans
TimeLabel.TextXAlignment = Enum.TextXAlignment.Left
TimeLabel.Parent = StatsFrame

local BelliLabel = Instance.new("TextLabel")
BelliLabel.Position = UDim2.new(0, 10, 0, 70)
BelliLabel.Size = UDim2.new(1, -20, 0, 20)
BelliLabel.BackgroundTransparency = 1
BelliLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
BelliLabel.Text = "Beli ganho: 0"
BelliLabel.TextSize = 14
BelliLabel.Font = Enum.Font.SourceSans
BelliLabel.TextXAlignment = Enum.TextXAlignment.Left
BelliLabel.Parent = StatsFrame

-- Botão para ativar/desativar auto-farm
local ToggleFarmButton = Instance.new("TextButton")
ToggleFarmButton.Position = UDim2.new(0, 10, 0, 205)
ToggleFarmButton.Size = UDim2.new(1, -20, 0, 30)
ToggleFarmButton.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
ToggleFarmButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleFarmButton.Text = "Iniciar Auto Farm"
ToggleFarmButton.TextSize = 16
ToggleFarmButton.Font = Enum.Font.SourceSansBold
ToggleFarmButton.Parent = Frame

-- Dropdown para selecionar o mob alvo
local TargetDropdown = Instance.new("TextButton")
TargetDropdown.Position = UDim2.new(0, 10, 0, 245)
TargetDropdown.Size = UDim2.new(1, -20, 0, 30)
TargetDropdown.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
TargetDropdown.TextColor3 = Color3.fromRGB(255, 255, 255)
TargetDropdown.Text = "Selecionar Alvo"
TargetDropdown.TextSize = 16
TargetDropdown.Font = Enum.Font.SourceSansBold
TargetDropdown.Parent = Frame

-- Lista de mobs comuns no Blox Fruits
local commonMobs = {
    "Bandit",
    "Monkey",
    "Gorilla",
    "Pirate",
    "Brute",
    "Desert Bandit",
    "Desert Officer",
    "Snow Bandit",
    "Snowman",
    "Chief Petty Officer",
    "Sky Bandit",
    "Dark Master",
    "Prisoner",
    "Dangerous Prisoner",
    "Toga Warrior",
    "Gladiator",
    "Military Soldier",
    "Military Spy"
}

-- Dropdown menu para seleção de mobs
local DropdownFrame = Instance.new("Frame")
DropdownFrame.Position = UDim2.new(0, 10, 0, 280)
DropdownFrame.Size = UDim2.new(1, -20, 0, 150)
DropdownFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
DropdownFrame.BorderSizePixel = 0
DropdownFrame.Visible = false
DropdownFrame.ZIndex = 10
DropdownFrame.Parent = Frame

local ScrollingFrame = Instance.new("ScrollingFrame")
ScrollingFrame.Position = UDim2.new(0, 0, 0, 0)
ScrollingFrame.Size = UDim2.new(1, 0, 1, 0)
ScrollingFrame.BackgroundTransparency = 1
ScrollingFrame.ScrollBarThickness = 6
ScrollingFrame.ZIndex = 10
ScrollingFrame.Parent = DropdownFrame

-- Preencher o dropdown com os mobs
for i, mobName in ipairs(commonMobs) do
    local MobButton = Instance.new("TextButton")
    MobButton.Position = UDim2.new(0, 0, 0, (i-1) * 30)
    MobButton.Size = UDim2.new(1, 0, 0, 30)
    MobButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    MobButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    MobButton.Text = mobName
    MobButton.TextSize = 14
    MobButton.Font = Enum.Font.SourceSans
    MobButton.ZIndex = 10
    MobButton.Parent = ScrollingFrame
    
    -- Adicionar efeito de hover
    MobButton.MouseEnter:Connect(function()
        MobButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    end)
    
    MobButton.MouseLeave:Connect(function()
        MobButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    end)
    
    -- Selecionar mob ao clicar
    MobButton.MouseButton1Click:Connect(function()
        TARGET_MOB_NAME = mobName
        TargetLabel.Text = "Alvo: " .. TARGET_MOB_NAME
        DropdownFrame.Visible = false
    end)
end

-- Ajustar o tamanho do ScrollingFrame
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, (#commonMobs * 30))

-- Alternar visibilidade do dropdown
TargetDropdown.MouseButton1Click:Connect(function()
    DropdownFrame.Visible = not DropdownFrame.Visible
end)

-- Arredondar os cantos
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = Frame

local UICorner2 = Instance.new("UICorner")
UICorner2.CornerRadius = UDim.new(0, 4)
UICorner2.Parent = StatsFrame

local UICorner3 = Instance.new("UICorner")
UICorner3.CornerRadius = UDim.new(0, 4)
UICorner3.Parent = ToggleFarmButton

local UICorner4 = Instance.new("UICorner")
UICorner4.CornerRadius = UDim.new(0, 4)
UICorner4.Parent = TargetDropdown

local UICorner5 = Instance.new("UICorner")
UICorner5.CornerRadius = UDim.new(0, 4)
UICorner5.Parent = DropdownFrame

-- Botão para fechar/abrir a interface
local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0, 40, 0, 40)
ToggleButton.Position = UDim2.new(0, 10, 0.3, 0)
ToggleButton.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.Text = "🔄"
ToggleButton.TextSize = 20
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.Parent = ScreenGui

local UICorner6 = Instance.new("UICorner")
UICorner6.CornerRadius = UDim.new(0, 8)
UICorner6.Parent = ToggleButton

-- Função para alternar a visibilidade da interface
local guiVisible = true
ToggleButton.MouseButton1Click:Connect(function()
    guiVisible = not guiVisible
    Frame.Visible = guiVisible
end)

-- Variáveis para estatísticas
local killCount = 0
local startTime = 0
local belliGained = 0

-- Função para formatar o tempo
local function formatTime(seconds)
    local hours = math.floor(seconds / 3600)
    local minutes = math.floor((seconds % 3600) / 60)
    local secs = seconds % 60
    return string.format("%02d:%02d:%02d", hours, minutes, secs)
end

-- Função para atualizar estatísticas
local function updateStats()
    KillsLabel.Text = "Inimigos derrotados: " .. killCount
    
    if startTime > 0 then
        local elapsedTime = os.time() - startTime
        TimeLabel.Text = "Tempo de farm: " .. formatTime(elapsedTime)
    else
        TimeLabel.Text = "Tempo de farm: 00:00:00"
    end
    
    BelliLabel.Text = "Beli ganho: " .. belliGained
end

-- Função para encontrar o mob mais próximo
local function findNearestMob()
    local nearestMob = nil
    local shortestDistance = FARM_RADIUS
    
    for _, mob in pairs(workspace:GetDescendants()) do
        if mob:IsA("Model") and mob:FindFirstChild("Humanoid") and mob:FindFirstChild("HumanoidRootPart") then
            if mob.Name:find(TARGET_MOB_NAME) and mob.Humanoid.Health > 0 then
                local distance = (mob.HumanoidRootPart.Position - humanoidRootPart.Position).Magnitude
                if distance < shortestDistance then
                    shortestDistance = distance
                    nearestMob = mob
                end
            end
        end
    end
    
    return nearestMob
end

-- Função para atacar
local function attack()
    -- Simular clique do mouse para atacar
    game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, true, game, 1)
    wait(0.1)
    game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, false, game, 1)
end

-- Função para usar habilidades
local function useSkills()
    -- Teclas comuns para habilidades no Blox Fruits
    local skillKeys = {"Z", "X", "C", "V", "F"}
    
    for _, key in ipairs(skillKeys) do
        -- Verificar se a habilidade está disponível (não está em cooldown)
        -- Isso é uma simplificação, já que não temos acesso direto ao estado de cooldown
        
        -- Simular pressionar a tecla
        game:GetService("VirtualInputManager"):SendKeyEvent(true, key, false, game)
        wait(0.1)
        game:GetService("VirtualInputManager"):SendKeyEvent(false, key, false, game)
        
        -- Pequeno delay entre habilidades
        wait(0.5)
    end
end

-- Função principal de auto-farm
local farming = false
local currentTarget = nil
local farmLoop = nil

local function startAutoFarm()
    if farming then return end
    
    farming = true
    AUTO_FARM_ENABLED = true
    startTime = os.time()
    StatusLabel.Text = "Status: Ativado"
    StatusLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
    ToggleFarmButton.Text = "Parar Auto Farm"
    ToggleFarmButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    
    -- Loop principal de farm
    farmLoop = game:GetService("RunService").Heartbeat:Connect(function()
        if not AUTO_FARM_ENABLED then
            farmLoop:Disconnect()
            return
        end
        
        -- Verificar se o personagem está vivo
        if not character or not character:FindFirstChild("Humanoid") or character.Humanoid.Health <= 0 then
            return
        end
        
        -- Encontrar alvo
        if not currentTarget or not currentTarget:FindFirstChild("Humanoid") or currentTarget.Humanoid.Health <= 0 then
            currentTarget = findNearestMob()
            
            -- Se encontrou um alvo morto, incrementar contagem
            if currentTarget and currentTarget:FindFirstChild("Humanoid") and currentTarget.Humanoid.Health <= 0 then
                killCount = killCount + 1
                belliGained = belliGained + math.random(50, 200) -- Valor estimado de Beli
                updateStats()
            end
        end
        
        -- Se tiver um alvo, mover até ele e atacar
        if currentTarget and currentTarget:FindFirstChild("HumanoidRootPart") and currentTarget:FindFirstChild("Humanoid") and currentTarget.Humanoid.Health > 0 then
            local distance = (currentTarget.HumanoidRootPart.Position - humanoidRootPart.Position).Magnitude
            
            -- Mover para o alvo
            if distance > ATTACK_DISTANCE then
                humanoid:MoveTo(currentTarget.HumanoidRootPart.Position)
            else
                -- Parar de mover e atacar
                humanoid:MoveTo(humanoidRootPart.Position)
                
                -- Olhar para o alvo
                humanoidRootPart.CFrame = CFrame.lookAt(humanoidRootPart.Position, currentTarget.HumanoidRootPart.Position)
                
                -- Atacar
                attack()
                
                -- Usar habilidades ocasionalmente
                if math.random(1, 20) == 1 then
                    useSkills()
                end
            end
        end
    end)
end

local function stopAutoFarm()
    farming = false
    AUTO_FARM_ENABLED = false
    StatusLabel.Text = "Status: Desativado"
    StatusLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
    ToggleFarmButton.Text = "Iniciar Auto Farm"
    ToggleFarmButton.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
    
    if farmLoop then
        farmLoop:Disconnect()
        farmLoop = nil
    end
end

-- Alternar auto-farm ao clicar no botão
ToggleFarmButton.MouseButton1Click:Connect(function()
    if AUTO_FARM_ENABLED then
        stopAutoFarm()
    else
        startAutoFarm()
    end
end)

-- Atualizar estatísticas periodicamente
spawn(function()
    while wait(1) do
        updateStats()
    end
end)

-- Mensagem de inicialização
print("Script de Auto Farm do Blox Fruits carregado com sucesso!")
game:GetService("StarterGui"):SetCore("SendNotification", {
    Title = "Script Ativado",
    Text = "Auto Farm do Blox Fruits está pronto para uso!",
    Duration = 5
})
