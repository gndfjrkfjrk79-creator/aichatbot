<html>
<head>
    <title>AI Chatbot</title>
    <style>
        body {
            font-family: Arial;
            background: #1e3a8a;
            padding: 20px;
            margin: 0;
        }
        
        body::before {
            content: '6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            font-size: 300px;
            font-weight: bold;
            color: rgba(59, 130, 246, 0.15);
            z-index: 0;
            pointer-events: none;
            line-height: 1.2;
            letter-spacing: 80px;
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }
        
        h1 {
            color: white;
            text-align: center;
        }
        
        .chat {
            background: white;
            border-radius: 10px;
            padding: 20px;
        }
        
        #messages {
            height: 400px;
            overflow-y: scroll;
            border: 2px solid #3b82f6;
            padding: 10px;
            margin-bottom: 20px;
            background: #eff6ff;
        }
        
        .message {
            margin: 10px 0;
            padding: 10px;
            border-radius: 5px;
            white-space: pre-wrap;
        }
        
        .user {
            background: #2563eb;
            color: white;
            text-align: right;
        }
        
        .bot {
            background: white;
            border: 2px solid #93c5fd;
            color: #1e40af;
        }
        
        input {
            width: 70%;
            padding: 10px;
            border: 2px solid #3b82f6;
            border-radius: 5px;
            font-size: 16px;
        }
        
        button {
            width: 25%;
            padding: 10px;
            background: #2563eb;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
        }
        
        button:hover {
            background: #1d4ed8;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>AI Chatbot</h1>
        
        <div class="chat">
            <div id="messages"></div>
            
            <input type="text" id="input" placeholder="Ask me anything or ask a math problem...">
            <button onclick="send()">Send</button>
        </div>
    </div>

    <script>
        // MASSIVE KNOWLEDGE BASE
        var knowledgeBase = {
            // === ROBLOX STUDIO (PRIORITY) ===
            "roblox": "I can help you with Roblox Studio! I know about building, scripting in Lua, creating GUIs, game mechanics, tools, effects, and more. What specific aspect of Roblox development would you like to learn about?",
            
            "studio": "Roblox Studio is a free game development platform where you can create 3D games. It uses the Lua programming language for scripting. You can build worlds, script gameplay, design GUIs, and publish your games for millions of players. What would you like to create?",
            
            "start": "To get started with Roblox Studio:\n\n1. Download Roblox Studio for free from roblox.com\n2. Open it and click 'New'\n3. Choose 'Baseplate' for a blank canvas\n4. Press F5 to test your game anytime\n5. Save regularly with Ctrl+S\n\nThe main tools are Move (M), Scale (R), and Rotate (T). What would you like to build?",
            
            "part": "To add parts in Roblox Studio:\n\n1. Click the 'Part' button in the Home tab\n2. Choose your shape: Block, Sphere, Cylinder, or Wedge\n3. Use Move tool (M) to position it\n4. Use Scale tool (R) to resize it\n5. Use Rotate tool (T) to rotate it\n\nCopy with Ctrl+C, paste with Ctrl+V. Parts are the building blocks of your game!",
            
            "script": "To add scripts in Roblox Studio:\n\n1. Select a part in the Explorer\n2. Click '+' next to it\n3. Choose 'Script' (server-side) or 'LocalScript' (client-side)\n4. Write your Lua code\n5. Press F5 to test\n\nExample script:\nprint('Hello, Roblox!')\n\nScripts control game logic, player interactions, and more.",
            
            "lava": "To create a lava block:\n\n1. Insert a Part\n2. Set color to orange/red (BrickColor)\n3. Set Material to 'Neon' for glow\n4. Anchor it (check box in Properties)\n5. Add Script:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = 0\n    end\nend)\n\nOptionally add Fire object for flames!",

            "kill": "Kill block script:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = 0\n    end\nend)\n\nThis instantly kills any player who touches it. Make it red so players know it's dangerous!",

            "damage": "Damage block script:\n\nlocal damage = 10\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = hit.Parent.Humanoid.Health - damage\n    end\nend)\n\nChange the damage variable to adjust how much health it removes.",

            "heal": "Healing block script:\n\nlocal healAmount = 25\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = hit.Parent.Humanoid.Health + healAmount\n    end\nend)\n\nMake it green so players know it's safe!",

            "speed": "Speed boost script:\n\nlocal speedBoost = 50\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.WalkSpeed = hit.Parent.Humanoid.WalkSpeed + speedBoost\n        wait(5)\n        hit.Parent.Humanoid.WalkSpeed = hit.Parent.Humanoid.WalkSpeed - speedBoost\n    end\nend)\n\nGives a 5-second speed boost. Default speed is 16.",

            "jump": "Jump boost script:\n\nlocal jumpBoost = 100\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.JumpPower = hit.Parent.Humanoid.JumpPower + jumpBoost\n        wait(5)\n        hit.Parent.Humanoid.JumpPower = hit.Parent.Humanoid.JumpPower - jumpBoost\n    end\nend)\n\nDefault JumpPower is 50.",

            "door": "Clickable door script:\n\nlocal door = script.Parent\nlocal open = false\n\nscript.Parent.ClickDetector.MouseClick:Connect(function()\n    if open then\n        door.Transparency = 0\n        door.CanCollide = true\n        open = false\n    else\n        door.Transparency = 1\n        door.CanCollide = false\n        open = true\n    end\nend)\n\nAdd a ClickDetector to the door first!",

            "button": "Button script:\n\nscript.Parent.ClickDetector.MouseClick:Connect(function(player)\n    print(player.Name .. ' pressed the button!')\n    -- Add your code here\nend)\n\nRequires a ClickDetector in the part.",

            "teleport": "Teleporter script:\n\nlocal endPart = workspace.EndPart\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent:MoveTo(endPart.Position + Vector3.new(0, 3, 0))\n    end\nend)\n\nCreate two parts - entrance and 'EndPart' for the exit.",

            "checkpoint": "To make checkpoints:\n\n1. Insert SpawnLocation from Model tab\n2. Position where you want respawn point\n3. Change color (different for each checkpoint)\n4. Set Duration = 0\n5. Anchor it\n\nPlayers respawn at the last one they touched!",

            "moving platform": "Moving platform script:\n\nlocal platform = script.Parent\nlocal startPos = platform.Position\nlocal endPos = startPos + Vector3.new(20, 0, 0)\n\nwhile true do\n    platform.Position = endPos\n    wait(3)\n    platform.Position = startPos\n    wait(3)\nend\n\nFor smooth movement, use TweenService!",

            "spinning": "Spinning part script:\n\nwhile true do\n    wait(0.01)\n    script.Parent.CFrame = script.Parent.CFrame * CFrame.Angles(0, 0.1, 0)\nend\n\nAdjust 0.1 for rotation speed.",

            "disappearing": "Disappearing platform script:\n\nscript.Parent.Touched:Connect(function()\n    wait(0.5)\n    script.Parent.Transparency = 1\n    script.Parent.CanCollide = false\n    wait(3)\n    script.Parent.Transparency = 0\n    script.Parent.CanCollide = true\nend)",

            "coin": "Coin script:\n\nscript.Parent.Touched:Connect(function(hit)\n    local player = game.Players:GetPlayerFromCharacter(hit.Parent)\n    if player then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value + 1\n        script.Parent:Destroy()\n    end\nend)\n\nMake it yellow and spinning!",

            "gem": "Gem script:\n\nlocal gemValue = 10\n\nscript.Parent.Touched:Connect(function(hit)\n    local player = game.Players:GetPlayerFromCharacter(hit.Parent)\n    if player then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value + gemValue\n        script.Parent:Destroy()\n    end\nend)",

            "fire": "To add fire:\n\n1. Select part\n2. Insert → Fire\n3. Customize Size, Heat, Color in Properties\n\nGreat for lava, torches, campfires!",

            "light": "To add light:\n\n1. Select part\n2. Insert → PointLight\n3. Customize Brightness, Range, Color\n\nMakes parts glow!",

            "explosion": "Explosion script:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        local boom = Instance.new('Explosion')\n        boom.Position = script.Parent.Position\n        boom.Parent = workspace\n        script.Parent:Destroy()\n    end\nend)",

            "gui": "To make a GUI:\n\n1. StarterGui → Insert ScreenGui\n2. Add TextButton or TextLabel\n3. Customize in Properties\n4. For buttons, add LocalScript:\n\nscript.Parent.MouseButton1Click:Connect(function()\n    print('Clicked!')\nend)",

            "shop": "Shop GUI script:\n\nlocal button = script.Parent\nlocal price = 100\n\nbutton.MouseButton1Click:Connect(function()\n    local player = game.Players.LocalPlayer\n    if player.leaderstats.Coins.Value >= price then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value - price\n        print('Purchased!')\n    end\nend)",

            "leaderboard": "Leaderboard script (ServerScriptService):\n\ngame.Players.PlayerAdded:Connect(function(player)\n    local leaderstats = Instance.new('Folder')\n    leaderstats.Name = 'leaderstats'\n    leaderstats.Parent = player\n    \n    local coins = Instance.new('IntValue')\n    coins.Name = 'Coins'\n    coins.Value = 0\n    coins.Parent = leaderstats\nend)",

            "team": "To create teams:\n\n1. Insert Teams service\n2. Add Team objects\n3. Set TeamColor and Name\n4. Script to assign:\n\ngame.Players.PlayerAdded:Connect(function(player)\n    player.Team = game.Teams.RedTeam\nend)",

            "sound": "To add sound:\n\n1. Insert Sound in part\n2. Set SoundId = rbxassetid://[ID]\n3. Script: script.Parent.Sound:Play()\n\nSet Looped = true for music!",

            "sword": "Basic sword script:\n\nlocal damage = 10\n\nscript.Parent.Handle.Touched:Connect(function(hit)\n    if script.Parent.Parent:IsA('Model') and hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid:TakeDamage(damage)\n    end\nend)\n\nAdd to a Tool with Handle part.",

            "tool": "Basic tool script:\n\nscript.Parent.Activated:Connect(function()\n    print('Tool used!')\nend)\n\nAdd to a Tool object with Handle part.",

            // === SCIENCE ===
            "sky blue": "The sky appears blue due to Rayleigh scattering. Sunlight contains all colors, but when it enters Earth's atmosphere, it collides with gas molecules. Blue light has a shorter wavelength (about 450 nanometers) and scatters more easily than other colors. This scattered blue light reaches our eyes from all directions, making the sky appear blue.",
            
            "gravity": "Gravity is a fundamental force that attracts all objects with mass toward each other. On Earth, gravity gives objects weight and accelerates them at 9.8 m/s². The strength of gravity depends on mass and distance.",
            
            "photosynthesis": "Photosynthesis is the process by which plants convert light energy into chemical energy. Plants absorb sunlight, water, and CO2 to produce glucose (sugar) and oxygen. Overall: 6CO2 + 6H2O + light → C6H12O6 + 6O2",

            // === ANIMALS ===













































































