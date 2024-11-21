lua
local player = game.Players.LocalPlayer
local backpack = player.Backpack
local gui = Instance.new("ScreenGui", player.PlayerGui)

-- Lista de armas (substitua pelos nomes reais das armas no jogo)
local armas = {
    "Pistola",
    "M4A!",
    "Arma3",
}

-- Função para criar botões de ativar/desativar
local function createToggleButton(armaName, position)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 200, 0, 50)
    button.Position = position
    button.Text = "Ativar " .. armaName
    button.Parent = gui

    button.MouseButton1Click:Connect(function()
        local armaInstance = backpack:FindFirstChild(armaName)

        if armaInstance then
            armaInstance.Parent = nil -- Remove a arma (desativa)
            button.Text = "Ativar " .. armaName -- Atualiza o texto do botão
        else
            local storedArma = game.ServerStorage:FindFirstChild(armaName)
            if storedArma then
                local clone = storedArma:Clone()
                clone.Parent = backpack -- Adiciona a arma (ativa)
                button.Text = "Desativar " .. armaName -- Atualiza o texto do botão
            else
                print(armaName .. " não encontrada.")
            end
        end
    end)
end

-- Criar botões para cada arma na lista
for i, arma in ipairs(armas) do
    createToggleButton(arma, UDim2.new(0.5, -100, 0.1 + (i - 1) * 0.1, 0))
end
