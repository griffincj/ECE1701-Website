---
layout: page
title: Power factor correction
description: Interactive power triangle showing how a capacitor bank shrinks the reactive leg without changing real power.
parent: Module 1 - Fundamentals
nav_order: 2
---

# Power factor correction on the power triangle

<span class="label label-blue">2.19</span>
<span class="label label-blue">2.27</span>

The formula everybody memorises hides the one thing worth seeing:
<span class="q">P</span> never moves.

<div class="demo-widget">
<h2 class="sr-only">Interactive power triangle for a 50 kW load at 0.8 lagging power factor, with a slider that adds capacitive reactive power and reports the resulting power factor, line current, and capacitance.</h2>
<div style="padding:1rem 0">
<svg width="100%" viewBox="0 0 680 300" role="img"><title>Power triangle with capacitor correction</title><desc>Fixed horizontal real power leg, shrinking vertical reactive leg, and hypotenuse representing apparent power.</desc>
<path id="ghost" d="M140 250 L440 250 L440 62.5 Z" fill="none" stroke="var(--text-muted)" stroke-width="0.5" stroke-dasharray="5 4"/>
<path id="tri" fill="#7F77DD" fill-opacity="0.12" stroke="#534AB7" stroke-width="1.5"/>
<line id="qc" x1="440" y1="62.5" x2="440" y2="62.5" stroke="#1D9E75" stroke-width="4"/>
<line x1="140" y1="250" x2="440" y2="250" stroke="#185FA5" stroke-width="3"/>
<text class="th" x="290" y="272" text-anchor="middle">P = 50 kW (fixed)</text>
<text class="ts" id="ql" x="456" y="160">Q</text>
<text class="ts" id="qcl" x="456" y="80">Q from capacitors</text>
<text class="ts" id="sl" x="230" y="140">S</text>
</svg>
<div style="display:flex;align-items:center;gap:12px;margin:8px 0 1.25rem">
<label for="qc-in" style="font-size:14px;color:var(--text-secondary);white-space:nowrap">Capacitor bank</label>
<input type="range" id="qc-in" min="0" max="37.5" step="0.5" value="0" style="flex:1"/>
<span id="qco" style="font-size:14px;font-weight:500;min-width:74px;text-align:right">0 kvar</span>
</div>
<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(130px,1fr));gap:12px">
<div style="background:var(--surface-1);border-radius:8px;padding:1rem"><p style="font-size:13px;color:var(--text-secondary);margin:0 0 4px">Power factor</p><p id="k1" style="font-size:24px;font-weight:500;margin:0">0.80 lag</p></div>
<div style="background:var(--surface-1);border-radius:8px;padding:1rem"><p style="font-size:13px;color:var(--text-secondary);margin:0 0 4px">Apparent |S|</p><p id="k2" style="font-size:24px;font-weight:500;margin:0">62.5 kVA</p></div>
<div style="background:var(--surface-1);border-radius:8px;padding:1rem"><p style="font-size:13px;color:var(--text-secondary);margin:0 0 4px">Line current</p><p id="k3" style="font-size:24px;font-weight:500;margin:0">284 A</p></div>
<div style="background:var(--surface-1);border-radius:8px;padding:1rem"><p style="font-size:13px;color:var(--text-secondary);margin:0 0 4px">Capacitance</p><p id="k4" style="font-size:24px;font-weight:500;margin:0">0 &micro;F</p></div>
</div>
<div style="margin-top:1rem"><button onclick="sendPrompt('Walk me through problem 2.27 step by step, showing how to get from 0.8 lagging to 0.95 lagging and then to the capacitance in microfarads.')">Copy this as a question for Claude &#8599;</button></div>
</div>
<script>
(function(){
var P=50,Q0=37.5,V=220,W=2*Math.PI*60,SC=5,BX=440,BY=250;
var s=document.getElementById('qc-in');
function up(){
  var qc=+s.value, Q=Q0-qc, ty=BY-SC*Q;
  document.getElementById('tri').setAttribute('d','M140 '+BY+' L'+BX+' '+BY+' L'+BX+' '+ty.toFixed(1)+' Z');
  document.getElementById('qc').setAttribute('y1',ty.toFixed(1));
  document.getElementById('qc').setAttribute('y2',(BY-SC*Q0).toFixed(1));
  document.getElementById('ql').setAttribute('y',(BY-SC*Q/2+4).toFixed(1));
  document.getElementById('ql').textContent='Q = '+Q.toFixed(1)+' kvar';
  document.getElementById('qcl').setAttribute('y',(BY-SC*(Q0+Q)/2+4).toFixed(1));
  document.getElementById('qcl').style.display=qc<1?'none':'';
  document.getElementById('sl').setAttribute('y',(BY-SC*Q/2-8).toFixed(1));
  var S=Math.sqrt(P*P+Q*Q), pf=P/S, C=qc*1000/(W*V*V)*1e6;
  document.getElementById('qco').textContent=qc.toFixed(1)+' kvar';
  document.getElementById('k1').textContent=pf.toFixed(3)+(Q>0.05?' lag':(Q<-0.05?' lead':''));
  document.getElementById('k2').textContent=S.toFixed(1)+' kVA';
  document.getElementById('k3').textContent=Math.round(S*1000/V)+' A';
  document.getElementById('k4').textContent=Math.round(C)+' \u00B5F';
}
s.addEventListener('input',up); up();
})();
</script>
</div>

A <span class="q">50 kW</span> load at <span class="q">0.8</span> lagging, on a
<span class="q">220 V</span>, <span class="q">60 Hz</span> supply. The dashed
outline is where the triangle started. The blue base is real power, and it does not
move no matter how far you push the slider.

## What to try

- Stop at <span class="q">21.0 kvar</span>. Power factor reads
  <span class="q">0.950</span> and capacitance reads about
  <span class="q">1150 &micro;F</span> — that is the answer to **2.27**, arrived at
  by dragging. (The slider moves in half-kvar steps; the exact figures are
  <span class="q">21.07 kvar</span> and <span class="q">1155 &micro;F</span>.)
- Watch line current while you do it: <span class="q">284 A</span> down to
  <span class="q">239 A</span>, a 16% reduction, with the load receiving exactly the
  same real power. This is the entire economic argument for capacitor banks.
  Thinner conductors, smaller transformers, lower losses.
- Keep an eye on the blue base line the whole time. Students routinely write
  answers in which real power changes after correction. It cannot. The capacitors
  are not doing work.
- Push all the way to <span class="q">37.5 kvar</span>. The reactive leg vanishes
  and the power factor hits unity — the theoretical best. But look at the cost of
  getting there: the first <span class="q">21 kvar</span> bought
  <span class="q">45 A</span> of current reduction, and the remaining
  <span class="q">16.5 kvar</span> buys only <span class="q">12 A</span> more.
  Diminishing returns like that are why utilities target
  <span class="q">0.95</span> rather than <span class="q">1.0</span>.
{: .try }

One thing the slider cannot show you, because it stops at unity: over-correction.
Add more capacitance than <span class="q">37.5 kvar</span> and the triangle flips
below the axis, the power factor goes *leading*, and it degrades again. A leading
power factor is just as bad as a lagging one, and it can push voltage above nominal
at light load.

<details markdown="block">
<summary>Worked solution for 2.27</summary>

<span class="q">&theta;&#8321; = cos&#8315;&#185;(0.80) = 36.87&deg;</span>, so
<span class="q">Q&#8321; = 50 tan(36.87&deg;) = 37.5 kvar</span>.

<span class="q">&theta;&#8322; = cos&#8315;&#185;(0.95) = 18.19&deg;</span>, so
<span class="q">Q&#8322; = 50 tan(18.19&deg;) = 16.4 kvar</span>.

The capacitors supply the difference:
<span class="q">Q<sub>C</sub> = 37.5 &minus; 16.4 = 21.1 kvar</span>.

For a capacitor, <span class="q">Q<sub>C</sub> = &omega;CV&sup2;</span>, so
<span class="q">C = 21100 / (377 &times; 220&sup2;) = 1.16 &times; 10&#8315;&sup3; F
&asymp; 1155 &micro;F</span>.

</details>
