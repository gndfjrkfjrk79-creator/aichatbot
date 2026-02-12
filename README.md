<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Roblox Studio Ideas Bot</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #1e3a8a;
            min-height: 100vh;
            padding: 20px;
            position: relative;
            overflow-x: hidden;
        }

        body::before {
            content: '6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            font-size: 150px;
            font-weight: bold;
            color: rgba(59, 130, 246, 0.1);
            line-height: 1.5;
            word-wrap: break-word;
            z-index: 0;
            pointer-events: none;
            letter-spacing: 50px;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }

        .header {
            text-align: center;
            color: white;
            margin-bottom: 20px;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.1em;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
        }

        .api-key-setup {
            background: white;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        .api-key-setup h3 {
            margin-bottom: 10px;
            color: #1e40af;
        }

        .api-key-setup input {
            width: calc(100% - 120px);
            padding: 10px;
            border: 2px solid #3b82f6;
            border-radius: 5px;
            font-size: 14px;
            margin-right: 10px;
        }

        .api-key-setup input:focus {
            outline: none;
            border-color: #2563eb;
        }

        .api-key-setup button {
            padding: 10px 20px;
            background-color: #2563eb;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
        }

        .api-key-setup button:hover {
            background-color: #1d4ed8;
        }

        .api-key-setup .instructions {
            margin-top: 10px;
            font-size: 12px;
            color: #666;
        }

        .api-key-setup .instructions a {
            color: #2563eb;
        }

        .chat-container {
            background: white;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            display: none;
        }

        .chat-container.active {
            display: block;
        }

        .messages {
            height: 500px;
            overflow-y: auto;
            border: 2px solid #bfdbfe;
            padding: 15px;
            margin-bottom: 20px;
            border-radius: 8px;
            background-color: #eff6ff;
        }

        .message {
            margin: 15px 0;
            padding: 12px 15px;
            border-radius: 8px;
            animation: fadeIn 0.3s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .user-message {
            background-color: #2563eb;
            color: white;
            margin-left: 50px;
            text-align: right;
        }

        .bot-message {
            background-color: white;
            border: 2px solid #93c5fd;
            margin-right: 50px;
            color: #1e40af;
        }

        .input-area {
            display: flex;
            gap: 10px;
        }

        #userInput {
            flex: 1;
            padding: 12px;
            border: 2px solid #3b82f6;
            border-radius: 8px;
            font-size: 16px;
            transition: border-color 0.3s;
        }

        #userInput:focus {
            outline: none;
            border-color: #2563eb;
        }

        #sendBtn {
            padding: 12px 30px;
            background-color: #2563eb;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: background-color 0.3s;
        }

        #sendBtn:hover {
            background-color: #1d4ed8;
        }

        #sendBtn:disabled {
            background-color: #93c5fd;
            cursor: not-allowed;
        }

        .typing-indicator {
            display: none;
            padding: 10px;
            color: #1e40af;
            font-style: italic;
        }

        .typing-indicator.active {
            display: block;
        }

        .reset-key {
            text-align: center;
            margin-top: 15px;
        }

        .reset-key button {
            padding: 8px 16px;
            background-color: #dc2626;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 12px;
        }

        .reset-key button:hover {
            background-color: #b91c1c;
        }

        /* Custom scrollbar for messages */
        .messages::-webkit-scrollbar {
            width: 8px;
        }

        .messages::-webkit-scrollbar-track {
            background: #dbeafe;
            border-radius: 4px;
        }

        .messages::-webkit-scrollbar-thumb {
            background: #3b82f6;
            border-radius: 4px;
        }

        .messages::-webkit-scrollbar-thumb:hover {
            background: #2563eb;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎮 Roblox Studio Ideas Bot</h1>
            <p>Get creative ideas for your next Roblox game!</p>
        </div>

        <div class="api-key-setup" id="apiKeySetup">
            <h3>🔑 Setup Your API Key</h3>
            <div>
                <input 
                    type="password" 
                    id="apiKeyInput" 
                    placeholder="Enter your Anthropic API key..."
                >
                <button onclick="saveApiKey()">Start Chatting</button>
            </div>
            <div class="instructions">
                Don't have an API key? Get one from <a href="https://console.anthropic.com/" target="_blank">Anthropic Console</a>. Your key is stored locally in your browser.
            </div>
        </div>

        <div class="chat-container" id="chatContainer">
            <div class="messages" id="messages"></div>
            <div class="typing-indicator" id="typingIndicator">Bot is typing...</div>
            <div class="input-area">
                <input 
                    type="text" 
                    id="userInput" 
                    placeholder="Ask for Roblox Studio game ideas..."
                    onkeypress="handleKeyPress(event)"
                >
                <button onclick="sendMessage()" id="sendBtn">Send</button>
            </div>
            <div class="reset-key">
                <button onclick="resetApiKey()">Reset API Key</button>
            </div>
        </div>
    </div>

    <script>
        const messagesDiv = document.getElementById('messages');
        const userInput = document.getElementById('userInput');
        const sendBtn = document.getElementById('sendBtn');
        const typingIndicator = document.getElementById('typingIndicator');
        const apiKeySetup = document.getElementById('apiKeySetup');
        const chatContainer = document.getElementById('chatContainer');
        const apiKeyInput = document.getElementById('apiKeyInput');

        let apiKey = localStorage.getItem('anthropic_api_key');

        // Check if API key exists
        if (apiKey) {
            showChatInterface();
        }

        function saveApiKey() {
            const key = apiKeyInput.value.trim();
            if (!key) {
                alert('Please enter an API key');
                return;
            }
            localStorage.setItem('anthropic_api_key', key);
            apiKey = key;
            showChatInterface();
        }

        function resetApiKey() {
            if (confirm('Are you sure you want to reset your API key?')) {
                localStorage.removeItem('anthropic_api_key');
                apiKey = null;
                apiKeySetup.style.display = 'block';
                chatContainer.classList.remove('active');
                messagesDiv.innerHTML = '';
            }
        }

        function showChatInterface() {
            apiKeySetup.style.display = 'none';
            chatContainer.classList.add('active');
            addMessage('bot', '👋 Hi! I can help you come up with fun ideas for Roblox Studio! What kind of game would you like to make? Adventure? Racing? Pet simulator? Let me know!');
        }

        function addMessage(type, text) {
            const messageDiv = document.createElement('div');
            messageDiv.className = `message ${type}-message`;
            messageDiv.textContent = text;
            messagesDiv.appendChild(messageDiv);
            messagesDiv.scrollTop = messagesDiv.scrollHeight;
        }

        function handleKeyPress(event) {
            if (event.key === 'Enter' && !sendBtn.disabled) {
                sendMessage();
            }
        }

        async function sendMessage() {
            const message = userInput.value.trim();
            if (!message) return;

            // Disable input while processing
            userInput.disabled = true;
            sendBtn.disabled = true;
            typingIndicator.classList.add('active');

            // Add user message
            addMessage('user', message);
            userInput.value = '';

            try {
                const response = await fetch('https://api.anthropic.com/v1/messages', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'x-api-key': apiKey,
                        'anthropic-version': '2023-06-01'
                    },
                    body: JSON.stringify({
                        model: 'claude-sonnet-4-20250514',
                        max_tokens: 1000,
                        messages: [{
                            role: 'user',
                            content: `You are a helpful, enthusiastic assistant that gives creative, age-appropriate Roblox Studio game ideas to young developers (around 8 years old). Keep suggestions fun, simple to understand, encouraging, and safe. Break down ideas into easy steps when helpful. Here's their question: ${message}`
                        }]
                    })
                });

                if (!response.ok) {
                    throw new Error('API request failed');
                }

                const data = await response.json();
                const botReply = data.content[0].text;
                addMessage('bot', botReply);

            } catch (error) {
                addMessage('bot', '😕 Sorry, I had trouble connecting. Please check your API key and try again!');
                console.error('Error:', error);
            }

            // Re-enable input
            typingIndicator.classList.remove('active');
            userInput.disabled = false;
            sendBtn.disabled = false;
            userInput.focus();
        }
    </script>
</body>
</html>
