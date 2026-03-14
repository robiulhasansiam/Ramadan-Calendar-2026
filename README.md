<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="color-scheme" content="dark">
    <title>Mission 0.1%: Jamalpur Ramadan Tracker</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&family=Orbitron:wght@700&family=Amiri:wght@700&display=swap');

        :root {
            --bg-color: #02040a;
            --card-bg: rgba(18, 20, 33, 0.8);
            --primary: #ffd700;
            --accent: #23d5ab;
            --text: #ffffff;
            --shadow: rgba(255, 215, 0, 0.3);
        }

        /* Themes */
        .theme-blue { --primary: #00d2ff; --shadow: rgba(0, 210, 255, 0.4); }
        .theme-red { --primary: #ff4d6d; --shadow: rgba(255, 77, 109, 0.4); }
        .theme-purple { --primary: #bc13fe; --shadow: rgba(188, 19, 254, 0.4); }
        .theme-green { --primary: #39ff14; --shadow: rgba(57, 255, 20, 0.4); }
        .theme-orange { --primary: #ff9100; --shadow: rgba(255, 145, 0, 0.4); }
        .theme-pink { --primary: #ff00ff; --shadow: rgba(255, 0, 255, 0.4); }
        .theme-cyan { --primary: #00ffff; --shadow: rgba(0, 255, 255, 0.4); }
        .theme-emerald { --primary: #50c878; --shadow: rgba(80, 200, 120, 0.4); }
        .theme-gold { --primary: #ffd700; --shadow: rgba(255, 215, 0, 0.4); }
        .theme-crimson { --primary: #dc143c; --shadow: rgba(220, 20, 60, 0.4); }

        * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        
        body {
            font-family: 'Poppins', sans-serif;
            background-color: var(--bg-color) !important;
            color: var(--text) !important;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            overflow-x: hidden;
            transition: all 0.4s ease;
        }

        .container { width: 100%; max-width: 500px; text-align: center; padding: 15px; flex-grow: 1; }
        
        .theme-btn {
            position: fixed; top: 15px; right: 15px; z-index: 100;
            background: var(--card-bg); border: 1px solid var(--primary);
            width: 40px; height: 40px; border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            cursor: pointer; box-shadow: 0 0 10px var(--shadow);
        }

        h1.main-title { font-family: 'Amiri', serif; font-size: 2.2rem; color: var(--primary); text-shadow: 0 0 10px var(--shadow); }

        .day-header {
            background: var(--card-bg); border-radius: 15px; padding: 12px; margin-bottom: 15px;
            border: 1px solid var(--primary); box-shadow: 0 0 15px var(--shadow);
        }

        .clock { 
            width: 140px; height: 140px; border: 3px solid var(--primary); border-radius: 50%; 
            margin: 10px auto; position: relative; background: #000; box-shadow: 0 0 15px var(--shadow);
        }
        .hand { position: absolute; bottom: 50%; left: 50%; transform-origin: bottom; transform: translateX(-50%); border-radius: 5px; }
        .h-sec { background: #ff0000; width: 2px; height: 55px; z-index: 5; }
        .h-min { background: #fff; width: 3px; height: 45px; }
        .h-hour { background: var(--primary); width: 5px; height: 35px; }
        .clock-center { position: absolute; top: 50%; left: 50%; width: 10px; height: 10px; background: var(--primary); border-radius: 50%; transform: translate(-50%, -50%); z-index: 10; }
        .num { position: absolute; width: 100%; height: 100%; text-align: center; color: var(--primary); font-weight: 700; font-size: 0.8rem; padding: 2px; }
        
        .n12 { transform: rotate(0deg); } .n1 { transform: rotate(30deg); } .n2 { transform: rotate(60deg); }
        .n3 { transform: rotate(90deg); } .n4 { transform: rotate(120deg); } .n5 { transform: rotate(150deg); }
        .n6 { transform: rotate(180deg); } .n7 { transform: rotate(210deg); } .n8 { transform: rotate(240deg); }
        .n9 { transform: rotate(270deg); } .n10 { transform: rotate(300deg); } .n11 { transform: rotate(330deg); }
        .num span { display: inline-block; }
        .n1 span { transform: rotate(-30deg); } .n2 span { transform: rotate(-60deg); } .n3 span { transform: rotate(-90deg); }
        .n4 span { transform: rotate(-120deg); } .n5 span { transform: rotate(-150deg); } .n6 span { transform: rotate(-180deg); }
        .n7 span { transform: rotate(-210deg); } .n8 span { transform: rotate(-240deg); } .n9 span { transform: rotate(-270deg); }
        .n10 span { transform: rotate(-300deg); } .n11 span { transform: rotate(-330deg); }

        .duration-container {
            position: relative; width: 100%; height: 40px; background: rgba(255,255,255,0.05);
            border-radius: 10px; margin-bottom: 15px; border: 1px solid rgba(255,255,255,0.1);
            overflow: hidden; display: flex; align-items: center; justify-content: center;
        }
        #progress-fill {
            position: absolute; left: 0; top: 0; height: 100%; background: var(--primary);
            width: 0%; transition: width 1s linear; opacity: 0.6; z-index: 1;
        }
        .duration-text {
            position: relative; z-index: 2; font-weight: 700; font-size: 0.85rem; 
            display: flex; gap: 10px; width: 100%; justify-content: center;
        }
        #percent-display { color: var(--accent); text-shadow: 0 0 5px #000; }

        .track-row { display: flex; gap: 10px; margin-bottom: 15px; }
        .track-box { flex: 1; background: var(--card-bg); padding: 12px 5px; border-radius: 12px; border: 1px solid rgba(255,255,255,0.1); }
        .urgent { border: 1.5px solid #ff4d6d !important; animation: pulse 1.5s infinite; }
        @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(255,77,109,0.5); } 70% { box-shadow: 0 0 0 10px rgba(255,77,109,0); } 100% { box-shadow: 0 0 0 0 rgba(255,77,109,0); } }

        .time-label { font-size: 0.7rem; color: var(--primary); font-weight: 700; }
        .time-val { font-family: 'Orbitron', sans-serif; font-size: 1.1rem; margin: 3px 0; }
        .live-timer { font-size: 0.75rem; color: var(--accent); }

        .cal-tabs { display: flex; gap: 5px; margin-bottom: 10px; }
        .tab-btn { flex: 1; padding: 8px; font-size: 0.7rem; background: var(--card-bg); border: 1px solid var(--primary); color: var(--text); border-radius: 5px; cursor: pointer; font-weight: 600; transition: 0.3s; }
        .tab-btn.active { background: var(--primary); color: #000; box-shadow: 0 0 10px var(--shadow); }
        .cal-content { display: none; background: var(--card-bg); border-radius: 10px; border: 1px solid rgba(255,255,255,0.1); }
        .cal-content.active { display: block; }
        
        table { width: 100%; border-collapse: collapse; font-size: 0.7rem; }
        th, td { padding: 6px 2px; border-bottom: 1px solid rgba(255,255,255,0.05); text-align: center; }
        .active-row { background: rgba(255, 255, 255, 0.1) !important; color: var(--primary); font-weight: bold; }

        .footer { margin-top: auto; width: 100%; padding: 20px 10px; background: rgba(0,0,0,0.5); border-top: 1px solid rgba(255,255,255,0.1); font-size: 0.8rem; }
        .insta-btn { 
            display: inline-flex; align-items: center; gap: 8px; margin-top: 10px; 
            padding: 8px 15px; background: linear-gradient(45deg, #f09433, #e6683c, #dc2743, #cc2366, #bc1888);
            border-radius: 20px; color: white; text-decoration: none; font-weight: 600;
            box-shadow: 0 4px 15px rgba(0,0,0,0.3); transition: 0.3s;
        }

        @media (max-height: 700px) {
            h1.main-title { font-size: 1.8rem; }
            .clock { width: 110px; height: 110px; }
            .track-box { padding: 8px 2px; }
            table { font-size: 0.65rem; }
        }
    </style>
</head>
<body onclick="initAudio()">

    <div class="theme-btn" onclick="nextTheme()">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="var(--primary)"><path d="M12 3c-4.97 0-9 4.03-9 9s4.03 9 9 9c.83 0 1.5-.67 1.5-1.5 0-.39-.15-.74-.39-1.01-.23-.26-.38-.61-.38-.99 0-.83.67-1.5 1.5-1.5H16c2.76 0 5-2.24 5-5 0-4.42-4.03-8-9-8zm-5.5 9c-.83 0-1.5-.67-1.5-1.5S5.67 9 6.5 9s1.5.67 1.5 1.5S7.33 12 6.5 12zm3-4C8.67 8 8 7.33 8 6.5S8.67 5 9.5 5s1.5.67 1.5 1.5S10.33 8 9.5 8zm5 0c-.83 0-1.5-.67-1.5-1.5S13.67 5 14.5 5s1.5.67 1.5 1.5S15.33 8 14.5 8zm3 4c-.83 0-1.5-.67-1.5-1.5S16.67 12 17.5 12s1.5.67 1.5 1.5-.67 1.5-1.5 1.5z"/></svg>
    </div>

    <div class="container">
        <h1 class="main-title">Ramadan Kareem</h1>
        
        <div class="day-header">
            <h2 id="display-day">DAY --</h2>
            <div id="display-date" style="font-weight: 600; color: var(--accent); font-size: 0.8rem;">Loading...</div>
        </div>

        <div class="clock" id="analog-clock">
            <div class="clock-center"></div>
            <div id="hour" class="hand h-hour"></div><div id="minute" class="hand h-min"></div><div id="second" class="hand h-sec"></div>
        </div>

        <div class="duration-container">
            <div id="progress-fill"></div>
            <div class="duration-text">
                <span id="fast-duration" style="opacity: 0.7;">Fasting: --h --m</span>
                <span id="percent-display">0.00%</span>
            </div>
        </div>

        <div class="track-row">
            <div class="track-box" id="sehri-box">
                <span class="time-label" id="sehri-label">SEHRI ENDS</span>
                <div class="time-val" id="sehri-time">--:-- AM</div>
                <div class="live-timer" id="sehri-live">--:--:--</div>
            </div>
            <div class="track-box" id="iftar-box">
                <span class="time-label">IFTAR STARTS</span>
                <div class="time-val" id="iftar-time">--:-- PM</div>
                <div class="live-timer" id="iftar-live">--:--:--</div>
            </div>
        </div>

        <div id="verse-card" style="font-size: 0.8rem; font-style: italic; margin-bottom: 15px; min-height: 45px; color: #ccc;">
            "Take suhoor, for in suhoor there is blessing."
        </div>

        <div class="cal-tabs">
            <button class="tab-btn active" onclick="showTab(1)">RAHMAH</button>
            <button class="tab-btn" onclick="showTab(2)">MAGHFIRAH</button>
            <button class="tab-btn" onclick="showTab(3)">NAJAT</button>
        </div>

        <div id="tab-1" class="cal-content active"><table><thead><tr><th>D</th><th>Date</th><th>Sehri</th><th>Iftar</th></tr></thead><tbody id="cal-1"></tbody></table></div>
        <div id="tab-2" class="cal-content"><table><thead><tr><th>D</th><th>Date</th><th>Sehri</th><th>Iftar</th></tr></thead><tbody id="cal-2"></tbody></table></div>
        <div id="tab-3" class="cal-content"><table><thead><tr><th>D</th><th>Date</th><th>Sehri</th><th>Iftar</th></tr></thead><tbody id="cal-3"></tbody></table></div>

        <div class="footer">
            <div>Developed by <strong>Robiul Hasan Siam</strong></div>
            <a href="https://www.instagram.com/robiul_hasan_siam?igsh=MWZ3ZWx5cTVmdzFhMA==" class="insta-btn" target="_blank">
                Connect with me
            </a>
            <div style="font-size: 0.6rem; margin-top: 10px; opacity: 0.5;">2026 Ramadan Tracker | Jamalpur</div>
        </div>
    </div>

    <script>
        let audioCtx; 
        const themes = ['theme-gold', 'theme-blue', 'theme-red', 'theme-purple', 'theme-green', 'theme-orange', 'theme-pink', 'theme-cyan', 'theme-emerald', 'theme-crimson'];
        let currentThemeIdx = 0;

        const sehriList = ["05:16", "05:15", "05:14", "05:13", "05:13", "05:12", "05:11", "05:10", "05:09", "05:09", "05:08", "05:07", "05:06", "05:06", "05:05", "05:03", "05:02", "05:01", "05:00", "04:59", "04:58", "04:57", "04:56", "04:55", "04:54", "04:53", "04:52", "04:51", "04:50", "04:49"];
        const iftarList = ["17:57", "17:58", "17:58", "17:59", "17:59", "18:00", "18:00", "18:01", "18:02", "18:02", "18:03", "18:03", "18:04", "18:04", "18:05", "18:05", "18:06", "18:06", "18:07", "18:07", "18:08", "18:08", "18:09", "18:09", "18:09", "18:10", "18:10", "18:11", "18:11", "18:12"];

        const hadiths = [
            "When Ramadan begins, the gates of Paradise are opened.",
            "Fasting is a shield from sins and Hellfire.",
            "Take suhoor, for in suhoor there is blessing.",
            "The smell from the mouth of a fasting person is beloved to Allah.",
            "Seek Laylat al-Qadr in the last ten nights of Ramadan."
        ];

        function nextTheme() {
            document.body.classList.remove(themes[currentThemeIdx]);
            currentThemeIdx = (currentThemeIdx + 1) % themes.length;
            document.body.classList.add(themes[currentThemeIdx]);
        }

        function showTab(n) {
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
            document.querySelectorAll('.cal-content').forEach(c => c.classList.remove('active'));
            event.target.classList.add('active');
            document.getElementById('tab-'+n).classList.add('active');
        }

        function initClock() {
            const clock = document.getElementById('analog-clock');
            for(let i=1; i<=12; i++) {
                const div = document.createElement('div');
                div.className = `num n${i}`;
                div.innerHTML = `<span>${i}</span>`;
                clock.appendChild(div);
            }
        }

        function update() {
            const now = new Date();
            const h = now.getHours(), m = now.getMinutes(), s = now.getSeconds();
            
            // Analog Clock Hands
            document.getElementById('second').style.transform = `translateX(-50%) rotate(${s * 6}deg)`;
            document.getElementById('minute').style.transform = `translateX(-50%) rotate(${m * 6 + s * 0.1}deg)`;
            document.getElementById('hour').style.transform = `translateX(-50%) rotate(${(h % 12) * 30 + m * 0.5}deg)`;

            // Fix: Calculate Day relative to the specific Ramadan start date (Feb 19, 2026)
            const startRamadan = new Date(2026, 1, 19, 0, 0, 0); // Month is 0-indexed, 1 = Feb
            const timeDiff = now.getTime() - startRamadan.getTime();
            const diffDays = Math.floor(timeDiff / (1000 * 60 * 60 * 24));
            
            if(diffDays >= 0 && diffDays < 30) {
                const currentDayIdx = diffDays;
                const nextDayIdx = diffDays + 1;

                const currentSehri = sehriList[currentDayIdx];
                const currentIftar = iftarList[currentDayIdx];
                
                // Format today's date for Date object parsing
                const todayStr = now.getFullYear() + '-' + String(now.getMonth() + 1).padStart(2, '0') + '-' + String(now.getDate()).padStart(2, '0');
                const tomorrow = new Date(now);
                tomorrow.setDate(now.getDate() + 1);
                const tomorrowStr = tomorrow.getFullYear() + '-' + String(tomorrow.getMonth() + 1).padStart(2, '0') + '-' + String(tomorrow.getDate()).padStart(2, '0');

                document.getElementById('display-day').innerText = "RAMADAN DAY " + (currentDayIdx + 1 < 10 ? '0'+(currentDayIdx+1) : (currentDayIdx+1));
                document.getElementById('display-date').innerText = now.toLocaleDateString('en-GB', {weekday:'long', day:'numeric', month:'short'});

                // Logic: 9 PM switch to Next Day's Sehri display
                if (h >= 21) {
                    if(nextDayIdx < 30) {
                        document.getElementById('sehri-label').innerText = `NEXT SEHRI (DAY ${nextDayIdx + 1})`;
                        document.getElementById('sehri-time').innerText = format12(sehriList[nextDayIdx]);
                        countDown(tomorrowStr, sehriList[nextDayIdx], "sehri-live", "sehri-box", "Sehri Ended");
                    }
                } else {
                    document.getElementById('sehri-label').innerText = "SEHRI ENDS";
                    document.getElementById('sehri-time').innerText = format12(currentSehri);
                    countDown(todayStr, currentSehri, "sehri-live", "sehri-box", "Sehri Ended");
                }

                // Iftar countdown for the current date
                document.getElementById('iftar-time').innerText = format12(currentIftar);
                countDown(todayStr, currentIftar, "iftar-live", "iftar-box", "Iftar Started");

                // Progress Bar calculation: Start from Sehri End and fill until Iftar Start
                const sTime = new Date(`${todayStr}T${currentSehri}`).getTime();
                const iTime = new Date(`${todayStr}T${currentIftar}`).getTime();
                const totalFastingMs = iTime - sTime;
                
                let pct = 0;
                if (now.getTime() > sTime && now.getTime() < iTime) {
                    pct = ((now.getTime() - sTime) / totalFastingMs) * 100;
                } else if (now.getTime() >= iTime) {
                    pct = 100;
                }

                document.getElementById('progress-fill').style.width = pct.toFixed(2) + "%";
                document.getElementById('percent-display').innerText = pct.toFixed(2) + "%";
                
                let totalMin = totalFastingMs / 60000;
                document.getElementById('fast-duration').innerText = `Fasting: ${Math.floor(totalMin/60)}h ${Math.floor(totalMin%60)}m`;
            }
        }

        function countDown(date, time, id, boxId, endMsg) {
            const target = new Date(`${date}T${time}`).getTime();
            const diff = target - new Date().getTime();
            const el = document.getElementById(id);
            const box = document.getElementById(boxId);

            if(diff > 0) {
                const hh = Math.floor(diff/3600000), mm = Math.floor((diff%3600000)/60000), ss = Math.floor((diff%60000)/1000);
                el.innerText = `${hh.toString().padStart(2,'0')}:${mm.toString().padStart(2,'0')}:${ss.toString().padStart(2,'0')}`;
                if(diff < 1200000) box.classList.add('urgent'); else box.classList.remove('urgent');
            } else {
                el.innerText = endMsg;
                box.classList.remove('urgent');
            }
        }

        function format12(t) {
            if(!t) return "--:--";
            let [h, m] = t.split(':');
            h = parseInt(h);
            const suffix = h >= 12 ? 'PM' : 'AM';
            h = h % 12 || 12;
            return `${h}:${m} ${suffix}`;
        }

        function buildCalendar() {
            for(let i=0; i<30; i++) {
                const d = new Date(2026, 1, 19 + i);
                const tid = i < 10 ? 'cal-1' : (i < 20 ? 'cal-2' : 'cal-3');
                const row = document.createElement('tr');
                if(d.toDateString() === new Date().toDateString()) row.className = 'active-row';
                row.innerHTML = `<td>${i+1}</td><td>${d.getDate()} ${d.toLocaleDateString('en-US',{month:'short'})}</td><td>${sehriList[i]}</td><td>${format12(iftarList[i])}</td>`;
                document.getElementById(tid).appendChild(row);
            }
        }

        function initAudio() { if(!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)(); }
        
        setInterval(update, 1000);
        setInterval(() => {
            document.getElementById('verse-card').innerText = `"${hadiths[Math.floor(Math.random()*hadiths.length)]}"`;
        }, 15000);

        window.onload = () => { initClock(); buildCalendar(); update(); };
    </script>
</body>
</html>
