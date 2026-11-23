 <!DOCTYPE html>
<html lang="id">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Kuis PKn</title>
<style>
:root{--bg:#e8f4fb;--card:#fff8e7;--fg:#1c1c3a;--brd:#d9a441;--ok:#2e9e4f;--no:#d63a3a;--btn:#fff;--head:#c62828;--hl:#ffd54f;
box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#0f1b2a;--card:#2a2416;--fg:#f3ecd9;--brd:#a87b22;--btn:#3a3322;--head:#b71c1c}}
*{box-sizing:border-box}
html,body{margin:0;background:var(--bg);color:var(--fg);font-family:system-ui,-apple-system,"Segoe UI",sans-serif}
.wrap{max-width:560px;margin:0 auto;padding:12px}
.hero{background:var(--head);color:#fff;border-radius:18px;padding:16px;text-align:center;border:3px solid var(--hl)}
.hero h1{margin:0;font-size:28px;letter-spacing:.5px}
.hero p{margin:6px 0 0;font-size:14px}
.bar{display:flex;justify-content:space-between;gap:8px;margin:12px 0;font-weight:700;font-size:14px}
.bar span{background:var(--card);border:2px solid var(--brd);border-radius:12px;padding:6px 10px;flex:1;text-align:center}
.q{background:var(--card);border:2px solid var(--brd);border-radius:16px;padding:12px;margin-bottom:12px}
.qh{display:flex;gap:10px;align-items:flex-start}
.n{flex:none;width:36px;height:36px;border-radius:50%;color:#fff;font-weight:800;display:grid;place-items:center;font-size:18px}
.qh p{margin:0;font-weight:700;line-height:1.35}
.opts{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:10px}
@media (max-width:380px){.opts{grid-template-columns:1fr}}
.o{display:flex;gap:8px;align-items:center;text-align:left;background:var(--btn);color:var(--fg);border:2px solid var(--brd);border-radius:12px;padding:10px;font:inherit;font-size:14px;cursor:pointer}
.o b{flex:none;width:24px;height:24px;border-radius:50%;background:var(--hl);color:#222;display:grid;place-items:center;font-size:13px}
.o.ok{background:var(--ok);border-color:var(--ok);color:#fff}
.o.no{background:var(--no);border-color:var(--no);color:#fff}
.o:disabled{cursor:default}
.fb{margin-top:8px;font-size:13px;display:none;line-height:1.4}
.fb.on{display:block}
.act{display:flex;gap:8px;margin:14px 0}
.act button{flex:1;padding:13px;border:0;border-radius:12px;font:inherit;font-weight:700;font-size:15px;cursor:pointer}
#key{background:#1976d2;color:#fff}#rst{background:#ef6c00;color:#fff}
#res{display:none;text-align:center;background:var(--hl);color:#222;border-radius:16px;padding:14px;font-weight:700;margin-top:10px}
#res.on{display:block}
</style>
</head>
<body>
<div class="wrap">
  <div class="hero"><h1>Kuis PKn</h1><p>Pancasila, Simbol, dan Hak serta Kewajiban<br>Pilihlah jawaban yang paling tepat!</p></div>
  <div class="bar"><span id="s1">Terjawab 0/5</span><span id="s2">Benar 0</span></div>
  <div id="list"></div>
  <div id="res"></div>
  <div class="act"><button id="key">Lihat Kunci Jawaban</button><button id="rst">Ulangi</button></div>
</div>
<script>
const Q=[
{q:"Apa kedudukan Pancasila bagi bangsa Indonesia?",o:["Lambang negara","Dasar negara","Peraturan daerah","Semboyan bangsa"],a:1,e:"Pancasila adalah dasar negara Indonesia.",c:"#e53935"},
{q:"Simbol yang melambangkan sila pertama Pancasila adalah ....",o:["Rantai emas","Bintang","Pohon beringin","Kepala banteng"],a:1,e:"Bintang melambangkan sila pertama, Ketuhanan Yang Maha Esa.",c:"#fb8c00"},
{q:"Rantai emas merupakan simbol dari sila Pancasila yang ke ....",o:["Pertama","Kedua","Ketiga","Keempat"],a:1,e:"Rantai emas melambangkan sila kedua, Kemanusiaan yang Adil dan Beradab.",c:"#43a047"},
{q:"Manakah contoh kewajiban warga negara?",o:["Mendapatkan pendidikan","Mendapatkan kesehatan","Menjaga persatuan dan menaati hukum","Mendapatkan perlindungan"],a:2,e:"Menjaga persatuan dan menaati hukum adalah kewajiban; yang lain adalah hak.",c:"#1e88e5"},
{q:"Pohon beringin melambangkan sila ke ....",o:["Satu","Dua","Tiga","Empat"],a:2,e:"Pohon beringin melambangkan sila ketiga, Persatuan Indonesia.",c:"#8e24aa"}
];
const L="ABCD",st=Q.map(()=>null);
const list=document.getElementById("list");
function render(){
 list.innerHTML="";
 Q.forEach((x,i)=>{
  const d=document.createElement("div");d.className="q";
  d.innerHTML='<div class="qh"><div class="n" style="background:'+x.c+'">'+(i+1)+'</div><p>'+x.q+'</p></div><div class="opts"></div><div class="fb"></div>';
  const ob=d.querySelector(".opts");
  x.o.forEach((t,j)=>{
   const b=document.createElement("button");b.className="o";b.innerHTML="<b>"+L[j]+"</b><span>"+t+"</span>";
   b.onclick=()=>pick(i,j);ob.appendChild(b);
  });
  list.appendChild(d);
 });
}
function paint(i,reveal){
 const d=list.children[i],bs=d.querySelectorAll(".o"),fb=d.querySelector(".fb"),p=st[i];
 bs.forEach((b,j)=>{
  b.classList.remove("ok","no");
  if(p!==null||reveal){b.disabled=true;if(j===Q[i].a)b.classList.add("ok");else if(j===p)b.classList.add("no");}
 });
 if(p!==null||reveal){
  fb.classList.add("on");
  fb.textContent=(p===null?"Jawaban: ":(p===Q[i].a?"Benar! ":"Kurang tepat. Jawaban: "))+L[Q[i].a]+". "+Q[i].o[Q[i].a]+". "+Q[i].e;
 }
}
function pick(i,j){
 if(st[i]!==null)return;
 st[i]=j;paint(i,false);stat();
}
function stat(){
 const done=st.filter(v=>v!==null).length,ok=st.filter((v,i)=>v===Q[i].a).length;
 s1.textContent="Terjawab "+done+"/5";s2.textContent="Benar "+ok;
 const r=document.getElementById("res");
 if(done===5){r.classList.add("on");r.textContent="Skor kamu: "+(ok*20)+" ("+ok+" dari 5 benar). "+(ok===5?"Sempurna, hebat!":ok>=3?"Bagus, terus belajar!":"Ayo belajar lagi, kamu pasti bisa!");}
 else r.classList.remove("on");
}
document.getElementById("key").onclick=()=>{Q.forEach((_,i)=>paint(i,true));};
document.getElementById("rst").onclick=()=>{st.fill(null);render();stat();window.scrollTo(0,0);};
render();stat();
</script>
</body>
</html>
