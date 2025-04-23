local TweenService = game:GetService("TweenService")

local ScreenGui = Instance.new("ScreenGui")
local main = Instance.new("Frame")
local label = Instance.new("TextLabel")
local Hitbox = Instance.new("TextButton")

ScreenGui.Parent = game.CoreGui

-- Main Frame
main.Name = "main"
main.Parent = ScreenGui
main.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
main.Position = UDim2.new(0.4, 0, 0.35, 0)
main.Size = UDim2.new(0, 200, 0, 150)
main.Active = true
main.Draggable = true
main.BorderSizePixel = 0
main.BackgroundTransparency = 0.1

-- Label (Título)
label.Name = "label"
label.Parent = main
label.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
label.Size = UDim2.new(1, 0, 0, 30)
label.Font = Enum.Font.GothamBold
label.Text = "Sticky Xit"
label.TextColor3 = Color3.fromRGB(255, 255, 255)
label.TextScaled = true
label.TextWrapped = true
label.BorderSizePixel = 0

-- Botão
Hitbox.Name = "Hitbox"
Hitbox.Parent = main
Hitbox.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
Hitbox.Position = UDim2.new(0.1, 0, 0.5, 0)
Hitbox.Size = UDim2.new(0.8, 0, 0, 50)
Hitbox.Font = Enum.Font.GothamSemibold
Hitbox.Text = "Ativar Xit (0%)"
Hitbox.TextColor3 = Color3.fromRGB(255, 255, 255)
Hitbox.TextScaled = true
Hitbox.BorderSizePixel = 0
Hitbox.AutoButtonColor = true
Hitbox.BackgroundTransparency = 0.05

-- Variável de contagem
local count = 0
_G.Disabled = false

-- Animação de clique
local function animateButton(btn)
	local shrink = TweenService:Create(btn, TweenInfo.new(0.1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
		Size = UDim2.new(0.75, 0, 0, 45)
	})
	local grow = TweenService:Create(btn, TweenInfo.new(0.1, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
		Size = UDim2.new(0.8, 0, 0, 50)
	})

	shrink:Play()
	shrink.Completed:Connect(function()
		grow:Play()
	end)
end

-- Função do botão
Hitbox.MouseButton1Down:Connect(function()
	animateButton(Hitbox)

	count += 0.5
	_G.HeadSize = 75 * (1 + count)
	_G.Disabled = true
	Hitbox.Text = "Ativar Xit (" .. math.floor(count * 100) .. "%)"

	game:GetService('RunService').RenderStepped:Connect(function()
		if _G.Disabled then
			for _, v in pairs(game:GetService('Players'):GetPlayers()) do
				if v.Name ~= game.Players.LocalPlayer.Name then
					pcall(function()
						local hrp = v.Character and v.Character:FindFirstChild("HumanoidRootPart")
						if hrp then
							hrp.Size = Vector3.new(_G.HeadSize, _G.HeadSize, _G.HeadSize)
							hrp.Transparency = 1
							hrp.BrickColor = BrickColor.new("Really black")
							hrp.Material = Enum.Material.Neon
							hrp.CanCollide = false
						end
					end)
				end
			end
		end
	end)
end)
