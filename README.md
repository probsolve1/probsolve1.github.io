
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ProbSolver</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/katex.min.css">
<link rel="shortcut icon" type="https://ibb.co/Y4Hrqbjc" />
    <script src="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/katex.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/katex@0.16.8/dist/contrib/auto-render.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #1a1a1a;
            color: #e5e7eb;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            align-items: center;
            min-height: 100vh;
            padding: 1rem;
            box-sizing: border-box;
            position: relative;
            background: linear-gradient(-45deg, #1a1a1a, #2c3e50, #1a1a1a, #34495e);
            background-size: 400% 400%;
            animation: gradientBG 15s ease infinite;
        }
        @keyframes gradientBG {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        .main-container {
            width: 100%;
            max-width: 800px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            align-items: center;
            padding-bottom: 2rem;
            position: relative;
            min-height: calc(100vh - 2rem);
        }
        .title-container {
            position: absolute;
            top: 10%;
            width: 100%;
            text-align: center;
            transition: opacity 0.5s ease;
        }
        .solution-box {
            overflow-y: auto;
            max-height: calc(100vh - 200px);
            width: 100%;
            opacity: 0;
            transform: translateY(20px);
            animation: fadeInAndSlideUp 0.5s ease-out forwards;
            background-color: #242424;
            border-radius: 1rem;
            padding: 1.5rem;
            margin-bottom: 1rem;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.4);
            position: relative;
        }
        .solution-box h2 {
            font-size: 1.5rem;
            font-weight: 600;
            margin-top: 1rem;
            margin-bottom: 0.5rem;
            color: #e5e7eb;
        }
        .solution-box p {
            margin-bottom: 1rem;
        }
        .input-bar-container {
            width: 100%;
            max-width: 800px;
            background-color: #242424;
            border-radius: 2rem;
            padding: 0.5rem 1rem;
            display: flex;
            align-items: center;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06), 0 0 15px rgba(255, 255, 255, 0);
            border: 1px solid #3d3d3d;
            transition: box-shadow 0.3s ease-in-out;
            position: relative;
        }
        .input-bar-container:hover {
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06), 0 0 15px rgba(255, 255, 255, 0.2);
        }
        .input-bar {
            flex-grow: 1;
            background-color: transparent;
            border: none;
            outline: none;
            color: #e5e7eb;
            font-size: 1rem;
            padding: 0.5rem 0;
            line-height: 1.5;
            z-index: 10;
        }
        .input-bar::placeholder {
            color: #9ca3af;
            font-weight: 300;
        }
        .input-bar-button {
            background-color: transparent;
            border: none;
            cursor: pointer;
            padding: 0.5rem;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: transform 0.2s ease-in-out;
        }
        .input-bar-button:hover {
            transform: scale(1.1);
        }
        .input-bar-button svg {
            width: 24px;
            height: 24px;
            fill: #9ca3af;
            transition: fill 0.2s ease-in-out;
        }
        .input-bar-button:hover svg {
            fill: #fff;
        }
        #voiceButton { margin-right: -0.5rem; }
        .loading-dots {
            display: flex;
            align-items: center;
            margin-right: 8px;
        }
        .dot {
            width: 8px;
            height: 8px;
            background-color: #4CAF50;
            border-radius: 50%;
            margin: 0 4px;
            animation: bounce 1s infinite;
        }
        .dot:nth-child(2) {
            background-color: #2196F3;
            animation-delay: 0.2s;
        }
        .dot:nth-child(3) {
            background-color: #FFC107;
            animation-delay: 0.4s;
        }
        .hidden { display: none; }
        @keyframes bounce {
            0%, 80%, 100% { transform: scale(0); }
            40% { transform: scale(1.0); }
        }
        @keyframes fadeInAndSlideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .button-group {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 0.75rem;
            margin-top: 1rem;
        }
        #imagePreviewContainer {
            width: 100%;
            max-width: 100%;
            margin-bottom: 1rem;
            border-radius: 1rem;
            background-color: #242424;
            padding: 1rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
        }
        .image-preview {
            max-width: 100%;
            max-height: 400px;
            border-radius: 0.5rem;
        }
        .remove-ss-button {
            position: absolute;
            top: 0.5rem;
            right: 0.5rem;
            background-color: rgba(255, 255, 255, 0.1);
            color: white;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: background-color 0.2s ease-in-out;
            z-index: 20;
        }
        .remove-ss-button:hover {
            background-color: rgba(255, 255, 255, 0.2);
        }
        .message-container {
            width: 100%;
            display: flex;
            flex-direction: column;
            gap: 1rem;
            margin-bottom: 1rem;
        }
        .message {
            padding: 1rem;
            border-radius: 1rem;
            max-width: 80%;
            word-wrap: break-word;
        }
        .user-message {
            background-color: #007bff;
            color: white;
            align-self: flex-end;
            border-bottom-right-radius: 0;
            animation: fadeInFromRight 0.3s ease-in-out;
        }
        .ai-message {
            background-color: #242424;
            color: #e5e7eb;
            align-self: flex-start;
            border-bottom-left-radius: 0;
            animation: fadeInFromLeft 0.3s ease-in-out;
        }
        @keyframes fadeInFromRight {
            from { opacity: 0; transform: translateX(20px); }
            to { opacity: 1; transform: translateX(0); }
        }
        @keyframes fadeInFromLeft {
            from { opacity: 0; transform: translateX(-20px); }
            to { opacity: 1; transform: translateX(0); }
        }
    </style>
</head>
<body>
    <div class="main-container">
        <div class="title-container" id="titleContainer">
            <h1 class="text-4xl font-bold text-white">ProbSolver</h1>
        </div>

        <div id="messageHistory" class="message-container"></div>
        <div id="loadingMessage" class="flex items-center text-gray-500 hidden mb-4">
            <div class="loading-dots">
                <div class="dot"></div>
                <div class="dot"></div>
                <div class="dot"></div>
            </div>
            <span>ProbSolver is thinking...</span>
        </div>

        <div id="imagePreviewContainer" class="hidden">
            <img id="imagePreview" class="image-preview" />
            <button id="removeSsButton" class="remove-ss-button hidden">&times;</button>
        </div>
        
        <div class="input-bar-container">
            <input type="text" id="wordProblemInput" placeholder="Ask ProbSolver" class="input-bar" />
            <label for="imageUpload" class="input-bar-button">
                <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path fill-rule="evenodd" clip-rule="evenodd" d="M15 11h2v-2h-2v2zM12 11h2v-2h-2v2zM9 11h2v-2H9v2zM7 13h10V9H7v4zm-2 2h14v-8h-14v8z" fill="currentColor"/>
                    <path d="M17 5.5H7A1.5 1.5 0 0 0 5.5 7v10A1.5 1.5 0 0 0 7 18.5h10a1.5 1.5 0 0 0 1.5-1.5V7A1.5 1.5 0 0 0 17 5.5zm0 13H7a.5.5 0 0 1-.5-.5V7a.5.5 0 0 1 .5-.5h10a.5.5 0 0 1 .5.5v10a.5.5 0 0 1-.5.5z" fill="currentColor"/>
                </svg>
            </label>
            <input type="file" id="imageUpload" class="hidden" accept="image/*" />
            <label for="cameraUpload" class="input-bar-button">
                <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M12 17a5 5 0 1 0 0-10 5 5 0 0 0 0 10zM12 9a3 3 0 1 1 0 6 3 3 0 0 1 0-6z" fill="currentColor"/>
                    <path d="M18 4H6a3 3 0 0 0-3 3v10a3 3 0 0 0 3 3h12a3 3 0 0 0 3-3V7a3 3 0 0 0-3-3zM6 6h12a1 1 0 0 1 1 1v10a1 1 0 0 1-1 1H6a1 1 0 0 1-1-1V7a1 1 0 0 1 1-1z" fill="currentColor"/>
                    <path d="M21 7h-2.5a.5.5 0 0 0 0 1H21v-1z" fill="currentColor"/>
                    <path d="M3 7h2.5a.5.5 0 0 1 0 1H3v-1z" fill="currentColor"/>
                    <path d="M15 4h-2a.5.5 0 0 0 0 1h2v-1z" fill="currentColor"/>
                </svg>
            </label>
            <input type="file" id="cameraUpload" class="hidden" accept="image/*" capture="camera" />
            <button id="voiceButton" class="input-bar-button">
                <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M12 14c-1.66 0-3-1.34-3-3V5c0-1.66 1.34-3 3-3s3 1.34 3 3v6c0 1.66-1.34 3-3 3z" fill="currentColor"/>
                    <path d="M19 11h-1.92c-.39 2.05-2.22 3.5-4.58 3.5S8.31 13.05 7.92 11H6c.45 2.5 2.19 4.4 4.5 5.51V19h-2v2h6v-2h-2v-2.49c2.31-1.11 4.05-3.01 4.5-5.51z" fill="currentColor"/>
                </svg>
            </button>
            <button id="solveButton" class="input-bar-button">
                <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M5 12h14M12 5l7 7-7 7" stroke="#9ca3af" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
            </button>
        </div>
    </div>

    <script>
        const wordProblemInput = document.getElementById('wordProblemInput');
        const imageUpload = document.getElementById('imageUpload');
        const cameraUpload = document.getElementById('cameraUpload');
        const solveButton = document.getElementById('solveButton');
        const voiceButton = document.getElementById('voiceButton');
        const messageHistory = document.getElementById('messageHistory');
        const loadingMessage = document.getElementById('loadingMessage');
        const titleContainer = document.getElementById('titleContainer');
        const imagePreview = document.getElementById('imagePreview');
        const imagePreviewContainer = document.getElementById('imagePreviewContainer');
        const removeSsButton = document.getElementById('removeSsButton');
        let uploadedImageBase64 = null;
        let uploadedImageMimeType = null;
        let lastProblemText = '';
        const API_KEY = 'AIzaSyDEeJkrym65-ZGNzTpY6_wHEMhoDETFX4w';

        function addMessage(text, sender, isImage = false, imageUrl = null) {
            const messageDiv = document.createElement('div');
            messageDiv.classList.add('message');
            messageDiv.classList.add(sender === 'user' ? 'user-message' : 'ai-message');
            
            if (isImage) {
                const img = document.createElement('img');
                img.src = imageUrl;
                img.style.maxWidth = '100%';
                img.style.borderRadius = '0.5rem';
                messageDiv.appendChild(img);
            } else {
                messageDiv.innerHTML = text;
            }

            messageHistory.appendChild(messageDiv);
            messageHistory.scrollTop = messageHistory.scrollHeight;
        }

        function parseAndRenderMarkdown(markdown) {
            let html = '';
            const lines = markdown.split('\n');
            lines.forEach(line => {
                if (line.startsWith('## ')) {
                    html += `<h2 class="text-xl font-bold mt-4 mb-2">${line.substring(3).trim()}</h2>`;
                } else if (line.startsWith('* ')) {
                    html += `<p class="mb-2 ml-4">${line.trim()}</p>`;
                } else if (line.trim() !== '') {
                    html += `<p class="mb-2">${line.trim()}</p>`;
                }
            });
            return html;
        }
        
        document.addEventListener('paste', (event) => {
            const items = (event.clipboardData || event.originalEvent.clipboardData).items;
            for (const item of items) {
                if (item.type.indexOf('image') !== -1) {
                    const blob = item.getAsFile();
                    if (blob) {
                        const reader = new FileReader();
                        reader.onload = (e) => {
                            uploadedImageBase64 = e.target.result.split(',')[1];
                            uploadedImageMimeType = blob.type;
                            imagePreview.src = e.target.result;
                            imagePreviewContainer.classList.remove('hidden');
                            removeSsButton.classList.remove('hidden');
                            wordProblemInput.placeholder = 'Image pasted. Ready to solve.';
                            wordProblemInput.value = '';
                        };
                        reader.readAsDataURL(blob);
                        break;
                    }
                }
            }
        });

        imageUpload.addEventListener('change', (event) => {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    uploadedImageBase64 = e.target.result.split(',')[1];
                    uploadedImageMimeType = file.type;
                    imagePreview.src = e.target.result;
                    imagePreviewContainer.classList.remove('hidden');
                    removeSsButton.classList.remove('hidden');
                    wordProblemInput.placeholder = 'Image uploaded. Ready to solve.';
                    wordProblemInput.value = '';
                };
                reader.readAsDataURL(file);
            }
        });

        cameraUpload.addEventListener('change', (event) => {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    uploadedImageBase64 = e.target.result.split(',')[1];
                    uploadedImageMimeType = file.type;
                    imagePreview.src = e.target.result;
                    imagePreviewContainer.classList.remove('hidden');
                    removeSsButton.classList.remove('hidden');
                    wordProblemInput.placeholder = 'Image captured. Ready to solve.';
                    wordProblemInput.value = '';
                };
                reader.readAsDataURL(file);
            }
        });

        removeSsButton.addEventListener('click', () => {
            uploadedImageBase64 = null;
            uploadedImageMimeType = null;
            imagePreview.src = '';
            imagePreviewContainer.classList.add('hidden');
            removeSsButton.classList.add('hidden');
            wordProblemInput.placeholder = 'Ask ProbSolver';
        });

        async function fetchAndDisplayResponse(prompt, title, systemPrompt, useTools = false, imageParts = []) {
            loadingMessage.classList.remove('hidden');
            
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${API_KEY}`;
            
            const textParts = [{ text: prompt }];
            const parts = [...textParts, ...imageParts];

            const payload = {
                contents: [{ parts }],
                tools: useTools ? [{ "google_search": {} }] : [],
                systemInstruction: {
                    parts: [{ text: systemPrompt }]
                },
            };
            
            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) {
                    throw new Error(`API response error: ${response.status}`);
                }

                const result = await response.json();
                const text = result?.candidates?.[0]?.content?.parts?.[0]?.text || `Sorry, I couldn't generate the ${title}.`;
                
                loadingMessage.classList.add('hidden');
                
                const solutionHtml = `
                    <div class="solution-box">
                        <h2>${title}</h2>
                        ${parseAndRenderMarkdown(text)}
                        <div class="button-group">
                            <button id="explainButton" class="bg-gray-800 text-gray-300 font-medium py-2 px-4 rounded-full shadow-lg hover:bg-gray-700 transition-colors">✨ Explain Concept</button>
                            <button id="practiceButton" class="bg-gray-800 text-gray-300 font-medium py-2 px-4 rounded-full shadow-lg hover:bg-gray-700 transition-colors">✨ Practice Problems</button>
                            <button id="shortenButton" class="bg-gray-800 text-gray-300 font-medium py-2 px-4 rounded-full shadow-lg hover:bg-gray-700 transition-colors">📝 Shorten it</button>
                        </div>
                    </div>
                `;
                addMessage(solutionHtml, 'ai');
                
                const newSolutionBox = messageHistory.lastChild.querySelector('.solution-box');
                
                newSolutionBox.querySelector('#explainButton').addEventListener('click', async () => {
                    const explainPrompt = `Explain the core mathematical or physics concept behind the following problem in a simple way for a student: "${lastProblemText}".`;
                    const explainSystemPrompt = "You are a helpful and expert math tutor. Your task is to explain the core concept behind a given problem. Use simple language and markdown to make the explanation easy to understand. Do not provide a solution to the problem. The output should be a single block of text representing the formatted explanation.";
                    await fetchAndDisplayResponse(explainPrompt, 'Explanation', explainSystemPrompt);
                });
                
                newSolutionBox.querySelector('#practiceButton').addEventListener('click', async () => {
                    const practicePrompt = `Generate 3 similar practice problems based on the following problem: "${lastProblemText}". Provide the questions and then a separate answer key.`;
                    const practiceSystemPrompt = "You are a helpful and expert math tutor. Your task is to generate similar practice problems based on a provided problem. Provide the questions and a separate answer key. The entire output should be a single block of text representing the formatted practice problems and their solutions.";
                    await fetchAndDisplayResponse(practicePrompt, 'Practice Problems', practiceSystemPrompt);
                });

                newSolutionBox.querySelector('#shortenButton').addEventListener('click', async () => {
                    const shortenPrompt = `Create a very short summary of the key steps in the solution to this problem: "${lastProblemText}". The summary should be concise and perfect for study notes.`;
                    const shortenSystemPrompt = "You are a helpful and expert math tutor. Your task is to create a very short, concise summary of the key steps in a solution to a problem. The summary should be perfect for study notes. The entire output should be a single block of text representing the formatted summary.";
                    await fetchAndDisplayResponse(shortenPrompt, 'Short Notes', shortenSystemPrompt);
                });
                
                renderMathInElement(newSolutionBox, {
                    delimiters: [
                        {left: "$$", right: "$$", display: true},
                        {left: "$", right: "$", display: false},
                        {left: "\\[", right: "\\]", display: true},
                        {left: "\\(", right: "\\)", display: false}
                    ]
                });
            } catch (error) {
                loadingMessage.classList.add('hidden');
                addMessage(`<p class="text-red-500 text-center">An API error occurred. Please try again. If the problem persists, the API may be temporarily unavailable.</p>`, 'ai');
                console.error('API Error:', error);
            }
        }

        solveButton.addEventListener('click', async () => {
            const userQuery = wordProblemInput.value.trim();
            lastProblemText = userQuery;
            if (!userQuery && !uploadedImageBase64) {
                addMessage(`<p class="text-red-500 text-center">Please enter a word problem or upload an image.</p>`, 'ai');
                return;
            }
            
            titleContainer.classList.add('hidden');
            
            if (userQuery) {
                addMessage(userQuery, 'user');
            }
            if (uploadedImageBase64) {
                addMessage(null, 'user', true, imagePreview.src);
            }

            loadingMessage.classList.remove('hidden');
            wordProblemInput.value = '';
            wordProblemInput.placeholder = '';
            imagePreviewContainer.classList.add('hidden');
            removeSsButton.classList.add('hidden');

            const systemPrompt = "You are a helpful and expert math tutor. Your task is to solve the provided math word problem, which may be given as text, an image, or both. First, provide a clear, step-by-step solution. Use markdown headings (e.g., ## Step 1) to break down the process. Use LaTeX for all mathematical expressions and formulas. After the solution steps, create a final heading titled 'Final Answer' and clearly state the answer. Do not include any introductory or concluding sentences. The entire output should be a single block of text representing the formatted solution.";
            
            const imageParts = uploadedImageBase64 ? [{ inlineData: { mimeType: uploadedImageMimeType, data: uploadedImageBase64 } }] : [];
            await fetchAndDisplayResponse(userQuery, 'Solution', systemPrompt, true, imageParts);

            uploadedImageBase64 = null;
        });

        wordProblemInput.addEventListener('keydown', (event) => {
            if (event.key === 'Enter') {
                event.preventDefault();
                solveButton.click();
            }
        });

        if (window.SpeechRecognition || window.webkitSpeechRecognition) {
            const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
            const recognition = new SpeechRecognition();
            recognition.continuous = false;
            recognition.lang = 'en-US';
            recognition.interimResults = false;
            recognition.maxAlternatives = 1;
            let isRecognizing = false;

            voiceButton.addEventListener('click', () => {
                if (isRecognizing) {
                    recognition.stop();
                } else {
                    recognition.start();
                }
            });

            recognition.onstart = () => {
                isRecognizing = true;
                voiceButton.querySelector('svg').style.fill = '#34d399';
                wordProblemInput.placeholder = 'Listening...';
            };

            recognition.onresult = (event) => {
                const transcript = event.results[0][0].transcript;
                wordProblemInput.value = transcript;
            };

            recognition.onend = () => {
                isRecognizing = false;
                voiceButton.querySelector('svg').style.fill = '#9ca3af';
                wordProblemInput.placeholder = 'Ask ProbSolver';
            };

            recognition.onerror = (event) => {
                console.error('Speech recognition error:', event.error);
                wordProblemInput.placeholder = 'Error listening. Try again.';
            };
        } else {
            voiceButton.style.display = 'none';
        }
    </script>
</body>
</html>
