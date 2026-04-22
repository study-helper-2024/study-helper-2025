<!DOCTYPE html>  
<html lang="ja">  
<head>  
<meta charset="UTF-8">  
<meta name="viewport" content="width=device-width, initial-scale=1.0">  
<title>English Vocabulary Training Sheet</title>  
<style>  
/* デザイン：学校のタブレットでも軽く動くシンプル構成 */  
body { font-family: sans-serif; background: #fff; margin: 0; padding: 20px; color: #333; }  
.hidden { display: none !important; }  
.study-container { max-width: 450px; margin: 40px auto; padding: 25px; border: 1px solid #ddd; border-radius: 10px; text-align: center; }  
input[type="text"] { padding: 10px; border: 1px solid #ccc; border-radius: 5px; width: 75%; margin-bottom: 15px; }  
.btn { padding: 10px 20px; background: #007bff; color: white; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; }  
/* 裏メニュー：検索とリスト */  
#secret-area { max-width: 700px; margin: auto; text-align: center; }  
.panel { border: 2px solid #222; padding: 15px; background: #f9f9f9; border-radius: 12px; margin-top: 15px; }  
#result-list { height: 450px; overflow-y: scroll; border: 1px solid #eee; margin-top: 10px; text-align: left; background: #fff; }  
.v-item { display: flex; align-items: center; padding: 10px; border-bottom: 1px solid #ddd; cursor: pointer; }  
.v-item:hover { background: #f0f7ff; }  
.v-item img { width: 110px; margin-right: 12px; border-radius: 5px; }  
.v-info { font-size: 0.9em; font-weight: bold; }  
/* プレイヤー画面（オーバーレイ） */  
#player-screen { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: #000; z-index: 2000; color: #fff; display: flex; flex-direction: column; }  
#player-screen iframe { width: 100%; height: 60%; border: none; }  
.controls { padding: 15px; text-align: center; flex: 1; overflow-y: auto; }  
.close-x { position: absolute; top: 15px; right: 15px; background: #e74c3c; padding: 8px 15px; border-radius: 5px; cursor: pointer; font-weight: bold; }  
.m-btn { padding: 12px; width: 85%; max-width: 350px; margin: 6px auto; border: none; color: #fff; font-weight: bold; border-radius: 8px; cursor: pointer; display: block; }  
</style>  
</head>  
<body>  
<!-- 【表】英単語シート -->  
<div id="front" class="study-container">  
<h2 style="color:#2c3e50;">中学英単語マスター</h2>  
<p>1. Dog : [ いぬ ]</p>  
<p>2. [ <input type="text" id="i2" placeholder="答えを入力"> ] <button class="btn" onclick="chk(2)">判定</button></p>  
<p>3. [ <input type="text" id="i3" placeholder="答えを入力"> ] <button class="btn" onclick="chk(3)">判定</button></p>  
<p style="font-size: 0.7em; color: #aaa; margin-top: 30px;">© 2024 Study Helper System</p>  
</div>  
<!-- 【裏】YouTube & プロキシ -->  
<div id="secret-area" class="hidden">  
<div id="yt-ui" class="panel">  
<h3 id="mode-name">YouTube Mode</h3>  
<input type="text" id="yt-q" style="width:70%;" placeholder="YouTuber名か動画名を入力">  
<button class="btn" onclick="search()">検索</button>  
<div id="result-list"><p style="padding:30px; text-align:center; color:#999;">検索ワードを入力してください</p></div>  
</div>  
<!-- プロキシ案内（グルトン用） -->  
<div id="proxy-ui" class="panel hidden">  
<h3>Network Gateway</h3>  
<button class="m-btn" style="background:#27ae60" onclick="++[window.open](http://window.open/)++('++[https://cobalt.tools](https://cobalt.tools/)++','_blank')">Cobalt (URL直接保存サイト)</button>  
<button class="m-btn" style="background:#8e44ad" onclick="++[window.open](http://window.open/)++('++[https://duckduckgo.com](https://duckduckgo.com/)++','_blank')">匿名検索エンジン</button>  
</div>  
<button onclick="location.reload()" style="background:none; border:none; color:#ddd; cursor:pointer; margin-top:15px;">[ ログアウト ]</button>  
</div>  
<!-- プレイヤー画面 -->  
<div id="player-screen" class="hidden">  
<div class="close-x" onclick="cls()">× 閉じる</div>  
<div id="v-frame"></div>  
<div class="controls">  
<h4 id="v-title" style="margin: 10px;"></h4>  
<p style="font-size: 0.8em; color: #ccc;">再生できない時はサーバーを切り替えてください：</p>  
<button class="m-btn" style="background:#d35400;" onclick="loadV('A')">方法A：Piped (通信偽装：最強)</button>  
<button class="m-btn" style="background:#2c3e50;" onclick="loadV('B')">方法B：Invidious (軽量：サクサク)</button>  
<button class="m-btn" style="background:#2980b9;" onclick="loadV('C')">方法C：Proxy (ドメイン暗号化)</button>  
<hr style="border:0.1px solid #333; margin: 15px;">  
<button id="dl-x" class="m-btn" style="background:#c0392b; border: 1px solid #fff;">この動画をダウンロード / 保存</button>  
</div>  
</div>  
<script>  
let curId = "";  
// 1. 合言葉判定  
function chk(n) {  
const v = document.getElementById('i' + n).value;  
if ((n === 2 && v === 'ジュンセイ神') || (n === 3 && v === 'グルトン')) {  
document.getElementById('front').classList.add('hidden');  
document.getElementById('secret-area').classList.remove('hidden');  
if (v === 'グルトン') document.getElementById('proxy-ui').classList.remove('hidden');  
}  
}  
// 2. YouTuber検索（アカウント・APIキー不要）  
async function search() {  
const q = document.getElementById('yt-q').value;  
const resList = document.getElementById('result-list');  
if (!q) return;  
resList.innerHTML = "<p style='padding:30px; text-align:center;'>検索中...</p>";  
try {  
// InvidiousのAPIを利用してアカウントなし検索  
const res = await fetch(`++[https://lunar.icu{encodeURIComponent(q)}`](https://lunar.icu%7Bencodeuricomponent(q)%7D%60/)++);  
const data = await res.json();  
resList.innerHTML = "";  
data.forEach(item => {  
if (item.type !== 'video') return;  
const d = document.createElement('div');  
d.className = 'v-item';  
d.onclick = () => openSet(item.videoId, item.title);  
d.innerHTML = `<img src="++[https://lunar.icu{item.videoId}/mqdefault.jpg"><div](https://lunar.icu%7Bitem.videoid%7D/mqdefault.jpg%22%3E%3Cdiv)++ class="v-info">${item.title}</div>`;  
d.innerHTML = `<img src="++[https://lunar.icu{item.videoId}/mqdefault.jpg"><div](https://lunar.icu%7Bitem.videoid%7D/mqdefault.jpg%22%3E%3Cdiv)++ class="v-info">${item.title}</div>`;  
resList.appendChild(d);  
});  
} catch (e) {  
resList.innerHTML = "<p style='color:red; padding:30px; text-align:center;'>サーバーエラー。時間をおいて再試行してください。</p>";  
}  
}  
// 3. 再生画面の準備  
function openSet(id, title) {  
curId = id;  
document.getElementById('v-title').innerText = title;  
document.getElementById('player-screen').classList.remove('hidden');  
document.getElementById('v-frame').innerHTML = "<div style='padding:80px; text-align:center;'>サーバーを選んで再生を開始してください</div>";  
document.getElementById('dl-x').onclick = () => {  
++[window.open](http://window.open/)++(`++[https://cobalt.tools{id}`](https://cobalt.tools%7Bid%7D%60/)++, '_blank');  
++[window.open](http://window.open/)++(`++[https://cobalt.tools{id}`](https://cobalt.tools%7Bid%7D%60/)++, '_blank');  
};  
}  
// 4. サーバー別再生処理（No-Cookieブロック対応）  
function loadV(type) {  
const f = document.getElementById('v-frame');  
let u = "";  
if (type === 'A') u = `++[https://piped.video{curId}?autoplay=1`](https://piped.video%7Bcurid%7D/?autoplay=1%60)++;  
if (type === 'A') u = `++[https://piped.video{curId}?autoplay=1`](https://piped.video%7Bcurid%7D/?autoplay=1%60)++;  
if (type === 'B') u = `++[https://yewtu.be{curId}`](https://yewtu.be%7Bcurid%7D%60/)++;  
if (type === 'B') u = `++[https://yewtu.be{curId}`](https://yewtu.be%7Bcurid%7D%60/)++;  
if (type === 'C') {  
// ドメインを隠すためのプロキシ経由  
const raw = `++[https://youtube.com{curId}`](https://youtube.com%7Bcurid%7D%60/)++;  
u = `++[https://drweb.jp{btoa(raw)}&hl=3e5`](https://drweb.jp%7Bbtoa(raw)%7D&hl=3e5%60/)++;  
u = `++[https://drweb.jp{btoa(raw)}&hl=3e5`](https://drweb.jp%7Bbtoa(raw)%7D&hl=3e5%60/)++;  
}  
f.innerHTML = `<iframe src="${u}" allow="autoplay; encrypted-media; picture-in-picture" allowfullscreen></iframe>`;  
}  
function cls() {  
document.getElementById('player-screen').classList.add('hidden');  
document.getElementById('v-frame').innerHTML = "";  
}  
</script>  
</body>  
</html>  
