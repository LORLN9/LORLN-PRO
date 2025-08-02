-- ✅ مضاد تنفيذات خطيرة + حذف أدوات التخريب بدون تعليق

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LP = Players.LocalPlayer
if not RunService:IsClient() then return end

local blockList = {
    "log", "logs", "clogs", "nv", "re", "uncmdbar2", "mute",
    "res", "kill", "fling", "giant", "size", "copy", "anticheat"
}

-- تأخير الفحص حتى ما يسبب تعليق
task.delay(2.5, function()
    pcall(function()
        for _, func in pairs(getgc(true)) do
            if typeof(func) == "function" and islclosure(func) and not isexecutorclosure(func) then
                local env = getfenv(func)
                if env and env.script then
                    local name = tostring(env.script):lower()
                    for _, word in pairs(blockList) do
                        if name:find(word) and not name:find("chat") then
                            hookfunction(func, function() return nil end)
                            break
                        end
                    end
                end
            end
        end
    end)
end)

-- حذف أي جسم أو سكربت ميوت أو تخريب من جسم اللاعب
task.spawn(function()
    while true do
        pcall(function()
            local char = LP.Character or LP.CharacterAdded:Wait()
            for _, part in pairs(char:GetDescendants()) do
                if part:IsA("BodyMover") or part:IsA("BodyVelocity") or part:IsA("BodyGyro")
                or part:IsA("AlignPosition") or part:IsA("AlignOrientation")
                or part.Name:lower():match("mute") then
                    part:Destroy()
                end
            end
        end)
        task.wait(2)
    end
end)
