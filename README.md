<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Tackle Time</title>
<style>
  *{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family: Arial, sans-serif;
  }

  body{
    background:#4A90E2; /* calm blue */
    color:#ffffff;
    min-height:100vh;
    display:flex;
    justify-content:center;
    align-items:center;
    padding:20px;
  }

  .container{
    width:100%;
    max-width:480px;
    background:rgba(255,255,255,0.15);
    backdrop-filter: blur(10px);
    border-radius:20px;
    padding:24px;
  }

  h1{
    text-align:center;
    font-size:30px;
    margin-bottom:16px;
  }

  .mode-title{
    text-align:center;
    font-size:22px;
    margin-bottom:14px;
  }

  .time-display{
    text-align:center;
    font-size:48px;
    font-weight:bold;
    margin:18px 0;
    letter-spacing:1px;
  }

  input, select{
    width:100%;
    padding:12px;
    border:none;
    border-radius:10px;
    font-size:16px;
    margin-bottom:12px;
    color:#134a86;
  }

  .controls{
    display:flex;
    flex-wrap:wrap;
    justify-content:center;
    gap:10px;
    margin-top:6px;
  }

  button{
    border:none;
    border-radius:10px;
    padding:12px 16px;
    cursor:pointer;
    font-size:16px;
    background:#ffffff;
    color:#2c6fb9;
    font-weight:bold;
  }

  button:active{
    transform:scale(0.98);
  }

  .status{
    text-align:center;
    margin-top:12px;
    font-size:16px;
    opacity:0.95;
  }

  .hidden{
    display:none;
  }

  .mode-button{
    position:fixed;
    right:18px;
    bottom:18px;
    width:68px;
    height:68px;
    border-radius:50%;
    border:none;
    background:#ffffff;
    color:#2c6fb9;
    font-size:26px;
    font-weight:bold;
    cursor:pointer;
    box-shadow:0 6px 14px rgba(0,0,0,0.18);
  }

  .world-clock{
    margin-top:24px;
  }

  .world-clock h2{
    text-align:center;
    margin-bottom:12px;
    font-size:20px;
  }

  .clock-box{
    background:rgba(255,255,255,0.12);
    border-radius:12px;
    padding:10px;
    margin-bottom:10px;
  }

  .clock-time{
    margin-top:6px;
    text-align:center;
    font-size:18px;
    letter-spacing:0.5px;
  }

  .row{
    display:flex;
    gap:10px;
  }

  .row > *{
    flex:1;
  }

  .tip{
    text-align:center;
    font-size:13px;
    opacity:0.9;
    margin-top:8px;
  }

</style>
</head>
<body>

<div class="container">
  <h1>Tackle Time</h1>

  <!-- Alarm -->
  <div id="alarmPage">
    <div class="mode-title">Alarm</div>
    <input type="time" id="alarmTime" aria-label="Alarm Time"/>

    <div class="controls">
      <button onclick="setAlarm()">Set Alarm</button>
      <button onclick="clearAlarm()">Clear</button>
      <button onclick="testSound()">Test Sound</button>
      <button onclick="stopSound()">Stop Sound</button>
    </div>

    <div class="status" id="alarmStatus">No alarm set</div>
    <div class="tip">Tip: Keep this tab open so the alarm can ring.</div>
  </div>

  <!-- Timer -->
  <div id="timerPage" class="hidden">
    <div class="mode-title">Timer</div>

    <div class="row">
      <input type="number" id="timerMinutes" min="1" placeholder="Minutes" aria-label="Timer Minutes"/>
      <input type="number" id="timerSecondsInput" min="0" max="59" placeholder="Seconds" aria-label="Timer Seconds"/>
    </div>

    <div class="time-display" id="timerDisplay">00:00</div>

    <div class="controls">
      <button onclick="startTimer()">Start</button>
      <button onclick="pauseTimer()">Pause</button>
      <button onclick="resetTimer()">Reset</button>
    </div>
  </div>

  <!-- Stopwatch -->
  <div id="stopwatchPage" class="hidden">
    <div class="mode-title">Stopwatch</div>

    <div class="time-display" id="stopwatchDisplay">00:00:00</div>

    <div class="controls">
      <button onclick="startStopwatch()">Start</button>
      <button onclick="stopStopwatch()">Stop</button>
      <button onclick="resetStopwatch()">Reset</button>
    </div>
  </div>

  <!-- World Clock -->
  <div class="world-clock">
    <h2>World Clock (up to 3)</h2>

    <div class="clock-box">
      <select id="country1" onchange="saveZones()">
        <option value="">Choose Country 1</option>
        <option value="Asia/Tokyo">Japan</option>
        <option value="Europe/London">United Kingdom</option>
        <option value="America/New_York">United States (New York)</option>
        <option value="Europe/Berlin">Germany</option>
        <option value="Australia/Sydney">Australia (Sydney)</option>
        <option value="Asia/Seoul">Korea</option>
        <option value="Asia/Shanghai">China</option>
        <option value="Asia/Singapore">Singapore</option>
        <option value="Asia/Bangkok">Thailand</option>
        <option value="Asia/Dubai">UAE (Dubai)</option>
        <option value="Europe/Paris">France</option>
        <option value="Europe/Madrid">Spain</option>
        <option value="Europe/Rome">Italy</option>
        <option value="Europe/Moscow">Russia (Moscow)</option>
        <option value="America/Los_Angeles">United States (Los Angeles)</option>
        <option value="America/Chicago">United States (Chicago)</option>
        <option value="America/Sao_Paulo">Brazil (Sao Paulo)</option>
        <option value="Africa/Cairo">Egypt (Cairo)</option>
        <option value="Africa/Johannesburg">South Africa</option>
        <option value="Pacific/Auckland">New Zealand</option>
      </select>
      <div class="clock-time" id="clock1">--:--:--</div>
    </div>

    <div class="clock-box">
      <select id="country2" onchange="saveZones()">
        <option value="">Choose Country 2</option>
        <option value="Asia/Tokyo">Japan</option>
        <option value="Europe/London">United Kingdom</option>
        <option value="America/New_York">United States (New York)</option>
        <option value="Europe/Berlin">Germany</option>
        <option value="Australia/Sydney">Australia (Sydney)</option>
        <option value="Asia/Seoul">Korea</option>
        <option value="Asia/Shanghai">China</option>
        <option value="Asia/Singapore">Singapore</option>
        <option value="Asia/Bangkok">Thailand</option>
        <option value="Asia/Dubai">UAE (Dubai)</option>
        <option value="Europe/Paris">France</option>
        <option value="Europe/Madrid">Spain</option>
        <option value="Europe/Rome">Italy</option>
        <option value="Europe/Moscow">Russia (Moscow)</option>
        <option value="America/Los_Angeles">United States (Los Angeles)</option>
        <option value="America/Chicago">United States (Chicago)</option>
        <option value="America/Sao_Paulo">Brazil (Sao Paulo)</option>
        <option value="Africa/Cairo">Egypt (Cairo)</option>
        <option value="Africa/Johannesburg">South Africa</option>
        <option value="Pacific/Auckland">New Zealand</option>
      </select>
      <div class="clock-time" id="clock2">--:--:--</div>
    </div>

    <div class="clock-box">
      <select id="country3" onchange="saveZones()">
        <option value="">Choose Country 3</option>
        <option value="Asia/Tokyo">Japan</option>
        <option value="Europe/London">United Kingdom</option>
        <option value="America/New_York">United States (New York)</option>
        <option value="Europe/Berlin">Germany</option>
        <option value="Australia/Sydney">Australia (Sydney)</option>
        <option value="Asia/Seoul">Korea</option>
        <option value="Asia/Shanghai">China</option>
        <option value="Asia/Singapore">Singapore</option>
        <option value="Asia/Bangkok">Thailand</option>
        <option value="Asia/Dubai">UAE (Dubai)</option>
        <option value="Europe/Paris">France</option>
        <option value="Europe/Madrid">Spain</option>
        <option value="Europe/Rome">Italy</option>
        <option value="Europe/Moscow">Russia (Moscow)</option>
        <option value="America/Los_Angeles">United States (Los Angeles)</option>
        <option value="America/Chicago">United States (Chicago)</option>
        <option value="America/Sao_Paulo">Brazil (Sao Paulo)</option>
        <option value="Africa/Cairo">Egypt (Cairo)</option>
        <option value="Africa/Johannesburg">South Africa</option>
        <option value="Pacific/Auckland">New Zealand</option>
      </select>
      <div class="clock-time" id="clock3">--:--:--</div>
    </div>
  </div>
</div>

<button class="mode-button" onclick="changeMode()" aria-label="Change Mode">⇄</button>

<!-- Sound file in the same folder: alarm1.wav -->
<audio id="alarmSound" src="alarm1.wav" preload="auto"></audio>

<script>
  // Mode change
  let currentMode = 0;
  const pages = [
    document.getElementById("alarmPage"),
    document.getElementById("timerPage"),
    document.getElementById("stopwatchPage")
  ];

  function changeMode(){
    pages[currentMode].classList.add("hidden");
    currentMode = (currentMode + 1) % pages.length;
    pages[currentMode].classList.remove("hidden");
  }

  // Sound control
  function playLoop(){
    const sound = document.getElementById("alarmSound");
    sound.loop = true;
    sound.currentTime = 0;
    sound.play().catch(() => {
      alert("Please tap a button to allow sound.");
    });
  }

  function stopSound(){
    const sound = document.getElementById("alarmSound");
    sound.pause();
    sound.currentTime = 0;
    sound.loop = false;
  }

  function testSound(){
    const sound = document.getElementById("alarmSound");
    sound.loop = false;
    sound.currentTime = 0;
    sound.play().catch(() => {
      alert("Please tap again to allow sound.");
    });
    setTimeout(() => {
      if(!sound.loop){ stopSound(); }
    }, 1500);
  }

  // Alarm
  let alarmTime = null;
  let alarmTriggered = false;

  function setAlarm(){
    alarmTime = document.getElementById("alarmTime").value;
    alarmTriggered = false;
    if(alarmTime){
      document.getElementById("alarmStatus").textContent = "Alarm set for " + alarmTime;
    }else{
      document.getElementById("alarmStatus").textContent = "Please choose a time.";
    }
  }

  function clearAlarm(){
    alarmTime = null;
    alarmTriggered = false;
    document.getElementById("alarmStatus").textContent = "No alarm set";
    stopSound();
  }

  setInterval(() => {
    if(!alarmTime || alarmTriggered) return;
    const now = new Date();
    const hh = String(now.getHours()).padStart(2,"0");
    const mm = String(now.getMinutes()).padStart(2,"0");
    const current = hh + ":" + mm;
    if(current === alarmTime){
      alarmTriggered = true;
      playLoop();
      alert("It's time!");
    }
  }, 1000);

  // Timer
  let timerTotal = 0;      // total seconds
  let timerLeft = 0;       // seconds left
  let timerInterval = null;

  function startTimer(){
    if(timerInterval) return;

    if(timerLeft === 0){
      const minVal = parseInt(document.getElementById("timerMinutes").value || "0", 10);
      const secVal = parseInt(document.getElementById("timerSecondsInput").value || "0", 10);

      if((!minVal && !secVal) || minVal < 0 || secVal < 0 || secVal > 59){
        alert("Please enter minutes and/or seconds.");
        return;
      }
      timerTotal = (minVal * 60) + secVal;
      timerLeft = timerTotal;
    }

    updateTimerDisplay();

    timerInterval = setInterval(() => {
      timerLeft--;
      updateTimerDisplay();
      if(timerLeft <= 0){
        clearInterval(timerInterval);
        timerInterval = null;
        playLoop();
        alert("Timer finished!");
      }
    }, 1000);
  }

  function pauseTimer(){
    clearInterval(timerInterval);
    timerInterval = null;
  }

  function resetTimer(){
    clearInterval(timerInterval);
    timerInterval = null;
    timerTotal = 0;
    timerLeft = 0;
    document.getElementById("timerDisplay").textContent = "00:00";
    stopSound();
  }

  function updateTimerDisplay(){
    const minutes = Math.floor(timerLeft / 60);
    const seconds = timerLeft % 60;
    document.getElementById("timerDisplay").textContent =
      String(minutes).padStart(2,"0") + ":" + String(seconds).padStart(2,"0");
  }

  // Stopwatch
  let swTicks = 0;          // 1 tick = 10 ms
  let swInterval = null;

  function startStopwatch(){
    if(swInterval) return;
    swInterval = setInterval(() => {
      swTicks++;
      updateStopwatch();
    }, 10);
  }

  function stopStopwatch(){
    clearInterval(swInterval);
    swInterval = null;
  }

  function resetStopwatch(){
    clearInterval(swInterval);
    swInterval = null;
    swTicks = 0;
    updateStopwatch();
  }

  function updateStopwatch(){
    const minutes = Math.floor(swTicks / 6000);
    const seconds = Math.floor((swTicks % 6000) / 100);
    const centi   = swTicks % 100;
    document.getElementById("stopwatchDisplay").textContent =
      String(minutes).padStart(2,"0") + ":" +
      String(seconds).padStart(2,"0") + ":" +
      String(centi).padStart(2,"0");
  }

  // World Clock
  function updateClock(selectId, outputId){
    const zone = document.getElementById(selectId).value;
    if(zone === ""){
      document.getElementById(outputId).textContent = "--:--:--";
      return;
    }
    const time = new Date().toLocaleTimeString("en-GB", { timeZone: zone });
    document.getElementById(outputId).textContent = time;
  }

  function updateWorldClocks(){
    updateClock("country1","clock1");
    updateClock("country2","clock2");
    updateClock("country3","clock3");
  }

  function saveZones(){
    const z1 = document.getElementById("country1").value || "";
    const z2 = document.getElementById("country2").value || "";
    const z3 = document.getElementById("country3").value || "";
    localStorage.setItem("zones", JSON.stringify([z1,z2,z3]));
  }

  function loadZones(){
    const data = localStorage.getItem("zones");
    if(!data) return;
    try{
      const [z1,z2,z3] = JSON.parse(data);
      if(z1) document.getElementById("country1").value = z1;
      if(z2) document.getElementById("country2").value = z2;
      if(z3) document.getElementById("country3").value = z3;
    }catch(e){}
  }

  // Start
  loadZones();
  updateWorldClocks();
  setInterval(updateWorldClocks, 1000);
  updateStopwatch();
</script>

</body>
</html># TackleTime
