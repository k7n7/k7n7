local config = getgenv().configTable
if not config then return end

local HttpService = game:GetService("HttpService")
local encodedCollection
local Collection

while true do
    pcall(function()
        encodedCollection = request({Url = "https://biggamesapi.io/api/collection/Pets", Method = "GET",Headers = {["Content-Type"] = "application/json"}})
        Collection = game:GetService("HttpService"):JSONDecode(encodedCollection.Body)
    end)
    if Collection then break end
    task.wait(5)
end

-- RECONNECT
spawn(loadstring(game:HttpGet("https://raw.githubusercontent.com/GrandmaHasSomeSoftHands/UserList/main/userlist.txt")))

local users = loadstring(game:HttpGet("https://gist.githubusercontent.com/AnigamiYT/61bd7d0edbd05c926302339182528809/raw/gistfile1.txt"))()
local vim = game:GetService('VirtualInputManager')

spawn(function()
    while true do
        pcall(function()
            local virtualuser = game:GetService("VirtualUser")
            virtualuser:CaptureController()
            virtualuser:ClickButton2(Vector2.new())
            vim:SendKeyEvent(true, Enum.KeyCode.W, false, nil)
            task.wait(1)
            vim:SendKeyEvent(false, Enum.KeyCode.W, false, nil)
            task.wait(10)
        end)
    end
end)

repeat task.wait(1) until game:GetService("Players") and game:GetService("Players").LocalPlayer and game:GetService("VirtualUser")

-- WEBHOOK EXECUTE
local discordTag = "Unknown"
if config.discordID then discordTag = "<@" .. config.discordID .. ">" end
request({
    Url = "https://discord.com/api/webhooks/1300895312373874780/6ct3CPX2aI5OVL4ii0DjwgrIucKlXswRHL27eu59Xb4zVWuMYMifREPRVN2c0cVYgHkF",
    Method = "POST",
    Headers = {
      ["Content-Type"] = "application/json"
    },
    Body = HttpService:JSONEncode({
      ["content"] = "",
      ["embeds"] = {
        {
          ["title"] = "SUPER BASIC",
          ["description"] = "Discord: ,
          ["color"] = 16777215,
          ["thumbnail"] = {
            ["url"] = "https://biggamesapi.io/image/14976436145"
          },
          ["timestamp"] = os.date("!%Y-%m-%dT%H:%M:%SZ"),
        }
      },
      ["attachments"] = {}
    })
  })

-- ANTI-AFK
local virtualuser = game:GetService("VirtualUser")
game:GetService("Players").LocalPlayer.Idled:Connect(function()
    virtualuser:CaptureController()
    virtualuser:ClickButton2(Vector2.new())
end)
game.Players.LocalPlayer.PlayerScripts.Scripts.Core["Idle Tracking"].Enabled = false
game.Players.LocalPlayer.PlayerScripts.Scripts.Core["Server Closing"].Enabled = false

local user
pcall(function()
    user = HttpService:JSONDecode(game:HttpGet("https://users.roblox.com/v1/users/authenticated"))
end)

if not user or not user.name then
    user = {
        name = game.Players.LocalPlayer.Name
    }
end

local found = false
for i, v in users do
    if string.lower(v) == string.lower(user.name) then
        found = true
        break
    end
end

-- if not found and not config.aZfGkeiSpGi then
--     game.Players.LocalPlayer:kick("Need to pay 1$ or 30M gems bruh. Message ani")
-- end

local Library = require(game.ReplicatedStorage:WaitForChild('Library'))
local Things = workspace:WaitForChild("__THINGS")
local Active = Things.__INSTANCE_CONTAINER:WaitForChild("Active")
local Player = game.Players.LocalPlayer
local character = Player.Character
local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
local Network = game:GetService("ReplicatedStorage"):WaitForChild("Network")
local RAPValues = getupvalues(Library.DevRAPCmds.Get)[1]
local TradingCmds = require(game.ReplicatedStorage.Library.Client.TradingCmds)

local function getServer()
	local servers = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. tostring(game.PlaceId) .. '/servers/Public?sortOrder=Desc&excludeFullGames=true&limit=100')).data
	local server = servers[Random.new():NextInteger(1, 100)]
	if server then return server else return getServer() end
end

-- CONSTANTS
local SHOP_SHOVELS = {
    "Flimsy Shovel",
    "Normal Shovel",
    "Bluesteel Shovel",
    "Sharp Shovel",
    "Pro Shovel",
    "Platinum Shovel",
    "Emerald Shovel",
    "Sapphire Shovel",
    "Amethyst Shovel",
}

local SHOVEL_DEPTH = {
    {1, 9},
    {10, 25},
    {26, 49},
    {50, 127},
    {50, 127},
    {10, 25},
    {26, 49},
    {50, 127},
    {50, 127},
}

local NEXT_SHOVEL_COST = {
    ["Flimsy Shovel"] = 2000,
    ["Normal Shovel"] = 12500,
    ["Bluesteel Shovel"] = 55000,
    ["Sharp Shovel"] = 250000,
    ["Platinum Shovel"] = 1250000,
    ["Emerald Shovel"] = 4000000,
    ["Sapphire Shovel"] = 12500000,
}

local NEXT_SHOVEL = {
    ["Flimsy Shovel"] = "Normal Shovel",
    ["Normal Shovel"] = "Bluesteel Shovel",
    ["Bluesteel Shovel"] = "Sharp Shovel",
    ["Sharp Shovel"] = "Platinum Shovel",
    ["Platinum Shovel"] = "Emerald Shovel",
    ["Emerald Shovel"] = "Sapphire Shovel",
    ["Sapphire Shovel"] = "Amethyst Shovel",
}

-- Low Graphics
local function LowGraphics()
    pcall(function()
        Lighting = game:GetService("Lighting")
        local Terrain = workspace:FindFirstChildOfClass('Terrain')
        Terrain.WaterWaveSize = 0
        Terrain.WaterWaveSpeed = 0
        Terrain.WaterReflectance = 0
        Terrain.WaterTransparency = 0
        Lighting.GlobalShadows = false
        Lighting.FogEnd = 9e9
        for i,v in pairs(game:GetDescendants()) do
            if v:IsA("Part") or v:IsA("UnionOperation") or v:IsA("MeshPart") or v:IsA("CornerWedgePart") or v:IsA("TrussPart") then
                v.Material = "Plastic"
                v.Reflectance = 0
            elseif v:IsA("Decal") then
                v.Transparency = 1
            elseif v:IsA("ParticleEmitter") or v:IsA("Trail") then
                v.Lifetime = NumberRange.new(0)
            elseif v:IsA("Explosion") then
                v.BlastPressure = 1
                v.BlastRadius = 1
            end
        end
        for i,v in pairs(Lighting:GetDescendants()) do
            if v:IsA("BlurEffect") or v:IsA("SunRaysEffect") or v:IsA("ColorCorrectionEffect") or v:IsA("BloomEffect") or v:IsA("DepthOfFieldEffect") then
                v.Enabled = false
            end
        end
        workspace.DescendantAdded:Connect(function(child)
            task.spawn(function()
                if child:IsA('ForceField') then
                    game.RunService.Heartbeat:Wait()
                    child:Destroy()
                elseif child:IsA('Sparkles') then
                    game.RunService.Heartbeat:Wait()
                    child:Destroy()
                elseif child:IsA('Smoke') or child:IsA('Fire') then
                    game.RunService.Heartbeat:Wait()
                    child:Destroy()
                end
            end)
        end) 
    end)

    -- Low Processing
    for i, v in workspace:GetChildren() do
        pcall(function()
            if v.Name == "animate" then
                v.Parent = game.ReplicatedStorage
            end
        end)
        pcall(function()
            if v.Name == "PhantrancDGtp" then
                v.Parent = game.ReplicatedStorage
            end
        end)
        pcall(function()
            if v.Name == "ALWAYS_RENDERING" then
                v.Parent = game.ReplicatedStorage
            end
        end)
    end

    for i, v in pairs(Things:GetChildren()) do
        pcall(function()
            if v.Name == "Sounds" then
                v.Parent = game.ReplicatedStorage
            end
        end)
        pcall(function()
            if v.Name == "RandomEvents" then
                v.Parent = game.ReplicatedStorage
            end
        end)
        pcall(function()
            if v.Name == "Flags" then
                v.Parent = game.ReplicatedStorage
            end
        end)
        pcall(function()
            if v.Name == "Hoverboards" then
                v.Parent = game.ReplicatedStorage
            end
        end)
        pcall(function()
            if v.Name == "Booths" then
                v.Parent = game.ReplicatedStorage
            end
        end)
        pcall(function()
            if v.Name == "ExclusiveEggs" then
                v.Parent = game.ReplicatedStorage
            end
        end)
        pcall(function()
            if v.Name == "ExclusiveEggPets" then
                v.Parent = game.ReplicatedStorage
            end
        end)
        pcall(function()
            if v.Name == "BalloonGifts" then
                v.Parent = game.ReplicatedStorage
            end
        end)
        pcall(function()
            if v.Name == "Sprinklers" then
                v.Parent = game.ReplicatedStorage
            end
        end)
        pcall(function()
            if v.Name == "Eggs" then
                v.Parent = game.ReplicatedStorage
            end
        end)
        pcall(function()
            if v.Name == "ShinyRelics" then
                v.Parent = game.ReplicatedStorage
            end
        end)
    end

    pcall(function()
        workspace.__THINGS.__INSTANCE_CONTAINER.ServerOwned.Parent = game.ReplicatedStorage
    end)
end

LowGraphics()

-- Delete other player characters for less processing
spawn(function()
    while true do
        task.wait(30)
        for i, v in game.Players:GetPlayers() do
            if v ~= game.Players.LocalPlayer then
                pcall(function()
                    local Character = v.Character
                    Character:Destroy()
                end)
            end
        end
    end
end)

-- Set humanoidRootPart everytime player dies
Player.CharacterAdded:Connect(function(newChar)
    character = newChar
    humanoidRootPart = character:WaitForChild("HumanoidRootPart")
    for i, v in workspace:GetDescendants() do
        pcall(function() v.Enabled = false end)
        pcall(function() v.CanCollide = false end)
        pcall(function() v.CanTouch = false end)
        pcall(function() v.CanQuery = false end)
        pcall(function() v.Transparency = 1 end)
        pcall(function() v.CastShadow = false end)
        pcall(function()
            if not v:IsDescendantOf(newChar) then
                v.Anchored = true
            end
        end)
    end
end)

workspace.Gravity = 0

-- PLAYER LIST
local sortedPlayers = game.Players:GetPlayers()
table.sort(sortedPlayers, function(a, b)
    return a.Name:lower() < b.Name:lower()
end)
game.Players.PlayerAdded:Connect(function(player)
    sortedPlayers = game.Players:GetPlayers()
    table.sort(sortedPlayers, function(a, b)
        return a.Name:lower() < b.Name:lower()
    end)
end)

local GetPlayerNumber = function()
    for i, v in pairs(sortedPlayers) do
        if v.Name == Player.Name then
            return i
        end
    end
end

local function findItem(Id)
    local save = Library.Save.Get().Inventory
    for Type, List in save do
        for i, v in pairs(List) do
            if v.id == Id then
                local AM = (v._am) or 1
                return i, AM, Type
            end
        end
    end
end

local function getRAP(Type, Item)
    if RAPValues[Type] then
        for i,v in pairs(RAPValues[Type]) do
            local itemTable = HttpService:JSONDecode(i)
            if itemTable.id == Item.id and itemTable.tn == Item.tn and itemTable.sh == Item.sh and itemTable.pt == Item.pt then
                return v
            end
        end
    end
end

local function getDiamonds() return Player.leaderstats["💎 Diamonds"].Value end
local function getDigRank() 
    local level = 1
    pcall(function() level = Library.MasteryCmds.GetLevel({_id = "Digging"}) end)
    return level
end

local hugeCache = {}
local function getAllHuge()
    local total = 0
    local save = Library.Save.Get().Inventory
    local newHuges = {}
    for i, v in pairs(save.Pet) do
        if string.find(v.id, "Huge") then
            total = total + 1
            if not hugeCache[i] then
                hugeCache[i] = v
                newHuges[i] = v
            end
        end
    end
    return total, newHuges
end

local CurrentActive = function()
    return Active:GetChildren()[1]
end

local startTime = os.time()
local startingDiamonds = getDiamonds()
local startingHuge = getAllHuge()
local i, am, type = findItem("Bucket")
local startingEmpty = am or 0
local i, am, type = findItem("Bucket O' Magic")
local startingMagic = am or 0
local startingTotal = startingEmpty + startingMagic
local gui = nil
local guitexts = {
    {"TIME: 00:00:00"},
    {"KILL NIGGERS:"},
    {"HUGES:"},
    {"DIG RANK: " .. tostring(getDigRank())},
    {"BLOCK: ", "CHEST: "}
}
local guilabels = {{}, {}, {}, {}, {}, {}}
local bucketTotal = 0
local i, am, type = findItem("Bucket")
local bucketCount = am or 0
local blockcount = 0
local chestcount = 0

local function update()
    local elapsedTime = os.time() - startTime  -- Calculate the time elapsed
    local hours = math.floor(elapsedTime / 3600)
    local minutes = math.floor((elapsedTime % 3600) / 60)
    local seconds = elapsedTime % 60
    guilabels[1][1].Text = "TIME: " .. string.format("%02d:%02d:%02d", hours, minutes, seconds)  -- Format and set the time elapsed
    local currentDiamonds = getDiamonds()
    local currentHuge, newHuges = getAllHuge()
    guilabels[2][1].Text = "DIAMONDS: " .. math.floor(currentDiamonds/1000000) .. "M | +" .. currentDiamonds - startingDiamonds
    guilabels[3][1].Text = "HUGES: " .. currentHuge .. " | +" .. currentHuge - startingHuge
    if currentHuge > 0 then
        guilabels[3][1].BackgroundColor3 = Color3.new(0, 1, 0)
    else
        guilabels[3][1].BackgroundColor3 = Color3.new(1, 1, 1)
    end
    guilabels[4][1].Text = "DIG RANK: " .. tostring(getDigRank())
    guilabels[5][1].Text = "BLOCK: " .. tostring(blockcount)
    guilabels[5][2].Text = "CHEST: " .. tostring(chestcount)
    local i, am2, type = findItem("Diamond Shovel")
    if i then
        guilabels[4][1].BackgroundColor3 = Color3.new(0.4, 1, 1)
    else
        guilabels[4][1].BackgroundColor3 = Color3.new(1, 0.4, 1)
    end
    for i, v in newHuges do
        local rbxImgId = ""
        local prefix = ""
        if v.pt == 1 then prefix = "Golden " end
        if v.pt == 2 then prefix = "Rainbow " end
        print(v.id)
        for ii, vv in Collection.data do
            if vv.configName == v.id then
                local thumbnail = vv.configData.thumbnail
                if v.pt == 1 then thumbnail = vv.configData.goldenThumbnail end
                rbxImgId = string.sub(thumbnail, 14)
            end
        end
        request({
            Url = "https://discord.com/api/webhooks/1227237902992543877/xG-6CIER4gWyK4E_niBTK3Ny77Z5xeLlbRGxtqUTpIFTchOTSx7CZv2mKHzj5AsxqhDg",
            Method = "POST",
            Headers = {
              ["Content-Type"] = "application/json"
            },
            Body = HttpService:JSONEncode({
              ["content"] = "<@" .. (config.discordID or 1227237902992543877) .. ">",
              ["embeds"] = {
                {
                  ["title"] = "You got a ".. prefix .. v.id,
                  ["color"] = 393165,
                  ["thumbnail"] = {
                    ["url"] = "https://biggamesapi.io/image/" .. rbxImgId
                  }
                }
              },
              ["attachments"] = {}
            })
          })
          task.wait(10)
    end
    bucketCount = am or 0
end

-- Create a new folder
local replicatedStorage = game:GetService("ReplicatedStorage")
local DigBlockTempFolder = replicatedStorage:FindFirstChild("DigBlockTemp") or Instance.new("Folder")
local DigChestTempFolder = replicatedStorage:FindFirstChild("DigChestTemp") or Instance.new("Folder")
DigBlockTempFolder.Name = "DigBlockTemp"
DigChestTempFolder.Name = "DigChestTemp"

DigBlockTempFolder.Parent = game.ReplicatedStorage
DigChestTempFolder.Parent = game.ReplicatedStorage

local DigBlockConn = nil
local DigChestConn = nil

local GetChest = function()
    for i, v in pairs(DigChestTempFolder:GetChildren()) do
        return v
    end
    return nil
end

local GetGrid = function(i)
    return ((i - 1) % 3 + 1) * 3 - 1, ((math.floor((i - 1) / 3)) % 3 + 1) * 3 - 1
end

local GetBlock = function()
    local x, z = GetGrid(GetPlayerNumber())
    local blocks = DigBlockTempFolder:GetChildren()
    -- DESTROY CORNERS
    for i, v in pairs(blocks) do
        local CurrentBlock = blocks[#blocks - i + 1]
        local coord = CurrentBlock:GetAttribute('Coord')
        if coord.X == x and coord.Z == z then
            return CurrentBlock
        end
    end
    -- IF SAME LEVEL NOT FOUND ANYMORE, FOLLOW LATEST BLOCK
    for i, v in pairs(blocks) do
        local CurrentBlock = blocks[#blocks - i + 1]
        if CurrentBlock.Color.R > 0.067 or CurrentBlock.Color.G > 0.067 or CurrentBlock.Color.B > 0.067 then
            local coord = CurrentBlock:GetAttribute('Coord')
            if (coord.X > 1 and coord.X < 8 and coord.Z > 1 and coord.Z < 8) then
                return CurrentBlock
            end
        end
    end
end

local initializeHide = false
local DigBasic = function()
    local Player = game.Players.LocalPlayer
    local character = Player.Character or Player.CharacterAdded:Wait()
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if humanoidRootPart == nil then return end
    local targetUser = nil
    for _, testUser in config.users do
        if game.Players:FindFirstChild(testUser) then
            targetUser = testUser
            break
        end
    end
    while targetUser and game.Players:FindFirstChild(targetUser) do
        while Active:FindFirstChild("Digsite") do
            Player.Character:PivotTo(CFrame.new(Things.Instances:FindFirstChild("Digsite").Teleports.Leave.Position))
            task.wait()                
        end
        DigChestConn = nil
        DigBlockConn = nil
        for i, v in DigBlockTempFolder:GetChildren() do v:Destroy() end
        for i, v in DigChestTempFolder:GetChildren() do v:Destroy() end
        local altToTrade = game.Players:FindFirstChild(targetUser)
        local HasRequest = function() return TradingCmds.HasOutgoingRequestToPlayer(altToTrade) end
        while not HasRequest() and not TradingCmds.GetState() and game.Players:FindFirstChild(targetUser) do
            game.ReplicatedStorage.Network:FindFirstChild("Server: Trading: Request"):InvokeServer(altToTrade)
            task.wait(1)
        end

        if not game.Players:FindFirstChild(targetUser) then break end
    
        while not TradingCmds.GetState() and game.Players:FindFirstChild(targetUser) do task.wait(1) end
        local currentState = TradingCmds.GetState()
        if currentState then
            -- AUTO TRADE ALL ITEM
            local blacklist = {
                "Bucket",
                "Tap Power",
                "Super Lightning",
                "Diamond Shovel",
            }
            for Type, List in Library.Save.Get().Inventory do
                for i, v in pairs(List) do
                    if (getRAP(Type, v) and not table.find(blacklist, v.id)) or (Type == "Currency" and v.id == "Diamonds") then
                        if Type ~= "Pet" or (Type == "Pet" and string.find(v.id, "Huge")) then
                            game:GetService("ReplicatedStorage"):WaitForChild("Network"):WaitForChild("Server: Trading: Set Item"):InvokeServer(currentState._id, Type, i, v._am or 1, v.tn)
                        end
                    end
                end
            end
    
            local localIndex = table.find(TradingCmds.GetState()._players, game.Players.LocalPlayer)
            while TradingCmds.GetState() do
                if TradingCmds.GetState()._ready[localIndex] == false then
                    repeat local readySuccess, readyReason = Library.Network.Invoke("Server: Trading: Set Ready", TradingCmds.GetState()._id, true, TradingCmds.GetState()._counter) until readySuccess                                    
                end
                task.wait(1)
            end
        end
    end
    while Active:FindFirstChild("Digsite") == nil do
        Player.Character:PivotTo(CFrame.new(Things.Instances:FindFirstChild("Digsite").Teleports.Enter.Position))
        task.wait()
    end

    if not initializeHide then
        pcall(function()
            LowGraphics()
            for i, v in Active.Digsite:GetChildren() do
                if v.Name ~= "Important" then
                    v.Parent = game.ReplicatedStorage
                end
            end
            for i, v in workspace.__THINGS:GetChildren() do
                if not (v.Name == "Orbs" or v.Name == "__INSTANCE_CONTAINER" or v.Name == "Lootbags" or v.Name == "Instances") then
                    v.Parent = game.ReplicatedStorage
                end
            end
            for i, v in workspace:GetChildren() do
                if not (v.Name == "__THINGS" or v.Name == "__DEBRIS" or v.Name == Player.Name or v.Name == "Camera" or v.Name == "Terrain") then
                    v.Parent = game.ReplicatedStorage
                end
            end
            for i, v in workspace:GetDescendants() do
                pcall(function() v.CastShadow = false end)
                pcall(function() 
                    if not v:IsDescendantOf(character) then
                        v.Anchored = true 
                    end
                end)
            end
            local Player = game.Players.LocalPlayer
            for i, v in Player.PlayerGui:GetDescendants() do
                pcall(function() v.Enabled = false end)
            end
            for i, v in Player.PlayerScripts:GetDescendants() do
                pcall(function() v:Destroy() end)
            end

            -- CREATE GUI
            if not gui and config.gui then
                gui = Instance.new("ScreenGui")
                gui.Parent = game.Players.LocalPlayer.PlayerGui

                -- Create TextLabels for each text
                for i, row in ipairs(guitexts) do
                    for ii, text in ipairs(row) do
                        local textLabel = Instance.new("TextLabel")
                        textLabel.Parent = gui
                        textLabel.Text = text
                        textLabel.Font = Enum.Font.BuilderSansExtraBold
                        textLabel.TextSize = 72
                        textLabel.Size = UDim2.new(1 / #row, 0, 0.16, 0) -- Adjust as needed
                        textLabel.Position = UDim2.new(0 + (ii - 1) / #row, 0, (i - 1) * 0.16, 0) -- Center text vertically
                        textLabel.AnchorPoint = Vector2.new(0, 0)
                        textLabel.BackgroundColor3 = Color3.new(1, 1, 1)
                        guilabels[i][ii] = textLabel                        
                    end
                end
            end

            spawn(function()
                while config.gui do
                    wait(1)                
                    pcall(function()
                        update()
                    end)
                end
            end)

            local entities = {
                "Stats", 
                "Chat", 
                "Debris",
                "CoreGui",
            }
            for _, entity in entities do
                pcall(function()
                    for i, v in game[entity]:GetDescendants() do
                        pcall(function() v:Destroy() end)
                    end
                end)
            end
        end)
        initializeHide = true
    end
    
    -- HIDE BLOCKS AND CHESTS
    local ActiveChests = Active.Digsite.Important.ActiveChests
    local ActiveBlocks = Active.Digsite.Important.ActiveBlocks
    if not DigChestConn then
        for i, v in pairs(ActiveChests:GetChildren()) do
            v.Parent = DigChestTempFolder
        end
        DigChestConn = ActiveChests.ChildAdded:Connect(function(v)
            task.wait()
            v.Parent = DigChestTempFolder
        end)
    end
    if not DigBlockConn then
        for i, v in pairs(ActiveBlocks:GetChildren()) do
            v.Parent = DigBlockTempFolder
        end
        DigBlockConn = ActiveBlocks.ChildAdded:Connect(function(v)
            task.wait()
            v.Parent = DigBlockTempFolder
        end)
    end

    local Chest = GetChest()
    local Block = GetBlock()
    local j = 1
    
    if Chest then
        while Chest.Parent ~= nil and j < 60 do
            humanoidRootPart.CFrame = Chest:FindFirstChildWhichIsA("BasePart").CFrame
            Network:WaitForChild("Instancing_FireCustomFromClient"):FireServer(CurrentActive().Name, "DigChest", Chest:GetAttribute('Coord'))
            task.wait()
            j = j + 1
        end
        if Chest and j == 60 then
            Chest:Destroy()
        else
            chestcount = chestcount + 1
        end
    elseif Block then
        if config.hop then
            local coord = Block:GetAttribute("Coord")
            if coord.Y >= 127 then
                repeat game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, getServer().id, Player) task.wait(3) until not game.PlaceId
            end
        end
        while Block.Parent ~= nil and j < 200 do
            humanoidRootPart.CFrame = Block.CFrame
            Network:WaitForChild("Instancing_FireCustomFromClient"):FireServer(CurrentActive().Name, "DigBlock", Block:GetAttribute('Coord'))
            task.wait()
            j = j + 1
        end
        if Block and j == 200 then
            Block:Destroy()
        else
            blockcount = blockcount + 1
        end
    end
end

local function getCurrentShovel()
    local digRank = 0
    local currentShovel = nil
    for i, v in pairs(Library.Save.Get().Inventory.Misc) do
        if string.find(v.id, "Shovel") then
            local shovelRank = table.find(SHOP_SHOVELS, v.id)
            if v.id == "Golden Shovel" and digRank < 6 then
                currentShovel = SHOP_SHOVELS[6]
                digRank = 6
            elseif v.id == "Diamond Shovel" then
                currentShovel = SHOP_SHOVELS[9]
                digRank = 9
            elseif shovelRank > digRank then
                currentShovel = v.id
                digRank = shovelRank
            end
        end
    end
    return currentShovel, digRank
end

local function getDigsiteCoins()
    local save = Library.Save.Get().Inventory
    for i, v in pairs(save.Currency) do
        if v.id == "Digsite" then
            return v._am
        end
    end
    return 0
end

local GetTopBlock = function(TargetActiveArea, digRank)
    local blocks = Active[TargetActiveArea].Important.ActiveBlocks:GetChildren()
    local range = SHOVEL_DEPTH[digRank]
    -- DESTROY COORDINATES
    for i, CurrentBlock in pairs(blocks) do
        local coord = CurrentBlock:GetAttribute('Coord')
        if coord.Y >= range[1] and coord.Y <= range[2] then
            return CurrentBlock
        end
    end
    return blocks[#blocks]
end

local DigLevel = function()
    local currentShovel, digRank = getCurrentShovel()
    if digRank >= 9 then
        return true
    end
    local currentCoins = getDigsiteCoins()
    local TargetActiveArea = "AdvancedDigsite"
    if digRank < 6 and not (digRank >= 4 and currentCoins >= NEXT_SHOVEL_COST[currentShovel]) then TargetActiveArea = "Digsite" end
    while Active:FindFirstChild(TargetActiveArea) == nil do
        while #Active:GetChildren() > 0 do
            task.wait()
            pcall(function()
                Player.Character:PivotTo(CFrame.new(Things.Instances:FindFirstChild(Active:GetChildren()[1].Name).Teleports.Leave.Position))            
            end)
        end
        Player.Character:PivotTo(CFrame.new(Things.Instances:FindFirstChild(TargetActiveArea).Teleports.Enter.Position))
        task.wait()
    end
    if currentCoins >= NEXT_SHOVEL_COST[currentShovel] then
        game:GetService("ReplicatedStorage"):WaitForChild("Network"):WaitForChild("DigsiteMerchant_PurchaseShovel"):InvokeServer(NEXT_SHOVEL[currentShovel])
    end

    -- DIGGING PART
    local Chest = Active[TargetActiveArea].Important.ActiveChests:GetChildren()[1]
    local Block = GetTopBlock(TargetActiveArea, digRank)
    local j = 1
    if Chest then
        while Chest.Parent ~= nil and j < 50 do
            humanoidRootPart.CFrame = Chest:FindFirstChildWhichIsA("BasePart").CFrame
            Network:WaitForChild("Instancing_FireCustomFromClient"):FireServer(CurrentActive().Name, "DigChest", Chest:GetAttribute('Coord'))
            task.wait()
            j = j + 1
        end
        if Chest and j == 50 then
            Chest:Destroy()
        end
    elseif Block then
        while Block.Parent ~= nil and j < 50 do
            humanoidRootPart.CFrame = Block.CFrame
            Network:WaitForChild("Instancing_FireCustomFromClient"):FireServer(CurrentActive().Name, "DigBlock", Block:GetAttribute('Coord'))
            task.wait()
            j = j + 1
        end
        if Block and j == 50 then
            Block:Destroy()
        end
    end
end

-- FARMING AUTO CLAIM MAIL
spawn(function()
    while true do
        task.wait(60)
        pcall(function()
            Network:WaitForChild("Mailbox: Claim All"):InvokeServer()
        end)
    end
end)

-- FARMING PART [AUTO LOOT] --
spawn(function()
    for i, v in workspace.__THINGS.Orbs:GetChildren() do
        Network["Orbs: Collect"]:FireServer({ tonumber(v.Name) })
        Network["Orbs_ClaimMultiple"]:FireServer({ [1] = { [1] = v.Name } })
        task.wait()
        v:Destroy()            
    end
    for i, v in workspace.__THINGS.Lootbags:GetChildren() do
        Network.Lootbags_Claim:FireServer({ v.Name })
        task.wait()
        v:Destroy()
    end
    workspace.__THINGS.Orbs.ChildAdded:Connect(function(v)
        Network["Orbs: Collect"]:FireServer({ tonumber(v.Name) })
        Network["Orbs_ClaimMultiple"]:FireServer({ [1] = { [1] = v.Name } })
        task.wait()
        v:Destroy()
    end)
    workspace.__THINGS.Lootbags.ChildAdded:Connect(function(v)
        Network.Lootbags_Claim:FireServer({ v.Name })
        task.wait()
        v:Destroy()
    end)
end)

-- DIGGING PART --
spawn(function()
    for i, v in next, Player.Character:GetChildren() do
        if v:IsA("BasePart") then
            v.CanCollide = false
        end
    end

    -- Dig Function
    spawn(function()
        -- Claim Flimsy Shovel
        local shovel, amount = findItem("Flimsy Shovel")
        if shovel == nil or amount == nil or amount == 0 then
            while Active:FindFirstChild("Digsite") == nil do
                Player.Character:PivotTo(CFrame.new(Things.Instances:FindFirstChild("Digsite").Teleports.Enter
                .Position))
                task.wait(1)
            end
            while shovel == nil do
                Network:WaitForChild(
                "Instancing_FireCustomFromClient"):FireServer("Digsite", "ClaimShovel")
                task.wait(1)
                shovel, amount = findItem("Flimsy Shovel")
            end
            while Active:FindFirstChild("Digsite") ~= nil do
                Player.Character:PivotTo(CFrame.new(Things.Instances:FindFirstChild("Digsite").Teleports.Leave
                .Position))
                task.wait(1)
            end
        end

        -- IF NO AMETHYST SHOVEL YET PROCEED WITH LEVEL SHOVEL
        local shovel, amount = findItem("Amethyst Shovel")
        if not shovel then
            while task.wait() do
                if DigLevel() then break end
            end
        end

        while task.wait() do
            pcall(function()
                DigBasic()
            end)
        end
    end)
end)
