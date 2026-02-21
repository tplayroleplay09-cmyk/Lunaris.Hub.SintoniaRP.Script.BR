-- Lunaris Hub - Script para Sintonia Roleplay
-- Versão SPEED 15 STUDS + AIMBOT SUPREMO
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
local speedValue = 0 -- 0 = normal, 1-15 = studs extra
local speedConnection = nil
local originalWalkSpeed = 16 -- Velocidade padrão do Roblox
local espEnabled = false
local espConnections = {}
local espLines = {}
local espLineColor = Color3.fromRGB(180, 130, 255)
local espLineTransparency = 0.5
local espLineThickness = 2
local staffEspEnabled = false
local staffEspConnections = {}
local staffEspLines = {}
local aimbotEnabled = false
local aimbotConnection = nil
local aimbotFov = 150
local aimbotStrength = 5 -- 1 a 10, quanto maior mais puxa
local fovCircle = nil
local fovColor = Color3.fromRGB(180, 130, 255)
local fovVisible = true

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

-- Speed Slider (0 a 15 studs)
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
            -- Atualizar velocidade em tempo real
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
local CombatSection = CombatTab:CreateSection("🎯 Aimbot (CABEÇA)")

-- Função para criar círculo FOV fixo no centro
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

-- Aimbot Toggle
local aimbotToggle = CombatTab:CreateToggle({
    Name = "🎯 Ativar Aimbot (CABEÇA)",
    CurrentValue = false,
    Flag = "AimbotToggle",
    Callback = function(Value)
        aimbotEnabled = Value
        if Value then
            startAimbotWorking()
            if fovVisible then
                createFovCircle()
            end
            Lunaris:Notify({
                Title = "⚔️ Combate",
                Content = "Aimbot ativado! Força: " .. aimbotStrength .. "/10",
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

-- Toggle FOV
CombatTab:CreateToggle({
    Name = "👁️ Mostrar FOV",
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

-- FOV Slider
CombatTab:CreateSlider({
    Name = "📏 Tamanho do FOV",
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

-- Cor do FOV
CombatTab:CreateColorPicker({
    Name = "🎨 Cor do FOV",
    Color = Color3.fromRGB(180, 130, 255),
    Flag = "FOVColor",
    Callback = function(Value)
        fovColor = Value
        if fovCircle then
            fovCircle.Color = Value
        end
    end
})

-- Slider de Força do Aimbot (1 a 10)
CombatTab:CreateSlider({
    Name = "💪 Força do Aimbot (10 = TRAVA NA CABEÇA)",
    Range = {1, 10},
    Increment = 1,
    Suffix = "",
    CurrentValue = 5,
    Flag = "AimbotStrength",
    Callback = function(Value)
        aimbotStrength = Value
        Lunaris:Notify({
            Title = "🎯 Aimbot",
            Content = "Força ajustada para: " .. Value .. "/10",
            Duration = 1,
            Image = "target"
        })
    end
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
-- ABA VISUAL - ESP STAFF PISCANTE
-- ============================================
local StaffESPSection = VisualTab:CreateSection("👮 ESP Staff Piscante")

-- Staff ESP Toggle
local staffEspToggle = VisualTab:CreateToggle({
    Name = "🌈 Ativar ESP Staff PISCANTE",
    CurrentValue = false,
    Flag = "StaffESPToggle",
    Callback = function(Value)
        staffEspEnabled = Value
        if Value then
            startStaffESP()
            Lunaris:Notify({
                Title = "🌈 Staff ESP",
                Content = "ESP Staff PISCANTE ativado!",
                Duration = 4,
                Image = "rainbow"
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
    Content = "Versão 9.0 - SPEED 15 STUDS!\nSpeed: 0-15 studs extra (até 31 studs total!)\nAimbot: 1-10 (10 = TRAVA NA CABEÇA!)"
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

-- Speed (até 15 studs)
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

-- AIMBOT - FORÇA REAL (1 = leve, 10 = TRAVA)
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
        
        -- Encontrar o alvo mais próximo do centro da tela
        for _, target in pairs(game.Players:GetPlayers()) do
            if target ~= player and target.Character then
                local head = target.Character:FindFirstChild("Head")
                local humanoid = target.Character:FindFirstChild("Humanoid")
                
                if head and humanoid and humanoid.Health > 0 then
                    -- Verificar se o alvo está na tela
                    local headPos, onScreen = camera:WorldToViewportPoint(head.Position)
                    
                    if onScreen then
                        -- Calcular distância do centro da tela
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
        
        -- Mover a câmera com força baseada no slider (1-10)
        if closestTarget and closestHead then
            local headPos = closestHead.Position
            local cameraPos = camera.CFrame.Position
            
            -- Calcular a direção para a cabeça do alvo
            local targetCFrame = CFrame.lookAt(cameraPos, headPos)
            
            -- Força real (1 = 0.1, 5 = 0.5, 10 = 1.0)
            local strength = aimbotStrength / 10
            
            -- Se for 10, usar 1.0 para travar completamente
            if aimbotStrength == 10 then
                strength = 1.0
            end
            
            -- Interpolar entre a rotação atual e a rotação alvo
            camera.CFrame = camera.CFrame:Lerp(targetCFrame, strength)
        end
    end)
end

-- ESP
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

-- Staff ESP Piscante
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
    
    local function isStaffByName(targetPlayer)
        if targetPlayer.Name and string.find(targetPlayer.Name:upper(), "STAFF") then
            return true
        end
        if targetPlayer.DisplayName and string.find(targetPlayer.DisplayName:upper(), "STAFF") then
            return true
        end
        return false
    end
    
    local function createStaffLine(targetPlayer)
        if targetPlayer == player or not isStaffByName(targetPlayer) then return end
        
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
            
            if not isStaffByName(targetPlayer) then
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
    Content = "Menu carregado! Pressione K\nVersão 9.0 - SPEED 15 STUDS! 🚀",
    Duration = 5,
    Image = "sparkles"
})
