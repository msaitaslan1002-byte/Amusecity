<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Karanlık Terminal</title>
<style>
  html, body {margin:0;padding:0;height:100%;width:100%;background:black;color:#0f0;font-family:monospace;overflow:hidden;user-select:none;}
  #overlay{position:fixed;top:0;left:0;width:100vw;height:100vh;background:black;display:flex;justify-content:center;align-items:center;z-index:9999;}
  #closeBtn{font-size:100px;color:red;cursor:pointer;border:5px solid red;border-radius:50%;width:150px;height:150px;line-height:140px;text-align:center;font-weight:900;box-shadow:0 0 40px red;user-select:none;}
  #nameBox{position:fixed;top:45%;left:50%;transform:translate(-50%,-50%);padding:20px 50px;border:4px solid #0f0;color:#0f0;font-size:48px;font-weight:bold;background:black;z-index:900;user-select:none;opacity:1;transition:opacity 0.5s;}
  #nameBox.red{color:red;animation:fastBlink 0.25s infinite;}
  @keyframes fastBlink{0%,50%,100%{opacity:1;}25%,75%{opacity:0;}}
  #terminal{position:fixed;top:10px;left:10px;right:10px;bottom:10px;padding:20px;box-sizing:border-box;background:black;color:#0f0;font-size:20px;line-height:1.5em;white-space:pre-wrap;overflow-y:auto;display:none;user-select:none;font-weight:600;filter:none;}
  #terminal.tv-effect{animation:tvShake 0.15s infinite alternate;}
  @keyframes tvShake{0%{filter:none;transform:translate(0,0);}25%{filter:contrast(1.2) brightness(1.1) blur(0.6px);transform:translate(1px,-1px);}50%{filter:contrast(1.5) brightness(1.3) blur(1.2px);transform:translate(-1px,1px);}75%{filter:contrast(1.1) brightness(1) blur(0.3px);transform:translate(1px,1px);}100%{filter:none;transform:translate(0,0);}}
  #errorScreen{position:fixed;top:0;left:0;width:100vw;height:100vh;background:rgba(255,0,0,0.85);color:black;font-size:56px;font-weight:900;text-align:center;line-height:100vh;user-select:none;display:none;animation:errorFlash 1s infinite;z-index:10000;}
  @keyframes errorFlash{0%,50%,100%{opacity:1;}25%,75%{opacity:0;}}
  #warningMarks{position:fixed;top:0;left:0;width:100vw;height:100vh;pointer-events:none;user-select:none;overflow:hidden;z-index:10001;}
  .mark{position:absolute;color:red;font-weight:900;font-size:24px;animation:markBlink 1.2s infinite;user-select:none;}
  @keyframes markBlink{0%,50%,100%{opacity:1;}25%,75%{opacity:0;}}
</style>
</head>
<body>

<div id="overlay"><div id="closeBtn" aria-label="Başlat">×</div></div>
<div id="nameBox">@MHMTXASLAN</div>
<div id="terminal"></div>
<div id="errorScreen">SİSTEM HATASI</div>
<div id="warningMarks"></div>

<audio id="beep" src="https://actions.google.com/sounds/v1/alarms/beep_short.ogg"></audio>
<audio id="alarm" src="https://actions.google.com/sounds/v1/alarms/alarm_clock.ogg"></audio>

<script>
const closeBtn=document.getElementById('closeBtn');const overlay=document.getElementById('overlay');const nameBox=document.getElementById('nameBox');const terminal=document.getElementById('terminal');const errorScreen=document.getElementById('errorScreen');const warningMarks=document.getElementById('warningMarks');const beepSound=document.getElementById('beep');const alarmSound=document.getElementById('alarm');

const terminalMessages=[
"SİSTEM TARANIYOR...","VERİLER ANALİZ EDİLİYOR...","GÜVENLİK AÇIĞI BULUNDU...","KÖK ERİŞİM SAĞLANDI...","SUNUCU ELE GEÇİRİLDİ...","FIREWALL BAYPAS EDİLİYOR...","VERİLER ŞİFRELİLENİYOR...","İŞLEM TAMAMLANIYOR...","KODLAR DEŞİFRE EDİLİYOR...","AĞ TRAFİĞİ İZLENİYOR...","ŞİFRELER KIRILIYOR...","ROOTKIT YÜKLENİYOR...","SİSTEM LOGLARI SİLİNİYOR...","ARAYÜZ KAPANDI...","KULLANICI HESABI ELE GEÇİRİLDİ...","GİZLİ DOSYALAR AÇILIYOR...","ŞİFRE KORUMALARI BAYPAS EDİLİYOR...","VERİLER ÇALINIYOR...","SUNUCUYA GERİ DÖNÜŞ YOK...","İŞLEM BAŞARILI...","GÜVENLİK DUVARI ÇÖKTÜ...","KOMUT SATIRI ELDE EDİLDİ...","VERİ TABANI ELE GEÇİRİLDİ...","BİLGİLER İNDİRİLİYOR...","HACKER GİRİŞİ ONAYLANDI...","TEHDİT SEVİYESİ: KRİTİK...","KAMERA KAPANDI...","SES KAYDI ALINIYOR...","TÜM PORTLAR AÇIK...","YENİ GİRİŞLER ENGELLENDİ...","SİSTEM ELE GEÇİRİLDİ..."
];

let beepInterval;let beepSpeed=500;let terminalIdx=0;let totalDuration=30000;let linesCount=terminalMessages.length;let intervalSpeed=totalDuration/linesCount;

function createWarningMarks(count=50){warningMarks.innerHTML='';for(let i=0;i<count;i++){const mark=document.createElement('div');mark.classList.add('mark');mark.textContent='!';mark.style.top=Math.random()*100+'vh';mark.style.left=Math.random()*100+'vw';mark.style.animationDelay=(Math.random()*1.2)+'s';mark.style.fontSize=(12+Math.random()*30)+'px';warningMarks.appendChild(mark);}

}

function startBeep(){beepSpeed=500;beepInterval=setInterval(()=>{beepSound.currentTime=0;beepSound.play();beepSpeed-=50;if(beepSpeed<150)beepSpeed=150;clearInterval(beepInterval);beepInterval=setInterval(()=>{beepSound.currentTime=0;beepSound.play();},beepSpeed);},beepSpeed);}
function stopBeep(){clearInterval(beepInterval);}

function startTerminal(){terminal.style.display='block';terminal.classList.add('tv-effect');terminal.innerText='';terminalIdx=0;startBeep();nameBox.style.opacity=0;let interval=setInterval(()=>{if(terminalIdx>=terminalMessages.length){clearInterval(interval);stopBeep();startError();return;}terminal.innerText+=terminalMessages[terminalIdx]+"\n\n";terminal.scrollTop=terminal.scrollHeight;terminalIdx++;},intervalSpeed);}

function startRedBlink(){nameBox.classList.add('red');}
function stopRedBlink(){nameBox.classList.remove('red');}

function startError(){alarmSound.currentTime=0;alarmSound.play();errorScreen.style.display='block';createWarningMarks(60);let lightOn=true;const redFlash=setInterval(()=>{if(lightOn){document.body.style.background='rgba(255,0,0,0.7)';}else{document.body.style.background='black';}lightOn=!lightOn;},500);setTimeout(()=>{clearInterval(redFlash);document.body.style.background='black';errorScreen.style.display='none';warningMarks.innerHTML='';alert("Sistem Hatası Tamamlandı.");},10000);}

closeBtn.onclick=()=>{beepSound.currentTime=0;beepSound.play();overlay.style.display='none';setTimeout(()=>{startRedBlink();},2000);setTimeout(()=>{stopRedBlink();startTerminal();},5000);}
</script>

</body>
</html>
