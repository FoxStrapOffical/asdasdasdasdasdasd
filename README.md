-- ================================================================
-- KITTYHUB V2 - EDUCATIONAL SCRIPT (ANTI-CHEAT TEST)
-- ALL TEAM CHECKS REMOVED, FULL FEATURE SET
-- ================================================================

-- BRIEF NOTIFICATION
local function briefNotify(text)
    local notif = Instance.new("ScreenGui")
    notif.Name = "KittyHubBriefNotif"
    notif.ResetOnSpawn = false
    notif.DisplayOrder = 999
    notif.ZIndexBehavior = Enum.ZIndexBehavior.Global
    notif.IgnoreGuiInset = true
    notif.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 280, 0, 24)
    frame.Position = UDim2.new(1, -290, 1, -34)
    frame.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
    frame.BackgroundTransparency = 1
    frame.BorderSizePixel = 0
    frame.ZIndex = 10
    frame.Parent = notif
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
    local stroke = Instance.new("UIStroke")
    stroke.Color = Color3.fromRGB(130,130,130)
    stroke.Thickness = 1
    stroke.Transparency = 0.4
    stroke.Parent = frame
    local lbl = Instance.new("TextLabel")
    lbl.Size = UDim2.new(1,-12,1,0)
    lbl.Position = UDim2.new(0,6,0,0)
    lbl.BackgroundTransparency = 1
    lbl.Text = text
    lbl.TextColor3 = Color3.fromRGB(215,215,215)
    lbl.TextSize = 10
    lbl.Font = Enum.Font.GothamSemibold
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.TextTransparency = 1
    lbl.ZIndex = 10
    lbl.Parent = frame
    game:GetService("TweenService"):Create(frame, TweenInfo.new(0.25), {BackgroundTransparency = 0.22}):Play()
    game:GetService("TweenService"):Create(lbl, TweenInfo.new(0.25), {TextTransparency = 0}):Play()
    task.delay(3, function()
        game:GetService("TweenService"):Create(frame, TweenInfo.new(0.35), {BackgroundTransparency = 1}):Play()
        game:GetService("TweenService"):Create(lbl, TweenInfo.new(0.35), {TextTransparency = 1}):Play()
        task.wait(0.4)
        notif:Destroy()
    end)
end
briefNotify("KittyHub V2 - Use responsibly. Detection outside Rivals increased.")

-- ================================================================
-- SERVICES AND CORE GLOBALS
-- ================================================================
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local HttpService = game:GetService("HttpService")
local Camera = workspace.CurrentCamera
local VirtualUser = game:GetService("VirtualUser")
local Stats = game:GetService("Stats")
local NetworkClient = game:GetService("NetworkClient")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local CoreGui = game:GetService("CoreGui")
local LP = Players.LocalPlayer

-- Owner IDs (first ID is primary for /dev)
local OWNER_IDS = {
    10344279846,   -- Your ID (has /dev access)
    4458239590     -- Friend's ID (key bypass only, no /dev)
}

-- ================================================================
-- STATIC KEY LIST — 88 FRESH V2 KEYS
-- ================================================================
local STATIC_KEYS = {
    "V2A1-XQ9M-LP7W-R3NB","V2A2-JT8K-HC6Z-DS2F","V2A3-MN7P-QW5R-ET4Y",
    "V2A4-VP8L-KJ3H-GF9D","V2A5-NB6X-ZM4V-CS2A","V2A6-QR5T-YU3O-IP7W",
    "V2A7-AS4D-FG2J-HK8L","V2A8-ZX3V-CB1M-NQ6P","V2A9-WE2T-RY7I-UO5P",
    "V2B0-LK1H-JG6D-SF4A","V2B1-PO3U-IY8R-TE6W","V2B2-QW7R-ET5I-UO3P",
    "V2B3-AS6F-DH4G-JK2L","V2B4-ZX5V-CB3M-NP1Q","V2B5-WE4T-RY2I-UO8P",
    "V2B6-LK9H-JG7D-SF5A","V2B7-MN8Q-PB6Z-XM4C","V2B8-VS3A-QR1Y-TU9I",
    "V2B9-OP7W-AS5F-DG3H","V2C0-JK1L-ZX9V-CB7M","V2C1-MP5Q-WE3T-RY1U",
    "V2C2-IO7P-LK5H-JG3F","V2C3-DS1A-QW9R-ET7Y","V2C4-UI5O-AS3F-DH1G",
    "V2C5-JK7L-ZX5V-CB3M","V2C6-MQ1P-WE9T-RY7U","V2C7-IO5P-LK3H-JG1F",
    "V2C8-DS9A-QR7Y-TU5I","V2C9-OP3W-AS1F-DG9H","V2D0-JK7L-ZX5V-CB3N",
    "V2D1-MQ9P-WE7T-RY5U","V2D2-IO3P-LK1H-JG9F","V2D3-DS7A-QW5R-ET3Y",
    "V2D4-UI1O-AS9F-DH7G","V2D5-JK5L-ZX3V-CB1M","V2D6-MQ7P-WE5T-RY3U",
    "V2D7-IO1P-LK9H-JG7F","V2D8-DS5A-QR3Y-TU1I","V2D9-OP9W-AS7F-DG5H",
    "V2E0-JK3L-ZX1V-CB9M","V2E1-KP8Q-NW6T-MR4Y","V2E2-HJ7K-LG5D-SF3A",
    "V2E3-XC6B-VN4M-QP2W","V2E4-ER5T-YU3I-OP1L","V2E5-KJ4H-GF2D-SA9Q",
    "V2E6-NB3X-ZM1V-CS7A","V2E7-QR2T-YU9O-IP5W","V2E8-AS1D-FG7J-HK5L",
    "V2E9-ZX9V-CB7M-NQ3P","V2F0-WE8T-RY5I-UO3P","V2F1-LK7H-JG5D-SF1A",
    "V2F2-PO2U-IY6R-TE4W","V2F3-QW5R-ET3I-UO1P","V2F4-AS4F-DH2G-JK9L",
    "V2F5-ZX3V-CB9M-NP7Q","V2F6-WE2T-RY8I-UO6P","V2F7-LK8H-JG6D-SF4A",
    "V2F8-MN7Q-PB5Z-XM3C","V2F9-VS2A-QR9Y-TU7I","V2G0-OP6W-AS4F-DG2H",
    "V2G1-JK9L-ZX7V-CB5M","V2G2-MP4Q-WE2T-RY9U","V2G3-IO8P-LK6H-JG4F",
    "V2G4-DS3A-QW1R-ET8Y","V2G5-UI9O-AS7F-DH5G","V2G6-JK6L-ZX4V-CB2M",
    "V2G7-MQ3P-WE1T-RY8U","V2G8-IO6P-LK4H-JG2F","V2G9-DS2A-QR9Y-TU6I",
    "V2H0-OP8W-AS6F-DG4H","V2H1-JK2L-ZX8V-CB6M","V2H2-MQ5P-WE3T-RY6U",
    "V2H3-IO9P-LK7H-JG5F","V2H4-DS4A-QW2R-ET9Y","V2H5-UI6O-AS4F-DH2G",
    "V2H6-JK4L-ZX2V-CB8M","V2H7-MQ1P-WE8T-RY5U","V2H8-IO4P-LK2H-JG9F",
    "V2H9-DS8A-QR6Y-TU4I","V2I0-OP4W-AS2F-DG8H","V2I1-JK8L-ZX6V-CB4M",
    "V2I2-MP3Q-WE9T-RY2U","V2I3-IO2P-LK8H-JG6F","V2I4-DS6A-QW4R-ET2Y",
    "V2I5-UI4O-AS2F-DH9G","V2I6-JK2L-ZX9V-CB7N","V2I7-MQ6P-WE4T-RY1U",
    "V2I8-IO5P-LK3H-JG8F"
}

-- ================================================================
-- VALIDATION — REMEMBER ME (Multiple Owners)
-- ================================================================
local VALIDATION_FILE = "KittyHubV2_Validated.txt"
local function isUserValidated()
    for _, id in ipairs(OWNER_IDS) do
        if LP.UserId == id then return true end
    end
    local ok, c = pcall(function() if readfile then return readfile(VALIDATION_FILE) end end)
    return ok and c == tostring(LP.UserId)
end
local function markUserValidated()
    pcall(function() if writefile then writefile(VALIDATION_FILE, tostring(LP.UserId)) end end)
end

-- ================================================================
-- DRAWING HELPERS
-- ================================================================
local function newLine(thick, col)
    local l = Drawing.new("Line")
    l.Thickness = thick or 1.5
    l.Color = col or Color3.new(1,1,1)
    l.Visible = false
    return l
end
local function newText(sz, col)
    local t = Drawing.new("Text")
    t.Size = sz or 13
    t.Color = col or Color3.new(1,1,1)
    t.Outline = true
    t.OutlineColor = Color3.new(0,0,0)
    t.Visible = false
    return t
end
local function newCirc(r, filled, col)
    local c = Drawing.new("Circle")
    c.Radius = r
    c.Filled = filled
    c.Color = col or Color3.new(1,1,1)
    c.Visible = false
    return c
end

-- ================================================================
-- KEY ENTRY GUI — KittyHub V2
-- ================================================================
local function showKeyEntry(errMsg)
    UIS.MouseIconEnabled = true
    local kg = Instance.new("ScreenGui")
    kg.Name = "KittyHubV2KeyEntry"
    kg.ResetOnSpawn = false
    kg.DisplayOrder = 999
    kg.ZIndexBehavior = Enum.ZIndexBehavior.Global
    kg.IgnoreGuiInset = true
    kg.Parent = LP:WaitForChild("PlayerGui")
    local f = Instance.new("Frame")
    f.Size = UDim2.new(0, 310, 0, 200)
    f.Position = UDim2.new(0.5, -155, 0.5, -100)
    f.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
    f.BackgroundTransparency = 0.06
    f.BorderSizePixel = 0
    f.ZIndex = 10
    f.Parent = kg
    Instance.new("UICorner", f).CornerRadius = UDim.new(0, 16)
    local st = Instance.new("UIStroke")
    st.Color = Color3.fromRGB(130,130,130)
    st.Thickness = 2
    st.Transparency = 0.3
    st.Parent = f
    local iconL = Instance.new("ImageLabel")
    iconL.Size = UDim2.new(0,28,0,28)
    iconL.Position = UDim2.new(0,6,0,6)
    iconL.BackgroundTransparency = 1
    iconL.Image = "rbxthumb://type=Asset&id=12140499158&w=56&h=56"
    iconL.ZIndex = 11
    iconL.Parent = f
    local iconR = Instance.new("ImageLabel")
    iconR.Size = UDim2.new(0,28,0,28)
    iconR.Position = UDim2.new(1,-34,0,6)
    iconR.BackgroundTransparency = 1
    iconR.Image = "rbxthumb://type=Asset&id=12140499158&w=56&h=56"
    iconR.ZIndex = 11
    iconR.Parent = f
    local function fl(txt, sz, col, posY, xa)
        local l = Instance.new("TextLabel")
        l.BackgroundTransparency = 1
        l.Text = txt
        l.TextColor3 = col
        l.TextSize = sz
        l.Font = Enum.Font.GothamBold
        l.TextXAlignment = xa or Enum.TextXAlignment.Center
        l.ZIndex = 10
        l.Size = UDim2.new(1,0,0,25)
        l.Position = UDim2.new(0,0,0,posY)
        l.Parent = f
        return l
    end
    fl("Enter KittyHub V2 Key", 14, Color3.fromRGB(235,235,235), 12)
    fl("discord.gg/cTtSuya3Rb", 11, Color3.fromRGB(185,185,185), 36)
    local box = Instance.new("TextBox")
    box.Size = UDim2.new(0.82,0,0,32)
    box.Position = UDim2.new(0.09,0,0,68)
    box.BackgroundColor3 = Color3.fromRGB(40,40,40)
    box.TextColor3 = Color3.fromRGB(255,255,255)
    box.PlaceholderText = "V2XX-XXXX-XXXX-XXXX"
    box.PlaceholderColor3 = Color3.fromRGB(120,120,120)
    box.TextSize = 13
    box.Font = Enum.Font.Code
    box.BorderSizePixel = 0
    box.ZIndex = 10
    box.Parent = f
    Instance.new("UICorner", box).CornerRadius = UDim.new(0, 6)
    local errLbl = Instance.new("TextLabel")
    errLbl.Size = UDim2.new(0.82,0,0,18)
    errLbl.Position = UDim2.new(0.09,0,0,108)
    errLbl.BackgroundTransparency = 1
    errLbl.Text = errMsg or ""
    errLbl.TextColor3 = Color3.fromRGB(255,100,100)
    errLbl.TextSize = 11
    errLbl.Font = Enum.Font.Gotham
    errLbl.ZIndex = 10
    errLbl.Parent = f
    local sub = Instance.new("TextButton")
    sub.Size = UDim2.new(0.52,0,0,30)
    sub.Position = UDim2.new(0.24,0,0,155)
    sub.BackgroundColor3 = Color3.fromRGB(65,65,65)
    sub.Text = "Submit (5s)"
    sub.TextColor3 = Color3.fromRGB(255,255,255)
    sub.TextSize = 13
    sub.Font = Enum.Font.GothamBold
    sub.BorderSizePixel = 0
    sub.ZIndex = 10
    sub.AutoButtonColor = false
    sub.Parent = f
    Instance.new("UICorner", sub).CornerRadius = UDim.new(0, 8)
    local t0 = tick()
    local tc = RunService.RenderStepped:Connect(function()
        local r = math.max(0, 5 - (tick() - t0))
        if r > 0 then
            sub.Text = string.format("Submit (%.1f)", r)
            sub.BackgroundColor3 = Color3.fromRGB(45,45,45)
        else
            sub.Text = "Submit"
            sub.BackgroundColor3 = Color3.fromRGB(88,88,88)
        end
    end)
    sub.MouseButton1Click:Connect(function()
        if tick() - t0 < 5 then return end
        local key = box.Text:gsub("%s",""):upper()
        if table.find(STATIC_KEYS, key) then
            markUserValidated()
            tc:Disconnect()
            UIS.MouseIconEnabled = false
            kg:Destroy()
            loadMainScript()
        else
            errLbl.Text = "Invalid V2 key - discord.gg/cTtSuya3Rb"
        end
    end)
    box.FocusLost:Connect(function(enter)
        if enter then sub.MouseButton1Click:Fire() end
    end)
end

-- ================================================================
-- ADMIN PANEL (/dev — only primary owner)
-- ================================================================
local function showAdminPanel()
    if LP.UserId ~= OWNER_IDS[1] then return end  -- Only original owner
    UIS.MouseIconEnabled = true
    local ag = Instance.new("ScreenGui")
    ag.Name = "KittyHubV2Admin"
    ag.ResetOnSpawn = false
    ag.DisplayOrder = 1000
    ag.ZIndexBehavior = Enum.ZIndexBehavior.Global
    ag.Parent = LP:WaitForChild("PlayerGui")
    local mf = Instance.new("Frame")
    mf.Size = UDim2.new(0,600,0,500)
    mf.Position = UDim2.new(0.5,-300,0.5,-250)
    mf.BackgroundColor3 = Color3.fromRGB(28,28,33)
    mf.BorderSizePixel = 0
    mf.ZIndex = 10
    mf.Parent = ag
    Instance.new("UICorner", mf).CornerRadius = UDim.new(0, 12)
    local tb = Instance.new("Frame")
    tb.Size = UDim2.new(1,0,0,40)
    tb.BackgroundColor3 = Color3.fromRGB(65,65,65)
    tb.BorderSizePixel = 0
    tb.ZIndex = 11
    tb.Parent = mf
    Instance.new("UICorner", tb).CornerRadius = UDim.new(0, 12)
    local ti = Instance.new("TextLabel")
    ti.Size = UDim2.new(1,-80,1,0)
    ti.Position = UDim2.new(0,10,0,0)
    ti.BackgroundTransparency = 1
    ti.Text = "KittyHub V2 Admin Panel - " .. LP.Name
    ti.TextColor3 = Color3.fromRGB(255,255,255)
    ti.TextSize = 17
    ti.Font = Enum.Font.GothamBold
    ti.TextXAlignment = Enum.TextXAlignment.Left
    ti.ZIndex = 11
    ti.Parent = tb
    local cb = Instance.new("TextButton")
    cb.Size = UDim2.new(0,30,0,30)
    cb.Position = UDim2.new(1,-35,0,5)
    cb.BackgroundColor3 = Color3.fromRGB(145,45,45)
    cb.Text = "X"
    cb.TextColor3 = Color3.fromRGB(255,255,255)
    cb.TextSize = 18
    cb.Font = Enum.Font.GothamBold
    cb.BorderSizePixel = 0
    cb.ZIndex = 11
    cb.Parent = tb
    Instance.new("UICorner", cb).CornerRadius = UDim.new(0, 6)
    cb.MouseButton1Click:Connect(function() UIS.MouseIconEnabled = false; ag:Destroy() end)

    local tabHolder = Instance.new("Frame")
    tabHolder.Size = UDim2.new(1,-20,0,30)
    tabHolder.Position = UDim2.new(0,10,0,50)
    tabHolder.BackgroundTransparency = 1
    tabHolder.Parent = mf
    local pages = {}
    local function createTab(name)
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(0,100,1,0)
        btn.Position = UDim2.new(0, #pages*105, 0,0)
        btn.BackgroundColor3 = Color3.fromRGB(55,55,55)
        btn.Text = name
        btn.TextColor3 = Color3.fromRGB(255,255,255)
        btn.TextSize = 14
        btn.Font = Enum.Font.GothamBold
        btn.BorderSizePixel = 0
        btn.ZIndex = 11
        btn.Parent = tabHolder
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
        local page = Instance.new("ScrollingFrame")
        page.Size = UDim2.new(1,-20,1,-85)
        page.Position = UDim2.new(0,10,0,85)
        page.BackgroundTransparency = 1
        page.BorderSizePixel = 0
        page.ScrollBarThickness = 8
        page.ZIndex = 10
        page.Visible = (#pages == 0)
        page.Parent = mf
        btn.MouseButton1Click:Connect(function()
            for _, p in ipairs(pages) do p.page.Visible = false end
            page.Visible = true
        end)
        table.insert(pages, {btn=btn, page=page})
        return page
    end

    local keysPage = createTab("Keys")
    local gpPage = createTab("Gamepasses")
    
    local y = 5
    for _, key in ipairs(STATIC_KEYS) do
        local it = Instance.new("Frame")
        it.Size = UDim2.new(1,-10,0,40)
        it.Position = UDim2.new(0,5,0,y)
        it.BackgroundColor3 = Color3.fromRGB(45,35,60)
        it.BorderSizePixel = 0
        it.ZIndex = 10
        it.Parent = keysPage
        Instance.new("UICorner", it).CornerRadius = UDim.new(0, 6)
        local kl = Instance.new("TextLabel")
        kl.Size = UDim2.new(0.72,0,1,0)
        kl.Position = UDim2.new(0,10,0,0)
        kl.BackgroundTransparency = 1
        kl.Text = key
        kl.TextColor3 = Color3.fromRGB(195,155,255)
        kl.TextSize = 13
        kl.Font = Enum.Font.Code
        kl.TextXAlignment = Enum.TextXAlignment.Left
        kl.ZIndex = 10
        kl.Parent = it
        local cp = Instance.new("TextButton")
        cp.Size = UDim2.new(0.2,0,0.7,0)
        cp.Position = UDim2.new(0.76,0,0.15,0)
        cp.BackgroundColor3 = Color3.fromRGB(85,55,135)
        cp.Text = "Copy"
        cp.TextColor3 = Color3.fromRGB(255,255,255)
        cp.TextSize = 12
        cp.Font = Enum.Font.GothamBold
        cp.BorderSizePixel = 0
        cp.ZIndex = 10
        cp.Parent = it
        Instance.new("UICorner", cp).CornerRadius = UDim.new(0, 4)
        local capturedKey = key
        cp.MouseButton1Click:Connect(function()
            pcall(function() setclipboard(capturedKey) end)
        end)
        y = y + 45
    end
    keysPage.CanvasSize = UDim2.new(0,0,0,y+10)

    local GAMEPASS_IDS = {
        ["Key Beta Testers"] = 1797193305,
        ["Key Vstaff"] = 1799991040,
        ["Key No Role"] = 1797409420
    }
    local gpWhiteList = {}
    local gpY = 5
    local function addGpEntry(name, id)
        local frame = Instance.new("Frame")
        frame.Size = UDim2.new(1,-10,0,60)
        frame.Position = UDim2.new(0,5,0,gpY)
        frame.BackgroundColor3 = Color3.fromRGB(45,45,50)
        frame.BorderSizePixel = 0
        frame.ZIndex = 10
        frame.Parent = gpPage
        Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
        local lbl = Instance.new("TextLabel")
        lbl.Size = UDim2.new(0.6,0,1,0)
        lbl.Position = UDim2.new(0,10,0,0)
        lbl.BackgroundTransparency = 1
        lbl.Text = name .. " (ID: "..id..")"
        lbl.TextColor3 = Color3.fromRGB(215,215,215)
        lbl.TextSize = 13
        lbl.Font = Enum.Font.Gotham
        lbl.TextXAlignment = Enum.TextXAlignment.Left
        lbl.ZIndex = 10
        lbl.Parent = frame
        local whitelistBox = Instance.new("TextBox")
        whitelistBox.Size = UDim2.new(0,120,0,28)
        whitelistBox.Position = UDim2.new(0.62,0,0.5,-14)
        whitelistBox.BackgroundColor3 = Color3.fromRGB(40,40,40)
        whitelistBox.TextColor3 = Color3.fromRGB(255,255,255)
        whitelistBox.PlaceholderText = "UserID"
        whitelistBox.TextSize = 12
        whitelistBox.Font = Enum.Font.Code
        whitelistBox.BorderSizePixel = 0
        whitelistBox.ZIndex = 10
        whitelistBox.Parent = frame
        Instance.new("UICorner", whitelistBox).CornerRadius = UDim.new(0, 4)
        local addBtn = Instance.new("TextButton")
        addBtn.Size = UDim2.new(0,70,0,28)
        addBtn.Position = UDim2.new(0.82,0,0.5,-14)
        addBtn.BackgroundColor3 = Color3.fromRGB(75,135,75)
        addBtn.Text = "Whitelist"
        addBtn.TextColor3 = Color3.fromRGB(255,255,255)
        addBtn.TextSize = 12
        addBtn.Font = Enum.Font.GothamBold
        addBtn.BorderSizePixel = 0
        addBtn.ZIndex = 10
        addBtn.Parent = frame
        Instance.new("UICorner", addBtn).CornerRadius = UDim.new(0, 4)
        addBtn.MouseButton1Click:Connect(function()
            local uid = tonumber(whitelistBox.Text)
            if uid then
                gpWhiteList[uid] = true
                briefNotify("User "..uid.." whitelisted for "..name)
            end
        end)
        local remBtn = Instance.new("TextButton")
        remBtn.Size = UDim2.new(0,70,0,28)
        remBtn.Position = UDim2.new(0.92,0,0.5,-14)
        remBtn.BackgroundColor3 = Color3.fromRGB(135,75,75)
        remBtn.Text = "Remove"
        remBtn.TextColor3 = Color3.fromRGB(255,255,255)
        remBtn.TextSize = 12
        remBtn.Font = Enum.Font.GothamBold
        remBtn.BorderSizePixel = 0
        remBtn.ZIndex = 10
        remBtn.Parent = frame
        Instance.new("UICorner", remBtn).CornerRadius = UDim.new(0, 4)
        remBtn.MouseButton1Click:Connect(function()
            local uid = tonumber(whitelistBox.Text)
            if uid then
                gpWhiteList[uid] = nil
                briefNotify("User "..uid.." removed from whitelist for "..name)
            end
        end)
        gpY = gpY + 65
    end
    for name, id in pairs(GAMEPASS_IDS) do
        addGpEntry(name, id)
    end
    gpPage.CanvasSize = UDim2.new(0,0,0,gpY+10)

    _G.CheckGamepassOwnership = function(userId, passId)
        for _, id in ipairs(OWNER_IDS) do
            if userId == id then return true end
        end
        if gpWhiteList[userId] then return true end
        local success, owns = pcall(function()
            return game:GetService("MarketplaceService"):UserOwnsGamePassAsync(userId, passId)
        end)
        return success and owns
    end
end
LP.Chatted:Connect(function(msg)
    if msg == "/dev" and LP.UserId == OWNER_IDS[1] then
        showAdminPanel()
    end
end)

-- ================================================================
-- STATE TABLE
-- ================================================================
local S = {}
local function initState()
    S.chamsEnabled = false
    S.chamsFill = Color3.fromRGB(255, 240, 150)
    S.chamsOutline = Color3.fromRGB(0, 0, 0)
    S.chamsFillTr = 0.4
    S.chamsOutTr = 0
    S.chamsMaxDist = 500
    S.highlights = {}
    S.boxESP = false
    S.boxStyle = "Full"
    S.boxColor = Color3.fromRGB(255, 182, 193)
    S.boxWidth = 0.38
    S.boxThickness = 1
    S.cornerThickness = 1.5
    S.wireframeESP = false
    S.wireframeColor = Color3.fromRGB(173, 216, 230)
    S.wireframeThickness = 0.08
    S.wireframeCache = {}
    S.nameESP = false
    S.nameFontSize = 10
    S.distESP = false
    S.hpBarPos = "Right"
    S.hpBarColor = Color3.fromRGB(75,215,75)
    S.skelESP = false
    S.skelColor = Color3.fromRGB(255, 182, 193)
    S.skelThickness = 1.2
    S.circPiece = false
    S.circDrawings = {}
    S.snapEnabled = false
    S.snapColor = Color3.fromRGB(255, 182, 193)
    S.snapMode = "Cursor"
    S.fovEnabled = false
    S.fovRadius = 100
    S.fovMode = "Mouse"
    S.fovFilled = false
    S.fovFillColor = Color3.fromRGB(255, 182, 193)
    S.fovFillTr = 0.21
    S.glassWalls = false
    S.glassCache = {}
    S.tintEnabled = false
    S.tintColor = Color3.fromRGB(190, 130, 255)
    S.tintBright = 0.15
    S.colorCorr = nil
    S.darkTextures = false
    S.darkTexCache = {}
    S.skyboxEnabled = false
    S.skyboxId = ""
    S.currentSky = nil
    S.crossEnabled = false
    S.crossType = "cross"
    S.crossSize = 21
    S.crossThick = 3
    S.crossColor = Color3.fromRGB(255, 182, 193)
    S.crossSpin = false
    S.crossSpinSpd = 2.2
    S.crossAngle = 0
    S.watermarkOn = false
    S.wideCamera = false
    S.wideIntensity = 3.5
    S.lockSky = false
    S.skyTime = 0
    S.snowEnabled = false
    S.snowPart = nil
    S.textureNuker = false
    S.nukedParts = {}
    S.lastNuke = 0
    S.motionBlurEnabled = false
    S.motionBlurStrength = 4
    S.forcedFOV = 179
    S.alwaysJump = false
    S.extendSlideEnabled = false
    S.extendSlideSpeed = 18
    S.extendSlideKey = Enum.KeyCode.C
    S.extendSlideActive = false
    S.extendSlideDuration = 3
    S.aimbotOn = false
    S.aimbotAggr = 95
    S.aimbotDist = 500
    S.aimbotPart = "Head"
    S.visCheck = false
    S.aimSens = 1.0
    S.aimPrediction = 0.0
    -- Silent Aim
    S.silentAimEnabled = false
    S.silentAimFOV = 120
    S.silentAimTransparency = 0.5
    S.silentAimColor = Color3.fromRGB(255, 255, 255)
    S.hitSoundOn = false
    S.hitSoundVol = 0.93
    S.hitSoundId = "rbxassetid://135596452339298"
    S.killSoundOn = false
    S.killSoundVol = 20
    S.killSoundId = "rbxassetid://138215774591879"
    S.killChatSpam = false
    S.jitter = false
    S.jitterStr = 12
    S.hitMsg = ""
    S.hitNotifOn = false
    S.noclipOn = false
    S.flightOn = false
    S.flightSpeed = 50
    S.cfOn = false
    S.cfSpeed = 50
    S.strafeOn = false
    S.strafeKey = Enum.KeyCode.R
    S.strafeRMB = false
    S.strafeDist = 3
    S.strafeDir = 1
    S.autoBackstab = false
    S.followTarget = false
    S.orbitActive = false
    S.orbitHeight = 4
    S.orbitBind = Enum.KeyCode.O
    S.backstabBind = Enum.KeyCode.B
    S.followBind = Enum.KeyCode.L
    S.knifeKey = Enum.KeyCode.Three
    S.antiKicia = false
    S.antiKiciaBind = Enum.KeyCode.K
    S.akDist = 5
    S.akHeight = 0
    S.akChaos = 12
    S.akAimAssist = true
    S.floorbang = false
    S.floorbangKey = Enum.KeyCode.U
    S.floorbangSpinEnabled = false
    S.floorbangSpinSpeed = 5
    S.floorbangHeightOffset = -5
    S.floorbangAimAggr = 100
    S.antiHit = false
    S.antiHitKey = Enum.KeyCode.J
    S.antiHitSpeed = 10
    S.antiHitRadius = 3
    S.wrapEnabled = false
    S.wrapName = ""
    S.deviceSpoofTarget = "VR"
    S.deviceSpoofEnabled = false
    S.winStreakSpoof = false
    S.winStreakTarget = 10
    S.skinChangerLoaded = false
    S.hitSoundsList = {
        "Pop Sound","Creamy Keyboard","Sussy","Hitmarker","MLG",
        "Punch","Ding","Fortnite Shield Crack","Clapping"
    }
    S.hitSoundIds = {
        ["Pop Sound"] = "rbxassetid://140550289436553",
        ["Creamy Keyboard"] = "rbxassetid://140494750798924",
        ["Sussy"] = "rbxassetid://138719267809645",
        ["Hitmarker"] = "rbxassetid://138832207290954",
        ["MLG"] = "rbxassetid://118069354428047",
        ["Punch"] = "rbxassetid://84899000629835",
        ["Ding"] = "rbxassetid://137874566525685",
        ["Fortnite Shield Crack"] = "rbxassetid://135596452339298",
        ["Clapping"] = "rbxassetid://130596072444862"
    }
    S.killSoundsList = {
        "kill sound","wow","anime girl","joe bart","use your brain",
        "crash out","Fortnite Downed","Victory Royale","You King"
    }
    S.killSoundIds = {
        ["kill sound"] = "rbxassetid://137613835560539",
        ["wow"] = "rbxassetid://137612233332702",
        ["anime girl"] = "rbxassetid://135783751129220",
        ["joe bart"] = "rbxassetid://139766870825377",
        ["use your brain"] = "rbxassetid://138215774591879",
        ["crash out"] = "rbxassetid://84899435095135",
        ["Fortnite Downed"] = "rbxassetid://134390262954871",
        ["Victory Royale"] = "rbxassetid://111604678283753",
        ["You King"] = "rbxassetid://114974301548754"
    }
    S.killChatMsgs = {
        "KittyHub V2 On Top!!","Get KittyHub V2 Next Time","KittyHub V2 > Kicia, UE",
        "Join Today! discord.gg/cTtSuya3Rb","KittyHub V2 > You",
        "Stay down, next time get KittyHub V2","V2 says: Try harder",
        "KittyHub V2 on top, you on bottom","Skill issue? Get KittyHub V2"
    }
end

-- ================================================================
-- NOTIFICATION SYSTEM
-- ================================================================
local NotifGui, showNotification
local activeNotifs = {}
local function initNotifs()
    NotifGui = Instance.new("ScreenGui")
    NotifGui.Name = "KittyHubV2Notifs"
    NotifGui.ResetOnSpawn = false
    NotifGui.DisplayOrder = 1000
    NotifGui.ZIndexBehavior = Enum.ZIndexBehavior.Global
    NotifGui.IgnoreGuiInset = true
    NotifGui.Parent = LP:WaitForChild("PlayerGui")
    showNotification = function(text)
        local baseY = 0.70
        local frame = Instance.new("Frame")
        frame.Size = UDim2.new(0, 250, 0, 32)
        frame.Position = UDim2.new(0.5,-125, baseY, -16 + (#activeNotifs) * 38)
        frame.BackgroundColor3 = Color3.fromRGB(28,28,28)
        frame.BackgroundTransparency = 1
        frame.BorderSizePixel = 0
        frame.ZIndex = 10
        frame.Parent = NotifGui
        Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
        local st2 = Instance.new("UIStroke")
        st2.Color = Color3.fromRGB(95,95,95)
        st2.Thickness = 1
        st2.Transparency = 0.4
        st2.Parent = frame
        local lbl = Instance.new("TextLabel")
        lbl.Size = UDim2.new(1,-16,1,0)
        lbl.Position = UDim2.new(0,8,0,0)
        lbl.BackgroundTransparency = 1
        lbl.Text = text
        lbl.TextColor3 = Color3.fromRGB(215,215,215)
        lbl.TextSize = 12
        lbl.Font = Enum.Font.GothamSemibold
        lbl.TextXAlignment = Enum.TextXAlignment.Left
        lbl.TextTransparency = 1
        lbl.ZIndex = 10
        lbl.Parent = frame
        TweenService:Create(frame, TweenInfo.new(0.25), {BackgroundTransparency = 0.22}):Play()
        TweenService:Create(lbl, TweenInfo.new(0.25), {TextTransparency = 0}):Play()
        table.insert(activeNotifs, frame)
        task.delay(2.8, function()
            TweenService:Create(frame, TweenInfo.new(0.35), {BackgroundTransparency = 1}):Play()
            TweenService:Create(lbl, TweenInfo.new(0.35), {TextTransparency = 1}):Play()
            task.wait(0.4)
            frame:Destroy()
            local idx = table.find(activeNotifs, frame)
            if idx then table.remove(activeNotifs, idx) end
            for i, f in ipairs(activeNotifs) do
                f.Position = UDim2.new(0.5,-125, baseY, -16 + (i-1)*38)
            end
        end)
    end
end

-- ================================================================
-- DAMAGE NOTIFICATIONS
-- ================================================================
local dmgNotifs = {}
local function showDmgNotif(victimName, damage)
    if not S.hitNotifOn then return end
    local txt = S.hitMsg ~= "" and S.hitMsg or string.format("%s — %d dmg", victimName, damage)
    local baseY = 0.65
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0,195,0,24)
    frame.Position = UDim2.new(0.5,-97,baseY,-12 + (#dmgNotifs)*28)
    frame.BackgroundColor3 = Color3.fromRGB(26,26,26)
    frame.BackgroundTransparency = 0.18
    frame.BorderSizePixel = 0
    frame.ZIndex = 10
    frame.Parent = NotifGui
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 6)
    local fs = Instance.new("UIStroke")
    fs.Color = Color3.fromRGB(85,85,85)
    fs.Thickness = 1
    fs.Transparency = 0.5
    fs.Parent = frame
    local lbl = Instance.new("TextLabel")
    lbl.Size = UDim2.new(1,-10,1,0)
    lbl.Position = UDim2.new(0,5,0,0)
    lbl.BackgroundTransparency = 1
    lbl.Text = txt
    lbl.TextColor3 = Color3.fromRGB(205,205,205)
    lbl.TextSize = 11
    lbl.Font = Enum.Font.GothamBold
    lbl.TextXAlignment = Enum.TextXAlignment.Center
    lbl.ZIndex = 10
    lbl.Parent = frame
    table.insert(dmgNotifs, frame)
    task.delay(3, function()
        TweenService:Create(frame, TweenInfo.new(0.5), {BackgroundTransparency = 1}):Play()
        TweenService:Create(lbl, TweenInfo.new(0.5), {TextTransparency = 1}):Play()
        task.wait(0.6)
        frame:Destroy()
        local idx = table.find(dmgNotifs, frame)
        if idx then table.remove(dmgNotifs, idx) end
        for i, f in ipairs(dmgNotifs) do
            f.Position = UDim2.new(0.5,-97,baseY,-12 + (i-1)*28)
        end
    end)
end

-- ================================================================
-- CHAMS
-- ================================================================
local function applyChams(p, char)
    if S.highlights[p] then S.highlights[p]:Destroy() end
    if not S.chamsEnabled then return end
    local root = char:FindFirstChild("HumanoidRootPart")
    if root and (root.Position - Camera.CFrame.Position).Magnitude > S.chamsMaxDist then return end
    local hl = Instance.new("Highlight")
    hl.FillColor = S.chamsFill
    hl.OutlineColor = S.chamsOutline
    hl.FillTransparency = S.chamsFillTr
    hl.OutlineTransparency = S.chamsOutTr
    hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
    hl.Adornee = char
    hl.Parent = char
    S.highlights[p] = hl
end
local function removeChams(p)
    if S.highlights[p] then S.highlights[p]:Destroy(); S.highlights[p] = nil end
end
local function refreshChams()
    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= LP then
            if S.chamsEnabled and p.Character then applyChams(p, p.Character)
            else removeChams(p) end
        end
    end
end

-- ================================================================
-- FLIGHT ENGINE
-- ================================================================
local flightConn, flightAtt, flightVel
local function startFlight()
    local char = LP.Character; if not char then return end
    local root = char:FindFirstChild("HumanoidRootPart"); if not root then return end
    local hum = char:FindFirstChildOfClass("Humanoid")
    if hum then hum.PlatformStand = true end
    if flightAtt then flightAtt:Destroy() end
    if flightVel then flightVel:Destroy() end
    flightAtt = Instance.new("Attachment"); flightAtt.Parent = root
    local ok, lv = pcall(function()
        local v = Instance.new("LinearVelocity")
        v.Attachment0 = flightAtt
        v.MaxForce = math.huge
        v.RelativeTo = Enum.ActuatorRelativeTo.World
        v.VectorVelocity = Vector3.zero
        v.Parent = root
        return v
    end)
    if ok then flightVel = lv
    else
        local bv = Instance.new("BodyVelocity")
        bv.MaxForce = Vector3.new(9e9,9e9,9e9)
        bv.P = 2e4
        bv.Velocity = Vector3.zero
        bv.Parent = root
        flightVel = bv
    end
    if flightConn then flightConn:Disconnect() end
    flightConn = RunService.RenderStepped:Connect(function()
        if not S.flightOn or not flightVel or not flightVel.Parent then return end
        local dir = Vector3.zero
        local cf = Camera.CFrame
        if UIS:IsKeyDown(Enum.KeyCode.W) then dir = dir + cf.LookVector end
        if UIS:IsKeyDown(Enum.KeyCode.S) then dir = dir - cf.LookVector end
        if UIS:IsKeyDown(Enum.KeyCode.A) then dir = dir - cf.RightVector end
        if UIS:IsKeyDown(Enum.KeyCode.D) then dir = dir + cf.RightVector end
        if UIS:IsKeyDown(Enum.KeyCode.Space) then dir = dir + Vector3.yAxis end
        if UIS:IsKeyDown(Enum.KeyCode.LeftControl) then dir = dir - Vector3.yAxis end
        local vel = dir.Magnitude > 0 and dir.Unit * S.flightSpeed or Vector3.zero
        if flightVel:IsA("LinearVelocity") then flightVel.VectorVelocity = vel
        else flightVel.Velocity = vel end
    end)
end
local function stopFlight()
    if flightConn then flightConn:Disconnect(); flightConn = nil end
    if flightAtt then flightAtt:Destroy(); flightAtt = nil end
    if flightVel then flightVel:Destroy(); flightVel = nil end
    local hum = LP.Character and LP.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.PlatformStand = false end
end

-- ================================================================
-- CFRAME MOVE ENGINE
-- ================================================================
local cfConn
local savedWalk = 16
local function startCF()
    if cfConn then cfConn:Disconnect() end
    cfConn = RunService.RenderStepped:Connect(function(dt)
        if not S.cfOn then return end
        local char = LP.Character; if not char then return end
        local root = char:FindFirstChild("HumanoidRootPart")
        local hum = char:FindFirstChildOfClass("Humanoid")
        if not root or not hum then return end
        if hum.WalkSpeed ~= 0 then savedWalk = hum.WalkSpeed end
        hum.WalkSpeed = 0
        local dir = Vector3.zero
        local cf = Camera.CFrame
        if UIS:IsKeyDown(Enum.KeyCode.W) then dir = dir + cf.LookVector end
        if UIS:IsKeyDown(Enum.KeyCode.S) then dir = dir - cf.LookVector end
        if UIS:IsKeyDown(Enum.KeyCode.A) then dir = dir - cf.RightVector end
        if UIS:IsKeyDown(Enum.KeyCode.D) then dir = dir + cf.RightVector end
        if UIS:IsKeyDown(Enum.KeyCode.Space) then dir = dir + Vector3.yAxis end
        if UIS:IsKeyDown(Enum.KeyCode.LeftControl) then dir = dir - Vector3.yAxis end
        if dir.Magnitude > 0 then
            root.CFrame = root.CFrame + dir.Unit * S.cfSpeed * math.min(dt, 0.05)
        end
    end)
end
local function stopCF()
    if cfConn then cfConn:Disconnect(); cfConn = nil end
    local hum = LP.Character and LP.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.WalkSpeed = savedWalk end
end

-- ================================================================
-- EXTEND SLIDE
-- ================================================================
local extendSlideConn, extendSlideStopTime
local function startExtendSlide()
    if extendSlideConn then extendSlideConn:Disconnect() end
    S.extendSlideActive = true
    extendSlideStopTime = tick() + S.extendSlideDuration
    extendSlideConn = RunService.RenderStepped:Connect(function(dt)
        if not S.extendSlideActive or tick() > extendSlideStopTime then
            S.extendSlideActive = false
            if extendSlideConn then extendSlideConn:Disconnect(); extendSlideConn = nil end
            return
        end
        local char = LP.Character; if not char then return end
        local root = char:FindFirstChild("HumanoidRootPart")
        if not root then return end
        local moveDir = Vector3.zero
        local cf = Camera.CFrame
        if UIS:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + cf.LookVector end
        if UIS:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - cf.LookVector end
        if UIS:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - cf.RightVector end
        if UIS:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + cf.RightVector end
        if moveDir.Magnitude > 0 then
            root.CFrame = root.CFrame + moveDir.Unit * S.extendSlideSpeed * math.min(dt, 0.05)
        end
    end)
end
UIS.InputBegan:Connect(function(input, gp)
    if gp then return end
    if input.KeyCode == S.extendSlideKey and S.extendSlideEnabled then
        startExtendSlide()
    end
end)

-- ================================================================
-- GLASS WALLS
-- ================================================================
local function isPlayerPart(part)
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= LP and plr.Character and part:IsDescendantOf(plr.Character) then return true end
    end
    return false
end
local function applyGlass()
    if not S.glassWalls then return end
    for _, part in ipairs(workspace:GetDescendants()) do
        if part:IsA("BasePart") and not isPlayerPart(part)
            and not S.glassCache[part] and part.Size.Magnitude >= 5 then
            S.glassCache[part] = {
                trans = part.Transparency, mat = part.Material, ref = part.Reflectance
            }
            part.Transparency = 0.65
            part.Material = Enum.Material.Glass
            part.Reflectance = 0.15
            part.Color = Color3.fromRGB(180,220,255)
        end
    end
end
local function restoreGlass()
    for part, orig in pairs(S.glassCache) do
        if part and part.Parent then
            part.Transparency = orig.trans
            part.Material = orig.mat
            part.Reflectance = orig.ref
        end
    end
    S.glassCache = {}
end

-- ================================================================
-- DARK TEXTURES
-- ================================================================
local function applyDarkTextures()
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") and not isPlayerPart(obj)
            and not S.darkTexCache[obj] then
            S.darkTexCache[obj] = {
                Color = obj.Color,
                Material = obj.Material,
                Reflectance = obj.Reflectance
            }
            obj.Color = Color3.fromRGB(22, 22, 22)
            obj.Material = Enum.Material.SmoothPlastic
            obj.Reflectance = 0
            for _, child in ipairs(obj:GetChildren()) do
                if child:IsA("Texture") or child:IsA("Decal") or child:IsA("SurfaceAppearance") then
                    if not S.darkTexCache[child] then
                        S.darkTexCache[child] = { Transparency = child.Transparency }
                    end
                    child.Transparency = 1
                end
            end
        end
    end
    if not S.darkTexCC then
        S.darkTexCC = Instance.new("ColorCorrectionEffect")
        S.darkTexCC.Brightness = -0.18
        S.darkTexCC.Contrast = 0.12
        S.darkTexCC.TintColor = Color3.fromRGB(60, 60, 75)
        S.darkTexCC.Parent = Lighting
    end
end
local function restoreDarkTextures()
    for obj, orig in pairs(S.darkTexCache) do
        if obj and obj.Parent then
            if obj:IsA("BasePart") then
                obj.Color = orig.Color
                obj.Material = orig.Material
                obj.Reflectance = orig.Reflectance
            else
                pcall(function() obj.Transparency = orig.Transparency end)
            end
        end
    end
    S.darkTexCache = {}
    if S.darkTexCC then S.darkTexCC:Destroy(); S.darkTexCC = nil end
end

-- ================================================================
-- WORLD TINT
-- ================================================================
local function updateTint()
    if S.tintEnabled then
        if not S.colorCorr then
            S.colorCorr = Instance.new("ColorCorrectionEffect")
            S.colorCorr.Parent = Lighting
        end
        S.colorCorr.TintColor = S.tintColor
        S.colorCorr.Brightness = S.tintBright
    else
        if S.colorCorr then S.colorCorr:Destroy(); S.colorCorr = nil end
    end
end

-- ================================================================
-- SKYBOX CHANGER
-- ================================================================
local SKYBOX_IDS = {
    ["Galaxy"] = "4389125610",
    ["Gridbox"] = "5024243214",
    ["Stormy Night"] = "1310975167",
    ["Blue Night"] = "4662930582",
    ["Purple Nebula"] = "17859485788",
    ["Cloudy Day"] = "102108763126711",
    ["Trippy"] = "9953029281",
    ["Purple Clouds"] = "11992952107",
    ["Chill Blue Night"] = "104721821743853"
}
local function setSkybox(id)
    for _, child in ipairs(Lighting:GetChildren()) do
        if child:IsA("Sky") then child:Destroy() end
    end
    if id and id ~= "" then
        local sky = Instance.new("Sky")
        sky.SkyboxBk = "rbxthumb://type=Asset&id="..id.."&w=420&h=420"
        sky.SkyboxDn = "rbxthumb://type=Asset&id="..id.."&w=420&h=420"
        sky.SkyboxFt = "rbxthumb://type=Asset&id="..id.."&w=420&h=420"
        sky.SkyboxLf = "rbxthumb://type=Asset&id="..id.."&w=420&h=420"
        sky.SkyboxRt = "rbxthumb://type=Asset&id="..id.."&w=420&h=420"
        sky.SkyboxUp = "rbxthumb://type=Asset&id="..id.."&w=420&h=420"
        sky.Parent = Lighting
        S.currentSky = sky
    else
        S.currentSky = nil
    end
end

-- ================================================================
-- SNOWFALL
-- ================================================================
local function startSnow()
    if S.snowPart then return end
    S.snowPart = Instance.new("Part")
    S.snowPart.Name = "SnowEmitterPart"
    S.snowPart.Size = Vector3.new(30, 20, 30)
    S.snowPart.Transparency = 1
    S.snowPart.CanCollide = false
    S.snowPart.Anchored = true
    S.snowPart.CastShadow = false
    S.snowPart.Parent = Camera
    RunService.RenderStepped:Connect(function()
        if S.snowPart and S.snowPart.Parent then
            S.snowPart.CFrame = Camera.CFrame * CFrame.new(0, 0, -15)
        end
    end)
    local em = Instance.new("ParticleEmitter")
    em.Parent = S.snowPart
    em.Texture = "rbxassetid://6070688741"
    em.Color = ColorSequence.new(Color3.fromRGB(255,255,255))
    em.LightEmission = 0.7
    em.Size = NumberSequence.new(0.15, 0.25)
    em.Transparency = NumberSequence.new(0.2)
    em.Lifetime = NumberRange.new(5, 8)
    em.Rate = 500
    em.RotSpeed = NumberRange.new(-20, 20)
    em.Orientation = Enum.ParticleOrientation.VelocityPerpendicular
    em.VelocityInheritance = 0
    em.SpreadAngle = Vector2.new(20, 20)
    em.Acceleration = Vector3.new(0, -2, 0)
    em.Drag = 0.8
    em.Speed = NumberRange.new(0.8, 1.5)
    em.Enabled = true
    em.Shape = Enum.ParticleEmitterShape.Cylinder
    em.ShapePartial = 0.8
    em.ShapeInOut = Enum.ParticleEmitterShapeInOut.Outward
end
local function stopSnow()
    if S.snowPart then S.snowPart:Destroy(); S.snowPart = nil end
end

-- ================================================================
-- MOTION BLUR
-- ================================================================
local motionBlurEffect = nil
local lastCamCF = Camera.CFrame
local function updateMotionBlur()
    if not S.motionBlurEnabled then
        if motionBlurEffect then motionBlurEffect:Destroy(); motionBlurEffect = nil end
        return
    end
    local delta = (Camera.CFrame.Position - lastCamCF.Position).Magnitude
    local angleDelta = (Camera.CFrame.LookVector - lastCamCF.LookVector).Magnitude
    local movement = delta + angleDelta * 10
    local blurAmt = math.min(movement * S.motionBlurStrength, 20)
    if blurAmt > 0.5 then
        if not motionBlurEffect then
            motionBlurEffect = Instance.new("BlurEffect")
            motionBlurEffect.Parent = Camera
        end
        motionBlurEffect.Size = blurAmt
    else
        if motionBlurEffect then motionBlurEffect:Destroy(); motionBlurEffect = nil end
    end
    lastCamCF = Camera.CFrame
end

-- ================================================================
-- CHAT SENDER
-- ================================================================
local function sendChat(msg)
    task.spawn(function()
        local ok = false
        local tcs = game:GetService("TextChatService")
        if tcs and tcs.TextChannels then
            local ch = tcs.TextChannels:FindFirstChild("General")
                    or tcs.TextChannels:FindFirstChild("RBXGeneral")
            if ch then pcall(function() ch:SendAsync(msg) end); ok = true end
        end
        if not ok then
            local rep = game:GetService("ReplicatedStorage"):FindFirstChild("DefaultChatSystemChatEvents")
            if rep then
                local sr = rep:FindFirstChild("SayMessageRequest")
                if sr then pcall(function() sr:FireServer(msg, "All") end) end
            end
        end
    end)
end

-- ================================================================
-- DEVICE SPOOFER
-- ================================================================
local function detectRealDevice()
    if UIS.TouchEnabled and not UIS.KeyboardEnabled then return "Touch" end
    if UIS.GamepadEnabled then return "Gamepad" end
    return "MouseKeyboard"
end
local function applyDeviceSpoof(wantedDevice)
    local yourDevice = detectRealDevice()
    local ok, remote = pcall(function()
        return game:GetService("ReplicatedStorage")
            :WaitForChild("Remotes", 5)
            :WaitForChild("Replication", 5)
            :WaitForChild("Fighter", 5)
            :WaitForChild("SetControls", 5)
    end)
    if not ok or not remote then showNotification("Spoof remote not found!"); return end
    remote:FireServer(yourDevice)
    task.wait(0.3)
    remote:FireServer(wantedDevice)
    showNotification("Device: " .. yourDevice .. " → " .. wantedDevice)
end

-- ================================================================
-- WIN STREAK SPOOFER
-- ================================================================
local winStreakConn = nil
local function startWinStreakSpoof()
    if winStreakConn then return end
    winStreakConn = RunService.Heartbeat:Connect(function()
        if not S.winStreakSpoof then return end
        local cls = LP:FindFirstChild("CustomLeaderstats")
        if not cls then return end
        local ws = cls:FindFirstChild("Win Streak")
        if ws then
            pcall(function() ws.Value = S.winStreakTarget end)
        end
        local flame = LP:FindFirstChild("Flame") or LP:FindFirstChild("WinStreakFlame")
        if flame and flame:IsA("NumberValue") then
            pcall(function() flame.Value = S.winStreakTarget end)
        end
    end)
end
local function stopWinStreakSpoof()
    if winStreakConn then winStreakConn:Disconnect(); winStreakConn = nil end
end

-- ================================================================
-- 3D WIREFRAME BOX ESP
-- ================================================================
local function createWireframeLine(name, adornTo, offset, size)
    local line = Instance.new("BoxHandleAdornment")
    line.Name = name
    line.Adornee = adornTo
    line.AlwaysOnTop = true
    line.ZIndex = 10
    line.Color3 = S.wireframeColor
    line.Transparency = 0
    line.Size = size
    line.CFrame = offset
    return line
end
local function applyWireframe(character)
    if not character or character == LP.Character then return end
    local root = character:WaitForChild("HumanoidRootPart", 10)
    if not root then return end
    if character:FindFirstChild("KittyWireframe") then character.KittyWireframe:Destroy() end
    local folder = Instance.new("Folder")
    folder.Name = "KittyWireframe"
    local BOX_SIZE = Vector3.new(4, 5.5, 2.5)
    local x, y, z = BOX_SIZE.X/2, BOX_SIZE.Y/2, BOX_SIZE.Z/2
    createWireframeLine("V1", root, CFrame.new(x, 0, z), Vector3.new(S.wireframeThickness, BOX_SIZE.Y, S.wireframeThickness)).Parent = folder
    createWireframeLine("V2", root, CFrame.new(-x, 0, z), Vector3.new(S.wireframeThickness, BOX_SIZE.Y, S.wireframeThickness)).Parent = folder
    createWireframeLine("V3", root, CFrame.new(x, 0, -z), Vector3.new(S.wireframeThickness, BOX_SIZE.Y, S.wireframeThickness)).Parent = folder
    createWireframeLine("V4", root, CFrame.new(-x, 0, -z), Vector3.new(S.wireframeThickness, BOX_SIZE.Y, S.wireframeThickness)).Parent = folder
    createWireframeLine("TH1", root, CFrame.new(0, y, z), Vector3.new(BOX_SIZE.X, S.wireframeThickness, S.wireframeThickness)).Parent = folder
    createWireframeLine("TH2", root, CFrame.new(0, y, -z), Vector3.new(BOX_SIZE.X, S.wireframeThickness, S.wireframeThickness)).Parent = folder
    createWireframeLine("TV1", root, CFrame.new(x, y, 0), Vector3.new(S.wireframeThickness, S.wireframeThickness, BOX_SIZE.Z)).Parent = folder
    createWireframeLine("TV2", root, CFrame.new(-x, y, 0), Vector3.new(S.wireframeThickness, S.wireframeThickness, BOX_SIZE.Z)).Parent = folder
    createWireframeLine("BH1", root, CFrame.new(0, -y, z), Vector3.new(BOX_SIZE.X, S.wireframeThickness, S.wireframeThickness)).Parent = folder
    createWireframeLine("BH2", root, CFrame.new(0, -y, -z), Vector3.new(BOX_SIZE.X, S.wireframeThickness, S.wireframeThickness)).Parent = folder
    createWireframeLine("BV1", root, CFrame.new(x, -y, 0), Vector3.new(S.wireframeThickness, S.wireframeThickness, BOX_SIZE.Z)).Parent = folder
    createWireframeLine("BV2", root, CFrame.new(-x, -y, 0), Vector3.new(S.wireframeThickness, S.wireframeThickness, BOX_SIZE.Z)).Parent = folder
    folder.Parent = character
    S.wireframeCache[character] = folder
end
local function removeWireframe(character)
    if S.wireframeCache[character] then
        S.wireframeCache[character]:Destroy()
        S.wireframeCache[character] = nil
    end
end
local function refreshWireframes()
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= LP then
            if S.wireframeESP and plr.Character then
                applyWireframe(plr.Character)
            else
                removeWireframe(plr.Character)
            end
        end
    end
end

-- ================================================================
-- GUN WRAP ENGINE
-- ================================================================
local ARM_KEYWORDS = {"arm","hand","glove","sleeve","finger","wrist","shoulder","elbow","forearm","upperarm","lowerarm","leftarm","rightarm","lefthand","righthand"}
local function isArmPart(part)
    local name = part.Name:lower()
    local parentName = (part.Parent and part.Parent.Name or ""):lower()
    local gpName = (part.Parent and part.Parent.Parent and part.Parent.Parent.Name or ""):lower()
    for _, kw in ipairs(ARM_KEYWORDS) do
        if name:find(kw) or parentName:find(kw) or gpName:find(kw) then
            return true
        end
    end
    return false
end

local WRAP_PRESETS = {
    {name="Glass (Static)", material=Enum.Material.Glass, color=Color3.fromRGB(255,255,255), transparency=0.65, reflectance=1.0},
    {name="Galaxy (Glassy)", textureId="rbxthumb://type=Asset&id=508197089&w=420&h=420", material=Enum.Material.Glass, color=Color3.fromRGB(220,220,255), transparency=0.5, reflectance=0.8, tileScale=1.5, anim="parallax"},
    {name="Aurora", textureId="rbxthumb://type=Asset&id=783434863&w=420&h=420", material=Enum.Material.Glass, color=Color3.fromRGB(180,255,230), transparency=0.4, reflectance=0.9, tileScale=1.5, anim="parallax"},
    {name="Hologram (Ripple)", textureId="rbxthumb://type=Asset&id=347835105&w=420&h=420", material=Enum.Material.Glass, color=Color3.fromRGB(200,220,255), transparency=0.55, reflectance=0.7, tileScale=1.2, texTransparency=0.2, anim="holo_ripple"},
    {name="Money (Hologram)", textureId="rbxthumb://type=Asset&id=181036521&w=420&h=420", material=Enum.Material.SmoothPlastic, color=Color3.fromRGB(255,255,255), transparency=0, reflectance=0.1, tileScale=0.8, anim="parallax"},
    {name="5box (Gliding)", textureId="rbxthumb://type=Asset&id=102204427772169&w=420&h=420", material=Enum.Material.SmoothPlastic, color=Color3.fromRGB(255,255,255), transparency=0, reflectance=0.1, tileScale=4.5, anim="glide"},
    {name="Glitch (Heavy)", textureId="rbxthumb://type=Asset&id=3128134660&w=420&h=420", material=Enum.Material.Neon, color=Color3.fromRGB(255,255,255), transparency=0, reflectance=0, tileScale=1.0, anim="glitch_heavy"},
    {name="LV (Glitch)", textureId="rbxthumb://type=Asset&id=4874897174&w=420&h=420", material=Enum.Material.SmoothPlastic, color=Color3.fromRGB(255,255,255), transparency=0, reflectance=0.2, tileScale=1.5, anim="glitch"},
    {name="Eyes (Twitch)", textureId="rbxthumb://type=Asset&id=7544390558&w=420&h=420", material=Enum.Material.SmoothPlastic, color=Color3.fromRGB(0,0,0), transparency=0, reflectance=0.3, tileScale=0.3, anim="twitch"},
    {name="Chrome Hearts", textureId="rbxthumb://type=Asset&id=135025152418303&w=420&h=420", material=Enum.Material.Metal, color=Color3.fromRGB(255,255,255), transparency=0, reflectance=1.0, tileScale=1.0, anim="twinkle"},
    {name="1B VISITS", textureId="rbxthumb://type=Asset&id=89679182570410&w=420&h=420", material=Enum.Material.Metal, color=Color3.fromRGB(255,255,255), transparency=0, reflectance=0.6, tileScale=1.0, anim="scroll_slow"},
    {name="Hello Kitty", textureId="rbxthumb://type=Asset&id=74319666661779&w=420&h=420", material=Enum.Material.SmoothPlastic, color=Color3.fromRGB(255,255,255), transparency=0, reflectance=0.1, tileScale=1.0, anim="float"},
    {name="Scribble (Jitter)", textureId="rbxthumb://type=Asset&id=13299542970&w=420&h=420", material=Enum.Material.Metal, color=Color3.fromRGB(255,255,255), transparency=0, reflectance=0.8, tileScale=1.0, anim="jitter"},
    {name="Shattered Glass", textureId="rbxthumb://type=Asset&id=9817982402&w=420&h=420", material=Enum.Material.Glass, color=Color3.fromRGB(240,240,240), transparency=0.5, reflectance=1.0, tileScale=1.1, anim="fracture"},
    {name="Arena (Blink)", textureId="rbxthumb://type=Asset&id=18931785052&w=420&h=420", material=Enum.Material.SmoothPlastic, color=Color3.fromRGB(255,255,255), transparency=0, reflectance=0.3, tileScale=0.85, anim="blink"},
    {name="Iron Ore", textureId="rbxthumb://type=Asset&id=3786068482&w=420&h=420", material=Enum.Material.Slate, color=Color3.fromRGB(150,150,150), transparency=0, reflectance=0.1, tileScale=1.2, anim="float"},
    {name="Dirt", textureId="rbxthumb://type=Asset&id=3633730375&w=420&h=420", material=Enum.Material.Slate, color=Color3.fromRGB(120,100,80), transparency=0, reflectance=0},
    {name="No Texture", textureId="rbxthumb://type=Asset&id=9094483852&w=420&h=420", material=Enum.Material.SmoothPlastic, color=Color3.fromRGB(255,255,255), transparency=0, reflectance=0.1, tileScale=1.0, anim="glitch_slow"},
    {name="Nether Portal", textureId="rbxthumb://type=Asset&id=9228727987&w=420&h=420", material=Enum.Material.Glass, color=Color3.fromRGB(200,100,255), transparency=0.4, reflectance=0.9, tileScale=1.5, anim="swirl"},
    {name="Carbon V2", textureId="rbxthumb://type=Asset&id=113316355960656&w=420&h=420", material=Enum.Material.Metal, color=Color3.fromRGB(255,255,255), transparency=0, reflectance=1.0, tileScale=1.2, anim="float"},
    {name="Bones", textureId="rbxthumb://type=Asset&id=84599836229719&w=420&h=420", material=Enum.Material.SmoothPlastic, color=Color3.fromRGB(0,0,0), transparency=0, reflectance=0.2, tileScale=0.5, anim="float"},
    {name="Magma", textureId="rbxthumb://type=Asset&id=2861750&w=420&h=420", material=Enum.Material.Neon, color=Color3.fromRGB(255,80,0), transparency=0.1, reflectance=0.6, tileScale=1.5, anim="magma"}
}
local WRAP_PRESET_MAP = {}
for _, w in ipairs(WRAP_PRESETS) do WRAP_PRESET_MAP[w.name] = w end
local wrapActiveTextures = {}
local wrapActiveParts = {}
local currentWrap = nil
local wrapFrameCounter = 0
local WRAP_ENFORCE_EVERY = 2
local WRAP_EXCLUDE = {"charm","attachment","accessory","decal","sticker"}
local function wrapIsExcluded(part)
    local name = part.Name:lower()
    for _, kw in ipairs(WRAP_EXCLUDE) do if name:find(kw) then return true end end
    return false
end
local function applyWrapToPart(part, settings)
    if not part:IsA("BasePart") then return end
    if part.Transparency >= 0.9 then return end
    if wrapIsExcluded(part) then return end
    if isArmPart(part) then return end
    for _, child in ipairs(part:GetChildren()) do
        if child:IsA("Texture") or child:IsA("Decal") or child:IsA("SurfaceAppearance") then
            child:Destroy()
        end
    end
    wrapActiveParts[part] = true
    part.Material = settings.material
    part.Color = settings.color
    part.Transparency = settings.transparency
    part.Reflectance = settings.reflectance
    if settings.textureId then
        local id = settings.textureId
        for _, face in ipairs(Enum.NormalId:GetEnumItems()) do
            local tex = Instance.new("Texture", part)
            tex.Name = "KittyWrapTex"
            tex.Texture = id
            tex.Face = face
            tex.StudsPerTileU = settings.tileScale or 1
            tex.StudsPerTileV = settings.tileScale or 1
            tex.Transparency = settings.texTransparency or 0
            if settings.anim then
                wrapActiveTextures[tex] = settings.anim
            end
        end
    end
end
local function wrapScanModel(model, settings)
    for _, p in ipairs(model:GetDescendants()) do
        applyWrapToPart(p, settings)
    end
end
local function wrapApplyAll()
    if not S.wrapEnabled or not currentWrap then return end
    wrapActiveTextures = {}
    wrapActiveParts = {}
    local vmFolder = workspace:FindFirstChild("ViewModels")
    if vmFolder then
        for _, m in ipairs(vmFolder:GetChildren()) do
            wrapScanModel(m, currentWrap)
        end
    end
    for _, m in ipairs(Camera:GetChildren()) do
        if m:IsA("Model") then wrapScanModel(m, currentWrap) end
    end
end
local function onToolEquipped(tool)
    if not S.wrapEnabled or not currentWrap then return end
    task.wait(0.1)
    wrapApplyAll()
end
if LP.Character then
    LP.Character.ChildAdded:Connect(function(c)
        if c:IsA("Tool") then onToolEquipped(c) end
    end)
end
LP.CharacterAdded:Connect(function(char)
    char.ChildAdded:Connect(function(c)
        if c:IsA("Tool") then onToolEquipped(c) end
    end)
end)

-- ================================================================
-- COMBINED RENDER LOOP FOR TEXTURE ANIMATIONS
-- ================================================================
RunService.RenderStepped:Connect(function()
    local t = tick()
    local pitch, yaw = Camera.CFrame:ToOrientation()
    if S.wrapEnabled and currentWrap then
        wrapFrameCounter = wrapFrameCounter + 1
        if wrapFrameCounter % WRAP_ENFORCE_EVERY == 0 then
            for part in pairs(wrapActiveParts) do
                if not part or not part.Parent then
                    wrapActiveParts[part] = nil
                    continue
                end
                if part.Material ~= currentWrap.material then
                    part.Material = currentWrap.material
                end
                if part.Color ~= currentWrap.color then
                    part.Color = currentWrap.color
                end
                if part.Transparency ~= currentWrap.transparency then
                    part.Transparency = currentWrap.transparency
                end
                if part.Reflectance ~= currentWrap.reflectance then
                    part.Reflectance = currentWrap.reflectance
                end
                if part:IsA("MeshPart") and part.TextureID ~= "" then
                    part.TextureID = ""
                end
            end
        end
        if currentWrap.anim then
            for tex, mode in pairs(wrapActiveTextures) do
                if not tex or not tex.Parent then
                    wrapActiveTextures[tex] = nil
                    continue
                end
                if mode == "parallax" then
                    tex.OffsetStudsU = yaw * 1.0
                    tex.OffsetStudsV = pitch * 1.0
                elseif mode == "holo_ripple" then
                    tex.OffsetStudsU = (yaw * 1.0) + (math.sin(t * 2.5) * 0.15)
                    tex.OffsetStudsV = (pitch * 1.0) + (math.cos(t * 2.5) * 0.15)
                    tex.Transparency = (currentWrap.texTransparency or 0) + (math.sin(t * 6) * 0.2)
                elseif mode == "glide" then
                    tex.OffsetStudsU = (t * 0.03) % 10
                    tex.OffsetStudsV = math.sin(t * 0.5) * 0.05
                elseif mode == "glitch" then
                    if math.random() > 0.85 then
                        tex.OffsetStudsU = math.random(-50,50) * 0.01
                        tex.Transparency = (currentWrap.texTransparency or 0) + (math.random(0,2) * 0.1)
                    else
                        tex.Transparency = currentWrap.texTransparency or 0
                    end
                elseif mode == "glitch_heavy" then
                    if math.random() > 0.7 then
                        tex.OffsetStudsU = math.random(-100,100) * 0.01
                        tex.OffsetStudsV = math.random(-100,100) * 0.01
                        tex.StudsPerTileU = (currentWrap.tileScale or 1) + (math.random(-3,3) * 0.1)
                    end
                elseif mode == "twinkle" then
                    tex.OffsetStudsU = math.sin(t * 0.3) * 0.15
                    tex.Transparency = 0.1 + (math.sin(t * 3) * 0.25)
                elseif mode == "twitch" then
                    if math.random() > 0.94 then
                        tex.OffsetStudsU = math.random(-30,30) * 0.01
                        tex.OffsetStudsV = math.random(-30,30) * 0.01
                    end
                elseif mode == "scroll_slow" then
                    tex.OffsetStudsU = (t * 0.1) % 10
                elseif mode == "float" then
                    tex.OffsetStudsU = (t * 0.05) % 10
                    tex.OffsetStudsV = (t * 0.05) % 10
                elseif mode == "jitter" then
                    if math.random() > 0.9 then
                        tex.OffsetStudsU = math.random(-5,5) * 0.01
                        tex.OffsetStudsV = math.random(-5,5) * 0.01
                    end
                elseif mode == "fracture" then
                    if math.random() > 0.96 then
                        tex.OffsetStudsU = math.random(-2,2) * 0.1
                        tex.OffsetStudsV = math.random(-2,2) * 0.1
                    end
                elseif mode == "blink" then
                    tex.Transparency = (currentWrap.texTransparency or 0) + (math.sin(t * 8) * 0.3)
                elseif mode == "glitch_slow" then
                    if math.random() > 0.985 then
                        tex.OffsetStudsU = math.random(-8,8) * 0.01
                        tex.OffsetStudsV = math.random(-8,8) * 0.01
                    end
                elseif mode == "swirl" then
                    tex.OffsetStudsU = (t * 0.04) % 10 + math.sin(t * 1.0) * 0.15
                    tex.OffsetStudsV = (t * 0.06) % 10 + math.cos(t * 1.2) * 0.15
                elseif mode == "magma" then
                    tex.OffsetStudsU = (t * 0.02) % 10
                    tex.OffsetStudsV = (t * 0.03) % 10 + math.sin(t * 0.5) * 0.1
                    tex.Transparency = 0.1 + (math.sin(t * 2) * 0.05)
                end
            end
        end
    end
end)
Camera.DescendantAdded:Connect(function(desc)
    if not S.wrapEnabled or not currentWrap then return end
    task.defer(function()
        if not desc or not desc.Parent then return end
        if desc:IsA("BasePart") then
            applyWrapToPart(desc, currentWrap)
        end
    end)
end)
local vmFolderRef = workspace:FindFirstChild("ViewModels")
if vmFolderRef then
    vmFolderRef.ChildAdded:Connect(function(child)
        if not S.wrapEnabled or not currentWrap then return end
        task.wait(0.06)
        wrapScanModel(child, currentWrap)
    end)
    vmFolderRef.DescendantAdded:Connect(function(desc)
        if not S.wrapEnabled or not currentWrap then return end
        task.defer(function()
            if not desc or not desc.Parent then return end
            if desc:IsA("BasePart") then
                applyWrapToPart(desc, currentWrap)
            end
        end)
    end)
end

-- ================================================================
-- ESP POOL
-- ================================================================
local espPool = {}
local function makePool()
    return {
        box = {
            newLine(S.boxThickness, Color3.fromRGB(0,0,0)),
            newLine(S.boxThickness, Color3.fromRGB(0,0,0)),
            newLine(S.boxThickness, Color3.fromRGB(0,0,0)),
            newLine(S.boxThickness, Color3.fromRGB(0,0,0))
        },
        cornerLines = {
            newLine(S.cornerThickness, Color3.fromRGB(0,0,0)),
            newLine(S.cornerThickness, Color3.fromRGB(0,0,0)),
            newLine(S.cornerThickness, Color3.fromRGB(0,0,0)),
            newLine(S.cornerThickness, Color3.fromRGB(0,0,0)),
            newLine(S.cornerThickness, Color3.fromRGB(0,0,0)),
            newLine(S.cornerThickness, Color3.fromRGB(0,0,0)),
            newLine(S.cornerThickness, Color3.fromRGB(0,0,0)),
            newLine(S.cornerThickness, Color3.fromRGB(0,0,0))
        },
        hpBg = newLine(3, Color3.fromRGB(18,18,18)),
        hpFg = newLine(3, Color3.fromRGB(75,215,75)),
        hpCap = newCirc(2, true, Color3.fromRGB(75,215,75)),
        snap = newLine(1, Color3.fromRGB(0,0,0)),
        nameLbl = newText(S.nameFontSize, Color3.new(1,1,1)),
        distLbl = newText(11, Color3.fromRGB(195,180,255)),
        gunLbl = newText(11, Color3.fromRGB(255,220,100))
    }
end
local function getPool(p)
    if not espPool[p] then espPool[p] = makePool() end
    return espPool[p]
end
local function hidePool(o)
    for _, l in ipairs(o.box) do l.Visible = false end
    for _, l in ipairs(o.cornerLines) do l.Visible = false end
    o.hpBg.Visible = false
    o.hpFg.Visible = false
    o.hpCap.Visible = false
    o.snap.Visible = false
    o.nameLbl.Visible = false
    o.distLbl.Visible = false
    o.gunLbl.Visible = false
end
local function destroyPool(p)
    if not espPool[p] then return end
    local o = espPool[p]
    for _, l in ipairs(o.box) do l:Remove() end
    for _, l in ipairs(o.cornerLines) do l:Remove() end
    o.hpBg:Remove(); o.hpFg:Remove(); o.hpCap:Remove()
    o.snap:Remove(); o.nameLbl:Remove(); o.distLbl:Remove(); o.gunLbl:Remove()
    espPool[p] = nil
end

-- ================================================================
-- SKELETON ESP
-- ================================================================
local SKEL_R15 = {
    {"Head","UpperTorso"},
    {"UpperTorso","LowerTorso"},
    {"UpperTorso","LeftUpperArm"},
    {"LeftUpperArm","LeftLowerArm"},
    {"LeftLowerArm","LeftHand"},
    {"UpperTorso","RightUpperArm"},
    {"RightUpperArm","RightLowerArm"},
    {"RightLowerArm","RightHand"},
    {"LowerTorso","LeftUpperLeg"},
    {"LeftUpperLeg","LeftLowerLeg"},
    {"LeftLowerLeg","LeftFoot"},
    {"LowerTorso","RightUpperLeg"},
    {"RightUpperLeg","RightLowerLeg"},
    {"RightLowerLeg","RightFoot"}
}
local SKEL_R6 = {
    {"Head","Torso"},
    {"Torso","Left Arm"},
    {"Torso","Right Arm"},
    {"Torso","Left Leg"},
    {"Torso","Right Leg"}
}
local skelPool = {}
local SKEL_LINE_COUNT = 14
local function getSkelPool(p)
    if not skelPool[p] then
        local lines = {}
        for i = 1, SKEL_LINE_COUNT do lines[i] = newLine(S.skelThickness, Color3.fromRGB(255,255,255)) end
        skelPool[p] = lines
    end
    return skelPool[p]
end
local function hideSkel(p)
    if skelPool[p] then for _, l in ipairs(skelPool[p]) do l.Visible = false end end
end
local function destroySkel(p)
    if skelPool[p] then for _, l in ipairs(skelPool[p]) do l:Remove() end; skelPool[p] = nil end
end
local function renderSkeleton(plr, char)
    local lines = getSkelPool(plr)
    for _, l in ipairs(lines) do l.Visible = false; l.Thickness = S.skelThickness end
    local isR15 = char:FindFirstChild("UpperTorso") ~= nil
    local bones = isR15 and SKEL_R15 or SKEL_R6
    local idx = 0
    for _, pair in ipairs(bones) do
        local partA = char:FindFirstChild(pair[1])
        local partB = char:FindFirstChild(pair[2])
        if partA and partB then
            local spA, onA = Camera:WorldToViewportPoint(partA.Position)
            local spB, onB = Camera:WorldToViewportPoint(partB.Position)
            if onA and onB and spA.Z > 0 and spB.Z > 0 then
                idx = idx + 1
                if idx > SKEL_LINE_COUNT then break end
                local l = lines[idx]
                l.From = Vector2.new(spA.X, spA.Y)
                l.To = Vector2.new(spB.X, spB.Y)
                l.Color = S.skelColor
                l.Visible = true
            end
        end
    end
end

-- ================================================================
-- CIRCULAR PIECE
-- ================================================================
local function getCircPool(p)
    if not S.circDrawings[p] then
        local sh = Drawing.new("Circle")
        sh.Radius = 18; sh.Filled = true
        sh.Color = Color3.fromRGB(10,10,10)
        sh.Transparency = 0.55; sh.NumSides = 48; sh.Visible = false
        local inn = Drawing.new("Circle")
        inn.Radius = 11; inn.Filled = true
        inn.Color = Color3.fromRGB(20,20,20)
        inn.Transparency = 0.25; inn.NumSides = 48; inn.Visible = false
        S.circDrawings[p] = {shadow = sh, inner = inn}
    end
    return S.circDrawings[p]
end
local function hideCirc(p)
    if S.circDrawings[p] then
        S.circDrawings[p].shadow.Visible = false
        S.circDrawings[p].inner.Visible = false
    end
end
local function destroyCirc(p)
    if S.circDrawings[p] then
        S.circDrawings[p].shadow:Remove()
        S.circDrawings[p].inner:Remove()
        S.circDrawings[p] = nil
    end
end

-- ================================================================
-- FOV CIRCLES
-- ================================================================
local fovCircle = newCirc(100, false, Color3.fromRGB(255,255,255))
fovCircle.Thickness = 1; fovCircle.NumSides = 64; fovCircle.Visible = false
local fovFillCircle = newCirc(100, true, Color3.fromRGB(255,182,193))
fovFillCircle.NumSides = 64; fovFillCircle.Transparency = 0.21; fovFillCircle.Visible = false

-- ================================================================
-- WATERMARK
-- ================================================================
local watermarkDrawing = newText(12, Color3.fromRGB(255,255,255))
watermarkDrawing.Text = "KittyHub V2 | discord.gg/cTtSuya3Rb"
watermarkDrawing.Outline = true
watermarkDrawing.OutlineColor = Color3.new(0,0,0)
watermarkDrawing.Visible = false

-- ================================================================
-- HITMARKER
-- ================================================================
local activeHitmarkers = {}
local function createHitmarker(pos)
    local mk = {startTime = tick(), position = pos, lines = {}}
    mk.lines[1] = newLine(2.5, Color3.fromRGB(255,182,193))
    mk.lines[2] = newLine(2.5, Color3.fromRGB(255,182,193))
    mk.lines[1].Visible = true
    mk.lines[2].Visible = true
    table.insert(activeHitmarkers, mk)
end
RunService.RenderStepped:Connect(function()
    local toRm = {}
    local now = tick()
    for i, mk in ipairs(activeHitmarkers) do
        local age = now - mk.startTime
        if age > 0.4 then
            for _, l in ipairs(mk.lines) do l:Remove() end
            table.insert(toRm, i)
        else
            local pos = mk.position
            local sz = 6 + age * 3
            local al = 1 - (age / 0.4)
            mk.lines[1].From = Vector2.new(pos.X - sz, pos.Y - sz)
            mk.lines[1].To = Vector2.new(pos.X + sz, pos.Y + sz)
            mk.lines[2].From = Vector2.new(pos.X + sz, pos.Y - sz)
            mk.lines[2].To = Vector2.new(pos.X - sz, pos.Y + sz)
            for _, l in ipairs(mk.lines) do l.Transparency = 1 - al end
        end
    end
    for i = #toRm, 1, -1 do table.remove(activeHitmarkers, toRm[i]) end
end)

-- ================================================================
-- CROSSHAIR
-- ================================================================
local crossDot, crossH, crossV
local function updateCrosshair()
    if not S.crossEnabled then
        if crossDot then crossDot.Visible = false end
        if crossH then crossH.Visible = false end
        if crossV then crossV.Visible = false end
        watermarkDrawing.Visible = false
        return
    end
    local ctr = UIS:GetMouseLocation()
    if S.crossType == "dot" then
        if not crossDot then
            crossDot = Drawing.new("Circle")
            crossDot.Thickness = 0
            crossDot.Filled = true
        end
        crossDot.Visible = true
        crossDot.Radius = S.crossSize / 2
        crossDot.Color = S.crossColor
        crossDot.Position = ctr
        if crossH then crossH.Visible = false end
        if crossV then crossV.Visible = false end
    elseif S.crossType == "cross" then
        if not crossH then crossH = Drawing.new("Line") end
        if not crossV then crossV = Drawing.new("Line") end
        crossH.Visible = true; crossV.Visible = true
        crossH.Color = S.crossColor; crossV.Color = S.crossColor
        crossH.Thickness = S.crossThick; crossV.Thickness = S.crossThick
        if S.crossSpin then
            local rad = math.rad(S.crossAngle)
            local c_, s_ = math.cos(rad), math.sin(rad)
            local len = S.crossSize / 2
            crossH.From = ctr + Vector2.new(-len * c_, -len * s_)
            crossH.To = ctr + Vector2.new( len * c_, len * s_)
            crossV.From = ctr + Vector2.new( len * s_, -len * c_)
            crossV.To = ctr + Vector2.new(-len * s_, len * c_)
        else
            crossH.From = Vector2.new(ctr.X - S.crossSize/2, ctr.Y)
            crossH.To = Vector2.new(ctr.X + S.crossSize/2, ctr.Y)
            crossV.From = Vector2.new(ctr.X, ctr.Y - S.crossSize/2)
            crossV.To = Vector2.new(ctr.X, ctr.Y + S.crossSize/2)
        end
        if crossDot then crossDot.Visible = false end
    elseif S.crossType == "T-Shape" then
        if not crossH then crossH = Drawing.new("Line") end
        if not crossV then crossV = Drawing.new("Line") end
        crossH.Visible = true; crossV.Visible = true
        crossH.Color = S.crossColor; crossV.Color = S.crossColor
        crossH.Thickness = S.crossThick; crossV.Thickness = S.crossThick
        crossH.From = Vector2.new(ctr.X - S.crossSize/2, ctr.Y)
        crossH.To = Vector2.new(ctr.X + S.crossSize/2, ctr.Y)
        crossV.From = Vector2.new(ctr.X, ctr.Y)
        crossV.To = Vector2.new(ctr.X, ctr.Y + S.crossSize/2)
        if crossDot then crossDot.Visible = false end
    elseif S.crossType == "Inverted T-Shape" then
        if not crossH then crossH = Drawing.new("Line") end
        if not crossV then crossV = Drawing.new("Line") end
        crossH.Visible = true; crossV.Visible = true
        crossH.Color = S.crossColor; crossV.Color = S.crossColor
        crossH.Thickness = S.crossThick; crossV.Thickness = S.crossThick
        crossH.From = Vector2.new(ctr.X - S.crossSize/2, ctr.Y)
        crossH.To = Vector2.new(ctr.X + S.crossSize/2, ctr.Y)
        crossV.From = Vector2.new(ctr.X, ctr.Y)
        crossV.To = Vector2.new(ctr.X, ctr.Y - S.crossSize/2)
        if crossDot then crossDot.Visible = false end
    end
    if S.watermarkOn then
        watermarkDrawing.Visible = true
        local tw = watermarkDrawing.TextBounds.X
        watermarkDrawing.Position = Vector2.new(ctr.X - tw/2, ctr.Y + S.crossSize/2 + 8)
    else
        watermarkDrawing.Visible = false
    end
end
RunService.RenderStepped:Connect(function(dt)
    if S.crossSpin and S.crossEnabled and S.crossType == "cross" then
        S.crossAngle = S.crossAngle + S.crossSpinSpd * dt * 100
        updateCrosshair()
    end
    updateMotionBlur()
end)
UIS.InputChanged:Connect(updateCrosshair)

-- ================================================================
-- FAST BOUNDS HELPER
-- ================================================================
local function getFastBounds(char)
    local head = char:FindFirstChild("Head")
    local root = char:FindFirstChild("HumanoidRootPart")
    if not head or not root then return nil end
    local topPos = head.Position + Vector3.new(0, head.Size.Y * 0.6, 0)
    local botPos = root.Position - Vector3.new(0, 3, 0)
    local tsc, ton = Camera:WorldToViewportPoint(topPos)
    local bsc, bon = Camera:WorldToViewportPoint(botPos)
    if not ton or not bon or tsc.Z <= 0 or bsc.Z <= 0 then return nil end
    local h = math.abs(tsc.Y - bsc.Y)
    local w = h * S.boxWidth
    local cx = (tsc.X + bsc.X) / 2
    return cx - w/2, math.min(tsc.Y, bsc.Y), cx + w/2, math.max(tsc.Y, bsc.Y), cx
end

-- ================================================================
-- NEAREST ENEMY / BEST TARGET
-- ================================================================
local function getNearestEnemy()
    local best, bestD = nil, math.huge
    local myRoot = LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
    if not myRoot then return nil end
    for _, p in ipairs(Players:GetPlayers()) do
        if p == LP then continue end
        local char = p.Character; if not char then continue end
        local hum = char:FindFirstChildOfClass("Humanoid")
        if not hum or hum.Health <= 0 then continue end
        local root = char:FindFirstChild("HumanoidRootPart"); if not root then continue end
        local d = (root.Position - myRoot.Position).Magnitude
        if d < bestD then bestD = d; best = p end
    end
    return best
end
local function findBestTarget()
    local center = (S.fovMode == "Mouse")
        and UIS:GetMouseLocation()
        or Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
    local best, bestD = nil, S.fovRadius + 1
    for _, p in ipairs(Players:GetPlayers()) do
        if p == LP then continue end
        local char = p.Character; if not char then continue end
        local hum = char:FindFirstChildOfClass("Humanoid")
        if not hum or hum.Health <= 0 then continue end
        local part = char:FindFirstChild(S.aimbotPart) or char:FindFirstChild("HumanoidRootPart")
        if not part then continue end
        if S.visCheck then
            local ray = Ray.new(Camera.CFrame.Position,
                (part.Position - Camera.CFrame.Position).Unit * 2000)
            local hit = workspace:FindPartOnRay(ray, LP.Character)
            if not (hit == part or (hit and hit:IsDescendantOf(part.Parent))) then continue end
        end
        local root = char:FindFirstChild("HumanoidRootPart")
        if not root or (root.Position - Camera.CFrame.Position).Magnitude > S.aimbotDist then continue end
        local targetPos = part.Position
        if S.aimPrediction > 0 then
            local velocity = root.AssemblyLinearVelocity or Vector3.zero
            targetPos = targetPos + velocity * S.aimPrediction * 0.1
        end
        local sp, on = Camera:WorldToViewportPoint(targetPos)
        if not on or sp.Z <= 0 then continue end
        local d = (Vector2.new(sp.X, sp.Y) - center).Magnitude
        if d < S.fovRadius and d < bestD then bestD = d; best = p end
    end
    return best
end

-- ================================================================
-- HOOK PLAYERS — health tracking, hit/kill sounds
-- ================================================================
local function hookPlayer(plr)
    if plr == LP then return end
    local function hookChar(char)
        local hum = char:WaitForChild("Humanoid", 5); if not hum then return end
        local lastHP = hum.Health
        hum.HealthChanged:Connect(function(hp)
            local dmg = lastHP - hp
            if dmg > 0 then
                showDmgNotif(plr.Name, math.floor(dmg))
                if S.hitSoundOn then
                    local snd = Instance.new("Sound")
                    snd.SoundId = S.hitSoundId
                    snd.Volume = S.hitSoundVol
                    snd.RollOffMinDistance = 0
                    snd.RollOffMaxDistance = 1000
                    snd.RollOffMode = Enum.RollOffMode.Linear
                    snd.Parent = workspace
                    snd:Play()
                    game:GetService("Debris"):AddItem(snd, 5)
                    local root = char:FindFirstChild("HumanoidRootPart")
                    if root then
                        local sp, on = Camera:WorldToViewportPoint(root.Position)
                        if on then createHitmarker(Vector2.new(sp.X, sp.Y)) end
                    end
                end
            end
            if hp <= 0 and lastHP > 0 then
                if S.killSoundOn then
                    local snd = Instance.new("Sound")
                    snd.SoundId = S.killSoundId
                    snd.Volume = S.killSoundVol
                    snd.RollOffMinDistance = 0
                    snd.RollOffMaxDistance = 1000
                    snd.RollOffMode = Enum.RollOffMode.Linear
                    snd.Parent = workspace
                    snd:Play()
                    snd.Ended:Connect(function() snd:Destroy() end)
                    game:GetService("Debris"):AddItem(snd, 10)
                end
                if S.killChatSpam then
                    sendChat(S.killChatMsgs[math.random(1, #S.killChatMsgs)])
                end
            end
            lastHP = hp
        end)
    end
    if plr.Character then hookChar(plr.Character) end
    plr.CharacterAdded:Connect(hookChar)
end

-- ================================================================
-- ANTI HIT - ORBIT
-- ================================================================
local antiHitConn, antiHitParts = nil, {}
local function updateAntiHit()
    if antiHitConn then antiHitConn:Disconnect(); antiHitConn = nil end
    if not S.antiHit then return end
    local char = LP.Character
    if not char then return end
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    antiHitParts = {}
    local function addPart(name)
        local p = char:FindFirstChild(name)
        if p then table.insert(antiHitParts, p) end
    end
    addPart("Head")
    addPart("UpperTorso")
    addPart("LowerTorso")
    addPart("LeftUpperArm")
    addPart("LeftLowerArm")
    addPart("LeftHand")
    addPart("RightUpperArm")
    addPart("RightLowerArm")
    addPart("RightHand")
    addPart("LeftUpperLeg")
    addPart("LeftLowerLeg")
    addPart("LeftFoot")
    addPart("RightUpperLeg")
    addPart("RightLowerLeg")
    addPart("RightFoot")
    local angles = {}
    for i = 1, #antiHitParts do
        angles[i] = (i-1) * (2*math.pi / #antiHitParts)
    end
    local time = 0
    antiHitConn = RunService.RenderStepped:Connect(function(dt)
        if not S.antiHit or not char or not char.Parent then
            return
        end
        local target = getNearestEnemy()
        if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
            local targetPos = target.Character.HumanoidRootPart.Position
            root.CFrame = CFrame.new(root.Position, targetPos)
        end
        time = time + dt * S.antiHitSpeed
        local center = root.Position
        for i, part in ipairs(antiHitParts) do
            if part and part.Parent then
                local angle = angles[i] + time
                local offset = Vector3.new(math.cos(angle) * S.antiHitRadius, math.sin(angle*0.5) * S.antiHitRadius * 0.5, math.sin(angle) * S.antiHitRadius)
                part.CFrame = CFrame.new(center + offset) * CFrame.Angles(0, angle, 0)
            end
        end
    end)
end
UIS.InputBegan:Connect(function(input, gp)
    if gp then return end
    if input.KeyCode == S.antiHitKey then
        S.antiHit = not S.antiHit
        if S.antiHit then
            updateAntiHit()
            showNotification("Anti Hit ON")
        else
            if antiHitConn then antiHitConn:Disconnect(); antiHitConn = nil end
            showNotification("Anti Hit OFF")
        end
    end
end)

-- ================================================================
-- AUTO BACKSTAB (spams 3 and RMB, teleports behind)
-- ================================================================
local backstabActive = false
local backstabLoopRunning = false
local function autoBackstabLoop()
    backstabLoopRunning = true
    while backstabActive do
        if S.autoBackstab then
            local target = getNearestEnemy()
            if target and target.Character then
                local tr = target.Character:FindFirstChild("HumanoidRootPart")
                local mr = LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
                if tr and mr then
                    mr.CFrame = tr.CFrame * CFrame.new(0, 0, 3)
                end
            end
            pcall(function()
                VirtualUser:KeyDown(string.char(S.knifeKey.Value))
                task.wait(0.02)
                VirtualUser:KeyUp(string.char(S.knifeKey.Value))
                VirtualUser:ClickButton1(Vector2.new())
            end)
        end
        task.wait(0.05)
    end
    backstabLoopRunning = false
end
local function toggleAutoBackstab()
    S.autoBackstab = not S.autoBackstab
    if S.autoBackstab then
        backstabActive = true
        if not backstabLoopRunning then
            task.spawn(autoBackstabLoop)
        end
        showNotification("Auto Backstab ON")
    else
        backstabActive = false
        showNotification("Auto Backstab OFF")
    end
end
local keyToggles = {}
UIS.InputBegan:Connect(function(input, gp)
    if gp then return end
    local key = input.KeyCode
    if keyToggles[key] then return end
    keyToggles[key] = true
    if key == S.backstabBind then
        toggleAutoBackstab()
    elseif key == S.orbitBind then
        S.orbitActive = not S.orbitActive
        showNotification(S.orbitActive and "Orbit ON" or "Orbit OFF")
    elseif key == S.followBind then
        S.followTarget = not S.followTarget
        showNotification(S.followTarget and "Follow ON" or "Follow OFF")
    elseif key == S.antiKiciaBind then
        S.antiKicia = not S.antiKicia
        showNotification(S.antiKicia and "ANTI KICIA ON" or "ANTI KICIA OFF")
        if S.antiKicia then S.noclipOn = true end
    elseif key == S.floorbangKey then
        S.floorbang = not S.floorbang
        showNotification(S.floorbang and "Floorbang ON" or "Floorbang OFF")
    end
end)
UIS.InputEnded:Connect(function(input, gp)
    if gp then return end
    keyToggles[input.KeyCode] = nil
end)

-- ================================================================
-- SILENT AIM (Integrated with toggle)
-- ================================================================
local silentAimGui = nil
local silentAimConn = nil
local silentAimOrigRaycast = nil
local silentAimOrigPlayParticles = nil

local function getSilentTarget(origin)
    local center = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    local best, bestDist = nil, math.huge
    local myChar = LP.Character
    for _, p in pairs(Players:GetPlayers()) do
        if p == LP then continue end
        local char = p.Character
        if not char or char == myChar then continue end
        local head = char:FindFirstChild("Head")
        local hum = char:FindFirstChildOfClass("Humanoid")
        if not head or not hum or hum.Health <= 0 then continue end
        if (origin - head.Position).Magnitude > 1000 then continue end
        local sp, vis = Camera:WorldToViewportPoint(head.Position)
        if not vis then continue end
        local d = (Vector2.new(sp.X, sp.Y) - center).Magnitude
        if d < S.silentAimFOV and d < bestDist then
            bestDist = d
            best = head
        end
    end
    return best
end

local function startSilentAim()
    if silentAimGui then return end
    local util = require(ReplicatedStorage.Modules.Utility)
    silentAimOrigRaycast = util.Raycast
    silentAimOrigPlayParticles = util.PlayParticles

    -- Create ScreenGui
    silentAimGui = Instance.new("ScreenGui")
    silentAimGui.Name = "KittySilentAim"
    silentAimGui.IgnoreGuiInset = true
    silentAimGui.DisplayOrder = 999
    silentAimGui.Parent = CoreGui

    local circleFrame = Instance.new("Frame")
    circleFrame.Name = "FOV_Ring"
    circleFrame.AnchorPoint = Vector2.new(0.5, 0.5)
    circleFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
    circleFrame.Size = UDim2.new(0, S.silentAimFOV * 2, 0, S.silentAimFOV * 2)
    circleFrame.BackgroundTransparency = 1
    circleFrame.Parent = silentAimGui

    local uiStroke = Instance.new("UIStroke")
    uiStroke.Thickness = 1.5
    uiStroke.Color = S.silentAimColor
    uiStroke.Transparency = S.silentAimTransparency
    uiStroke.Parent = circleFrame

    local uiCorner = Instance.new("UICorner")
    uiCorner.CornerRadius = UDim.new(1, 0)
    uiCorner.Parent = circleFrame

    local snapline = Instance.new("Frame")
    snapline.Name = "Snapline"
    snapline.AnchorPoint = Vector2.new(0.5, 0.5)
    snapline.BackgroundColor3 = S.silentAimColor
    snapline.BorderSizePixel = 0
    snapline.BackgroundTransparency = S.silentAimTransparency
    snapline.Visible = false
    snapline.Parent = silentAimGui

    -- Hook Raycast
    util.Raycast = function(self, origin, direction, distance, ...)
        local target = getSilentTarget(origin)
        if target then
            return silentAimOrigRaycast(self, origin, target.Position, distance, ...)
        end
        return silentAimOrigRaycast(self, origin, direction, distance, ...)
    end

    util.PlayParticles = function(self, obj)
        if typeof(obj) == "Instance" then
            local n = obj.Name:lower()
            if n:find("flash") or n:find("smoke") or n:find("blind") then return end
        end
        return silentAimOrigPlayParticles(self, obj)
    end

    -- Render loop
    silentAimConn = RunService.RenderStepped:Connect(function()
        local center = Camera.ViewportSize / 2
        local myRoot = LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
        local target = getSilentTarget(myRoot and myRoot.Position or Camera.CFrame.Position)
        local stroke = circleFrame:FindFirstChildOfClass("UIStroke")
        local line = silentAimGui:FindFirstChild("Snapline")
        if not stroke or not line then return end
        if target then
            stroke.Color = Color3.fromRGB(255, 50, 50)
            stroke.Transparency = 0
            local sp, vis = Camera:WorldToViewportPoint(target.Position)
            if vis then
                local p1 = center
                local p2 = Vector2.new(sp.X, sp.Y)
                local midPoint = (p1 + p2) / 2
                local distance = (p2 - p1).Magnitude
                local angle = math.deg(math.atan2(p2.Y - p1.Y, p2.X - p1.X))
                line.Size = UDim2.new(0, distance, 0, 1.5)
                line.Position = UDim2.new(0, midPoint.X, 0, midPoint.Y)
                line.Rotation = angle
                line.Visible = true
            else
                line.Visible = false
            end
        else
            stroke.Color = S.silentAimColor
            stroke.Transparency = S.silentAimTransparency
            line.Visible = false
        end
        -- Update circle size
        circleFrame.Size = UDim2.new(0, S.silentAimFOV * 2, 0, S.silentAimFOV * 2)
    end)
end

local function stopSilentAim()
    if silentAimConn then
        silentAimConn:Disconnect()
        silentAimConn = nil
    end
    if silentAimGui then
        silentAimGui:Destroy()
        silentAimGui = nil
    end
    if silentAimOrigRaycast then
        local util = require(ReplicatedStorage.Modules.Utility)
        util.Raycast = silentAimOrigRaycast
        util.PlayParticles = silentAimOrigPlayParticles
        silentAimOrigRaycast = nil
        silentAimOrigPlayParticles = nil
    end
end

-- ================================================================
-- SKIN CHANGER LOADER
-- ================================================================
local function loadSkinChangerScript()
    local success, err = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/EndOverdosing/Soluna-API/refs/heads/main/skin-changer.lua", true))()
    end)
    if success then
        showNotification("Skin Changer Loaded")
    else
        showNotification("Failed to load Skin Changer: " .. tostring(err))
    end
end

-- ================================================================
-- PROFILES
-- ================================================================
local profiles = {
    ["Auto"] = function()
        initState()
        S.skyTime = 0; S.motionBlurEnabled = false
    end,
    ["Rage"] = function()
        initState()
        S.aimbotOn = true; S.aimbotAggr = 100; S.visCheck = false; S.aimSens = 1.0
        S.hitSoundOn = true; S.hitNotifOn = true
        S.boxESP = true; S.boxColor = Color3.fromRGB(138,43,226)
        S.snapEnabled = true; S.snapColor = Color3.fromRGB(138,43,226)
        S.skelESP = true; S.skelColor = Color3.fromRGB(138,43,226)
        S.chamsEnabled = true
        S.chamsFill = Color3.fromRGB(200,200,200); S.chamsFillTr = 0.7
        S.chamsOutline = Color3.fromRGB(255,255,255); S.chamsOutTr = 0.3
        S.cfOn = true; S.cfSpeed = 35
        S.flightOn = true; S.flightSpeed = 180
        S.orbitActive = true; S.orbitHeight = 12; S.noclipOn = true
        S.snowEnabled = true; S.lockSky = true; S.skyTime = 0
        S.crossEnabled = true; S.crossColor = Color3.fromRGB(138,43,226)
        S.fovEnabled = true; S.fovFilled = true; S.fovFillColor = Color3.fromRGB(138,43,226)
        S.watermarkOn = true; S.forcedFOV = 179
        S.nameESP = true; S.distESP = true
    end,
    ["Legit"] = function()
        initState()
        S.aimbotOn = true; S.aimbotAggr = 45; S.visCheck = true; S.aimSens = 1.0
        S.lockSky = true; S.skyTime = 0
    end,
    ["Semi-Rage"] = function()
        initState()
        S.aimbotOn = true; S.aimbotAggr = 70; S.hitNotifOn = true
        S.boxColor = Color3.fromRGB(255,105,180); S.snapColor = Color3.fromRGB(255,105,180)
        S.skelColor = Color3.fromRGB(255,105,180)
        S.cfOn = true; S.cfSpeed = 25; S.noclipOn = true; S.snowEnabled = true
        S.boxESP = true; S.snapEnabled = true; S.skelESP = true
        S.crossEnabled = true; S.fovEnabled = true; S.fovFilled = true; S.hitSoundOn = true
        S.nameESP = true
    end,
    ["Advanced Rage"] = function()
        initState()
        S.aimbotOn = true; S.aimbotAggr = 100; S.visCheck = false; S.aimSens = 0.8
        S.hitSoundOn = true; S.hitNotifOn = true
        S.boxESP = true; S.boxColor = Color3.fromRGB(255,0,0)
        S.snapEnabled = true; S.snapColor = Color3.fromRGB(255,0,0)
        S.skelESP = true; S.skelColor = Color3.fromRGB(255,0,0)
        S.chamsEnabled = true
        S.chamsFill = Color3.fromRGB(255,0,0); S.chamsFillTr = 0.6
        S.chamsOutline = Color3.fromRGB(0,0,0); S.chamsOutTr = 0
        S.cfOn = true; S.cfSpeed = 60
        S.flightOn = true; S.flightSpeed = 250
        S.orbitActive = true; S.orbitHeight = 15; S.noclipOn = true
        S.snowEnabled = true; S.lockSky = true; S.skyTime = 0
        S.crossEnabled = true; S.crossColor = Color3.fromRGB(255,0,0)
        S.fovEnabled = true; S.fovFilled = true; S.fovFillColor = Color3.fromRGB(255,0,0)
        S.watermarkOn = true; S.forcedFOV = 179
        S.nameESP = true; S.distESP = true
    end
}

-- ================================================================
-- UI COMPONENTS REGISTRY
-- ================================================================
local uiComponents = {}
local function addUI(flag, component) uiComponents[flag] = component; return component end
local function syncAllUI()
    for flagName, comp in pairs(uiComponents) do
        local val = S[flagName]
        if val ~= nil and comp and comp.Set then
            pcall(function()
                if typeof(val) == "EnumItem" then comp:Set(tostring(val))
                else comp:Set(val) end
            end)
        end
    end
end
local function applyProfile(profileName)
    local profileFunc = profiles[profileName]
    if not profileFunc then return false end
    profileFunc()
    refreshChams(); updateTint()
    if S.glassWalls then applyGlass() else restoreGlass() end
    if S.snowEnabled then startSnow() else stopSnow() end
    updateCrosshair()
    if S.lockSky then Lighting.ClockTime = S.skyTime end
    if S.flightOn then startFlight() else stopFlight() end
    if S.cfOn then startCF() else stopCF() end
    fovCircle.Visible = S.fovEnabled
    fovFillCircle.Visible = S.fovFilled and S.fovEnabled
    fovCircle.Radius = S.fovRadius
    fovFillCircle.Color = S.fovFillColor
    fovFillCircle.Transparency = S.fovFillTr
    if not S.crossSpin then S.crossAngle = 0 end
    if S.wrapEnabled and currentWrap then
        wrapApplyAll()
    end
    if S.skyboxEnabled then
        setSkybox(S.skyboxId)
    else
        setSkybox("")
    end
    refreshWireframes()
    if S.deviceSpoofEnabled then
        applyDeviceSpoof(S.deviceSpoofTarget)
    end
    if S.winStreakSpoof then startWinStreakSpoof() end
    if S.antiHit then updateAntiHit() end
    if S.silentAimEnabled then startSilentAim() else stopSilentAim() end
    syncAllUI()
    showNotification("Loaded profile: " .. profileName)
    return true
end

-- ================================================================
-- MAIN SCRIPT FUNCTION
-- ================================================================
function loadMainScript()
    initState()
    initNotifs()
    
    -- INFO BAR
    local infoGui = Instance.new("ScreenGui")
    infoGui.Name = "KittyHubV2Info"
    infoGui.ResetOnSpawn = false
    infoGui.DisplayOrder = 999
    infoGui.IgnoreGuiInset = true
    infoGui.Parent = LP:WaitForChild("PlayerGui")
    local infoF = Instance.new("Frame")
    infoF.Size = UDim2.new(0, 240, 0, 18)
    infoF.Position = UDim2.new(0, 6, 1, -24)
    infoF.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
    infoF.BackgroundTransparency = 0.2
    infoF.BorderSizePixel = 0
    infoF.Parent = infoGui
    Instance.new("UICorner", infoF).CornerRadius = UDim.new(0, 4)
    local ist = Instance.new("UIStroke")
    ist.Color = Color3.fromRGB(100,100,100)
    ist.Thickness = 1
    ist.Transparency = 0.4
    ist.Parent = infoF
    local statsLbl = Instance.new("TextLabel")
    statsLbl.Size = UDim2.new(1, -4, 1, 0)
    statsLbl.Position = UDim2.new(0, 2, 0, 0)
    statsLbl.BackgroundTransparency = 1
    statsLbl.Text = "FPS: -- | Ping: -- | Region: ???"
    statsLbl.TextColor3 = Color3.fromRGB(220,220,220)
    statsLbl.TextSize = 10
    statsLbl.Font = Enum.Font.Gotham
    statsLbl.TextXAlignment = Enum.TextXAlignment.Left
    statsLbl.Parent = infoF
    task.spawn(function()
        while true do
            local fps = math.floor(1 / (RunService.RenderStepped:Wait() or 0.016))
            local ping = math.floor((Stats.Network.ServerStatsItem["Data Ping"]:GetValue() or 0))
            local region = "???"
            pcall(function()
                local location = NetworkClient:GetServerLocation()
                if location and location ~= "" then
                    region = location:match("%((%w+)%)") or location
                else
                    region = LP:GetAttribute("Region") or "???"
                end
            end)
            statsLbl.Text = string.format("FPS: %d | Ping: %d | Region: %s", fps, ping, region)
            task.wait(0.5)
        end
    end)

    -- ============================================================
    -- LIBRARY
    -- ============================================================
    local library = loadstring(game:GetObjects("rbxassetid://7657867786")[1].Source)()
    local Window = library:CreateWindow({Name = "KittyHub V2", Themeable = true})
    pcall(function()
        library:SetTheme({
            Main = Color3.fromRGB(22, 22, 22),
            Background = Color3.fromRGB(16, 16, 16),
            OuterBorder = Color3.fromRGB(115, 115, 115),
            InnerBorder = Color3.fromRGB(85, 85, 85),
            TopGradient = Color3.fromRGB(75, 75, 75),
            BottomGradient = Color3.fromRGB(48, 48, 48),
            SectionBackground = Color3.fromRGB(35, 35, 35),
            Section = Color3.fromRGB(52, 52, 52),
            ElementText = Color3.fromRGB(215, 215, 215),
            OtherElementText = Color3.fromRGB(195, 195, 195),
            TabText = Color3.fromRGB(175, 175, 175),
            ElementBorder = Color3.fromRGB(135, 135, 135),
            SelectedOption = Color3.fromRGB(95, 95, 95),
            UnselectedOption = Color3.fromRGB(55, 55, 55),
            HoveredOptionTop = Color3.fromRGB(88, 88, 88),
            UnhoveredOptionTop = Color3.fromRGB(65, 65, 65),
            HoveredOptionBottom = Color3.fromRGB(72, 72, 72),
            UnhoveredOptionBottom = Color3.fromRGB(46, 46, 46)
        })
    end)
    task.wait(0.5)
    local mainGui = game:GetService("CoreGui"):FindFirstChild("KittyHub V2")
    if mainGui then
        local function addIcon(pos)
            local il = Instance.new("ImageLabel")
            il.Size = UDim2.new(0,28,0,28)
            il.Position = pos
            il.BackgroundTransparency = 1
            il.Image = "rbxthumb://type=Asset&id=12140499158&w=56&h=56"
            il.ZIndex = 100
            il.Parent = mainGui
        end
        addIcon(UDim2.new(0, 8, 0, 8))
        addIcon(UDim2.new(1,-36, 0, 8))
    end
    local menuBlur = nil
    task.spawn(function()
        while true do
            local mg = game:GetService("CoreGui"):FindFirstChild("KittyHub V2")
            if mg and mg.Enabled then
                if not menuBlur then
                    menuBlur = Instance.new("BlurEffect")
                    menuBlur.Size = 10
                    menuBlur.Parent = Camera
                end
            else
                if menuBlur then menuBlur:Destroy(); menuBlur = nil end
            end
            task.wait(0.2)
        end
    end)

    -- PLAYER HOOKS
    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= LP then
            p.CharacterAdded:Connect(function(c) task.wait(); applyChams(p, c); if S.wireframeESP then applyWireframe(c) end end)
            p.CharacterRemoving:Connect(function() removeChams(p); removeWireframe(p.Character) end)
            if p.Character then applyChams(p, p.Character); if S.wireframeESP then applyWireframe(p.Character) end end
        end
    end
    Players.PlayerAdded:Connect(function(p)
        if p ~= LP then
            p.CharacterAdded:Connect(function(c) task.wait(); applyChams(p, c); if S.wireframeESP then applyWireframe(c) end end)
            p.CharacterRemoving:Connect(function() removeChams(p); removeWireframe(p.Character) end)
        end
    end)
    Players.PlayerRemoving:Connect(function(p)
        removeChams(p); destroySkel(p)
    end)
    task.spawn(function()
        while true do
            task.wait(0.5)
            if S.chamsEnabled then refreshChams() end
            if S.wireframeESP then refreshWireframes() end
        end
    end)
    for _, p in ipairs(Players:GetPlayers()) do hookPlayer(p) end
    Players.PlayerAdded:Connect(hookPlayer)

    -- GAME SYSTEM INITS
    applyGlass(); updateTint()
    if S.snowEnabled then startSnow() end
    startWinStreakSpoof()
    if S.skyboxEnabled then setSkybox(S.skyboxId) end
    if S.silentAimEnabled then startSilentAim() end
    RunService.RenderStepped:Connect(function()
        Camera.FieldOfView = S.forcedFOV
    end)
    RunService.Stepped:Connect(function()
        if not S.noclipOn then return end
        local char = LP.Character; if not char then return end
        for _, p in ipairs(char:GetDescendants()) do
            if p:IsA("BasePart") then p.CanCollide = false end
        end
    end)
    RunService.RenderStepped:Connect(function()
        if S.lockSky then Lighting.ClockTime = S.skyTime end
    end)
    workspace.DescendantAdded:Connect(function(part)
        if S.glassWalls and part:IsA("BasePart") and not isPlayerPart(part)
            and part.Size.Magnitude >= 5 then
            task.wait(0.1)
            if not S.glassCache[part] then
                S.glassCache[part] = {
                    trans = part.Transparency, mat = part.Material, ref = part.Reflectance
                }
                part.Transparency = 0.65
                part.Material = Enum.Material.Glass
                part.Reflectance = 0.15
                part.Color = Color3.fromRGB(180,220,255)
            end
        end
        if S.darkTextures and part:IsA("BasePart") and not isPlayerPart(part)
            and not S.darkTexCache[part] then
            task.wait(0.1)
            S.darkTexCache[part] = {
                Color=part.Color, Material=part.Material, Reflectance=part.Reflectance
            }
            part.Color = Color3.fromRGB(22,22,22)
            part.Material = Enum.Material.SmoothPlastic
            part.Reflectance = 0
        end
    end)

    -- Texture nuker loop
    task.spawn(function()
        while true do
            task.wait(5)
            if S.textureNuker then
                if tick() - S.lastNuke >= 5 then
                    S.lastNuke = tick()
                    for _, part in ipairs(workspace:GetDescendants()) do
                        if part:IsA("BasePart") and not S.nukedParts[part] then
                            S.nukedParts[part] = {mat = part.Material}
                            part.Material = Enum.Material.SmoothPlastic
                            for _, d in ipairs(part:GetDescendants()) do
                                if d:IsA("Decal") then d.Visible = false end
                            end
                        end
                    end
                end
            else
                for part, orig in pairs(S.nukedParts) do
                    if part and part.Parent then
                        part.Material = orig.mat
                        for _, d in ipairs(part:GetDescendants()) do
                            if d:IsA("Decal") then d.Visible = true end
                        end
                    end
                end
                S.nukedParts = {}
            end
        end
    end)

    -- WIDE CAMERA
    local wideCamConn
    local function updateWideCamera()
        if wideCamConn then wideCamConn:Disconnect(); wideCamConn = nil end
        if not S.wideCamera then return end
        local cam = Camera
        local intensity = S.wideIntensity
        wideCamConn = RunService.RenderStepped:Connect(function()
            cam.FieldOfView = 120
            local currentCF = cam.CFrame
            local lookAt = currentCF.Position + (currentCF.LookVector * 10)
            cam.CFrame = CFrame.lookAt(
                currentCF.Position - (currentCF.RightVector * 0.001),
                lookAt
            ) * CFrame.new(0, 0, intensity)
        end)
        if LP.Character and LP.Character:FindFirstChild("Humanoid") then
            LP.Character.Humanoid.CameraOffset = Vector3.new(0, 0, 0)
        end
        showNotification("Wide Camera ON")
    end
    local function stopWideCamera()
        if wideCamConn then wideCamConn:Disconnect(); wideCamConn = nil end
        showNotification("Wide Camera OFF")
    end

    -- HVH render loop (Floorbang)
    local floorbangLmbHeld = false
    RunService.RenderStepped:Connect(function(dt)
        if not LP.Character then return end
        local function teleportBehind(t)
            local tr = t.Character and t.Character:FindFirstChild("HumanoidRootPart"); if not tr then return end
            local mr = LP.Character:FindFirstChild("HumanoidRootPart"); if not mr then return end
            mr.CFrame = tr.CFrame * CFrame.new(0, 0, 3)
        end
        local function placeAbove(t, h)
            local th = t.Character and t.Character:FindFirstChild("Head"); if not th then return end
            local mr = LP.Character:FindFirstChild("HumanoidRootPart"); if not mr then return end
            mr.CFrame = CFrame.new(th.Position + Vector3.new(0, h, 0), th.Position)
        end
        if S.orbitActive then
            local t = getNearestEnemy()
            if t then placeAbove(t, S.orbitHeight) end
        elseif S.autoBackstab then
            -- Handled by separate loop
        elseif S.followTarget then
            local t = getNearestEnemy()
            if t then
                local tr = t.Character and t.Character:FindFirstChild("HumanoidRootPart")
                if tr then LP.Character.HumanoidRootPart.CFrame = tr.CFrame * CFrame.new(0,0,3) end
            end
        elseif S.floorbang then
            local t = getNearestEnemy()
            if t and t.Character and t.Character:FindFirstChild("HumanoidRootPart") then
                local tr = t.Character.HumanoidRootPart
                local mr = LP.Character:FindFirstChild("HumanoidRootPart")
                if mr then
                    mr.CFrame = CFrame.new(tr.Position.X, tr.Position.Y + S.floorbangHeightOffset, tr.Position.Z)
                    if mr.Position.Y < -50 then mr.CFrame = mr.CFrame + Vector3.new(0, 100, 0) end
                    if S.floorbangSpinEnabled then
                        mr.CFrame = mr.CFrame
                            * CFrame.Angles(0, math.rad(S.floorbangSpinSpeed * dt * 360), 0)
                    end
                    local head = t.Character:FindFirstChild("Head")
                    if head then
                        local sp, on = Camera:WorldToViewportPoint(head.Position)
                        if on and sp.Z > 0 then
                            local delta = Vector2.new(sp.X, sp.Y) - UIS:GetMouseLocation()
                            if delta.Magnitude > 1 then
                                local aggr = S.floorbangAimAggr / 100
                                mousemoverel(delta.X * aggr, delta.Y * aggr)
                            end
                        end
                    end
                    if not floorbangLmbHeld then
                        pcall(function() VirtualUser:Button1Down(Vector2.new()) end)
                        floorbangLmbHeld = true
                    end
                end
            else
                if floorbangLmbHeld then
                    pcall(function() VirtualUser:Button1Up(Vector2.new()) end)
                    floorbangLmbHeld = false
                end
            end
        else
            if floorbangLmbHeld then
                pcall(function() VirtualUser:Button1Up(Vector2.new()) end)
                floorbangLmbHeld = false
            end
        end
    end)

    -- Anti Kicia loop
    RunService.RenderStepped:Connect(function()
        if not S.antiKicia or not LP.Character then return end
        local target = getNearestEnemy(); if not target or not target.Character then return end
        local mr = LP.Character:FindFirstChild("HumanoidRootPart")
        local tr = target.Character:FindFirstChild("HumanoidRootPart")
        if not mr or not tr then return end
        local tpos = tr.CFrame:PointToWorldSpace(Vector3.new(0, S.akHeight, -S.akDist))
        mr.CFrame = CFrame.new(tpos) * CFrame.Angles(math.rad(180), 0, 0)
        mr.AssemblyLinearVelocity = Vector3.zero
        if tpos.Y < 10 then mr.CFrame = mr.CFrame + Vector3.new(0, 50, 0) end
        if S.akAimAssist then
            local head = target.Character:FindFirstChild("Head")
            if head then
                local sp, on = Camera:WorldToViewportPoint(head.Position)
                if on and sp.Z > 0 then
                    local delta = Vector2.new(sp.X, sp.Y) - UIS:GetMouseLocation()
                    if delta.Magnitude > 2 then mousemoverel(delta.X * 0.8, delta.Y * 0.8) end
                end
            end
        end
    end)

    -- Strafe
    local strafeConn
    local function updateStrafe()
        if strafeConn then strafeConn:Disconnect(); strafeConn = nil end
        if not S.strafeOn then return end
        strafeConn = RunService.RenderStepped:Connect(function()
            local char = LP.Character; if not char then return end
            local root = char:FindFirstChild("HumanoidRootPart")
            local hum = char:FindFirstChildOfClass("Humanoid")
            if not root or not hum or hum.MoveDirection.Magnitude < 0.1 then return end
            local should = S.strafeRMB
                and UIS:IsMouseButtonPressed(Enum.UserInputType.MouseButton2)
                or UIS:IsKeyDown(S.strafeKey)
            if should then
                S.strafeDir = -S.strafeDir
                root.CFrame = root.CFrame + Camera.CFrame.RightVector * S.strafeDir * S.strafeDist
            end
        end)
    end

    LP.CharacterAdded:Connect(function()
        task.wait(0.5)
        if S.flightOn then startFlight() end
        if S.antiHit then updateAntiHit() end
    end)

    -- MASTER RENDER LOOP — ESP, Aimbot, FOV
    RunService.RenderStepped:Connect(function(dt)
        local fovPos = (S.fovMode == "Mouse")
            and UIS:GetMouseLocation()
            or Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
        fovCircle.Position = fovPos
        fovFillCircle.Position = fovPos
        fovFillCircle.Radius = S.fovRadius
        fovFillCircle.Color = S.fovFillColor
        fovFillCircle.Transparency = S.fovFillTr
        fovFillCircle.Visible = S.fovFilled and fovCircle.Visible
        if S.aimbotOn and not S.antiKicia and not S.floorbang then
            local rmbDown = UIS:IsMouseButtonPressed(Enum.UserInputType.MouseButton2)
            if rmbDown then
                if not S.rmbWasDown then S.lockedTarget = findBestTarget() end
                S.rmbWasDown = true
                if S.lockedTarget and S.lockedTarget.Character then
                    local part = S.lockedTarget.Character:FindFirstChild(S.aimbotPart)
                             or S.lockedTarget.Character:FindFirstChild("HumanoidRootPart")
                    if part then
                        local targetPos = part.Position
                        if S.aimPrediction > 0 then
                            local root = S.lockedTarget.Character:FindFirstChild("HumanoidRootPart")
                            if root then
                                targetPos = targetPos + (root.AssemblyLinearVelocity or Vector3.zero) * S.aimPrediction * 0.1
                            end
                        end
                        local sp, on = Camera:WorldToViewportPoint(targetPos)
                        if on and sp.Z > 0 then
                            local delta = Vector2.new(sp.X, sp.Y) - UIS:GetMouseLocation()
                            if delta.Magnitude > 0.3 then
                                local aggr = math.clamp(S.aimbotAggr/100, 0.01, 1)
                                local fx, fy = delta.X * aggr, delta.Y * aggr
                                if S.jitter then
                                    fx = fx + (math.random()*2-1) * S.jitterStr * 0.7
                                    fy = fy + (math.random()*2-1) * S.jitterStr * 0.7
                                end
                                mousemoverel(fx / S.aimSens, fy / S.aimSens)
                            end
                        end
                    end
                end
            else
                if S.rmbWasDown then S.lockedTarget = nil end
                S.rmbWasDown = false
            end
        else
            S.rmbWasDown = false
            S.lockedTarget = nil
        end
        local vp = Camera.ViewportSize
        local midX = vp.X / 2
        local midY = vp.Y / 2
        local anyESP = S.boxESP or S.nameESP or S.distESP
                    or S.snapEnabled or S.skelESP
        for _, plr in ipairs(Players:GetPlayers()) do
            if plr == LP then continue end
            local o = getPool(plr)
            local char = plr.Character
            if not anyESP or not char then hidePool(o); hideCirc(plr); hideSkel(plr); continue end
            local hum = char:FindFirstChildOfClass("Humanoid")
            if not hum or hum.Health <= 0 then hidePool(o); hideCirc(plr); hideSkel(plr); continue end
            local root = char:FindFirstChild("HumanoidRootPart")
            if not root then hidePool(o); hideCirc(plr); hideSkel(plr); continue end
            local L, T, R, B, cx = getFastBounds(char)
            if not L then hidePool(o); hideCirc(plr); hideSkel(plr); continue end
            local dist = (root.Position - Camera.CFrame.Position).Magnitude
            L = L - 1; R = R + 1; T = T - 1; B = B + 1
            if S.boxESP then
                local hp = hum.Health / hum.MaxHealth
                local hpCol = hp > 0.6 and S.hpBarColor
                           or hp > 0.3 and Color3.fromRGB(215,170,0)
                           or Color3.fromRGB(215,50,50)
                if S.boxStyle == "Full" then
                    for i, l in ipairs(o.box) do
                        l.Thickness = S.boxThickness
                        l.Color = S.boxColor
                    end
                    o.box[1].From = Vector2.new(L,T); o.box[1].To = Vector2.new(R,T); o.box[1].Visible = true
                    o.box[2].From = Vector2.new(R,T); o.box[2].To = Vector2.new(R,B); o.box[2].Visible = true
                    o.box[3].From = Vector2.new(R,B); o.box[3].To = Vector2.new(L,B); o.box[3].Visible = true
                    o.box[4].From = Vector2.new(L,B); o.box[4].To = Vector2.new(L,T); o.box[4].Visible = true
                    for _, l in ipairs(o.cornerLines) do l.Visible = false end
                else
                    local distScale = math.clamp(800 / math.max(dist, 1), 0.4, 1.2)
                    local cornerLen = math.min(12 * distScale, 30)
                    for i=1,8 do
                        o.cornerLines[i].Thickness = S.cornerThickness
                        o.cornerLines[i].Color = S.boxColor
                    end
                    o.cornerLines[1].From = Vector2.new(L, T); o.cornerLines[1].To = Vector2.new(L + cornerLen, T)
                    o.cornerLines[2].From = Vector2.new(L, T); o.cornerLines[2].To = Vector2.new(L, T + cornerLen)
                    o.cornerLines[3].From = Vector2.new(R, T); o.cornerLines[3].To = Vector2.new(R - cornerLen, T)
                    o.cornerLines[4].From = Vector2.new(R, T); o.cornerLines[4].To = Vector2.new(R, T + cornerLen)
                    o.cornerLines[5].From = Vector2.new(L, B); o.cornerLines[5].To = Vector2.new(L + cornerLen, B)
                    o.cornerLines[6].From = Vector2.new(L, B); o.cornerLines[6].To = Vector2.new(L, B - cornerLen)
                    o.cornerLines[7].From = Vector2.new(R, B); o.cornerLines[7].To = Vector2.new(R - cornerLen, B)
                    o.cornerLines[8].From = Vector2.new(R, B); o.cornerLines[8].To = Vector2.new(R, B - cornerLen)
                    for i=1,8 do o.cornerLines[i].Visible = true end
                    for _, l in ipairs(o.box) do l.Visible = false end
                end
                local bH = B - T
                local hp_filled = bH * hp
                if S.hpBarPos == "Right" then
                    local bx = R + 5
                    local fillY = B - hp_filled
                    o.hpBg.From = Vector2.new(bx,T); o.hpBg.To = Vector2.new(bx,B)
                    o.hpBg.Color = Color3.fromRGB(18,18,18); o.hpBg.Transparency = 0.55; o.hpBg.Visible = true
                    o.hpFg.From = Vector2.new(bx,fillY); o.hpFg.To = Vector2.new(bx,B)
                    o.hpFg.Color = hpCol; o.hpFg.Transparency = 0.1; o.hpFg.Visible = true
                    o.hpCap.Position = Vector2.new(bx,fillY); o.hpCap.Color = hpCol
                    o.hpCap.Transparency = 0.1; o.hpCap.Visible = true
                elseif S.hpBarPos == "Left" then
                    local bx = L - 5
                    local fillY = B - hp_filled
                    o.hpBg.From = Vector2.new(bx,T); o.hpBg.To = Vector2.new(bx,B)
                    o.hpBg.Color = Color3.fromRGB(18,18,18); o.hpBg.Transparency = 0.55; o.hpBg.Visible = true
                    o.hpFg.From = Vector2.new(bx,fillY); o.hpFg.To = Vector2.new(bx,B)
                    o.hpFg.Color = hpCol; o.hpFg.Transparency = 0.1; o.hpFg.Visible = true
                    o.hpCap.Position = Vector2.new(bx,fillY); o.hpCap.Color = hpCol
                    o.hpCap.Transparency = 0.1; o.hpCap.Visible = true
                elseif S.hpBarPos == "Top" then
                    local by = T - 8
                    local bxLeft = L
                    local bxRight = L + (R - L) * hp
                    o.hpBg.From = Vector2.new(L,by); o.hpBg.To = Vector2.new(R,by)
                    o.hpBg.Color = Color3.fromRGB(18,18,18); o.hpBg.Thickness = 5
                    o.hpBg.Transparency = 0.55; o.hpBg.Visible = true
                    o.hpFg.From = Vector2.new(bxLeft,by); o.hpFg.To = Vector2.new(bxRight,by)
                    o.hpFg.Color = hpCol; o.hpFg.Thickness = 5
                    o.hpFg.Transparency = 0.1; o.hpFg.Visible = true
                    o.hpCap.Position = Vector2.new(bxRight,by); o.hpCap.Color = hpCol
                    o.hpCap.Transparency = 0.1; o.hpCap.Visible = true
                elseif S.hpBarPos == "Bottom" then
                    local by = B + 8
                    local bxRight = L + (R - L) * hp
                    o.hpBg.From = Vector2.new(L,by); o.hpBg.To = Vector2.new(R,by)
                    o.hpBg.Color = Color3.fromRGB(18,18,18); o.hpBg.Thickness = 5
                    o.hpBg.Transparency = 0.55; o.hpBg.Visible = true
                    o.hpFg.From = Vector2.new(L,by); o.hpFg.To = Vector2.new(bxRight,by)
                    o.hpFg.Color = hpCol; o.hpFg.Thickness = 5
                    o.hpFg.Transparency = 0.1; o.hpFg.Visible = true
                    o.hpCap.Position = Vector2.new(bxRight,by); o.hpCap.Color = hpCol
                    o.hpCap.Transparency = 0.1; o.hpCap.Visible = true
                end
            else
                for _, l in ipairs(o.box) do l.Visible = false end
                for _, l in ipairs(o.cornerLines) do l.Visible = false end
                o.hpBg.Visible = false; o.hpFg.Visible = false; o.hpCap.Visible = false
            end
            if S.nameESP then
                local nameText = plr.Name
                if S.winStreakSpoof then
                    nameText = nameText .. " [WS: " .. S.winStreakTarget .. "]"
                end
                o.nameLbl.Text = nameText
                o.nameLbl.Size = S.nameFontSize
                o.nameLbl.Position = Vector2.new(cx - o.nameLbl.TextBounds.X/2, T - 16)
                o.nameLbl.Visible = true
            else o.nameLbl.Visible = false end
            if S.distESP then
                o.distLbl.Text = math.floor(dist) .. "m"
                o.distLbl.Position = Vector2.new(cx - o.distLbl.TextBounds.X/2, B + 3)
                o.distLbl.Visible = true
            else o.distLbl.Visible = false end
            o.gunLbl.Visible = false
            if S.snapEnabled then
                local tgt = S.snapMode == "Cursor" and UIS:GetMouseLocation()
                         or S.snapMode == "Centre" and Vector2.new(midX, midY)
                         or Vector2.new(midX, vp.Y)
                local sp = Camera:WorldToViewportPoint(root.Position)
                o.snap.From = Vector2.new(sp.X, sp.Y)
                o.snap.To = tgt
                o.snap.Color = S.snapColor
                o.snap.Transparency = 0.4
                o.snap.Visible = true
            else o.snap.Visible = false end
            if S.circPiece then
                local footW = root.Position - Vector3.new(0, root.Size.Y/2 + 0.05, 0)
                local fsc, fon = Camera:WorldToViewportPoint(footW)
                if fon and fsc.Z > 0 then
                    local r = math.clamp(1400 / math.max(dist, 1), 6, 28)
                    local cp = getCircPool(plr)
                    cp.shadow.Position = Vector2.new(fsc.X, fsc.Y); cp.shadow.Radius = r + 7; cp.shadow.Visible = true
                    cp.inner.Position = Vector2.new(fsc.X, fsc.Y); cp.inner.Radius = r; cp.inner.Visible = true
                else hideCirc(plr) end
            else hideCirc(plr) end
            if S.skelESP then renderSkeleton(plr, char) else hideSkel(plr) end
        end
        for p in pairs(espPool) do if not p or not p.Parent then destroyPool(p) end end
        for p in pairs(S.circDrawings) do if not p or not p.Parent then destroyCirc(p) end end
        for p in pairs(skelPool) do if not p or not p.Parent then destroySkel(p) end end
    end)

    UIS.MouseIconEnabled = false

    -- ============================================================
    -- UI TABS (Team Check toggle removed)
    -- ============================================================
    local VT = Window:CreateTab({Name = "Visuals"})
    local ChS = VT:CreateSection({Name = "Chams"})
    addUI("ChamsOn", ChS:AddToggle({Name="Enable",Flag="ChamsOn",Value=false,Callback=function(v) S.chamsEnabled=v; refreshChams(); showNotification(v and "Chams ON" or "Chams OFF") end}))
    addUI("ChamsFill",ChS:AddColorpicker({Name="Fill",Flag="ChamsFill",Color=S.chamsFill,Callback=function(v) S.chamsFill=v; for _,h in pairs(S.highlights) do h.FillColor=v end end}))
    addUI("ChamsOut", ChS:AddColorpicker({Name="Outline",Flag="ChamsOut",Color=S.chamsOutline,Callback=function(v) S.chamsOutline=v; for _,h in pairs(S.highlights) do h.OutlineColor=v end end}))
    addUI("ChamsFTr", ChS:AddSlider({Name="Fill Alpha",Flag="ChamsFTr",Min=0,Max=100,Value=40,Textbox=true,Format=function(v) return v.."%" end,Callback=function(v) S.chamsFillTr=v/100; for _,h in pairs(S.highlights) do h.FillTransparency=S.chamsFillTr end end}))
    addUI("ChamsOTr", ChS:AddSlider({Name="Outline Alpha",Flag="ChamsOTr",Min=0,Max=100,Value=0,Textbox=true,Format=function(v) return v.."%" end,Callback=function(v) S.chamsOutTr=v/100; for _,h in pairs(S.highlights) do h.OutlineTransparency=S.chamsOutTr end end}))
    addUI("ChamsDist",ChS:AddSlider({Name="Max Distance",Flag="ChamsDist",Min=50,Max=1000,Value=500,Textbox=true,Format=function(v) return v.."st" end,Callback=function(v) S.chamsMaxDist=v; refreshChams() end}))

    local EspS = VT:CreateSection({Name="Box & Labels", Side="Right"})
    addUI("BoxESP", EspS:AddToggle({Name="Box ESP",Flag="BoxESP",Value=false,Callback=function(v) S.boxESP=v; showNotification(v and "Box ESP ON" or "Box ESP OFF") end}))
    addUI("BoxStyle", EspS:AddDropdown({Name="Box Style",Flag="BoxStyle",List={"Full","Corner"},Value="Full",Callback=function(v) S.boxStyle=v end}))
    addUI("BoxCol", EspS:AddColorpicker({Name="Box Color",Flag="BoxCol",Color=S.boxColor,Callback=function(v) S.boxColor=v end}))
    addUI("BoxWidth", EspS:AddSlider({Name="Box Width",Flag="BoxWidth",Min=10,Max=80,Value=38,Textbox=true,Format=function(v) return v.."%" end,Callback=function(v) S.boxWidth=v/100 end}))
    addUI("BoxThickness", EspS:AddSlider({Name="Box Thickness",Flag="BoxThickness",Min=1,Max=10,Value=1,Textbox=true,Callback=function(v) S.boxThickness=v end}))
    addUI("CornerThickness", EspS:AddSlider({Name="Corner Thickness",Flag="CornerThickness",Min=1,Max=10,Value=1.5,Textbox=true,Callback=function(v) S.cornerThickness=v end}))
    addUI("NameESP", EspS:AddToggle({Name="Name ESP",Flag="NameESP",Value=false,Callback=function(v) S.nameESP=v end}))
    addUI("DistESP", EspS:AddToggle({Name="Distance ESP",Flag="DistESP",Value=false,Callback=function(v) S.distESP=v end}))
    addUI("HpBarPos", EspS:AddDropdown({Name="Health Bar Position",Flag="HpBarPos",List={"Right","Left","Top","Bottom"},Value="Right",Callback=function(v) S.hpBarPos=v end}))
    addUI("HpBarColor",EspS:AddColorpicker({Name="Health Bar Color",Flag="HpBarColor",Color=S.hpBarColor,Callback=function(v) S.hpBarColor=v end}))

    local WireS = VT:CreateSection({Name="3D Box ESP (Wireframe)"})
    addUI("WireframeESP", WireS:AddToggle({Name="Enable 3D Box ESP",Flag="WireframeESP",Value=false,Callback=function(v) S.wireframeESP=v; refreshWireframes(); showNotification(v and "3D Box ESP ON" or "3D Box ESP OFF") end}))
    addUI("WireframeColor", WireS:AddColorpicker({Name="Color",Flag="WireframeColor",Color=S.wireframeColor,Callback=function(v) S.wireframeColor=v; refreshWireframes() end}))
    addUI("WireframeThickness", WireS:AddSlider({Name="Thickness",Flag="WireframeThickness",Min=0.02,Max=0.3,Value=0.08,Precise=2,Textbox=true,Format=function(v) return string.format("%.2f",v) end,Callback=function(v) S.wireframeThickness=v; refreshWireframes() end}))

    local SkelS = VT:CreateSection({Name="Skeleton ESP"})
    addUI("SkelESP", SkelS:AddToggle({Name="Skeleton ESP",Flag="SkelESP",Value=false,Callback=function(v) S.skelESP=v; if not v then for p in pairs(skelPool) do hideSkel(p) end end; showNotification(v and "Skeleton ESP ON" or "Skeleton ESP OFF") end}))
    addUI("SkelCol", SkelS:AddColorpicker({Name="Bone Color",Flag="SkelCol",Color=S.skelColor,Callback=function(v) S.skelColor=v end}))
    addUI("SkelThickness", SkelS:AddSlider({Name="Bone Thickness",Flag="SkelThickness",Min=1,Max=10,Value=1.2,Textbox=true,Callback=function(v) S.skelThickness=v end}))

    local CircS = VT:CreateSection({Name="Circular Piece", Side="Right"})
    addUI("CircPiece",CircS:AddToggle({Name="Circular Piece",Flag="CircPiece",Value=false,Callback=function(v) S.circPiece=v; if not v then for p in pairs(S.circDrawings) do destroyCirc(p) end end; showNotification(v and "Circular Piece ON" or "Circular Piece OFF") end}))

    local SnapS = VT:CreateSection({Name="Snaplines"})
    addUI("Snap", SnapS:AddToggle({Name="Snaplines",Flag="Snap",Value=false,Callback=function(v) S.snapEnabled=v end}))
    addUI("SnapMode",SnapS:AddDropdown({Name="Target",Flag="SnapMode",List={"Cursor","Centre","Bottom"},Value="Cursor",Callback=function(v) S.snapMode=v end}))
    addUI("SnapCol", SnapS:AddColorpicker({Name="Color",Flag="SnapCol",Color=S.snapColor,Callback=function(v) S.snapColor=v end}))

    local FovS = VT:CreateSection({Name="FOV Circle", Side="Right"})
    addUI("FOVOn", FovS:AddToggle({Name="Show FOV",Flag="FOVOn",Value=false,Callback=function(v) S.fovEnabled=v; fovCircle.Visible=v; if not v then fovFillCircle.Visible=false end end}))
    addUI("FOVRad", FovS:AddSlider({Name="Radius",Flag="FOVRad",Min=10,Max=800,Value=100,Textbox=true,Format=function(v) return v.."px" end,Callback=function(v) S.fovRadius=v; fovCircle.Radius=v end}))
    addUI("FOVThk", FovS:AddSlider({Name="Thickness",Flag="FOVThk",Min=1,Max=5,Value=1,Textbox=true,Format=function(v) return v.."px" end,Callback=function(v) fovCircle.Thickness=v end}))
    addUI("FOVCol", FovS:AddColorpicker({Name="Outline Color",Flag="FOVCol",Color=Color3.fromRGB(255,255,255),Callback=function(v) fovCircle.Color=v end}))
    addUI("FOVOrig", FovS:AddDropdown({Name="Origin",Flag="FOVOrig",List={"Mouse","Centre"},Value="Mouse",Callback=function(v) S.fovMode=v end}))
    addUI("FOVFill", FovS:AddToggle({Name="Fill FOV",Flag="FOVFill",Value=false,Callback=function(v) S.fovFilled=v end}))
    addUI("FOVFillCol",FovS:AddColorpicker({Name="Fill Color",Flag="FOVFillCol",Color=S.fovFillColor,Callback=function(v) S.fovFillColor=v end}))
    addUI("FOVFillTr", FovS:AddSlider({Name="Fill Transparency",Flag="FOVFillTr",Min=0,Max=100,Value=21,Textbox=true,Format=function(v) return v.."%" end,Callback=function(v) S.fovFillTr=v/100 end}))

    local GlassS = VT:CreateSection({Name="Glass Walls"})
    addUI("GlassWalls",GlassS:AddToggle({Name="Enable Glass Walls",Flag="GlassWalls",Value=false,Callback=function(v) S.glassWalls=v; if v then applyGlass() else restoreGlass() end; showNotification(v and "Glass Walls ON" or "Glass Walls OFF") end}))

    local TintS = VT:CreateSection({Name="World Tint", Side="Right"})
    addUI("TintOn", TintS:AddToggle({Name="Enable Tint",Flag="TintOn",Value=false,Callback=function(v) S.tintEnabled=v; updateTint(); showNotification(v and "World Tint ON" or "World Tint OFF") end}))
    addUI("TintColor", TintS:AddColorpicker({Name="Tint Color",Flag="TintColor",Color=S.tintColor,Callback=function(v) S.tintColor=v; updateTint() end}))
    addUI("TintBright",TintS:AddSlider({Name="Brightness",Flag="TintBright",Min=0,Max=100,Value=15,Precise=2,Textbox=true,Format=function(v) return v.."%" end,Callback=function(v) S.tintBright=v/100; updateTint() end}))

    local DarkTexS = VT:CreateSection({Name="Dark Textures"})
    addUI("DarkTex",DarkTexS:AddToggle({Name="Dark Textures",Flag="DarkTex",Value=false,Callback=function(v)
        S.darkTextures=v
        if v then applyDarkTextures() else restoreDarkTextures() end
        showNotification(v and "Dark Textures ON" or "Dark Textures OFF")
    end}))

    VT:CreateSection({Name="Discord"}):AddLabel({Text="discord.gg/cTtSuya3Rb"})

    -- COMBAT TAB (Team Check toggle removed)
    local CT = Window:CreateTab({Name = "Combat"})
    local AimS = CT:CreateSection({Name="Aimbot"})
    addUI("AimOn", AimS:AddToggle({Name="Enable Aimbot",Flag="AimOn",Value=false,Callback=function(v) S.aimbotOn=v; if not v then S.lockedTarget=nil end; showNotification(v and "Aimbot ON" or "Aimbot OFF") end}))
    addUI("AimAggr", AimS:AddSlider({Name="Aggression",Flag="AimAggr",Min=1,Max=100,Value=95,Textbox=true,Format=function(v) return v==100 and "INSTANT LOCK" or v.."%" end,Callback=function(v) S.aimbotAggr=v end}))
    addUI("AimPrediction", AimS:AddSlider({Name="Prediction",Flag="AimPrediction",Min=0,Max=50,Value=0,Precise=1,Textbox=true,Format=function(v) return v.."x" end,Callback=function(v) S.aimPrediction=v end}))
    addUI("VisCheck", AimS:AddToggle({Name="Visibility Check",Flag="VisCheck",Value=false,Callback=function(v) S.visCheck=v end}))
    addUI("AimDist", AimS:AddSlider({Name="Max Distance",Flag="AimDist",Min=50,Max=1000,Value=500,Textbox=true,Format=function(v) return v.."st" end,Callback=function(v) S.aimbotDist=v end}))
    addUI("AimPart", AimS:AddDropdown({Name="Target Part",Flag="AimPart",List={"Head","HumanoidRootPart"},Value="Head",Callback=function(v) S.aimbotPart=v end}))
    addUI("AimSens", AimS:AddSlider({Name="Sensitivity Override",Flag="AimSens",Min=10,Max=500,Value=100,Precise=2,Textbox=true,Format=function(v) return string.format("%.2fx",v/100) end,Callback=function(v) S.aimSens=v/100 end}))

    local DevS = CT:CreateSection({Name="Device Spoofer", Side="Right"})
    addUI("DeviceTarget",DevS:AddDropdown({Name="Spoof As",Flag="DeviceTarget",List={"VR","MouseKeyboard","Touch","Gamepad"},Value="VR",Callback=function(v) S.deviceSpoofTarget=v end}))
    addUI("DeviceSpoofEnabled",DevS:AddToggle({Name="Auto Spoof on Load",Flag="DeviceSpoofEnabled",Value=false,Callback=function(v)
        S.deviceSpoofEnabled=v
        if v then applyDeviceSpoof(S.deviceSpoofTarget) end
    end}))
    DevS:AddButton({Name="Apply Spoof Now",Callback=function() applyDeviceSpoof(S.deviceSpoofTarget) end})
    DevS:AddButton({Name="Reset to Real Device",Callback=function()
        local real = detectRealDevice()
        local ok, remote = pcall(function()
            return game:GetService("ReplicatedStorage")
                :WaitForChild("Remotes",5):WaitForChild("Replication",5)
                :WaitForChild("Fighter",5):WaitForChild("SetControls",5)
        end)
        if ok and remote then remote:FireServer(real); showNotification("Reset to: "..real)
        else showNotification("Spoof remote not found!") end
    end})

    local WinS = CT:CreateSection({Name="Spoof Win Streak"})
    addUI("WinStreakSpoof", WinS:AddToggle({Name="Enable Win Streak Spoof",Flag="WinStreakSpoof",Value=false,Callback=function(v)
        S.winStreakSpoof=v
        if v then startWinStreakSpoof(); showNotification("Win Streak Spoofing: "..S.winStreakTarget)
        else showNotification("Win Streak Spoof OFF") end
    end}))
    addUI("WinStreakTarget",WinS:AddSlider({Name="Target Win Streak",Flag="WinStreakTarget",Min=0,Max=9999,Value=10,Textbox=true,Format=function(v) return v.." wins" end,Callback=function(v) S.winStreakTarget=v end}))

    local WpnS = CT:CreateSection({Name="Weapon Mods"})
    addUI("HitSnd", WpnS:AddToggle({Name="Hit Sound",Flag="HitSnd",Value=false,Callback=function(v) S.hitSoundOn=v; showNotification(v and "Hit Sound ON" or "Hit Sound OFF") end}))
    addUI("HitSndChoice",WpnS:AddDropdown({Name="Hit Sound",Flag="HitSndChoice",List=S.hitSoundsList,Value="Fortnite Shield Crack",Callback=function(v) S.hitSoundId=S.hitSoundIds[v] end}))
    addUI("HitVol", WpnS:AddSlider({Name="Hit Volume",Flag="HitVol",Min=0,Max=20,Value=18,Precise=2,Textbox=true,Format=function(v) return string.format("%.2fx",v/20) end,Callback=function(v) S.hitSoundVol=v/20 end}))
    addUI("KillSnd", WpnS:AddToggle({Name="Kill Sound",Flag="KillSnd",Value=false,Callback=function(v) S.killSoundOn=v; showNotification(v and "Kill Sound ON" or "Kill Sound OFF") end}))
    addUI("KillSndChoice",WpnS:AddDropdown({Name="Kill Sound",Flag="KillSndChoice",List=S.killSoundsList,Value="use your brain",Callback=function(v) S.killSoundId=S.killSoundIds[v] end}))
    addUI("KillVol", WpnS:AddSlider({Name="Kill Volume",Flag="KillVol",Min=0,Max=20,Value=20,Precise=2,Textbox=true,Format=function(v) return v.."x" end,Callback=function(v) S.killSoundVol=v end}))

    local HitMsgS = CT:CreateSection({Name="Custom Hit Message"})
    addUI("HitMsg",HitMsgS:AddTextbox({Name="Message (max 28)",Flag="HitMsg",Placeholder="e.g. MEOW",Callback=function(txt) S.hitMsg=txt:sub(1,28) end}))
    addUI("HitNotifOn",HitMsgS:AddToggle({Name="Hit Notifications",Flag="HitNotifOn",Value=false,Callback=function(v) S.hitNotifOn=v end}))

    CT:CreateSection({Name="Discord"}):AddLabel({Text="discord.gg/cTtSuya3Rb"})

    -- MOVEMENT TAB
    local MTB = Window:CreateTab({Name = "Movement"})
    local FlyS = MTB:CreateSection({Name="Flight"})
    addUI("FlyOn", FlyS:AddToggle({Name="Enable Flight",Flag="FlyOn",Value=false,Callback=function(v) S.flightOn=v; if v then startFlight() else stopFlight() end; showNotification(v and "Flight ON" or "Flight OFF") end}))
    addUI("FlySpd",FlyS:AddSlider({Name="Speed",Flag="FlySpd",Min=5,Max=600,Value=50,Textbox=true,Format=function(v) return v.." st/s" end,Callback=function(v) S.flightSpeed=v end}))

    local CfS = MTB:CreateSection({Name="CFrame Move", Side="Right"})
    addUI("CfOn", CfS:AddToggle({Name="Enable CFrame",Flag="CfOn",Value=false,Callback=function(v) S.cfOn=v; if v then startCF() else stopCF() end; showNotification(v and "CFrame ON" or "CFrame OFF") end}))
    addUI("CfSpd",CfS:AddSlider({Name="Speed",Flag="CfSpd",Min=10,Max=600,Value=50,Textbox=true,Format=function(v) return v.." st/s" end,Callback=function(v) S.cfSpeed=v end}))

    local NcS = MTB:CreateSection({Name="Noclip"})
    addUI("NoclipOn",NcS:AddToggle({Name="Noclip",Flag="NoclipOn",Value=false,Callback=function(v) S.noclipOn=v; showNotification(v and "Noclip ON" or "Noclip OFF") end}))

    local WSpS = MTB:CreateSection({Name="Walk & Jump", Side="Right"})
    local walkOn,walkSpd,jumpOn,jumpPow = false,16,false,50
    addUI("WalkOn", WSpS:AddToggle({Name="Custom Walkspeed",Flag="WalkOn",Value=false,Callback=function(v) walkOn=v; local h=LP.Character and LP.Character:FindFirstChildOfClass("Humanoid"); if h then h.WalkSpeed=v and walkSpd or 16 end end}))
    addUI("WalkSpd",WSpS:AddSlider({Name="Walk Speed",Flag="WalkSpd",Min=0,Max=600,Value=16,Textbox=true,Format=function(v) return "Spd "..v end,Callback=function(v) walkSpd=v; if walkOn then local h=LP.Character and LP.Character:FindFirstChildOfClass("Humanoid"); if h then h.WalkSpeed=v end end end}))
    addUI("JumpOn", WSpS:AddToggle({Name="Custom Jump",Flag="JumpOn",Value=false,Callback=function(v) jumpOn=v; local h=LP.Character and LP.Character:FindFirstChildOfClass("Humanoid"); if h then h.JumpPower=v and jumpPow or 50 end end}))
    addUI("JumpPow",WSpS:AddSlider({Name="Jump Power",Flag="JumpPow",Min=0,Max=600,Value=50,Textbox=true,Format=function(v) return "Pow "..v end,Callback=function(v) jumpPow=v; if jumpOn then local h=LP.Character and LP.Character:FindFirstChildOfClass("Humanoid"); if h then h.JumpPower=v end end end}))
    addUI("AlwaysJump", WSpS:AddToggle({Name="Always Jump",Flag="AlwaysJump",Value=false,Callback=function(v) S.alwaysJump=v end}))

    UIS.JumpRequest:Connect(function()
        if not S.alwaysJump then return end
        local h = LP.Character and LP.Character:FindFirstChildOfClass("Humanoid")
        if h then h:ChangeState(Enum.HumanoidStateType.Jumping) end
    end)

    local StrafeS = MTB:CreateSection({Name="Auto Strafe"})
    addUI("AutoStrafe",StrafeS:AddToggle({Name="Enable Auto Strafe",Flag="AutoStrafe",Value=false,Callback=function(v) S.strafeOn=v; updateStrafe(); showNotification(v and "Auto Strafe ON" or "Auto Strafe OFF") end}))
    addUI("StrafeKey", StrafeS:AddKeybind({Name="Toggle Key",Flag="StrafeKey",Value="R",Callback=function(k) S.strafeKey=Enum.KeyCode[k] end}))
    addUI("StrafeRMB", StrafeS:AddToggle({Name="Use RMB Instead",Flag="StrafeRMB",Value=false,Callback=function(v) S.strafeRMB=v end}))
    addUI("StrafeDist",StrafeS:AddSlider({Name="Strafe Distance",Flag="StrafeDist",Min=1,Max=10,Value=3,Textbox=true,Format=function(v) return v.." studs" end,Callback=function(v) S.strafeDist=v end}))

    local GravS = MTB:CreateSection({Name="Gravity", Side="Right"})
    addUI("Gravity",GravS:AddSlider({Name="Gravity",Flag="Gravity",Min=0,Max=196,Value=196,Precise=1,Textbox=true,Format=function(v) return v.." studs/s" end,Callback=function(v) workspace.Gravity=v end}))

    MTB:CreateSection({Name="Discord"}):AddLabel({Text="discord.gg/cTtSuya3Rb"})

    -- MISC TAB
    local MSC = Window:CreateTab({Name = "Misc"})
    local LgtS = MSC:CreateSection({Name="Lighting"})
    local skies = {Default=14,Dawn=5.5,Dusk=18,Night=0,Midday=12}
    addUI("Sky", LgtS:AddDropdown({Name="Sky Preset",Flag="Sky",List={"Default","Dawn","Dusk","Night","Midday"},Value="Night",Callback=function(v) S.skyTime=skies[v] end}))
    addUI("ClkTime",LgtS:AddSlider({Name="Clock Time",Flag="ClkTime",Min=0,Max=24,Value=0,Precise=1,Textbox=true,Format=function(v) return "Time "..v end,Callback=function(v) S.skyTime=v end}))
    addUI("LockSky",LgtS:AddToggle({Name="Lock Sky",Flag="LockSky",Value=false,Callback=function(v) S.lockSky=v end}))
    addUI("FB", LgtS:AddToggle({Name="Fullbright",Flag="FB",Value=false,Callback=function(v) Lighting.Brightness=v and 10 or 2; Lighting.FogEnd=v and 1e6 or 100000 end}))

    local MscS = MSC:CreateSection({Name="Misc"})
    addUI("FOV", MscS:AddSlider({Name="Field of View",Flag="FOV",Min=1,Max=179,Value=179,Textbox=true,Format=function(v) return "FOV "..v end,Callback=function(v) S.forcedFOV=v end}))
    addUI("AFK", MscS:AddToggle({Name="Anti AFK",Flag="AFK",Value=false,Callback=function(v) if v then LP.Idled:Connect(function() local vu=Instance.new("VirtualUser"); vu:CaptureController(); vu:ClickButton2(Vector2.new()); vu:Destroy() end) end end}))
    MscS:AddButton({Name="Respawn",Callback=function() local h=LP.Character and LP.Character:FindFirstChildOfClass("Humanoid"); if h then h.Health=0 end end})
    MscS:AddButton({Name="Print Players",Callback=function() for i,p in ipairs(Players:GetPlayers()) do print(i,p.Name) end end})
    addUI("TextureNuker",MscS:AddToggle({Name="Texture Nuker",Flag="TextureNuker",Value=false,Callback=function(v) S.textureNuker=v end}))
    addUI("MotionBlur", MscS:AddToggle({Name="Motion Blur",Flag="MotionBlur",Value=false,Callback=function(v) S.motionBlurEnabled=v; if not v and motionBlurEffect then motionBlurEffect:Destroy(); motionBlurEffect=nil end end}))
    addUI("MotionStr", MscS:AddSlider({Name="Motion Blur Strength",Flag="MotionStr",Min=1,Max=20,Value=4,Textbox=true,Callback=function(v) S.motionBlurStrength=v end}))
    MscS:AddButton({Name="Chat Spammer",Callback=function() sendChat("KittyHub V2 On Top!!!"); task.wait(0.2); sendChat("KittyHub V2 On Top!!!") end})
    addUI("KillChat", MscS:AddToggle({Name="Kill Chat Spam",Flag="KillChat",Value=false,Callback=function(v) S.killChatSpam=v end}))
    addUI("Snowfall", MscS:AddToggle({Name="Snowfall Effect",Flag="Snowfall",Value=false,Callback=function(v) S.snowEnabled=v; if v then startSnow() else stopSnow() end end}))
    MscS:AddButton({Name="End Match (Void)",Callback=function()
        local char=LP.Character; if not char then return end
        local root=char:FindFirstChild("HumanoidRootPart"); if not root then return end
        root.CFrame=CFrame.new(0,-200,0)
        local bv=Instance.new("BodyVelocity"); bv.MaxForce=Vector3.new(math.huge,math.huge,math.huge)
        bv.Velocity=Vector3.new(0,-500,0)+Vector3.new(math.random(-200,200),0,math.random(-200,200))
        bv.Parent=root; game:GetService("Debris"):AddItem(bv,3)
        showNotification("Sent to the void!")
    end})

    local ExtendS = MSC:CreateSection({Name="Extend Slide", Side="Right"})
    addUI("ExtendSlideEnabled", ExtendS:AddToggle({Name="Enable Extend Slide",Flag="ExtendSlideEnabled",Value=false,Callback=function(v) S.extendSlideEnabled=v; showNotification(v and "Extend Slide ON (Press C)" or "Extend Slide OFF") end}))
    addUI("ExtendSlideKey", ExtendS:AddKeybind({Name="Activation Key",Flag="ExtendSlideKey",Value="C",Callback=function(k) S.extendSlideKey=Enum.KeyCode[k] end}))
    addUI("ExtendSlideSpeed", ExtendS:AddSlider({Name="Boost Speed",Flag="ExtendSlideSpeed",Min=5,Max=50,Value=18,Textbox=true,Format=function(v) return v.." studs/s" end,Callback=function(v) S.extendSlideSpeed=v end}))
    addUI("ExtendSlideDuration", ExtendS:AddSlider({Name="Duration",Flag="ExtendSlideDuration",Min=1,Max=10,Value=3,Textbox=true,Format=function(v) return v.." sec" end,Callback=function(v) S.extendSlideDuration=v end}))

    local ResetS = MSC:CreateSection({Name="Reset Visuals"})
    ResetS:AddButton({Name="Reset All Visuals",Callback=function()
        S.glassWalls=false; restoreGlass()
        S.tintEnabled=false; updateTint()
        S.darkTextures=false; restoreDarkTextures()
        for _, child in ipairs(Lighting:GetChildren()) do
            if child:IsA("Sky") or child:IsA("ColorCorrectionEffect") then child:Destroy() end
        end
        Lighting.Brightness=2; Lighting.ClockTime=14; Lighting.FogEnd=100000
        Lighting.FogStart=0; Lighting.GlobalShadows=true
        Lighting.Ambient=Color3.fromRGB(70,70,70)
        Lighting.OutdoorAmbient=Color3.fromRGB(140,140,140)
        Lighting.ShadowSoftness=0.2; S.lockSky=false
        showNotification("All Visuals Reset")
    end})

    local CrossS = MSC:CreateSection({Name="Crosshair"})
    addUI("CrosshairOn", CrossS:AddToggle({Name="Enable Crosshair",Flag="CrosshairOn",Value=false,Callback=function(v) S.crossEnabled=v; updateCrosshair() end}))
    addUI("CrossStyle", CrossS:AddDropdown({Name="Style",Flag="CrossStyle",List={"dot","cross","T-Shape","Inverted T-Shape"},Value="cross",Callback=function(v) S.crossType=v; updateCrosshair() end}))
    addUI("CrossSize", CrossS:AddSlider({Name="Size",Flag="CrossSize",Min=4,Max=30,Value=21,Textbox=true,Format=function(v) return v.."px" end,Callback=function(v) S.crossSize=v; updateCrosshair() end}))
    addUI("CrossThick", CrossS:AddSlider({Name="Thickness",Flag="CrossThick",Min=1,Max=5,Value=3,Textbox=true,Format=function(v) return v.."px" end,Callback=function(v) S.crossThick=v; updateCrosshair() end}))
    addUI("CrossColor", CrossS:AddColorpicker({Name="Color",Flag="CrossColor",Color=S.crossColor,Callback=function(v) S.crossColor=v; updateCrosshair() end}))
    addUI("CrossSpin", CrossS:AddToggle({Name="Spin",Flag="CrossSpin",Value=false,Callback=function(v) S.crossSpin=v; if not v then S.crossAngle=0; updateCrosshair() end end}))
    addUI("CrossSpinSpd",CrossS:AddSlider({Name="Spin Speed",Flag="CrossSpinSpd",Min=5,Max=1000,Value=220,Precise=1,Textbox=true,Format=function(v) return string.format("%.1fx",v/100) end,Callback=function(v) S.crossSpinSpd=v/100 end}))
    addUI("WatermarkOn", CrossS:AddToggle({Name="Show Watermark",Flag="WatermarkOn",Value=false,Callback=function(v) S.watermarkOn=v; updateCrosshair(); showNotification(v and "Watermark ON" or "Watermark OFF") end}))

    local WideS = MSC:CreateSection({Name="Wide Camera", Side="Right"})
    addUI("WideCamera", WideS:AddToggle({Name="Enable Third Person",Flag="WideCamera",Value=false,Callback=function(v)
        S.wideCamera = v
        if v then updateWideCamera() else stopWideCamera() end
    end}))
    addUI("WideIntensity", WideS:AddSlider({Name="Stretch Intensity",Flag="WideIntensity",Min=1,Max=20,Value=3.5,Precise=1,Textbox=true,Callback=function(v)
        S.wideIntensity = v
        if S.wideCamera then updateWideCamera() end
    end}))

    local ProfS = MSC:CreateSection({Name="Quick Profiles"})
    ProfS:AddButton({Name="Load Auto", Callback=function() applyProfile("Auto") end})
    ProfS:AddButton({Name="Load Rage", Callback=function() applyProfile("Rage") end})
    ProfS:AddButton({Name="Load Legit", Callback=function() applyProfile("Legit") end})
    ProfS:AddButton({Name="Load Semi-Rage", Callback=function() applyProfile("Semi-Rage") end})
    ProfS:AddButton({Name="Load Advanced Rage", Callback=function() applyProfile("Advanced Rage") end})

    MSC:CreateSection({Name="Discord"}):AddLabel({Text="discord.gg/cTtSuya3Rb"})

    -- HVH TAB (with Silent Aim)
    local HVH = Window:CreateTab({Name = "HVH"})
    local BsS = HVH:CreateSection({Name="Auto Backstab"})
    addUI("AutoBackstab",BsS:AddToggle({Name="Enable",Flag="AutoBackstab",Value=false,Callback=function(v) S.autoBackstab=v; if v then backstabActive=true; if not backstabLoopRunning then task.spawn(autoBackstabLoop) end else backstabActive=false end; showNotification(v and "Auto Backstab ON" or "Auto Backstab OFF") end}))
    addUI("BackstabKey", BsS:AddKeybind({Name="Toggle Key",Flag="BackstabKey",Value="B",Callback=function(k) S.backstabBind=Enum.KeyCode[k] end}))

    local FlwS = HVH:CreateSection({Name="Follow"})
    addUI("Follow", FlwS:AddToggle({Name="Enable Follow",Flag="Follow",Value=false,Callback=function(v) S.followTarget=v; showNotification(v and "Follow ON" or "Follow OFF") end}))
    addUI("FollowKey",FlwS:AddKeybind({Name="Toggle Key",Flag="FollowKey",Value="L",Callback=function(k) S.followBind=Enum.KeyCode[k] end}))

    local OrbS = HVH:CreateSection({Name="Orbit", Side="Right"})
    addUI("Orbit", OrbS:AddToggle({Name="Enable Orbit",Flag="Orbit",Value=false,Callback=function(v) S.orbitActive=v; showNotification(v and "Orbit ON" or "Orbit OFF") end}))
    addUI("OrbitHeight",OrbS:AddSlider({Name="Height Above Head",Flag="OrbitHeight",Min=2,Max=15,Value=4,Textbox=true,Format=function(v) return v.." studs" end,Callback=function(v) S.orbitHeight=v end}))
    addUI("OrbitKey", OrbS:AddKeybind({Name="Orbit Toggle Key",Flag="OrbitKey",Value="O",Callback=function(k) S.orbitBind=Enum.KeyCode[k] end}))

    local KnfS = HVH:CreateSection({Name="Knife Settings"})
    addUI("KnifeKey",KnfS:AddKeybind({Name="Knife Slot Key",Flag="KnifeKey",Value="Three",Callback=function(k) S.knifeKey=Enum.KeyCode[k] end}))

    local HumS = HVH:CreateSection({Name="Humanization", Side="Right"})
    addUI("Jitter", HumS:AddToggle({Name="Mouse Jitter",Flag="Jitter",Value=false,Callback=function(v) S.jitter=v end}))
    addUI("JitterStr",HumS:AddSlider({Name="Jitter Strength",Flag="JitterStr",Min=0,Max=40,Value=12,Textbox=true,Format=function(v) return v.." px" end,Callback=function(v) S.jitterStr=v end}))

    local AKS = HVH:CreateSection({Name="Anti Kicia"})
    addUI("AntiKicia", AKS:AddToggle({Name="Enable Anti Kicia",Flag="AntiKicia",Value=false,Callback=function(v) S.antiKicia=v; if v then S.noclipOn=true end; showNotification(v and "ANTI KICIA ON" or "ANTI KICIA OFF") end}))
    addUI("AntiKiciaKey",AKS:AddKeybind({Name="Toggle Key",Flag="AntiKiciaKey",Value="K",Callback=function(k) S.antiKiciaBind=Enum.KeyCode[k] end}))
    addUI("AKDist", AKS:AddSlider({Name="Distance Behind",Flag="AKDist",Min=1,Max=20,Value=5,Textbox=true,Format=function(v) return v.." studs" end,Callback=function(v) S.akDist=v end}))
    addUI("AKHeight", AKS:AddSlider({Name="Height Offset",Flag="AKHeight",Min=-5,Max=10,Value=0,Textbox=true,Format=function(v) return v.." studs" end,Callback=function(v) S.akHeight=v end}))
    addUI("AKChaos", AKS:AddSlider({Name="Chaos / Speed",Flag="AKChaos",Min=1,Max=30,Value=12,Textbox=true,Callback=function(v) S.akChaos=v end}))

    local FloorS = HVH:CreateSection({Name="Floorbang"})
    addUI("Floorbang",FloorS:AddToggle({Name="Enable Floorbang",Flag="Floorbang",Value=false,Callback=function(v)
        S.floorbang=v
        if not v and floorbangLmbHeld then
            pcall(function() VirtualUser:Button1Up(Vector2.new()) end)
            floorbangLmbHeld=false
        end
        showNotification(v and "Floorbang ON" or "Floorbang OFF")
    end}))
    addUI("FloorbangKey", FloorS:AddKeybind({Name="Toggle Key",Flag="FloorbangKey",Value="U",Callback=function(k) S.floorbangKey=Enum.KeyCode[k] end}))
    addUI("FloorbangHeight", FloorS:AddSlider({Name="Height Offset",Flag="FloorbangHeight",Min=-15,Max=5,Value=-5,Textbox=true,Format=function(v) return v.." studs" end,Callback=function(v) S.floorbangHeightOffset=v end}))
    addUI("FloorbangAimAggr", FloorS:AddSlider({Name="Aimbot Aggression",Flag="FloorbangAimAggr",Min=50,Max=100,Value=100,Textbox=true,Format=function(v) return v.."%" end,Callback=function(v) S.floorbangAimAggr=v end}))
    addUI("FloorbangSpin", FloorS:AddToggle({Name="Spin Character",Flag="FloorbangSpin",Value=false,Callback=function(v) S.floorbangSpinEnabled=v; showNotification(v and "Floorbang Spin ON" or "Floorbang Spin OFF") end}))
    addUI("FloorbangSpinSpd",FloorS:AddSlider({Name="Spin Speed",Flag="FloorbangSpinSpd",Min=1,Max=30,Value=5,Textbox=true,Format=function(v) return v.."x" end,Callback=function(v) S.floorbangSpinSpeed=v end}))

    local AntiHitS = HVH:CreateSection({Name="Anti Hit - Orbit", Side="Right"})
    addUI("AntiHit", AntiHitS:AddToggle({Name="Enable Anti Hit",Flag="AntiHit",Value=false,Callback=function(v) S.antiHit=v; if v then updateAntiHit() else if antiHitConn then antiHitConn:Disconnect(); antiHitConn=nil end end; showNotification(v and "Anti Hit ON" or "Anti Hit OFF") end}))
    addUI("AntiHitKey", AntiHitS:AddKeybind({Name="Toggle Key",Flag="AntiHitKey",Value="J",Callback=function(k) S.antiHitKey=Enum.KeyCode[k] end}))
    addUI("AntiHitSpeed", AntiHitS:AddSlider({Name="Orbit Speed",Flag="AntiHitSpeed",Min=1,Max=30,Value=10,Textbox=true,Callback=function(v) S.antiHitSpeed=v; if S.antiHit then updateAntiHit() end end}))
    addUI("AntiHitRadius", AntiHitS:AddSlider({Name="Orbit Radius",Flag="AntiHitRadius",Min=1,Max=10,Value=3,Textbox=true,Format=function(v) return v.." studs" end,Callback=function(v) S.antiHitRadius=v; if S.antiHit then updateAntiHit() end end}))

    -- Silent Aim Section
    local SilentS = HVH:CreateSection({Name="Silent Aim"})
    addUI("SilentAimEnabled", SilentS:AddToggle({Name="Enable Silent Aim",Flag="SilentAimEnabled",Value=false,Callback=function(v)
        S.silentAimEnabled = v
        if v then
            startSilentAim()
            showNotification("Silent Aim ON")
        else
            stopSilentAim()
            showNotification("Silent Aim OFF")
        end
    end}))
    addUI("SilentAimFOV", SilentS:AddSlider({Name="FOV Radius",Flag="SilentAimFOV",Min=50,Max=300,Value=120,Textbox=true,Format=function(v) return v.." px" end,Callback=function(v)
        S.silentAimFOV = v
        if S.silentAimEnabled and silentAimGui then
            local circle = silentAimGui:FindFirstChild("FOV_Ring")
            if circle then circle.Size = UDim2.new(0, v*2, 0, v*2) end
        end
    end}))
    addUI("SilentAimTransparency", SilentS:AddSlider({Name="Transparency",Flag="SilentAimTransparency",Min=0,Max=100,Value=50,Textbox=true,Format=function(v) return v.."%" end,Callback=function(v)
        S.silentAimTransparency = v/100
        if S.silentAimEnabled and silentAimGui then
            local stroke = silentAimGui:FindFirstChild("FOV_Ring"):FindFirstChildOfClass("UIStroke")
            local line = silentAimGui:FindFirstChild("Snapline")
            if stroke then stroke.Transparency = v/100 end
            if line then line.BackgroundTransparency = v/100 end
        end
    end}))
    addUI("SilentAimColor", SilentS:AddColorpicker({Name="FOV Color",Flag="SilentAimColor",Color=S.silentAimColor,Callback=function(v)
        S.silentAimColor = v
        if S.silentAimEnabled and silentAimGui then
            local stroke = silentAimGui:FindFirstChild("FOV_Ring"):FindFirstChildOfClass("UIStroke")
            local line = silentAimGui:FindFirstChild("Snapline")
            if stroke then stroke.Color = v end
            if line then line.BackgroundColor3 = v end
        end
    end}))

    HVH:CreateSection({Name="Discord"}):AddLabel({Text="discord.gg/cTtSuya3Rb"})

    -- WRAPS TAB (includes Skin Changer Loader & Skybox)
    local WT = Window:CreateTab({Name = "Wraps"})
    local WrapInfoS = WT:CreateSection({Name="Gun Wraps"})
    local WRAP_NAME_LIST = {"None"}
    for _, w in ipairs(WRAP_PRESETS) do table.insert(WRAP_NAME_LIST, w.name) end
    local WrapPresetS = WT:CreateSection({Name="Select Gun Wrap", Side="Right"})
    addUI("WrapPresetSelect",WrapPresetS:AddDropdown({
        Name="Wrap Preset",Flag="WrapPresetSelect",List=WRAP_NAME_LIST,Value="None",
        Callback=function(v)
            if v=="None" then
                currentWrap=nil; S.wrapName=""; S.wrapEnabled=false
                wrapActiveTextures={}; wrapActiveParts={}
                showNotification("Wrap cleared")
            else
                local preset=WRAP_PRESET_MAP[v]
                if preset then
                    currentWrap=preset; S.wrapName=v
                    if S.wrapEnabled then wrapApplyAll() end
                    showNotification("Wrap selected: "..v)
                end
            end
        end
    }))
    addUI("WrapEnabled",WrapPresetS:AddToggle({
        Name="Enable Gun Wrap",Flag="WrapEnabled",Value=false,
        Callback=function(v)
            S.wrapEnabled=v
            if v then
                if currentWrap then wrapApplyAll(); showNotification("Wrap ON: "..S.wrapName)
                else showNotification("Select a wrap preset first!"); S.wrapEnabled=false end
            else
                wrapActiveTextures={}; wrapActiveParts={}
                showNotification("Wrap disabled")
            end
        end
    }))
    WrapPresetS:AddButton({Name="Re-Apply Now",Callback=function()
        if S.wrapEnabled and currentWrap then wrapApplyAll(); showNotification("Wrap re-applied: "..S.wrapName)
        else showNotification("Enable wrap and select a preset first") end
    end})
    WrapPresetS:AddButton({Name="Clear Wrap",Callback=function()
        S.wrapEnabled=false; S.wrapName=""; currentWrap=nil
        wrapActiveTextures={}; wrapActiveParts={}
        showNotification("Wrap cleared")
    end})

    local SkinLoaderS = WT:CreateSection({Name="Skin Changer Loader"})
    SkinLoaderS:AddButton({Name="Load Skin Changer", Callback=function()
        loadSkinChangerScript()
    end})

    local SkyboxWrapS = WT:CreateSection({Name="Skybox Changer"})
    local skyList = {"None"}
    for name,_ in pairs(SKYBOX_IDS) do table.insert(skyList, name) end
    addUI("SkyboxSelect",SkyboxWrapS:AddDropdown({Name="Skybox",Flag="SkyboxSelect",List=skyList,Value="None",Callback=function(v)
        if v == "None" then
            S.skyboxEnabled = false
            S.skyboxId = ""
            setSkybox("")
        else
            S.skyboxEnabled = true
            S.skyboxId = SKYBOX_IDS[v]
            setSkybox(S.skyboxId)
        end
        showNotification(v == "None" and "Skybox removed" or "Skybox: "..v)
    end}))

    WT:CreateSection({Name="Discord"}):AddLabel({Text="discord.gg/cTtSuya3Rb"})

    print("[KittyHub V2] Rebuild complete. All team checks removed, all features working.")
end

-- STARTUP GATE
if isUserValidated() then
    loadMainScript()
else
    showKeyEntry("Enter your KittyHub V2 key to continue.")
end
