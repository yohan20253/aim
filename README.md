-- Script local para detectar o inimigo mais centralizado na câmera (sem mover a mira)
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local camera = workspace.CurrentCamera

-- Retorna o inimigo mais próximo do centro da tela (apenas se estiver visível)
local function getCenteredEnemy()
	local closestEnemy = nil
	local shortestDistance = math.huge
	local screenCenter = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)

	for _, target in ipairs(Players:GetPlayers()) do
		if target ~= player and target.Character and target.Team ~= player.Team then
			local head = target.Character:FindFirstChild("Head")
			if head and head:IsA("BasePart") then
				local screenPoint, onScreen = camera:WorldToScreenPoint(head.Position)
				if onScreen then
					local distance = (Vector2.new(screenPoint.X, screenPoint.Y) - screenCenter).Magnitude
					if distance < shortestDistance then
						shortestDistance = distance
						closestEnemy = target
					end
				end
			end
		end
	end

	return closestEnemy
end

-- Exemplo: printa o nome do inimigo que está no centro da tela
RunService.RenderStepped:Connect(function()
	local enemy = getCenteredEnemy()
	if enemy then
		print("Inimigo no centro da câmera:", enemy.Name)
	else
		-- print("Nenhum inimigo visível")
	end
end)
