-- LocalScript (coloque em StarterPlayerScripts)
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local flying = false
local speed = 50
local bodyGyro, bodyVelocity

-- Criar botão
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "FlyMobileUI"

local button = Instance.new("TextButton", gui)
button.Size = UDim2.new(0, 150, 0, 40)
button.Position = UDim2.new(0, 20, 0.8, 0)
button.Text = "Ativar Voo"
button.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
button.TextColor3 = Color3.new(1, 1, 1)
button.Font = Enum.Font.SourceSansBold
button.TextSize = 16
Instance.new("UICorner", button)

-- Função de voo
local function startFlying()
	character = player.Character or player.CharacterAdded:Wait()
	humanoidRootPart = character:WaitForChild("HumanoidRootPart")

	bodyGyro = Instance.new("BodyGyro")
	bodyGyro.P = 9e4
	bodyGyro.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
	bodyGyro.CFrame = humanoidRootPart.CFrame
	bodyGyro.Parent = humanoidRootPart

	bodyVelocity = Instance.new("BodyVelocity")
	bodyVelocity.Velocity = Vector3.new(0, 0, 0)
	bodyVelocity.MaxForce = Vector3.new(9e9, 9e9, 9e9)
	bodyVelocity.Parent = humanoidRootPart

	RunService.RenderStepped:Connect(function()
		if flying then
			local moveDirection = player:GetMouse().Hit.LookVector
			bodyVelocity.Velocity = moveDirection * speed
			bodyGyro.CFrame = workspace.CurrentCamera.CFrame
		end
	end)
end

-- Ativar/desativar voo
button.MouseButton1Click:Connect(function()
	flying = not flying
	if flying then
		button.Text = "Desativar Voo"
		startFlying()
	else
		button.Text = "Ativar Voo"
		if bodyGyro then bodyGyro:Destroy() end
		if bodyVelocity then bodyVelocity:Destroy() end
	end
end)
