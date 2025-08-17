local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Variáveis de voo
local flying = false
local speed = 50
local bodyGyro
local bodyVelocity

-- Ativar/desativar voo com tecla F
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.F then
		flying = not flying

		if flying then
			-- Ativar voo
			bodyGyro = Instance.new("BodyGyro")
			bodyGyro.P = 9e4
			bodyGyro.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
			bodyGyro.CFrame = humanoidRootPart.CFrame
			bodyGyro.Parent = humanoidRootPart

			bodyVelocity = Instance.new("BodyVelocity")
			bodyVelocity.Velocity = Vector3.new(0, 0, 0)
			bodyVelocity.MaxForce = Vector3.new(9e9, 9e9, 9e9)
			bodyVelocity.Parent = humanoidRootPart

			-- Atualizar movimento
			RunService.RenderStepped:Connect(function()
				if flying then
					local camera = workspace.CurrentCamera
					local direction = Vector3.new()
					if UserInputService:IsKeyDown(Enum.KeyCode.W) then
						direction = direction + camera.CFrame.LookVector
					end
					if UserInputService:IsKeyDown(Enum.KeyCode.S) then
						direction = direction - camera.CFrame.LookVector
					end
					if UserInputService:IsKeyDown(Enum.KeyCode.A) then
						direction = direction - camera.CFrame.RightVector
					end
					if UserInputService:IsKeyDown(Enum.KeyCode.D) then
						direction = direction + camera.CFrame.RightVector
					end
					if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
						direction = direction + camera.CFrame.UpVector
					end
					if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then
						direction = direction - camera.CFrame.UpVector
					end
					bodyVelocity.Velocity = direction.Unit * speed
					bodyGyro.CFrame = camera.CFrame
				end
			end)
		else
			-- Desativar voo
			if bodyGyro then bodyGyro:Destroy() end
			if bodyVelocity then bodyVelocity:Destroy() end
		end
	end
end)

-- Teleporte com clique direito do mouse
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.UserInputType == Enum.UserInputType.MouseButton2 then
		local mouse = player:GetMouse()
		local targetPos = mouse.Hit.Position
		humanoidRootPart.CFrame = CFrame.new(targetPos + Vector3.new(0, 5, 0))
	end
end)
# cOOlkidd-hub
Biscoito
