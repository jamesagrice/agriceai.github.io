<div id="app-container">
    
    <div id="sidebar-left">
        <div class="agent-info">
            <div class="logo-placeholder">A</div>
            <h3>AGRICE</h3>
            <p style="font-size:0.9em;">Assistant Fiches Intervention</p>
        </div>
        
        <div class="navigation-area">
            <button class="nav-link" onclick="newChat()">➕ Nouveau Chat</button>
            
            <div class="nav-title">HISTORIQUE (Simulation)</div>
            <button class="nav-link" disabled>Session du 09/12 (Dernière)</button>
            <button class="nav-link" disabled>Session du 08/12</button>
        </div>
        
        <div style="text-align:center; font-size: 0.7em; color: #bdc3c7; margin-top: 20px;">
            Propulsé par Make.com
        </div>
    </div>
    
    <div id="main-content">
        <header>AGRICE - L'assistant sur les fichiers d'intervention du SUPPORT</header>
        <div id="chatbox">
            <div class="message-agent"><span class="bubble agent-bubble">Bonjour ! Je suis l'Agent AGRICE, votre assistant pour les fichiers d'intervention. Que puis-je faire pour vous aujourd'hui ?</span></div>
        </div>
        <div class="input-area">
            <input type="text" id="userInput" placeholder="Saisissez votre demande..." onkeydown="if(event.key === 'Enter') sendChatMessage()">
            <button onclick="sendChatMessage()">Envoyer</button>
        </div>
    </div>
</div>

    body { 
            font-family: Arial, sans-serif; 
            margin: 0; 
            padding: 0; 
            background-color: #f4f4f9; 
            display: flex; 
            min-height: 100vh; 
        }
        
        #app-container {
            display: flex;
            width: 100%;
            max-width: 1200px;
            margin: auto;
            border-radius: 8px;
            overflow: hidden; /* Pour contenir les coins arrondis */
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }

        /* --- BARRE LATÉRALE GAUCHE (SIDEBAR) --- */
        #sidebar-left {
            width: 280px;
            background-color: #2f3945; /* Couleur sombre comme Olivia */
            color: white;
            padding: 20px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }
        .agent-info {
            text-align: center;
            padding-bottom: 20px;
            border-bottom: 1px solid #81c45a;
        }
        .logo-placeholder {
            /* Remplacez ceci par une vraie image de logo */
            width: 80px;
            height: 80px;
            background-color: #81c45a; /* Couleur du logo */
            border-radius: 50%;
            margin: 0 auto 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            font-weight: bold;
        }
        .nav-link {
            padding: 10px;
            margin: 5px 0;
            background: none;
            border: none;
            color: white;
            text-align: left;
            width: 100%;
            cursor: pointer;
            border-radius: 4px;
            transition: background-color 0.3s;
        }
        .nav-link:hover {
            background-color: #81c45a;
        }
        .nav-title {
            color: #bdc3c7;
            font-size: 0.8em;
            margin-top: 20px;
            margin-bottom: 5px;
            padding-left: 10px;
        }
        
        /* --- ZONE PRINCIPALE DE CHAT --- */
        #main-content {
            flex-grow: 1;
            background: white;
            display: flex;
            flex-direction: column;
        }
        header { 
            background-color: #2f3945; /* Couleur de la barre de titre principale */
            color: white; 
            padding: 15px; 
            text-align: center; 
            font-size: 1.1em; 
        }
        #chatbox { 
            height: 500px; 
            overflow-y: auto; 
            padding: 15px; 
            border-bottom: 1px solid #eee; 
            flex-grow: 1; /* Permet à la chatbox de prendre l'espace restant */
        }
        .message-user { text-align: right; margin-bottom: 10px; }
        .message-agent { text-align: left; margin-bottom: 10px; }
        .bubble { display: inline-block; padding: 10px 15px; border-radius: 20px; max-width: 80%; line-height: 1.4; }
        .user-bubble { background-color: #dcf8c6; color: #000; }
        .agent-bubble { background-color: #f1f0f0; color: #000; }
        .input-area { display: flex; padding: 15px; border-top: 1px solid #eee; }
        #userInput { flex-grow: 1; padding: 10px; border: 1px solid #ccc; border-radius: 20px; font-size: 1em; margin-right: 10px; }
        button { padding: 10px 15px; background-color: #2f3945; /* VOTRE COULEUR PERSONNALISÉE */ color: white; border: none; border-radius: 20px; cursor: pointer; transition: background-color 0.3s; }
        button:hover { background-color: #81c45a; }

        // --- REMPLACEZ CECI PAR L'URL DE VOTRE WEBHOOK MAKE.COM ---
    const WEBHOOK_URL = 'https://hook.us2.make.com/ibj8yb1zd3ixclq5bhbqetfop756owsf'; 
    // --------------------------------------------------------

    // --- FONCTIONNALITÉS DU CHAT (Même logique que précédemment) ---

    function newChat() {
        // Fonction pour effacer la conversation et simuler un nouveau chat
        document.getElementById('chatbox').innerHTML = '<div class="message-agent"><span class="bubble agent-bubble">Bonjour ! Je suis l\'Agent AGRICE, votre assistant pour les fichiers d\'intervention. Que puis-je faire pour vous aujourd\'hui ?</span></div>';
        document.getElementById('userInput').focus();
    }

    async function sendChatMessage() {
        // [Votre logique d'envoi et de réception reste la même]
        const userInput = document.getElementById('userInput');
        const message = userInput.value;
        userInput.value = '';

        if (message.trim() === '') return;

        appendMessage('Vous', message, 'message-user', 'user-bubble');

        try {
            const loadingId = appendMessage('Agent AGRICE', '...', 'message-agent', 'agent-bubble');
            
            const response = await fetch(WEBHOOK_URL, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ message: message }) 
            });

            const data = await response.json();
            
            const agentResponse = data.response; 
            updateMessage(loadingId, 'Agent AGRICE', agentResponse, 'message-agent', 'agent-bubble');
            
        } catch (error) {
            updateMessage(loadingId, 'Agent AGRICE', 'Erreur de connexion à l\'Agent : ' + error.message, 'message-agent', 'agent-bubble');
            console.error(error);
        }
    }

    function appendMessage(sender, text, rowClass, bubbleClass) {
        const chatbox = document.getElementById('chatbox');
        const messageRow = document.createElement('div');
        const messageBubble = document.createElement('span');
        const uniqueId = 'msg-' + Date.now();

        messageRow.className = rowClass;
        messageRow.id = uniqueId;
        
        messageBubble.className = 'bubble ' + bubbleClass;
        messageBubble.innerHTML = text.replace(/\n/g, '<br>');

        messageRow.appendChild(messageBubble);
        chatbox.appendChild(messageRow);
        chatbox.scrollTop = chatbox.scrollHeight;
        
        return uniqueId;
    }
    
    function updateMessage(id, sender, newText, rowClass, bubbleClass) {
        const messageRow = document.getElementById(id);
        if (messageRow) {
            messageRow.querySelector('.bubble').innerHTML = newText.replace(/\n/g, '<br>');
            document.getElementById('chatbox').scrollTop = document.getElementById('chatbox').scrollHeight;
        }
    }
