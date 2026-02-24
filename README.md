-- Lunaris Hub - Script para Sintonia Roleplay
-- Versão SPEED 15 STUDS + AIMBOT 4D + AIMBOT SUPREMO + CAIXINHA 2D COM RGB PARA STAFF
-- Criado com Interface Lunaris

local Lunaris = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Configuração de cores personalizadas - Tema Roxo
local LunarisTheme = {
    TextColor = Color3.fromRGB(240, 240, 240),
    Background = Color3.fromRGB(20, 20, 20),
    Topbar = Color3.fromRGB(75, 40, 120),
    Shadow = Color3.fromRGB(10, 10, 10),
    NotificationBackground = Color3.fromRGB(75, 40, 120),
    NotificationActionsBackground = Color3.fromRGB(230, 230, 230),
    TabBackground = Color3.fromRGB(55, 45, 70),
    TabStroke = Color3.fromRGB(180, 130, 255),
    TabBackgroundSelected = Color3.fromRGB(180, 130, 255),
    TabTextColor = Color3.fromRGB(240, 240, 240),
    SelectedTabTextColor = Color3.fromRGB(20, 20, 20),
    ElementBackground = Color3.fromRGB(35, 30, 45),
    ElementBackgroundHover = Color3.fromRGB(55, 45, 70),
    SecondaryElementBackground = Color3.fromRGB(30, 25, 40),
    ElementStroke = Color3.fromRGB(180, 130, 255),
    SecondaryElementStroke = Color3.fromRGB(75, 50, 100),
    SliderBackground = Color3.fromRGB(140, 80, 255),
    SliderProgress = Color3.fromRGB(180, 130, 255),
    SliderStroke = Color3.fromRGB(200, 160, 255),
    ToggleBackground = Color3.fromRGB(30, 25, 40),
    ToggleEnabled = Color3.fromRGB(180, 130, 255),
    ToggleDisabled = Color3.fromRGB(80, 70, 100),
    ToggleEnabledStroke = Color3.fromRGB(200, 160, 255),
    ToggleDisabledStroke = Color3.fromRGB(100, 90, 120),
    ToggleEnabledOuterStroke = Color3.fromRGB(120, 100, 150),
    ToggleDisabledOuterStroke = Color3.fromRGB(65, 55, 75),
    DropdownSelected = Color3.fromRGB(55, 45, 70),
    DropdownUnselected = Color3.fromRGB(35, 30, 45),
    InputBackground = Color3.fromRGB(30, 25, 40),
    InputStroke = Color3.fromRGB(180, 130, 255),
    PlaceholderColor = Color3.fromRGB(178, 178, 178)
}

-- Criar janela principal
local Window = Lunaris:CreateWindow({
    Name = "Lunaris Hub",
    Icon = 0,
    LoadingTitle = "Lunaris Hub",
    LoadingSubtitle = "by Sintonia Roleplay",
    ShowText = "Lunaris",
    Theme = LunarisTheme,
    ToggleUIKeybind = "K",
    DisableRayfieldPrompts = false,
    DisableBuildWarnings = false,
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "LunarisHub",
        FileName = "Config"
    },
    Discord = {
        Enabled = true,
        Invite = "v97Afg9zZj",
        RememberJoins = true
    },
    KeySystem = false
})

-- Sistema de Minimizar (Lua)
local UIS = game:GetService("UserInputService")
local minimized = false
local moonButton = nil

-- Criar botão da lua quando minimizar
local function createMoonButton()
    if moonButton then moonButton:Destroy() end
    
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "LunarisMoonButton"
    screenGui.Parent = game:GetService("CoreGui")
    
    moonButton = Instance.new("TextButton")
    moonButton.Name = "MoonButton"
    moonButton.Size = UDim2.new(0, 50, 0, 50)
    moonButton.Position = UDim2.new(0, 10, 0, 10)
    moonButton.BackgroundColor3 = LunarisTheme.Topbar
    moonButton.BackgroundTransparency = 0.2
    moonButton.Text = "🌙"
    moonButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    moonButton.TextSize = 30
    moonButton.Font = Enum.Font.GothamBold
    moonButton.BorderSizePixel = 0
    moonButton.Parent = screenGui
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 25)
    corner.Parent = moonButton
    
    local shadow = Instance.new("ImageLabel")
    shadow.Name = "Shadow"
    shadow.BackgroundTransparency = 1
    shadow.Size = UDim2.new(1, 10, 1, 10)
    shadow.Position = UDim2.new(0, -5, 0, -5)
    shadow.Image = "rbxassetid://6015897843"
    shadow.ImageColor3 = Color3.fromRGB(0, 0, 0)
    shadow.ImageTransparency = 0.5
    shadow.ScaleType = Enum.ScaleType.Slice
    shadow.SliceCenter = Rect.new(10, 10, 118, 118)
    shadow.Parent = moonButton
    
    local dragging = false
    local dragInput, dragStart, startPos
    
    moonButton.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = moonButton.Position
            
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    
    moonButton.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    
    UIS.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            moonButton.Position = UDim2.new(
                startPos.X.Scale,
                startPos.X.Offset + delta.X,
                startPos.Y.Scale,
                startPos.Y.Offset + delta.Y
            )
        end
    end)
    
    moonButton.MouseButton1Click:Connect(function()
        minimized = false
        Window:ToggleUI(true)
        if moonButton and moonButton.Parent then
            moonButton.Parent:Destroy()
            moonButton = nil
        end
    end)
end

-- Modificar o botão de fechar para minimizar
local function setupMinimizeButton()
    spawn(function()
        wait(1)
        local gui = game:GetService("CoreGui"):FindFirstChild("Rayfield")
        if gui then
            local closeButton = gui:FindFirstChild("Main"):FindFirstChild("Window"):FindFirstChild("Topbar"):FindFirstChild("Container"):FindFirstChild("CloseButton")
            if closeButton then
                closeButton.MouseButton1Click:Connect(function()
                    minimized = true
                    Window:ToggleUI(false)
                    createMoonButton()
                end)
            end
        end
    end)
end

-- Notificação de boas-vindas
Lunaris:Notify({
    Title = "✨ Lunaris Hub ✨",
    Content = "Bem-vindo ao Lunaris Hub! Script para Sintonia Roleplay",
    Duration = 5,
    Image = "sparkles"
})

-- Variáveis globais para funcionalidades
local noclipEnabled = false
local noclipConnection = nil
local speedEnabled = false
local speedValue = 0
local speedConnection = nil
local originalWalkSpeed = 16
local espEnabled = false
local espConnections = {}
local espLines = {}
local espLineColor = Color3.fromRGB(180, 130, 255)
local espLineTransparency = 0.5
local espLineThickness = 2

-- Variáveis ESP Staff
local staffEspEnabled = false
local staffEspConnections = {}
local staffEspLines = {}

-- Variáveis Aimbot Supremo
local aimbotEnabled = false
local aimbotConnection = nil
local aimbotFov = 150
local aimbotStrength = 5
local fovCircle = nil
local fovColor = Color3.fromRGB(180, 130, 255)
local fovVisible = true

-- Variáveis Aimbot 4D
local aimbot4DEnabled = false
local aimbot4DConnection = nil
local aimbot4DFov = 200
local aimbot4DFovCircle = nil
local aimbot4DColor = Color3.fromRGB(255, 100, 255)
local aimbot4DFovVisible = true
local aimbot4DTime = 0

-- Variáveis Caixinha 2D
local box2DEnabled = false
local box2DColor = Color3.fromRGB(180, 130, 255)
local box2DShowName = true
local box2DObjects = {}
local box2DConnections = {}

-- Função para verificar se o player é STAFF (pelo nome ou time)
local function isStaff(player)
    -- Verificar pelo nome (contém STAFF)
    if player.Name and string.find(player.Name:upper(), "STAFF") then
        return true
    end
    if player.DisplayName and string.find(player.DisplayName:upper(), "STAFF") then
        return true
    end
    
    -- Verificar pelo time (Team)
    if player.Team then
        local teamName = player.Team.Name:upper()
        if string.find(teamName, "STAFF") or string.find(teamName, "ADMIN") or string.find(teamName, "MOD") then
            return true
        end
    end
    
    -- Verificar por tags no nome
    local name = player.Name:upper()
    local displayName = player.DisplayName:upper()
    
    local staffTags = {"STAFF", "ADMIN", "MOD", "OWNER", "FUNDADOR", "CRIADOR", "DEV", "DESENVOLVEDOR", "GERENTE", "SUPORTE"}
    
    for _, tag in ipairs(staffTags) do
        if string.find(name, tag) or string.find(displayName, tag) then
            return true
        end
    end
    
    return false
end

-- Criar abas
local PlayerTab = Window:CreateTab("👤 Player", "user")
local CombatTab = Window:CreateTab("⚔️ Combate", "swords")
local VisualTab = Window:CreateTab("👁️ Visual", "eye")
local WorldTab = Window:CreateTab("🌍 Mundo", "sun")
local MiscTab = Window:CreateTab("🎮 Diversos", "settings")
local CreditsTab = Window:CreateTab("💜 Créditos", "heart")

-- ============================================
-- ABA PLAYER
-- ============================================
local PlayerSection = PlayerTab:CreateSection("🛡️ Player Settings")

-- Noclip Toggle
local noclipToggle = PlayerTab:CreateToggle({
    Name = "🚫 Noclip (Atravessar Paredes)",
    CurrentValue = false,
    Flag = "NoclipToggle",
    Callback = function(Value)
        noclipEnabled = Value
        if Value then
            startNoclip()
            Lunaris:Notify({
                Title = "👤 Player",
                Content = "Noclip ativado! Você pode atravessar paredes",
                Duration = 3,
                Image = "slash"
            })
        else
            stopNoclip()
            Lunaris:Notify({
                Title = "👤 Player",
                Content = "Noclip desativado",
                Duration = 2,
                Image = "slash"
            })
        end
    end
})

-- Speed Toggle
local speedToggle = PlayerTab:CreateToggle({
    Name = "⚡ Ativar Speed (ATÉ 15 STUDS!)",
    CurrentValue = false,
    Flag = "SpeedToggle",
    Callback = function(Value)
        speedEnabled = Value
        if Value then
            startSpeed()
            Lunaris:Notify({
                Title = "👤 Player",
                Content = "Speed ativado! (" .. speedValue .. " studs extra)",
                Duration = 3,
                Image = "zap"
            })
        else
            stopSpeed()
            Lunaris:Notify({
                Title = "👤 Player",
                Content = "Speed desativado",
                Duration = 2,
                Image = "zap"
            })
        end
    end
})

-- Speed Slider
local speedSlider = PlayerTab:CreateSlider({
    Name = "📊 Velocidade Extra (studs)",
    Range = {0, 15},
    Increment = 1,
    Suffix = " studs",
    CurrentValue = 0,
    Flag = "SpeedValue",
    Callback = function(Value)
        speedValue = Value
        if speedEnabled then
            local player = game.Players.LocalPlayer
            if player and player.Character then
                local humanoid = player.Character:FindFirstChild("Humanoid")
                if humanoid then
                    humanoid.WalkSpeed = originalWalkSpeed + speedValue
                end
            end
            Lunaris:Notify({
                Title = "⚡ Speed",
                Content = "Velocidade: " .. (originalWalkSpeed + speedValue) .. " (" .. speedValue .. " studs extra)",
                Duration = 1,
                Image = "zap"
            })
        end
    end
})

PlayerTab:CreateDivider()

-- ============================================
-- ABA COMBATE
-- ============================================
local CombatSection = CombatTab:CreateSection("🎯 Aimbot Supremo (TRAVADO)")

-- Função para criar círculo FOV fixo no centro (Aimbot Supremo)
local function createFovCircle()
    if fovCircle then 
        fovCircle:Destroy()
        fovCircle = nil
    end
    
    local viewportSize = workspace.CurrentCamera.ViewportSize
    
    fovCircle = Drawing.new("Circle")
    fovCircle.Visible = fovVisible and aimbotEnabled
    fovCircle.Radius = aimbotFov
    fovCircle.Thickness = 2
    fovCircle.Color = fovColor
    fovCircle.Filled = false
    fovCircle.NumSides = 64
    fovCircle.Position = Vector2.new(viewportSize.X / 2, viewportSize.Y / 2)
    
    game:GetService("RunService").RenderStepped:Connect(function()
        if fovCircle and fovVisible and aimbotEnabled then
            local currentViewport = workspace.CurrentCamera.ViewportSize
            fovCircle.Position = Vector2.new(currentViewport.X / 2, currentViewport.Y / 2)
            fovCircle.Radius = aimbotFov
            fovCircle.Visible = true
        elseif fovCircle then
            fovCircle.Visible = false
        end
    end)
end

-- Aimbot Supremo Toggle
local aimbotToggle = CombatTab:CreateToggle({
    Name = "🎯 Ativar Aimbot SUPREMO (TRAVADO)",
    CurrentValue = false,
    Flag = "AimbotToggle",
    Callback = function(Value)
        if Value and aimbot4DEnabled then
            aimbot4DEnabled = false
            if aimbot4DConnection then
                aimbot4DConnection:Disconnect()
                aimbot4DConnection = nil
            end
            if aimbot4DFovCircle then
                aimbot4DFovCircle.Visible = false
                aimbot4DFovCircle:Destroy()
                aimbot4DFovCircle = nil
            end
            Lunaris:Notify({
                Title = "⚔️ Combate",
                Content = "Aimbot 4D desativado (ativou Supremo)",
                Duration = 2,
                Image = "target"
            })
        end
        
        aimbotEnabled = Value
        if Value then
            startAimbotWorking()
            if fovVisible then
                createFovCircle()
            end
            Lunaris:Notify({
                Title = "⚔️ Combate",
                Content = "Aimbot SUPREMO ativado! Força: " .. aimbotStrength .. "/10",
                Duration = 3,
                Image = "target"
            })
        else
            if aimbotConnection then
                aimbotConnection:Disconnect()
                aimbotConnection = nil
            end
            if fovCircle then
                fovCircle.Visible = false
                fovCircle:Destroy()
                fovCircle = nil
            end
        end
    end
})

-- Toggle FOV (Supremo)
CombatTab:CreateToggle({
    Name = "👁️ Mostrar FOV (Supremo)",
    CurrentValue = true,
    Flag = "FOVVisible",
    Callback = function(Value)
        fovVisible = Value
        if aimbotEnabled then
            if Value then
                createFovCircle()
            elseif fovCircle then
                fovCircle.Visible = false
                fovCircle:Destroy()
                fovCircle = nil
            end
        end
    end
})

-- FOV Slider (Supremo)
CombatTab:CreateSlider({
    Name = "📏 Tamanho do FOV (Supremo)",
    Range = {50, 500},
    Increment = 10,
    Suffix = "px",
    CurrentValue = 150,
    Flag = "FOVSize",
    Callback = function(Value)
        aimbotFov = Value
        if fovCircle then
            fovCircle.Radius = Value
        end
    end
})

-- Cor do FOV (Supremo)
CombatTab:CreateColorPicker({
    Name = "🎨 Cor do FOV (Supremo)",
    Color = Color3.fromRGB(180, 130, 255),
    Flag = "FOVColor",
    Callback = function(Value)
        fovColor = Value
        if fovCircle then
            fovCircle.Color = Value
        end
    end
})

-- Slider de Força do Aimbot Supremo
CombatTab:CreateSlider({
    Name = "💪 Força do Aimbot SUPREMO (10 = TRAVA)",
    Range = {1, 10},
    Increment = 1,
    Suffix = "",
    CurrentValue = 5,
    Flag = "AimbotStrength",
    Callback = function(Value)
        aimbotStrength = Value
        Lunaris:Notify({
            Title = "🎯 Aimbot Supremo",
            Content = "Força ajustada para: " .. Value .. "/10",
            Duration = 1,
            Image = "target"
        })
    end
})

CombatTab:CreateDivider()

-- ============================================
-- AIMBOT 4D - VAI E VEM RÁPIDO (PUUXA FORTE)
-- ============================================
local Combat4DSection = CombatTab:CreateSection("🌀 Aimbot 4D (VAI E VEM RÁPIDO)")

-- Função para criar círculo FOV do Aimbot 4D
local function createAimbot4DFovCircle()
    if aimbot4DFovCircle then 
        aimbot4DFovCircle:Destroy()
        aimbot4DFovCircle = nil
    end
    
    local viewportSize = workspace.CurrentCamera.ViewportSize
    
    aimbot4DFovCircle = Drawing.new("Circle")
    aimbot4DFovCircle.Visible = aimbot4DFovVisible and aimbot4DEnabled
    aimbot4DFovCircle.Radius = aimbot4DFov
    aimbot4DFovCircle.Thickness = 2
    aimbot4DFovCircle.Color = aimbot4DColor
    aimbot4DFovCircle.Filled = false
    aimbot4DFovCircle.NumSides = 64
    aimbot4DFovCircle.Position = Vector2.new(viewportSize.X / 2, viewportSize.Y / 2)
    
    game:GetService("RunService").RenderStepped:Connect(function()
        if aimbot4DFovCircle and aimbot4DFovVisible and aimbot4DEnabled then
            local currentViewport = workspace.CurrentCamera.ViewportSize
            aimbot4DFovCircle.Position = Vector2.new(currentViewport.X / 2, currentViewport.Y / 2)
            aimbot4DFovCircle.Radius = aimbot4DFov
            aimbot4DFovCircle.Visible = true
        elseif aimbot4DFovCircle then
            aimbot4DFovCircle.Visible = false
        end
    end)
end

-- Aimbot 4D Toggle
local aimbot4DToggle = CombatTab:CreateToggle({
    Name = "🌀 Ativar Aimbot 4D (VAI E VEM RÁPIDO)",
    CurrentValue = false,
    Flag = "Aimbot4DToggle",
    Callback = function(Value)
        if Value and aimbotEnabled then
            aimbotEnabled = false
            if aimbotConnection then
                aimbotConnection:Disconnect()
                aimbotConnection = nil
            end
            if fovCircle then
                fovCircle.Visible = false
                fovCircle:Destroy()
                fovCircle = nil
            end
            Lunaris:Notify({
                Title = "⚔️ Combate",
                Content = "Aimbot Supremo desativado (ativou 4D Rápido)",
                Duration = 2,
                Image = "target"
            })
        end
        
        aimbot4DEnabled = Value
        if Value then
            startAimbot4D()
            if aimbot4DFovVisible then
                createAimbot4DFovCircle()
            end
            Lunaris:Notify({
                Title = "🌀 Aimbot 4D",
                Content = "Aimbot 4D RÁPIDO ativado! Puxa FORTE na cabeça + vai e vem",
                Duration = 4,
                Image = "move"
            })
        else
            if aimbot4DConnection then
                aimbot4DConnection:Disconnect()
                aimbot4DConnection = nil
            end
            if aimbot4DFovCircle then
                aimbot4DFovCircle.Visible = false
                aimbot4DFovCircle:Destroy()
                aimbot4DFovCircle = nil
            end
        end
    end
})

-- Toggle FOV 4D
CombatTab:CreateToggle({
    Name = "👁️ Mostrar FOV 4D",
    CurrentValue = true,
    Flag = "FOV4DVisible",
    Callback = function(Value)
        aimbot4DFovVisible = Value
        if aimbot4DEnabled then
            if Value then
                createAimbot4DFovCircle()
            elseif aimbot4DFovCircle then
                aimbot4DFovCircle.Visible = false
                aimbot4DFovCircle:Destroy()
                aimbot4DFovCircle = nil
            end
        end
    end
})

-- FOV Slider 4D
CombatTab:CreateSlider({
    Name = "📏 Tamanho do FOV 4D",
    Range = {50, 500},
    Increment = 10,
    Suffix = "px",
    CurrentValue = 200,
    Flag = "FOV4DSize",
    Callback = function(Value)
        aimbot4DFov = Value
        if aimbot4DFovCircle then
            aimbot4DFovCircle.Radius = Value
        end
    end
})

-- Cor do FOV 4D
CombatTab:CreateColorPicker({
    Name = "🎨 Cor do FOV 4D",
    Color = Color3.fromRGB(255, 100, 255),
    Flag = "FOV4DColor",
    Callback = function(Value)
        aimbot4DColor = Value
        if aimbot4DFovCircle then
            aimbot4DFovCircle.Color = Value
        end
    end
})

-- Texto explicativo
CombatTab:CreateParagraph({
    Title = "ℹ️ Sobre o Aimbot 4D RÁPIDO",
    Content = "✓ SÓ AFETA OUTROS PLAYERS (nunca você)\n✓ DENTRO DO FOV: PUUXA FORTE na cabeça + vai e vem\n✓ FORA DO FOV: Câmera normal (você controla)\n✓ Velocidade: 4.0 (RÁPIDO)\n✓ Amplitude: 1.5 studs\n✓ Força: 0.7 (quase travado)\n✓ Cabeça → Direita → Cabeça → Esquerda → Cabeça"
})

CombatTab:CreateDivider()

-- ============================================
-- ABA VISUAL
-- ============================================
local VisualSection = VisualTab:CreateSection("👁️ ESP Geral")

-- ESP Toggle
local espToggle = VisualTab:CreateToggle({
    Name = "👁️ Ativar ESP (Linhas)",
    CurrentValue = false,
    Flag = "ESPToggle",
    Callback = function(Value)
        espEnabled = Value
        if Value then
            startESP()
        else
            stopESP()
        end
    end
})

-- Cor das linhas ESP
VisualTab:CreateColorPicker({
    Name = "🎨 Cor das Linhas",
    Color = Color3.fromRGB(180, 130, 255),
    Flag = "ESPLineColor",
    Callback = function(Value)
        espLineColor = Value
        for _, line in pairs(espLines) do
            if line then
                line.Color = Value
            end
        end
    end
})

-- Opacidade das linhas
VisualTab:CreateSlider({
    Name = "📊 Opacidade das Linhas",
    Range = {0, 1},
    Increment = 0.05,
    Suffix = "",
    CurrentValue = 0.5,
    Flag = "ESPLineTransparency",
    Callback = function(Value)
        espLineTransparency = Value
        for _, line in pairs(espLines) do
            if line then
                line.Transparency = 1 - Value
            end
        end
    end
})

-- Espessura das linhas
VisualTab:CreateSlider({
    Name = "📏 Espessura das Linhas",
    Range = {1, 5},
    Increment = 1,
    Suffix = "px",
    CurrentValue = 2,
    Flag = "ESPLineThickness",
    Callback = function(Value)
        espLineThickness = Value
        for _, line in pairs(espLines) do
            if line then
                line.Thickness = Value
            end
        end
    end
})

VisualTab:CreateDivider()

-- ============================================
-- ABA VISUAL - CAIXINHA 2D (ESP BOX)
-- ============================================
local Box2DSection = VisualTab:CreateSection("📦 Caixinha 2D (ESP Box)")

-- Função para criar a caixinha 2D
local function createBox2D(player)
    if player == game.Players.LocalPlayer then return end
    
    local rainbowColors = {
        Color3.fromRGB(255, 0, 0),    -- Vermelho
        Color3.fromRGB(255, 165, 0),  -- Laranja
        Color3.fromRGB(255, 255, 0),  -- Amarelo
        Color3.fromRGB(0, 255, 0),    -- Verde
        Color3.fromRGB(0, 0, 255),    -- Azul
        Color3.fromRGB(128, 0, 128),  -- Roxo
        Color3.fromRGB(255, 192, 203) -- Rosa
    }
    
    local colorIndex = 1
    local lastUpdate = tick()
    
    local function updateBox()
        if not box2DEnabled or not player.Character then
            if box2DObjects[player] then
                for _, obj in pairs(box2DObjects[player]) do
                    if obj then obj:Destroy() end
                end
                box2DObjects[player] = nil
            end
            return
        end
        
        local character = player.Character
        local humanoid = character:FindFirstChild("Humanoid")
        local rootPart = character:FindFirstChild("HumanoidRootPart")
        local head = character:FindFirstChild("Head")
        
        if not humanoid or not rootPart or not head or humanoid.Health <= 0 then
            if box2DObjects[player] then
                for _, obj in pairs(box2DObjects[player]) do
                    if obj then obj:Destroy() end
                end
                box2DObjects[player] = nil
            end
            return
        end
        
        local camera = workspace.CurrentCamera
        local isStaffPlayer = isStaff(player)
        
        -- Atualizar cor RGB para staff
        if isStaffPlayer and tick() - lastUpdate > 0.1 then
            colorIndex = colorIndex + 1
            if colorIndex > #rainbowColors then
                colorIndex = 1
            end
            lastUpdate = tick()
        end
        
        -- Posições para calcular a caixa
        local headPos = head.Position
        local rootPos = rootPart.Position
        
        -- Calcular altura total do personagem (dos pés à cabeça)
        local footPos = rootPos - Vector3.new(0, 2, 0) -- Aproximadamente a posição dos pés
        local topPos = headPos + Vector3.new(0, 0.5, 0) -- Um pouco acima da cabeça
        
        -- Converter para posição na tela
        local topPoint, topVisible = camera:WorldToViewportPoint(topPos)
        local footPoint, footVisible = camera:WorldToViewportPoint(footPos)
        
        if not topVisible and not footVisible then
            if box2DObjects[player] then
                for _, obj in pairs(box2DObjects[player]) do
                    if obj then obj.Visible = false end
                end
            end
            return
        end
        
        -- Calcular tamanho da caixa na tela (baseado na altura do personagem)
        local boxHeight = math.abs(topPoint.Y - footPoint.Y)
        local boxWidth = boxHeight * 0.5 -- Proporção largura/altura
        
        -- Posição central da caixa (horizontalmente centralizada no personagem)
        local centerX = topPoint.X
        
        -- Calcular os 4 cantos da caixa
        local left = centerX - boxWidth/2
        local right = centerX + boxWidth/2
        local top = topPoint.Y - 5
        local bottom = footPoint.Y + 5
        
        -- Definir cor atual (RGB para staff, cor escolhida para normais)
        local currentColor = box2DColor
        if isStaffPlayer then
            currentColor = rainbowColors[colorIndex]
        end
        
        -- Se não existir a caixa, criar as linhas
        if not box2DObjects[player] then
            box2DObjects[player] = {}
            
            -- Criar as 4 linhas da caixa
            for i = 1, 4 do
                local line = Drawing.new("Line")
                line.Visible = true
                line.Color = currentColor
                line.Thickness = 2
                line.Transparency = 1
                table.insert(box2DObjects[player], line)
            end
            
            -- Criar o nome do player
            local nameText = Drawing.new("Text")
            nameText.Visible = box2DShowName
            nameText.Color = currentColor
            nameText.Size = 16
            nameText.Center = true
            nameText.Outline = true
            nameText.Font = 2
            nameText.Text = player.Name .. (isStaffPlayer and " [STAFF]" or "")
            table.insert(box2DObjects[player], nameText)
            
            -- Criar texto de distância
            local distText = Drawing.new("Text")
            distText.Visible = true
            distText.Color = Color3.fromRGB(255, 255, 255)
            distText.Size = 12
            distText.Center = true
            distText.Outline = true
            distText.Font = 2
            table.insert(box2DObjects[player], distText)
        end
        
        -- Atualizar posição e cor das linhas
        if box2DObjects[player] then
            -- Linha superior
            local line1 = box2DObjects[player][1]
            line1.From = Vector2.new(left, top)
            line1.To = Vector2.new(right, top)
            line1.Color = currentColor
            line1.Visible = true
            
            -- Linha direita
            local line2 = box2DObjects[player][2]
            line2.From = Vector2.new(right, top)
            line2.To = Vector2.new(right, bottom)
            line2.Color = currentColor
            line2.Visible = true
            
            -- Linha inferior
            local line3 = box2DObjects[player][3]
            line3.From = Vector2.new(right, bottom)
            line3.To = Vector2.new(left, bottom)
            line3.Color = currentColor
            line3.Visible = true
            
            -- Linha esquerda
            local line4 = box2DObjects[player][4]
            line4.From = Vector2.new(left, bottom)
            line4.To = Vector2.new(left, top)
            line4.Color = currentColor
            line4.Visible = true
            
            -- Nome do player
            local nameText = box2DObjects[player][5]
            if nameText then
                nameText.Visible = box2DShowName
                nameText.Color = currentColor
                nameText.Position = Vector2.new(centerX, top - 18)
                nameText.Text = player.Name .. (isStaffPlayer and " [STAFF]" or "")
            end
            
            -- Distância
            local distText = box2DObjects[player][6]
            if distText then
                local distance = (camera.CFrame.Position - rootPos).Magnitude
                distText.Position = Vector2.new(centerX, bottom + 16)
                distText.Text = string.format("%.1f m", distance)
            end
        end
    end
    
    -- Criar conexão para atualizar a caixa
    local connection = game:GetService("RunService").RenderStepped:Connect(updateBox)
    table.insert(box2DConnections, {
        player = player,
        connection = connection
    })
end

-- Toggle da Caixinha 2D
local box2DToggle = VisualTab:CreateToggle({
    Name = "📦 Ativar Caixinha 2D (ESP Box)",
    CurrentValue = false,
    Flag = "Box2DToggle",
    Callback = function(Value)
        box2DEnabled = Value
        
        if Value then
            -- Limpar objetos existentes
            for player, objects in pairs(box2DObjects) do
                for _, obj in ipairs(objects) do
                    if obj then obj:Destroy() end
                end
            end
            box2DObjects = {}
            
            -- Criar caixas para todos os players
            for _, player in ipairs(game.Players:GetPlayers()) do
                if player ~= game.Players.LocalPlayer then
                    createBox2D(player)
                end
            end
            
            -- Conectar eventos de player adicionado/removido
            table.insert(box2DConnections, {
                type = "playerAdded",
                connection = game.Players.PlayerAdded:Connect(function(newPlayer)
                    task.wait(0.5)
                    if box2DEnabled then
                        createBox2D(newPlayer)
                    end
                end)
            })
            
            table.insert(box2DConnections, {
                type = "playerRemoving",
                connection = game.Players.PlayerRemoving:Connect(function(leavingPlayer)
                    if box2DObjects[leavingPlayer] then
                        for _, obj in ipairs(box2DObjects[leavingPlayer]) do
                            if obj then obj:Destroy() end
                        end
                        box2DObjects[leavingPlayer] = nil
                    end
                end)
            })
            
            Lunaris:Notify({
                Title = "📦 Caixinha 2D",
                Content = "Caixinha 2D ativada! STAFF ficam RGB",
                Duration = 3,
                Image = "box"
            })
        else
            -- Remover todas as caixas
            for player, objects in pairs(box2DObjects) do
                for _, obj in ipairs(objects) do
                    if obj then obj:Destroy() end
                end
            end
            box2DObjects = {}
            
            -- Desconectar todas as conexões
            for _, data in ipairs(box2DConnections) do
                if data.connection then
                    data.connection:Disconnect()
                end
            end
            box2DConnections = {}
            
            Lunaris:Notify({
                Title = "📦 Caixinha 2D",
                Content = "Caixinha 2D desativada",
                Duration = 2,
                Image = "box"
            })
        end
    end
})

-- Cor da Caixinha 2D (para players normais)
VisualTab:CreateColorPicker({
    Name = "🎨 Cor da Caixinha (Players Normais)",
    Color = Color3.fromRGB(180, 130, 255),
    Flag = "Box2DColor",
    Callback = function(Value)
        box2DColor = Value
        
        -- Atualizar cor de todas as caixas de players normais
        for player, objects in pairs(box2DObjects) do
            if not isStaff(player) then
                for _, obj in ipairs(objects) do
                    if obj and obj:IsA("Drawing") then
                        if obj.Type == "Line" or (obj.Type == "Text" and obj ~= objects[6]) then
                            obj.Color = Value
                        end
                    end
                end
            end
        end
    end
})

-- Toggle para mostrar nome
VisualTab:CreateToggle({
    Name = "📝 Mostrar Nome do Player",
    CurrentValue = true,
    Flag = "Box2DShowName",
    Callback = function(Value)
        box2DShowName = Value
        
        -- Atualizar visibilidade dos nomes
        for player, objects in pairs(box2DObjects) do
            if objects[5] then
                objects[5].Visible = Value
            end
        end
    end
})

-- Texto explicativo
VisualTab:CreateParagraph({
    Title = "ℹ️ Sobre a Caixinha 2D",
    Content = "✓ Caixa no corpo INTEIRO do jogador\n✓ Tamanho ajusta automaticamente\n✓ STAFF ficam com caixinha RGB piscante\n✓ Mostra nome e distância\n✓ Personalize a cor dos players normais\n✓ Funciona através das paredes"
})

VisualTab:CreateDivider()

-- ============================================
-- ABA VISUAL - ESP STAFF
-- ============================================
local StaffESPSection = VisualTab:CreateSection("👮 ESP Staff")

-- Staff ESP Toggle
local staffEspToggle = VisualTab:CreateToggle({
    Name = "👮 Ativar ESP Staff",
    CurrentValue = false,
    Flag = "StaffESPToggle",
    Callback = function(Value)
        staffEspEnabled = Value
        if Value then
            startStaffESP()
            Lunaris:Notify({
                Title = "👮 ESP Staff",
                Content = "ESP Staff ativado!",
                Duration = 4,
                Image = "users"
            })
        else
            stopStaffESP()
        end
    end
})

VisualTab:CreateDivider()

-- ============================================
-- ABA MUNDO
-- ============================================
local WorldSection = WorldTab:CreateSection("☀️ Controles do Mundo")

-- Dia
WorldTab:CreateButton({
    Name = "☀️ Dia Perfeito",
    Callback = function()
        game:GetService("Lighting").ClockTime = 14
        Lunaris:Notify({
            Title = "🌍 Mundo",
            Content = "Dia ativado!",
            Duration = 2,
            Image = "sun"
        })
    end
})

-- Noite
WorldTab:CreateButton({
    Name = "🌙 Noite Perfeita",
    Callback = function()
        game:GetService("Lighting").ClockTime = 0
        Lunaris:Notify({
            Title = "🌍 Mundo",
            Content = "Noite ativada!",
            Duration = 2,
            Image = "moon"
        })
    end
})

-- ============================================
-- ABA DIVERSOS
-- ============================================
local MiscSection = MiscTab:CreateSection("🎮 Diversos")

-- Discord
MiscTab:CreateButton({
    Name = "💬 Discord",
    Callback = function()
        setclipboard("https://discord.gg/v97Afg9zZj")
        Lunaris:Notify({
            Title = "📱 Discord",
            Content = "Link copiado!",
            Duration = 2,
            Image = "message-circle"
        })
    end
})

-- ============================================
-- ABA CRÉDITOS
-- ============================================
CreditsTab:CreateParagraph({
    Title = "✨ Lunaris Hub",
    Content = "Versão 15.0 - AIMBOT 4D + CAIXINHA 2D!\n• Speed: 0-15 studs extra\n• Aimbot Supremo: Força 1-10 (TRAVA!)\n• Aimbot 4D RÁPIDO\n• Caixinha 2D no corpo INTEIRO\n• STAFF ficam com caixinha RGB\n• ESP Staff (linhas para STAFF)\n• Noclip e muito mais!"
})

-- ============================================
-- FUNÇÕES PRINCIPAIS
-- ============================================

-- Noclip
function startNoclip()
    local player = game.Players.LocalPlayer
    if noclipConnection then
        noclipConnection:Disconnect()
    end
    noclipConnection = game:GetService("RunService").Stepped:Connect(function()
        if noclipEnabled and player.Character then
            for _, v in pairs(player.Character:GetDescendants()) do
                if v:IsA("BasePart") and v.CanCollide then
                    v.CanCollide = false
                end
            end
        end
    end)
end

function stopNoclip()
    if noclipConnection then
        noclipConnection:Disconnect()
        noclipConnection = nil
    end
    local player = game.Players.LocalPlayer
    if player.Character then
        for _, v in pairs(player.Character:GetDescendants()) do
            if v:IsA("BasePart") then
                v.CanCollide = true
            end
        end
    end
end

-- Speed
function startSpeed()
    local player = game.Players.LocalPlayer
    
    if speedConnection then
        speedConnection:Disconnect()
    end
    
    speedConnection = game:GetService("RunService").Heartbeat:Connect(function()
        if speedEnabled and player.Character then
            local humanoid = player.Character:FindFirstChild("Humanoid")
            if humanoid then
                humanoid.WalkSpeed = originalWalkSpeed + speedValue
            end
        end
    end)
end

function stopSpeed()
    if speedConnection then
        speedConnection:Disconnect()
        speedConnection = nil
    end
    
    local player = game.Players.LocalPlayer
    if player and player.Character then
        local humanoid = player.Character:FindFirstChild("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = originalWalkSpeed
        end
    end
end

-- AIMBOT SUPREMO
function startAimbotWorking()
    aimbotConnection = game:GetService("RunService").RenderStepped:Connect(function()
        if not aimbotEnabled then return end
        
        local player = game.Players.LocalPlayer
        local camera = workspace.CurrentCamera
        local viewportSize = camera.ViewportSize
        local centerScreen = Vector2.new(viewportSize.X / 2, viewportSize.Y / 2)
        local closestTarget = nil
        local closestHead = nil
        local shortestDistance = aimbotFov
        
        for _, target in pairs(game.Players:GetPlayers()) do
            if target ~= player and target.Character then
                local head = target.Character:FindFirstChild("Head")
                local humanoid = target.Character:FindFirstChild("Humanoid")
                
                if head and humanoid and humanoid.Health > 0 then
                    local headPos, onScreen = camera:WorldToViewportPoint(head.Position)
                    
                    if onScreen then
                        local distance = (centerScreen - Vector2.new(headPos.X, headPos.Y)).Magnitude
                        
                        if distance < shortestDistance then
                            shortestDistance = distance
                            closestTarget = target
                            closestHead = head
                        end
                    end
                end
            end
        end
        
        if closestTarget and closestHead then
            local headPos = closestHead.Position
            local cameraPos = camera.CFrame.Position
            
            local targetCFrame = CFrame.lookAt(cameraPos, headPos)
            local strength = aimbotStrength / 10
            
            if aimbotStrength == 10 then
                strength = 1.0
            end
            
            camera.CFrame = camera.CFrame:Lerp(targetCFrame, strength)
        end
    end)
end

-- AIMBOT 4D - VAI E VEM RÁPIDO
function startAimbot4D()
    aimbot4DTime = 0
    local movementSpeed = 4.0
    local movementAmplitude = 1.5
    
    aimbot4DConnection = game:GetService("RunService").RenderStepped:Connect(function(dt)
        if not aimbot4DEnabled then return end
        
        aimbot4DTime = aimbot4DTime + dt * movementSpeed
        
        local player = game.Players.LocalPlayer
        local camera = workspace.CurrentCamera
        local viewportSize = camera.ViewportSize
        local centerScreen = Vector2.new(viewportSize.X / 2, viewportSize.Y / 2)
        local targetInFov = false
        local targetHead = nil
        local targetPos = nil
        
        for _, target in pairs(game.Players:GetPlayers()) do
            if target ~= player and target.Character then
                local head = target.Character:FindFirstChild("Head")
                local humanoid = target.Character:FindFirstChild("Humanoid")
                
                if head and humanoid and humanoid.Health > 0 then
                    local headPos, onScreen = camera:WorldToViewportPoint(head.Position)
                    
                    if onScreen then
                        local distance = (centerScreen - Vector2.new(headPos.X, headPos.Y)).Magnitude
                        
                        if distance <= aimbot4DFov then
                            targetInFov = true
                            targetHead = head
                            targetPos = head.Position
                            break
                        end
                    end
                end
            end
        end
        
        if targetInFov and targetHead and targetPos then
            local cameraPos = camera.CFrame.Position
            local targetRight = camera.CFrame.RightVector
            local offsetAmount = math.sin(aimbot4DTime) * movementAmplitude
            local targetPosWithOffset = targetPos + (targetRight * offsetAmount)
            local targetCFrame = CFrame.lookAt(cameraPos, targetPosWithOffset)
            local smoothness = 0.7
            
            camera.CFrame = camera.CFrame:Lerp(targetCFrame, smoothness)
        end
    end)
end

-- ESP (linhas)
function startESP()
    local player = game.Players.LocalPlayer
    
    local function createESPLine(targetPlayer)
        if targetPlayer == player then return end
        
        local line = Drawing.new("Line")
        line.Visible = true
        line.Color = espLineColor
        line.Thickness = espLineThickness
        line.Transparency = 1 - espLineTransparency
        
        table.insert(espLines, line)
        
        local connection
        connection = game:GetService("RunService").RenderStepped:Connect(function()
            if not espEnabled or not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("Head") then
                line.Visible = false
                return
            end
            
            line.Color = espLineColor
            line.Thickness = espLineThickness
            line.Transparency = 1 - espLineTransparency
            
            local targetPos = targetPlayer.Character.Head.Position
            local screenPos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(targetPos)
            
            if onScreen then
                line.Visible = true
                line.From = Vector2.new(screenPos.X, screenPos.Y)
                local viewportSize = workspace.CurrentCamera.ViewportSize
                line.To = Vector2.new(viewportSize.X / 2, viewportSize.Y / 2)
            else
                line.Visible = false
            end
        end)
        
        table.insert(espConnections, {
            player = targetPlayer,
            line = line,
            connection = connection
        })
    end
    
    for _, target in pairs(game.Players:GetPlayers()) do
        createESPLine(target)
    end
    
    table.insert(espConnections, {
        type = "playerAdded",
        connection = game.Players.PlayerAdded:Connect(createESPLine)
    })
    
    table.insert(espConnections, {
        type = "playerRemoving",
        connection = game.Players.PlayerRemoving:Connect(function(targetPlayer)
            for i, data in pairs(espConnections) do
                if data.player == targetPlayer and data.line then
                    data.line:Destroy()
                    table.remove(espConnections, i)
                    break
                end
            end
        end)
    })
end

function stopESP()
    for _, data in pairs(espConnections) do
        if data.connection then
            data.connection:Disconnect()
        end
        if data.line then
            data.line:Destroy()
        end
    end
    espConnections = {}
    espLines = {}
end

-- ESP Staff
function startStaffESP()
    local player = game.Players.LocalPlayer
    
    local staffColors = {
        Color3.fromRGB(255, 0, 0),
        Color3.fromRGB(255, 165, 0),
        Color3.fromRGB(255, 255, 0),
        Color3.fromRGB(0, 255, 0),
        Color3.fromRGB(0, 0, 255),
        Color3.fromRGB(128, 0, 128),
        Color3.fromRGB(255, 192, 203),
        Color3.fromRGB(0, 255, 255),
        Color3.fromRGB(255, 0, 255)
    }
    
    local function createStaffLine(targetPlayer)
        if targetPlayer == player or not isStaff(targetPlayer) then return end
        
        local line = Drawing.new("Line")
        line.Visible = true
        line.Thickness = 4
        line.Transparency = 0.1
        
        table.insert(staffEspLines, line)
        
        local colorIndex = 1
        local lastUpdate = tick()
        
        local connection
        connection = game:GetService("RunService").RenderStepped:Connect(function()
            if not staffEspEnabled or not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("Head") then
                line.Visible = false
                return
            end
            
            if not isStaff(targetPlayer) then
                line.Visible = false
                return
            end
            
            if tick() - lastUpdate > 0.1 then
                colorIndex = colorIndex + 1
                if colorIndex > #staffColors then
                    colorIndex = 1
                end
                line.Color = staffColors[colorIndex]
                lastUpdate = tick()
            end
            
            local targetPos = targetPlayer.Character.Head.Position
            local screenPos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(targetPos)
            local viewportSize = workspace.CurrentCamera.ViewportSize
            local centerScreen = Vector2.new(viewportSize.X / 2, viewportSize.Y / 2)
            
            if onScreen then
                line.Visible = true
                line.From = Vector2.new(screenPos.X, screenPos.Y)
                line.To = centerScreen
            else
                line.Visible = false
            end
        end)
        
        table.insert(staffEspConnections, {
            player = targetPlayer,
            line = line,
            connection = connection
        })
    end
    
    for _, target in pairs(game.Players:GetPlayers()) do
        createStaffLine(target)
    end
    
    table.insert(staffEspConnections, {
        type = "playerAdded",
        connection = game.Players.PlayerAdded:Connect(function(newPlayer)
            task.wait(0.5)
            createStaffLine(newPlayer)
        end)
    })
    
    table.insert(staffEspConnections, {
        type = "playerRemoving",
        connection = game.Players.PlayerRemoving:Connect(function(targetPlayer)
            for i, data in pairs(staffEspConnections) do
                if data.player == targetPlayer then
                    if data.line then
                        data.line:Destroy()
                    end
                    if data.connection then
                        data.connection:Disconnect()
                    end
                    table.remove(staffEspConnections, i)
                    break
                end
            end
        end)
    })
end

function stopStaffESP()
    for _, data in pairs(staffEspConnections) do
        if data.connection then
            data.connection:Disconnect()
        end
        if data.line then
            data.line:Destroy()
        end
    end
    staffEspConnections = {}
    staffEspLines = {}
end

-- Configurar botão de minimizar
setupMinimizeButton()

print("✨ Lunaris Hub carregado com sucesso!")
Lunaris:Notify({
    Title = "✨ Lunaris Hub ✨",
    Content = "Menu carregado! Pressione K\nVersão 15.0 - AIMBOT 4D + CAIXINHA 2D RGB STAFF! 🚀",
    Duration = 5,
    Image = "sparkles"
})
