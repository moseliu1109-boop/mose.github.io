<!DOCTYPE html>  
<html lang="zh-CN">  
<head>  
<meta charset="UTF-8">  
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">  
<title>**白绿总格计算器**</title>  
<style>  
*{margin:0;padding:0;box-sizing:border-box}  
body{font-family:'PingFang SC',Arial,sans-serif;max-width:500px;margin:0 auto;padding:8px;background:#1a1a2e;color:#eee;font-size:13px}  
h2{text-align:center;color:#e0b0ff;margin:8px 0;font-size:16px}  
.section{background:#16213e;padding:10px;margin:6px 0;border-radius:8px}  
.section h3{color:#e0b0ff;font-size:13px;margin:0 0 6px;border-bottom:1px solid #333;padding-bottom:4px}  
.row{display:flex;gap:6px;margin:3px 0;align-items:center}  
.row label{width:75px;text-align:right;font-size:12px;flex-shrink:0}  
.row input{flex:1;padding:5px;border:1px solid #444;border-radius:4px;background:#0f0f23;color:#fff;font-size:12px;min-width:0}  
button{width:100%;padding:10px;background:#e0b0ff;color:#1a1a2e;border:none;border-radius:6px;font-size:15px;font-weight:bold;cursor:pointer;margin:8px 0}  
.result{background:#16213e;padding:10px;border-radius:6px;white-space:pre-wrap;font-size:11px;line-height:1.4;max-height:70vh;overflow:auto;font-family:monospace}  
.green{color:#7cfc00}.red{color:#ff4444}.gold{color:#ffd700}.purple{color:#da70d6}  
.pwd-box{text-align:center;padding:50px 20px}  
.pwd-box input{padding:10px;font-size:16px;border-radius:6px;border:1px solid #e0b0ff;background:#0f0f23;color:#fff;width:200px;text-align:center}  
.pwd-box button{margin-top:10px;width:200px}  
</style>  
</head>  
<body>  
<div id="pwdPage" class="pwd-box">  
<h2>**请输入密码**</h2>  
<input type="password" id="pwdInput" placeholder="**请输入密码**">  
<button onclick="checkPwd()">**验证**</button>  
<p id="pwdError" style="color:#ff4444;margin-top:10px;font-size:12px"></p>  
</div>  
<div id="mainPage" style="display:none">  
<h2>**白绿总格计算器**</h2>  
<div class="section"><h3>**第一回合**</h3>  
<div class="row"><label>**总件数**I</label><input id="I"></div>  
<div class="row"><label>**白绿均格**V</label><input id="V"></div></div>  
<div class="section"><h3>**第二回合**</h3>  
<div class="row"><label>**金均格**A</label><input id="A"></div>  
<div class="row"><label>**蓝件数**Q</label><input id="Q"></div></div>  
<div class="section"><h3>**第三回合**</h3>  
<div class="row"><label>**紫均格**B</label><input id="B"></div></div>  
<div class="section"><h3>**第四回合**</h3>  
<div class="row"><label>**蓝均格**W</label><input id="W"></div></div>  
<div class="section"><h3>**总格**</h3>  
<div class="row"><label>**全总格**T</label><input id="T"></div>  
<div class="row"><label>**白绿总格**G</label><input id="G"></div>  
<div class="row"><label>**蓝总格**L</label><input id="L"></div>  
<div class="row"><label>**紫总格**Z</label><input id="Z"></div>  
<div class="row"><label>**金总格**J</label><input id="J"></div></div>  
<div class="section"><h3>**件数**</h3>  
<div class="row"><label>**白绿件数**P</label><input id="P"></div>  
<div class="row"><label>**紫件数**Y</label><input id="Y"></div>  
<div class="row"><label>**金件数**X</label><input id="X"></div></div>  
<div class="section"><h3>**均价**</h3>  
<div class="row"><label>**白绿均价**</label><input id="AP"></div>  
<div class="row"><label>**蓝均价**</label><input id="AQ"></div>  
<div class="row"><label>**紫均价**</label><input id="AR"></div>  
<div class="row"><label>**金均价**</label><input id="AU"></div></div>  
<div class="section"><h3>**最少下限**</h3>  
<div class="row"><label>**白绿件**≥</label><input id="PL"></div>  
<div class="row"><label>**白绿格**≥</label><input id="GL"></div>  
<div class="row"><label>**蓝件**≥</label><input id="QL"></div>  
<div class="row"><label>**蓝格**≥</label><input id="LL"></div>  
<div class="row"><label>**紫件**≥</label><input id="YL"></div>  
<div class="row"><label>**紫格**≥</label><input id="ZL"></div>  
<div class="row"><label>**金件**≥</label><input id="XL"></div>  
<div class="row"><label>**金格**≥</label><input id="JL"></div>  
<div class="row"><label>**红件**≥</label><input id="SL"></div>  
<div class="row"><label>**红格**≥</label><input id="HL"></div></div>  
<button onclick="calc()">**开始计算**</button>  
<div class="result" id="out"></div>  
</div>  
<script>  
const PWD="mose1314";  
function checkPwd(){if(document.getElementById('pwdInput').value===PWD){document.getElementById('pwdPage').style.display='none';document.getElementById('mainPage').style.display='block'}else{document.getElementById('pwdError').textContent='**密码错误**'}}  
function gv(id){let v=document.getElementById(id).value.trim();return v===""?null:parseFloat(v)}  
function trunc(v,d=2){let s=v.toFixed(10);let i=s.indexOf('.');return parseFloat(s.substring(0,i+d+1))}  
function fmt(v){return Number.isInteger(v)?v:v}  
function lv(arr){arr=**[**...new Set(arr)**]**.sort((a,b)=>a-b);if(arr.length===1)return fmt(arr**[**0**]**);return'**[**'+arr.map(fmt).join(',')+'**]**'}  
function rng(arr){arr=**[**...new Set(arr)**]**.sort((a,b)=>a-b);if(arr.length===1)return fmt(arr**[**0**]**);return fmt(arr**[**0**]**)+'~'+fmt(arr**[**arr.length-1**]**)}  
function rd(t){return'<span class="red">'+t+'</span>'}  
function gn(t){return'<span class="green">'+t+'</span>'}  
function gd(t){return'<span class="gold">'+t+'</span>'}  
function pp(t){return'<span class="purple">'+t+'</span>'}  
  
function jUpper(x){if(x===1)return 18;if(x===2)return 34;return 45}  
function zUpper(y){if(y===1)return 12;if(y===2)return 22;if(y===3)return 31;if(y===4)return 40;if(y===5)return 49;return 70}  
function lUpper(q){if(q===1)return 20;if(q===2)return 36;if(q===3)return 51;return 70}  
function gUpper(p){if(p===1)return 12;if(p===2)return 21;if(p===3)return 30;if(p===4)return 38;if(p===5)return 42;return 55}  
function hUpper(s){if(s===1)return 16;if(s===2)return 32;if(s===3)return 48;return 50}  
  
function priceY(price,ub){if(price===null)return null;let lo=Math.floor(price),hi=lo+1,res=**[]**;for(let y=1;y<=ub;y++){for(let r=lo*y;r<=hi*y;r++){if(trunc(r/y,2)===trunc(price,2)){res.push(y);break}}}return**[**...new Set(res)**]**.sort((a,b)=>a-b)}  
function eg(avg,ub,upperFn){if(avg===null)return**[]**;let lo=Math.floor(avg),hi=lo+1,pool=**[]**;for(let n=1;n<=ub;n++){if(avg*n>upperFn(n))break;let loB=Math.max(Math.ceil(avg),lo*n),hiB=Math.min(upperFn(n),hi*n);for(let g=loB;g<=hiB;g++){if(g<n)continue;if(trunc(g/n,2)===trunc(avg,2)){pool.push(**[**n,g**]**);break}}}return pool}  
  
function calc(){  
let I=gv('I'),V=gv('V'),A=gv('A'),Qv=gv('Q'),B=gv('B'),W=gv('W'),T=gv('T'),G=gv('G'),L=gv('L'),Z=gv('Z'),J=gv('J'),P=gv('P'),Y=gv('Y'),X=gv('X'),AP=gv('AP'),AQ=gv('AQ'),AR=gv('AR'),AU=gv('AU'),PL=gv('PL'),GL=gv('GL'),QL=gv('QL'),LL=gv('LL'),YL=gv('YL'),ZL=gv('ZL'),XL=gv('XL'),JL=gv('JL'),SL=gv('SL'),HL=gv('HL');  
  
if(A===0||X===0||J===0){A=0;X=0;J=0}  
if(B===0||Y===0||Z===0){B=0;Y=0;Z=0}  
if(V===0||P===0||G===0){V=0;P=0;G=0}  
if(W===0||Qv===0||L===0){W=0;Qv=0;L=0}  
  
let gY=priceY(AU,12),zY=priceY(AR,20);  
let jPool=**[]**;  
if(J!==null){jPool=**[[**X!==null?X:Math.round(J/(A||1)),Math.round(J)**]]**}  
else if(X!==null&&A!==null){jPool=**[[**Math.round(X),Math.round(X*A)**]]**}  
else if(A!==null){jPool=eg(A,12,jUpper);if(gY)jPool=jPool.filter(p=>gY.includes(p**[**0**]**))}  
let zPool=**[]**;  
if(Z!==null){zPool=**[[**Y!==null?Y:Math.round(Z/(B||1)),Math.round(Z)**]]**}  
else if(Y!==null){if(B!==null)zPool=**[[**Math.round(Y),Math.round(Y*B)**]]**;else zPool=**[[**Math.round(Y),null**]]**}  
else if(B!==null){zPool=eg(B,20,zUpper);if(zY)zPool=zPool.filter(p=>zY.includes(p**[**0**]**))}  
else if(zY!==null){zY.forEach(y=>zPool.push(**[**y,null**]**))}  
let GC=**[]**;  
if(G!==null)GC=**[**Math.round(G)**]**;  
else if(P!==null&&V!==null)GC=**[**Math.round(P*V)**]**;  
else if(V!==null)GC=eg(V,40,gUpper).map(p=>p**[**1**]**);  
else GC=Array.from({length:51},(_,i)=>i);  
let LC=**[]**;  
if(L!==null)LC=**[**Math.round(L)**]**;  
else if(Qv!==null&&W!==null)LC=**[**Math.round(Qv*W)**]**;  
else LC=Array.from({length:56},(_,i)=>i);  
let TC=null;  
if(T!==null)TC=**[**Math.round(T)**]**;  
let PC=**[]**;  
if(P!==null)PC=**[**Math.round(P)**]**;  
else if(V!==null&&G!==null)PC=**[**Math.round(G/V)**]**;  
else if(V!==null)PC=eg(V,40,gUpper).map(p=>p**[**0**]**);  
else PC=Array.from({length:41},(_,i)=>i);  
let QC=**[]**;  
if(Qv!==null)QC=**[**Math.round(Qv)**]**;  
else if(W!==null&&L!==null)QC=**[**Math.round(L/W)**]**;  
else QC=Array.from({length:36},(_,i)=>i);  
let SC=Array.from({length:9},(_,i)=>i);  
let Tup=Math.min(3.5*(I||0),180);  
let out='';  
out+=`**白绿件数候选**: ${lv(**[**...new Set(PC)**]**)}\n`;  
out+=`**白绿总格候选**: ${lv(**[**...new Set(GC)**]**)}\n`;  
out+=`**金件数候选**: ${gd(lv(**[**...new Set(jPool.filter(p=>p**[**0**]**!==null).map(p=>p**[**0**]**))**]**))}\n`;  
out+=`**紫件数候选**: ${pp(lv(**[**...new Set(zPool.filter(p=>p**[**0**]**!==null).map(p=>p**[**0**]**))**]**))}\n`;  
out+=`**金总格候选**: ${gd(lv(jPool.map(p=>p**[**1**]**)))}\n`;  
out+=`**紫总格候选**: ${pp(lv(zPool.filter(p=>p**[**1**]**!==null).map(p=>p**[**1**]**)))}\n\n`;  
let all=**[]**;  
for(let g of GC){  
if(GL!==null&&g<GL)continue;  
let P**实际**;  
if(P!==null)P**实际**=**[**Math.round(P)**]**;  
else if(V!==null)P**实际**=**[**Math.round(g/V)**]**;  
else{let pLo=Math.round(g/7);pLo=pLo<0?0:pLo;let pHi=g>0?g:0;P**实际**=PC.filter(p=>p>=pLo&&p<=pHi);if(g===0)P**实际**=**[**0**]**}  
for(let l of LC){  
if(LL!==null&&l<LL)continue;  
let Q**実際**;  
if(Qv!==null)Q**実際**=**[**Math.round(Qv)**]**;  
else if(W!==null)Q**実際**=**[**Math.round(l/W)**]**;  
else{let qLo=Math.round(l/18);qLo=qLo<0?0:qLo;let qHi=l>0?l:0;Q**実際**=QC.filter(q=>q>=qLo&&q<=qHi);if(l===0)Q**実際**=**[**0**]**}  
for(let**[**x,j**]**of jPool){  
if(x!==null&&j<x)continue;  
if(x!==null&&j>jUpper(x))continue;  
if(j===0&&x!==0)continue;  
if(j>=1&&(x===null||x<1))continue;  
if(JL!==null&&j<JL)continue;  
if(XL!==null&&x!==null&&x<XL)continue;  
for(let**[**y,z**]**of zPool){  
if(z===null)continue;  
if(y!==null&&z<y)continue;  
if(y!==null&&z>zUpper(y))continue;  
if(z===0&&y!==0)continue;  
if(z>=1&&(y===null||y<1))continue;  
if(ZL!==null&&z<ZL)continue;  
if(YL!==null&&y!==null&&y<YL)continue;  
let xy=(x||0)+(y||0);  
if(xy>=I)continue;  
let rem=I-xy;  
for(let p of P**实际**){  
if(g===0&&p!==0)continue;  
if(g>=1&&p<1)continue;  
if(g>gUpper(p))continue;  
if(g<p)continue;  
if(PL!==null&&p<PL)continue;  
for(let q of Q**実際**){  
if(l===0&&q!==0)continue;  
if(l>=1&&q<1)continue;  
if(l>lUpper(q))continue;  
if(l<q)continue;  
if(QL!==null&&q<QL)continue;  
let s=Math.round(rem-p-q);  
if(!SC.includes(s))continue;  
if(SL!==null&&s<SL)continue;  
let sum=g+l+j+z;  
if(TC!==null){for(let t of TC){let h=t-sum;if(h>=0&&h<=50&&h>=s&&h<=hUpper(s)){if(h===0&&s!==0)continue;if(h>=1&&s<1)continue;if(HL!==null&&h<HL)continue;if(t>=40&&t<=Tup)all.push(**[**x,y,p,q,s,g,l,j,z,t,h**]**)}}}  
else{for(let h=s;h<=hUpper(s);h++){if(h<0||h>50)continue;if(h===0&&s!==0)continue;if(h>=1&&s<1)continue;if(HL!==null&&h<HL)continue;let t=sum+h;if(t>=40&&t<=Tup)all.push(**[**x,y,p,q,s,g,l,j,z,t,h**]**)}}}}}}}}  
if(all.length===0){out+='**无有效组合。**';document.getElementById('out').innerHTML=out;return}  
let effX=**[**...new Set(all.map(v=>v**[**0**]**).filter(v=>v!==null))**]**.sort((a,b)=>a-b);  
let effY=**[**...new Set(all.map(v=>v**[**1**]**).filter(v=>v!==null))**]**.sort((a,b)=>a-b);  
let effJ=**[**...new Set(all.map(v=>v**[**7**]**))**]**.sort((a,b)=>a-b);  
let effZ=**[**...new Set(all.map(v=>v**[**8**]**))**]**.sort((a,b)=>a-b);  
let effP=**[**...new Set(all.map(v=>v**[**2**]**))**]**.sort((a,b)=>a-b);  
let effG=**[**...new Set(all.map(v=>v**[**5**]**))**]**.sort((a,b)=>a-b);  
out+=`**有效白绿件数**: ${lv(effP)}\n`;  
out+=`**有效白绿总格**: ${lv(effG)}\n`;  
out+=`**有效金件数**: ${gd(lv(effX))}\n`;  
out+=`**有效紫件数**: ${pp(lv(effY))}\n`;  
out+=`**有效金总格**: ${gd(lv(effJ))}\n`;  
out+=`**有效紫总格**: ${pp(lv(effZ))}\n\n`;  
let sVals=new Set(all.map(v=>v**[**4**]**)),hVals=new Set(all.map(v=>v**[**10**]**));  
let byS=sVals.size<hVals.size;  
if(byS){  
let grp={};  
all.forEach(v=>{let s=v**[**4**]**;if(!grp**[**s**]**)grp**[**s**]**=**[]**;grp**[**s**]**.push(v)});  
Object.keys(grp).sort((a,b)=>a-b).forEach(s=>{  
if(out.includes('---'))out+='----------------------------------------\n';  
let items=grp**[**s**]**;  
let pairs=**[**...new Set(items.map(v=>`${v**[**1**]**},${v**[**0**]**},${v**[**8**]**},${v**[**7**]**},${v**[**10**]**}`))**]**.map(s=>s.split(',').map(Number)).sort((a,b)=>a**[**0**]**-b**[**0**]**);  
let yArr=pairs.map(p=>p**[**0**]**),xArr=pairs.map(p=>p**[**1**]**),zArr=pairs.map(p=>p**[**2**]**),jArr=pairs.map(p=>p**[**3**]**),hArr=pairs.map(p=>p**[**4**]**);  
let pVals=**[**...new Set(items.map(v=>v**[**2**]**))**]**.sort((a,b)=>a-b);  
let qVals=**[**...new Set(items.map(v=>v**[**3**]**))**]**.sort((a,b)=>a-b);  
let gVals=**[**...new Set(items.map(v=>v**[**5**]**))**]**.sort((a,b)=>a-b);  
let lVals=**[**...new Set(items.map(v=>v**[**6**]**))**]**.sort((a,b)=>a-b);  
let totals=**[**...new Set(items.filter(v=>v**[**0**]**!==null&&v**[**1**]**!==null).map(v=>Math.round(v**[**1**]***0.5)+Math.round(v**[**0**]***3)+Math.round(s*18)))**]**.sort((a,b)=>a-b);  
out+=`**白绿** ${lv(pVals)}  **蓝** ${lv(qVals)}  **紫** ${pp(lv(yArr))}  **金** ${gd(lv(xArr))} -> **红** ${rd(s>=1?s:'')}\n`;  
out+=`**总共** ${gn(lv(totals))} **万**\n\n`;  
if(hArr.length>=3){out+=`G ${lv(gVals)}  L ${lv(lVals)}  Z ${pp(lv(zArr))}  J ${gd(lv(jArr))} -> H ${rd(rng(hArr))}\n`;let tAll=**[**...new Set(items.map(v=>Math.round(v**[**8**]***0.3)+Math.round(v**[**7**]***0.7)+Math.round(v**[**10**]***5)))**]**.sort((a,b)=>a-b);out+=`Total ${gn(rng(tAll))} **万**\n`}  
else{for(let i=0;i<pairs.length;i++){out+=`G ${lv(gVals)}  L ${lv(lVals)}  Z ${pp(zArr**[**i**]**)}  J ${gd(jArr**[**i**]**)} -> H ${rd(hArr**[**i**]**>=1?hArr**[**i**]**:'')}\n`;let ttl=Math.round(zArr**[**i**]***0.3)+Math.round(jArr**[**i**]***0.7)+Math.round(hArr**[**i**]***5);out+=`Total ${gn(ttl)} **万**\n`}}  
});  
}else{  
let grp={};  
all.forEach(v=>{let h=v**[**10**]**;if(!grp**[**h**]**)grp**[**h**]**=**[]**;grp**[**h**]**.push(v)});  
Object.keys(grp).sort((a,b)=>a-b).forEach(h=>{  
if(out.includes('---'))out+='----------------------------------------\n';  
let items=grp**[**h**]**;  
let sGrp={};  
items.forEach(v=>{let s=v**[**4**]**;if(!sGrp**[**s**]**)sGrp**[**s**]**=**[]**;sGrp**[**s**]**.push(v)});  
Object.keys(sGrp).sort((a,b)=>a-b).forEach(s=>{  
let sv=sGrp**[**s**]**;  
let pairs=**[**...new Set(sv.map(v=>`${v**[**1**]**},${v**[**0**]**},${v**[**8**]**},${v**[**7**]**}`))**]**.map(s=>s.split(',').map(Number)).sort((a,b)=>a**[**0**]**-b**[**0**]**);  
let yArr=pairs.map(p=>p**[**0**]**),xArr=pairs.map(p=>p**[**1**]**);  
let pVals=**[**...new Set(sv.map(v=>v**[**2**]**))**]**.sort((a,b)=>a-b);  
let qVals=**[**...new Set(sv.map(v=>v**[**3**]**))**]**.sort((a,b)=>a-b);  
let totals=**[**...new Set(sv.filter(v=>v**[**0**]**!==null&&v**[**1**]**!==null).map(v=>Math.round(v**[**1**]***0.5)+Math.round(v**[**0**]***3)+Math.round(s*18)))**]**.sort((a,b)=>a-b);  
out+=`**白绿** ${lv(pVals)}  **蓝** ${lv(qVals)}  **紫** ${pp(lv(yArr))}  **金** ${gd(lv(xArr))} -> **红** ${rd(s>=1?s:'')}\n`;  
out+=`**总共** ${gn(lv(totals))} **万**\n`;  
});  
out+='\n';  
let pairs=**[**...new Set(items.map(v=>`${v**[**1**]**},${v**[**0**]**},${v**[**8**]**},${v**[**7**]**}`))**]**.map(s=>s.split(',').map(Number)).sort((a,b)=>a**[**0**]**-b**[**0**]**);  
let zArr=pairs.map(p=>p**[**2**]**),jArr=pairs.map(p=>p**[**3**]**);  
let gVals=**[**...new Set(items.map(v=>v**[**5**]**))**]**.sort((a,b)=>a-b);  
let lVals=**[**...new Set(items.map(v=>v**[**6**]**))**]**.sort((a,b)=>a-b);  
let ttlVals=**[**...new Set(items.map(v=>Math.round(v**[**8**]***0.3)+Math.round(v**[**7**]***0.7)+Math.round(h*5)))**]**.sort((a,b)=>a-b);  
out+=`G ${lv(gVals)}  L ${lv(lVals)}  Z ${pp(lv(zArr))}  J ${gd(lv(jArr))} -> H ${rd(h>=1?h:'')}\n`;  
out+=`Total ${gn(lv(ttlVals))} **万**\n`;  
});  
}  
if(all.length>0){  
out+='\n';  
let P2S={},P2H={};  
all.forEach(v=>{let p=v**[**2**]**,s=v**[**4**]**,h=v**[**10**]**;if(!P2S**[**p**]**){P2S**[**p**]**=new Set();P2H**[**p**]**=new Set()}P2S**[**p**]**.add(s);P2H**[**p**]**.add(h)});  
let lockPS=Object.values(P2S).filter(s=>s.size===1).length;  
let lockPH=Object.values(P2H).filter(h=>h.size===1).length;  
let safePS4=Object.values(P2S).filter(s=>Math.min(...s)>=4).length;  
let safePH15=Object.values(P2H).filter(h=>Math.min(...h)>=15).length;  
let totalP=Object.keys(P2S).length;  
out+=`**──拿**P**──**\n**可锁定唯一**S: ${Math.round(lockPS/totalP*100)}%\n**可锁定唯一**H: ${Math.round(lockPH/totalP*100)}%\n**可锁定**S≥4: ${Math.round(safePS4/totalP*100)}%\n**可锁定**H≥15: ${Math.round(safePH15/totalP*100)}%\n\n`;  
  
let Y2S={},Y2H={};  
all.forEach(v=>{let y=v**[**1**]**,s=v**[**4**]**,h=v**[**10**]**;if(!Y2S**[**y**]**){Y2S**[**y**]**=new Set();Y2H**[**y**]**=new Set()}Y2S**[**y**]**.add(s);Y2H**[**y**]**.add(h)});  
let lockYS=Object.values(Y2S).filter(s=>s.size===1).length;  
let lockYH=Object.values(Y2H).filter(h=>h.size===1).length;  
let safeYS4=Object.values(Y2S).filter(s=>Math.min(...s)>=4).length;  
let safeYH15=Object.values(Y2H).filter(h=>Math.min(...h)>=15).length;  
let totalY=Object.keys(Y2S).length;  
out+=`**──拿**Y**──**\n**可锁定唯一**S: ${Math.round(lockYS/totalY*100)}%\n**可锁定唯一**H: ${Math.round(lockYH/totalY*100)}%\n**可锁定**S≥4: ${Math.round(safeYS4/totalY*100)}%\n**可锁定**H≥15: ${Math.round(safeYH15/totalY*100)}%\n\n`;  
  
let T2S={},T2H={};  
all.forEach(v=>{let t=v**[**9**]**,s=v**[**4**]**,h=v**[**10**]**;if(!T2S**[**t**]**){T2S**[**t**]**=new Set();T2H**[**t**]**=new Set()}T2S**[**t**]**.add(s);T2H**[**t**]**.add(h)});  
let lockTS=Object.values(T2S).filter(s=>s.size===1).length;  
let lockTH=Object.values(T2H).filter(h=>h.size===1).length;  
let safeTS4=Object.values(T2S).filter(s=>Math.min(...s)>=4).length;  
let safeTH15=Object.values(T2H).filter(h=>Math.min(...h)>=15).length;  
let totalT=Object.keys(T2S).length;  
out+=`**──拿**T**──**\n**可锁定唯一**S: ${Math.round(lockTS/totalT*100)}%\n**可锁定唯一**H: ${Math.round(lockTH/totalT*100)}%\n**可锁定**S≥4: ${Math.round(safeTS4/totalT*100)}%\n**可锁定**H≥15: ${Math.round(safeTH15/totalT*100)}%\n\n`;  
  
let X2S={},X2H={};  
all.forEach(v=>{let x=v**[**0**]**,s=v**[**4**]**,h=v**[**10**]**;if(!X2S**[**x**]**){X2S**[**x**]**=new Set();X2H**[**x**]**=new Set()}X2S**[**x**]**.add(s);X2H**[**x**]**.add(h)});  
let lockXS=Object.values(X2S).filter(s=>s.size===1).length;  
let lockXH=Object.values(X2H).filter(h=>h.size===1).length;  
let safeXS4=Object.values(X2S).filter(s=>Math.min(...s)>=4).length;  
let safeXH15=Object.values(X2H).filter(h=>Math.min(...h)>=15).length;  
let totalX=Object.keys(X2S).length;  
out+=`**──拿**X**──**\n**可锁定唯一**S: ${Math.round(lockXS/totalX*100)}%\n**可锁定唯一**H: ${Math.round(lockXH/totalX*100)}%\n**可锁定**S≥4: ${Math.round(safeXS4/totalX*100)}%\n**可锁定**H≥15: ${Math.round(safeXH15/totalX*100)}%\n`;  
}  
document.getElementById('out').innerHTML=out;  
}  
</script>  
</body>  
</html>  
