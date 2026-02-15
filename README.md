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
        // SIMPLIFIED answers database
        var answers = {
            // Roblox Studio How-To
            "how to start": "🎮 HOW TO START:\n1. Download Roblox Studio (FREE)\n2. Open and click 'New'\n3. Choose 'Baseplate'\n4. Press F5 to test!\n5. Save with Ctrl+S",
            "how start": "🎮 HOW TO START:\n1. Download Roblox Studio (FREE)\n2. Open and click 'New'\n3. Choose 'Baseplate'\n4. Press F5 to test!\n5. Save with Ctrl+S",
            "getting started": "🎮 HOW TO START:\n1. Download Roblox Studio (FREE)\n2. Open and click 'New'\n3. Choose 'Baseplate'\n4. Press F5 to test!\n5. Save with Ctrl+S",
            
            "add part": "🧱 HOW TO ADD PARTS:\n1. Click 'Part' button in Home tab\n2. Choose shape (Block/Sphere/Cylinder)\n3. Press M to move\n4. Press R to resize\n5. Press T to rotate\n\nCopy: Ctrl+C, Paste: Ctrl+V",
            "create part": "🧱 HOW TO ADD PARTS:\n1. Click 'Part' button in Home tab\n2. Choose shape (Block/Sphere/Cylinder)\n3. Press M to move\n4. Press R to resize\n5. Press T to rotate",
            
            "script": "📝 HOW TO SCRIPT:\n1. Click the part\n2. Press '+' in Explorer\n3. Choose 'Script'\n4. Type code!\n5. Press F5 to test\n\nExample:\nprint('Hello!')\nwait(2)\nscript.Parent.BrickColor = BrickColor.Random()",
            "how to code": "📝 HOW TO SCRIPT:\n1. Click the part\n2. Press '+' in Explorer\n3. Choose 'Script'\n4. Type code!\n5. Press F5 to test",
            
            "change color": "🎨 CHANGE COLORS:\n1. Select part\n2. Find 'BrickColor' in Properties\n3. Click to pick color\n4. Done!\n\nAlso try changing 'Material' for cool effects!",
            "color": "🎨 CHANGE COLORS:\n1. Select part\n2. Find 'BrickColor' in Properties\n3. Click to pick color!",
            
            "make move": "🏃 MAKE THINGS MOVE:\n\nwhile true do\n    wait(0.1)\n    script.Parent.Position = script.Parent.Position + Vector3.new(0.1, 0, 0)\nend\n\nThis makes it slide!",
            "movement": "🏃 MAKE THINGS MOVE:\n\nwhile true do\n    wait(0.1)\n    script.Parent.Position = script.Parent.Position + Vector3.new(0.1, 0, 0)\nend",
            
            "teleport": "✨ MAKE TELEPORTER:\n1. Create 2 parts\n2. Name one 'EndPart'\n3. Add script to first part:\n\nlocal endPart = workspace.EndPart\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent:MoveTo(endPart.Position)\n    end\nend)",
            
            "gui": "📱 MAKE GUI:\n1. StarterGui → Insert ScreenGui\n2. Add TextButton\n3. Change text in Properties\n4. Add LocalScript:\n\nscript.Parent.MouseButton1Click:Connect(function()\n    print('Clicked!')\nend)",
            "menu": "📱 MAKE GUI:\n1. StarterGui → Insert ScreenGui\n2. Add TextButton\n3. Change text in Properties",
            
            "sound": "🔊 ADD SOUNDS:\n1. Find sound on Roblox\n2. Copy Sound ID\n3. Insert Sound in part\n4. Paste ID: rbxassetid://12345\n5. Script: sound:Play()",
            "music": "🔊 ADD SOUNDS:\n1. Find sound on Roblox\n2. Copy Sound ID\n3. Insert Sound in part\n4. Script: sound:Play()",
            
            "spawn": "🎯 SPAWN POINT:\n1. Model tab → Spawn\n2. Move where you want\n3. Change color\n4. Check 'Anchored'!",
            "respawn": "🎯 SPAWN POINT:\n1. Model tab → Spawn\n2. Move where you want\n3. Check 'Anchored'!",
            
            "leaderboard": "📊 MAKE LEADERBOARD:\n\ngame.Players.PlayerAdded:Connect(function(player)\n    local stats = Instance.new('Folder')\n    stats.Name = 'leaderstats'\n    stats.Parent = player\n    \n    local coins = Instance.new('IntValue')\n    coins.Name = 'Coins'\n    coins.Value = 0\n    coins.Parent = stats\nend)",
            "leaderstats": "📊 MAKE LEADERBOARD:\nAdd Folder named 'leaderstats' to player\nAdd IntValue for coins/points inside it!",
            
            "coin": "💰 MAKE COINS:\n\nscript.Parent.Touched:Connect(function(hit)\n    local player = game.Players:GetPlayerFromCharacter(hit.Parent)\n    if player then\n        player.leaderstats.Coins.Value = player.leaderstats.Coins.Value + 1\n        script.Parent:Destroy()\n    end\nend)",
            "collect": "💰 MAKE COINS:\nUse Touched event to detect player\nAdd to their coins, then Destroy the coin!",
            
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
            "🏪 MALL TYCOON: Build shopping mall empire from 1 shop to 50+!"
        ];

        var jokes = [
            "Why don't scientists trust atoms? Because they make up everything! 😄",
            "What do you call a bear with no teeth? A gummy bear! 🐻",
            "Why did the bicycle fall over? It was two-tired! 🚲",
            "What do you call a fake noodle? An impasta! 🍝"
        ];

        addBot("👋 Hey! I'm your Roblox AI Helper!\n\nI can help with:\n• Roblox Studio tutorials\n• Game ideas\n• Science questions\n• Jokes\n\nWhat would you like?");

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
                response = "👋 Hey! Ask me about Roblox Studio or game ideas!";
            }

            if (!response && lower.includes('thank')) {
                response = "😊 You're welcome!";
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
                response = "I can help with:\n• How to start\n• How to add parts\n• How to script\n• How to change colors\n• How to make teleporters\n• Game ideas\n• Science questions\n\nJust ask!";
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
