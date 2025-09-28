
## 👨‍💻 Sobre mim
- 🎓 Cursando **Tecnologias em Computação** na UFF  
- 📚 Fiz **3 períodos na FAETERJ Petrópolis**  
- 🖥️ Tenho curso de **Programador de Sistemas pela Estácio**
- 🖥️ Tenho curso de **Cibersegurança pela CISCO®**  
- 🔎 Focado em aprender tanto **Back-end** quanto **Front-end**, explorando diferentes stacks  
- 🚀 Sempre buscando novos desafios para expandir meus conhecimentos, viciado em escrever códigos limpos  

---

## 🚀 Skills & Ferramentas

<div align="left">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/html5/html5-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/css3/css3-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/typescript/typescript-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodejs/nodejs-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/git/git-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/github/github-original.svg" width="40" />
</div>

---
## 📫 Conheça mais sobre mim:
- 💼 [LinkedIn](www.linkedin.com/in/joão-gabriel-menezes-marra-45b1381a9).  
- 📧 **joaommenezes100@gmail.com**

---
<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Caça ao Café — Mini Jogo</title>
  <style>
    :root{--bg:#0f1724;--card:#0b1220;--accent:#ffb86b;--muted:#9aa4b2;}
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter,Segoe UI,Arial;background:linear-gradient(180deg,#071021 0%, #0f1724 100%);color:#e6eef6;display:flex;align-items:center;justify-content:center;min-height:100vh}
    .wrap{width:min(720px,94vw);background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(0,0,0,0.05));border:1px solid rgba(255,255,255,0.03);padding:20px;border-radius:12px;box-shadow:0 8px 30px rgba(2,6,23,0.6)}
    h1{margin:0 0 8px;font-size:20px;display:flex;align-items:center;gap:10px}
    p.lead{margin:0 0 16px;color:var(--muted);font-size:13px}
    .game-area{position:relative;background:linear-gradient(180deg,#07121a,#0d1b27);height:320px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);overflow:hidden}
    .target{position:absolute;width:64px;height:64px;border-radius:50%;background:radial-gradient(circle at 30% 30%, #fff6c8 0 10%, #ffef9e 10% 30%, #ffb86b 31% 60%, #d97706 61%);display:flex;align-items:center;justify-content:center;color:#2b1a00;font-weight:700;font-size:20px;cursor:pointer;box-shadow:0 6px 18px rgba(255,184,107,0.15), inset 0 -6px 12px rgba(0,0,0,0.15);transition:transform 120ms ease}
    .hud{display:flex;gap:12px;align-items:center;margin-top:12px}
    .stat{background:rgba(255,255,255,0.03);padding:8px 12px;border-radius:8px;font-weight:600;font-size:14px}
    .controls{margin-left:auto;display:flex;gap:8px}
    button{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:8px 12px;border-radius:8px;color:inherit;cursor:pointer}
    button.primary{background:linear-gradient(90deg,var(--accent),#ff9a44);border:none;color:#2b1a00;font-weight:700}
    .footer{margin-top:12px;color:var(--muted);font-size:13px}
    .small{font-size:12px;color:var(--muted)}
    @media (max-width:520px){.target{width:54px;height:54px;font-size:18px}}
  </style>
</head>
<body>
  <div class="wrap">
    <h1>☕ Caça ao Café — Mini Joguinho</h1>
    <p class="lead">Clique na xícara que aparece na tela para marcar pontos. A cada acerto a xícara se move e o tempo diminui — veja quantos pontos você consegue em 30 segundos!</p>

    <div class="game-area" id="gameArea" aria-label="Área do jogo">
      <!-- Target será criado via JS -->
    </div>

    <div class="hud">
      <div class="stat">Tempo: <span id="time">30</span>s</div>
      <div class="stat">Pontos: <span id="score">0</span></div>
      <div class="stat">Melhor: <span id="best">0</span></div>
      <div class="controls">
        <button id="startBtn" class="primary">Iniciar</button>
        <button id="resetBtn">Resetar</button>
      </div>
    </div>

    <div class="footer">
      <div class="small">Dica: Clique rápido! A cada acerto a xícara fica mais rápida e menor. Salve o arquivo <code>jogo/index.html</code> no seu repositório e adicione um link no seu README para abrir o jogo.</div>
    </div>
  </div>

  <script>
    const gameArea = document.getElementById('gameArea')
    const timeEl = document.getElementById('time')
    const scoreEl = document.getElementById('score')
    const bestEl = document.getElementById('best')
    const startBtn = document.getElementById('startBtn')
    const resetBtn = document.getElementById('resetBtn')

    let time = 30
    let score = 0
    let best = Number(localStorage.getItem('cafebest') || 0)
    bestEl.textContent = best
    let timer = null
    let running = false
    let target = null
    let speed = 1000 // tempo entre movimentos (ms)
    let size = 64

    function createTarget(){
      target = document.createElement('button')
      target.className = 'target'
      target.setAttribute('aria-label','xícara de café')
      target.innerHTML = '☕'
      target.style.width = size + 'px'
      target.style.height = size + 'px'
      target.addEventListener('click', hit)
      gameArea.appendChild(target)
      moveTarget(true)
    }

    function moveTarget(justCreated = false){
      const pad = 8
      const areaRect = gameArea.getBoundingClientRect()
      const maxX = Math.max(0, areaRect.width - size - pad)
      const maxY = Math.max(0, areaRect.height - size - pad)
      const x = Math.round(Math.random()*maxX) + pad/2
      const y = Math.round(Math.random()*maxY) + pad/2
      target.style.transform = `translate(${x}px, ${y}px)`
      if(!justCreated){
        // animação rápida no clique
        target.style.transition = 'transform 120ms ease'
      } else {
        target.style.transition = 'transform 220ms ease'
      }
    }

    function hit(e){
      if(!running) return
      // incrementar pontos
      score += 1
      scoreEl.textContent = score
      // reduzir tempo entre movimentos (acelera)
      speed = Math.max(300, speed - 40)
      // reduzir o tamanho um pouco a cada 5 pontos
      if(score % 5 === 0){
        size = Math.max(36, size - 6)
        target.style.width = size + 'px'
        target.style.height = size + 'px'
      }
      // mover para nova posição imediata
      moveTarget()
    }

    function tick(){
      time -= 1
      timeEl.textContent = time
      if(time <= 0){
        stopGame()
      }
    }

    function gameLoop(){
      if(!running) return
      moveTarget()
      setTimeout(gameLoop, speed)
    }

    function startGame(){
      if(running) return
      running = true
      time = 30
      score = 0
      speed = 1000
      size = 64
      timeEl.textContent = time
      scoreEl.textContent = score
      if(!target) createTarget()
      target.style.width = size + 'px'
      target.style.height = size + 'px'
      timer = setInterval(tick, 1000)
      // começar loop de movimento
      setTimeout(gameLoop, speed)
      startBtn.textContent = 'Jogando...'
      startBtn.disabled = true
    }

    function stopGame(){
      running = false
      clearInterval(timer)
      startBtn.textContent = 'Iniciar'
      startBtn.disabled = false
      // atualizar melhor pontuação
      if(score > best){
        best = score
        bestEl.textContent = best
        localStorage.setItem('cafebest', best)
      }
      // efeito de final
      if(target) {
        target.style.transform += ' scale(0.8)'
      }
    }

    function resetAll(){
      stopGame()
      time = 30
      score = 0
      speed = 1000
      size = 64
      timeEl.textContent = time
      scoreEl.textContent = score
      if(target){
        target.remove()
        target = null
      }
      startBtn.disabled = false
    }

    startBtn.addEventListener('click', startGame)
    resetBtn.addEventListener('click', resetAll)

    // teclado: Espaço para iniciar
    window.addEventListener('keydown', (e)=>{
      if(e.code === 'Space'){
        e.preventDefault()
        if(!running) startGame()
      }
    })

    // acessibilidade: ajustar posição quando a janela muda
    window.addEventListener('resize', ()=>{
      if(target) moveTarget()
    })
  </script>
</body>
</html>
