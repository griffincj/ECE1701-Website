---
layout: page
title: Instantaneous power
description: Interactive plot of voltage, current, and their product as the phase angle varies.
parent: Module 1 - Fundamentals
nav_order: 1
---

# Instantaneous power and the phase angle

<span class="label label-blue">2.12</span>
<span class="label label-blue">2.19</span>

Let's take a closer look at how instantaneous power is related to voltage and current, and how phase angle changes these calculations.

<div class="demo-widget">
<h2 class="sr-only">Interactive plot of voltage, current, and instantaneous power over two cycles, with a slider controlling the phase angle between voltage and current.</h2>
<div style="padding:1rem 0">
<svg width="100%" viewBox="0 0 680 320" role="img"><title>Instantaneous power waveform</title><desc>Voltage and current sinusoids plus their product, showing positive and negative power lobes.</desc>
<line x1="60" y1="170" x2="640" y2="170" stroke="var(--border-strong)" stroke-width="0.5"/>
<g id="pfill"></g>
<polyline id="pavg" fill="none" stroke="#534AB7" stroke-width="1.5" stroke-dasharray="6 4"/>
<polyline id="vw" fill="none" stroke="#185FA5" stroke-width="1.5"/>
<polyline id="iw" fill="none" stroke="#BA7517" stroke-width="1.5" stroke-dasharray="5 3"/>
<polyline id="pw" fill="none" stroke="#0F6E56" stroke-width="2"/>
<text class="ts" x="60" y="300">0</text>
<text class="ts" x="350" y="300" text-anchor="middle">one cycle</text>
<text class="ts" x="640" y="300" text-anchor="end">two cycles</text>
</svg>
<div style="display:flex;gap:20px;flex-wrap:wrap;font-size:13px;color:var(--text-secondary);margin:4px 0 16px">
<span><span style="display:inline-block;width:14px;height:2px;background:#185FA5;vertical-align:3px"></span> v(t)</span>
<span><span style="display:inline-block;width:14px;height:2px;background:#BA7517;vertical-align:3px"></span> i(t)</span>
<span><span style="display:inline-block;width:14px;height:2px;background:#0F6E56;vertical-align:3px"></span> p(t) = v&middot;i</span>
<span><span style="display:inline-block;width:14px;height:2px;background:#534AB7;vertical-align:3px"></span> average = P</span>
</div>
<div style="display:flex;align-items:center;gap:12px;margin:0 0 1.25rem">
<label for="th" style="font-size:14px;color:var(--text-secondary);white-space:nowrap">Phase angle &theta;</label>
<input type="range" id="th" min="-90" max="90" step="1" value="0" style="flex:1"/>
<span id="tho" style="font-size:14px;font-weight:500;min-width:52px;text-align:right">60&deg;</span>
</div>
<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(120px,1fr));gap:12px">
<div style="background:var(--surface-1);border-radius:8px;padding:1rem"><p style="font-size:13px;color:var(--text-secondary);margin:0 0 4px">Power factor</p><p id="m1" style="font-size:24px;font-weight:500;margin:0">0.50</p></div>
<div style="background:var(--surface-1);border-radius:8px;padding:1rem"><p style="font-size:13px;color:var(--text-secondary);margin:0 0 4px">Real power P</p><p id="m2" style="font-size:24px;font-weight:500;margin:0">188 W</p></div>
<div style="background:var(--surface-1);border-radius:8px;padding:1rem"><p style="font-size:13px;color:var(--text-secondary);margin:0 0 4px">Reactive Q</p><p id="m3" style="font-size:24px;font-weight:500;margin:0">325 var</p></div>
<div style="background:var(--surface-1);border-radius:8px;padding:1rem"><p style="font-size:13px;color:var(--text-secondary);margin:0 0 4px">Returned energy</p><p id="m4" style="font-size:24px;font-weight:500;margin:0">15%</p></div>
</div>
</div>
<script>
(function(){
var X0=60,X1=640,Y0=170,N=400,S=375,VI=750,PS=0.155,AMP=58;
var th=document.getElementById('th');
function px(k){return X0+(X1-X0)*k/N;}
function draw(){
  var d=+th.value, r=d*Math.PI/180, pts=[],vp=[],ip=[],pv=[];
  for(var k=0;k<=N;k++){
    var x=4*Math.PI*k/N, v=Math.cos(x), i=Math.cos(x-r), p=VI*v*i;
    vp.push(px(k)+','+(Y0-AMP*v).toFixed(1));
    ip.push(px(k)+','+(Y0-AMP*i).toFixed(1));
    pv.push(p); pts.push(px(k));
  }
  document.getElementById('vw').setAttribute('points',vp.join(' '));
  document.getElementById('iw').setAttribute('points',ip.join(' '));
  document.getElementById('pw').setAttribute('points',pv.map(function(p,k){return pts[k]+','+(Y0-PS*p).toFixed(1);}).join(' '));
  var P=S*Math.cos(r),Q=S*Math.sin(r);
  document.getElementById('pavg').setAttribute('points',X0+','+(Y0-PS*P).toFixed(1)+' '+X1+','+(Y0-PS*P).toFixed(1));
  var g='',run=null,neg=0,tot=0;
  for(var k=0;k<=N;k++){
    var s=pv[k]<0?-1:1; tot+=Math.abs(pv[k]); if(pv[k]<0)neg+=-pv[k];
    if(run===null||run.s!==s){ if(run)g+=seg(run); run={s:s,a:[]}; }
    run.a.push(pts[k]+','+(Y0-PS*pv[k]).toFixed(1));
  }
  if(run)g+=seg(run);
  document.getElementById('pfill').innerHTML=g;
  document.getElementById('tho').textContent=d+'\u00B0';
  document.getElementById('m1').textContent=Math.abs(Math.cos(r)).toFixed(2)+(Math.abs(d)<1?'':(d>0?' lag':' lead'));
  document.getElementById('m2').textContent=Math.round(P)+' W';
  document.getElementById('m3').textContent=Math.round(Q)+' var';
  document.getElementById('m4').textContent=Math.round(100*neg/tot)+'%';
}
function seg(r){
  var c=r.s<0?'#D85A30':'#1D9E75';
  var f=r.a[0].split(',')[0], l=r.a[r.a.length-1].split(',')[0];
  return '<path d="M'+f+','+Y0+' L'+r.a.join(' L')+' L'+l+','+Y0+' Z" fill="'+c+'" fill-opacity="0.16" stroke="none"/>';
}
th.addEventListener('input',draw); draw();
})();
</script>
</div>

The green curve shows instantaneous power **p(t)**. This is the power actually consumed by the load
at each point in time. For example, at time (t) = 0 and phase angle (<span class="q">&theta;</span>) = 0, 375 W flows into the load.
Furthermore, note that instantaneous power is a function of t **p(t)** meaning we only consider instantaneous power in reference to 
a specific time. This natually brings us to our next quantity of interest, **real power**

**Real power (P)** is shown by the dashed purple line, and is the average of the instantaneous power. More specifically, you would use the
average power formula from ECE 402 to calculate this. For your convenience, this is: 

Everything the orange regions represent averages out to
nothing, yet it still had to be carried by the conductors on the way there and
back. That round trip is <span class="q">Q</span>.

## What to try

- Drag <span class="q">&theta;</span> to <span class="q">0&deg;</span>. The orange
  curve collapses onto the blue one and the negative lobes vanish completely. That
  is the resistor in **2.12(a)** — every instant of the cycle delivers power in the
  same direction.
- Drag to <span class="q">&minus;90&deg;</span>. The average goes flat at zero
  while <span class="q">p(t)</span> is still swinging as hard as ever. That is the
  capacitor in **2.12(b)**: it absorbs and returns the same energy every quarter
  cycle, so it consumes nothing on average, but it is still moving current through
  your wires.
- Watch the *Returned energy* figure as you sweep. At
  <span class="q">&plusmn;90&deg;</span> exactly half the energy that flows in
  flows back out. That fraction, not <span class="q">Q</span> itself, is what makes
  a low power factor expensive.
- Compare <span class="q">+60&deg;</span> and <span class="q">&minus;60&deg;</span>.
  The picture is nearly identical and <span class="q">P</span> is identical, but the
  sign of <span class="q">Q</span> flips — inductive versus capacitive. This is the
  sign convention that **2.19** depends on.
{: .try }