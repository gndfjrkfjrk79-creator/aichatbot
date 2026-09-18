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
            
            <input type="text" id="input" placeholder="Ask me anything or math problems...">
            <button onclick="send()">Send</button>
        </div>
    </div>

    <script>
        var knowledgeBase = {
            "roblox": "I can help with Roblox Studio development!",
            "lava": "To create lava: Insert part → orange/red color → Material = Neon → Anchor it → Add this script:\n\nscript.Parent.Touched:Connect(function(hit)\n    if hit.Parent:FindFirstChild('Humanoid') then\n        hit.Parent.Humanoid.Health = 0\n    end\nend)",
            "part": "To add parts: Click Part button → Choose shape → Use M to move, R to scale, T to rotate.",
            "script": "To add script: Select part → Click + in Explorer → Choose Script → Write Lua code → F5 to test.",
        };

        var jokes = [
            "Why don't scientists trust atoms? Because they make up everything!",
            "What do you call a bear with no teeth? A gummy bear!",
            "Why did the bicycle fall over? It was two-tired!",
            "What do you call a fake noodle? An impasta!",
            "What did the ocean say to the beach? Nothing, it just waved!"
        ];

        addBot("Hello! I can solve ANY math problem!\n\nTry:\n• 4x4\n• 5*3\n• 100 + 50\n• 20 - 10\n• 100 / 5\n• 12 times 7\n\nI also know about Roblox and other topics!");

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

        function solveMath(msg) {
            var lower = msg.toLowerCase();
            
            // Replace 'x' and 'X' with '*' for multiplication
            var cleaned = lower.replace(/\s*x\s*/g, '*');
            cleaned = cleaned.replace(/\s*times\s*/g, '*');
            cleaned = cleaned.replace(/multiply\s+(\d+)\s+by\s+(\d+)/g, '$1*$2');
            
            // Remove words like "what is", "calculate", etc.
            cleaned = cleaned.replace(/what\s+is\s+/g, '');
            cleaned = cleaned.replace(/calculate\s+/g, '');
            cleaned = cleaned.replace(/solve\s+/g, '');
            
            // Check if there's a math operation
            if (cleaned.match(/\d+[\+\-\*\/]\d+/)) {
                try {
                    var result = eval(cleaned);
                    if (typeof result === 'number') {
                        return "🧮 " + msg + " = " + result;
                    }
                } catch(e) {
                    return null;
                }
            }
            
            return null;
        }

        function send() {
            var input = document.getElementById('input');
            var msg = input.value.trim();
            if (!msg) return;

            addUser(msg);
            input.value = '';

            var lower = msg.toLowerCase();
            var response = "";

            // TRY MATH FIRST
            var mathResult = solveMath(msg);
            if (mathResult) {
                response = mathResult;
            }

            // Greetings
            if (!response && (lower.includes('hello') || lower.includes('hi') || lower.includes('hey'))) {
                response = "Hello! How can I help?";
            }

            if (!response && (lower.includes('thank') || lower.includes('thx'))) {
                response = "You're welcome!";
            }

            // Jokes
            if (!response && lower.includes('joke')) {
                response = random(jokes);
            }

            // Check knowledge base
            if (!response) {
                for (var key in knowledgeBase) {
                    if (lower.includes(key)) {
                        response = knowledgeBase[key];
                        break;
                    }
                }
            }

            // Default
            if (!response) {
                response = "I can solve math problems and help with Roblox! What do you need?";
            }

            setTimeout(function() {
                addBot(response);
            }, 300);
        }

        document.getElementById('input').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                send();
            }
        });
    </script>
</body>
</html>












































































