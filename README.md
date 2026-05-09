-- ========================================
-- GRAVITY SUAVE + TELA ESTICADA + FPS BOOST ULTRA
-- COM SISTEMA DE KEY
-- ========================================

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- ==================== SISTEMA DE KEY ====================
local Key = "davizin-SCRIPTS-71hdnhauw7"  -- ← Mude aqui a key se quiser

local CorrectKey = false

local KeyWindow = Rayfield:CreateWindow({
    Name = "🔑 Sistema de Key",
    LoadingTitle = "Verificação de Key",
})

local KeyTab = KeyWindow:CreateTab("Key System", 4483362458)

KeyTab:CreateLabel("Digite a Key para continuar")

local KeyInput = KeyTab:CreateInput({
    Name = "Digite a Key",
    PlaceholderText = "Insira a key aqui...",
    RemoveTextAfterFocusLost = false,
    Callback = function(Text)
        if Text == Key then
            CorrectKey = true
            Rayfield:Notify({
                Title = "✅ Key Correta!",
                Content = "Acesso Liberado",
                Duration = 5,
            })
            loadMainScript()
        else
            Rayfield:Notify({
                Title = "❌ Key Errada",
                Content = "Tente novamente",
                Duration = 4,
            })
        end
    end,
})

KeyTab:CreateButton({
    Name = "Confirmar Key",
    Callback = function()
        -- Já é verificado no input
    end,
})

-- ==================== SCRIPT PRINCIPAL ====================
function loadMainScript()
    if not CorrectKey then return end

    local Window = Rayfield:CreateWindow({
        Name = "Gravity + FOV + FPS Ultra",
        LoadingTitle = "Carregando Menu...",
    })

    -- ==================== TAB GRAVIDADE ====================
    local GravityTab = Window:CreateTab("🌍 Gravidade Suave", 4483362458)

    local currentGravity = workspace.Gravity
    local defaultGravity = workspace.Gravity
    local antiResetEnabled = false
    local gravityConnection = nil

    GravityTab:CreateSlider({
        Name = "Valor da Gravidade",
        Range = {0, 1000},
        Increment = 1,
        CurrentValue = currentGravity,
        Callback = function(Value)
            currentGravity = Value
            workspace.Gravity = Value
        end,
    })

    GravityTab:CreateToggle({
        Name = "🔒 Ativar Gravidade Suave (Anti-Reset)",
        CurrentValue = false,
        Callback = function(Value)
            antiResetEnabled = Value
            if Value then
                if gravityConnection then gravityConnection:Disconnect() end
                gravityConnection = game:GetService("RunService").Heartbeat:Connect(function()
                    if workspace.Gravity ~= currentGravity then
                        workspace.Gravity = currentGravity
                    end
                end)
            else
                if gravityConnection then gravityConnection:Disconnect() gravityConnection = nil end
            end
        end,
    })

    GravityTab:CreateButton({Name = "🌕 Lua (50)", Callback = function() currentGravity = 50; workspace.Gravity = 50 end})
    GravityTab:CreateButton({Name = "🪐 Baixa (120)", Callback = function() currentGravity = 120; workspace.Gravity = 120 end})
    GravityTab:CreateButton({Name = "🌍 Normal (196.2)", Callback = function()
        antiResetEnabled = false
        if gravityConnection then gravityConnection:Disconnect() gravityConnection = nil end
        currentGravity = defaultGravity
        workspace.Gravity = defaultGravity
    end})
    GravityTab:CreateButton({Name = "🚀 Alta (350)", Callback = function() currentGravity = 350; workspace.Gravity = 350 end})

    -- ==================== TAB TELA ESTICADA ====================
    local CameraTab = Window:CreateTab("📺 Tela Esticada + FOV", 4483362458)

    local fovEnabled = false
    local fovConnection = nil
    local originalFOV = workspace.CurrentCamera.FieldOfView
    local currentFOV = 115
    local stretchIntensity = 1.48

    local function ApplyFOV()
        if not fovEnabled then return end
        local cam = workspace.CurrentCamera
        if cam then
            cam.FieldOfView = currentFOV
            local size = cam.ViewportSize
            cam.ViewportSize = Vector2.new(size.X * stretchIntensity, size.Y)
        end
    end

    CameraTab:CreateToggle({
        Name = "🔄 Ativar Tela Esticada + FOV",
        CurrentValue = false,
        Callback = function(Value)
            fovEnabled = Value
            if Value then
                originalFOV = workspace.CurrentCamera.FieldOfView
                if fovConnection then fovConnection:Disconnect() end
                fovConnection = game:GetService("RunService").RenderStepped:Connect(ApplyFOV)
            else
                if fovConnection then fovConnection:Disconnect() fovConnection = nil end
                workspace.CurrentCamera.FieldOfView = originalFOV
            end
        end,
    })

    CameraTab:CreateSlider({Name = "FOV", Range = {70, 145}, Increment = 1, CurrentValue = currentFOV, Callback = function(Value) currentFOV = Value end})
    CameraTab:CreateSlider({Name = "Intensidade Stretch", Range = {1.0, 2.0}, Increment = 0.01, CurrentValue = stretchIntensity, Callback = function(Value) stretchIntensity = Value end})

    -- ==================== TAB FPS BOOST ====================
    local BoostTab = Window:CreateTab("⚡ FPS Boost", 4483362458)

    local function EnableUltraFPSBoost()
        settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
        local Lighting = game:GetService("Lighting")
        Lighting.GlobalShadows = false
        Lighting.FogEnd = 999999
        Lighting.Brightness = 2

        for _, v in pairs(workspace:GetDescendants()) do
            if v:IsA("BasePart") then
                v.Material = Enum.Material.Plastic
            elseif v:IsA("ParticleEmitter") or v:IsA("Trail") or v:IsA("Smoke") or v:IsA("Fire") then
                v.Enabled = false
            elseif v:IsA("Decal") or v:IsA("Texture") then
                v.Transparency = 1
            end
        end
    end

    BoostTab:CreateButton({
        Name = "🔥 FPS BOOST ULTRA (Máximo)",
        Callback = function()
            EnableUltraFPSBoost()
        end,
    })

    BoostTab:CreateButton({
        Name = "🔄 Resetar Gráficos",
        Callback = function()
            settings().Rendering.QualityLevel = Enum.QualityLevel.Automatic
            game:GetService("Lighting").GlobalShadows = true
        end,
    })

    Rayfield:Notify({
        Title = "✅ Script Carregado com Sucesso!",
        Content = "Key verificada • Bom uso!",
        Duration = 8,
    })
end

print("🔑 Sistema de Key carregado! Digite a key: " .. Key)
