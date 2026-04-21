<!DOCTYPE html>
<html lang="ko">
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>강아지 타자 게임</title>

<link rel="icon" href="icon.png">
<meta name="theme-color" content="#4CAF50">

<style>
body {
  margin:0;
  font-family:sans-serif;
  background: linear-gradient(#87CEFA,#fff);
  overflow:hidden;
}

.screen {
  display:none;
  height:100vh;
  text-align:center;
  padding:20px;
}
.active { display:block; }

/* 애니메이션 */
@keyframes bounce {
  0%{transform:translateY(0)}
  50%{transform:translateY(-20px)}
  100%{transform:translateY(0)}
}

@keyframes shake {
  0%{transform:translateX(0)}
  25%{transform:translateX(-5px)}
  50%{transform:translateX(5px)}
  75%{transform:translateX(-5px)}
  100%{transform:translateX(0)}
}

@keyframes pop {
  0%{transform:scale(1)}
  50%{transform:scale(1.3)}
  100%{transform:scale(1)}
}

#dog {
  font-size:90px;
  margin:20px;
}

/* 버튼 */
button {
  font-size:24px;
  padding:15px 30px;
  border:none;
  border-radius:15px;
  background:#4CAF50;
  color:white;
}

/* 단어 */
#word {
  font-size:42px;
  font-weight:bold;
  margin:20px;
}

/* 입력 */
input {
  font-size:26px;
  padding:12px;
  width:80%;
  border-radius:12px;
  border:2px solid #ddd;
}

/* 점수 */
#top {
  display:flex;
  justify-content:space-between;
  font-size:18px;
}

</style>
</head>

<body>

<!-- 시작 -->
<div id="start" class="screen active">
  <h1>🐶 타자 게임</h1>
  <button onclick="start()">시작</button>
</div>

<!-- 게임 -->
<div id="game" class="screen">
  <div id="top">
    <div id="score">0점</div>
    <div id="life">❤️❤️❤️</div>
  </div>

  <div id="dog">🐶</div>
  <div id="word"></div>

  <input id="input" placeholder="입력!">
  <br><br>
  <button onclick="check()">확인</button>
</div>

<!-- 종료 -->
<div id="over" class="screen">
  <h1>🏆 끝!</h1>
  <p id="final"></p>
  <button onclick="restart()">다시</button>
</div>

<!-- 효과음 -->
<audio id="ok" src="https://actions.google.com/sounds/v1/cartoon/clang_and_wobble.ogg"></audio>
<audio id="no" src="https://actions.google.com/sounds/v1/cartoon/boing.ogg"></audio>

<script>
const words = ["가방","학교","친구","연필","지우개","공책","책상","의자","선생님","교실"];

let i=0, score=0, life=3;

function screen(id){
  document.querySelectorAll(".screen").forEach(s=>s.classList.remove("active"));
  document.getElementById(id).classList.add("active");
}

function start(){
  screen("game");
  document.getElementById("input").value = "";
  show();
}

function show(){
  document.getElementById("word").textContent = words[i];
}

function sound(id){
  let a=document.getElementById(id);
  a.currentTime=0;
  a.play().catch(()=>{});
}

function animateDog(type){
  const dog=document.getElementById("dog");

  if(type==="good"){
    dog.style.animation="bounce 0.4s";
  } else {
    dog.style.animation="shake 0.3s";
  }

  setTimeout(()=>dog.style.animation="",400);
}

function animateWord(){
  const word=document.getElementById("word");
  word.style.animation="pop 0.3s";
  setTimeout(()=>word.style.animation="",300);
}

function check(){
  const input=document.getElementById("input").value;

  if(input===words[i]){
    score++;
    sound("ok");
    animateDog("good");
    animateWord();

    i++;
    document.getElementById("score").textContent = score + "점";
    document.getElementById("input").value="";

    if(i>=words.length) return end();

    show();

  } else {
    life--;
    sound("no");
    animateDog("bad");

    document.getElementById("life").textContent="❤️".repeat(life);

    if(life<=0) end();
  }
}

function end(){
  document.getElementById("final").textContent="점수: "+score;
  screen("over");
}

function restart(){
  i=0; score=0; life=3;
  document.getElementById("life").textContent="❤️❤️❤️";
  document.getElementById("input").value = "";
  screen("start");
}

document.getElementById("input").addEventListener("keydown", e=>{
  if(e.key==="Enter") check();
});
</script>

</body>
</html>
