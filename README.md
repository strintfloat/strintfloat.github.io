---
title: "Michael Thomas Ray — Work, Profile, Contact"
description: "Plain Markdown profile for Michael Thomas Ray: background, work history, and contact. No tracking."
---

# Michael Thomas Ray

## Table of Contents

- [Personal](#personal)
- [Current Focus](#current-focus)
- [Work History](#work-history)
- [Contact](#contact)
- [Klein Bottle Render](#klein-bottle-render)

---

## Personal

Welcome to my website, which hosts my public work and background.

Over the past two years, I married my partner of fourteen years and, shortly before our wedding, purchased a home where we now live and operate an equestrian & house pet boarding business. Before that, I spent over fifteen years in procurement and operational roles across higher education, corporate, and military environments.

I am resuming my career as an Accounts Payable Supervisor with Duquesne University, while hosting and deploying multiple AI/LLM models and frameworks for myself and my friends.

---

## Current Focus

- Career transition  
- Raising Wensley  
- Managing a horse boarding business  

**Computational Modeling**

- [Galaxies](https://strintfloat.github.io/MilkyWay.html)  
- [Geometric objects](https://strintfloat.github.io/Torus_Lab.html)  
- [Fractals](https://strintfloat.github.io/fractal.html)  
- [.3mf or .stl renders](https://strintfloat.github.io/benchy3d.html)  
- [WebGL-based OS in Browser, antiquity inspired](https://strintfloat.github.io/Tabularium.html)  

---

## Work History

**Duquesne University** — AP Supervisor (2024–Present)  
[duq.edu](https://www.duq.edu/)  

- Run and maintain an equestrian boarding business.

**Tailworthy Stables** — Co-Owner & Operator (2023–Present)  
[Tailworthy Stables on Facebook](https://www.facebook.com/p/Tail-Worthy-Stables-61558109177331/)  

- Run and maintain an equestrian boarding business.

**Carnegie Mellon University** — Procurement Specialist / Associate Buyer (2018–2024)  
[cmu.edu](https://www.cmu.edu/)  

- Handled purchasing and procurement for the College of Engineering and the wider university.

**LANXESS Corporation** — Vendor Data Analyst (2015–2018)  
[lanxess.com](https://lanxess.com/)  

- Maintained supplier records and aligned data policies with global headquarters in Germany.

**Education Management Corporation** — Finance & Procurement Systems Analyst (2012–2015)  
[Education Management Corporation](https://en.wikipedia.org/wiki/Education_Management_Corporation)  

- Managed purchasing systems, ERP upgrades, and corporate transitions.

**Duquesne University** — Accounts Payable Clerk, Work Study (2011–2012)  
[duq.edu](https://www.duq.edu/)  

- Processed invoices and expenses to support daily university operations.

**19th ESC & USAG Daegu PAO** — Administrative Support (2008–2009)  
[19th ESC](https://www.army.mil/19thESC) · [USAG Daegu PAO](https://home.army.mil/daegu/about/Garrison/public-affairs)  

- Supported logistics and communications for military transport and supply.  
- Wrote newsletters and photographed cultural and military events in Daegu, South Korea.

---

## Contact

**Email**  
[raym08@gmail.com](mailto:raym08@gmail.com)

**Links**  
- [LinkedIn](https://www.linkedin.com/in/michael-ray-610094360/)  
- [GitHub](https://github.com/strintfloat)  

**vCard**  
[Download vCard](data:text/vcard;charset=utf-8,BEGIN%3AVCARD%0AVERSION%3A4.0%0AFN%3AMichael%20Thomas%20Ray%0AORG%3ATailworthy%20Stables%0ATITLE%3AOwner%2FOperator%0AEMAIL%3BTYPE%3Dwork%3Araym08%40gmail.com%0ATEL%3BTYPE%3Dwork%2Cvoice%3A%2B14122593479%0AADR%3BTYPE%3Dwork%3A%3B%3B2210%20Henry%20Road%3B%3BSewickley%3BPA%3B15143%3BUSA%0AURL%3BTYPE%3Dwork%3Ahttps%3A%2F%2Fstrintfoat.github.io%0ABDAY%3A1989-07-20%0AEND%3AVCARD)

---

## Klein Bottle Render

<div style="margin-top:1rem;">
<canvas id="klein"></canvas>
</div>

<script>
/* ==== WebGL setup ==== */
const vs = `
  attribute vec3 a;
  uniform mat4 m;
  uniform float b;   // subtle scale (breath)
  varying vec3 p;
  void main(){
    p = a;
    gl_Position = m * vec4(a * (1.0 + b), 1.0);
  }`;
const fs = `
  precision mediump float;
  varying vec3 p;
  uniform float t;
  void main(){
    // rotate color field around central axis (Z) without moving geometry
    float ang = t * 0.0015;          // rotation speed
    float c = cos(ang), s = sin(ang);
    vec2 q = mat2(c, -s, s, c) * p.xy;

    // hue from rotated polar angle; slight radial wobble to avoid banding
    float h = fract(atan(q.y, q.x) / 6.2831853 + 0.08 * sin(6.2831853 * length(q)));

    vec3 col = 0.5 + 0.5 * cos(6.2831853 * (h + vec3(0.0, 0.3333333, 0.6666667)));
    gl_FragColor = vec4(col, 1.0);
  }`;


const g = document.getElementById('klein');
const gl = g.getContext('webgl', {antialias:true});
if(!gl) { throw 'WebGL unavailable'; }

/* ==== canvas sizing (mobile-balanced preset) ==== */
function resize(){
  const dpr = Math.min(window.devicePixelRatio || 1, 1.5); // cap DPR for perf
  g.width  = innerWidth * dpr;
  g.height = 300 * dpr;                                     // balanced height
  g.style.width  = '100%';
  g.style.height = '300px';
  gl.viewport(0,0,g.width,g.height);
}
addEventListener('resize', resize);

/* ==== program ==== */
function sh(t,s){ const h=gl.createShader(t); gl.shaderSource(h,s); gl.compileShader(h);
  if(!gl.getShaderParameter(h,gl.COMPILE_STATUS)) throw gl.getShaderInfoLog(h); return h; }
const prog = gl.createProgram();
gl.attachShader(prog, sh(gl.VERTEX_SHADER, vs));
gl.attachShader(prog, sh(gl.FRAGMENT_SHADER, fs));
gl.linkProgram(prog);
if(!gl.getProgramParameter(prog, gl.LINK_STATUS)) throw gl.getProgramInfoLog(prog);
gl.useProgram(prog);
const a  = gl.getAttribLocation(prog,'a');
const uM = gl.getUniformLocation(prog,'m');
const uT = gl.getUniformLocation(prog,'t');
const uB = gl.getUniformLocation(prog,'b');
/* ==== Klein param ==== */
function k(u,v){
  const A=1.4, cu2=Math.cos(u*0.5), su2=Math.sin(u*0.5), sv=Math.sin(v), s2v=Math.sin(2*v);
  const R=A + cu2*sv - su2*s2v;
  return [ R*Math.cos(u), R*Math.sin(u), su2*sv + cu2*s2v ];
}

/* ==== fragmented tile geometry (single draw) ==== */
// Mobile-balanced preset
const nu = 64, nv = 32;   // more tiles => smoother silhouette
const inset = 0.20;       // gap between tiles (0..0.49)

function lerp(a,b,t){ return a + (b-a)*t; }

// 4 verts per cell, 6 indices per cell
const verts = new Float32Array(nu*nv*4*3);
const indices = new Uint16Array(nu*nv*6);
let vp=0, ip=0, base=0;

for(let i=0;i<nu;i++){
  const u0=i/nu*Math.PI*2, u1=(i+1)/nu*Math.PI*2;
  const ui0=lerp(u0,u1,inset), ui1=lerp(u0,u1,1-inset);
  for(let j=0;j<nv;j++){
    const v0=j/nv*Math.PI*2, v1=(j+1)/nv*Math.PI*2;
    const vi0=lerp(v0,v1,inset), vi1=lerp(v0,v1,1-inset);

    const p00=k(ui0,vi0), p10=k(ui1,vi0), p11=k(ui1,vi1), p01=k(ui0,vi1);

    verts[vp++]=p00[0]; verts[vp++]=p00[1]; verts[vp++]=p00[2];
    verts[vp++]=p10[0]; verts[vp++]=p10[1]; verts[vp++]=p10[2];
    verts[vp++]=p11[0]; verts[vp++]=p11[1]; verts[vp++]=p11[2];
    verts[vp++]=p01[0]; verts[vp++]=p01[1]; verts[vp++]=p01[2];

    indices[ip++]=base; indices[ip++]=base+1; indices[ip++]=base+2;
    indices[ip++]=base; indices[ip++]=base+2; indices[ip++]=base+3;
    base += 4;
  }
}

// normalize to fit view
(function(){
  let minX=1e9,minY=1e9,minZ=1e9,maxX=-1e9,maxY=-1e9,maxZ=-1e9;
  for(let i=0;i<verts.length;i+=3){
    const x=verts[i], y=verts[i+1], z=verts[i+2];
    if(x<minX)minX=x; if(y<minY)minY=y; if(z<minZ)minZ=z;
    if(x>maxX)maxX=x; if(y>maxY)maxY=y; if(z>maxZ)maxZ=z;
  }
  const cx=(minX+maxX)/2, cy=(minY+maxY)/2, cz=(minZ+maxZ)/2;
  const s=1.6/Math.max(maxX-minX, Math.max(maxY-minY, maxZ-minZ)); // a bit smaller
  for(let i=0;i<verts.length;i+=3){
    verts[i]=(verts[i]-cx)*s; verts[i+1]=(verts[i+1]-cy)*s; verts[i+2]=(verts[i+2]-cz)*s;
  }
})();

// upload buffers
const VB = gl.createBuffer(); gl.bindBuffer(gl.ARRAY_BUFFER, VB);
gl.bufferData(gl.ARRAY_BUFFER, verts, gl.STATIC_DRAW);
const IB = gl.createBuffer(); gl.bindBuffer(gl.ELEMENT_ARRAY_BUFFER, IB);
gl.bufferData(gl.ELEMENT_ARRAY_BUFFER, indices, gl.STATIC_DRAW);
gl.enableVertexAttribArray(a); gl.vertexAttribPointer(a, 3, gl.FLOAT, false, 0, 0);

/* ==== math ==== */
function I(){return new Float32Array([1,0,0,0, 0,1,0,0, 0,0,1,0, 0,0,0,1]);}
function P(f,a,n,F){const t=1/Math.tan(f/2), nf=1/(n-F), m=new Float32Array(16);
  m[0]=t/a; m[5]=t; m[10]=(F+n)*nf; m[11]=-1; m[14]=2*F*n*nf; return m;}
function T(x,y,z){const m=I(); m[12]=x; m[13]=y; m[14]=z; return m;}
function Rx(a){const c=Math.cos(a),s=Math.sin(a); return new Float32Array([1,0,0,0, 0,c,-s,0, 0,s,c,0, 0,0,0,1]);}
function Ry(a){const c=Math.cos(a),s=Math.sin(a); return new Float32Array([c,0,s,0, 0,1,0,0, -s,0,c,0, 0,0,0,1]);}
function MM(A,B){const o=new Float32Array(16); for(let r=0;r<4;r++)for(let c=0;c<4;c++)
  o[r*4+c]=A[r*4+0]*B[0*4+c]+A[r*4+1]*B[1*4+c]+A[r*4+2]*B[2*4+c]+A[r*4+3]*B[3*4+c]; return o;}

/* ==== interaction & camera ==== */
let yaw=0, pitch=0, panX=0, panY=0, dist=3.5;
const fov=45*Math.PI/180, distMin=1.0, distMax=20.0;
let dragging=false, panMode=false, lastX=0, lastY=0;
let lastInteraction=0;
const idleDelay=0, autoYawPerSec=0.15;
g.style.cursor='grab';

function clamp(v,a,b){ return Math.max(a, Math.min(b,v)); }
function mark(){ lastInteraction = performance.now(); }

g.addEventListener('mousedown', e=>{
  dragging=true; lastX=e.clientX; lastY=e.clientY;
  panMode=(e.button===2)||e.ctrlKey||e.metaKey;
  g.style.cursor='grabbing'; mark();
});
addEventListener('mouseup', ()=>{ dragging=false; g.style.cursor='grab'; mark(); });
g.addEventListener('contextmenu', e=>e.preventDefault());
g.addEventListener('mousemove', e=>{
  if(!dragging) return;
  const dx=e.clientX-lastX, dy=e.clientY-lastY; lastX=e.clientX; lastY=e.clientY;
  const asp=g.width/g.height;
  if(panMode){
    const worldPerPixel=(2*dist*Math.tan(fov/2))/g.height;
    panX -= dx*worldPerPixel*asp;
    panY += dy*worldPerPixel;
  }else{
    yaw   += dx*0.005;
    pitch  = clamp(pitch + dy*0.005, -1.5, 1.5);
  }
  mark();
});
g.addEventListener('wheel', e=>{
  e.preventDefault();
  const k=Math.exp(e.deltaY*0.0015);
  dist = clamp(dist*k, distMin, distMax);
  mark();
},{passive:false});

/* ==== render ==== */
gl.enable(gl.DEPTH_TEST);
gl.clearColor(.07,.07,.09,1);
resize();

let prevT=null;
function frame(t){
  if(prevT===null) prevT=t;
  const dt=(t-prevT)/1000; prevT=t;
  
  if(!dragging && (t-lastInteraction)>idleDelay){ yaw += autoYawPerSec*dt; }
const breatheScale = 0.015 * Math.sin(t * 0.0008);
gl.uniform1f(uB, breatheScale);

  gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
  const asp=g.width/g.height;
  const Pp=P(fov, asp, .1, 100);
  const R=MM(Ry(yaw), Rx(pitch));
  const V=T(panX,panY,-dist);
  const MVP=MM(R, MM(V, Pp));
  gl.uniformMatrix4fv(uM, false, MVP);
  gl.uniform1f(uT, t);

  // single draw call
  gl.drawElements(gl.TRIANGLES, indices.length, gl.UNSIGNED_SHORT, 0);

  requestAnimationFrame(frame);
}
requestAnimationFrame(frame);
</script>

---

<small>Static Markdown with inline WebGL canvas. No tracking or third-party requests. © <time datetime="2025">2025</time> Michael Thomas Ray.</small>
