
--// NOOBIE HUB V3 - INTRO CORRIGIDA + ESP 👀

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local TextChatService = game:GetService("TextChatService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local PURPLE = Color3.fromRGB(170, 40, 255)
local DARK_PURPLE = Color3.fromRGB(80, 20, 120)
local BLACK = Color3.fromRGB(0, 0, 0)
local WHITE = Color3.fromRGB(255, 255, 255)

-- Coloque o ID de uma imagem enviada ao Roblox
local GHOST_IMAGE = "rbxassetid://75660339817032"

local old = playerGui:FindFirstChild("NoobieEditionRemaster")
if old then old:Destroy() end

local gui = Instance.new("ScreenGui")
gui.Name = "NoobieEditionRemaster"
gui.IgnoreGuiInset = true
gui.ResetOnSpawn = false
gui.DisplayOrder = 100
gui.Parent = playerGui

local function corner(obj, radius)
    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, radius)
    c.Parent = obj
end

local function outline(obj, color, thickness)
    local s = Instance.new("UIStroke")
    s.Color = color
    s.Thickness = thickness
    s.Parent = obj
    return s
end

local function tween(obj, duration, props, style, direction)
    local t = TweenService:Create(
        obj,
        TweenInfo.new(
            duration,
            style or Enum.EasingStyle.Quint,
            direction or Enum.EasingDirection.Out
        ),
        props
    )
    t:Play()
    return t
end

--==================================================
-- INTRO
--==================================================

local intro = Instance.new("Frame")
intro.Name = "Intro"
intro.Size = UDim2.fromScale(1, 1)
intro.BackgroundColor3 = BLACK
intro.BorderSizePixel = 0
intro.ZIndex = 50
intro.Parent = gui

local mainText = Instance.new("TextLabel")
mainText.Name = "MainTitle"
mainText.AnchorPoint = Vector2.new(0.5, 0.5)
mainText.Position = UDim2.fromScale(0.5, 0.40)
mainText.Size = UDim2.new(0.96, 0, 0, 100)
mainText.BackgroundTransparency = 1
mainText.Text = "Painel City V2"
mainText.TextColor3 = PURPLE
mainText.TextScaled = true
mainText.Font = Enum.Font.GothamBlack
mainText.TextTransparency = 1
mainText.ZIndex = 51
mainText.Parent = intro

local mainLimit = Instance.new("UITextSizeConstraint")
mainLimit.MaxTextSize = 90
mainLimit.MinTextSize = 20
mainLimit.Parent = mainText

local subText = Instance.new("TextLabel")
subText.Name = "Subtitle"
subText.AnchorPoint = Vector2.new(0.5, 0.5)
subText.Position = UDim2.fromScale(0.5, 0.58)
subText.Size = UDim2.new(0.8, 0, 0, 58)
subText.BackgroundTransparency = 1
subText.Text = "Noobie Hub"
subText.TextColor3 = DARK_PURPLE
subText.TextScaled = true
subText.Font = Enum.Font.GothamBold
subText.TextTransparency = 1
subText.ZIndex = 51
subText.Parent = intro

local subStroke = outline(subText, WHITE, 1.3)
subStroke.Transparency = 1

local particleLayer = Instance.new("Frame")
particleLayer.Name = "Particles"
particleLayer.Size = UDim2.fromScale(1, 1)
particleLayer.BackgroundTransparency = 1
particleLayer.ClipsDescendants = true
particleLayer.ZIndex = 52
particleLayer.Parent = intro

local random = Random.new()
local introRunning = true

local function createParticle()
    if not introRunning or not particleLayer.Parent then
        return
    end

    local dot = Instance.new("Frame")
    local size = random:NextInteger(4, 12)

    dot.AnchorPoint = Vector2.new(0.5, 0.5)
    dot.Size = UDim2.fromOffset(size, size)
    dot.Position = UDim2.fromScale(
        random:NextNumber(0.02, 0.98),
        random:NextNumber(0.1, 0.98)
    )
    dot.BackgroundColor3 = PURPLE
    dot.BackgroundTransparency = random:NextNumber(0.1, 0.45)
    dot.BorderSizePixel = 0
    dot.ZIndex = 52
    dot.Parent = particleLayer

    corner(dot, 20)

    local destinationY = math.max(0, dot.Position.Y.Scale - 0.2)

    tween(dot, random:NextNumber(2, 4), {
        Position = UDim2.fromScale(
            math.clamp(
                dot.Position.X.Scale + random:NextNumber(-0.1, 0.1),
                0,
                1
            ),
            destinationY
        ),
        BackgroundTransparency = 1,
        Size = UDim2.fromOffset(1, 1)
    }, Enum.EasingStyle.Sine)

    task.delay(4.2, function()
        if dot.Parent then dot:Destroy() end
    end)
end

local function createGhost()
    if not introRunning or not particleLayer.Parent then
        return
    end

    local ghost = Instance.new("TextLabel")
    ghost.Name = "FloatingGhost"
    ghost.AnchorPoint = Vector2.new(0.5, 0.5)
    ghost.Size = UDim2.fromOffset(
        random:NextInteger(28, 45),
        random:NextInteger(28, 45)
    )
    ghost.Position = UDim2.fromScale(
        random:NextNumber(0.05, 0.95),
        random:NextNumber(0.2, 0.95)
    )
    ghost.BackgroundTransparency = 1
    ghost.Text = "👻"
    ghost.TextScaled = true
    ghost.TextTransparency = 0.15
    ghost.Rotation = random:NextInteger(-20, 20)
    ghost.ZIndex = 53
    ghost.Parent = particleLayer

    tween(ghost, random:NextNumber(2, 3.5), {
        Position = UDim2.fromScale(
            ghost.Position.X.Scale,
            math.max(0, ghost.Position.Y.Scale - 0.16)
        ),
        TextTransparency = 1,
        Rotation = ghost.Rotation + random:NextInteger(-20, 20)
    }, Enum.EasingStyle.Sine)

    task.delay(3.7, function()
        if ghost.Parent then ghost:Destroy() end
    end)
end

task.spawn(function()
    while introRunning and intro.Parent do
        createParticle()

        if random:NextNumber() > 0.55 then
            createGhost()
        end

        task.wait(0.18)
    end
end)

tween(mainText, 0.9, {
    TextTransparency = 0,
    Position = UDim2.fromScale(0.5, 0.34)
})

task.wait(0.3)

tween(subText, 0.8, {
    TextTransparency = 0,
    Position = UDim2.fromScale(0.5, 0.53)
})
tween(subStroke, 0.8, {Transparency = 0})

task.wait(2.7)

-- Desligar a criação de efeitos antes do fade
introRunning = false

tween(mainText, 0.55, {TextTransparency = 1})
tween(subText, 0.55, {TextTransparency = 1})
tween(subStroke, 0.55, {Transparency = 1})
tween(intro, 0.65, {BackgroundTransparency = 1})

for _, item in ipairs(particleLayer:GetDescendants()) do
    if item:IsA("TextLabel") then
        tween(item, 0.4, {TextTransparency = 1})
    elseif item:IsA("Frame") then
        tween(item, 0.4, {BackgroundTransparency = 1})
    end
end

task.wait(0.7)
intro:Destroy()

--==================================================
-- PAINEL
--==================================================

local panelSize = UDim2.fromOffset(250, 166)

local panel = Instance.new("Frame")
panel.Name = "MainPanel"
panel.AnchorPoint = Vector2.new(0.5, 0.5)
panel.Size = UDim2.fromOffset(10, 10)
panel.Position = UDim2.fromScale(0.5, 0.55)
panel.BackgroundColor3 = BLACK
panel.BackgroundTransparency = 1
panel.BorderSizePixel = 0
panel.Visible = true
panel.ClipsDescendants = true
panel.Parent = gui

corner(panel, 13)
local panelStroke = outline(panel, PURPLE, 2)
panelStroke.Transparency = 1

local header = Instance.new("Frame")
header.Name = "Header"
header.Size = UDim2.new(1, -12, 0, 32)
header.Position = UDim2.fromOffset(6, 6)
header.BackgroundColor3 = Color3.fromRGB(15, 5, 22)
header.BorderSizePixel = 0
header.Parent = panel

corner(header, 8)
outline(header, DARK_PURPLE, 1)

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -38, 1, 0)
title.Position = UDim2.fromOffset(10, 0)
title.BackgroundTransparency = 1
title.Text = "NOOBIE HUB"
title.TextColor3 = PURPLE
title.TextScaled = true
title.Font = Enum.Font.GothamBlack
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = header

local function button(name, text, size, pos)
    local b = Instance.new("TextButton")
    b.Name = name
    b.Size = size
    b.Position = pos
    b.BackgroundColor3 = BLACK
    b.BorderSizePixel = 0
    b.Text = text
    b.TextColor3 = WHITE
    b.TextScaled = true
    b.Font = Enum.Font.GothamBold
    b.AutoButtonColor = false
    b.Parent = panel

    corner(b, 9)
    outline(b, PURPLE, 1.5)

    b.MouseEnter:Connect(function()
        tween(b, 0.15, {
            BackgroundColor3 = Color3.fromRGB(32, 10, 45)
        })
    end)

    b.MouseLeave:Connect(function()
        tween(b, 0.15, {BackgroundColor3 = BLACK})
    end)

    return b
end

local closeButton = button(
    "CloseButton", "×",
    UDim2.fromOffset(25, 25),
    UDim2.new(1, -29, 0, 9)
)

local renderButton = button(
    "RenderButton", "R👻",
    UDim2.fromOffset(70, 49),
    UDim2.fromOffset(12, 48)
)

local skullButton = button(
    "SkullButton", "M👻",
    UDim2.fromOffset(70, 49),
    UDim2.fromOffset(90, 48)
)

local tireButton = button(
    "TireButton", "FP👻",
    UDim2.fromOffset(70, 49),
    UDim2.fromOffset(168, 48)
)

-- Somente ESP 👀: nenhum botão ou código de invisibilidade
local espButton = button(
    "ESPButton", "👁️",
    UDim2.fromOffset(226, 34),
    UDim2.fromOffset(12, 112)
)
espButton.TextSize = 22

-- Ícone que aparece quando o painel fecha
local closed = Instance.new("ImageButton")
closed.Name = "ClosedGhostIcon"
closed.AnchorPoint = Vector2.new(0.5, 0.5)
closed.Size = UDim2.fromOffset(62, 62)
closed.Position = panel.Position
closed.BackgroundColor3 = BLACK
closed.BorderSizePixel = 0
closed.Image = GHOST_IMAGE
closed.ScaleType = Enum.ScaleType.Crop
closed.Visible = false
closed.AutoButtonColor = false
closed.Parent = gui

corner(closed, 50)
local closedStroke = outline(closed, PURPLE, 3)

task.spawn(function()
    while gui.Parent do
        tween(closedStroke, 0.7, {
            Color = Color3.fromRGB(220, 170, 255),
            Thickness = 4
        })
        task.wait(0.7)

        tween(closedStroke, 0.7, {
            Color = PURPLE,
            Thickness = 2
        })
        task.wait(0.7)
    end
end)

--==================================================
-- ABRIR / FECHAR COM ANIMAÇÃO SUAVE
--==================================================

local busy = false
local closedSize = UDim2.fromOffset(62, 62)

local function closePanel()
    if busy or not panel.Visible then return end
    busy = true

    closed.Position = panel.Position
    closed.Size = UDim2.fromOffset(10, 10)
    closed.BackgroundTransparency = 1
    closed.ImageTransparency = 1
    closed.Visible = true

    tween(panelStroke, 0.2, {Transparency = 1})
    tween(panel, 0.3, {
        Size = UDim2.fromOffset(15, 15),
        BackgroundTransparency = 1
    }, Enum.EasingStyle.Back, Enum.EasingDirection.In)

    tween(closed, 0.38, {
        Size = closedSize,
        BackgroundTransparency = 0,
        ImageTransparency = 0
    }, Enum.EasingStyle.Back)

    task.wait(0.32)

    panel.Visible = false
    panel.Size = panelSize
    panel.BackgroundTransparency = 0
    panelStroke.Transparency = 0
    busy = false
end

local function openPanel()
    if busy or not closed.Visible then return end
    busy = true

    panel.Position = closed.Position
    panel.Size = UDim2.fromOffset(12, 12)
    panel.BackgroundTransparency = 1
    panelStroke.Transparency = 1
    panel.Visible = true

    tween(closed, 0.2, {
        Size = UDim2.fromOffset(10, 10),
        ImageTransparency = 1,
        BackgroundTransparency = 1
    }, Enum.EasingStyle.Quad, Enum.EasingDirection.In)

    tween(panel, 0.48, {
        Size = panelSize,
        BackgroundTransparency = 0
    }, Enum.EasingStyle.Back)

    tween(panelStroke, 0.35, {Transparency = 0})

    task.wait(0.23)

    closed.Visible = false
    closed.Size = closedSize
    closed.ImageTransparency = 0
    closed.BackgroundTransparency = 0
    busy = false
end

closeButton.Activated:Connect(closePanel)
closed.Activated:Connect(openPanel)

--==================================================
-- ARRASTAR PAINEL E ÍCONE
--==================================================

local function draggable(object, handle)
    local dragging = false
    local startInput
    local startPos

    handle.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            startInput = input.Position
            startPos = object.Position
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if not dragging then return end

        if input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch then
            local delta = input.Position - startInput
            object.Position = UDim2.new(
                startPos.X.Scale,
                startPos.X.Offset + delta.X,
                startPos.Y.Scale,
                startPos.Y.Offset + delta.Y
            )
        end
    end)

    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
end

draggable(panel, header)
draggable(closed, closed)

-- Entrada do painel somente depois que a intro foi destruída
panel.Position = UDim2.fromScale(0.5, 0.55)
panel.Size = UDim2.fromOffset(12, 12)
panel.BackgroundTransparency = 1
panelStroke.Transparency = 1

tween(panel, 0.5, {
    Size = panelSize,
    BackgroundTransparency = 0
}, Enum.EasingStyle.Back)

tween(panelStroke, 0.4, {Transparency = 0})

--==================================================
-- COMANDOS
--==================================================

local function sendCommand(message)
    local channels = TextChatService:FindFirstChild("TextChannels")
    local channel = channels and (
        channels:FindFirstChild("RBXGeneral")
        or channels:FindFirstChildWhichIsA("TextChannel")
    )

    if not channel then
        warn("NOOBIE HUB: canal de chat não encontrado.")
        return
    end

    local ok, err = pcall(function()
        channel:SendAsync(message)
    end)

    if not ok then
        warn("NOOBIE HUB: erro ao enviar comando:", err)
    end
end

renderButton.Activated:Connect(function()
    sendCommand("//Render 👻")
end)

skullButton.Activated:Connect(function()
    sendCommand("//Tiro Crânio - Mat 👻")
end)

tireButton.Activated:Connect(function()
    sendCommand("//Furar pneu 1x 💀")
end)

--==================================================
-- ESP: DESTAQUE ROXO + DISPLAYNAME + USERNAME
--==================================================

local espEnabled = false
local espObjects = {}

local function removeESP(target)
    local data = espObjects[target]
    if not data then return end

    if data.highlight then data.highlight:Destroy() end
    if data.billboard then data.billboard:Destroy() end

    espObjects[target] = nil
end

local function addESP(target)
    if target == player or not target.Character then return end

    removeESP(target)

    local character = target.Character
    local head = character:FindFirstChild("Head")
    if not head then return end

    local highlight = Instance.new("Highlight")
    highlight.Name = "NoobieESP"
    highlight.Adornee = character
    highlight.FillColor = PURPLE
    highlight.OutlineColor = WHITE
    highlight.FillTransparency = 0.4
    highlight.OutlineTransparency = 0
    highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
    highlight.Parent = character

    local billboard = Instance.new("BillboardGui")
    billboard.Name = "NoobiePlayerName"
    billboard.Adornee = head
    billboard.Size = UDim2.fromOffset(200, 45)
    billboard.StudsOffset = Vector3.new(0, 3, 0)
    billboard.AlwaysOnTop = true
    billboard.Parent = gui

    local label = Instance.new("TextLabel")
    label.Size = UDim2.fromScale(1, 1)
    label.BackgroundTransparency = 1
    label.TextColor3 = WHITE
    label.TextStrokeColor3 = DARK_PURPLE
    label.TextStrokeTransparency = 0
    label.TextScaled = true
    label.Font = Enum.Font.GothamBold
    label.Text = target.DisplayName .. "\n@" .. target.Name
    label.Parent = billboard

    espObjects[target] = {
        highlight = highlight,
        billboard = billboard
    }
end

local function refreshESP()
    for _, target in ipairs(Players:GetPlayers()) do
        if target ~= player then
            if espEnabled then
                addESP(target)
            else
                removeESP(target)
            end
        end
    end
end

espButton.Activated:Connect(function()
    espEnabled = not espEnabled
    espButton.Text = espEnabled and "👀 ON" or "👀"
    refreshESP()
end)

local function setupPlayer(target)
    if target == player then return end

    target.CharacterAdded:Connect(function()
        task.wait(1)
        if espEnabled then
            addESP(target)
        end
    end)
end

for _, target in ipairs(Players:GetPlayers()) do
    setupPlayer(target)
end

Players.PlayerAdded:Connect(setupPlayer)
Players.PlayerRemoving:Connect(removeESP)

player.CharacterAdded:Connect(function()
    if espEnabled then
        task.wait(1)
        refreshESP()
    end
end)

print("NOOBIE HUB V3 carregado!")
