<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>My HTML Games</title>
<style>
body { margin:0; font-family: Arial, sans-serif; background: #121212; color: #fff; }
header { display:flex; justify-content: space-between; align-items:center; padding: 15px 30px; background: #1e1e1e; }
header h1 { margin:0; font-size:24px; color:#00ff88; }
nav a { margin-left:20px; color:#fff; text-decoration:none; font-weight:bold; }
nav a:hover { color:#00ff88; }
.container { padding:20px 40px; }
h2 { color:#00ff88; }
.games-grid { display:grid; grid-template-columns: repeat(auto-fill,minmax(300px,1fr)); gap:20px; margin-top:20px; }
.game-card { background:#1e1e1e; border-radius:8px; overflow:hidden; box-shadow:0 2px 10px rgba(0,0,0,0.5); cursor:pointer; }
.game-card iframe { width:100%; height:300px; border:none; display:block; }
.game-card h3 { margin:10px; text-align:center; }
footer { text-align:center; padding:20px; background:#1e1e1e; margin-top:40px; color:#888; }
.modal { display:none; position:fixed; top:0; left:0; width:100%; height:100%; background: rgba(0,0,0,0.9); justify-content:center; align-items:center; z-index:1000; }
.modal iframe { width:90%; height:90%; border:none; }
.modal-close { position:absolute; top:20px; right:40px; font-size:30px; color:#fff; cursor:pointer; }
</style>
</head>
<body>

<header>
  <h1>My HTML Games</h1>
  <nav>
    <a href="#games">Games</a>
  </nav>
</header>

<div class="container">
  <section id="games">
    <h2>Game Gallery</h2>
    <div class="games-grid" id="gamesGrid"></div>
  </section>
</div>

<footer>
  &copy; 2026 My HTML Games
</footer>

<div class="modal" id="gameModal">
  <span class="modal-close" id="modalClose">&times;</span>
  <iframe id="modalIframe"></iframe>
</div>

<script>
const gamesGrid = document.getElementById('gamesGrid');
const modal = document.getElementById('gameModal');
const modalIframe = document.getElementById('modalIframe');
const modalClose = document.getElementById('modalClose');

// List of games stored in your /games folder
const games = [
  { title: "3D Snake", url: "games/3d-snake.html" },
  { title: "Tic Tac Toe", url: "games/tic-tac-toe.html" }
];

// Render game cards
games.forEach(game => {
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

// Modal close
modalClose.addEventListener('click', ()=> modal.style.display='none');
modal.addEventListener('click', e=> { if(e.target===modal) modal.style.display='none'; });
</script>
</body>
</html>
