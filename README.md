<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>My HTML Games</title>
<style>
  body { margin:0; font-family: Arial, sans-serif; background: #121212; color: #fff; }
  header { display:flex; justify-content: space-between; align-items:center; padding: 15px 30px; background: #1e1e1e; box-shadow:0 2px 5px rgba(0,0,0,0.5);}
  header h1 { margin:0; font-size:24px; color: #00ff88;}
  nav a { margin-left:20px; color:#fff; text-decoration:none; font-weight: bold;}
  nav a:hover { color: #00ff88; }
  .container { padding: 20px 40px; }
  h2 { color:#00ff88; }
  .games-grid { display:grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap:20px; margin-top:20px;}
  .game-card { background:#1e1e1e; border-radius:8px; overflow:hidden; box-shadow:0 2px 10px rgba(0,0,0,0.5); transition: transform 0.2s; cursor:pointer;}
  .game-card:hover { transform: scale(1.05);}
  .game-card iframe { width:100%; height:300px; border:none; display:block;}
  .game-card h3 { margin:10px; font-size:18px; text-align:center;}
  footer { text-align:center; padding:20px; background:#1e1e1e; margin-top:40px; color:#888;}
  /* Modal for full-screen preview */
  .modal { display:none; position:fixed; top:0; left:0; width:100%; height:100%; background: rgba(0,0,0,0.9); justify-content:center; align-items:center; z-index:1000;}
  .modal iframe { width:90%; height:90%; border:none; }
  .modal-close { position:absolute; top:20px; right:40px; font-size:30px; color:#fff; cursor:pointer;}
</style>
</head>
<body>

<header>
  <h1>My HTML Games</h1>
  <nav>
    <a href="#games">Games</a>
    <a href="#add-game">Add Game</a>
  </nav>
</header>

<div class="container">

  <section id="games">
    <h2>Game Gallery</h2>
    <div class="games-grid" id="gamesGrid">
      <!-- Game cards will be injected here -->
    </div>
  </section>

  <section id="add-game" style="margin-top:50px;">
    <h2>Add a New Game</h2>
    <form id="gameForm">
      <label>Game Title:<br>
        <input type="text" id="gameTitle" required style="width:100%; padding:8px; margin-top:5px;">
      </label><br><br>
      <label>Game URL (HTML file path):<br>
        <input type="text" id="gameURL" required placeholder="example: games/mygame.html" style="width:100%; padding:8px; margin-top:5px;">
      </label><br><br>
      <button type="submit" style="padding:10px 20px; background:#00ff88; color:#000; border:none; cursor:pointer; font-weight:bold;">Add Game</button>
    </form>
  </section>

</div>

<footer>
  &copy; 2026 My HTML Games
</footer>

<!-- Modal for full-screen game preview -->
<div class="modal" id="gameModal">
  <span class="modal-close" id="modalClose">&times;</span>
  <iframe id="modalIframe"></iframe>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r152/three.min.js"></script>
<script>
  const gamesGrid = document.getElementById('gamesGrid');
  const gameForm = document.getElementById('gameForm');
  const modal = document.getElementById('gameModal');
  const modalIframe = document.getElementById('modalIframe');
  const modalClose = document.getElementById('modalClose');

  // Embedded 3D Snake game as a data URI
  const snakeGameHTML = `
  <!DOCTYPE html><html lang="en"><head><meta charset="UTF-8"><title>3D Snake</title>
  <style>body{margin:0;overflow:hidden;background:#111;}canvas{display:block;}</style>
  </head><body><script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r152/three.min.js"></script>
  <script>
  const scene=new THREE.Scene();scene.background=new THREE.Color(0x111111);
  const camera=new THREE.PerspectiveCamera(75,window.innerWidth/window.innerHeight,0.1,1000);
  camera.position.set(10,15,20);camera.lookAt(0,0,0);
  const renderer=new THREE.WebGLRenderer({antialias:true});
  renderer.setSize(window.innerWidth,window.innerHeight);document.body.appendChild(renderer.domElement);
  const gridHelper=new THREE.GridHelper(20,20,0x444444,0x333333);scene.add(gridHelper);
  const ambientLight=new THREE.AmbientLight(0xffffff,0.6);scene.add(ambientLight);
  const directionalLight=new THREE.DirectionalLight(0xffffff,0.6);directionalLight.position.set(10,20,10);scene.add(directionalLight);
  const snake=[];let snakeDirection=new THREE.Vector3(1,0,0);let newDirection=snakeDirection.clone();let snakeLength=3;
  for(let i=0;i<snakeLength;i++){const cube=new THREE.Mesh(new THREE.BoxGeometry(1,1,1),new THREE.MeshStandardMaterial({color:0x00ff00}));
  cube.position.set(-i,0,0);scene.add(cube);snake.push(cube);}
  let food;function spawnFood(){if(food)scene.remove(food);const geometry=new THREE.BoxGeometry(1,1,1);
  const material=new THREE.MeshStandardMaterial({color:0xff0000});food=new THREE.Mesh(geometry,material);
  food.position.set(Math.floor(Math.random()*20-10),0,Math.floor(Math.random()*20-10));scene.add(food);}
  spawnFood();
  window.addEventListener('keydown',(e)=>{switch(e.key){case 'ArrowUp':if(snakeDirection.z===0)newDirection.set(0,0,-1);break;
  case 'ArrowDown':if(snakeDirection.z===0)newDirection.set(0,0,1);break;case 'ArrowLeft':if(snakeDirection.x===0)newDirection.set(-1,0,0);break;
  case 'ArrowRight':if(snakeDirection.x===0)newDirection.set(1,0,0);break;}});
  let lastTime=0;const speed=5;function animate(time){requestAnimationFrame(animate);
  const delta=(time-lastTime)/1000;if(delta>1/speed){lastTime=time;moveSnake();}renderer.render(scene,camera);}
  animate();function moveSnake(){snakeDirection.copy(newDirection);const headPos=snake[0].position.clone().add(snakeDirection);
  if(Math.abs(headPos.x)>10||Math.abs(headPos.z)>10){alert('Game Over!');window.location.reload();return;}
  for(let seg of snake){if(seg.position.equals(headPos)){alert('Game Over!');window.location.reload();return;}}
  const tail=snake.pop();tail.position.copy(headPos);snake.unshift(tail);
  if(headPos.equals(food.position)){const cube=new THREE.Mesh(new THREE.BoxGeometry(1,1,1),new THREE.MeshStandardMaterial({color:0x00ff00}));
  cube.position.copy(food.position);snake.push(cube);scene.add(cube);spawnFood();}}
  window.addEventListener('resize',()=>{camera.aspect=window.innerWidth/window.innerHeight;camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth,window.innerHeight);});
  </script></body></html>
  `;

  let games = [
    { title: "3D Snake", url: "data:text/html;base64," + btoa(snakeGameHTML) }
  ];

  function renderGames() {
    gamesGrid.innerHTML = "";
    games.forEach((game, index)=>{
      const card = document.createElement('div');
      card.className = "game-card";
      card.innerHTML = `
        <iframe src="${game.url}"></iframe>
        <h3>${game.title}</h3>
      `;
      card.addEventListener('click', ()=>{
        modal.style.display = "flex";
        modalIframe.src = game.url;
      });
      gamesGrid.appendChild(card);
    });
  }

  renderGames();

  // Add new game
  gameForm.addEventListener('submit', e=>{
    e.preventDefault();
    const title = document.getElementById('gameTitle').value;
    const url = document.getElementById('gameURL').value;
    games.push({title, url});
    renderGames();
    gameForm.reset();
  });

  // Modal
  modalClose.addEventListener('click', ()=> modal.style.display='none');
  modal.addEventListener('click', e=> { if(e.target===modal) modal.style.display='none'; });

</script>

</body>
</html>
