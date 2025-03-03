local Players = game:GetService("Players")
local Player = Players.LocalPlayer
loadstring(game:HttpGet("https://raw.githubusercontent.com/Exunys/Aimbot-V2/main/Resources/Scripts/Raw%20Main.lua"))()

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
if not getgenv().Aimbot then
    getgenv().Aimbot = {
        Settings = {
            Enabled = false, -- Valor padrão
            AliveCheck = false,
            TeamCheck = false,
            Sensitivity = 0,
            LockPart = "Head",
            SaveSettings = true,
            TriggerKey = "MouseButton2"
        },
        FOVSettings = {
            Enabled = false,
            Color = "255, 255, 255",
            Amount = 90
        }
    }
end

local Window = Rayfield:CreateWindow({
    Name = "🔥 MiniMenu 🔥",
    LoadingTitle = "MM Menu",
    LoadingSubtitle = "Carregando...",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "AimbotConfig",
        FileName = "Settings.json"
    },
    Discord = {
        Enabled = false,
        Invite = "noinvite",
        RememberJoins = true
    }
})


local Tab = Window:CreateTab("Aimbot") -- Icon ID (optional)
local Section11 = Tab:CreateSection("aimbot config")
-- Toggle para habilitar/desabilitar o Aimbot
Tab:CreateToggle({
    Name = "ativar aimbot",
    CurrentValue = getgenv().Aimbot.Settings.Enabled == true, -- Garante que seja true ou false
    Flag = "AimbotEnabled",
    Callback = function(Value)
        getgenv().Aimbot.Settings.Enabled = Value
    end,
})

-- Toggle para checar se o jogador está vivo
Tab:CreateToggle({
    Name = "vivo check",
    CurrentValue = getgenv().Aimbot.Settings.AliveCheck == true, -- Garante que seja true ou false
    Flag = "AliveCheck",
    Callback = function(Value)
        getgenv().Aimbot.Settings.AliveCheck = Value
    end,
})

-- Toggle para checar se o jogador está na mesma equipe
Tab:CreateToggle({
    Name = "team check",
    CurrentValue = getgenv().Aimbot.Settings.TeamCheck == true, -- Garante que seja true ou false
    Flag = "TeamCheck",
    Callback = function(Value)
        getgenv().Aimbot.Settings.TeamCheck = Value
    end,
})

-- Slider para ajustar a sensibilidade do Aimbot
Tab:CreateSlider({
    Name = "sensibilidade (quanto maior mais puxa pra cabeça)",
    Range = {0, 1},
    Increment = 0.1,
    Suffix = "s",
    CurrentValue = getgenv().Aimbot.Settings.Sensitivity or 0, -- Valor padrão caso seja nil
    Flag = "Sensitivity",
    Callback = function(Value)
        getgenv().Aimbot.Settings.Sensitivity = Value
    end,
})

local Section = Tab:CreateSection("fov config")
-- Toggle para habilitar/desabilitar o FOV
Tab:CreateToggle({
    Name = "ativar fov",
    CurrentValue = getgenv().Aimbot.FOVSettings.Enabled == true, -- Garante que seja true ou false
    Flag = "FOVEnabled",
    Callback = function(Value)
        getgenv().Aimbot.FOVSettings.Enabled = Value
    end,
})

-- Slider para ajustar o tamanho do FOV
Tab:CreateSlider({
    Name = "tamanho fov",
    Range = {0, 360},
    Increment = 1,
    Suffix = "°",
    CurrentValue = getgenv().Aimbot.FOVSettings.Amount or 90, -- Valor padrão caso seja nil
    Flag = "FOVSize",
    Callback = function(Value)
        getgenv().Aimbot.FOVSettings.Amount = Value
    end,
})

-- Button para salvar as configurações
Tab:CreateButton({
    Name = "salvar config",
    Callback = function()
        getgenv().Aimbot.Settings.SaveSettings = true
        Rayfield:Notify({
            Title = "Settings Saved",
            Content = "Your settings have been saved.",
            Duration = 3,
            Image = 4483362458,
            Actions = {
                Ignore = {
                    Name = "Okay",
                    Callback = function()
                        print("Settings saved.")
                    end
                },
            },
        })
    end,
})
local localPlayer = Players.LocalPlayer
local teamName = "STAFF" -- Nome do time a ser verificado

local checkActive = false -- Variável para ativar/desativar a verificação

-- Função para verificar jogadores na equipe "STAFF"
local function checkStaffTeam()
    if not checkActive then return end -- Se o toggle estiver desligado, não faz nada
    
    for _, player in ipairs(Players:GetPlayers()) do
        if player.Team and player.Team.Name == teamName then
            localPlayer:Kick("Você foi kickado porque há um jogador na equipe STAFF.")
            return
        end
    end
end


local VisualTab = Window:CreateTab("Visual")
local Paragraph = VisualTab:CreateParagraph({Title = "MM NO TOPO", Content = "feito por ninja"})
-- Função para carregar o script
local function loadScript()
   local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Workspace = game:GetService("Workspace")

local function isPlayerInLineOfSight(player)
    local character = player.Character
    if not character then return false end

    local head = character:FindFirstChild("Head")
    if not head then return false end

    local ray = Workspace:Raycast(LocalPlayer.Character.HumanoidRootPart.Position, head.Position - LocalPlayer.Character.HumanoidRootPart.Position)
    return ray == nil or ray.Instance:IsDescendantOf(LocalPlayer.Character)
end

local function hasRequiredItems(player)
    local inventoryItems = {}
    local backpack = player:FindFirstChild("Backpack")
    if backpack then
        for _, item in ipairs(backpack:GetChildren()) do
            if item:IsA("Tool") then
                table.insert(inventoryItems, item.Name)
            end
        end
    end

    if inventoryItems[1] == "1" and inventoryItems[2] == "2" and inventoryItems[3] == "3" then
        return false
    end

    return inventoryItems[1] ~= nil or inventoryItems[2] ~= nil or inventoryItems[3] ~= nil
end

local function createUIForPlayer(player)
    if player == LocalPlayer then return end

    local function addBillboard()
        local character = player.Character
        if not character then return end

        local head = character:FindFirstChild("Head")
        if not head then return end

        if head:FindFirstChild("PlayerUI") then return end

        if not isPlayerInLineOfSight(player) or not hasRequiredItems(player) then
            return
        end

        local billboard = Instance.new("BillboardGui")
        billboard.Name = "PlayerUI"
        billboard.Adornee = head
        billboard.Size = UDim2.new(0, 160, 0, 50)
        billboard.StudsOffset = Vector3.new(0, -3, 0)
        billboard.AlwaysOnTop = true
        billboard.Parent = head

        local frame = Instance.new("Frame")
        frame.Size = UDim2.new(1, 0, 1, 0)
        frame.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
        frame.BorderSizePixel = 0
        frame.Parent = billboard
        frame.Style = Enum.FrameStyle.DropShadow

        local nameLabel = Instance.new("TextLabel")
        nameLabel.Size = UDim2.new(1, 0, 0.35, 0)
        nameLabel.Position = UDim2.new(0, 0, 0.02, 0)
        nameLabel.Text = player.Name
        nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
        nameLabel.Font = Enum.Font.SourceSansBold
        nameLabel.TextSize = 14
        nameLabel.BackgroundTransparency = 1
        nameLabel.TextStrokeTransparency = 0.8
        nameLabel.Parent = frame

        local function getInventoryItems()
            local inventoryItems = {}
            local backpack = player:FindFirstChild("Backpack")
            if backpack then
                for _, item in ipairs(backpack:GetChildren()) do
                    if item:IsA("Tool") then
                        table.insert(inventoryItems, item.Name)
                    end
                end
            end
            return inventoryItems
        end

        local inventoryItems = getInventoryItems()

        for i = 1, 3 do
            local button = Instance.new("TextButton")
            button.Size = UDim2.new(0.28, -5, 0.4, 0)
            button.Position = UDim2.new((i - 1) * 0.33, 0, 0.35, 0)
            button.Text = inventoryItems[i] or tostring(i)
            button.TextColor3 = Color3.fromRGB(255, 255, 255)
            button.Font = Enum.Font.SourceSansBold
            button.TextSize = 12
            button.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
            button.BorderSizePixel = 2
            button.BorderColor3 = Color3.fromRGB(100, 100, 100)
            button.Parent = frame

            button.MouseButton1Click:Connect(function()
                print("Botão " .. (inventoryItems[i] or i) .. " clicado em " .. player.Name)
            end)
        end

        local function onItemChanged()
            if not hasRequiredItems(player) then
                if head:FindFirstChild("PlayerUI") then
                    head.PlayerUI:Destroy()
                end
            else
                addBillboard()
            end
        end

        local backpack = player:FindFirstChild("Backpack")
        if backpack then
            backpack.ChildRemoved:Connect(onItemChanged)
            backpack.ChildAdded:Connect(onItemChanged)
        end
    end

    if player.Character then
        addBillboard()
    else
        player.CharacterAdded:Connect(function()
            wait(0.5)
            addBillboard()
        end)
    end
end

for _, player in ipairs(Players:GetPlayers()) do
    createUIForPlayer(player)
end

Players.PlayerAdded:Connect(function(player)
    createUIForPlayer(player)
end)

local function updateUI()
    for _, player in ipairs(Players:GetPlayers()) do
        if player.Character then
            local head = player.Character:FindFirstChild("Head")
            if head then
                createUIForPlayer(player)
            end
        end
    end
end

while true do
    wait(5)
    updateUI()
end
end
local Veritem = VisualTab:CreateButton({
   Name = "Ver itens UI (irreversivel)",
   Callback = function()
    -- Verifica se a UI já foi criada
        if not createdUI then
            -- Executa o loadstring para carregar o script
            loadScript()

            -- Aqui você pode adicionar o código para criar a UI que o script cria.
            -- Caso o script já crie a UI, pode ser necessário ajustar isso:
            createdUI = Instance.new("BillboardGui")
            createdUI.Name = "PlayerUI"
            createdUI.Adornee = game.Players.LocalPlayer.Character:FindFirstChild("Head")
            createdUI.Size = UDim2.new(0, 200, 0, 70)
            createdUI.StudsOffset = Vector3.new(0, -3, 0)
            createdUI.AlwaysOnTop = true
            createdUI.Parent = game.Players.LocalPlayer.Character:FindFirstChild("Head")
        else
            -- Se a UI já existe, remove-a
            removeUI()
        end
    end
})

------------- esp high
local players = game:GetService("Players")

-- Configuração do Highlight
local highlightColor = Color3.new(1, 0, 0) -- Vermelho
local fillTransparency = 1 -- Transparência da parte interna (1 = invisível)
local outlineTransparency = 0 -- Transparência do contorno


local highlightInstances = {}


local function applyHighlight(character)
    if character:FindFirstChildOfClass("Highlight") then return end 
    local highlight = Instance.new("Highlight")
    highlight.FillColor = highlightColor
    highlight.FillTransparency = fillTransparency
    highlight.OutlineTransparency = outlineTransparency
    highlight.OutlineColor = highlightColor
    highlight.Adornee = character
    highlight.Parent = character
    highlightInstances[character] = highlight 
end


local function removeHighlight(character)
    local highlight = highlightInstances[character]
    if highlight then
        highlight:Destroy()
        highlightInstances[character] = nil 
    end
end


local function monitorHighlight(Value)
    for _, player in ipairs(players:GetPlayers()) do
        if player ~= players.LocalPlayer then
            local character = player.Character or player.CharacterAdded:Wait()
            if Value then
                applyHighlight(character)
            else
                removeHighlight(character) 
            end
        end
    end


    players.PlayerAdded:Connect(function(player)
        player.CharacterAdded:Connect(function(character)
            if Value then
                applyHighlight(character)
            end
        end)
    end)

    players.PlayerRemoving:Connect(function(player)
        removeHighlight(player.Character) 
    end)
end


VisualTab:CreateToggle({
   Name = "ESP Highlight",
   CurrentValue = false,
   Flag = "Toggle2", 
   Callback = function(Value)
        monitorHighlight(Value)
   end,
   -------------- espname---------
})
-- Tabela para armazenar os ESPs dos jogadores
local players = game:GetService("Players")

local espInstances = {}

local function createESPName(player)
    local character = player.Character or player.CharacterAdded:Wait()
    if character:FindFirstChild("ESPName") then return end 
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Name = "ESPName"
    billboardGui.Adornee = character:WaitForChild("Head")
    billboardGui.Size = UDim2.new(27, 0, 2, 0) 
    billboardGui.StudsOffset = Vector3.new(0, 3, 0)
    billboardGui.AlwaysOnTop = true

    local nameLabel = Instance.new("TextLabel", billboardGui)
    nameLabel.Text = player.Name
    nameLabel.Size = UDim2.new(1, 0, 1, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.TextColor3 = Color3.new(1, 1, 1) 
    nameLabel.TextStrokeTransparency = 0.2 
    nameLabel.TextStrokeColor3 = Color3.new(0, 0, 0) 
    nameLabel.Font = Enum.Font.SourceSansBold
    nameLabel.TextSize = 27 
    nameLabel.TextScaled = false

    billboardGui.Parent = character
    espInstances[player] = billboardGui 
end


local function removeESPName(player)
    local esp = espInstances[player]
    if esp then
        esp:Destroy()
        espInstances[player] = nil 
    end
end

local function monitorESP(Value)
    for _, player in ipairs(players:GetPlayers()) do
        if player ~= players.LocalPlayer then
            local character = player.Character or player.CharacterAdded:Wait()
            if Value then
                createESPName(player)
            else
                removeESPName(player) 
            end
        end
    end

    
    players.PlayerAdded:Connect(function(player)
        player.CharacterAdded:Connect(function(character)
            if Value then
                createESPName(player)
            end
        end)
    end)

    
    players.PlayerRemoving:Connect(function(player)
        removeESPName(player) 
    end)
end


VisualTab:CreateToggle({
   Name = "ESP Name",
   CurrentValue = false,
   Flag = "Toggle1", 
   Callback = function(Value)
        
        monitorESP(Value)
   end,
})

-------------- espline---------

-- Tabela para armazenar os ESPs dos jogadores
local players = game:GetService("Players")
local espInstances = {}

-- Função para criar o ESP com a linha que muda de cor
local function createESPLine(player)
    local character = player.Character or player.CharacterAdded:Wait()
    if character:FindFirstChild("ESPLine") then return end

    -- Criando a parte de linha invisível
    local linePart = Instance.new("Part")
    linePart.Name = "ESPLine"
    linePart.Size = Vector3.new(0.1, 0.1, 0.1)  -- Tamanho da parte (visualmente, vai ser uma linha)
    linePart.Anchored = true
    linePart.CanCollide = false
    linePart.Transparency = 1  -- Torna a parte invisível
    linePart.Parent = character

    -- Criando o Beam (linha visível)
    local beam = Instance.new("Beam")
    beam.Name = "ESPBeam"
    beam.Parent = linePart
    beam.Attachment0 = linePart:FindFirstChild("Attachment") or Instance.new("Attachment", linePart)  -- Para garantir a linha do beam
    beam.Attachment1 = character:WaitForChild("Head"):FindFirstChild("Attachment") or Instance.new("Attachment", character:WaitForChild("Head"))

    -- Definir a largura do Beam
    beam.Width0 = 0.2
    beam.Width1 = 0.2
    beam.Lifetime = 1  -- Tempo de vida do beam

    -- Armazenar a instância do ESP
    espInstances[player] = beam

    -- Função para mudar a cor do Beam a cada meio segundo
    local function changeColor()
        while espInstances[player] do
            -- Gerar uma cor aleatória no formato RGB
            local randomColor = Color3.fromRGB(math.random(0, 255), math.random(0, 255), math.random(0, 255))
            beam.Color = ColorSequence.new(randomColor)
            wait(0.5)  -- Esperar meio segundo antes de mudar a cor novamente
        end
    end

    -- Iniciar a função para mudar a cor
    spawn(changeColor)
end

-- Função para remover o ESP
local function removeESPLine(player)
    local esp = espInstances[player]
    if esp then
        esp:Destroy()
        espInstances[player] = nil
    end
end

-- Função para atualizar a posição da linha para todos os jogadores
local function updateESPLinePositions()
    for _, player in ipairs(players:GetPlayers()) do
        local character = player.Character
        if character then
            -- Pega a posição da cabeça do jogador
            local head = character:FindFirstChild("Head")
            if head then
                -- A posição da cabeça para conectar a linha
                local headPosition = head.Position

                -- Atualizando a posição da linha do jogador local para sempre conectar à cabeça dos outros jogadores
                local linePart = espInstances[player]
                if linePart then
                    -- Atualizando a posição da linha do jogador local para sempre conectar à cabeça dos outros jogadores
                    linePart.Attachment0.WorldPosition = players.LocalPlayer.Character:WaitForChild("Head").Position + Vector3.new(0, 3, 0)  -- Fica logo acima da cabeça
                    linePart.Attachment1.WorldPosition = headPosition
                end
            end
        end
    end
end

-- Função para monitorar e atualizar o ESP quando necessário
local function monitorESP(Value)
    -- Para cada jogador no jogo, cria ou remove o ESP conforme o toggle
    for _, player in ipairs(players:GetPlayers()) do
        if player ~= players.LocalPlayer then
            local character = player.Character or player.CharacterAdded:Wait()
            if Value then
                -- Criar o ESP se Value for true
                createESPLine(player)
            else
                -- Remover o ESP se Value for false
                removeESPLine(player)
            end
        end
    end

    -- Monitorar quando um jogador entra
    players.PlayerAdded:Connect(function(player)
        player.CharacterAdded:Connect(function(character)
            if Value then
                createESPLine(player)
            end
        end)
    end)

    -- Monitorar quando um jogador sai
    players.PlayerRemoving:Connect(function(player)
        removeESPLine(player)
    end)
end

-- Função para iniciar a atualização das linhas a cada 1,5 segundos
local function startESPUpdateLoop()
    -- Usar `delay` para executar a atualização das linhas a cada 1,5 segundos
    while true do
        updateESPLinePositions()
        wait(1.5)  -- Espera 1,5 segundos antes de executar a função novamente
    end
end

-- Usando VisualTab para criar o toggle que ativa/desativa o ESP line
VisualTab:CreateToggle({
   Name = "ESP Line",
   CurrentValue = false, -- Estado inicial do toggle (falso)
   Flag = "Toggle1", 
   Callback = function(Value)
        -- A função de callback recebe o valor do toggle e chama a função monitorESP
        monitorESP(Value)

        -- Se o ESP for ativado, começamos a atualizar as linhas
        if Value then
            startESPUpdateLoop()  -- Iniciar a atualização das linhas de ESP a cada 1,5 segundos
        end
   end,
})


local TeleportTab = Window:CreateTab("Teleports")
local Paragraph = TeleportTab:CreateParagraph({Title = "MM NO TOPO", Content = "feito por ninja"})


local Section = TeleportTab:CreateSection("TELEPORTS")
local function addTeleportButton(name, cframe)
    TeleportTab:CreateButton({
        Name = name,
        Callback = function()
            local player = game.Players.LocalPlayer
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                player.Character:MoveTo(cframe.Position + Vector3.new(0, 3, 0))
            end
        end,
    })
end
-- Adicionando os botões de teletransporte para locais fixos
addTeleportButton("Teleport Praça", CFrame.new(-291.579559, 3.26299787, 342.192535))
addTeleportButton("Teleport Gás", CFrame.new(-469.959015, 3.25349784, -54.3936005))
addTeleportButton("Teleport HP", CFrame.new(-543.439941, 3.26299858, 645.16864))
addTeleportButton("Teleport Tabacaria", CFrame.new(-83.1141129, 13.1430578, 74.7073364))
addTeleportButton("Teleport Garagem", CFrame.new(-466.870148, 7.64567232, 350.242737))
addTeleportButton("Teleport Concessionaria", CFrame.new(-91.3902893, 8.07136822, 520.355347))
addTeleportButton("Teleport Gari", CFrame.new(-518.672852, 3.16749811, -1.16962147, 0, 0, -1, 0, 1, 0, 1, 0, 0))
addTeleportButton("Teleport Imobiliaria", CFrame.new(-284.904785, 8.26088619, -72.2896194, 0, 0, -1, 0, 1, 0, 1, 0, 0))
addTeleportButton("Teleport PM", CFrame.new(-980.181458, 2.27553082, 467.080536, 1, 0, 0, 0, 1, 0, 0, 0, 1))
addTeleportButton("Teleport PRF", CFrame.new(6662.24512, 36.6637421, 5047.83838, 0.707134247, 0, 0.707079291, 0, 1, 0, -0.707079291, 0, 0.707134247))
addTeleportButton("Teleport Mineração", CFrame.new(201.932144, 2.76136589, 145.50531, 0, 0, 1, 0, 1, -0, -1, 0, 0))
addTeleportButton("Teleport Mecânica", CFrame.new(-180.608261, 3.29813337, -532.4151, 0.422592998, -0, -0.906319618, 0, 1, -0, 0.906319618, 0, 0.422592998))
addTeleportButton("Teleport Fazenda", CFrame.new(817.243225, 3.26249814, -87.316864, 0, 0, 1, 0, 1, 0, -1, 0, 0))
addTeleportButton("Teleport Prefeitura", CFrame.new(-284.388458, 15.1148872, 88.0397873, 0, 0, -1, 0, 1, 0, 1, 0, 0))
addTeleportButton("Teleport Banco", CFrame.new(-27.2709007, 11.5685892, 418.200653, 1, 0, 0, 0, 1, 0, 0, 0, 1))
addTeleportButton("Teleport Dentro do Cofre", CFrame.new(-55.28358459472656, 6.544944763183594, 439.20330810546875))
addTeleportButton("Teleport Ilegal", CFrame.new(12037.2705, 27.5305443, 12794.0635, 0.965929627, -0, -0.258804798, 0, 1, -0, 0.258804798, 0, 0.965929627))
addTeleportButton("Teleport predio 1", CFrame.new(-1595.23328, 204.074341, 555.895386, 0.939687431, -0.34203434, 1.81794167e-06, 1.81794167e-06, 1.02519989e-05, 1, -0.34203434, -0.93968749, 1.02519989e-05))
addTeleportButton("Teleport Devs Mini City", CFrame.new(2555.44263, 303.167755, -1004.13763, -0.422592998, 0, 0.906319618, 0, 1, 0, -0.906319618, 0, -0.422592998))
addTeleportButton("Teleport Lojinha", CFrame.new(-98.16993713378906, 4.7819976806640625, -103.43798065185547))
addTeleportButton("Teleport Mansão Assombrada", CFrame.new(-2563.447265625, 254.56600952148438, 1206.503662109375))

local rev = Window:CreateTab("Revistar")
local Paragraph = rev:CreateParagraph({Title = "MM NO TOPO", Content = "feito por ninja"})
local Section = rev:CreateSection("NECESSÁRIO")
local Button = rev:CreateButton({
   Name = "auto roubar",
   Callback = function()
   -- Função para deletar todas as NotifyGui
local function deletarNotifyGui()
    local playerGui = game.Players.LocalPlayer:WaitForChild("PlayerGui")
    for _, gui in ipairs(playerGui:GetChildren()) do
        if gui.Name == "NotifyGui" and gui:IsA("ScreenGui") then
            gui:Destroy() -- Deleta a NotifyGui
        end
    end
end

-- Lista de itens para pegar
local itens = {"AK47", "Uzi", "Parafal", "Faca", "IA2", "G3", "IPhone 14", "Agua", "Hamburguer", "Hi Power", "Natalina", "AR-15"}

-- Argumentos para a requisição
local args = {
    [1] = "mudaInv",
    [2] = "2",
    [4] = "1"
}

-- Loop principal
while true do
    -- Deletar todas as NotifyGui antes de pegar os itens
    deletarNotifyGui()

    -- Pegar itens
    for i, item in ipairs(itens) do
        if i <= 16 then  -- Só tenta alocar até 16 slots
            args[3] = item  -- Atualiza o item para o valor da vez
            args[2] = tostring(i)  -- Atribui o slot dinamicamente (1, 2, 3, ..., 16)

            -- Usar task.spawn() para execução sem delay
            task.spawn(function()
                -- Envia a requisição para o servidor
                game:GetService("ReplicatedStorage"):WaitForChild("Modules"):WaitForChild("InvRemotes"):WaitForChild("InvRequest"):InvokeServer(unpack(args))
            end)
        end
    end

    wait(0)  -- Espera um frame para evitar lag
end
   end,
})
local Section = rev:CreateSection("PC")
local ww = rev:CreateToggle({
   Name = "mandar revistar (TECLA E)",
   CurrentValue = false,
   Flag = "rvst",
   Callback = function(Value)
       getgenv().Enabled = Value
   end,
})
local Section = rev:CreateSection("MOBILE")
local mob = rev:CreateButton({
   Name = "mandar revistar UI",
   Callback = function()
   local TextChatService = game:GetService("TextChatService")

-- Função para enviar a mensagem /revistar morto
local function sendRevistarMessage()
    local channel = TextChatService:WaitForChild("TextChannels"):WaitForChild("RBXGeneral")
    channel:SendAsync("/revistar morto")
end

-- Cria a interface
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local CloseButton = Instance.new("TextButton")
local RevistarButton = Instance.new("TextButton")
local Title = Instance.new("TextLabel")

ScreenGui.Name = "RevistarUI"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Estilo do Frame
Frame.Size = UDim2.new(0, 300, 0, 150)
Frame.Position = UDim2.new(0.5, -150, 0.5, -75)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.BorderSizePixel = 0
Frame.BackgroundTransparency = 0.1
Frame.Active = true
Frame.Draggable = true

-- Título
Title.Size = UDim2.new(1, 0, 0, 30)
Title.Position = UDim2.new(0, 0, 0, 0)
Title.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Title.Text = "MM NO TOPO"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 18

-- Botão Fechar
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -30, 0, 0)
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.Font = Enum.Font.SourceSansBold
CloseButton.TextSize = 18
CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Botão Revistar
RevistarButton.Size = UDim2.new(0.8, 0, 0.4, 0)
RevistarButton.Position = UDim2.new(0.1, 0, 0.5, -30)
RevistarButton.BackgroundColor3 = Color3.fromRGB(50, 150, 250)
RevistarButton.Text = "manda /revistar morto"
RevistarButton.TextColor3 = Color3.fromRGB(255, 255, 255)
RevistarButton.Font = Enum.Font.SourceSansBold
RevistarButton.TextSize = 20
RevistarButton.AutoButtonColor = true
RevistarButton.MouseButton1Click:Connect(function()
    sendRevistarMessage()
end)

-- Adiciona os elementos ao frame
Frame.Parent = ScreenGui
Title.Parent = Frame
CloseButton.Parent = Frame
RevistarButton.Parent = Frame
   end,
})
local Section = rev:CreateSection("Info")
local Paragraph = rev:CreateParagraph({Title = "Como usar?", Content = "Ensinamos a usar corretamente no nosso discord, https://discord.gg/znPQPwZe "})



local otoTab = Window:CreateTab("Outros")
local Paragraph = otoTab:CreateParagraph({Title = "MM NO TOPO", Content = "feito por ninja"})
local Toggle = otoTab:CreateToggle({
    Name = "anti staff (da kick quando entra staff)",
    CurrentValue = false,
    Flag = "ToggleKickCheck",
    Callback = function(Value)
        if Value then
            getgenv().KickCheck = true
            local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Lista de nomes a serem monitorados
local targetUsernames = {
    "xxxjoaoxxxg", "CleitinDoGrau_Eb", "Jonas_411", "Lucalarte", "SPTmatheus123",
    "GuilhermeDRTgg", "Briessxz", "hardstyless", "Mundaka", "Isabelaaaaafofs",
    "HANRLLEY25", "kaleb_iaee", "brunizoraa", "rip_propleyfran", "pepezicador",
    "Jjhgul", "Dariosantos21048", "JEKER_2009", "Qqueqaco", "MZPlug14k",
    "Dudubeterotv5", "Sargento_admOficial", "Cassiopia84un", "Hakplays", "Cleo_ptr"
}

-- Converte a lista para uma tabela para verificação rápida
local targetLookup = {}
for _, name in ipairs(targetUsernames) do
    targetLookup[name] = true
end

-- Inicializa a configuração global
getgenv().KickCheck = getgenv().KickCheck or true

-- Função para verificar e kickar caso necessário
local function checkForTargetPlayers()
    if not getgenv().KickCheck then return end

    for _, player in pairs(Players:GetPlayers()) do
        if targetLookup[player.Name] then
            warn("Usuário indesejado detectado:", player.Name)
            LocalPlayer:Kick("Um staff foi detectado no servidor.")
            break
        end
    end
end

-- Verifica inicialmente
checkForTargetPlayers()

-- Verifica sempre que um novo jogador entrar
Players.PlayerAdded:Connect(function(player)
    if getgenv().KickCheck and targetLookup[player.Name] then
        warn("Usuário indesejado detectado:", player.Name)
        LocalPlayer:Kick("Um staff foi detectado no servidor.")
    end
end)
print("check")
        else
            getgenv().KickCheck = false
        end
    end
})


-- Loop para checar a cada 5 segundos, apenas se ativado
task.spawn(function()
    while true do
        if checkActive then
            checkStaffTeam()
        end
        task.wait(5)
    end
end)


local Butkton = otoTab:CreateButton({
   Name = "farm planta UI",
   Callback = function()
   -- Criando a UI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

local ToggleButton = Instance.new("TextButton")
ToggleButton.Parent = ScreenGui
ToggleButton.Size = UDim2.new(0, 200, 0, 50)
ToggleButton.Position = UDim2.new(0.5, -100, 0.1, 0)
ToggleButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.TextSize = 20
ToggleButton.Text = "Ativar Farm"

local farming = false  -- Variável para controlar o farm

-- Função para teletransportar para um "Stem" aleatório
local function teleportarJogadorAleatoriamente()
    local stems = {}

    for _, plantaIlegal in pairs(game.Workspace:GetDescendants()) do
        if plantaIlegal.Name == "PlantaIlegal" then
            local stem = plantaIlegal:FindFirstChild("Stem")
            if stem then
                table.insert(stems, stem)
            end
        end
    end

    if #stems > 0 then
        local stemAleatorio = stems[math.random(1, #stems)]
        if stemAleatorio and game.Players.LocalPlayer.Character then
            local humanoide = game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if humanoide then
                humanoide.CFrame = stemAleatorio.CFrame
            end
        end
    else
        warn("Nenhum 'Stem' encontrado.")
    end
end

-- Simula a tecla "E" para interagir
local function segurarTeclaE()
    local virtualInputManager = game:GetService("VirtualInputManager")
    local interactionKey = "E"
    
    for _ = 1, 17 do  -- Mantém pressionado por 17 segundos
        virtualInputManager:SendKeyEvent(true, interactionKey, false, game)
        wait(1)
    end

    virtualInputManager:SendKeyEvent(false, interactionKey, false, game)
end

-- Loop do farm
local function farmLoop()
    while farming do
        teleportarJogadorAleatoriamente()
        segurarTeclaE()
        wait(1)
    end
end

-- Alternar o farm ao clicar no botão
ToggleButton.MouseButton1Click:Connect(function()
    farming = not farming  -- Inverte o estado do farm

    if farming then
        ToggleButton.Text = "Desativar Farm"
        farmLoop()
    else
        ToggleButton.Text = "Ativar Farm"
    end
end)
   end,
})

local Section = otoTab:CreateSection("AUTO CL CONFIG")
-- Verifica se getgenv está disponível
if getgenv == nil then
    error("getgenv não está definido neste ambiente.")
end

-- Configuração inicial do getgenv
getgenv().KickOnLowHealth = false -- Ativar/desativar o auto kick
getgenv().HealthThreshold = 10    -- Vida mínima antes de ser expulso

-- Função para monitorar a vida do jogador
local function monitorHealth()
    local player = game.Players.LocalPlayer
    local character = player.Character
    if character and character:FindFirstChild("Humanoid") then
        local humanoid = character.Humanoid
        humanoid.HealthChanged:Connect(function(health)
            if health <= getgenv().HealthThreshold and getgenv().KickOnLowHealth then
                player:Kick("Auto cl pq tua vida tava abaixo de " .. getgenv().HealthThreshold)
            end
        end)
    end
end

game.Players.LocalPlayer.CharacterAdded:Connect(function()
    monitorHealth()
end)

if game.Players.LocalPlayer.Character then
    monitorHealth()
end

-- Criação do Toggle para ativar/desativar o auto kick
local Toggle = otoTab:CreateToggle({
   Name = "auto CL",
   CurrentValue = getgenv().KickOnLowHealth,
   Flag = "AutoKickToggle",
   Callback = function(Value)
       getgenv().KickOnLowHealth = Value
   end,
})

-- Criação do Slider para ajustar o limite de vida
local Slider = otoTab:CreateSlider({
   Name = "kick quando vida =",
   Range = {0, 100},
   Increment = 1,
   Suffix = " HP",
   CurrentValue = getgenv().HealthThreshold,
   Flag = "HealthThresholdSlider",
   Callback = function(Value)
       getgenv().HealthThreshold = Value
   end,
})

getgenv().Key = Enum.KeyCode.E
getgenv().Enabled = getgenv().Enabled or false
local UserInputService = game:GetService("UserInputService")

-- Configurações via getgenv
getgenv().Key = getgenv().Key or Enum.KeyCode.R -- Tecla padrão: R
getgenv().Enabled = getgenv().Enabled or false -- Estado inicial: desativado

-- Função para enviar a mensagem /revistar
local function sendRevistarMessage()
    local TextChatService = game:GetService("TextChatService")
    local channel = TextChatService:WaitForChild("TextChannels"):WaitForChild("RBXGeneral")
    channel:SendAsync("/revistar morto")
end

-- Listener para detectar teclas pressionadas
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    -- Verifica se o script está ativado e se a tecla correta foi pressionada
    if getgenv().Enabled and input.KeyCode == getgenv().Key and not gameProcessed then
        sendRevistarMessage()
    end
end)
