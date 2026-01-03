local p = game:GetService("Players").LocalPlayer
local rs = game:GetService("RunService")
local cg = game:GetService("CoreGui")
local userInput = game:GetService("UserInputService")

local parent = cg:FindFirstChild("RobloxGui") or p:WaitForChild("PlayerGui")
local sg = Instance.new("ScreenGui", parent)
sg.Name = "CyberExposer_Elite_V7"

local f = Instance.new("Frame")
f.Size = UDim2.new(0, 550, 0, 380)
f.Position = UDim2.new(0.5, -275, 0.4, -190)
f.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
f.BackgroundTransparency = 0.1
f.Active = true
f.Visible = false
f.Parent = sg
Instance.new("UICorner", f).CornerRadius = UDim.new(0, 12)

-- LÓGICA DAS BORDAS LED RGB (NEON)
local s = Instance.new("UIStroke")
s.Thickness = 3.5 -- Borda mais grossa para destacar o LED
s.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
s.Parent = f

local uig = Instance.new("UIGradient")
uig.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 0)),
    ColorSequenceKeypoint.new(0.2, Color3.fromRGB(255, 255, 0)),
    ColorSequenceKeypoint.new(0.4, Color3.fromRGB(0, 255, 0)),
    ColorSequenceKeypoint.new(0.6, Color3.fromRGB(0, 255, 255)),
    ColorSequenceKeypoint.new(0.8, Color3.fromRGB(0, 0, 255)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 0, 255))
}
uig.Parent = s

-- Barra de Título (Arraste)
local t = Instance.new("TextLabel")
t.Size = UDim2.new(1, 0, 0, 40)
t.Text = "⚡ CYBERNODRY EXPOSER V7 [ELITE] ⚡"
t.TextColor3 = Color3.new(1, 1, 1)
t.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
t.BackgroundTransparency = 0.5
t.Font = Enum.Font.GothamBold
t.TextSize = 14
t.Parent = f
Instance.new("UICorner", t).CornerRadius = UDim.new(0, 12)

-- Botão Auto-Roll
local scrollBtn = Instance.new("TextButton")
scrollBtn.Size = UDim2.new(0, 110, 0, 26)
scrollBtn.Position = UDim2.new(1, -125, 0, 7)
scrollBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
scrollBtn.Text = "AUTO ROLL: OFF"
scrollBtn.TextColor3 = Color3.new(1, 1, 1)
scrollBtn.Font = Enum.Font.GothamBold
scrollBtn.TextSize = 10
scrollBtn.Parent = f
Instance.new("UICorner", scrollBtn).CornerRadius = UDim.new(0, 6)
local scrollStroke = Instance.new("UIStroke", scrollBtn)
scrollStroke.Thickness = 1.5
scrollStroke.Color = Color3.fromRGB(70, 70, 70)

-- Botão de Resize (Canto inferior)
local resizer = Instance.new("TextButton")
resizer.Size = UDim2.new(0, 25, 0, 25)
resizer.Position = UDim2.new(1, -25, 1, -25)
resizer.BackgroundTransparency = 1
resizer.Text = "◢"
resizer.TextSize = 25
resizer.TextColor3 = Color3.fromRGB(200, 200, 200)
resizer.ZIndex = 10
resizer.Parent = f

-- Listagem de Objetos
local listFrame = Instance.new("ScrollingFrame")
listFrame.Size = UDim2.new(0.38, 0, 0.82, 0)
listFrame.Position = UDim2.new(0, 10, 0, 50)
listFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
listFrame.BackgroundTransparency = 0.3
listFrame.BorderSizePixel = 0
listFrame.ScrollBarThickness = 2
listFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
listFrame.Parent = f
Instance.new("UIListLayout", listFrame).Padding = UDim.new(0, 4)

-- Tela de Código/Detalhes
local detailFrame = Instance.new("ScrollingFrame")
detailFrame.Size = UDim2.new(0.58, 0, 0.82, 0)
detailFrame.Position = UDim2.new(0.4, 5, 0, 50)
detailFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
detailFrame.BorderSizePixel = 0
detailFrame.ScrollBarThickness = 2
detailFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
detailFrame.Parent = f

local detailText = Instance.new("TextBox")
detailText.Size = UDim2.new(1, -15, 1, 0)
detailText.Position = UDim2.new(0, 10, 0, 5)
detailText.MultiLine = true
detailText.TextEditable = false
detailText.TextColor3 = Color3.fromRGB(0, 255, 180)
detailText.Font = Enum.Font.Code
detailText.TextSize = 13
detailText.TextXAlignment = Enum.TextXAlignment.Left
detailText.TextYAlignment = Enum.TextYAlignment.Top
detailText.BackgroundTransparency = 1
detailText.ClearTextOnFocus = false
detailText.Text = "-- Equipe uma Tool para começar."
detailText.Parent = detailFrame

-- SISTEMA DE MOVIMENTAÇÃO E RESIZE
local function sync(obj, trigger, mode)
    local active, startPos, startObj
    trigger.InputBegan:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
            active = true; startPos = i.Position; startObj = (mode == "drag" and obj.Position or obj.Size)
        end
    end)
    userInput.InputChanged:Connect(function(i)
        if active and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
            local delta = i.Position - startPos
            if mode == "drag" then
                obj.Position = UDim2.new(startObj.X.Scale, startObj.X.Offset + delta.X, startObj.Y.Scale, startObj.Y.Offset + delta.Y)
            else
                obj.Size = UDim2.new(0, math.max(400, startObj.X.Offset + delta.X), 0, math.max(250, startObj.Y.Offset + delta.Y))
            end
        end
    end)
    userInput.InputEnded:Connect(function(i)
        if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then active = false end
    end)
end

sync(f, t, "drag")
sync(f, resizer, "size")

-- ANIMAÇÃO DAS BORDAS LED (GIRAR CORES)
rs.RenderStepped:Connect(function()
    uig.Rotation = uig.Rotation + 3
end)

-- LÓGICA AUTO-ROLL
local scrollingActive = false
scrollBtn.MouseButton1Click:Connect(function()
    scrollingActive = not scrollingActive
    scrollBtn.Text = scrollingActive and "AUTO ROLL: ON" or "AUTO ROLL: OFF"
    scrollStroke.Color = scrollingActive and Color3.fromRGB(0, 255, 120) or Color3.fromRGB(70, 70, 70)
end)

rs.RenderStepped:Connect(function()
    if scrollingActive and detailFrame.CanvasPosition.Y < detailFrame.AbsoluteCanvasSize.Y - detailFrame.AbsoluteWindowSize.Y then
        detailFrame.CanvasPosition = detailFrame.CanvasPosition + Vector2.new(0, 2)
    elseif scrollingActive and detailFrame.CanvasPosition.Y >= detailFrame.AbsoluteCanvasSize.Y - detailFrame.AbsoluteWindowSize.Y then
        scrollingActive = false
        scrollBtn.Text = "AUTO ROLL: OFF"
        scrollStroke.Color = Color3.fromRGB(70, 70, 70)
    end
end)

-- SCANNER DE TOOLS
local function scan(tool)
    if not tool or not tool:IsA("Tool") then return end
    f.Visible = true
    t.Text = "📡 SCAN: " .. tool.Name:upper()
    for _, v in pairs(listFrame:GetChildren()) do if v:IsA("TextButton") then v:Destroy() end end
    
    for _, obj in pairs(tool:GetDescendants()) do
        if obj:IsA("LocalScript") or obj:IsA("ModuleScript") or obj:IsA("Script") or obj:IsA("RemoteEvent") or obj:IsA("RemoteFunction") then
            local isRemote = obj:IsA("RemoteEvent") or obj:IsA("RemoteFunction")
            local b = Instance.new("TextButton")
            b.Size = UDim2.new(1, -10, 0, 32)
            b.BackgroundColor3 = isRemote and Color3.fromRGB(50, 20, 20) or Color3.fromRGB(40, 40, 40)
            b.Text = "  [" .. obj.ClassName:sub(1,3):upper() .. "] " .. obj.Name
            b.TextColor3 = Color3.new(1,1,1)
            b.Font = Enum.Font.GothamSemibold
            b.TextSize = 11
            b.TextXAlignment = Enum.TextXAlignment.Left
            b.Parent = listFrame
            Instance.new("UICorner", b).CornerRadius = UDim.new(0, 5)
            
            b.MouseButton1Click:Connect(function()
                detailFrame.CanvasPosition = Vector2.new(0,0)
                local content = "-- OBJ: " .. obj.Name .. "\n-- CLASS: " .. obj.ClassName .. "\n-- PATH: " .. obj:GetFullName() .. "\n"
                local success, src = pcall(function() return obj.Source end)
                if success and src and src ~= "" then
                    content = content .. "------------------\n" .. src
                else
                    local dSuccess, dResult = pcall(function() return decompile(obj) end)
                    content = content .. "------------------\n" .. (dSuccess and dResult or "-- Fonte Protegida")
                end
                detailText.Text = content
            end)
        end
    end
end

local function watch(char)
    char.ChildAdded:Connect(function(c) if c:IsA("Tool") then scan(c) end end)
    char.ChildRemoved:Connect(function(c) if c:IsA("Tool") then f.Visible = false end end)
    local st = char:FindFirstChildOfClass("Tool")
    if st then scan(st) end
end

if p.Character then watch(p.Character) end
p.CharacterAdded:Connect(watch)
