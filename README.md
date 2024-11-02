-- Runner Hub

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 200)
frame.Position = UDim2.new(0.5, -100, 0.5, -100)
frame.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
frame.BorderSizePixel = 0
frame.Parent = screenGui
frame.ClipsDescendants = true

-- Estilo arredondado
local UICorner = Instance.new("UICorner", frame)
UICorner.CornerRadius = UDim.new(0, 15)

-- Botão de fechar
local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -40, 0, 10)
closeButton.Text = "X"
closeButton.Parent = frame

closeButton.MouseButton1Click:Connect(function()
    frame.Visible = false
end)

-- Opção "Local Player"
local playerLabel = Instance.new("TextLabel")
playerLabel.Size = UDim2.new(1, 0, 0, 30)
playerLabel.Text = "Local Player"
playerLabel.BackgroundTransparency = 1
playerLabel.Parent = frame

local speedLabel = Instance.new("TextLabel")
speedLabel.Size = UDim2.new(1, 0, 0, 30)
speedLabel.Position = UDim2.new(0, 0, 0, 35)
speedLabel.Text = "Velocidade:"
speedLabel.BackgroundTransparency = 1
speedLabel.Parent = frame

local speedInput = Instance.new("TextBox")
speedInput.Size = UDim2.new(0, 100, 0, 30)
speedInput.Position = UDim2.new(0, 0, 0, 70)
speedInput.PlaceholderText = "Digite"
speedInput.Parent = frame

local setSpeedButton = Instance.new("TextButton")
setSpeedButton.Size = UDim2.new(0, 80, 0, 30)
setSpeedButton.Position = UDim2.new(0, 110, 0, 70)
setSpeedButton.Text = "Aumentar"
setSpeedButton.Parent = frame

setSpeedButton.MouseButton1Click:Connect(function()
    local speedValue = tonumber(speedInput.Text)
    if speedValue and speedValue > 0 then
        humanoid.WalkSpeed = speedValue
        print("Velocidade aumentada para:", speedValue)
    else
        print("Por favor, insira um número válido.")
    end
end)

-- Opção "Troll"
local trollLabel = Instance.new("TextLabel")
trollLabel.Size = UDim2.new(1, 0, 0, 30)
trollLabel.Position = UDim2.new(0, 0, 0, 110)
trollLabel.Text = "Troll"
trollLabel.BackgroundTransparency = 1
trollLabel.Parent = frame

local banButton = Instance.new("TextButton")
banButton.Size = UDim2.new(1, 0, 0, 30)
banButton.Position = UDim2.new(0, 0, 0, 140)
banButton.Text = "BAN (Button Premium)"
banButton.Parent = frame

banButton.MouseButton1Click:Connect(function()
    if player.MembershipType == Enum.MembershipType.Premium then
        -- Aqui você pode pedir ao usuário para escolher um jogador
        local targetPlayer = game.Players:FindFirstChild("NomeDoJogador") -- Substitua por lógica para escolher um jogador
        
        if targetPlayer then
            -- Tornar o jogador invisível apenas para o jogador que acionou o banimento
            targetPlayer.Character.HumanoidRootPart.Transparency = 1  -- Fica invisível
            targetPlayer.Character.HumanoidRootPart.CanCollide = false  -- Não colide com outros objetos
            
            -- Silenciar o jogador
            targetPlayer:SetAttribute("Muted", true)  -- Usando atributo para silenciar
            
            print(targetPlayer.Name .. " foi silenciado e está invisível para você.")
        else
            print("Jogador não encontrado.")
        end
    else
        print("Você precisa ser Premium para usar este botão.")
    end
end)

-- Permitir mover o Frame
local dragging = false
local dragStart = nil
local startPos = nil

frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = frame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

frame.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        frame.Position = startPos + UDim2.new(0, delta.X, 0, delta.Y)
    end
end)
