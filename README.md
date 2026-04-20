[index.html](https://github.com/user-attachments/files/26884281/index.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>萨克斯练习工具</title>
    <link rel="manifest" href="manifest.json">
    <meta name="theme-color" content="#1565c0">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="萨克斯练习工具">
    <link rel="apple-touch-icon" href="应用图标.png">
    <style>
        * { box-sizing: border-box; }
        body {
            font-family: 'Microsoft YaHei', sans-serif;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            margin: 0;
            padding: 20px;
        }

        h1 {
            font-size: 24px;
            color: #a0a0a0;
            margin-bottom: 25px;
            text-align: center;
        }

        /* 上排：计数器 + 计时器 */
        .top-row {
            display: flex;
            gap: 30px;
            flex-wrap: wrap;
            justify-content: center;
            margin-bottom: 25px;
        }

        .module {
            background: rgba(255,255,255,0.08);
            border-radius: 20px;
            padding: 25px 0;
            text-align: center;
            width: 300px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.3);
            border: 1px solid rgba(255,255,255,0.1);
        }

        /* 分隔线 */
        .divider {
            width: 630px;
            height: 1px;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            margin-bottom: 25px;
        }

        .module-title {
            font-size: 16px;
            color: #888;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .counter-display {
            font-size: 100px;
            font-weight: 900;
            padding: 15px;
            display: inline-block;
            transition: all 0.3s;
        }

        .timer-display {
            font-size: 70px;
            font-weight: 700;
            color: #00ff88;
            text-shadow: 0 0 20px #00ff88;
            padding: 10px 20px;
            font-variant-numeric: tabular-nums;
            transition: all 0.3s;
        }

        .timer-display.running {
            animation: pulse 1s ease-in-out infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }

        .btn-group {
            display: flex;
            gap: 10px;
            justify-content: center;
            flex-wrap: wrap;
            margin-top: 15px;
        }

        button {
            padding: 10px 18px;
            font-size: 15px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.2s;
        }

        button:hover { transform: scale(1.05); }
        button:active { transform: scale(0.95); }

        .btn-minus { background: #e74c3c; color: white; }
        .btn-plus { background: linear-gradient(135deg, #667eea, #764ba2); color: white; }
        .btn-reset { background: #555; color: white; }
        .btn-start { background: #27ae60; color: white; }
        .btn-pause { background: #f39c12; color: white; }
        .btn-stop { background: #c0392b; color: white; }

        .progress-container {
            width: 100%;
            max-width: 250px;
            height: 12px;
            background: rgba(255,255,255,0.1);
            border-radius: 6px;
            margin: 12px auto;
            overflow: hidden;
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.3);
        }

        .progress-bar {
            height: 100%;
            border-radius: 6px;
            width: 0%;
            transition: all 0.3s;
            box-shadow: 0 0 10px currentColor;
        }

        .progress-label {
            color: #aaa;
            font-size: 13px;
            margin-top: 5px;
        }

        .stats {
            margin-top: 15px;
            padding: 12px;
            background: rgba(0,0,0,0.2);
            border-radius: 8px;
            font-size: 13px;
            color: #888;
        }

        .stats div { margin: 5px 0; }
        .stats span { color: #fff; font-weight: bold; }

        /* 下排：节拍器 */
        .metronome-module {
            background: rgba(255,255,255,0.08);
            border-radius: 20px;
            padding: 25px 0;
            text-align: center;
            width: 630px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.3);
            border: 1px solid rgba(255,255,255,0.1);
        }

        .bpm-display {
            font-size: 80px;
            font-weight: 900;
            padding: 10px;
            display: inline-block;
            cursor: pointer;
            transition: all 0.3s;
            border: 2px dashed transparent;
            border-radius: 10px;
        }

        .bpm-display:hover {
            border-color: rgba(255,255,255,0.3);
        }

        .bpm-input {
            width: 120px;
            font-size: 60px;
            text-align: center;
            background: rgba(0,0,0,0.3);
            border: 2px solid #667eea;
            border-radius: 10px;
            color: white;
            padding: 5px;
        }

        .bpm-label {
            font-size: 14px;
            color: #888;
            margin-top: 5px;
        }

        .beat-indicator {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin: 20px 0;
        }

        .beat-light {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: rgba(255,255,255,0.2);
            transition: all 0.1s;
        }

        .beat-light.active {
            background: currentColor;
            box-shadow: 0 0 20px currentColor;
            transform: scale(1.3);
        }

        .time-sig {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin: 15px 0;
        }

        .time-sig-btn {
            padding: 8px 15px;
            font-size: 14px;
            background: rgba(255,255,255,0.1);
            color: #aaa;
            border: 1px solid rgba(255,255,255,0.2);
        }

        .time-sig-btn.active {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border-color: transparent;
        }

        .preset-btns {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin: 15px 0;
        }

        .preset-btn {
            padding: 8px 16px;
            font-size: 13px;
            background: linear-gradient(135deg, rgba(255,255,255,0.15), rgba(255,255,255,0.05));
            color: #ccc;
            border: 1px solid rgba(255,255,255,0.15);
        }

        .preset-btn:hover {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border-color: transparent;
            transform: scale(1.05);
        }

        .slider-container {
            margin: 15px auto;
            width: 80%;
        }

        input[type="range"] {
            width: 100%;
            height: 8px;
            -webkit-appearance: none;
            background: rgba(255,255,255,0.2);
            border-radius: 4px;
            outline: none;
        }

        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 20px;
            height: 20px;
            background: linear-gradient(135deg, #667eea, #764ba2);
            border-radius: 50%;
            cursor: pointer;
        }

        /* 粒子特效 */
        .particle {
            position: fixed;
            font-size: 24px;
            pointer-events: none;
            animation: float-up 1s ease-out forwards;
        }

        @keyframes float-up {
            0% { opacity: 1; transform: translateY(0) scale(1); }
            100% { opacity: 0; transform: translateY(-150px) scale(0.3); }
        }

        .achievement {
            position: fixed;
            top: 15%;
            left: 50%;
            transform: translateX(-50%);
            background: linear-gradient(135deg, #FFD700, #FFA500);
            color: #1a1a2e;
            padding: 20px 40px;
            border-radius: 12px;
            font-size: 24px;
            font-weight: bold;
            animation: pop-in 0.4s ease-out, fade-out 2s ease-in 2s forwards;
            box-shadow: 0 0 30px rgba(255,215,0,0.6);
            z-index: 100;
        }

        @keyframes pop-in {
            0% { transform: translateX(-50%) scale(0.5); }
            70% { transform: translateX(-50%) scale(1.1); }
            100% { transform: translateX(-50%) scale(1); }
        }

        @keyframes fade-out {
            to { opacity: 0; transform: translateX(-50%) translateY(-20px); }
        }

        .confetti {
            position: fixed;
            width: 10px;
            height: 10px;
            pointer-events: none;
            animation: fall 2s ease-out forwards;
        }

        @keyframes fall {
            to { transform: translateY(100vh) rotate(720deg); opacity: 0; }
        }

        .explosion-ring {
            position: fixed;
            width: 60px;
            height: 60px;
            border: 3px solid;
            border-radius: 50%;
            pointer-events: none;
            animation: ring-expand 0.5s ease-out forwards;
        }

        @keyframes ring-expand {
            to { transform: scale(3); opacity: 0; }
        }

        /* 彩虹动画 */
        .rainbow-text {
            background: linear-gradient(90deg, #ff0000, #ff7f00, #ffff00, #00ff00, #0000ff, #4b0082, #9400d3);
            background-size: 200% auto;
            background-clip: text;
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: rainbow-move 1s linear infinite;
        }

        @keyframes rainbow-move {
            to { background-position: 200% 0; }
        }

        .sound-btn {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: rgba(255,255,255,0.1);
            color: #888;
            padding: 10px 15px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
        }

        .subtitle {
            color: #666;
            margin-top: 20px;
            font-size: 13px;
        }

        @media (max-width: 700px) {
            .top-row { flex-direction: column; }
            .divider { width: 100%; }
            .counter-display { font-size: 70px; }
            .timer-display { font-size: 50px; }
            .metronome-module { min-width: auto; width: 100%; }
            .bpm-display { font-size: 60px; }
        }
    </style>
</head>
<body>
    <h1>🎷 萨克斯练习工具</h1>

    <!-- 上排：计数器 + 计时器 -->
    <div class="top-row">
        <!-- 计数器 -->
        <div class="module">
            <div class="module-title">📊 练习计数</div>
            <div class="counter-display" id="display">0</div>
            <div class="progress-container">
                <div class="progress-bar" id="progress"></div>
            </div>
            <div class="progress-label" id="progress-label">下一成就: 10次</div>
            <div class="btn-group">
                <button class="btn-minus" onclick="decrease()">-1</button>
                <button class="btn-reset" onclick="resetCount()">重置</button>
                <button class="btn-plus" onclick="increase()">+1</button>
            </div>
        </div>

        <!-- 计时器 -->
        <div class="module">
            <div class="module-title">⏱️ 练习计时</div>
            <div class="timer-display" id="timerDisplay">00:00</div>
            <div class="btn-group">
                <button class="btn-start" onclick="startTimer()">▶ 开始</button>
                <button class="btn-pause" onclick="pauseTimer()">⏸ 暂停</button>
                <button class="btn-stop" onclick="stopTimer()">⏹ 重置</button>
            </div>
            <div class="stats">
                <div>本次练习: <span id="sessionTime">00:00</span></div>
                <div>累计时长: <span id="totalTime">00:00</span></div>
            </div>
        </div>
    </div>

    <div class="divider"></div>

    <!-- 下排：节拍器 -->
    <div class="metronome-module">
        <div class="module-title">🎵 节拍器</div>
        
        <div class="bpm-display" id="bpmDisplay" onclick="editBpm()">80</div>
        <div class="bpm-label">BPM · <span id="timeSigDisplay">4/4</span></div>

        <!-- 节拍指示灯 -->
        <div class="beat-indicator" id="beatIndicator">
            <div class="beat-light" data-beat="0"></div>
            <div class="beat-light" data-beat="1"></div>
            <div class="beat-light" data-beat="2"></div>
            <div class="beat-light" data-beat="3"></div>
        </div>

        <!-- BPM 滑动条 -->
        <div class="slider-container">
            <input type="range" id="bpmSlider" min="40" max="240" value="80" oninput="updateBpm(this.value)">
        </div>

        <!-- 微调按钮 -->
        <div class="btn-group">
            <button class="btn-minus" onclick="adjustBpm(-5)">-5</button>
            <button class="btn-minus" onclick="adjustBpm(-1)">-1</button>
            <button class="btn-reset" onclick="editBpm()">输入</button>
            <button class="btn-plus" onclick="adjustBpm(1)">+1</button>
            <button class="btn-plus" onclick="adjustBpm(5)">+5</button>
        </div>

        <!-- 拍号选择 -->
        <div class="time-sig">
            <button class="time-sig-btn active" data-sig="4/4" onclick="setTimeSig('4/4')">4/4</button>
            <button class="time-sig-btn" data-sig="3/4" onclick="setTimeSig('3/4')">3/4</button>
            <button class="time-sig-btn" data-sig="2/4" onclick="setTimeSig('2/4')">2/4</button>
            <button class="time-sig-btn" data-sig="6/8" onclick="setTimeSig('6/8')">6/8</button>
        </div>

        <!-- 预设速度 -->
        <div class="preset-btns">
            <button class="preset-btn" onclick="setBpm(60)">慢练 60</button>
            <button class="preset-btn" onclick="setBpm(80)">标准 80</button>
            <button class="preset-btn" onclick="setBpm(100)">正常 100</button>
            <button class="preset-btn" onclick="setBpm(120)">加速 120</button>
        </div>

        <!-- 播放控制 -->
        <div class="btn-group">
            <button class="btn-start" onclick="toggleMetronome()" id="metronomeBtn">▶ 开始</button>
        </div>
    </div>

    <p class="subtitle">如有疑问，可联系 1034058528@qq.com</p>

    <button class="sound-btn" onclick="toggleSound()" id="soundBtn">🔊 音效</button>

    <script>
        // ============ 通用颜色系统 ============
        const levels = [
            { min: 0, color: '#ffffff', shadow: 'white', name: '白' },
            { min: 5, color: '#87CEEB', shadow: '#87CEEB', name: '天蓝' },
            { min: 10, color: '#98FB98', shadow: '#98FB98', name: '浅绿' },
            { min: 20, color: '#FFD700', shadow: '#FFD700', name: '金黄' },
            { min: 30, color: '#FF69B4', shadow: '#FF69B4', name: '粉红' },
            { min: 40, color: '#DA70D6', shadow: '#DA70D6', name: '紫兰' },
            { min: 50, color: '#FF6347', shadow: '#FF6347', name: '番茄' },
            { min: 75, color: '#7B68EE', shadow: '#7B68EE', name: '紫蓝' },
            { min: 100, color: '#00FFFF', shadow: '#00FFFF', name: '青色' },
            { min: 150, color: '#FF00FF', shadow: '#FF00FF', name: '品红' },
            { min: 200, color: 'rainbow', shadow: '#FF0000', name: '彩虹' },
        ];

        function getLevel(count) {
            let level = levels[0];
            for (let l of levels) {
                if (count >= l.min) level = l;
            }
            return level;
        }

        function applyColor(el, level) {
            el.style.color = level.color;
            el.style.textShadow = `0 0 30px ${level.shadow}`;
            el.style.webkitTextFillColor = level.color;
            el.style.background = 'none';
            el.style.webkitBackgroundClip = 'initial';
            el.style.backgroundClip = 'initial';
            el.style.animation = 'none';

            if (level.color === 'rainbow') {
                el.style.background = 'linear-gradient(90deg, #ff0000, #ff7f00, #ffff00, #00ff00, #0000ff, #4b0082, #9400d3)';
                el.style.backgroundSize = '200% auto';
                el.style.backgroundClip = 'text';
                el.style.webkitBackgroundClip = 'text';
                el.style.webkitTextFillColor = 'transparent';
                el.style.animation = 'rainbow-move 1s linear infinite';
            }
        }

        // ============ 计数器 ============
        let count = 0;
        let achieved = new Set();
        let soundOn = true;
        let audioCtx = null;

        const achievements = {
            10: '🌟 初露锋芒', 25: '🎯 小有所成', 50: '🔥 坚持不懈',
            75: '⭐ 渐入佳境', 100: '🏆 百次达成', 150: '💫 高手风范',
            200: '👑 萨克斯王者', 300: '🎭 登峰造极', 500: '🌈 传奇之路',
        };

        function initAudio() {
            if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            if (audioCtx.state === 'suspended') audioCtx.resume();
        }

        function play(type) {
            if (!soundOn) return;
            initAudio();
            const t = audioCtx.currentTime;

            if (type === 'click') {
                const o = audioCtx.createOscillator();
                const g = audioCtx.createGain();
                o.connect(g); g.connect(audioCtx.destination);
                o.frequency.value = 500 + count * 3;
                o.type = 'sine';
                g.gain.setValueAtTime(0.3, t);
                g.gain.exponentialRampToValueAtTime(0.01, t + 0.1);
                o.start(t); o.stop(t + 0.1);
            } else if (type === 'milestone') {
                [523, 659, 784, 1047].forEach((f, i) => {
                    const o = audioCtx.createOscillator();
                    const g = audioCtx.createGain();
                    o.connect(g); g.connect(audioCtx.destination);
                    o.frequency.value = f; o.type = 'sine';
                    g.gain.setValueAtTime(0.25, t + i*0.1);
                    g.gain.exponentialRampToValueAtTime(0.01, t + i*0.1 + 0.2);
                    o.start(t + i*0.1); o.stop(t + i*0.1 + 0.2);
                });
            } else if (type === 'achievement') {
                [523, 659, 784].forEach((f) => {
                    const o = audioCtx.createOscillator();
                    const g = audioCtx.createGain();
                    o.connect(g); g.connect(audioCtx.destination);
                    o.frequency.value = f; o.type = 'triangle';
                    g.gain.setValueAtTime(0.2, t);
                    g.gain.exponentialRampToValueAtTime(0.01, t + 0.8);
                    o.start(t); o.stop(t + 0.8);
                });
            } else if (type === 'rainbow') {
                [262, 294, 330, 349, 392, 440, 494, 523, 587, 659, 698, 784].forEach((f, i) => {
                    const o = audioCtx.createOscillator();
                    const g = audioCtx.createGain();
                    o.connect(g); g.connect(audioCtx.destination);
                    o.frequency.value = f; o.type = 'sine';
                    g.gain.setValueAtTime(0.12, t + i*0.03);
                    g.gain.exponentialRampToValueAtTime(0.01, t + i*0.03 + 0.12);
                    o.start(t + i*0.03); o.stop(t + i*0.03 + 0.12);
                });
            }
        }

        function toggleSound() {
            soundOn = !soundOn;
            document.getElementById('soundBtn').textContent = soundOn ? '🔊 音效' : '🔇 静音';
        }

        function updateCount() {
            const el = document.getElementById('display');
            const level = getLevel(count);
            applyColor(el, level);
            el.textContent = count;

            const next = Object.keys(achievements).find(k => count < parseInt(k)) || '500';
            const prev = Object.keys(achievements).reverse().find(k => count >= parseInt(k)) || '0';
            const pct = ((count - parseInt(prev)) / (parseInt(next) - parseInt(prev))) * 100;
            document.getElementById('progress').style.width = Math.min(pct, 100) + '%';
            document.getElementById('progress').style.background = level.color === 'rainbow' 
                ? 'linear-gradient(90deg, #ff0000, #ff7f00, #ffff00, #00ff00, #0000ff)' 
                : level.color;
            document.getElementById('progress-label').textContent = `下一成就: ${next}次 [${level.name}]`;
            
            // 同步更新计时器和节拍器颜色
            updateTimerDisplay();
            updateMetronomeColor();
        }

        function particles(x, y) {
            const emojis = ['✨', '⭐', '🌟', '💫', '🎵', '🎶', '♪', '🔥', '💥', '🎷'];
            for (let i = 0; i < 5; i++) {
                const p = document.createElement('div');
                p.className = 'particle';
                p.textContent = emojis[Math.floor(Math.random() * emojis.length)];
                p.style.left = (x + (Math.random() - 0.5) * 100) + 'px';
                p.style.top = (y + (Math.random() - 0.5) * 50) + 'px';
                document.body.appendChild(p);
                setTimeout(() => p.remove(), 1000);
            }
        }

        function explosion(x, y) {
            const colors = ['#FFD700', '#FF6347', '#FF69B4', '#00CED1', '#7FFF00'];
            colors.forEach(c => {
                const r = document.createElement('div');
                r.className = 'explosion-ring';
                r.style.borderColor = c;
                r.style.left = (x - 30) + 'px';
                r.style.top = (y - 30) + 'px';
                document.body.appendChild(r);
                setTimeout(() => r.remove(), 500);
            });
        }

        function confetti() {
            const colors = ['#ff0000', '#ff7f00', '#ffff00', '#00ff00', '#0000ff', '#9400d3', '#ff1493'];
            for (let i = 0; i < 30; i++) {
                const c = document.createElement('div');
                c.className = 'confetti';
                c.style.left = Math.random() * window.innerWidth + 'px';
                c.style.top = '-10px';
                c.style.background = colors[Math.floor(Math.random() * colors.length)];
                c.style.animationDelay = Math.random() * 0.3 + 's';
                c.style.borderRadius = Math.random() > 0.5 ? '50%' : '0';
                document.body.appendChild(c);
                setTimeout(() => c.remove(), 2500);
            }
        }

        function showAchievement(text) {
            const a = document.createElement('div');
            a.className = 'achievement';
            a.textContent = text;
            document.body.appendChild(a);
            setTimeout(() => a.remove(), 2500);
        }

        function increase() {
            const wasRainbow = getLevel(count).color === 'rainbow';
            count++;
            updateCount();

            const btn = document.querySelector('.btn-plus');
            const rect = btn.getBoundingClientRect();
            particles(rect.left + 20, rect.top);

            if (count % 10 === 0) {
                explosion(rect.left + 20, rect.top);
                play('milestone');
            }

            if (!wasRainbow && getLevel(count).color === 'rainbow') {
                play('rainbow');
                confetti();
            }

            if (achievements[count] && !achieved.has(count)) {
                achieved.add(count);
                showAchievement(achievements[count]);
                play('achievement');
                confetti();
            } else {
                play('click');
            }
        }

        function decrease() {
            if (count > 0) {
                count--;
                updateCount();
                play('click');
            }
        }

        function resetCount() {
            count = 0;
            achieved.clear();
            updateCount();
            play('click');
        }

        // ============ 计时器 ============
        let timerSeconds = 0;
        let timerInterval = null;
        let isRunning = false;
        let totalSeconds = 0;

        function formatTime(seconds) {
            const h = Math.floor(seconds / 3600);
            const m = Math.floor((seconds % 3600) / 60);
            const s = seconds % 60;
            if (h > 0) {
                return `${h}:${String(m).padStart(2, '0')}:${String(s).padStart(2, '0')}`;
            }
            return `${String(m).padStart(2, '0')}:${String(s).padStart(2, '0')}`;
        }

        function updateTimerDisplay() {
            const el = document.getElementById('timerDisplay');
            const level = getLevel(count);
            applyColor(el, level);
            el.textContent = formatTime(timerSeconds);
            document.getElementById('sessionTime').textContent = formatTime(timerSeconds);
            document.getElementById('totalTime').textContent = formatTime(totalSeconds);
        }

        function startTimer() {
            if (isRunning) return;
            isRunning = true;
            document.getElementById('timerDisplay').classList.add('running');
            timerInterval = setInterval(() => {
                timerSeconds++;
                totalSeconds++;
                updateTimerDisplay();
            }, 1000);
        }

        function pauseTimer() {
            if (!isRunning) return;
            isRunning = false;
            document.getElementById('timerDisplay').classList.remove('running');
            clearInterval(timerInterval);
        }

        function stopTimer() {
            pauseTimer();
            timerSeconds = 0;
            updateTimerDisplay();
        }

        // ============ 节拍器 ============
        let bpm = 80;
        let beatsPerMeasure = 4;
        let metronomeInterval = null;
        let currentBeat = 0;
        let metronomeRunning = false;

        function updateBpmDisplay() {
            const el = document.getElementById('bpmDisplay');
            el.textContent = bpm;
            document.getElementById('bpmSlider').value = bpm;
        }

        function updateBpm(value) {
            bpm = parseInt(value);
            updateBpmDisplay();
            if (metronomeRunning) {
                stopMetronomeBeat();
                startMetronomeBeat();
            }
        }

        function adjustBpm(delta) {
            bpm = Math.max(40, Math.min(240, bpm + delta));
            updateBpmDisplay();
            if (metronomeRunning) {
                stopMetronomeBeat();
                startMetronomeBeat();
            }
        }

        function editBpm() {
            const input = prompt('输入 BPM (40-240):', bpm);
            if (input !== null) {
                const newBpm = parseInt(input);
                if (!isNaN(newBpm) && newBpm >= 40 && newBpm <= 240) {
                    bpm = newBpm;
                    updateBpmDisplay();
                    if (metronomeRunning) {
                        stopMetronomeBeat();
                        startMetronomeBeat();
                    }
                }
            }
        }

        function setBpm(value) {
            bpm = value;
            updateBpmDisplay();
            if (metronomeRunning) {
                stopMetronomeBeat();
                startMetronomeBeat();
            }
        }

        function setTimeSig(sig) {
            document.querySelectorAll('.time-sig-btn').forEach(btn => {
                btn.classList.toggle('active', btn.dataset.sig === sig);
            });
            beatsPerMeasure = parseInt(sig.split('/')[0]);
            document.getElementById('timeSigDisplay').textContent = sig;
            
            // 更新节拍指示灯数量
            const indicator = document.getElementById('beatIndicator');
            indicator.innerHTML = '';
            for (let i = 0; i < beatsPerMeasure; i++) {
                const light = document.createElement('div');
                light.className = 'beat-light';
                light.dataset.beat = i;
                indicator.appendChild(light);
            }
            
            // 重新应用颜色
            updateMetronomeColor();
            
            if (metronomeRunning) {
                stopMetronomeBeat();
                startMetronomeBeat();
            }
        }

        function updateMetronomeColor() {
            const level = getLevel(count);
            
            // BPM数字颜色同步
            const bpmDisplay = document.getElementById('bpmDisplay');
            bpmDisplay.style.color = level.color === 'rainbow' ? '#ff7f00' : level.color;
            bpmDisplay.style.textShadow = `0 0 30px ${level.color === 'rainbow' ? '#ff7f00' : level.color}`;
            if (level.color === 'rainbow') {
                bpmDisplay.style.background = 'linear-gradient(90deg, #ff0000, #ff7f00, #ffff00, #00ff00, #0000ff, #4b0082, #9400d3)';
                bpmDisplay.style.backgroundSize = '200% auto';
                bpmDisplay.style.backgroundClip = 'text';
                bpmDisplay.style.webkitBackgroundClip = 'text';
                bpmDisplay.style.webkitTextFillColor = 'transparent';
                bpmDisplay.style.animation = 'rainbow-move 1s linear infinite';
            } else {
                bpmDisplay.style.background = 'none';
                bpmDisplay.style.webkitTextFillColor = level.color;
                bpmDisplay.style.animation = 'none';
            }
            
            // 节拍灯颜色同步
            const lights = document.querySelectorAll('.beat-light');
            lights.forEach(light => {
                light.style.color = level.color === 'rainbow' ? '#ff7f00' : level.color;
            });
        }

        function playMetronomeSound(isAccent) {
            if (!soundOn) return;
            initAudio();
            const t = audioCtx.currentTime;
            const o = audioCtx.createOscillator();
            const g = audioCtx.createGain();
            o.connect(g);
            g.connect(audioCtx.destination);
            
            if (isAccent) {
                o.frequency.value = 1000;
                g.gain.setValueAtTime(0.5, t);
            } else {
                o.frequency.value = 600;
                g.gain.setValueAtTime(0.3, t);
            }
            g.gain.exponentialRampToValueAtTime(0.01, t + 0.08);
            o.start(t);
            o.stop(t + 0.08);
        }

        function tick() {
            const lights = document.querySelectorAll('.beat-light');
            lights.forEach(light => light.classList.remove('active'));
            if (lights[currentBeat]) {
                lights[currentBeat].classList.add('active');
            }
            
            playMetronomeSound(currentBeat === 0);
            
            currentBeat = (currentBeat + 1) % beatsPerMeasure;
        }

        function startMetronomeBeat() {
            currentBeat = 0;
            tick();
            metronomeInterval = setInterval(tick, 60000 / bpm);
        }

        function stopMetronomeBeat() {
            clearInterval(metronomeInterval);
            document.querySelectorAll('.beat-light').forEach(light => light.classList.remove('active'));
        }

        function toggleMetronome() {
            const btn = document.getElementById('metronomeBtn');
            if (metronomeRunning) {
                stopMetronomeBeat();
                btn.textContent = '▶ 开始';
                metronomeRunning = false;
            } else {
                startMetronomeBeat();
                btn.textContent = '⏸ 暂停';
                metronomeRunning = true;
            }
        }

        // 键盘
        document.addEventListener('keydown', e => {
            if (e.target.tagName === 'INPUT') return;
            if (e.key === 'ArrowRight' || e.key === 'd') increase();
            else if (e.key === 'ArrowLeft' || e.key === 'a') decrease();
            else if (e.key === ' ') { e.preventDefault(); resetCount(); }
        });

        // 初始化
        updateCount();
        updateTimerDisplay();
        updateBpmDisplay();
        updateMetronomeColor();

        // Service Worker 注册
        if ('serviceWorker' in navigator) {
            navigator.serviceWorker.register('sw.js')
                .then(reg => console.log('SW 注册成功'))
                .catch(err => console.log('SW 注册失败', err));
        }
    </script>
</body>
</html>
