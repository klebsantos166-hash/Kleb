local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

-- ScreenGui
local gui = Instance.new("ScreenGui")
gui.Name = "FraseColorida"
gui.ResetOnSpawn = false
gui.Parent = game.CoreGui

-- Frame
local frame = Instance.new("Frame")
frame.Parent = gui
frame.Size = UDim2.new(0, 320, 0, 90)
frame.Position = UDim2.new(0.5, -160, 0.15, 0)
frame.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
frame.BorderSizePixel = 0

-- Cantos arredondados
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 10)
corner.Parent = frame

-- Texto
local text = Instance.new("TextLabel")
text.Parent = frame
text.Size = UDim2.new(1, -16, 1, -16)
text.Position = UDim2.new(0, 8, 0, 8)
text.BackgroundTransparency = 1
text.TextWrapped = true
text.TextScaled = true
text.Font = Enum.Font.GothamBold
text.TextColor3 = Color3.fromRGB(255, 255, 255)
text.Text = [[
Mil cairão ao teu lado, dez mil à tua direita,
mas tu não serás atingido.
Salmos 91:7
]]

-- 🌈 CORES FORTES ANIMADAS
task.spawn(function()
	while true do
		for hue = 0, 1, 0.01 do
			local color = Color3.fromHSV(hue, 1, 1)
			TweenService:Create(
				frame,
				TweenInfo.new(0.3, Enum.EasingStyle.Linear),
				{BackgroundColor3 = color}
			):Play()
			task.wait(0.05)
		end
	end
end)

-- 🖱️ Arrastar (PC + Mobile)
local dragging, dragStart, startPos

frame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
	or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = frame.Position
	end
end)

frame.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
	or input.UserInputType == Enum.UserInputType.Touch then
		dragging = false
	end
end)

UIS.InputChanged:Connect(function(input)
	if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement
	or input.UserInputType == Enum.UserInputType.Touch) then
		local delta = input.Position - dragStart
		frame.Position = UDim2.new(
			startPos.X.Scale,
			startPos.X.Offset + delta.X,
			startPos.Y.Scale,
			startPos.Y.Offset + delta.Y
		)
	end
end)
