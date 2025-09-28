<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>For Becky — A Little Love Page</title>
  <style>
    :root{
      --bg1:#ff4d4d; /* red */
      --bg2:#4d6bff; /* blue */
      --glass: rgba(255,255,255,0.08);
      --accent: rgba(255,255,255,0.9);
    }
    /* cool animated multicolor background (red <-> blue) */
    html,body{height:100%;margin:0;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,'Helvetica Neue',Arial}
    body{
      background: linear-gradient(120deg,var(--bg1),var(--bg2));
      background-size:400% 400%;
      animation: bgmove 12s ease-in-out infinite;
      color:var(--accent);
      -webkit-font-smoothing:antialiased;
      display:flex;align-items:center;justify-content:center;padding:20px;
    }
    @keyframes bgmove{
      0%{background-position:0% 50%}
      50%{background-position:100% 50%}
      100%{background-position:0% 50%}
    }

    .card{
      width:100%;max-width:920px;border-radius:20px;padding:26px;background:linear-gradient(180deg, rgba(255,255,255,0.06), rgba(255,255,255,0.03));box-shadow:0 10px 40px rgba(0,0,0,0.25);backdrop-filter: blur(8px);
    }

    header{display:flex;align-items:center;gap:18px}
    .avatar{
      width:84px;height:84px;border-radius:18px;background:linear-gradient(135deg, rgba(255,255,255,0.08), rgba(255,255,255,0.02));display:flex;align-items:center;justify-content:center;font-weight:700;font-size:26px;color:rgba(255,255,255,0.95);
      box-shadow:0 6px 18px rgba(0,0,0,0.35) inset;
    }

    h1{margin:0;font-size:28px;letter-spacing:0.2px}
    p.lead{margin:6px 0 0 0;opacity:0.95}

    .row{display:flex;gap:18px;margin-top:18px}
    .col{flex:1;min-width:220px}

    .panel{background:rgba(255,255,255,0.03);padding:16px;border-radius:14px}

    /* glowing love badge */
    .love-badge{display:inline-block;padding:12px 18px;border-radius:999px;font-weight:700;margin-top:8px;position:relative;box-shadow:0 6px 30px rgba(255,80,120,0.18)}
    .love-glow{font-size:22px;color:#fff;text-shadow:0 0 12px rgba(255,120,170,0.9), 0 0 36px rgba(255,80,160,0.45);animation:pulse 2s infinite}
    @keyframes pulse{0%{transform:scale(1);filter:brightness(1)}50%{transform:scale(1.03);filter:brightness(1.12)}100%{transform:scale(1)} }

    .messages{display:grid;gap:10px}
    .msg{padding:10px;border-radius:10px;background:linear-gradient(90deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));}
    .msg.heart{display:flex;align-items:center;gap:10px}

    /* riddles */
    .riddle{margin-top:10px}
    .riddle h3{margin:0 0 6px 0;font-size:16px}
    .riddle input{width:100%;padding:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.08);background:transparent;color:var(--accent);outline:none}
    .riddle button{margin-top:8px;padding:10px 12px;border-radius:10px;border:none;background:rgba(255,255,255,0.1);cursor:pointer}

    footer{margin-top:16px;text-align:center;font-size:13px;opacity:0.95}

    /* upload area */
    .uploader{display:flex;align-items:center;justify-content:center;border:2px dashed rgba(255,255,255,0.08);padding:18px;border-radius:12px;min-height:120px;flex-direction:column;gap:8px}
    .thumbs{display:flex;gap:8px;flex-wrap:wrap;margin-top:10px}
    .thumbs img{width:80px;height:80px;object-fit:cover;border-radius:10px;box-shadow:0 6px 18px rgba(0,0,0,0.3)}

    /* small screens */
    @media (max-width:720px){
      header{gap:12px}
      .row{flex-direction:column}
    }

    /* confetti canvas sits on top when triggered */
    #confettiCanvas{position:fixed;left:0;top:0;width:100%;height:100%;pointer-events:none;z-index:9999}
  </style>
</head>
<body>
  <canvas id="confettiCanvas"></canvas>
  <div class="card">
    <header>
      <div class="avatar" id="initials">B</div>
      <div style="flex:1">
        <h1>For Becky — little notes to make you smile</h1>
        <p class="lead">Tiny love, big smile. A special place just for you 💕</p>
        <div class="love-badge"><span class="love-glow">love 💕</span></div>
      </div>
      <div style="text-align:right">
        <small>Open on phone for best effect</small>
      </div>
    </header>

    <div class="row">
      <div class="col">
        <div class="panel">
          <h2 style="margin-top:0">Sweet Messages</h2>
          <div class="messages" id="messages">
            <!-- messages inserted by JS -->
          </div>
        </div>

        <div class="panel" style="margin-top:14px">
          <h2 style="margin-top:0">Riddles (solve them and get a surprise!)</h2>

          <div id="riddlesArea">
            <!-- riddles inserted by JS -->
          </div>

        </div>
      </div>

      <div class="col">
        <div class="panel">
          <h2>Photo corner</h2>
          <p>Drop or pick a few of Becky’s pictures (they stay local on your phone — no upload anywhere).</p>
          <div class="uploader" id="dropZone">
            <div>Tap to choose photos or drag them here</div>
            <input id="fileInput" type="file" accept="image/*" multiple style="display:none">
            <div class="thumbs" id="thumbs"></div>
          </div>
        </div>

        <div class="panel" style="margin-top:14px">
          <h2>Little secret lock</h2>
          <p>Type Becky (or Rebecca) to unlock an extra message</p>
          <input id="unlockInput" placeholder="Type her name..." />
          <button id="unlockBtn">Unlock</button>
          <div id="secret" style="margin-top:10px;display:none;padding:10px;border-radius:10px;background:rgba(255,255,255,0.02)">Hey Becky — you stole my heart. 💖</div>
        </div>
      </div>
    </div>

    <footer>Made with a big smile — deliver this page to Becky and watch her grin 😊</footer>
  </div>

  <script>
    // lovely messages array
    const messages = [
      "You light up every room — and my phone screen when you smile.",
      "If smiles were stars, you'd be the whole sky.",
      "Becky — you make ordinary days feel magical.",
      "Tiny note: I hope your day is as bright as your laugh.",
      "PS: You look amazing when you read this. Keep smiling. ❤️"
    ];

    // riddles (answer checking is case-insensitive; simple fun ones)
    const riddles = [
      {q: "I have keys but no locks. I have space but no room. You can enter, but can't go outside. What am I?", a: "keyboard"},
      {q: "What has a heart that doesn’t beat?", a: "artichoke"},
      {q: "I’m not alive, but I grow. I don’t have lungs, but I need air. What am I?", a: "fire"}
    ];

    // render messages
    const msgs = document.getElementById('messages');
    messages.forEach((m,i)=>{
      const d = document.createElement('div');d.className='msg'+(i===0?' heart':'');d.innerHTML = `<strong>Note ${i+1}:</strong> ${m}`;msgs.appendChild(d);
    });

    // render riddles with inputs
    const riddlesArea = document.getElementById('riddlesArea');
    riddles.forEach((r,idx)=>{
      const wrap = document.createElement('div');wrap.className='riddle panel';
      wrap.innerHTML = `<h3>Riddle ${idx+1}</h3><div style="font-size:14px;margin-bottom:8px">${r.q}</div><input placeholder="Your answer..." id="ranswer${idx}"><button data-idx="${idx}">Check answer</button><div class="rresult" id="rres${idx}" style="margin-top:8px"></div>`;
      riddlesArea.appendChild(wrap);
    });

    // check answer logic
    riddlesArea.addEventListener('click',e=>{
      if(e.target.tagName!=='BUTTON')return;
      const idx = +e.target.dataset.idx;
      const input = document.getElementById('ranswer'+idx);
      const res = document.getElementById('rres'+idx);
      const ans = input.value.trim().toLowerCase();
      if(!ans){res.innerText='Please type an answer.';return}
      if(ans === riddles[idx].a.toLowerCase()){
        res.innerHTML = `<strong>Correct!</strong> Sweet — you got it. 💖`;
        fireConfetti();
      } else {
        res.innerText = 'Not quite — try again!';
      }
    });

    // unlock secret
    document.getElementById('unlockBtn').addEventListener('click',()=>{
      const v = document.getElementById('unlockInput').value.trim().toLowerCase();
      if(v === 'becky' || v === 'rebecca'){
        document.getElementById('secret').style.display='block';
        fireConfetti();
      } else {
        alert('Hmm, that name didn\'t unlock it. Try Becky.');
      }
    });

    // upload photos (local only)
    const drop = document.getElementById('dropZone');
    const fin = document.getElementById('fileInput');
    const thumbs = document.getElementById('thumbs');

    drop.addEventListener('click',()=>fin.click());
    fin.addEventListener('change',ev=>handleFiles(ev.target.files));

    drop.addEventListener('dragover',e=>{e.preventDefault();drop.style.opacity=0.9});
    drop.addEventListener('dragleave',e=>{drop.style.opacity=1});
    drop.addEventListener('drop',e=>{e.preventDefault();drop.style.opacity=1;handleFiles(e.dataTransfer.files)});

    function handleFiles(files){
      Array.from(files).slice(0,8).forEach(file=>{
        if(!file.type.startsWith('image/'))return;
        const r = new FileReader();
        r.onload = ()=>{
          const img = document.createElement('img');img.src = r.result;thumbs.appendChild(img);
        };r.readAsDataURL(file);
      });
    }

    // small confetti engine
    const confettiCanvas = document.getElementById('confettiCanvas');
    confettiCanvas.width = innerWidth; confettiCanvas.height = innerHeight;
    window.addEventListener('resize',()=>{confettiCanvas.width = innerWidth; confettiCanvas.height = innerHeight});
    const ctx = confettiCanvas.getContext('2d');
    let confettiPieces = [];
    function fireConfetti(){
      // create pieces
      for(let i=0;i<80;i++){
        confettiPieces.push({x:Math.random()*confettiCanvas.width,y:-10,vy:Math.random()*4+2,angle:Math.random()*360,s:Math.random()*6+4,lc:Math.random()>0.5? 'rgba(255,100,120,1)':'rgba(120,140,255,1)',life:Math.random()*80+60});
      }
      if(!anim)animate();
    }
    let anim=false;
    function animate(){anim=true;ctx.clearRect(0,0,confettiCanvas.width,confettiCanvas.height);
      for(let i=confettiPieces.length-1;i>=0;i--){
        const p = confettiPieces[i];
        p.y += p.vy; p.vy += 0.05; p.angle += 6;
        ctx.save();ctx.translate(p.x,p.y);ctx.rotate(p.angle*Math.PI/180);
        ctx.fillStyle = p.lc; ctx.fillRect(-p.s/2,-p.s/2,p.s,p.s*0.6);
        ctx.restore();
        p.life--; if(p.life<0 || p.y>confettiCanvas.height+50) confettiPieces.splice(i,1);
      }
      if(confettiPieces.length>0)requestAnimationFrame(animate); else{anim=false;ctx.clearRect(0,0,confettiCanvas.width,confettiCanvas.height)}
    }

    // set initials from known name
    document.getElementById('initials').innerText = 'B';

    // small accessibility: reveal messages one by one for added delight
    (function reveal(){
      const els = Array.from(document.querySelectorAll('.messages .msg'));
      let i=0; function step(){if(i>=els.length) return; els[i].style.opacity=0; els[i].style.transform='translateY(6px)';
        setTimeout(()=>{els[i].style.transition='all 500ms ease'; els[i].style.opacity=1; els[i].style.transform='translateY(0)'; i++; setTimeout(step,300)},80);
      }
      step();
    })();
  </script>
</body>
</html>
