# brainrot# Interface de controle
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

local espEnabled = true
local teamCheck = true

--# Interface Simples
local ScreenGui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
ScreenGui.Name = "ESP_GUI"

local toggleButton = Instance.new("TextButton", ScreenGui)
toggleButton.Size = UDim2.new(0, 100, 0, 40)
toggleButton.Position = UDim2.new(0, 10, 0, 10)
toggleButton.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
toggleButton.TextColor3 = Color3.new(1, 1, 1)
toggleButton.Text = "ESP ON"

toggleButton.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    toggleButton.Text = espEnabled and "ESP ON" or "ESP OFF"
end)

--# Função para criar o ESP
function createESP(player)
    if player == LocalPlayer then return end
    local BillboardGui = Instance.new("BillboardGui")
    BillboardGui.Name = "ESP_GUI"
    BillboardGui.Adornee = player.Character:WaitForChild("Head")
    BillboardGui.Size = UDim2.new(0, 200, 0, 50)
    BillboardGui.StudsOffset = Vector3.new(0, 3, 0)
    BillboardGui.AlwaysOnTop = true
    BillboardGui.Parent = player.Character

    local nameLabel = Instance.new("TextLabel", BillboardGui)
    nameLabel.Size = UDim2.new(1, 0, 1, 0)
    nameLabel.BackgroundTransparency = 1
    nameLabel.TextColor3 = Color3.new(1, 1, 1)
    nameLabel.TextStrokeTransparency = 0
    nameLabel.Text = player.Name
    nameLabel.TextScaled = true
end

--# Atualiza e aplica o ESP
function updateESP()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
            if teamCheck and player.Team == LocalPlayer.Team then
                -- mesmo time, pular
                continue
            end
            if not player.Character:FindFirstChild("ESP_GUI") then
                createESP(player)
            end
        end
    end
end

--# Atualização constante
RunService.RenderStepped:Connect(function()
    if not espEnabled then
        -- remove ESP se desligado
        for _, player in pairs(Players:GetPlayers()) do
            if player.Character and player.Character:FindFirstChild("ESP_GUI") then
                player.Character:FindFirstChild("ESP_GUI"):Destroy()
            end
        end
        return
    end
    updateESP()
end)

--# Novo jogador entra
Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        wait(1)
        if espEnabled then
            updateESP()
        end
    end)
end)
