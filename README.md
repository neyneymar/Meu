_G.AutoFarm = true
_G.AutoQuest = true
_G.AutoStats = false -- Ativa se quiser distribuir pontos automaticamente (perigoso!)
_G.Teleport = false -- Ativa se quiser teleportar para ilhas

while _G.AutoFarm do
    pcall(function()
        -- Vai até o NPC de quest
        if _G.AutoQuest then
            -- Código para pegar a quest automática (exemplo genérico)
            local args = {
                [1] = "StartQuest",
                [2] = "BanditQuest1", -- Troque pelo nome da quest desejada
                [3] = 1
            }
            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
        end
        
        -- Procura e ataca inimigos automaticamente
        for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
            if v.Name == "Bandit [Lv. 5]" and v.Humanoid.Health > 0 then
                repeat
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame + Vector3.new(0,5,0)
                    game:GetService("VirtualUser"):Button1Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
                    wait(0.2)
                until v.Humanoid.Health <= 0 or not _G.AutoFarm
            end
        end

        -- Auto Stat Points (Opcional)
        if _G.AutoStats then
            for i = 1, 100 do
                game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint","Melee")
            end
        end

        wait(1)
    end)
end

-- Para teleportar para uma ilha específica (exemplo, Middle Town)
if _G.Teleport then
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-260.417, 8.352, 2105.671) -- Coordenadas de Middle Town
end