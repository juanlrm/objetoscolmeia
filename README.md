local nomes = {}
for _,obj in pairs(workspace:GetDescendants()) do
    if string.find(obj:GetFullName(), "Hive") or string.find(obj:GetFullName(), "colmeia") then
        table.insert(nomes, obj:GetFullName())
    end
end

local sg = Instance.new("ScreenGui", game:GetService("CoreGui"))
sg.Name = "ColmeiasListaSG"
local frame = Instance.new("Frame", sg)
frame.Size = UDim2.new(0, 600, 0, 400)
frame.Position = UDim2.new(0.5, -300, 0.5, -200)
frame.BackgroundColor3 = Color3.fromRGB(30,30,30)
frame.BackgroundTransparency = 0.2
frame.Active = true
frame.Name = "ColmeiasFrame"

local frameCorner = Instance.new("UICorner", frame)
frameCorner.CornerRadius = UDim.new(0.06,0)

local label = Instance.new("TextLabel", frame)
label.Size = UDim2.new(1, -40, 0, 40)
label.Position = UDim2.new(0, 0, 0, 0)
label.BackgroundTransparency = 1
label.TextColor3 = Color3.fromRGB(220,30,30)
label.TextSize = 18
label.Font = Enum.Font.FredokaOne
label.Text = "Lista de colmeias - copie, edite e cole!"
label.TextXAlignment = Enum.TextXAlignment.Left

-- Botão de fechar
local closeBtn = Instance.new("TextButton", frame)
closeBtn.Size = UDim2.new(0, 36, 0, 36)
closeBtn.Position = UDim2.new(1, -38, 0, 2)
closeBtn.BackgroundTransparency = 0.15
closeBtn.BackgroundColor3 = Color3.fromRGB(220,30,30)
closeBtn.TextColor3 = Color3.fromRGB(255,255,255)
closeBtn.Font = Enum.Font.FredokaOne
closeBtn.TextSize = 22
closeBtn.Text = "X"
local btnCorner = Instance.new("UICorner", closeBtn)
btnCorner.CornerRadius = UDim.new(0.5,0)

closeBtn.MouseButton1Click:Connect(function()
    sg:Destroy()
end)

-- Caixa de texto editável e copiável
local textBox = Instance.new("TextBox", frame)
textBox.Size = UDim2.new(1, -20, 1, -60)
textBox.Position = UDim2.new(0, 10, 0, 50)
textBox.BackgroundColor3 = Color3.fromRGB(40,40,40)
textBox.TextColor3 = Color3.fromRGB(220,220,220)
textBox.TextSize = 14
textBox.Font = Enum.Font.Code
textBox.TextXAlignment = Enum.TextXAlignment.Left
textBox.TextYAlignment = Enum.TextYAlignment.Top
textBox.TextWrapped = true
textBox.TextEditable = true
textBox.MultiLine = true
textBox.Text = table.concat(nomes, "\n")
textBox.ClearTextOnFocus = false

local boxCorner = Instance.new("UICorner", textBox)
boxCorner.CornerRadius = UDim.new(0.06,0)

-- Permite arrastar o quadro
local dragging, dragInput, dragStart, startPos
frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 and input.Position.Y < (frame.AbsolutePosition.Y + 40) then
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
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)
game:GetService("UserInputService").InputChanged:Connect(function(input)
    if dragging and input == dragInput then
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
