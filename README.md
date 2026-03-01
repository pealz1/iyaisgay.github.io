<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>goodnight lol</title>
<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@700;900&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'Nunito', sans-serif;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
    transition: background 2s ease;
  }

  /* ── WEATHER BADGE ── */
  #weather-badge {
    position: fixed;
    top: 18px; right: 18px;
    background: rgba(255,255,255,0.06);
    border: 1px solid rgba(255,182,213,0.2);
    border-radius: 14px;
    padding: 8px 16px;
    font-size: .75rem;
    color: rgba(255,255,255,0.55);
    backdrop-filter: blur(8px);
    z-index: 100;
    letter-spacing: 1px;
    font-family: 'Inter', sans-serif;
  }

  /* ── SKY BACKGROUNDS ── */
  body.night-clear  { background: #080812; }
  body.night-cloudy { background: #0b0b18; }
  body.night-rain   { background: #060710; }
  body.day-clear    { background: linear-gradient(to bottom, #87CEEB 0%, #c9e8f5 60%, #e8f4f8 100%); }
  body.day-cloudy   { background: linear-gradient(to bottom, #6a7fa8 0%, #8fa3c4 50%, #b8c9dd 100%); }
  body.day-rain     { background: linear-gradient(to bottom, #3a4a5c 0%, #4e6070 50%, #6b7f8f 100%); }

  /* ── AURORA / MESH GRADIENT ── */
  #aurora {
    position: fixed; inset: 0; z-index: 0; pointer-events: none;
    overflow: hidden;
  }
  .aurora-blob {
    position: absolute;
    border-radius: 50%;
    filter: blur(80px);
    opacity: 0;
    animation: auroraMove var(--at, 12s) var(--ad, 0s) ease-in-out infinite alternate;
  }
  body.night-clear .aurora-blob,
  body.night-cloudy .aurora-blob,
  body.night-rain .aurora-blob { opacity: 1; }

  @keyframes auroraMove {
    0%   { transform: translate(0, 0) scale(1); }
    50%  { transform: translate(var(--mx, 40px), var(--my, -30px)) scale(1.15); }
    100% { transform: translate(var(--mx2, -30px), var(--my2, 20px)) scale(0.9); }
  }

  /* ── STARS ── */
  #star-layer { position: fixed; inset: 0; pointer-events: none; z-index: 1; }
  .star {
    position: absolute; border-radius: 50%; background: white;
    animation: twinkle var(--td, 3s) var(--dd, 0s) ease-in-out infinite alternate;
  }
  @keyframes twinkle {
    from { opacity: 0.15; transform: scale(1); }
    to   { opacity: 0.9;  transform: scale(1.4); }
  }

  /* ── CLOUDS ── */
  #cloud-layer { position: fixed; inset: 0; pointer-events: none; z-index: 2; overflow: hidden; }
  .cloud {
    position: absolute; border-radius: 50px;
    animation: driftCloud var(--cd, 40s) var(--cde, 0s) linear infinite;
  }
  body.night-clear .cloud,
  body.night-cloudy .cloud,
  body.night-rain .cloud { background: #161830; }
  body.night-cloudy .cloud { background: #1a1e38; }
  body.night-rain .cloud  { background: #0e1020; }
  body.day-clear .cloud, body.day-cloudy .cloud { background: rgba(255,255,255,0.75); }
  body.day-rain .cloud { background: rgba(130,145,165,0.75); }
  @keyframes driftCloud {
    from { transform: translateX(-450px); }
    to   { transform: translateX(110vw); }
  }

  /* ── RAIN ── */
  #rain-layer {
    position: fixed; inset: 0; pointer-events: none; z-index: 3; overflow: hidden;
    display: none;
  }
  body.night-rain #rain-layer, body.day-rain #rain-layer { display: block; }
  .drop {
    position: absolute; width: 1.5px; border-radius: 2px;
    animation: fall var(--rd, .8s) var(--rde, 0s) linear infinite;
  }
  body.night-rain .drop { background: rgba(160,185,255,0.5); }
  body.day-rain   .drop { background: rgba(100,140,200,0.45); }
  @keyframes fall {
    0%   { top: -10%; transform: skewX(-10deg); opacity: 0; }
    10%  { opacity: 0.45; }
    100% { top: 110%; transform: skewX(-10deg) translateX(-30px); opacity: 0.1; }
  }

  /* ── LIGHTNING ── */
  #lightning {
    position: fixed; inset: 0; pointer-events: none; z-index: 4;
    background: rgba(200,210,255,0); display: none;
  }
  body.night-rain #lightning { display: block; animation: lightning 8s ease-in-out infinite; }
  @keyframes lightning {
    0%,91%,93%,95%,100% { background: rgba(200,210,255,0); }
    92%,94%              { background: rgba(200,210,255,0.06); }
  }

  /* ── SUN ── */
  #sun {
    position: fixed; top: 60px; right: 100px;
    width: 80px; height: 80px; border-radius: 50%;
    background: radial-gradient(circle, #ffe566, #ffb830);
    box-shadow: 0 0 35px 15px rgba(255,200,50,.4), 0 0 70px 35px rgba(255,180,0,.15);
    display: none; z-index: 2;
    animation: sunPulse 4s ease-in-out infinite;
  }
  body.day-clear #sun { display: block; }
  @keyframes sunPulse {
    0%,100% { box-shadow: 0 0 35px 15px rgba(255,200,50,.4), 0 0 70px 35px rgba(255,180,0,.15); }
    50%     { box-shadow: 0 0 50px 22px rgba(255,200,50,.55), 0 0 95px 48px rgba(255,180,0,.25); }
  }

  /* ── CARD ── */
  .center {
    position: relative; display: flex; flex-direction: column;
    align-items: center; gap: 10px; z-index: 10;
  }

  .moon-emoji {
    font-size: 3.5rem;
    animation: moonWobble 3s ease-in-out infinite;
    filter: drop-shadow(0 0 10px rgba(255,158,205,0.5));
  }
  @keyframes moonWobble {
    0%, 100% { transform: rotate(-8deg) scale(1); }
    50%       { transform: rotate(8deg) scale(1.06); }
  }

  .title {
    font-size: clamp(3rem, 12vw, 6rem);
    font-weight: 900;
    color: #fff;
    letter-spacing: -1px;
    animation: pulse 3s ease-in-out infinite;
  }
  @keyframes pulse {
    0%, 100% { text-shadow: 0 0 12px rgba(255,158,205,0.6), 0 0 30px rgba(224,82,154,0.25); }
    50%       { text-shadow: 0 0 18px rgba(255,184,217,0.7), 0 0 45px rgba(255,158,205,0.3); }
  }

  .subtitle {
    font-size: 1rem; color: rgba(255,158,205,0.7);
    letter-spacing: 2px; font-weight: 700;
  }

  .emoji-float {
    position: absolute; font-size: 2rem;
    animation: floatEmoji var(--et, 4s) var(--ed, 0s) ease-in-out infinite alternate;
    opacity: 0.6;
  }
  @keyframes floatEmoji {
    from { transform: translateY(0) rotate(0deg); }
    to   { transform: translateY(-16px) rotate(8deg); }
  }

  .card {
    position: relative; padding: 2.5rem 4rem;
    border-radius: 24px;
    background: rgba(255,255,255,0.03);
    backdrop-filter: blur(10px);
    outline: 1px solid rgba(255,158,205,0.18);
    display: flex; flex-direction: column;
    align-items: center; gap: 8px;
    box-shadow: 0 0 40px rgba(224,82,154,0.08), inset 0 1px 0 rgba(255,255,255,0.06);
    animation: cardPulse 4s ease-in-out infinite;
  }
  @keyframes cardPulse {
    0%, 100% { outline-color: rgba(255,158,205,0.15); box-shadow: 0 0 40px rgba(224,82,154,0.08), inset 0 1px 0 rgba(255,255,255,0.06); }
    50%       { outline-color: rgba(255,158,205,0.35); box-shadow: 0 0 60px rgba(224,82,154,0.15), inset 0 1px 0 rgba(255,255,255,0.06); }
  }

  /* rain vibe */
  #rain-vibe {
    position: fixed; bottom: 90px; left: 50%;
    transform: translateX(-50%);
    color: rgba(255,255,255,0.3); font-size: .7rem;
    letter-spacing: 2px; display: none; z-index: 100;
    font-family: 'Inter', sans-serif;
  }
  body.night-rain #rain-vibe, body.day-rain #rain-vibe { display: block; }

  /* ─────────────────────────────────────────
     MUSIC PLAYER
  ───────────────────────────────────────── */
  #player {
    position: fixed;
    bottom: 20px; left: 20px;
    width: 260px;
    background: rgba(12, 10, 20, 0.75);
    backdrop-filter: blur(20px);
    border: 1px solid rgba(255,158,205,0.15);
    border-radius: 18px;
    padding: 14px 16px;
    z-index: 200;
    box-shadow: 0 8px 32px rgba(0,0,0,0.4);
    font-family: 'Inter', sans-serif;
    transition: border-color .3s;
  }
  #player:hover { border-color: rgba(255,158,205,0.3); }

  .player-top {
    display: flex; align-items: center; gap: 12px; margin-bottom: 12px;
  }
  .album-art {
    width: 44px; height: 44px; border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 1.4rem; flex-shrink: 0;
    background: rgba(255,255,255,0.07);
    position: relative; overflow: hidden;
    animation: spinArt 8s linear infinite;
    animation-play-state: paused;
  }
  .album-art.playing { animation-play-state: running; }
  @keyframes spinArt {
    from { transform: rotate(0deg); }
    to   { transform: rotate(360deg); }
  }

  .track-info { flex: 1; min-width: 0; }
  .track-name {
    font-size: .78rem; font-weight: 500;
    color: rgba(255,255,255,0.9);
    white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
    letter-spacing: .2px;
  }
  .track-artist {
    font-size: .68rem; color: rgba(255,158,205,0.65);
    margin-top: 2px; letter-spacing: .3px;
  }

  /* progress bar */
  .progress-wrap {
    width: 100%; height: 3px; background: rgba(255,255,255,0.1);
    border-radius: 4px; margin-bottom: 10px; cursor: pointer;
    position: relative; overflow: hidden;
  }
  .progress-fill {
    height: 100%; background: linear-gradient(90deg, #ff9ecd, #e0529a);
    border-radius: 4px; width: 0%;
    transition: width .5s linear;
  }

  /* controls */
  .controls {
    display: flex; align-items: center; justify-content: center; gap: 18px;
  }
  .ctrl-btn {
    background: none; border: none; cursor: pointer;
    color: rgba(255,255,255,0.5); font-size: .85rem;
    padding: 4px; transition: color .2s, transform .15s;
    display: flex; align-items: center;
  }
  .ctrl-btn:hover { color: rgba(255,255,255,0.9); transform: scale(1.1); }
  .ctrl-btn.play-btn {
    width: 34px; height: 34px; border-radius: 50%;
    background: linear-gradient(135deg, #ff9ecd, #e0529a);
    color: white; font-size: 1rem;
    box-shadow: 0 0 14px rgba(224,82,154,0.3);
    justify-content: center;
  }
  .ctrl-btn.play-btn:hover { transform: scale(1.12); box-shadow: 0 0 20px rgba(224,82,154,0.5); }

  /* soundwave bars (playing indicator) */
  .soundwave {
    display: flex; align-items: flex-end; gap: 2px; height: 14px;
    margin-left: auto;
  }
  .bar {
    width: 3px; border-radius: 2px;
    background: rgba(255,158,205,0.7);
    animation: bounce var(--bs, .8s) var(--bd, 0s) ease-in-out infinite alternate;
    animation-play-state: paused;
  }
  .bar.active { animation-play-state: running; }
  @keyframes bounce {
    from { height: 3px; }
    to   { height: 14px; }
  }

  /* queue label */
  .queue-label {
    font-size: .6rem; color: rgba(255,255,255,0.2);
    letter-spacing: 1.5px; text-align: center;
    margin-top: 8px; text-transform: uppercase;
  }
</style>
</head>
<body>

<div id="weather-badge">📍 Fresno, CA &nbsp;·&nbsp; <span id="temp">--</span> &nbsp;·&nbsp; <span id="cond-text">--</span></div>
<div id="rain-vibe">🌧️ &nbsp; raining out there... stay cozy</div>

<!-- layers -->
<div id="aurora"></div>
<div id="star-layer"></div>
<div id="cloud-layer"></div>
<div id="rain-layer"></div>
<div id="lightning"></div>
<div id="sun"></div>

<!-- main -->
<div class="center">

  <div class="card">
    <div class="moon-emoji" id="moonEmoji">🌙</div>
    <div class="title">goodnight!!</div>
    <div class="subtitle">i was bored so i made this for you :D</div>
  </div>
</div>

<!-- music player -->
<div id="player">
  <div class="player-top">
    <div class="album-art" id="albumArt">🎵</div>
    <div class="track-info">
      <div class="track-name" id="trackName">From The Start</div>
      <div class="track-artist" id="trackArtist">Laufey</div>
    </div>
    <div class="soundwave" id="soundwave">
      <div class="bar" style="--bs:.5s;--bd:0s"></div>
      <div class="bar" style="--bs:.7s;--bd:.1s"></div>
      <div class="bar" style="--bs:.6s;--bd:.2s"></div>
      <div class="bar" style="--bs:.8s;--bd:.05s"></div>
    </div>
  </div>
  <div class="progress-wrap" id="progressWrap">
    <div class="progress-fill" id="progressFill"></div>
  </div>
  <div class="controls">
    <button class="ctrl-btn" id="prevBtn" title="Previous">&#9664;&#9664;</button>
    <button class="ctrl-btn play-btn" id="playBtn">&#9654;</button>
    <button class="ctrl-btn" id="nextBtn" title="Next">&#9654;&#9654;</button>
  </div>
  <div class="queue-label" id="queueLabel">1 / 6 &nbsp;·&nbsp; getul is cool</div>
</div>

<script>
/* ── TRACKS ── */
const tracks = [
  { name: 'From The Start',       artist: 'Laufey',       art: '🌸', spotify: 'https://open.spotify.com/track/2GMVoGMdCxjRqHJJmh0FWH' },
  { name: 'Bewitched',            artist: 'Laufey',       art: '✨', spotify: 'https://open.spotify.com/track/4TqBMJuVBHZCigKJxFnq5s' },
  { name: 'Ivy',                  artist: 'Frank Ocean',  art: '🍃', spotify: 'https://open.spotify.com/track/2ZWlPOoWh0626oTaHrnl2a' },
  { name: 'Nights',               artist: 'Frank Ocean',  art: '🌙', spotify: 'https://open.spotify.com/track/7eqoqGkKwgOaWNNHx90uEZ' },
  { name: 'Passionfruit',         artist: 'Drake',        art: '🍑', spotify: 'https://open.spotify.com/track/7qEHsqek33rTcFNT9PFqLf' },
  { name: 'Hold On, We\'re Going Home', artist: 'Drake', art: '🏠', spotify: 'https://open.spotify.com/track/54t8hwCCoF1aOAF2TRpAF2' },
];

let current = 0;
let isPlaying = false;
let progress = 0;
let progressInterval = null;
const TRACK_DURATION = 9999999; // fake 3.5min

const trackNameEl  = document.getElementById('trackName');
const trackArtEl   = document.getElementById('trackArtist');
const albumArtEl   = document.getElementById('albumArt');
const playBtn      = document.getElementById('playBtn');
const progressFill = document.getElementById('progressFill');
const queueLabel   = document.getElementById('queueLabel');
const bars         = document.querySelectorAll('.bar');

function loadTrack(idx) {
  const t = tracks[idx];
  trackNameEl.textContent  = t.name;
  trackArtEl.textContent   = t.artist;
  albumArtEl.textContent   = t.art;
  queueLabel.textContent   = `${idx+1} / ${tracks.length}  ·  iya sucks!`;
  progress = 0;
  updateProgress();
}

function updateProgress() {
  progressFill.style.width = ((progress / TRACK_DURATION) * 100) + '%';
}

function play() {
  isPlaying = true;
  playBtn.innerHTML = '&#9646;&#9646;';
  albumArtEl.classList.add('playing');
  bars.forEach(b => b.classList.add('active'));
  clearInterval(progressInterval);
  progressInterval = setInterval(() => {
    progress++;
    if (progress >= TRACK_DURATION) { progress = 0; next(); return; }
    updateProgress();
  }, 1000);
}

function pause() {
  isPlaying = false;
  playBtn.innerHTML = '&#9654;';
  albumArtEl.classList.remove('playing');
  bars.forEach(b => b.classList.remove('active'));
  clearInterval(progressInterval);
}

function next() {
  current = (current + 1) % tracks.length;
  loadTrack(current);
  if (isPlaying) play();
}

function prev() {
  if (progress > 5) { progress = 0; updateProgress(); return; }
  current = (current - 1 + tracks.length) % tracks.length;
  loadTrack(current);
  if (isPlaying) play();
}

playBtn.addEventListener('click', () => isPlaying ? pause() : play());
document.getElementById('nextBtn').addEventListener('click', next);
document.getElementById('prevBtn').addEventListener('click', prev);
document.getElementById('queueLabel').addEventListener('click', () => window.open(tracks[current].spotify, '_blank'));
albumArtEl.addEventListener('click', () => window.open(tracks[current].spotify, '_blank'));
albumArtEl.style.cursor = 'pointer';

loadTrack(0);

/* ── AURORA BLOBS ── */
function buildAurora() {
  const layer = document.getElementById('aurora');
  layer.innerHTML = '';
  const blobs = [
    { w:500, h:300, top:'10%',  left:'5%',  color:'rgba(180,80,160,0.12)', at:'14s', ad:'0s',  mx:'60px',  my:'-40px', mx2:'-30px', my2:'20px' },
    { w:400, h:250, top:'50%',  left:'55%', color:'rgba(80,60,180,0.10)',  at:'18s', ad:'3s',  mx:'-50px', my:'30px',  mx2:'40px',  my2:'-20px' },
    { w:350, h:200, top:'5%',   left:'60%', color:'rgba(255,100,160,0.08)',at:'11s', ad:'6s',  mx:'30px',  my:'50px',  mx2:'-60px', my2:'10px' },
    { w:300, h:200, top:'65%',  left:'10%', color:'rgba(100,80,200,0.09)', at:'16s', ad:'2s',  mx:'-40px', my:'-30px', mx2:'50px',  my2:'40px' },
  ];
  blobs.forEach(b => {
    const el = document.createElement('div');
    el.className = 'aurora-blob';
    el.style.cssText = `
      width:${b.w}px; height:${b.h}px;
      top:${b.top}; left:${b.left};
      background:${b.color};
      --at:${b.at}; --ad:${b.ad};
      --mx:${b.mx}; --my:${b.my};
      --mx2:${b.mx2}; --my2:${b.my2};
    `;
    layer.appendChild(el);
  });
}
buildAurora();

/* ── WEATHER ── */
const WEATHER = { temp: 62, condition: 'cloudy', isDay: false };

function applyWeather(w) {
  const { condition, isDay, temp } = w;
  const timeKey = isDay ? 'day' : 'night';
  const condKey = condition.includes('rain') || condition.includes('drizzle') ? 'rain'
                : condition.includes('cloud') || condition.includes('overcast') || condition.includes('mist') || condition.includes('fog') ? 'cloudy'
                : condition.includes('snow') || condition.includes('sleet') ? 'rain'
                : 'clear';
  document.body.className = `${timeKey}-${condKey}`;
  document.getElementById('temp').textContent = `${Math.round(temp)}°F`;
  document.getElementById('cond-text').textContent = condition.charAt(0).toUpperCase() + condition.slice(1);
  document.getElementById('moonEmoji').textContent = isDay ? '☀️' : '🌙';
  buildStars(isDay ? 0 : condKey === 'clear' ? 90 : condKey === 'cloudy' ? 35 : 12);
  buildClouds(condKey === 'clear' ? 3 : condKey === 'cloudy' ? 9 : 13, condKey);
  if (condKey === 'rain') buildRain(130);
}

function buildStars(count) {
  const layer = document.getElementById('star-layer');
  layer.innerHTML = '';
  for (let i = 0; i < count; i++) {
    const s = document.createElement('div');
    s.className = 'star';
    const size = Math.random() * 2 + 0.4;
    s.style.cssText = `width:${size}px;height:${size}px;top:${Math.random()*92}%;left:${Math.random()*100}%;--td:${2+Math.random()*5}s;--dd:${Math.random()*5}s;`;
    layer.appendChild(s);
  }
}

function buildClouds(count, condKey) {
  const layer = document.getElementById('cloud-layer');
  layer.innerHTML = '';
  for (let i = 0; i < count; i++) {
    const c = document.createElement('div');
    c.className = 'cloud';
    const w = 130 + Math.random() * 210;
    const h = w * 0.36;
    const op = condKey === 'clear' ? 0.07 + Math.random()*0.06
              : condKey === 'cloudy' ? 0.18 + Math.random()*0.18
              : 0.3 + Math.random()*0.25;
    c.style.cssText = `width:${w}px;height:${h}px;top:${Math.random()*(condKey==='clear'?50:75)}%;left:${Math.random()*100}%;opacity:${op};--cd:${32+Math.random()*55}s;--cde:-${Math.random()*45}s;`;
    layer.appendChild(c);
  }
}

function buildRain(count) {
  const layer = document.getElementById('rain-layer');
  layer.innerHTML = '';
  for (let i = 0; i < count; i++) {
    const d = document.createElement('div');
    d.className = 'drop';
    const h = 14 + Math.random() * 24;
    d.style.cssText = `left:${Math.random()*110}%;height:${h}px;--rd:${0.4+Math.random()*0.9}s;--rde:-${Math.random()*1.6}s;`;
    layer.appendChild(d);
  }
}

async function fetchWeather() {
  try {
    const res = await fetch('https://wttr.in/Fresno,CA?format=j1', { signal: AbortSignal.timeout(4000) });
    const data = await res.json();
    const cur  = data.current_condition[0];
    const desc = cur.weatherDesc[0].value.toLowerCase();
    const tempF = parseInt(cur.temp_F);
    const hourPST = ((new Date().getUTCHours() - 8) + 24) % 24;
    const isDay = hourPST >= 6 && hourPST < 20;
    const condition = desc.includes('rain') || desc.includes('drizzle') ? 'rain'
                    : desc.includes('cloud') || desc.includes('overcast') || desc.includes('mist') ? 'cloudy'
                    : desc.includes('snow') || desc.includes('sleet') ? 'rain'
                    : 'clear';
    applyWeather({ temp: tempF, condition, isDay });
  } catch {
    applyWeather(WEATHER);
  }
}

fetchWeather();
</script>
</body>
</html>
