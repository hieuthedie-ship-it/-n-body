un body by hieudz
-- SCRIPT ẨN BỘ PHẬN NHÂN VẬT CHO DELTA MOBILE
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local CoreGui = game:GetService("CoreGui")

local isHidden = false

-- ==========================================
-- TẠO NÚT NỔI TRÊN MÀN HÌNH ĐIỆN THOẠI
-- ==========================================
local ScreenGui = Instance.new("ScreenGui")
local ToggleButton = Instance.new("TextButton")
local UICorner = Instance.new("UICorner")

ScreenGui.Name = "DeltaBodyLimbsGui"
ScreenGui.Parent = CoreGui
ScreenGui.ResetOnSpawn = false

ToggleButton.Name = "LimbsButton"
ToggleButton.Parent = ScreenGui
ToggleButton.Size = UDim2.new(0, 120, 0, 45)
ToggleButton.Position = UDim2.new(0.05, 0, 0.3, 0) -- Nằm dưới nút Teleport cũ một chút
ToggleButton.BackgroundColor3 = Color3.fromRGB(139, 0, 0) -- Màu đỏ tối
ToggleButton.Text = "Ẩn Bộ Phận: OFF"
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.TextSize = 14
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.Active = true
ToggleButton.Draggable = true -- Có thể kéo di chuyển trên màn hình

UICorner.CornerRadius = UDim.new(0, 8)
UICorner.Parent = ToggleButton

-- ==========================================
-- HÀM XỬ LÝ ẨN/HIỆN BỘ PHẬN (Limb Hider)
-- ==========================================
local function toggleBodyParts()
    local character = LocalPlayer.Character
    if not character then return end

    -- Danh sách các bộ phận cần ẩn (Thân, tay, chân cho cả R6 và R15)
    local limbs = {
        "Left Arm", "Right Arm", "Left Leg", "Right Leg", "Torso", -- R6
        "LeftUpperArm", "LeftLowerArm", "LeftHand", 
        "RightUpperArm", "RightLowerArm", "RightHand",
        "LeftUpperLeg", "LeftLowerLeg", "LeftFoot", 
        "RightUpperLeg", "RightLowerLeg", "RightFoot",
        "UpperTorso", "LowerTorso" -- R15
    }

    for _, partName in ipairs(limbs) do
        local part = character:FindFirstChild(partName)
        if part and part:IsA("BasePart") then
            if isHidden then
                part.Transparency = 1 -- Làm tàng hình hoàn toàn
                
                -- Ẩn các hình dán (vết sẹo, áo quần) trên bộ phận đó
                for _, child in ipairs(part:GetChildren()) do
                    if child:IsA("Decal") or child:IsA("Texture") then
                        child.Transparency = 1
                    end
                end
            else
                part.Transparency = 0 -- Hiện lại bình thường
                for _, child in ipairs(part:GetChildren()) do
                    if child:IsA("Decal") or child:IsA("Texture") then
                        child.Transparency = 0
                    end
                end
            end
        end
    end
end

-- Sự kiện bấm nút
ToggleButton.MouseButton1Click:Connect(function()
    isHidden = not isHidden
    if isHidden then
        ToggleButton.BackgroundColor3 = Color3.fromRGB(0, 100, 80) -- Màu xanh
        ToggleButton.Text = "Ẩn Bộ Phận: ON"
    else
        ToggleButton.BackgroundColor3 = Color3.fromRGB(139, 0, 0) -- Màu đỏ
        ToggleButton.Text = "Ẩn Bộ Phận: OFF"
    end
    toggleBodyParts()
end)

-- Tự động ẩn lại khi nhân vật hồi sinh (Reset)
LocalPlayer.CharacterAdded:Connect(function(char)
    if isHidden then
        task.wait(1) -- Chờ nhân vật load xong
        toggleBodyParts()
    end
end)

print("[Delta Mobile]: Script ẩn thân, tay, chân đã kích hoạt!")
