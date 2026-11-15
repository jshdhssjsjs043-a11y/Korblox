local Players = game:GetService("Players")

local player = Players.LocalPlayer

local korbloxBielConnections = {}

local function applyKorbloxBiel()
    local char = player.Character or player.CharacterAdded:Wait()
    local hum = char:WaitForChild("Humanoid", 5)

    if not hum or char:FindFirstChild("KorbloxFolder") then
        return
    end

    local folder = Instance.new("Folder")
    folder.Name = "KorbloxFolder"
    folder.Parent = char

    -- R15
    if hum.RigType == Enum.HumanoidRigType.R15 then

        local ru = char:WaitForChild("RightUpperLeg", 3)
        local rl = char:WaitForChild("RightLowerLeg", 3)
        local rf = char:WaitForChild("RightFoot", 3)

        if not (ru and rl and rf) then
            return
        end

        ru.Transparency = 1
        rl.Transparency = 1
        rf.Transparency = 1

        local korbloxPart = Instance.new("Part")
        korbloxPart.Name = "KorbloxR15Visual"
        korbloxPart.Size = Vector3.new(1, 2, 1)
        korbloxPart.CanCollide = false
        korbloxPart.Massless = true
        korbloxPart.Parent = folder

        local mesh = Instance.new("SpecialMesh")
        mesh.MeshId = "rbxassetid://902942096"
        mesh.TextureId = "rbxassetid://902843398"
        mesh.Scale = Vector3.new(1, 1, 1)
        mesh.Parent = korbloxPart

        local weld = Instance.new("Weld")
        weld.Part0 = ru
        weld.Part1 = korbloxPart
        weld.C0 = CFrame.new(0.1, 0.2, 0)
        weld.Parent = korbloxPart

    -- R6
    elseif hum.RigType == Enum.HumanoidRigType.R6 then

        local rightLeg = char:WaitForChild("Right Leg", 3)

        if not rightLeg then
            return
        end

        for _, v in ipairs(char:GetChildren()) do
            if v:IsA("CharacterMesh") and v.BodyPart == Enum.BodyPart.RightLeg then
                v:Destroy()
            end
        end

        rightLeg.Transparency = 1

        local korbloxLeg = Instance.new("Part")
        korbloxLeg.Name = "KorbloxR6Visual"
        korbloxLeg.Size = Vector3.new(1, 2, 1)
        korbloxLeg.CanCollide = false
        korbloxLeg.Massless = true
        korbloxLeg.Parent = folder

        local mesh = Instance.new("SpecialMesh")
        mesh.MeshType = Enum.MeshType.FileMesh
        mesh.MeshId = "rbxassetid://101851696"
        mesh.TextureId = "rbxassetid://101851254"
        mesh.Scale = Vector3.new(1, 1, 1)
        mesh.Parent = korbloxLeg

        local weld = Instance.new("Weld")
        weld.Part0 = rightLeg
        weld.Part1 = korbloxLeg
        weld.C0 = CFrame.new(0, 0, 0)
        weld.Parent = korbloxLeg
    end
end

local function korbloxBielOn()

    if player.Character then
        task.spawn(applyKorbloxBiel)
    end

    table.insert(korbloxBielConnections, player.CharacterAdded:Connect(function()
        task.wait(0.5)
        applyKorbloxBiel()
    end))

    print("✅ KORBLOX BIEL ON")
end

local function korbloxBielOff()

    for _, conn in ipairs(korbloxBielConnections) do
        pcall(function()
            conn:Disconnect()
        end)
    end

    korbloxBielConnections = {}

    local char = player.Character

    if char then

        local folder = char:FindFirstChild("KorbloxFolder")

        if folder then
            folder:Destroy()
        end

        local hum = char:FindFirstChild("Humanoid")

        if hum then

            if hum.RigType == Enum.HumanoidRigType.R15 then

                for _, name in ipairs({
                    "RightUpperLeg",
                    "RightLowerLeg",
                    "RightFoot"
                }) do

                    local part = char:FindFirstChild(name)

                    if part then
                        part.Transparency = 0
                    end
                end

            elseif hum.RigType == Enum.HumanoidRigType.R6 then

                local rl = char:FindFirstChild("Right Leg")

                if rl then
                    rl.Transparency = 0
                end
            end
        end
    end

    print("❌ KORBLOX BIEL OFF")
end

-- Ativar
korbloxBielOn()
