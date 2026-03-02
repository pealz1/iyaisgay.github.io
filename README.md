<html lang="en">
<head>
<meta charset="UTF-8">
<title>goodnight</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
html,body{
margin:0;
padding:0;
width:100%;
height:100%;
overflow:hidden;
font-family:ui-rounded,system-ui,-apple-system,BlinkMacSystemFont;
background:radial-gradient(circle at top,#fce4ec,#f48fb1,#c2185b);
}

#scene{
position:fixed;
inset:0;
}

.star{
position:absolute;
width:2px;
height:2px;
background:rgba(255,255,255,0.8);
border-radius:50%;
animation:twinkle 4s infinite ease-in-out;
}

@keyframes twinkle{
0%{opacity:.2}
50%{opacity:1}
100%{opacity:.2}
}

.blossom{
position:absolute;
width:36px;
height:36px;
opacity:0;
animation:blossomFloat 20s ease-in-out infinite;
}

@keyframes blossomFloat{
0%{transform:translateY(110vh) rotate(0deg);opacity:0}
5%{opacity:.75}
90%{opacity:.6}
100%{transform:translateY(-10vh) rotate(360deg);opacity:0}
}

#centerpiece{
position:absolute;
top:50%;
left:50%;
transform:translate(-50%,-50%);
text-align:center;
color:#fff;
animation:breathe 6s ease-in-out infinite;
}

@keyframes breathe{
0%{transform:translate(-50%,-50%) scale(1)}
50%{transform:translate(-50%,-50%) scale(1.03)}
100%{transform:translate(-50%,-50%) scale(1)}
}

#message{
font-size:clamp(36px,7vw,72px);
letter-spacing:.05em;
opacity:.95;
text-shadow:0 2px 24px rgba(194,24,91,.5);
}

#sub{
margin-top:14px;
font-size:18.5px;
opacity:.65;
letter-spacing:.03em;
}

#cursor-blossom{
position:fixed;
width:32px;
height:32px;
pointer-events:none;
transform:translate(-50%,-50%);
transition:transform .15s ease;
}

</style>
</head>
<body>

<div id="scene"></div>

<div id="centerpiece">
<div id="message"></div>
<div id="sub">sleep well big fat dragon!!</div>
</div>

<!-- cursor blossom -->
<svg id="cursor-blossom" viewBox="0 0 100 100">
  <g transform="translate(50,50)">
    <ellipse rx="10" ry="22" fill="#ffb6d5" opacity=".9" transform="rotate(0)"/>
    <ellipse rx="10" ry="22" fill="#ffb6d5" opacity=".9" transform="rotate(60)"/>
    <ellipse rx="10" ry="22" fill="#ffb6d5" opacity=".9" transform="rotate(120)"/>
    <ellipse rx="10" ry="22" fill="#ffd6ea" opacity=".9" transform="rotate(30)"/>
    <ellipse rx="10" ry="22" fill="#ffd6ea" opacity=".9" transform="rotate(90)"/>
    <ellipse rx="10" ry="22" fill="#ffd6ea" opacity=".9" transform="rotate(150)"/>
    <circle r="10" fill="#fff0f7"/>
    <circle r="4" fill="#f48fb1"/>
  </g>
</svg>

<script>
const scene=document.getElementById("scene")
const msg=document.getElementById("message")
const cursor=document.getElementById("cursor-blossom")

const texts=["goodnight iya"]
msg.textContent=texts[Math.floor(Math.random()*texts.length)]

// stars
for(let i=0;i<120;i++){
  const s=document.createElement("div")
  s.className="star"
  s.style.left=Math.random()*100+"%"
  s.style.top=Math.random()*100+"%"
  s.style.animationDelay=Math.random()*4+"s"
  scene.appendChild(s)
}

// floating blossoms (SVG)
const petalColors=[["#ffb6d5","#ffd6ea"],["#fce4ec","#f8bbd0"],["#fff0f7","#ffb6d5"]]
for(let i=0;i<18;i++){
  const svg=document.createElementNS("http://www.w3.org/2000/svg","svg")
  svg.setAttribute("viewBox","0 0 100 100")
  svg.classList.add("blossom")
  svg.style.left=Math.random()*100+"%"
  svg.style.bottom="0"
  svg.style.animationDelay=Math.random()*18+"s"
  svg.style.animationDuration=16+Math.random()*10+"s"
  const scale=0.5+Math.random()*1
  svg.style.width=scale*38+"px"
  svg.style.height=scale*38+"px"
  const [c1,c2]=petalColors[i%petalColors.length]
  svg.innerHTML=`
    <g transform="translate(50,50)">
      <ellipse rx="10" ry="22" fill="${c1}" opacity=".9" transform="rotate(0)"/>
      <ellipse rx="10" ry="22" fill="${c1}" opacity=".9" transform="rotate(60)"/>
      <ellipse rx="10" ry="22" fill="${c1}" opacity=".9" transform="rotate(120)"/>
      <ellipse rx="10" ry="22" fill="${c2}" opacity=".85" transform="rotate(30)"/>
      <ellipse rx="10" ry="22" fill="${c2}" opacity=".85" transform="rotate(90)"/>
      <ellipse rx="10" ry="22" fill="${c2}" opacity=".85" transform="rotate(150)"/>
      <circle r="9" fill="#fff0f7"/>
      <circle r="4" fill="#f48fb1"/>
    </g>`
  scene.appendChild(svg)
}

document.addEventListener("mousemove",e=>{
  cursor.style.left=e.clientX+"px"
  cursor.style.top=e.clientY+"px"
})

let t=0
function loop(){
  t+=0.002
  cursor.style.transform=`translate(-50%,-50%) rotate(${t*40}deg)`
  requestAnimationFrame(loop)
}
loop()
</script>

</body>
</html>
