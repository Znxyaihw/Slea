--[[ 
🔹 Script Administrativo (feito para o dono do jogo)
🔹 Autor: ChatGPT (para uso do dono do jogo)
🔹 Compatível com executores como Delta
]]

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- 🧠 Configure aqui o nome do dono do jogo (você):
local DonoDoJogo = "JONATHA132820" -- << coloque seu nome exato no Roblox

if LocalPlayer.Name ~= DonoDoJogo then
    return -- impede que outros jogadores usem o script
end

-- 🧱 Criação da GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "PainelAdmin"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 300, 0, 420)
MainFrame.Position = UDim2.new(0.3, 0, 0.3, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 8)

local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, -40, 0, 30)
Title.Position = UDim2.new(0, 10, 0, 5)
Title.BackgroundTransparency = 1
Title.Text = "Painel Administrativo"
Title.TextColor3 = Color3.fromRGB(0, 255, 150)
Title.TextScaled = true
Title.Font = Enum.Font.GothamBold

local CloseButton = Instance.new("TextButton", MainFrame)
CloseButton.Size = UDim2.new(0, 25, 0, 25)
CloseButton.Position = UDim2.new(1, -30, 0, 5)
CloseButton.Text = "X"
CloseButton.TextScaled = true
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

local MinButton = Instance.new("TextButton", MainFrame)
MinButton.Size = UDim2.new(0, 25, 0, 25)
MinButton.Position = UDim2.new(1, -60, 0, 5)
MinButton.Text = "-"
MinButton.TextScaled = true
MinButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
MinButton.Font = Enum.Font.GothamBold

local ContentFrame = Instance.new("Frame", MainFrame)
ContentFrame.Size = UDim2.new(1, -20, 1, -60)
ContentFrame.Position = UDim2.new(0, 10, 0, 50)
ContentFrame.BackgroundTransparency = 1

local function setMinimized(min)
    for _, obj in ipairs(ContentFrame:GetChildren()) do
        obj.Visible = not min
    end
    MainFrame.Size = min and UDim2.new(0, 300, 0, 50) or UDim2.new(0, 300, 0, 420)
end

local minimized = false
MinButton.MouseButton1Click:Connect(function()
    minimized = not minimized
    setMinimized(minimized)
end)

-- ⚔️ Botões de Ação
local KillAllButton = Instance.new("TextButton", ContentFrame)
KillAllButton.Size = UDim2.new(1, 0, 0, 40)
KillAllButton.Position = UDim2.new(0, 0, 0, 0)
KillAllButton.Text = "⚠️ Matar Todos (menos eu)"
KillAllButton.Font = Enum.Font.GothamBold
KillAllButton.TextColor3 = Color3.fromRGB(255, 255, 255)
KillAllButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
KillAllButton.AutoButtonColor = true
Instance.new("UICorner", KillAllButton)

-- 🧾 Lista de Jogadores
local PlayerListFrame = Instance.new("ScrollingFrame", ContentFrame)
PlayerListFrame.Size = UDim2.new(1, 0, 0, 150)
PlayerListFrame.Position = UDim2.new(0, 0, 0, 50)
PlayerListFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
PlayerListFrame.ScrollBarThickness = 6
PlayerListFrame.BackgroundTransparency = 0.2
PlayerListFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
Instance.new("UICorner", PlayerListFrame)

local UIListLayout = Instance.new("UIListLayout", PlayerListFrame)
UIListLayout.Padding = UDim.new(0, 4)
UIListLayout.SortOrder = Enum.SortOrder.Name

local selectedPlayer = nil

local function refreshPlayerList()
    for _, c in ipairs(PlayerListFrame:GetChildren()) do
        if c:IsA("TextButton") then
            c:Destroy()
        end
    end

    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer then
            local Btn = Instance.new("TextButton", PlayerListFrame)
            Btn.Size = UDim2.new(1, -10, 0, 30)
            Btn.Text = plr.Name
            Btn.Font = Enum.Font.Gotham
            Btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            Btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
            Instance.new("UICorner", Btn)

            Btn.MouseButton1Click:Connect(function()
                selectedPlayer = plr
                for _, b in ipairs(PlayerListFrame:GetChildren()) do
                    if b:IsA("TextButton") then
                        b.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
                    end
                end
                Btn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
            end)
        end
    end
    PlayerListFrame.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y)
end

Players.PlayerAdded:Connect(refreshPlayerList)
Players.PlayerRemoving:Connect(refreshPlayerList)
refreshPlayerList()

-- 🧍‍♂️ Configurar Hitbox
local HitboxLabel = Instance.new("TextLabel", ContentFrame)
HitboxLabel.Size = UDim2.new(1, 0, 0, 30)
HitboxLabel.Position = UDim2.new(0, 0, 0, 210)
HitboxLabel.BackgroundTransparency = 1
HitboxLabel.Text = "Tamanho da Hitbox (5-100):"
HitboxLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
HitboxLabel.Font = Enum.Font.Gotham
HitboxLabel.TextScaled = true

local HitboxBox = Instance.new("TextBox", ContentFrame)
HitboxBox.Size = UDim2.new(1, 0, 0, 30)
HitboxBox.Position = UDim2.new(0, 0, 0, 240)
HitboxBox.PlaceholderText = "Digite o tamanho"
HitboxBox.Text = ""
HitboxBox.TextScaled = true
HitboxBox.Font = Enum.Font.Gotham
HitboxBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
HitboxBox.TextColor3 = Color3.fromRGB(255, 255, 255)
Instance.new("UICorner", HitboxBox)

local ToggleButton = Instance.new("TextButton", ContentFrame)
ToggleButton.Size = UDim2.new(1, 0, 0, 40)
ToggleButton.Position = UDim2.new(0, 0, 0, 280)
ToggleButton.Text = "Ativar Hitbox"
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.BackgroundColor3 = Color3.fromRGB(50, 150, 250)
Instance.new("UICorner", ToggleButton)

local hitboxAtiva = false
local hitboxPart = nil

local function toggleHitbox()
    if not selectedPlayer or not selectedPlayer.Character or not selectedPlayer.Character:FindFirstChild("HumanoidRootPart") then
        warn("Selecione um jogador válido.")
        return
    end

    if not hitboxAtiva then
        local size = tonumber(HitboxBox.Text) or 5
        size = math.clamp(size, 5, 100)

        if not hitboxPart then
            hitboxPart = Instance.new("Part")
            hitboxPart.Anchored = false
            hitboxPart.CanCollide = false
            hitboxPart.Transparency = 1
            hitboxPart.Size = Vector3.new(size, size, size)
            hitboxPart.Name = "HitboxInvisivel"
            hitboxPart.Parent = selectedPlayer.Character
        end

        local hrp = selectedPlayer.Character:FindFirstChild("HumanoidRootPart")
        hitboxPart.CFrame = hrp.CFrame
        hitboxPart.Size = Vector3.new(size, size, size)
        hitboxPart.Transparency = 1

        local weld = Instance.new("WeldConstraint", hitboxPart)
        weld.Part0 = hrp
        weld.Part1 = hitboxPart

        hitboxAtiva = true
        ToggleButton.Text = "Desativar Hitbox"
        ToggleButton.BackgroundColor3 = Color3.fromRGB(250, 100, 100)
    else
        if hitboxPart then
            hitboxPart:Destroy()
            hitboxPart = nil
        end
        hitboxAtiva = false
        ToggleButton.Text = "Ativar Hitbox"
        ToggleButton.BackgroundColor3 = Color3.fromRGB(50, 150, 250)
    end
end

ToggleButton.MouseButton1Click:Connect(toggleHitbox)

KillAllButton.MouseButton1Click:Connect(function()
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("Humanoid") then
            plr.Character.Humanoid.Health = 0
        end
    end
end)
