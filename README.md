local Players = game:GetService("Players")
local CoinsFolder = workspace:WaitForChild("Coins")

local COIN_RADIUS = 12 -- distância para coletar
local COIN_VALUE = 1  -- valor da coin

-- Leaderstats
Players.PlayerAdded:Connect(function(player)
	local leaderstats = Instance.new("Folder")
	leaderstats.Name = "leaderstats"
	leaderstats.Parent = player

	local Coins = Instance.new("IntValue")
	Coins.Name = "Coins"
	Coins.Value = 0
	Coins.Parent = leaderstats
end)

-- Loop de verificação
while true do
	task.wait(0.3)

	for _, player in pairs(Players:GetPlayers()) do
		local char = player.Character
		if not char then continue end

		local hrp = char:FindFirstChild("HumanoidRootPart")
		if not hrp then continue end

		for _, coin in pairs(CoinsFolder:GetChildren()) do
			if coin:IsA("BasePart") then
				
				-- Ignora altura (Y)
				local playerPos = Vector3.new(hrp.Position.X, 0, hrp.Position.Z)
				local coinPos = Vector3.new(coin.Position.X, 0, coin.Position.Z)

				local distance = (playerPos - coinPos).Magnitude

				if distance <= COIN_RADIUS then
					player.leaderstats.Coins.Value += COIN_VALUE
					coin:Destroy()
				end
			end
		end
	end
end
