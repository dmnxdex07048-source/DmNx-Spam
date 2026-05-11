--[[
    DmNx Spam
    Owner: DmNx_Ji
    Status: VIP Edition
]]

---------------- SERVICES ----------------
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TextChatService = game:GetService("TextChatService")

local lp = Players.LocalPlayer
local displayName = lp.DisplayName

---------------- STATE ----------------
local active = false
local index = 1
local waitTime = 0
local iyLoaded = false
local oxrLoaded = false
local gazeLoaded = false
local redzLoaded = false

---------------- PHRASES ----------------
local phrases = {
    "DmNx Hub On Top!", "🅣🅔🅡🅘 🅖@@@ 🅕🅐🅡 🅓🅤🅝🅖🅐", "Ｔｅｒａ 𝓑𝓪𝓪𝓹 𝕜𝕒 🅻",
    "ᴛᴇʀɪ ᴍᴜᴍᴇ ᴍᴏᴏᴛ ᴅᴜɴɢᴀ", "🅃🄴🅁🄸 🄱🄷🄴🄷🄴🄽 🄺🄰 🄻🄰🅆", "DmNx PAPA BOL" , "ʟᴀᴅʟᴇᴇᴇ ᴍᴇᴏᴡ ɢʜᴏᴘ ɢʜᴏᴘ ɢʜᴏᴘ", "ℍ𝕒𝕥 𝕥𝕖𝕣𝕚 𝕓𝕜𝕔"
}

---------------- CHAT ANNOUNCEMENT (Like Screenshot) ----------------
local function autoAnnounce(msg)
    pcall(function()
        local formattedMsg = "⚠️⚜️ " .. msg .. " ⚠️⚜️"
        if TextChatService.ChatVersion == Enum.ChatVersion.TextChatService then
            TextChatService.TextChannels.RBXGeneral:SendAsync(formattedMsg)
        else
            ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer(formattedMsg, "All")
        end
    end)
end

---------------- UI CLEANUP ----------------
pcall(function()
    if game.CoreGui:FindFirstChild("DmNx_Spam") then
        game.CoreGui.DmNx_Spam:Destroy()
    end
end)

---------------- UI SETUP ----------------
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "DmNx_Spam"
gui.ResetOnSpawn = false

-- CIRCLE TOGGLE (With Lion Logo)
local toggleBtn = Instance.new("ImageButton", gui)
toggleBtn.Size = UDim2.fromOffset(65, 65)
toggleBtn.Position = UDim2.new(0.02, 0, 0.4, 0) -- Side position
toggleBtn.Image = "rbxassetid://1850623297" 
toggleBtn.BackgroundColor3 = Color3.fromRGB(212, 175, 55)
toggleBtn.BorderSizePixel = 0
local toggleCorner = Instance.new("UICorner", toggleBtn)
toggleCorner.CornerRadius = UDim.new(1, 0)

-- MAIN FRAME
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.fromScale(0.36, 0.75)
frame.Position = UDim2.fromScale(0.32, 0.15)
frame.BackgroundColor3 = Color3.fromRGB(15, 0, 30)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
Instance.new("UICorner", frame).CornerRadius = UDim.new(0,16)

-- TITLE
local header = Instance.new("TextLabel", frame)
header.Size = UDim2.fromScale(1, 0.1)
header.Text = "ᴅᴍɴx sᴘᴀᴍ"
header.BackgroundTransparency = 1
header.TextColor3 = Color3.fromRGB(212, 175, 55)
header.Font = Enum.Font.GothamBold
header.TextScaled = true

-- BIO / WELCOME MESSAGE
local bio = Instance.new("TextLabel", frame)
bio.Size = UDim2.fromScale(1, 0.05)
bio.Position = UDim2.fromScale(0, 0.1)
bio.Text = "ᴡᴇʟᴄᴏᴍᴇ " .. displayName
bio.BackgroundTransparency = 1
bio.TextColor3 = Color3.fromRGB(255, 255, 255)
bio.Font = Enum.Font.Gotham
bio.TextSize = 14

---------------- UI HELPERS ----------------
local function makeBtn(text, x, y, sx)
    local b = Instance.new("TextButton", frame)
    b.Size = UDim2.fromScale(sx or 0.42, 0.08)
    b.Position = UDim2.fromScale(x, y)
    b.Text = text
    b.BackgroundColor3 = Color3.fromRGB(100, 0, 180)
    b.TextColor3 = Color3.new(1,1,1)
    b.Font = Enum.Font.GothamBold
    b.TextScaled = true
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,12)
    return b
end

local input = Instance.new("TextBox", frame)
input.Size = UDim2.fromScale(0.9, 0.08)
input.Position = UDim2.fromScale(0.05, 0.18)
input.PlaceholderText = "Enter Spam Text..."
input.BackgroundColor3 = Color3.fromRGB(40, 0, 60)
input.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", input).CornerRadius = UDim.new(0,10)

---------------- BUTTONS ----------------
local startBtn = makeBtn("START", 0.05, 0.28)
local stopBtn  = makeBtn("STOP",  0.53, 0.28)
local iyBtn    = makeBtn("INFINITE YIELD", 0.05, 0.45)
local oxrBtn   = makeBtn("OXR MUSIC", 0.53, 0.45)
local gazeBtn  = makeBtn("GAZE EMOTES", 0.05, 0.55)
local redzBtn  = makeBtn("REDZ HUB", 0.53, 0.55)

---------------- LOGIC ----------------
startBtn.MouseButton1Click:Connect(function() active = true end)
stopBtn.MouseButton1Click:Connect(function() active = false end)

-- SPAM LOOP
task.spawn(function()
    while true do
        if active then
            autoAnnounce(input.Text .. " " .. phrases[index])
            index = index % #phrases + 1
            task.wait(waitTime + 0.5)
        end
        task.wait(0.1)
    end
end)

-- LOADERS WITH AUTO-CHAT
oxrBtn.MouseButton1Click:Connect(function()
    if not oxrLoaded then
        oxrLoaded = true
        autoAnnounce("OXR MUSIC Loaded")
        loadstring(game:HttpGet("https://rawscripts.net/raw/Brookhaven-RP-OXR-RECHAT-207620"))()
    end
end)

iyBtn.MouseButton1Click:Connect(function()
    if not iyLoaded then
        iyLoaded = true
        autoAnnounce("INFINITE YIELD Loaded")
        loadstring(game:HttpGet("https://raw.githubusercontent.com/4Peek/Fractal/refs/heads/main/iy_modded.lua"))()
    end
end)

-- TOGGLE
toggleBtn.MouseButton1Click:Connect(function()
    frame.Visible = not frame.Visible
end)

print("✅ ᴅᴍɴx sᴘᴀᴍ Active")
