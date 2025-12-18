local player = game.Players.LocalPlayer
local character = script.Parent
local humanoid = character:WaitForChild("Humanoid")
local root = character:WaitForChild("HumanoidRootPart")
local UIS = game:GetService("UserInputService")

-- CONFIGURAÇÕES
local TECLA_ROLAR = Enum.KeyCode.G
local FORCA_ROLAGEM = 65
local DURACAO_IMPULSO = 0.35
local COOLDOWN = 1.2

-- ANIMAÇÃO (substitua pelo seu ID de rolagem)
local anim = Instance.new("Animation")
anim.AnimationId = "rbxassetid://0000000000" -- coloque sua animação aqui
local rollAnim = humanoid:LoadAnimation(anim)

local podeRolar = true

local function rolar()
	if not podeRolar then return end
	if humanoid.Health <= 0 then return end
	if humanoid.FloorMaterial == Enum.Material.Air then return end

	podeRolar = false

	-- Animação
	rollAnim:Play()

	-- Impulso para frente
	local bv = Instance.new("BodyVelocity")
	bv.MaxForce = Vector3.new(1e5, 0, 1e5)
	bv.Velocity = root.CFrame.LookVector * FORCA_ROLAGEM
	bv.Parent = root

	task.wait(DURACAO_IMPULSO)

	bv:Destroy()
	rollAnim:Stop()

	task.wait(COOLDOWN)
	podeRolar = true
end

UIS.InputBegan:Connect(function(input, gp)
	if gp then return end
	if input.KeyCode == TECLA_ROLAR then
		rolar()
	end
end)
