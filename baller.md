--[[
Baller script
made by DDanii#0247  formerly... DaniVB

hats:
https://www.roblox.com/catalog/6685365462/Red-Stickman-Head
https://www.roblox.com/catalog/3499972183/International-Fedora-Colombia
https://www.roblox.com/catalog/3438342658/International-Fedora-Argentina
https://www.roblox.com/catalog/3409612660/International-Fedora-USA
https://www.roblox.com/catalog/3992084515/International-Fedora-Vietnam
https://www.roblox.com/catalog/4047554959/International-Fedora-Brazil


thanks to REIM for the idea.
]]



_G.ThrowStrength = 200     --Throw strength


rejoinkeybind     = "-"            --Quick rejoin
resetballkeybind  = "r"            --Resetting the ball  for if you threw it in the void
sprintkey         = "LeftControl"  --Sprinting








--reanimate by MyWorld#4430 discord.gg/pYVHtSJmEY
game:GetService("Players").LocalPlayer.Character.InternationalFedora.Name = "exp_1"
game:GetService("Players").LocalPlayer.Character.InternationalFedora.Name = "exp_2"
game:GetService("Players").LocalPlayer.Character.InternationalFedora.Name = "exp_3"
game:GetService("Players").LocalPlayer.Character.InternationalFedora.Name = "exp_4"
game:GetService("Players").LocalPlayer.Character["International Fedora"].Name = "exp_5"
if game:GetService("Players").LocalPlayer.Character.Humanoid.RigType == Enum.HumanoidRigType.R6 then
game:GetService("Players").LocalPlayer.Character['exp_1'].Handle.SpecialMesh:destroy()
game:GetService("Players").LocalPlayer.Character['exp_2'].Handle.SpecialMesh:destroy()
game:GetService("Players").LocalPlayer.Character['exp_3'].Handle.SpecialMesh:destroy()
game:GetService("Players").LocalPlayer.Character['exp_4'].Handle.SpecialMesh:destroy()
game:GetService("Players").LocalPlayer.Character['exp_5'].Handle.SpecialMesh:destroy()

end

local Vector3_101 = Vector3.new(1, 0, 1)
local netless_Y = Vector3.new(0, 25.1, 0)
local function getNetlessVelocity(realPartVelocity)
    local mag = realPartVelocity.Magnitude
    if (mag > 1) and (mag < 100 xss=removed> 0.25) or (unit.Y < -0.75) then
            return realPartVelocity * (25.1 / realPartVelocity.Y)
        end
        realPartVelocity = unit * 125
    end
    return (realPartVelocity * Vector3_101) + netless_Y
end
local simradius = "shp" --simulation radius (net bypass) method
--"shp" - sethiddenproperty
--"ssr" - setsimulationradius
--false - disable
local healthHide = true --moves your head under map every 3 seconds so players dont see your health bar
local noclipAllParts = false --set it to true if you want noclip
local antiragdoll = true --removes hingeConstraints and ballSocketConstraints from your character
local newanimate = true --disables the animate script and enables after reanimation
local discharscripts = true --disables all localScripts parented to your character before reanimation
local R15toR6 = true --tries to convert your character to r6 if its r15
local hatcollide = false --makes hats cancollide (credit to ShownApe) (works only with reanimate method 0)
local humState16 = true --enables collisions for limbs before the humanoid dies (using hum:ChangeState)
local addtools = false --puts all tools from backpack to character and lets you hold them after reanimation
local hedafterneck = true --disable aligns for head and enable after neck or torso is removed
local loadtime = game:GetService("Players").RespawnTime + 0.5 --anti respawn delay
local method = 3 --reanimation method
--methods:
--0 - breakJoints (takes [loadtime] seconds to laod)
--1 - limbs
--2 - limbs + anti respawn
--3 - limbs + breakJoints after [loadtime] seconds
--4 - remove humanoid + breakJoints
--5 - remove humanoid + limbs
local alignmode = 4 --AlignPosition mode
--modes:
--1 - AlignPosition rigidity enabled true
--2 - 2 AlignPositions rigidity enabled both true and false
--3 - AlignPosition rigidity enabled false
--4 - CFrame (if u dont have the isnetworkowner function it will use alignmode 2)
local flingpart = "HumanoidRootPart" --name of the part or the hat used for flinging
--the fling function
--usage: fling(target, duration, velocity)
--target can be set to: basePart, CFrame, Vector3, character model or humanoid (flings at mouse.Hit if argument not provided))
--duration (fling time in seconds) can be set to: a number or a string convertable to the number (0.5s if not provided),
--velocity (fling part rotation velocity) can be set to a vector3 value (Vector3.new(20000, 20000, 20000) if not provided)

local lp = game:GetService("Players").LocalPlayer
local rs = game:GetService("RunService")
local stepped = rs.Stepped
local heartbeat = rs.Heartbeat
local renderstepped = rs.RenderStepped
local sg = game:GetService("StarterGui")
local ws = game:GetService("Workspace")
local cf = CFrame.new
local v3 = Vector3.new
local v3_0 = v3(0, 0, 0)
local inf = math.huge

local c = lp.Character

if not (c and c.Parent) then
    return
end

c:GetPropertyChangedSignal("Parent"):Connect(function()
    if not (c and c.Parent) then
        c = nil
    end
end)

local function gp(parent, name, className)
    if typeof(parent) == "Instance" then
        for i, v in pairs(parent:GetChildren()) do
            if (v.Name == name) and v:IsA(className) then
                return v
            end
        end
    end
    return nil
end

if type(getNetlessVelocity) ~= "function" then
    getNetlessVelocity = nil
end

local fenv = getfenv()
local shp = fenv.sethiddenproperty or fenv.set_hidden_property or fenv.set_hidden_prop or fenv.sethiddenprop
local ssr = fenv.setsimulationradius or fenv.set_simulation_radius or fenv.set_sim_radius or fenv.setsimradius or fenv.set_simulation_rad or fenv.setsimulationrad
local ino = fenv.isnetworkowner or fenv.is_network_owner or fenv.isnetowner or fenv.is_net_owner

if (alignmode == 4) and (not ino) then
    alignmode = 2
end

local physp = PhysicalProperties.new(0.01, 0, 1, 0, 0)
local function align(Part0, Part1)
    
    local att0 = Instance.new("Attachment")
    att0.Orientation = v3_0
    att0.Position = v3_0
    att0.Name = "att0_" .. Part0.Name
    local att1 = Instance.new("Attachment")
    att1.Orientation = v3_0
    att1.Position = v3_0
    att1.Name = "att1_" .. Part1.Name
    
    if alignmode == 4 then
    
        local con = nil
        local rot, angles = math.rad(0.05), CFrame.Angles
        con1 = heartbeat:Connect(function()
            if Part0 and Part1 and att1 then
                if ino(Part0) then
                    Part0.CFrame = Part1.CFrame * att1.CFrame * angles(0, 0, rot)
                    rot = -rot
                end
            else
                con:Disconnect()
            end
        end)
    
    else
        
        Part0.CustomPhysicalProperties = physp
        if (alignmode == 1) or (alignmode == 2) then
            local ape = Instance.new("AlignPosition", att0)
            ape.ApplyAtCenterOfMass = false
            ape.MaxForce = inf
            ape.MaxVelocity = inf
            ape.ReactionForceEnabled = false
            ape.Responsiveness = 200
            ape.Attachment1 = att1
            ape.Attachment0 = att0
            ape.Name = "AlignPositionRtrue"
            ape.RigidityEnabled = true
        end
        
        if (alignmode == 2) or (alignmode == 3) then
            local apd = Instance.new("AlignPosition", att0)
            apd.ApplyAtCenterOfMass = false
            apd.MaxForce = inf
            apd.MaxVelocity = inf
            apd.ReactionForceEnabled = false
            apd.Responsiveness = 200
            apd.Attachment1 = att1
            apd.Attachment0 = att0
            apd.Name = "AlignPositionRfalse"
            apd.RigidityEnabled = false
        end
        
        local ao = Instance.new("AlignOrientation", att0)
        ao.MaxAngularVelocity = inf
        ao.MaxTorque = inf
        ao.PrimaryAxisOnly = false
        ao.ReactionTorqueEnabled = false
        ao.Responsiveness = 200
        ao.Attachment1 = att1
        ao.Attachment0 = att0
        ao.RigidityEnabled = false
    
    end

    if getNetlessVelocity then
        local vel = Part0.Velocity
        local con0, con1 = nil, nil
        if alignmode == 4 then
            con0 = stepped:Connect(function(_, delta)
                if not (Part0 and Part1) then return con0:Disconnect() and con1:Disconnect() end
                Part0.RotVelocity = Part1.RotVelocity
            end)
            con1 = heartbeat:Connect(function()
                if not (Part0 and Part1) then return con0:Disconnect() and con1:Disconnect() end
                Part0.Velocity = getNetlessVelocity(Part1.Velocity)
            end)
        else
            con0 = renderstepped:Connect(function()
                if not (Part0 and Part1) then return con0:Disconnect() and con1:Disconnect() end
                Part0.Velocity = vel
            end)
            con1 = heartbeat:Connect(function()
                if not (Part0 and Part1) then return con0:Disconnect() and con1:Disconnect() end
                vel = Part0.Velocity
                Part0.Velocity = getNetlessVelocity(Part1.Velocity)
            end)
        end
    end
    
    att0:GetPropertyChangedSignal("Parent"):Connect(function()
        Part0 = att0.Parent
        if not Part0:IsA("BasePart") then
            att0 = nil
            Part0 = nil
        end
    end)
    att0.Parent = Part0
    
    att1:GetPropertyChangedSignal("Parent"):Connect(function()
        Part1 = att1.Parent
        if not Part1:IsA("BasePart") then
            att1 = nil
            Part1 = nil
        end
    end)
    att1.Parent = Part1
end

local function respawnrequest()
    local ccfr = ws.CurrentCamera.CFrame
    local c = lp.Character
    lp.Character = nil
    lp.Character = c
    local con = nil
    con = ws.CurrentCamera.Changed:Connect(function(prop)
        if (prop ~= "Parent") and (prop ~= "CFrame") then
            return
        end
        ws.CurrentCamera.CFrame = ccfr
        con:Disconnect()
    end)
end

local destroyhum = (method == 4) or (method == 5)
local breakjoints = (method == 0) or (method == 4)
local antirespawn = (method == 0) or (method == 2) or (method == 3)

hatcollide = hatcollide and (method == 0)

addtools = addtools and gp(lp, "Backpack", "Backpack")

if shp and (simradius == "shp") then
    spawn(function()
        while c and heartbeat:Wait() do
            shp(lp, "SimulationRadius", inf)
        end
    end)
elseif ssr and (simradius == "ssr") then
    spawn(function()
        while c and heartbeat:Wait() do
            ssr(inf)
        end
    end)
end

antiragdoll = antiragdoll and function(v)
    if v:IsA("HingeConstraint") or v:IsA("BallSocketConstraint") then
        v.Parent = nil
    end
end

if antiragdoll then
    for i, v in pairs(c:GetDescendants()) do
        antiragdoll(v)
    end
    c.DescendantAdded:Connect(antiragdoll)
end

if antirespawn then
    respawnrequest()
end

if method == 0 then
    wait(loadtime)
    if not c then
        return
    end
end

if discharscripts then
    for i, v in pairs(c:GetChildren()) do
        if v:IsA("LocalScript") then
            v.Disabled = true
        end
    end
elseif newanimate then
    local animate = gp(c, "Animate", "LocalScript")
    if animate and (not animate.Disabled) then
        animate.Disabled = true
    else
        newanimate = false
    end
end

if addtools then
    for i, v in pairs(addtools:GetChildren()) do
        if v:IsA("Tool") then
            v.Parent = c
        end
    end
end

pcall(function()
    settings().Physics.AllowSleep = false
    settings().Physics.PhysicsEnvironmentalThrottle = Enum.EnviromentalPhysicsThrottle.Disabled
end)

local OLDscripts = {}

for i, v in pairs(c:GetDescendants()) do
    if v.ClassName == "Script" then
        table.insert(OLDscripts, v)
    end
end

local scriptNames = {}

for i, v in pairs(c:GetDescendants()) do
    if v:IsA("BasePart") then
        local newName = tostring(i)
        local exists = true
        while exists do
            exists = false
            for i, v in pairs(OLDscripts) do
                if v.Name == newName then
                    exists = true
                end
            end
            if exists then
                newName = newName .. "_"    
            end
        end
        table.insert(scriptNames, newName)
        Instance.new("Script", v).Name = newName
    end
end

c.Archivable = true
local hum = c:FindFirstChildOfClass("Humanoid")
if hum then
    for i, v in pairs(hum:GetPlayingAnimationTracks()) do
        v:Stop()
    end
end
local cl = c:Clone()
if hum and humState16 then
    hum:ChangeState(Enum.HumanoidStateType.Physics)
    if destroyhum then
        wait(1.6)
    end
end
if hum and hum.Parent and destroyhum then
    hum:Destroy()
end

if not c then
    return
end

local head = gp(c, "Head", "BasePart")
local torso = gp(c, "Torso", "BasePart") or gp(c, "UpperTorso", "BasePart")
local root = gp(c, "HumanoidRootPart", "BasePart")
if hatcollide and c:FindFirstChildOfClass("Accessory") then
    local anything = c:FindFirstChildOfClass("BodyColors") or gp(c, "Health", "Script")
    if not (torso and root and anything) then
        return
    end
    torso:Destroy()
    root:Destroy()
    anything:Destroy()
end

local model = Instance.new("Model", c)
model:GetPropertyChangedSignal("Parent"):Connect(function()
    if not (model and model.Parent) then
        model = nil
    end
end)

for i, v in pairs(c:GetChildren()) do
    if v ~= model then
        if addtools and v:IsA("Tool") then
            for i1, v1 in pairs(v:GetDescendants()) do
                if v1 and v1.Parent and v1:IsA("BasePart") then
                    local bv = Instance.new("BodyVelocity", v1)
                    bv.Velocity = v3_0
                    bv.MaxForce = v3(1000, 1000, 1000)
                    bv.P = 1250
                    bv.Name = "bv_" .. v.Name
                end
            end
        end
        v.Parent = model
    end
end

if breakjoints then
    model:BreakJoints()
else
    if head and torso then
        for i, v in pairs(model:GetDescendants()) do
            if v:IsA("JointInstance") then
                local save = false
                if (v.Part0 == torso) and (v.Part1 == head) then
                    save = true
                end
                if (v.Part0 == head) and (v.Part1 == torso) then
                    save = true
                end
                if save then
                    if hedafterneck then
                        hedafterneck = v
                    end
                else
                    v:Destroy()
                end
            end
        end
    end
    if method == 3 then
        task.delay(loadtime, pcall, model.BreakJoints, model)
    end
end

for i, v in pairs(cl:GetChildren()) do
    v.Parent = c
end
cl:Destroy()

local uncollide, noclipcon = nil, nil
if noclipAllParts then
    uncollide = function()
        if c then
            for i, v in pairs(c:GetDescendants()) do
                if v:IsA("BasePart") then
                    v.CanCollide = false
                end
            end
        else
            noclipcon:Disconnect()
        end
    end
else
    uncollide = function()
        if model then
            for i, v in pairs(model:GetDescendants()) do
                if v:IsA("BasePart") then
                    v.CanCollide = false
                end
            end
        else
            noclipcon:Disconnect()
        end
    end
end
noclipcon = stepped:Connect(uncollide)
uncollide()

for i, scr in pairs(model:GetDescendants()) do
    if (scr.ClassName == "Script") and table.find(scriptNames, scr.Name) then
        local Part0 = scr.Parent
        if Part0:IsA("BasePart") then
            for i1, scr1 in pairs(c:GetDescendants()) do
                if (scr1.ClassName == "Script") and (scr1.Name == scr.Name) and (not scr1:IsDescendantOf(model)) then
                    local Part1 = scr1.Parent
                    if (Part1.ClassName == Part0.ClassName) and (Part1.Name == Part0.Name) then
                        align(Part0, Part1)
                        scr:Destroy()
                        scr1:Destroy()
                        break
                    end
                end
            end
        end
    end
end

for i, v in pairs(c:GetDescendants()) do
    if v and v.Parent and (not v:IsDescendantOf(model)) then
        if v:IsA("Decal") then
            v.Transparency = 1
        elseif v:IsA("BasePart") then
            v.Transparency = 1
            v.Anchored = false
        elseif v:IsA("ForceField") then
            v.Visible = false
        elseif v:IsA("Sound") then
            v.Playing = false
        elseif v:IsA("BillboardGui") or v:IsA("SurfaceGui") or v:IsA("ParticleEmitter") or v:IsA("Fire") or v:IsA("Smoke") or v:IsA("Sparkles") then
            v.Enabled = false
        end
    end
end

if newanimate then
    local animate = gp(c, "Animate", "LocalScript")
    if animate then
        animate.Disabled = false
    end
end

if addtools then
    for i, v in pairs(c:GetChildren()) do
        if v:IsA("Tool") then
            v.Parent = addtools
        end
    end
end

local hum0, hum1 = model:FindFirstChildOfClass("Humanoid"), c:FindFirstChildOfClass("Humanoid")
if hum0 then
    hum0:GetPropertyChangedSignal("Parent"):Connect(function()
        if not (hum0 and hum0.Parent) then
            hum0 = nil
        end
    end)
end
if hum1 then
    hum1:GetPropertyChangedSignal("Parent"):Connect(function()
        if not (hum1 and hum1.Parent) then
            hum1 = nil
        end
    end)

    ws.CurrentCamera.CameraSubject = hum1
    local camSubCon = nil
    local function camSubFunc()
        camSubCon:Disconnect()
        if c and hum1 then
            ws.CurrentCamera.CameraSubject = hum1
        end
    end
    camSubCon = renderstepped:Connect(camSubFunc)
    if hum0 then
        hum0:GetPropertyChangedSignal("Jump"):Connect(function()
            if hum1 then
                hum1.Jump = hum0.Jump
            end
        end)
    else
        respawnrequest()
    end
end

local rb = Instance.new("BindableEvent", c)
rb.Event:Connect(function()
    rb:Destroy()
    sg:SetCore("ResetButtonCallback", true)
    if destroyhum then
        if c then c:BreakJoints() end
        return
    end
    if model and hum0 and (hum0.Health > 0) then
        model:BreakJoints()
        hum0.Health = 0
    end
    if antirespawn then
        respawnrequest()
    end
end)
sg:SetCore("ResetButtonCallback", rb)

spawn(function()
    while wait() and c do
        if hum0 and hum1 then
            hum1.Jump = hum0.Jump
        end
    end
    sg:SetCore("ResetButtonCallback", true)
end)

R15toR6 = R15toR6 and hum1 and (hum1.RigType == Enum.HumanoidRigType.R15)
if R15toR6 then
    local part = gp(c, "HumanoidRootPart", "BasePart") or gp(c, "UpperTorso", "BasePart") or gp(c, "LowerTorso", "BasePart") or gp(c, "Head", "BasePart") or c:FindFirstChildWhichIsA("BasePart")
    if part then
        local cfr = part.CFrame
        local R6parts = { 
            head = {
                Name = "Head",
                Size = v3(2, 1, 1),
                R15 = {
                    Head = 0
                }
            },
            torso = {
                Name = "Torso",
                Size = v3(2, 2, 1),
                R15 = {
                    UpperTorso = 0.2,
                    LowerTorso = -0.8
                }
            },
            root = {
                Name = "HumanoidRootPart",
                Size = v3(2, 2, 1),
                R15 = {
                    HumanoidRootPart = 0
                }
            },
            leftArm = {
                Name = "Left Arm",
                Size = v3(1, 2, 1),
                R15 = {
                    LeftHand = -0.849,
                    LeftLowerArm = -0.174,
                    LeftUpperArm = 0.415
                }
            },
            rightArm = {
                Name = "Right Arm",
                Size = v3(1, 2, 1),
                R15 = {
                    RightHand = -0.849,
                    RightLowerArm = -0.174,
                    RightUpperArm = 0.415
                }
            },
            leftLeg = {
                Name = "Left Leg",
                Size = v3(1, 2, 1),
                R15 = {
                    LeftFoot = -0.85,
                    LeftLowerLeg = -0.29,
                    LeftUpperLeg = 0.49
                }
            },
            rightLeg = {
                Name = "Right Leg",
                Size = v3(1, 2, 1),
                R15 = {
                    RightFoot = -0.85,
                    RightLowerLeg = -0.29,
                    RightUpperLeg = 0.49
                }
            }
        }
        for i, v in pairs(c:GetChildren()) do
            if v:IsA("BasePart") then
                for i1, v1 in pairs(v:GetChildren()) do
                    if v1:IsA("Motor6D") then
                        v1.Part0 = nil
                    end
                end
            end
        end
        part.Archivable = true
        for i, v in pairs(R6parts) do
            local part = part:Clone()
            part:ClearAllChildren()
            part.Name = v.Name
            part.Size = v.Size
            part.CFrame = cfr
            part.Anchored = false
            part.Transparency = 1
            part.CanCollide = false
            for i1, v1 in pairs(v.R15) do
                local R15part = gp(c, i1, "BasePart")
                local att = gp(R15part, "att1_" .. i1, "Attachment")
                if R15part then
                    local weld = Instance.new("Weld", R15part)
                    weld.Name = "Weld_" .. i1
                    weld.Part0 = part
                    weld.Part1 = R15part
                    weld.C0 = cf(0, v1, 0)
                    weld.C1 = cf(0, 0, 0)
                    R15part.Massless = true
                    R15part.Name = "R15_" .. i1
                    R15part.Parent = part
                    if att then
                        att.Parent = part
                        att.Position = v3(0, v1, 0)
                    end
                end
            end
            part.Parent = c
            R6parts[i] = part
        end
        local R6joints = {
            neck = {
                Parent = R6parts.torso,
                Name = "Neck",
                Part0 = R6parts.torso,
                Part1 = R6parts.head,
                C0 = cf(0, 1, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0),
                C1 = cf(0, -0.5, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0)
            },
            rootJoint = {
                Parent = R6parts.root,
                Name = "RootJoint" ,
                Part0 = R6parts.root,
                Part1 = R6parts.torso,
                C0 = cf(0, 0, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0),
                C1 = cf(0, 0, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0)
            },
            rightShoulder = {
                Parent = R6parts.torso,
                Name = "Right Shoulder",
                Part0 = R6parts.torso,
                Part1 = R6parts.rightArm,
                C0 = cf(1, 0.5, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0),
                C1 = cf(-0.5, 0.5, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0)
            },
            leftShoulder = {
                Parent = R6parts.torso,
                Name = "Left Shoulder",
                Part0 = R6parts.torso,
                Part1 = R6parts.leftArm,
                C0 = cf(-1, 0.5, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0),
                C1 = cf(0.5, 0.5, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0)
            },
            rightHip = {
                Parent = R6parts.torso,
                Name = "Right Hip",
                Part0 = R6parts.torso,
                Part1 = R6parts.rightLeg,
                C0 = cf(1, -1, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0),
                C1 = cf(0.5, 1, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0)
            },
            leftHip = {
                Parent = R6parts.torso,
                Name = "Left Hip" ,
                Part0 = R6parts.torso,
                Part1 = R6parts.leftLeg,
                C0 = cf(-1, -1, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0),
                C1 = cf(-0.5, 1, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0)
            }
        }
        for i, v in pairs(R6joints) do
            local joint = Instance.new("Motor6D")
            for prop, val in pairs(v) do
                joint[prop] = val
            end
            R6joints[i] = joint
        end
        if hum1 then
            hum1.RigType = Enum.HumanoidRigType.R6
            hum1.HipHeight = 0
        end
    end
end

local torso1 = torso
torso = gp(c, "Torso", "BasePart") or ((not R15toR6) and gp(c, torso.Name, "BasePart"))
if (typeof(hedafterneck) == "Instance") and head and torso and torso1 then
    local conNeck = nil
    local conTorso = nil
    local contorso1 = nil
    local aligns = {}
    local function enableAligns()
        conNeck:Disconnect()
        conTorso:Disconnect()
        conTorso1:Disconnect()
        for i, v in pairs(aligns) do
            v.Enabled = true
        end
    end
    conNeck = hedafterneck.Changed:Connect(function(prop)
        if table.find({"Part0", "Part1", "Parent"}, prop) then
            enableAligns()
        end
    end)
    conTorso = torso:GetPropertyChangedSignal("Parent"):Connect(enableAligns)
    conTorso1 = torso1:GetPropertyChangedSignal("Parent"):Connect(enableAligns)
    for i, v in pairs(head:GetDescendants()) do
        if v:IsA("AlignPosition") or v:IsA("AlignOrientation") then
            i = tostring(i)
            aligns[i] = v
            v:GetPropertyChangedSignal("Parent"):Connect(function()
                aligns[i] = nil
            end)
            v.Enabled = false
        end
    end
end

local flingpart0 = gp(model, flingpart, "BasePart") or gp(gp(model, flingpart, "Accessory"), "Handle", "BasePart")
local flingpart1 = gp(c, flingpart, "BasePart") or gp(gp(c, flingpart, "Accessory"), "Handle", "BasePart")

local fling = function() end
if flingpart0 and flingpart1 then
    flingpart0:GetPropertyChangedSignal("Parent"):Connect(function()
        if not (flingpart0 and flingpart0.Parent) then
            flingpart0 = nil
            fling = function() end
        end
    end)
    flingpart0.Archivable = true
    flingpart1:GetPropertyChangedSignal("Parent"):Connect(function()
        if not (flingpart1 and flingpart1.Parent) then
            flingpart1 = nil
            fling = function() end
        end
    end)
    local att0 = gp(flingpart0, "att0_" .. flingpart0.Name, "Attachment")
    local att1 = gp(flingpart1, "att1_" .. flingpart1.Name, "Attachment")
    if att0 and att1 then
        att0:GetPropertyChangedSignal("Parent"):Connect(function()
            if not (att0 and att0.Parent) then
                att0 = nil
                fling = function() end
            end
        end)
        att1:GetPropertyChangedSignal("Parent"):Connect(function()
            if not (att1 and att1.Parent) then
                att1 = nil
                fling = function() end
            end
        end)
        local lastfling = nil
        local mouse = lp:GetMouse()
        fling = function(target, duration, rotVelocity)
            if typeof(target) == "Instance" then
                if target:IsA("BasePart") then
                    target = target.Position
                elseif target:IsA("Model") then
                    target = gp(target, "HumanoidRootPart", "BasePart") or gp(target, "Torso", "BasePart") or gp(target, "UpperTorso", "BasePart") or target:FindFirstChildWhichIsA("BasePart")
                    if target then
                        target = target.Position
                    else
                        return
                    end
                elseif target:IsA("Humanoid") then
                    target = target.Parent
                    if not (target and target:IsA("Model")) then
                        return
                    end
                    target = gp(target, "HumanoidRootPart", "BasePart") or gp(target, "Torso", "BasePart") or gp(target, "UpperTorso", "BasePart") or target:FindFirstChildWhichIsA("BasePart")
                    if target then
                        target = target.Position
                    else
                        return
                    end
                else
                    return
                end
            elseif typeof(target) == "CFrame" then
                target = target.Position
            elseif typeof(target) ~= "Vector3" then
                target = mouse.Hit
                if target then
                    target = target.Position
                else
                    return
                end
            end
            if target.Y < ws xss=removed xss=removed xss=removed xss=removed xss=removed xss=removed xss=removed flingpart.Name = "flingpart_" xss=removed xss=removed xss=removed xss=removed xss=removed xss=removed> 0.5 then
                flingpart0.Transparency = 0.5
            end
            att1.Parent = flingpart
            local con = nil
            local rotchg = v3(0, rotVelocity.Unit.Y * -1000, 0)
            con = heartbeat:Connect(function(delta)
                if target and (lastfling == target) and flingpart and flingpart0 and flingpart1 and att0 and att1 then
                    flingpart.Orientation += rotchg * delta
                    flingpart0.RotVelocity = rotVelocity
                else
                    con:Disconnect()
                end
            end)
            if alignmode ~= 4 then
                local con = nil
                con = renderstepped:Connect(function()
                    if flingpart0 and target then
                        flingpart0.RotVelocity = v3_0
                    else
                        con:Disconnect()
                    end
                end)
            end
            wait(duration)
            if lastfling ~= target then
                if flingpart then
                    if att1 and (att1.Parent == flingpart) then
                        att1.Parent = flingpart1
                    end
                    flingpart:Destroy()
                end
                return
            end
            target = nil
            if not (flingpart and flingpart0 and flingpart1 and att0 and att1) then
                return
            end
            flingpart0.RotVelocity = v3_0
            att1.Parent = flingpart1
            if flingpart then
                flingpart:Destroy()
            end
        end
    end
end

--start script--




tweenservice = game:GetService("TweenService")
p = game.Players.LocalPlayer
ch = p.Character
hum = ch:FindFirstChildOfClass("Humanoid")
root = ch.HumanoidRootPart
Root = ch.HumanoidRootPart
sine = 0
change = 1
head = ch.Head
leftarm = ch['Left Arm']
rightarm = ch['Right Arm']
leftleg = ch['Left Leg']
rightleg = ch['Right Leg']
tor = ch['Torso']
bwait = false
attack = false

-- Welds

neck = Instance.new("Weld",ch.Torso)
neck.Part0 = ch.Torso
neck.Part1 = ch.Head
neck.C0 = CFrame.new(0,1,0)
neck.C1 = CFrame.new(0,-0.5,0)

torso = Instance.new("Weld",root)
torso.Part0 = root
torso.Part1 = ch.Torso


rs = Instance.new("Weld",ch.Torso)
rs.Part0 = ch.Torso
rs.Part1 = ch["Right Arm"]
rs.C0 = CFrame.new(1.5,0.5,0)
rs.C1 = CFrame.new(0,0.5,0)

ls = Instance.new("Weld",ch.Torso)
ls.Part0 = ch.Torso
ls.Part1 = ch["Left Arm"]
ls.C0 = CFrame.new(-1.5,0.5,0)
ls.C1 = CFrame.new(0,0.5,0)

rh = Instance.new("Weld",ch.Torso)
rh.Part0 = ch.Torso
rh.Part1 = ch["Right Leg"]
rh.C0 = CFrame.new(0.5,-1,0)
rh.C1 = CFrame.new(0,1,0)

lh = Instance.new("Weld",ch.Torso)
lh.Part0 = ch.Torso
lh.Part1 = ch["Left Leg"]
lh.C0 = CFrame.new(-0.5,-1,0)
lh.C1 = CFrame.new(0,1,0)


local plr = game.Players.LocalPlayer
local chr = plr.Character
local redball = chr["RedStickman"].Handle

redball:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = redball
Weld.Part0 = game:GetService("Players").LocalPlayer.Character["Right Arm"]
Weld.C0 = CFrame.new(0,-1,0)*CFrame.Angles(math.rad(0),math.rad(0),math.rad(0))

local exphat = chr["exp_1"].Handle
exphat:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = exphat
Weld.Part0 = game:GetService("Players").LocalPlayer.Character.Torso
Weld.C0 = CFrame.new(0,0,0.02)*CFrame.Angles(math.rad(90),math.rad(0),math.rad(0))

local exphat = chr["exp_2"].Handle
exphat:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = exphat
Weld.Part0 = game:GetService("Players").LocalPlayer.Character.Torso
Weld.C0 = CFrame.new(0,0,0.02)*CFrame.Angles(math.rad(90),math.rad(0),math.rad(0))

local exphat = chr["exp_3"].Handle
exphat:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = exphat
Weld.Part0 = game:GetService("Players").LocalPlayer.Character.Torso
Weld.C0 = CFrame.new(0,0,0.02)*CFrame.Angles(math.rad(90),math.rad(0),math.rad(0))

local exphat = chr["exp_4"].Handle
exphat:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = exphat
Weld.Part0 = game:GetService("Players").LocalPlayer.Character.Torso
Weld.C0 = CFrame.new(0,0,0.02)*CFrame.Angles(math.rad(90),math.rad(0),math.rad(0))

local exphat = chr["exp_5"].Handle
exphat:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = exphat
Weld.Part0 = game:GetService("Players").LocalPlayer.Character.Torso
Weld.C0 = CFrame.new(0,0,0.02)*CFrame.Angles(math.rad(90),math.rad(0),math.rad(0))





function tweenobject(object,stuff,edirection,estyle,speed,waitthing)
 local speedthing = 1
 local tween = tweenservice:Create(object,TweenInfo.new(speed/speedthing,estyle,edirection,0,false,0),stuff)
 tween:Play()
 if waitthing == true then
  tween.Completed:Wait()
  tween:Destroy()
 end
end








ch.Humanoid.WalkSpeed = 16
holdingshift = false





local UIS = game:GetService("UserInputService")

UIS.InputBegan:Connect(function(Input, GameProcessedEvent)
if Input.KeyCode == Enum.KeyCode[sprintkey] then
if anim ~= 'idle' then
holdingshift = true
ch.Humanoid.WalkSpeed = 60
end
end
end)


UIS.InputEnded:Connect(function(Input, GameProcessedEvent)
 if Input.KeyCode == Enum.KeyCode[sprintkey] then
  game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = NormalSpeed
holdingshift = false
ch.Humanoid.WalkSpeed = 16
 end
end)











mouse = game.Players.LocalPlayer:GetMouse()
mouse.KeyDown:connect(function(key)
if key == rejoinkeybind then
local ts = game:GetService("TeleportService")
local p = game:GetService("Players").LocalPlayer
ts:Teleport(game.PlaceId, p)
elseif key == resetballkeybind then
if throwcheck == true then
redball:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = redball
Weld.Part0 = game:GetService("Players").LocalPlayer.Character["Right Arm"]
Weld.C0 = CFrame.new(0,-1,0)*CFrame.Angles(math.rad(0),math.rad(0),math.rad(0))

local exphat = chr["exp_1"].Handle
exphat:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = exphat
Weld.Part0 = game:GetService("Players").LocalPlayer.Character.Torso
Weld.C0 = CFrame.new(0,0,0.02)*CFrame.Angles(math.rad(90),math.rad(0),math.rad(0))

local exphat = chr["exp_2"].Handle
exphat:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = exphat
Weld.Part0 = game:GetService("Players").LocalPlayer.Character.Torso
Weld.C0 = CFrame.new(0,0,0.02)*CFrame.Angles(math.rad(90),math.rad(0),math.rad(0))

local exphat = chr["exp_3"].Handle
exphat:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = exphat
Weld.Part0 = game:GetService("Players").LocalPlayer.Character.Torso
Weld.C0 = CFrame.new(0,0,0.02)*CFrame.Angles(math.rad(90),math.rad(0),math.rad(0))

local exphat = chr["exp_4"].Handle
exphat:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = exphat
Weld.Part0 = game:GetService("Players").LocalPlayer.Character.Torso
Weld.C0 = CFrame.new(0,0,0.02)*CFrame.Angles(math.rad(90),math.rad(0),math.rad(0))

local exphat = chr["exp_5"].Handle
exphat:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = exphat
Weld.Part0 = game:GetService("Players").LocalPlayer.Character.Torso
Weld.C0 = CFrame.new(0,0,0.02)*CFrame.Angles(math.rad(90),math.rad(0),math.rad(0))



game:GetService("Players").LocalPlayer.Character:FindFirstChild('BALL'):destroy()
if game:GetService("Players").LocalPlayer.Character:FindFirstChild('explosion1') then
game:GetService("Players").LocalPlayer.Character:FindFirstChild('explosion1'):destroy()
game:GetService("Players").LocalPlayer.Character:FindFirstChild('explosion2'):destroy()
game:GetService("Players").LocalPlayer.Character:FindFirstChild('explosion3'):destroy()
game:GetService("Players").LocalPlayer.Character:FindFirstChild('explosion4'):destroy()
game:GetService("Players").LocalPlayer.Character:FindFirstChild('explosion5'):destroy()
end
throwcheck = false
STOPPP = false
wait(1)
attack = false
end
end


end)




mouse.Button1Down:connect(function()
if bwait == false and attack == false then
attack = true
holding = true
bwait = true
throwcheck = true
for i = 0,2,0.1 do
if anim == "idle" then
tweenobject(neck,{C0 = CFrame.new(0,1,0)*CFrame.Angles(math.rad(-3),math.rad(50),math.rad(7))},Enum.EasingDirection.Out,Enum.EasingStyle.Quart,0.5,false)
else 
tweenobject(neck,{C0 = CFrame.new(0,1,0)*CFrame.Angles(math.rad(-3),math.rad(0),math.rad(7))},Enum.EasingDirection.Out,Enum.EasingStyle.Quart,0.5,false)
end
tweenobject(rs,{C0 = CFrame.new(1.5,0.8,0)*CFrame.Angles(math.rad(185),math.rad(0),math.rad(40))},Enum.EasingDirection.Out,Enum.EasingStyle.Quart,0.5,false)
tweenobject(ls,{C0 = CFrame.new(-1.3,0.5,-0.2)*CFrame.Angles(math.rad(0),math.rad(-45),math.rad(-55))},Enum.EasingDirection.Out,Enum.EasingStyle.Quart,0.5,false)
end


wait(0.5)
for i = 0,2,0.1 do
tweenobject(torso,{C0 = CFrame.new(0,0,0)*CFrame.Angles(math.rad(0),math.rad(30),math.rad(0))},Enum.EasingDirection.Out,Enum.EasingStyle.Quart,0.3,false)
tweenobject(neck,{C0 = CFrame.new(0,1,0)*CFrame.Angles(math.rad(-3),math.rad(-30),math.rad(7))},Enum.EasingDirection.Out,Enum.EasingStyle.Quart,0.3,false)
tweenobject(rs,{C0 = CFrame.new(1.5,0.5,-0.5)*CFrame.Angles(math.rad(80),math.rad(0),math.rad(20))},Enum.EasingDirection.Out,Enum.EasingStyle.Quart,0.3,false)
tweenobject(ls,{C0 = CFrame.new(-1.3,0.5,-0.2)*CFrame.Angles(math.rad(0),math.rad(-25),math.rad(-55))},Enum.EasingDirection.Out,Enum.EasingStyle.Quart,0.3,false)
end
throwball()
wait(0.5)
bwait = false
end
end)

mouse.Button1Up:connect(function()
holding = false
end)









function throwball()
e = 0
expcount2 = 0
expcount = 0
STOPPP = false
local player = game:GetService("Players").LocalPlayer
local character = player.Character
local mouse = player:GetMouse()
local ballpart = Instance.new("Part")
ballpart.Size = Vector3.new(1,1,1)
ballpart.CFrame = character['Right Arm'].CFrame * CFrame.new(0,-3,0)
ballpart.Parent = game.Players.LocalPlayer.Character
ballpart.Name = 'BALL'
ballpart.Shape = "Ball"
ballpart.Transparency = 1

local direction = mouse.Hit.LookVector
local forceMultipliter = _G.ThrowStrength * ballpart:GetMass()
ballpart:ApplyImpulse(direction * forceMultipliter)


redball:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = redball
Weld.Part0 = ballpart
Weld.C0 = CFrame.new(0,0,0)*CFrame.Angles(math.rad(0),math.rad(0),math.rad(0))

--wait(0.05)
ballpart.Touched:connect(function(hit)
if hit:IsA("Part") and hit.Name ~= "Handle" and hit.Name ~= ch.HumanoidRootPart.Name then
if STOPPP == false then
throwing = true
STOPPP = true
print(hit)
explode()
e = e + 1
end
end
end)








end







function explode()
for i = 1,5 do
expcount = expcount + 1
local explosionpart = Instance.new("Part")
explosionpart.Size = Vector3.new(1,1,1)
explosionpart.CFrame = game:GetService("Players").LocalPlayer.Character:FindFirstChild('BALL').CFrame * CFrame.new(0,0,0)
explosionpart.Parent = game.Players.LocalPlayer.Character
explosionpart.Name = 'explosion' .. expcount
explosionpart.Shape = "Block"
explosionpart.Transparency = 1
local direction = Vector3.new(math.rad(math.random(-30,30)),math.rad(math.random(0,180)),math.rad(math.random(-30,30)))
local forceMultipliter = math.random(40,60) * explosionpart:GetMass()
explosionpart:ApplyImpulse(direction * forceMultipliter)
local exphat = chr["exp_"..expcount].Handle
exphat:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = exphat
Weld.Part0 = game:GetService("Players").LocalPlayer.Character['explosion'..expcount]
Weld.C0 = CFrame.new(0,0,0.02)*CFrame.Angles(math.rad(0),math.rad(0),math.rad(0))

end



wait(1.5)
redball:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = redball
Weld.Part0 = game:GetService("Players").LocalPlayer.Character["Right Arm"]
Weld.C0 = CFrame.new(0,-1,0)*CFrame.Angles(math.rad(0),math.rad(0),math.rad(0))

throwcheck = false
throwing = false
game:GetService("Players").LocalPlayer.Character:FindFirstChild('BALL'):destroy()

exploding = true
repeat
expcount2 = expcount2 + 1
for i,v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
if v.Name == "explosion" ..expcount2 then
v.CanCollide = false
wait(0.2)
local exphat = chr["exp_".. expcount2].Handle
exphat:BreakJoints()
local Weld = Instance.new("Weld", game.Players.LocalPlayer.Character)
Weld.Part1 = exphat
Weld.Part0 = game:GetService("Players").LocalPlayer.Character.Torso
Weld.C0 = CFrame.new(0,0,0.02)*CFrame.Angles(math.rad(90),math.rad(0),math.rad(0))

v:destroy()
end
end
until expcount2 > 4
repeat wait() until not ch:FindFirstChild('explosion1')
wait(1)
STOPPP = false
exploding = false
attack = false

end








--[[
--storage

]]

local Brick = game:GetService("Players").LocalPlayer.Character.Model.HumanoidRootPart
local speed = 1
Brick.Material = "Glass"
Brick.Transparency = 0.7
heartbeat:connect(function()
for i = 0,1,0.001*speed do
Brick.Color = Color3.fromHSV(i,1,1) --creates a color using i
task.wait()
end
end)

local numbers = {-4,-3,-2,2,3,4}
heartbeat:connect(function()
rockps = ch.RedStickman.Handle.Position
if throwcheck == true then

fling(Vector3.new(rockps.X+numbers[math.random(#numbers)],rockps.Y+numbers[math.random(#numbers)],rockps.Z+numbers[math.random(#numbers)]),task.wait())
end
end)



while true do 
game:GetService("RunService").Heartbeat:Wait()
sine = sine + change

local rlegray = Ray.new(ch["Right Leg"].Position + Vector3.new(0, 0.5, 0), Vector3.new(0, -2, 0))
local rlegpart, rlegendPoint = workspace:FindPartOnRay(rlegray, char)
local llegray = Ray.new(ch["Left Leg"].Position + Vector3.new(0, 0.5, 0), Vector3.new(0, -2, 0))
local llegpart, llegendPoint = workspace:FindPartOnRay(llegray, char)
local rightvector = (Root.Velocity * Root.CFrame.rightVector).X + (Root.Velocity * Root.CFrame.rightVector).Z
local lookvector = (Root.Velocity * Root.CFrame.lookVector).X + (Root.Velocity * Root.CFrame.lookVector).Z
if lookvector > ch.Humanoid.WalkSpeed then
lookvector = ch.Humanoid.WalkSpeed
end
if lookvector < -ch.Humanoid.WalkSpeed then
lookvector = -ch.Humanoid.WalkSpeed
end
if rightvector > ch.Humanoid.WalkSpeed then
rightvector = ch.Humanoid.WalkSpeed
end
if rightvector < -ch.Humanoid.WalkSpeed then
rightvector = -ch.Humanoid.WalkSpeed
end
local lookvel = lookvector / ch.Humanoid.WalkSpeed
local rightvel = rightvector / ch.Humanoid.WalkSpeed





 if hum.MoveDirection.Magnitude > 0 then

  anim = "walk"

 else 
  anim = "idle"
 end
 if hum:GetState() == Enum.HumanoidStateType.Freefall then
  anim = "jump"
 end



if anim == "jump"  then
    if bwait == false then
  tweenobject(torso,{C0 = CFrame.new(0,0,0)*CFrame.Angles(math.rad(8),math.rad(0),math.rad(0))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)

  tweenobject(neck,{C0 = CFrame.new(0,1,0)*CFrame.Angles(math.rad(8),math.rad(0),math.rad(0))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)
        tweenobject(rs,{C0 = CFrame.new(1.5,0.8,0)*CFrame.Angles(math.rad(155+10*math.cos(sine/60)),math.rad(0),math.rad(10))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)
  tweenobject(ls,{C0 = CFrame.new(-1.5,0.5,0)*CFrame.Angles(math.rad(0),math.rad(0),math.rad(-15))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)
end
  tweenobject(rh,{C0 = CFrame.new(0.5,0,-0.5)*CFrame.Angles(math.rad(-8),math.rad(0),math.rad(0))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)
  tweenobject(lh,{C0 = CFrame.new(-0.5,-0.75,-0.15)*CFrame.Angles(math.rad(0),math.rad(0),math.rad(0))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)
end
if root.Velocity.Y < -1 then
    if bwait == false then
  tweenobject(torso,{C0 = CFrame.new(0,0,0)*CFrame.Angles(math.rad(-8),math.rad(0),math.rad(0))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)

  tweenobject(neck,{C0 = CFrame.new(0,1,0)*CFrame.Angles(math.rad(-8),math.rad(0),math.rad(0))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)
        tweenobject(rs,{C0 = CFrame.new(1.5,0.8,0)*CFrame.Angles(math.rad(190+10*math.cos(sine/60)),math.rad(0),math.rad(10))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)
  tweenobject(ls,{C0 = CFrame.new(-1.5,0.5,0)*CFrame.Angles(math.rad(0),math.rad(0),math.rad(-65))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)
end
  tweenobject(rh,{C0 = CFrame.new(0.5,-0.5,-0.25)*CFrame.Angles(math.rad(-8),math.rad(0),math.rad(0))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)
  tweenobject(lh,{C0 = CFrame.new(-0.5,-0.75,-0.15)*CFrame.Angles(math.rad(0),math.rad(0),math.rad(0))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)
end

if anim == "idle" then 
for i = 0,2,0.1 do
    if bwait == false then
tweenobject(torso,{C0 = CFrame.new(0,0+0.1*math.cos(sine/56),0)*CFrame.Angles(math.rad(0),math.rad(-55),math.rad(0))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)

tweenobject(neck,{C0 = CFrame.new(0,1,0)*CFrame.Angles(math.rad(-7+3*math.cos(sine/70)),math.rad(50),math.rad(7-3*math.cos(sine/70)))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)
tweenobject(rs,{C0 = CFrame.new(1.5,0.8,0)*CFrame.Angles(math.rad(185+10*math.cos(sine/60)),math.rad(0),math.rad(10))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)
tweenobject(ls,{C0 = CFrame.new(-1.3,0.5,-0.2)*CFrame.Angles(math.rad(0),math.rad(-25),math.rad(-85-5*math.cos(sine/70)))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)
end
tweenobject(rh,{C0 = CFrame.new(0.5,-1-0.1*math.cos(sine/56),0)*CFrame.Angles(math.rad(0),math.rad(0),math.rad(5))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)
tweenobject(lh,{C0 = CFrame.new(-0.5,-1-0.1*math.cos(sine/56),0)*CFrame.Angles(math.rad(-5+5*math.cos(sine/70)),math.rad(0),math.rad(-15))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)
end
end



if anim == "walk" then
if holdingshift == false then
for i = 0,2,0.1 do
    if bwait == false then
tweenobject(torso,{C0 = CFrame.new(0,0+0.1*math.cos(sine/4),0)*CFrame.Angles(math.rad(0-lookvel*5),math.rad(0),math.rad(0-rightvel*3+tor.RotVelocity.Y*2))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)

tweenobject(neck,{C0 = CFrame.new(0,1,0)*CFrame.Angles(math.rad(0-lookvel*3),math.rad(0-rightvel*10+5*math.cos(sine/120)+tor.RotVelocity.Y*3),math.rad(0+rightvel*4))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)
tweenobject(rs,{C0 = CFrame.new(1.5,0.8,0)*CFrame.Angles(math.rad(185+10*math.cos(sine/60)),math.rad(0),math.rad(10))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)
tweenobject(ls,{C0 = CFrame.new(-1.5,0.5+0.1*math.cos(sine/8),0)*CFrame.Angles(math.rad(0+40*math.sin(sine/8)*lookvel),math.rad(0),math.rad(-4+7*math.cos(sine/8)*rightvel))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)
end
tweenobject(rh,{C0 = CFrame.new(0.5,-1+0.5*math.cos(sine/8),0+0*math.cos(sine/8)*lookvel)*CFrame.Angles(math.rad(0+50*math.sin(sine/8)*lookvel),math.rad(0),math.rad(5+50*math.sin(sine/8))*rightvel-tor.RotVelocity.Y/20)},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)
tweenobject(lh,{C0 = CFrame.new(-0.5,-1+-0.5*math.cos(sine/8),0+0*math.cos(sine/8)*lookvel)*CFrame.Angles(math.rad(0-50*math.sin(sine/8)*lookvel),math.rad(0),math.rad(-5+-50*math.sin(sine/8))*rightvel-tor.RotVelocity.Y/20)},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)
end
end
end
if anim == "walk" then
if holdingshift == true then
for i = 0,2,0.1 do
    if bwait == false then
tweenobject(torso,{C0 = CFrame.new(0,0+0.1*math.cos(sine/2),0)*CFrame.Angles(math.rad(0-lookvel*20),math.rad(0),math.rad(0-rightvel*3+tor.RotVelocity.Y*2))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)

tweenobject(neck,{C0 = CFrame.new(0,1,0)*CFrame.Angles(math.rad(10+5*math.cos(sine/2)-lookvel*3),math.rad(0-rightvel*40+5*math.cos(sine/120)+tor.RotVelocity.Y*3),math.rad(0+rightvel*4))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)
tweenobject(rs,{C0 = CFrame.new(1.5,0.8,0)*CFrame.Angles(math.rad(185+10*math.cos(sine/60)),math.rad(0),math.rad(10))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,1,false)
tweenobject(ls,{C0 = CFrame.new(-1.5,0.5+0.1*math.cos(sine/4),0)*CFrame.Angles(math.rad(0+60*math.sin(sine/4)*lookvel),math.rad(0),math.rad(-4+7*math.cos(sine/4)*rightvel))},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)
end
tweenobject(rh,{C0 = CFrame.new(0.5,-1+0.5*math.cos(sine/4),0+0*math.cos(sine/4)*lookvel)*CFrame.Angles(math.rad(0+100*math.sin(sine/4)*lookvel),math.rad(0),math.rad(5+60*math.sin(sine/4))*rightvel-tor.RotVelocity.Y/20)},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)
tweenobject(lh,{C0 = CFrame.new(-0.5,-1+-0.5*math.cos(sine/4),0+0*math.cos(sine/4)*lookvel)*CFrame.Angles(math.rad(0-100*math.sin(sine/4)*lookvel),math.rad(0),math.rad(-5+-60*math.sin(sine/4))*rightvel-tor.RotVelocity.Y/20)},Enum.EasingDirection.InOut,Enum.EasingStyle.Linear,0.1,false)
end
end
end






end




