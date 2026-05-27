<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SISTEM CALINDI // ACCESS DENIED</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            background-color: black;
            overflow: hidden;
            font-family: 'Courier New', Courier, monospace;
        }

        /* Matrix Kod Yağmuru Arka Planı */
        #matrix {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
            opacity: 0.15;
        }

        /* Arayüz Konteyneri */
        .terminal-container {
            position: relative;
            z-index: 2;
            padding: 30px;
            color: #00ff00;
            text-shadow: 0 0 5px #00ff00;
            max-width: 800px;
            margin: 0 auto;
        }

        /* Kırmızı Korkutucu Uyarı Kutusu */
        .warning-box {
            text-align: center;
            border: 3px solid #ff0000;
            padding: 20px;
            background: rgba(255, 0, 0, 0.15);
            color: #ff0000;
            text-shadow: 0 0 10px #ff0000;
            margin-bottom: 40px;
            box-shadow: 0 0 30px rgba(255, 0, 0, 0.6);
            border-radius: 5px;
        }

        .warning-box h1 {
            margin: 0 0 10px 0;
            font-size: 2rem;
            letter-spacing: 2px;
        }

        /* Yanıp Sönen Yazı Efekti */
        .blink {
            animation: blinker 0.8s linear infinite;
            font-weight: bold;
            font-size: 1.3rem;
            letter-spacing: 1px;
        }

        @keyframes blinker {
            50% { opacity: 0; }
        }

        /* Konsol Yazıları */
        .console-text {
            font-size: 18px;
            line-height: 1.8;
            white-space: pre-wrap;
        }

        /* Yazı yazma imleci efekti */
        .cursor {
            display: inline-block;
            width: 10px;
            height: 18px;
            background-color: #00ff00;
            animation: blinker 0.5s step-end infinite;
            margin-left: 5px;
            vertical-align: middle;
        }
    </style>
</head>
<body>

    <canvas id="matrix"></canvas>

    <div class="terminal-container">
        <div class="warning-box">
            <h1>[ TESPİT EDİLDİNİZ ]</h1>
            <p class="blink">SİSTEMİNİZ BİZİM KONTROLÜMÜZDE</p>
        </div>
        
        <div class="console-text">
            <span id="console"></span><span class="cursor"></span>
        </div>
    </div>

    <script>
        // --- MATRIX YAĞMURU EFEKTİ ---
        const canvas = document.getElementById('matrix');
        const ctx = canvas.getContext('2d');

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        resizeCanvas();
        window.addEventListener('resize', resizeCanvas);

        const letters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789@#$%&*§Я🔥💀';
        const fontSize = 16;
        let columns = canvas.width / fontSize;
        let drops = Array(Math.floor(columns)).fill(1);

        function drawMatrix() {
            ctx.fillStyle = 'rgba(0, 0, 0, 0.05)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = '#00ff00';
            ctx.font = fontSize + 'px monospace';

            for (let i = 0; i < drops.length; i++) {
                const text = letters[Math.floor(Math.random() * letters.length)];
                ctx.fillText(text, i * fontSize, drops[i] * fontSize);
                if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
                    drops[i] = 0;
                }
                drops[i]++;
            }
        }
        setInterval(drawMatrix, 33);

        // --- HACKER YAZI EFEKTİ ---
        const consoleElement = document.getElementById('console');
        const lines = [
            "> Kurban IP Adresi: " + (Math.floor(Math.random() * 160) + 40) + "." + Math.floor(Math.random() * 255) + "." + Math.floor(Math.random() * 255) + "." + Math.floor(Math.random() * 255),
            "> Port 80, 443 ve 8080 sızma noktaları açılıyor...",
            "> Güvenlik duvarı (Firewall) devre dışı bırakıldı... %100",
            "> ROOT yetkisi alındı. C:/Windows/System32 dizinine bağlanıldı.",
            "> Kayıtlı tarayıcı şifreleri ve çerezler (cookies) okunuyor...",
            "> Kişisel dosyalar ve fotoğraflar uzak sunucuya yedekleniyor...",
            "> [DURUM]: VERİ TRANSFERİ BAŞARIYLA TAMAMLANDI.",
            "> NOT: Bu tarayıcıyı kapatsanız bile arka kapı (Backdoor) aktif kalacaktır.",
            "> Geçmiş olsun. Biz her yerdeyiz."
        ];

        let lineIndex = 0;
        
        function typeLines() {
            if (lineIndex < lines.length) {
                consoleElement.innerHTML += lines[lineIndex] + "<br><br>";
                lineIndex++;
                setTimeout(typeLines, 1400); // Satırlar arası 1.4 saniye bekler
            }
        }
        
        // İlk satırı 1 saniye sonra başlat
        setTimeout(typeLines, 1000);
    </script>
</body>
</html>
