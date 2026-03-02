# error404[index.html](https://github.com/user-attachments/files/25684136/index.html)
[index.html](https://github.com/user-attachments/files/25684136/index.html)
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>TERMINAL_ACCESS_PORT_8080</title>
    <style>
        body, html {
            margin: 0; padding: 0; width: 100%; height: 100%;
            background-color: #000; overflow: hidden;
            font-family: 'Courier New', Courier, monospace;
        }

        /* --- 1. 黑客终端样式 --- */
        #setup-screen {
            display: flex; justify-content: center; align-items: center;
            height: 100vh; background-color: #000; z-index: 9999; position: relative;
        }
        #terminal {
            width: 80%; max-width: 700px; height: 400px;
            color: #0f0; border: 1px solid #0f0; padding: 20px;
            box-shadow: 0 0 20px rgba(0, 255, 0, 0.3);
            font-size: 18px; line-height: 1.5;
        }
        .cursor { animation: blink 0.8s infinite; }
        @keyframes blink { 50% { opacity: 0; } }

        /* --- 2. 崩溃花屏样式 --- */
        #error-container { display: none; width: 100%; height: 100%; position: absolute; top: 0; left: 0; }
        
        @keyframes fullScreenPanic {
            0% { filter: contrast(100%) brightness(100%) hue-rotate(0deg); }
            10% { filter: contrast(500%) brightness(300%) invert(100%); background-color: #100; }
            20% { filter: contrast(100%) brightness(100%) hue-rotate(0deg); background-color: #000; }
            100% { filter: contrast(100%) brightness(100%); }
        }
        body.panic { animation: fullScreenPanic 0.1s infinite; }

        .glitch-block {
            position: absolute; mix-blend-mode: exclusion; z-index: 50; pointer-events: none;
        }
        .error-msg {
            position: absolute; background: #000; border: 2px solid #f00; color: #f00;
            padding: 10px; box-shadow: 0 0 15px #f00; z-index: 100; font-weight: bold;
        }
    </style>
</head>
<body id="main-body">

    <div id="setup-screen">
        <div id="terminal">
            <div id="console-output">
                [ REMOTE_LINK: ESTABLISHED ]<br>
                > SCANNING TARGET PORTS...<br>
                > FOUND OPEN PORT: 8080 (HTTP)<br>
                > INITIATING BUFFER OVERFLOW...<br>
            </div>
            <div id="input-line">
                > EXPLOIT_CMD: <span id="typing-text"></span><span class="cursor">_</span>
            </div>
        </div>
    </div>

    <div id="error-container"></div>

    <script>
        const body = document.getElementById('main-body');
        const setupScreen = document.getElementById('setup-screen');
        const container = document.getElementById('error-container');
        const typingText = document.getElementById('typing-text');
        const consoleOutput = document.getElementById('console-output');

        const exploitCmd = "PAYLOAD_INJECT --PORT 8080 --BYPASS_KERNEL --FORCE_REBOOT";
        let charIndex = 0;

        // 自动开始模拟打字
        window.onload = () => {
            setTimeout(typeWriter, 1000);
        };

        function typeWriter() {
            if (charIndex < exploitCmd.length) {
                typingText.innerHTML += exploitCmd.charAt(charIndex);
                charIndex++;
                setTimeout(typeWriter, 40);
            } else {
                setTimeout(triggerCrash, 1000);
            }
        }

        function triggerCrash() {
            consoleOutput.innerHTML += "<br><span style='color:white'>> EXPLOIT SUCCESSFUL.</span>";
            consoleOutput.innerHTML += "<br><span style='color:#f00'>> KERNEL CORRUPTED.</span>";
            
            setTimeout(() => {
                // 进入全屏并引爆效果
                if (document.documentElement.requestFullscreen) {
                    document.documentElement.requestFullscreen().catch(() => {});
                }
                
                setupScreen.style.display = 'none';
                container.style.display = 'block';
                body.classList.add('panic');

                // 启动声音
                startGlitchSound();

                // 启动视觉生成器
                setInterval(createGlitchBlock, 25);
                setInterval(createErrorWindow, 120);
            }, 800);
        }

        function startGlitchSound() {
            const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.type = 'sawtooth';
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            gain.gain.setValueAtTime(0.05, audioCtx.currentTime); // 5% 音量
            osc.start();
            setInterval(() => {
                osc.frequency.setValueAtTime(300 + Math.random() * 2000, audioCtx.currentTime);
            }, 50);
        }

        function createGlitchBlock() {
            const block = document.createElement('div');
            block.className = 'glitch-block';
            block.style.width = Math.random() * 400 + 'px';
            block.style.height = Math.random() * 20 + 'px';
            block.style.left = Math.random() * window.innerWidth + 'px';
            block.style.top = Math.random() * window.innerHeight + 'px';
            const colors = ['#f0f', '#0ff', '#ff0', '#f00'];
            block.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
            container.appendChild(block);
            setTimeout(() => { container.removeChild(block); }, 40);
        }

        function createErrorWindow() {
            const err = document.createElement('div');
            err.className = 'error-msg';
            err.style.left = Math.random() * (window.innerWidth - 200) + 'px';
            err.style.top = Math.random() * (window.innerHeight - 100) + 'px';
            err.innerHTML = "CRITICAL ERROR 404<br>SYSTEM_HALTED";
            container.appendChild(err);
            if (container.childElementCount > 60) {
                const msgs = container.getElementsByClassName('error-msg');
                if(msgs[0]) container.removeChild(msgs[0]);
            }
        }
    </script>
</body>
</html>
