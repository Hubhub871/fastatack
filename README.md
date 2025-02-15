local enabled = false
local player = game.Players.LocalPlayer

-- Multiplicador da velocidade do ataque (quanto menor, mais rápido)
local attackSpeedMultiplier = 0.2

player.Chatted:Connect(function(msg)
    local args = string.split(msg, " ")
    
    if args[1] == "!fastattack" then
        if args[2] == "on" then
            enabled = true
            game.StarterGui:SetCore("SendNotification", {
                Title = "Fast Attack",
                Text = "Ativado!",
                Duration = 2
            })
        elseif args[2] == "off" then
            enabled = false
            game.StarterGui:SetCore("SendNotification", {
                Title = "Fast Attack",
                Text = "Desativado!",
                Duration = 2
            })
        end
    end
end)

-- Loop para acelerar os ataques
game:GetService("RunService").RenderStepped:Connect(function()
    if enabled then
        local character = player.Character
        if character then
            for _, tool in pairs(character:GetChildren()) do
                if tool:IsA("Tool") and tool:FindFirstChild("Handle") then
                    local attack = tool:FindFirstChildOfClass("Animation")
                    if attack then
                        attack:AdjustSpeed(1 / attackSpeedMultiplier)
                    end
                end
            end
        end
    end
end)
