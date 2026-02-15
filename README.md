<!DOCTYPE html>
<html>
<head>
    <title>Roblox AI Helper</title>
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
        <h1>🎮 Roblox AI Helper</h1>
        
        <div class="chat">
            <div id="messages"></div>
            
            <input type="text" id="input" placeholder="Ask me anything...">
            <button onclick="send()">Send</button>
        </div>
    </div>

    <script>
        // EXPANDED answers database with LAVA BLOCK tutorial
        var answers = {
            // Roblox Studio How-To
            "start": "🎮 HOW TO START:\n1. Download Roblox Studio (FREE)\n2. Open and click 'New'\n3. Choose 'Baseplate'\n4. Press F5 to test!\n5. Save with Ctrl+S",
            
            "part": "🧱 HOW TO ADD PARTS:\n1. Click 'Part' button in Home tab\n2. Choose shape (Block/Sphere/Cylinder)\n3. Press M to move\n4. Press R to resize\n5. Press T to rotate\n\nCopy: Ctrl+C, Paste: Ctrl+V",
            
            "script": "📝 HOW TO SCRIPT:\n1. Click the part\n2. Press '+' in Explorer\n3. Choose 'Script'\n4. Type code!\n5. Press F5 to test\n\nExample:\nprint('Hello!')\nwait(2)\nscript.Parent.BrickColor = BrickColor.Random()",
            
            "color": "🎨 CHANGE COLORS:\n1. Select part\n2. Find 'BrickColor' in Properties\n3. Click to pick color\n4. Done!\n\nAlso try changing 'Material' for cool effects!",
            
            "move": "🏃 MAKE THINGS MOVE:\n\nwhile true do\n    wait(0.1)\n    script.Parent.Position = script.Parent.Position + Vector3.new(0.1, 0, 0)\nend\n\nThis makes it slide!",
            
            "teleport": "✨ MAKE TELEPORTER:\n1. Create 2 parts\n2. Name one 'EndPart'\n3. Add script to first part:\n\nlocal endPart = workspace.EndPart\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent:MoveTo(endPart.Position)\n    end\nend)",
            
            "gui": "📱 MAKE GUI:\n1. StarterGui → Insert ScreenGui\n2. Add TextButton\n3. Change text in Properties\n4. Add LocalScript:\n\nscript.Parent.MouseButton1Click:Connect(function()\n    print('Clicked!')\nend)",
            
            "sound": "🔊 ADD SOUNDS:\n1. Find sound on Roblox\n2. Copy Sound ID\n3. Insert Sound in part\n4. Paste ID: rbxassetid://12345\n5. Script: sound:Play()",
            
            "spawn": "🎯 SPAWN POINT:\n1. Model tab → Spawn\n2. Move where you want\n3. Change color\n4. Check 'Anchored'!",
            
            "leaderboard": "📊 MAKE LEADERBOARD:\n\ngame.Players.PlayerAdded:Connect(function(player)\n    local stats = Instance.new('Folder')\n    stats.Name = 'leaderstats'\n    stats.Parent = player\n    \n    local coins = Instance.new('IntValue')\n    coins.Name = 'Coins'\n    coins.Value = 0\n    coins.Parent = stats\nend)",
            
            "coin": "💰 MAKE COINS:\n\nscript.Parent.Touched:Connect(function(hit)\n    local player = game.Players:GetPlayerFromCharacter(hit.Parent)\n    if player then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value + 1\n        script.Parent:Destroy()\n    end\nend)",

            // LAVA BLOCK TUTORIAL - NEW!
            "lava": "🌋 HOW TO MAKE LAVA BLOCK:\n\nStep 1 - Create the block:\n1. Insert a Part\n2. Make it orange/red color (BrickColor)\n3. Set Material to 'Neon' for glow effect\n4. Make it Anchored (check box in Properties)\n\nStep 2 - Add the kill script:\n1. Add a Script to the part\n2. Paste this code:\n\nscript.Parent.Touched:Connect(function(hit)\n    local humanoid = hit.Parent:FindFirstChild('Humanoid')\n    if humanoid then\n        humanoid.Health = 0\n    end\nend)\n\nStep 3 (Optional):\n• Add Fire object inside part for flames!\n• Add orange PointLight for glow!\n\nNow players die when they touch it!",

            "lava block": "🌋 HOW TO MAKE LAVA BLOCK:\n\n1. Insert a Part\n2. Color it orange/red\n3. Material = Neon (for glow)\n4. Anchor it!\n5. Add Script with this code:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = 0\n    end\nend)\n\n6. Add Fire object for cool effect!\n\nDone! Deadly lava!",

            "kill block": "☠️ HOW TO MAKE KILL BLOCK:\n\n1. Create a Part\n2. Color it red (or any color)\n3. Add this Script:\n\nscript.Parent.Touched:Connect(function(hit)\n    local humanoid = hit.Parent:FindFirstChild('Humanoid')\n    if humanoid then\n        humanoid.Health = 0\n    end\nend)\n\nNow it kills players on touch!\n\nTip: Make it red so players know it's dangerous!",

            "damage": "💔 HOW TO MAKE DAMAGE BLOCK:\n\n1. Create a Part\n2. Add Script:\n\nlocal damage = 10  -- Change this number!\n\nscript.Parent.Touched:Connect(function(hit)\n    local humanoid = hit.Parent:FindFirstChild('Humanoid')\n    if humanoid then\n        humanoid.Health = humanoid.Health - damage\n    end\nend)\n\nThis removes 10 health per touch!\nChange damage = 10 to any number you want!",

            "fire": "🔥 HOW TO ADD FIRE:\n\n1. Select your part\n2. Click '+' in Explorer next to the part\n3. Choose 'Fire'\n4. Customize in Properties:\n   • Size = how big flames are\n   • Heat = how high flames go\n   • Color = flame color\n\nGreat for lava blocks, torches, campfires!",

            // Building Tips
            "build": "🏗️ BUILDING TIPS:\n\n1. Start with large parts, add details later\n2. Use grid snap for neat alignment\n3. Group parts together (Ctrl+G)\n4. Copy/paste to build faster (Ctrl+C, Ctrl+V)\n5. Use different materials (wood, metal, grass)\n6. Add lighting for atmosphere\n7. Test often with F5!\n\nPractice makes perfect!",
            
            "building": "🏗️ BUILDING TIPS:\n\n1. Start simple - make basic shapes first\n2. Use the grid to align parts\n3. Try different materials and colors\n4. Group related parts (Ctrl+G)\n5. Anchor parts that shouldn't move\n6. Add details last (windows, doors)\n7. Use workspace camera to see different angles\n\nTip: Build one thing at a time!",
            
            "tip": "💡 PRO BUILDING TIPS:\n\n• Use snap to grid for neat builds\n• Duplicate parts with Ctrl+D\n• Hold Shift while scaling for even sizing\n• Use Workspace → Show Grid\n• Name your parts in Explorer\n• Use folders to organize\n• Save versions of your work\n• Watch YouTube tutorials!\n\nWhat are you building?",
            
            "tips": "💡 QUICK TIPS:\n\n• F5 = Test game\n• Ctrl+S = Save\n• Ctrl+G = Group parts\n• Ctrl+D = Duplicate\n• M = Move tool\n• R = Resize tool\n• T = Rotate tool\n• Anchor important parts!\n• Use different colors and materials\n• Test your game often!",

            "anchor": "⚓ HOW TO ANCHOR:\n\n1. Select the part\n2. Find 'Anchored' in Properties\n3. Check the box\n\nAnchored parts don't fall!\n\nAnchor:\n• Floors and walls\n• Buildings\n• Platforms\n\nDon't anchor:\n• Moving objects\n• Things players push",

            "group": "📁 HOW TO GROUP:\n\n1. Select multiple parts (hold Ctrl and click)\n2. Press Ctrl+G (or right-click → Group)\n3. Name the group in Properties\n4. Now move all parts together!\n\nUngroup: Ctrl+U\n\nGrouping makes building easier!",

            "resize": "📏 HOW TO RESIZE:\n\nMethod 1:\n• Press R (resize tool)\n• Drag handles\n• Hold Shift for even sizing\n\nMethod 2:\n• Select part\n• Properties → Size\n• Change X, Y, Z numbers\n\nX=width, Y=height, Z=depth",

            "terrain": "🏔️ HOW TO USE TERRAIN:\n\n1. Home tab → Terrain Editor\n2. Choose Generate for instant terrain\n3. Use Grow tool to add\n4. Use Erode tool to remove\n5. Paint tool changes type (grass/sand/rock)\n6. Try different materials!\n\nTerrain = natural landscapes!",

            "lighting": "💡 HOW TO CHANGE LIGHTING:\n\n1. Find 'Lighting' in Explorer\n2. Change TimeOfDay:\n   • '14:00:00' = daytime\n   • '00:00:00' = nighttime\n3. Change Brightness (higher = brighter)\n4. Add fog with FogEnd and FogColor\n\nGood lighting = amazing games!",

            "detail": "✨ ADDING DETAILS:\n\n1. Use small parts for decorations\n2. Add textures/decals for pictures\n3. Use PointLights to make things glow\n4. Try different materials (neon, glass, wood)\n5. Add Smoke or Fire effects\n6. Use Unions to combine shapes\n7. Add plants from Toolbox!\n\nDetails make games special!",

            "organize": "📁 HOW TO ORGANIZE:\n\n1. Use Folders in Workspace\n2. Name everything clearly\n3. Group related parts\n4. Keep scripts in ServerScriptService\n5. Comment your code!\n\nGood organization = easier building!",

            // Science
            "sky blue": "🌤️ Sky is blue because of Rayleigh scattering! Blue light scatters more than other colors!",
            "planes fly": "✈️ Planes fly because of lift! Wings create lower pressure on top!",
            "gravity": "🌍 Gravity pulls objects together! Earth's gravity keeps us on ground!",
            "fastest animal": "🐆 Cheetah is fastest on land (70mph)! Peregrine falcon fastest overall (240mph)!",
            "biggest animal": "🐋 Blue whale is biggest - 100 feet long, 200 tons!",
            "planets": "🪐 8 planets: Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune!",
            "black hole": "⚫ Black hole has gravity SO strong nothing escapes - not even light!",
            "photosynthesis": "🌱 Plants make food from sunlight using water and CO2!",
            "dinosaurs": "🦖 Dinosaurs went extinct 65 million years ago from asteroid!"
        };

        var games = [
            "🏃 SPEED OBBY: Racing with moving platforms, boost pads, checkpoints, leaderboard, shortcuts!",
            "🌈 RAINBOW OBBY: Each level different color! RED=fire, BLUE=ice, GREEN=nature!",
            "🍕 PIZZA SIMULATOR: Run pizza shop! Hire staff, make pizzas, expand empire!",
            "🐾 PET SIMULATOR: Collect 100+ pets, evolve them, build homes, trade!",
            "⚔️ SWORD FIGHTER: Battle with legendary swords, upgrade armor, defeat bosses!",
            "🏎️ KART RACING: Race on 20 tracks with power-ups and customization!",
            "🏰 CASTLE ADVENTURE: Explore castle, solve puzzles, fight dragon boss!",
            "🏪 MALL TYCOON: Build shopping mall empire from 1 shop to 50+!",
            "🎢 THEME PARK: Build amusement park with custom roller coasters!",
            "🏝️ ISLAND BUILDER: Start tiny island, expand, plant crops, build houses!"
        ];

        var jokes = [
            "Why don't scientists trust atoms? Because they make up everything! 😄",
            "What do you call a bear with no teeth? A gummy bear! 🐻",
            "Why did the bicycle fall over? It was two-tired! 🚲",
            "What do you call a fake noodle? An impasta! 🍝",
            "What did the ocean say to the beach? Nothing, it just waved! 🌊"
        ];

        addBot("👋 Hey! I'm your Roblox AI Helper!\n\nI can help with:\n• Roblox Studio tutorials\n• Building tips\n• How to make lava blocks\n• Game ideas\n• Science questions\n• Jokes\n\nWhat would you like?");

        function addBot(text) {
            var div = document.createElement('div');
            div.className = 'message bot';
            div.innerText = text;
            document.getElementById('messages').appendChild(div);
            document.getElementById('messages').scrollTop = 999999;
        }

        function addUser(text) {
            var div = document.createElement('div');
            div.className = 'message user';
            div.innerText = text;
            document.getElementById('messages').appendChild(div);
            document.getElementById('messages').scrollTop = 999999;
        }

        function random(arr) {
            return arr[Math.floor(Math.random() * arr.length)];
        }

        function send() {
            var input = document.getElementById('input');
            var msg = input.value.trim();
            if (!msg) return;

            addUser(msg);
            input.value = '';

            var lower = msg.toLowerCase();
            var response = "";

            // Math
            if (lower.match(/\d+\s*[\+\-\*\/]\s*\d+/)) {
                try {
                    var result = eval(lower.replace(/[^0-9+\-*/().]/g, ''));
                    response = "🧮 " + lower + " = " + result;
                } catch(e) {}
            }

            // Greetings
            if (!response && (lower.includes('hello') || lower.includes('hi') || lower.includes('hey'))) {
                response = "👋 Hey! Ask me about Roblox Studio, lava blocks, building tips, or game ideas!";
            }

            if (!response && lower.includes('thank')) {
                response = "😊 You're welcome! Need more help?";
            }

            // Jokes
            if (!response && lower.includes('joke')) {
                response = random(jokes);
            }

            // Game ideas
            if (!response && (lower.includes('game') || lower.includes('idea') || lower.includes('obby') || lower.includes('simulator'))) {
                response = random(games);
            }

            // Check answers database
            if (!response) {
                for (var key in answers) {
                    if (lower.includes(key)) {
                        response = answers[key];
                        break;
                    }
                }
            }

            // Default
            if (!response) {
                response = "I can help with:\n\n🌋 How to make lava blocks\n☠️ How to make kill blocks\n🔥 How to add fire\n🏗️ Building tips\n🎮 How to start\n🧱 How to add parts\n📝 How to script\n💡 Game ideas\n🔬 Science questions\n\nWhat would you like to know?";
            }

            setTimeout(function() {
                addBot(response);
            }, 500);
        }

        document.getElementById('input').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                send();
            }
        });
    </script>
</body>
</html>
