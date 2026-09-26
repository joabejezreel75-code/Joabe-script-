
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local player = Players.LocalPlayer
local evento = ReplicatedStorage:WaitForChild("Teleportar")

local gui = Instance.new("ScreenGui")
gui.Name = "ProdigiozMods"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- Botao de abrir
local abrir = Instance.new("TextButton")
abrir.Size = UDim2.new(0, 90, 0, 38)
abrir.Position = UDim2.new(0, 15, 0.5, 0)
abrir.Text = "MENU"
abrir.TextColor3 = Color3.new(1, 1, 1)
abrir.BackgroundColor3 = Color3.fromRGB(35, 120, 255)
abrir.Font = Enum.Font.GothamBold
abrir.TextSize = 16
abrir.Parent = gui

Instance.new("UICorner", abrir).CornerRadius =
    UDim.new(0, 8)

-- Painel principal
local painel = Instance.new("Frame")
painel.Size = UDim2.new(0, 290, 0, 280)
painel.Position = UDim2.new(0.5, -145, 0.5, -140)
painel.BackgroundColor3 = Color3.fromRGB(22, 24, 34)
painel.BorderSizePixel = 0
painel.Visible = false
painel.Parent = gui

Instance.new("UICorner", painel).CornerRadius =
    UDim.new(0, 12)

local borda = Instance.new("UIStroke")
borda.Color = Color3.fromRGB(45, 110, 230)
borda.Thickness = 1.5
borda.Parent = painel

-- Titulo
local titulo = Instance.new("TextLabel")
titulo.Size = UDim2.new(1, -50, 0, 45)
titulo.Position = UDim2.new(0, 12, 0, 5)
titulo.BackgroundTransparency = 1
titulo.Text = "PRODIGIOZ MODS"
titulo.TextColor3 = Color3.fromRGB(100, 170, 255)
titulo.Font = Enum.Font.GothamBold
titulo.TextSize = 19
titulo.TextXAlignment = Enum.TextXAlignment.Left
titulo.Parent = painel

-- Fechar
local fechar = Instance.new("TextButton")
fechar.Size = UDim2.new(0, 32, 0, 32)
fechar.Position = UDim2.new(1, -40, 0, 10)
fechar.Text = "X"
fechar.TextColor3 = Color3.new(1, 1, 1)
fechar.BackgroundColor3 = Color3.fromRGB(190, 45, 55)
fechar.Font = Enum.Font.GothamBold
fechar.Parent = painel

Instance.new("UICorner", fechar).CornerRadius =
    UDim.new(0, 7)

-- Subtitulo
local subtitulo = Instance.new("TextLabel")
subtitulo.Size = UDim2.new(1, -20, 0, 25)
subtitulo.Position = UDim2.new(0, 10, 0, 52)
subtitulo.BackgroundTransparency = 1
subtitulo.Text = "TELEPORT MENU"
subtitulo.TextColor3 = Color3.fromRGB(170, 180, 200)
subtitulo.Font = Enum.Font.GothamBold
subtitulo.TextSize = 13
subtitulo.Parent = painel

-- Botoes
local lugares = {
    {Nome = "Spawn", Texto = "TP Spawn"},
    {Nome = "Cidade", Texto = "TP Cidade"},
    {Nome = "Loja", Texto = "TP Loja"},
    {Nome = "Arena", Texto = "TP Arena"}
}

for i, lugar in ipairs(lugares) do
    local botao = Instance.new("TextButton")

    local coluna = (i - 1) % 2
    local linha = math.floor((i - 1) / 2)

    botao.Size = UDim2.new(0, 125, 0, 48)
    botao.Position = UDim2.new(
        0, 13 + coluna * 132,
        0, 90 + linha * 58
    )

    botao.BackgroundColor3 = Color3.fromRGB(40, 105, 190)
    botao.Text = lugar.Texto
    botao.TextColor3 = Color3.new(1, 1, 1)
    botao.Font = Enum.Font.GothamBold
    botao.TextSize = 15
    botao.Parent = painel

    Instance.new("UICorner", botao).CornerRadius =
        UDim.new(0, 8)

    botao.Activated:Connect(function()
        evento:FireServer(lugar.Nome)
    end)
end

-- Abrir e fechar painel
abrir.Activated:Connect(function()
    painel.Visible = not painel.Visible
end)

fechar.Activated:Connect(function()
    painel.Visible = false
end)

-- Arrastar painel
local UserInputService =
    game:GetService("UserInputService")

local arrastando = false
local inicio
local posicao

titulo.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then

        arrastando = true
        inicio = input.Position
        posicao = painel.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if arrastando and (
        input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch
    ) then

        local delta = input.Position - inicio

        painel.Position = UDim2.new(
            posicao.X.Scale,
            posicao.X.Offset + delta.X,
            posicao.Y.Scale,
            posicao.Y.Offset + delta.Y
        )
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
        arrastando = false
    end
end)
