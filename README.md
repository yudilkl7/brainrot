-- Script para teleportar até um Brainrot (NPC/objeto com esse nome)

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Função para teleportar
function teleportTo(target)
    if not target or not target:IsA("BasePart") then return end
    local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    if char and char:FindFirstChild("HumanoidRootPart") then
        char:MoveTo(target.Position + Vector3.new(0, 5, 0)) -- Teleporta um pouco acima para evitar stuck
    end
end

-- Função para enc

