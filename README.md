# givrin-
تحليل بي دكاء الإصطناعي
<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>AI Football Analyzer PRO</title>
<style>
body{background:#020617;color:#e5e7eb;font-family:Arial}
.box{max-width:720px;margin:auto;background:#020617;padding:20px;border-radius:12px}
input,button{width:100%;padding:10px;margin:6px 0;border-radius:8px;border:none}
button{background:#22c55e;font-size:16px;cursor:pointer}
h2,h3{text-align:center}
.res{background:#020617;margin-top:15px;padding:15px;border-radius:10px}
.badge{padding:4px 8px;border-radius:6px;background:#1e293b}
hr{border:1px solid #1e293b}
</style>
</head>

<body>
<div class="box">
<h2>⚽ AI Match Analyzer PRO</h2>

<input id="home" placeholder="اسم الفريق المضيف">
<input id="away" placeholder="اسم الفريق الضيف">

<h3>📊 المضيف (آخر 5 مباريات على أرضه)</h3>
<input id="hGoals" placeholder="أهداف مسجلة (مثال: 2-1,3-0,1-1,4-2,2-0)">
<input id="hCon" placeholder="أهداف مستقبلة (نفس الشكل)">
<input id="hFH" placeholder="الشوط الأول له/عليه (مثال: 1-0,0-0,1-1,2-0,1-0)">

<h3>📊 الضيف (آخر 5 مباريات خارج أرضه)</h3>
<input id="aGoals" placeholder="أهداف مسجلة (مثال: 1-2,0-1,2-2,1-3,0-2)">
<input id="aCon" placeholder="أهداف مستقبلة (نفس الشكل)">
<input id="aFH" placeholder="الشوط الأول له/عليه (مثال: 0-1,0-0,1-1,0-2,0-1)">

<button onclick="analyze()">تحليل المباراة</button>

<div id="out" class="res"></div>
</div>

<script>
function parseTable(txt){
 let f=0,c=0;
 let arr=txt.split(',');
 arr.forEach(x=>{
  let p=x.trim().split('-');
  f+=parseInt(p[0]); c+=parseInt(p[1]);
 });
 return {for:f/arr.length, against:c/arr.length};
}

function analyze(){
let h=home.value, a=away.value;

let hG=parseTable(hGoals.value);
let hFH=parseTable(hFH.value);
let aG=parseTable(aGoals.value);
let aFH=parseTable(aFH.value);

let homeIndex = hG.for + hFH.for - hG.against;
let awayIndex = aG.for + aFH.for - aG.against;

let pHome=Math.max(20,Math.min(65,45+(homeIndex-awayIndex)*10));
let pAway=Math.max(15,100-pHome-20);
let pDraw=100-pHome-pAway;

let over=(hG.for+aG.for)>2.6?"Over 2.5 ✅":"Under 2.5 ❌";
let gg=(hG.for>0.8 && aG.for>0.8)?"GG ✅":"NO GG ❌";
let fh=(hFH.for>aFH.for)?h:(hFH.for<aFH.for)?a:"تعادل";

out.innerHTML=`
<h3>${h} 🆚 ${a}</h3>

<p><span class="badge">1</span> ${h}: ${pHome.toFixed(1)}%</p>
<p><span class="badge">X</span> تعادل: ${pDraw.toFixed(1)}%</p>
<p><span class="badge">2</span> ${a}: ${pAway.toFixed(1)}%</p>

<hr>
<p>⚽ ${over}</p>
<p>🎯 ${gg}</p>
<p>⏱️ الشوط الأول: ${fh}</p>
<p>🧮 نتيجة محتملة: ${Math.round(hG.for)} - ${Math.round(aG.for)}</p>

<hr>
<p>🧠 التفسير:
${h} يسجل بمعدل ${hG.for.toFixed(2)} داخل أرضه،
بينما ${a} خارج أرضه يسجل ${aG.for.toFixed(2)}،
ومقارنة الشوط الأول تعطي أفضلية واضحة.</p>
`;
}
</script>
</body>
</html
