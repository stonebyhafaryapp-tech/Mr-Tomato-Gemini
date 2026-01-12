<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">  
    <title>Mr. Tomato: Universal</title>  
    <script src="https://cdn.tailwindcss.com"></script>  
    <style>  
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap');  
          
        :root {  
            --glass: rgba(255, 255, 255, 0.75);  
            --border: rgba(255, 255, 255, 0.4);  
        }  
  
        body {  
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;  
            background: radial-gradient(circle at top right, #f8fafc, #e2e8f0);  
            overflow: hidden;  
            user-select: none;  
            height: 100dvh;  
            width: 100vw;  
            margin: 0;  
            color: #1d1d1f;  
            display: flex;  
            flex-direction: column;  
        }  
  
        .glass-panel {  
            background: var(--glass);  
            backdrop-filter: blur(20px);  
            -webkit-backdrop-filter: blur(20px);  
            border: 1px solid var(--border);  
            box-shadow: 0 4px 24px 0 rgba(0, 0, 0, 0.04);  
        }  
  
        .apple-button {  
            background: #007aff;  
            color: white;  
            border-radius: 14px;  
            font-weight: 600;  
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);  
        }  
  
        .apple-button:active {  
            transform: scale(0.96);  
            filter: brightness(0.9);  
        }  
  
        .tomato-container {  
            width: 32vh;  
            height: 32vh;  
            max-width: 65vw;  
            max-height: 65vw;  
            min-width: 150px;  
            min-height: 150px;  
        }  
  
        .speech-bubble {  
            position: relative;  
            background: white;  
            border-radius: 18px;  
            padding: 10px 18px;  
            box-shadow: 0 8px 20px rgba(0,0,0,0.06);  
            font-weight: 600;  
            font-size: clamp(0.9rem, 2.2vh, 1.2rem);  
            max-width: 85%;  
            text-align: center;  
            border: 1px solid rgba(0,0,0,0.03);  
            margin-bottom: 1.5vh;  
        }  
  
        .food-item {  
            flex: 0 0 72px;  
            height: 72px;  
            background: white;  
            border-radius: 20px;  
            display: flex;  
            align-items: center;  
            justify-content: center;  
            box-shadow: 0 2px 8px rgba(0,0,0,0.04);  
            border: 1px solid rgba(0,0,0,0.05);  
            transition: transform 0.15s ease-out;  
            cursor: grab;  
            touch-action: pan-x;  
        }  
  
        .food-item.active-drag {  
            transform: scale(0.85);  
            opacity: 0.6;  
        }  
  
        .mixing-vibrate { animation: apple-shake 0.15s infinite; }  
        @keyframes apple-shake {  
            0% { transform: translate(1px, 1px) rotate(0deg); }  
            50% { transform: translate(-1px, -1px) rotate(0.5deg); }  
            100% { transform: translate(1px, -1px) rotate(-0.5deg); }  
        }  
  
        .no-scrollbar::-webkit-scrollbar { display: none; }  
          
        #blender {  
            width: clamp(110px, 17vh, 150px);  
            height: clamp(120px, 19vh, 170px);  
        }  
  
        #pantry {  
            display: flex;  
            gap: 12px;  
            padding: 8px;  
            overflow-x: auto;  
            scroll-snap-type: x proximity;  
            -webkit-overflow-scrolling: touch;  
        }  
  
        .hint-text {  
            font-size: 10px;  
            font-weight: 700;  
            color: #86868b;  
            text-transform: uppercase;  
            letter-spacing: 0.05em;  
        }  
  
        @supports (padding: env(safe-area-inset-top)) {  
            .safe-pt { padding-top: env(safe-area-inset-top); }  
            .safe-pb { padding-bottom: env(safe-area-inset-bottom); }  
        }  
  
        .game-controls {  
            position: fixed;  
            top: env(safe-area-inset-top, 20px);  
            right: 20px;  
            z-index: 1000;  
            pointer-events: auto !important;  
        }  
    </style>  
</head>  
<body>  
  
<div class="game-controls">  
    <button id="restart-btn" type="button" class="w-12 h-12 glass-panel rounded-full flex items-center justify-center text-[#1d1d1f] active:bg-white active:scale-90 transition-all cursor-pointer shadow-lg">  
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="M3 12a9 9 0 0 1 9-9 9.75 9.75 0 0 1 6.74 2.74L21 8"></path><path d="M21 3v5h-5"></path><path d="M21 12a9 9 0 0 1-9 9 9.75 9.75 0 0 1-6.74-2.74L3 16"></path><path d="M3 21v-5h5"></path></svg>  
    </button>  
</div>  
  
<div id="home-screen" class="fixed inset-0 bg-[#fbfbfd] flex flex-col items-center justify-center z-[200] p-8 text-center transition-opacity duration-500">  
    <div class="text-[clamp(80px,20vh,150px)] mb-6 animate-bounce">🍅</div>  
    <h1 class="text-[clamp(1.5rem,5vw,3rem)] font-extrabold tracking-tight text-[#1d1d1f] mb-2">Mr. Tomato</h1>  
    <p class="text-[clamp(1rem,2vh,1.25rem)] text-[#86868b] mb-12 font-medium">Sweet Treats Mode 🍬</p>  
    <button id="start-btn" class="apple-button px-12 py-4 text-xl shadow-xl shadow-blue-500/10">Start Session</button>  
</div>  
  
<div id="game-wrapper" class="flex flex-col h-full w-full opacity-0 transition-opacity duration-700 pointer-events-none">  
      
    <header class="w-full px-6 pt-8 pb-4 safe-pt flex flex-col gap-2 relative z-50">  
        <div class="flex justify-between items-end">  
            <div>  
                <span class="text-[10px] uppercase tracking-widest font-bold text-[#86868b] block mb-1">Score</span>  
                <span class="text-3xl font-extrabold text-[#1d1d1f]" id="points">0</span>  
            </div>  
        </div>  
        <div class="h-1.5 w-full bg-black/5 rounded-full overflow-hidden">  
            <div id="anger-fill" class="h-full bg-blue-500 transition-all duration-700 w-0"></div>  
        </div>  
    </header>  
  
    <main class="flex-grow flex flex-col items-center justify-between py-2 overflow-hidden relative pointer-events-auto">  
        <div id="speech" class="speech-bubble">Initializing...</div>  
  
        <div id="tomato" class="tomato-container relative transition-all duration-500">  
            <svg viewBox="0 0 100 100" class="w-full h-full drop-shadow-2xl">  
                <defs>  
                    <radialGradient id="t-grad" cx="50%" cy="50%" r="50%" fx="30%" fy="30%">  
                        <stop offset="0%" style="stop-color:#ff5f5f" />  
                        <stop offset="100%" style="stop-color:#ef4444" />  
                    </radialGradient>  
                </defs>  
                <circle cx="50" cy="55" r="42" fill="url(#t-grad)" />  
                <path d="M50 15 Q55 25 65 20 M50 15 Q45 25 35 20 M50 15 L50 5" stroke="#34c759" stroke-width="5" fill="none" stroke-linecap="round"/>  
                <g id="eyes">  
                    <circle cx="35" cy="45" r="5" fill="white"/><circle cx="35" cy="45" r="2.5" fill="black"/>  
                    <circle cx="65" cy="45" r="5" fill="white"/><circle cx="65" cy="45" r="2.5" fill="black"/>  
                </g>  
                <path id="mouth" d="M35 68 Q50 78 65 68" stroke="black" stroke-width="4" fill="none" stroke-linecap="round"/>  
            </svg>  
        </div>  
  
        <div id="blender" class="glass-panel rounded-[28px] relative flex flex-col items-center justify-center p-3">  
            <div class="absolute -top-3 bg-[#1d1d1f] text-white px-3 py-1 rounded-full text-[8px] font-bold uppercase tracking-widest shadow-lg">Processor</div>  
            <div id="blender-contents" class="flex gap-2 min-h-[40px] items-center justify-center flex-wrap"></div>  
            <button id="blend-btn" class="absolute -bottom-5 apple-button bg-black text-white px-8 py-2.5 text-xs hidden whitespace-nowrap">Blend Mix</button>  
        </div>  
    </main>  
  
    <footer class="w-full px-4 pb-6 safe-pb flex flex-col gap-3 relative z-50 pointer-events-auto">  
        <div class="glass-panel px-4 py-2 rounded-2xl flex items-center justify-between overflow-x-auto no-scrollbar gap-4">  
            <span class="hint-text flex-shrink-0">Recipe:</span>  
            <div class="flex items-center gap-4 whitespace-nowrap text-sm font-semibold">  
                <span>🍱 Sweet Treats = 🍓 Berry + 🍫 Choco</span>  
            </div>  
        </div>  
  
        <div class="glass-panel p-2 rounded-[32px] shadow-2xl overflow-hidden">  
            <div id="pantry" class="no-scrollbar"></div>  
        </div>  
    </footer>  
</div>  
  
<script>  
    const apiKey = "";  
    const foods = [  
        {id:'strawberry', e:'🍓', n:'Berry'}, {id:'chocolate', e:'🍫', n:'Choco'},  
        {id:'fish', e:'🐟', n:'Fish'}, {id:'ice', e:'🧊', n:'Ice'},   
        {id:'apple', e:'🍎', n:'Apple'}, {id:'lemon', e:'🍋', n:'Lemon'},  
        {id:'egg', e:'🥚', n:'Egg'}, {id:'bacon', e:'🥓', n:'Bacon'}  
    ];  
    const recipes = {  
        'strawberry+chocolate': {id:'sweet', e:'🍱', n:'Sweet Treats'},  
        'fish+ice': {id:'sushi', e:'🍣', n:'Sushi'},  
        'lemon+ice': {id:'lemonade', e:'🥤', n:'Juice'},  
        'egg+bacon': {id:'breakfast', e:'🍳', n:'Breakfast'}  
    };  
  
    let points = 0, anger = 0, currentGoal = null, blenderItems = [], isBusy = false;  
    let audioCtx = null;  
  
    function playSound(type) {  
        if (!audioCtx) return;  
        const osc = audioCtx.createOscillator();  
        const gain = audioCtx.createGain();  
        osc.connect(gain); gain.connect(audioCtx.destination);  
        const now = audioCtx.currentTime;  
        if (type === 'pickup') {  
            osc.frequency.setValueAtTime(440, now); osc.frequency.exponentialRampToValueAtTime(880, now + 0.1);  
        } else if (type === 'success') {  
            osc.frequency.setValueAtTime(523, now); osc.frequency.setValueAtTime(783, now + 0.2);  
        }  
        gain.gain.setValueAtTime(0.05, now); gain.gain.linearRampToValueAtTime(0, now + 0.2);  
        osc.start(now); osc.stop(now + 0.2);  
    }  
  
    async function speak(text, mood = 'happy') {  
        if (!audioCtx) return;  
        try {  
            const voice = mood === 'angry' ? "Fenrir" : "Puck";  
            const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${apiKey}`, {  
                method: 'POST', headers: { 'Content-Type': 'application/json' },  
                body: JSON.stringify({  
                    contents: [{ parts: [{ text }] }],  
                    generationConfig: {  
                        responseModalities: ["AUDIO"],  
                        speechConfig: { voiceConfig: { prebuiltVoiceConfig: { voiceName: voice } } }  
                    },  
                    model: "gemini-2.5-flash-preview-tts"  
                })  
            });  
            const result = await response.json();  
            const data = result.candidates?.[0]?.content?.parts?.[0]?.inlineData?.data;  
            if (!data) return;  
            const binary = atob(data);  
            const bytes = new Int16Array(binary.length / 2);  
            for (let i = 0; i < binary.length; i += 2) bytes[i/2] = (binary.charCodeAt(i+1) << 8) | binary.charCodeAt(i);  
            const buffer = audioCtx.createBuffer(1, bytes.length, 24000);  
            buffer.getChannelData(0).set(Array.from(bytes).map(v => v / 32768));  
            const source = audioCtx.createBufferSource();  
            source.buffer = buffer; source.connect(audioCtx.destination); source.start();  
        } catch (e) {}  
    }  
  
    function initPantry() {  
        const p = document.getElementById('pantry');  
        p.innerHTML = '';  
        foods.forEach(f => {  
            const d = document.createElement('div');  
            d.className = 'food-item snap-center';  
            d.innerHTML = `<span class="text-4xl">${f.e}</span>`;  
            const startInteraction = (e) => {  
                if (isBusy) return;  
                const clientX = e.type.includes('touch') ? e.touches[0].clientX : e.clientX;  
                const clientY = e.type.includes('touch') ? e.touches[0].clientY : e.clientY;  
                playSound('pickup');  
                handleDrag(clientX, clientY, f, d);  
            };  
            d.addEventListener('mousedown', startInteraction);  
            d.addEventListener('touchstart', startInteraction, {passive: true});  
            p.appendChild(d);  
        });  
    }  
  
    function handleDrag(startX, startY, food, originalEl) {  
        const clone = document.createElement('div');  
        clone.className = 'fixed pointer-events-none z-[300] text-5xl transform -translate-x-1/2 -translate-y-1/2';  
        clone.innerText = food.e;  
        clone.style.left = `${startX}px`; clone.style.top = `${startY}px`;  
        document.body.appendChild(clone);  
        const onMove = (e) => {  
            const x = e.type.includes('touch') ? e.touches[0].clientX : e.clientX;  
            const y = e.type.includes('touch') ? e.touches[0].clientY : e.clientY;  
            clone.style.left = `${x}px`; clone.style.top = `${y}px`;  
        };  
        const onEnd = (e) => {  
            const x = e.type.includes('touch') ? (e.changedTouches ? e.changedTouches[0].clientX : startX) : e.clientX;  
            const y = e.type.includes('touch') ? (e.changedTouches ? e.changedTouches[0].clientY : startY) : e.clientY;  
            const b = document.getElementById('blender').getBoundingClientRect();  
            const t = document.getElementById('tomato').getBoundingClientRect();  
            if (x > b.left && x < b.right && y > b.top && y < b.bottom) addToBlender(food);  
            else if (x > t.left && x < t.right && y > t.top && y < t.bottom) handleFeed(food.id);  
            clone.remove();  
            window.removeEventListener('mousemove', onMove); window.removeEventListener('mouseup', onEnd);  
            window.removeEventListener('touchmove', onMove); window.removeEventListener('touchend', onEnd);  
        };  
        window.addEventListener('mousemove', onMove); window.addEventListener('mouseup', onEnd);  
        window.addEventListener('touchmove', onMove); window.addEventListener('touchend', onEnd);  
    }  
  
    function handleFeed(id) {  
        if (isBusy) return;  
        isBusy = true;  
        const tomato = document.getElementById('tomato');  
        if (id === currentGoal.id) {  
            playSound('success'); points += 100; anger = Math.max(0, anger - 2);  
            speak("Finally! Sweet treats for me!", "happy");  
            document.getElementById('speech').innerText = "Sweetness!";  
            tomato.style.transform = 'scale(1.2)';  
            setTimeout(() => { tomato.style.transform = 'scale(1)'; isBusy = false; nextTask(); }, 1800);  
        } else {  
            anger = Math.min(10, anger + 2.5);  
            speak("I only want sweet treats!", "angry");  
            document.getElementById('speech').innerText = "Not sweet!";  
            tomato.classList.add('mixing-vibrate');  
            setTimeout(() => { tomato.classList.remove('mixing-vibrate'); isBusy = false; updateUI(); }, 1500);  
        }  
        updateUI();  
    }  
  
    function addToBlender(food) {  
        if (blenderItems.length >= 2 || isBusy) return;  
        blenderItems.push(food);  
        const s = document.createElement('span'); s.innerText = food.e; s.className = 'text-3xl animate-bounce';  
        document.getElementById('blender-contents').appendChild(s);  
        if (blenderItems.length === 2) document.getElementById('blend-btn').classList.remove('hidden');  
    }  
  
    document.getElementById('blend-btn').onclick = () => {  
        isBusy = true; document.getElementById('blend-btn').classList.add('hidden');  
        document.getElementById('blender').classList.add('mixing-vibrate');  
        const k1 = `${blenderItems[0].id}+${blenderItems[1].id}`, k2 = `${blenderItems[1].id}+${blenderItems[0].id}`;  
        const res = recipes[k1] || recipes[k2];  
        setTimeout(() => {  
            document.getElementById('blender').classList.remove('mixing-vibrate');  
            document.getElementById('blender-contents').innerHTML = '';  
            if (res) {  
                const rEl = document.createElement('span'); rEl.innerText = res.e; rEl.className = 'text-5xl';  
                document.getElementById('blender-contents').appendChild(rEl);  
                setTimeout(() => { blenderItems = []; document.getElementById('blender-contents').innerHTML = ''; isBusy = false; handleFeed(res.id); }, 1200);  
            } else {  
                anger = Math.min(10, anger + 1); updateUI();  
                setTimeout(() => { blenderItems = []; isBusy = false; document.getElementById('blender-contents').innerHTML = ''; }, 1200);  
            }  
        }, 1200);  
    };  
  
    function updateUI() {  
        document.getElementById('points').innerText = points;  
        const fill = document.getElementById('anger-fill');  
        fill.style.width = `${anger * 10}%`;  
        fill.className = `h-full transition-all duration-700 ${anger > 7 ? 'bg-red-500' : anger > 4 ? 'bg-orange-400' : 'bg-blue-500'}`;  
        if (anger >= 10) { speak("You failed to give me my sweets!", "angry"); setTimeout(() => window.location.reload(), 3000); }  
    }  
  
    function nextTask() {  
        // Enforce Sweet Treats as the only goal  
        currentGoal = recipes['strawberry+chocolate'];  
        document.getElementById('speech').innerHTML = `I want: <span class="font-extrabold text-pink-500">${currentGoal.n}</span>`;  
        speak(`Can I have some Sweet Treats, please?`, anger > 7 ? 'angry' : 'happy');  
        updateUI();  
    }  
  
    document.getElementById('start-btn').onclick = async () => {  
        audioCtx = new (window.AudioContext || window.webkitAudioContext)();  
        document.getElementById('home-screen').style.opacity = '0';  
        setTimeout(() => {  
            document.getElementById('home-screen').style.display = 'none';  
            document.getElementById('game-wrapper').style.opacity = '1';  
            document.getElementById('game-wrapper').style.pointerEvents = 'auto';  
            initPantry(); nextTask();  
        }, 500);  
    };  
  
    const restartBtn = document.getElementById('restart-btn');  
    const doRestart = (e) => { e.preventDefault(); window.location.reload(); };  
    restartBtn.onclick = doRestart;  
    restartBtn.ontouchend = doRestart;  
</script>  
</body>  
</html>  
  
