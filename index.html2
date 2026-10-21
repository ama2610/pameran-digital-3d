<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Pameran Digital 3D</title>
<style>
  html, body { margin: 0; height: 100%; overflow: hidden; background: #3d3f2a; font-family: Georgia, serif; }
  canvas { display: block; }
  #start { position: fixed; inset: 0; display: flex; flex-direction: column; align-items: center; justify-content: center;
    background: rgba(61,63,42,.78); color: #FBF7F4; text-align: center; cursor: pointer; z-index: 5; }
  #start h1 { font-weight: 400; letter-spacing: .25em; margin: 0 0 .4em; font-size: clamp(22px, 5vw, 44px); }
  #start p { margin: .2em; color: #E8D8CE; font-family: system-ui, sans-serif; font-size: 14px; }
  #start b { display: inline-block; margin-top: 1.4em; padding: .7em 1.8em; border: 1px solid #CF7D65; color: #FBF7F4; font-weight: 400; letter-spacing: .15em; }
  #dot { position: fixed; left: 50%; top: 50%; width: 6px; height: 6px; margin: -3px; border-radius: 50%;
    background: rgba(251,247,244,.85); box-shadow: 0 0 0 1px rgba(59,47,42,.5); pointer-events: none; }
  #info { position: fixed; left: 50%; bottom: 28px; transform: translate(-50%, 20px); width: min(520px, 90vw); padding: 16px 22px;
    background: rgba(251,247,244,.94); color: #3d3f2a; border-left: 4px solid #CF7D65; opacity: 0; transition: .25s; pointer-events: none; }
  #info.on { opacity: 1; transform: translate(-50%, 0); }
  #info h2 { margin: 0 0 4px; font-size: 20px; font-weight: 400; }
  #info p { margin: 0; font: 14px/1.5 system-ui, sans-serif; color: #6B6D43; }
  #info small { display: block; margin-top: 6px; font: 11px system-ui, sans-serif; letter-spacing: .1em; color: #CF7D65; }
  #toolbar { position: fixed; right: 14px; bottom: 14px; z-index: 6; }
  #toolbar button { padding: 10px 18px; background: #CF7D65; color: #FBF7F4; border: 0; cursor: pointer;
    font: 13px system-ui, sans-serif; letter-spacing: .08em; }
  #toolbar button:hover { background: #6B6D43; }
  #editor { position: fixed; top: 0; right: 0; width: 290px; height: 100%; box-sizing: border-box; overflow: auto; z-index: 7; display: none;
    background: #FBF7F4; color: #3d3f2a; padding: 16px; font: 13px system-ui, sans-serif; box-shadow: -4px 0 20px rgba(0,0,0,.25); }
  #editor h3 { margin: 0 0 10px; font: 400 16px Georgia, serif; letter-spacing: .18em; }
  #editor h4 { margin: 16px 0 6px; font-size: 11px; letter-spacing: .12em; color: #CF7D65; text-transform: uppercase; }
  #editor label { display: block; margin: 8px 0 0; color: #6B6D43; font-size: 12px; }
  #editor input, #editor textarea { width: 100%; box-sizing: border-box; margin-top: 3px; padding: 6px; border: 1px solid #E8D8CE; background: #fff; font: inherit; color: #3d3f2a; }
  #editor input[type=color] { height: 30px; padding: 2px; } #editor input[type=range] { padding: 0; }
  #editor .row { display: flex; gap: 6px; flex-wrap: wrap; margin-top: 8px; }
  #editor button { flex: 1; padding: 7px 8px; border: 0; background: #E8D8CE; color: #3d3f2a; cursor: pointer; font: inherit; }
  #editor button:hover { background: #CF7D65; color: #fff; }
  #editor .note { color: #6B6D43; font-size: 11px; line-height: 1.5; }
  #toolbar.shift { right: 304px; } #toolbar button + button { margin-left: 6px; }
</style>
</head>
<body>
<div id="start"><h1>PAMERAN DIGITAL</h1><p>WASD / panah: berjalan &bull; Mouse: melihat sekeliling &bull; Esc: keluar</p>
  <p>Dekati karya untuk membaca keterangannya</p>
  <p>Arahkan ke bingkai lalu tekan <u>E</u> atau klik untuk mengganti dengan fotomu &bull; <u>U</u> / tombol kanan bawah: tambah banyak foto &bull; seret file gambar ke layar</p><b>MASUK RUANGAN</b> <b id="goEdit" style="margin-left:10px">BUKA EDITOR</b></div>
<div id="dot"></div>
<div id="toolbar"><button id="addBtn">+ TAMBAH FOTO</button><button id="modeBtn">MODE EDIT</button></div>
<div id="editor">
  <h3>EDITOR PAMERAN</h3>
  <h4>Pilih Bingkai</h4>

<label>Pilih karya
  <select id="frameSelect">
    <option value="-1">-- Pilih karya --</option>
  </select>
</label>

<p id="noSel" class="note">
  Pilih karya dari daftar di atas untuk mengeditnya.
</p>
  <div id="selForm" style="display:none">
    <small id="selName" class="note"></small>
    <label>Judul<input id="fTitle"></label>
    <label>Tahun<input id="fYear" placeholder="2026"></label>
    <label>Deskripsi / label museum<textarea id="fDesc" rows="3"></textarea></label>
    <label>Tautan (toko / info)<input id="fLink" placeholder="https://"></label>
    <label>Ukuran<input type="range" id="fScale" min="0.5" max="1.3" step="0.05" value="1"></label>
    <label>Warna bingkai<input type="color" id="fColor" value="#6b6d43"></label>
    <div class="row"><button id="bPhoto">Ganti foto</button><button id="bReset">Reset</button></div>
    <div class="row"><button id="bPrev">&larr; Tukar</button><button id="bNext">Tukar &rarr;</button></div>
  </div>
  <h4>Ruangan</h4>
  <div class="row"><button data-theme="bunga">Bunga</button><button data-theme="milk">Milk</button><button data-theme="blush">Blush</button><button data-theme="cocoa">Cocoa</button><button data-theme="dark">Espresso</button></div>
  <label>Pencahayaan<input type="range" id="lightRange" min="0.3" max="1.6" step="0.05" value="1"></label>
  <h4>Berkas</h4>
  <div class="row"><button id="bBulk">+ Banyak foto</button><button id="bSave">Simpan JSON</button><button id="bLoad">Muat JSON</button></div>
  <p class="note">Publikasi: unggah file HTML ini dan <b>exhibition.json</b> ke hosting yang sama (GitHub Pages, Netlify, dll.), lalu bagikan link dengan <b>?view=1</b> untuk tampilan pengunjung tanpa editor.</p>
</div>
<input type="file" id="jsonPicker" accept="application/json" hidden>
<input type="file" id="picker" accept="image/*" hidden>
<div id="info"><h2></h2><p></p><small></small></div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
/* ============================================================
   buildExhibitionRoom(scene)
   Return: { group, spawnPoint, wallSlots, pedestals }
   Tambahan: colliders (titik tabrakan), artworks (karya interaktif), spinners
   ============================================================ */
function buildExhibitionRoom(scene) {
  const group = new THREE.Group();
  const rad = d => d * Math.PI / 180;
  const C = { milk: 0xFBF7F4, oat: 0xE8D8CE, rose: 0xB98F88, taupe: 0x7A6258, espresso: 0x3B2F2A };
  const mWall  = new THREE.MeshStandardMaterial({ color: C.milk,  roughness: 0.55, side: THREE.DoubleSide });
  const mOat   = new THREE.MeshStandardMaterial({ color: C.oat,   roughness: 0.6,  side: THREE.DoubleSide });
  const mTaupe = new THREE.MeshStandardMaterial({ color: C.taupe, roughness: 0.5,  side: THREE.DoubleSide });
  const mRose  = new THREE.MeshStandardMaterial({ color: C.rose,  roughness: 0.5 });
  const mEsp   = new THREE.MeshStandardMaterial({ color: C.espresso, roughness: 0.6 });
  const mGlow  = new THREE.MeshBasicMaterial({ color: 0xFFE9D2, side: THREE.DoubleSide });
  // tekstur noise prosedural (beton / plester halus)
  function noiseTex(size, base, amp, rep, srgb) {
    const cv = document.createElement('canvas'); cv.width = cv.height = size;
    const g = cv.getContext('2d'), d = g.createImageData(size, size);
    for (let i = 0; i < d.data.length; i += 4) { const v = base + (Math.random() - 0.5) * amp; d.data[i] = d.data[i + 1] = d.data[i + 2] = v; d.data[i + 3] = 255; }
    g.putImageData(d, 0, 0);
    for (let i = 0; i < 40; i++) { // bercak besar lembut
      const x = Math.random() * size, y = Math.random() * size, r = size * (0.08 + Math.random() * 0.2);
      const gr = g.createRadialGradient(x, y, 0, x, y, r), c = Math.random() < 0.5 ? '0,0,0' : '255,255,255';
      gr.addColorStop(0, `rgba(${c},0.07)`); gr.addColorStop(1, `rgba(${c},0)`); g.fillStyle = gr; g.fillRect(x - r, y - r, 2 * r, 2 * r); }
    const t = new THREE.CanvasTexture(cv); t.wrapS = t.wrapT = THREE.RepeatWrapping; t.repeat.set(rep, rep); t.anisotropy = 8;
    if (srgb) t.encoding = THREE.sRGBEncoding; return t;
  }
  const mFloor = new THREE.MeshStandardMaterial({ color: 0x80706a, map: noiseTex(512, 225, 40, 10, true), roughness: 0.55, metalness: 0.1 });
  mFloor.roughnessMap = mFloor.bumpMap = noiseTex(512, 190, 70, 10, false); mFloor.bumpScale = 0.4;
  mWall.bumpMap = mOat.bumpMap = noiseTex(256, 128, 50, 0.4, false); mWall.bumpScale = mOat.bumpScale = 0.25;
  const mSage = new THREE.MeshStandardMaterial({ color: 0x99B4AA, roughness: 0.55, side: THREE.DoubleSide, bumpMap: mWall.bumpMap, bumpScale: 0.25 });  // dinding aksen
  const mFrame = new THREE.MeshStandardMaterial({ color: C.espresso, roughness: 0.5 });

  const colliders = [];   // {x, z, r} titik-titik penghalang
  const artworks = [];    // mesh karya (untuk raycast)
  const spinners = [];    // objek digital yang berputar
  const frames = [];      // semua bingkai yang bisa diisi foto

  // ---- Dinding busur (sektor cincin). Sudut dunia: x=cx+r cos, z=cz+r sin ----
  function arc(cx, cz, ri, ro, a0, a1, y0, h, mat) {
    const s = new THREE.Shape(), n = 48;
    for (let i = 0; i <= n; i++) { const a = rad(a0 + (a1 - a0) * i / n);
      i ? s.lineTo(ro * Math.cos(a), -ro * Math.sin(a)) : s.moveTo(ro * Math.cos(a), -ro * Math.sin(a)); }
    for (let i = n; i >= 0; i--) { const a = rad(a0 + (a1 - a0) * i / n); s.lineTo(ri * Math.cos(a), -ri * Math.sin(a)); }
    const g = new THREE.ExtrudeGeometry(s, { depth: h, bevelEnabled: false, curveSegments: 1 });
    g.rotateX(-Math.PI / 2);
    const m = new THREE.Mesh(g, mat); m.position.set(cx, y0, cz); m.castShadow = mat !== mGlow; m.receiveShadow = true; group.add(m); return m;
  }
  // titik tabrakan sepanjang garis tengah busur
  function addCollider(cx, cz, r, a0, a1, rr) {
    const steps = Math.ceil(rad(a1 - a0) * r / 0.35);
    for (let i = 0; i <= steps; i++) { const a = rad(a0 + (a1 - a0) * i / steps);
      colliders.push({ x: cx + r * Math.cos(a), z: cz + r * Math.sin(a), r: rr }); }
  }
  // dinding + baseboard + overhang + lampu cove tersembunyi
  function curvedWall(cx, cz, r, a0, a1, h, t, wm = mWall) {
    const ri = r - t, ro = r;
    arc(cx, cz, ri, ro, a0, a1, 0.12, h - 0.12, wm);
    arc(cx, cz, ri - 0.04, ro + 0.04, a0, a1, 0, 0.12, mTaupe);
    arc(cx, cz, ri - 0.2, ro + 0.2, a0, a1, h - 0.25, 0.25, wm);
    arc(cx, cz, ri - 0.2, ri, a0, a1, h - 0.27, 0.03, mGlow);
    arc(cx, cz, ro, ro + 0.2, a0, a1, h - 0.27, 0.03, mGlow);
    addCollider(cx, cz, r - t / 2, a0, a1, 0.08 + t / 2);
  }

  // ---- Lantai + cincin lampu ----
  const floor = new THREE.Mesh(new THREE.CircleGeometry(24, 128), mFloor);
  floor.rotation.x = -Math.PI / 2; floor.receiveShadow = true; group.add(floor);
  const ringLight = (x, z, a, b, y, up) => { const r = new THREE.Mesh(new THREE.RingGeometry(a, b, 96), mGlow);
    r.rotation.x = up ? Math.PI / 2 : -Math.PI / 2; r.position.set(x, y, z); group.add(r); };
  ringLight(0, 0, 16.2, 16.3, 0.01, false);
  // karpet penuntun dari pintu
  const mRunner = new THREE.MeshStandardMaterial({ color: 0xd8c2b6, roughness: 0.95 });
  const runner = new THREE.Mesh(new THREE.PlaneGeometry(3, 10), mRunner);
  runner.rotation.x = -Math.PI / 2; runner.position.set(0, 0.012, 14.5); runner.receiveShadow = true; group.add(runner);
  // kolom pembingkai pintu masuk
  [130, 410].forEach(d => { const a = rad(d);
    const c = new THREE.Mesh(new THREE.CylinderGeometry(0.3, 0.3, 3.2, 32), mOat);
    c.position.set(14.85 * Math.cos(a), 1.6, 14.85 * Math.sin(a)); c.castShadow = c.receiveShadow = true; group.add(c); });

  // ---- Dinding luar bulat (lebar, pintu depan ±80°) ----
  curvedWall(0, 0, 15, 130, 410, 3.2, 0.3);
  // ---- Dinding S berkelok: dua busur berlawanan bersambung di pusat ----
  curvedWall(0, 5, 5, 160, 270, 2.6, 0.25, mSage);
  curvedWall(0, -5, 5, -20, 90, 2.6, 0.25, mSage);

  // ---- Plafon + cincin cahaya + downlight ----
  const ceil = new THREE.Mesh(new THREE.CircleGeometry(17, 128), mWall);
  ceil.rotation.x = Math.PI / 2; ceil.position.y = 3.6; group.add(ceil);
  [[12.2, 12.35], [7.8, 7.95], [3.2, 3.35]].forEach(([a, c]) => ringLight(0, 0, a, c, 3.59, true));
  [[10, 24], [5, 12]].forEach(([R, N]) => { for (let i = 0; i < N; i++) { const a = i / N * Math.PI * 2;
    const dl = new THREE.Mesh(new THREE.CircleGeometry(0.13, 24), mGlow);
    dl.rotation.x = Math.PI / 2; dl.position.set(R * Math.cos(a), 3.59, R * Math.sin(a)); group.add(dl); } });

  // ---- Tanaman realistis: ribuan helai daun (InstancedMesh) bertekstur urat daun, bukan blok low-poly ----
  const sway = [];   // grup yang bergoyang pelan (dipakai demo)
  function leafTex(w, c1, c2) {            // tekstur daun prosedural (alpha = bentuk daun)
    const cv = document.createElement('canvas'); cv.width = cv.height = 256; const g = cv.getContext('2d');
    const x0 = 128 - 118 * w, x1 = 128 + 118 * w;
    g.beginPath(); g.moveTo(128, 252); g.bezierCurveTo(x0, 210, x0 - 6, 70, 128, 4); g.bezierCurveTo(x1 + 6, 70, x1, 210, 128, 252); g.closePath();
    const gr = g.createLinearGradient(0, 252, 0, 0); gr.addColorStop(0, c1); gr.addColorStop(1, c2); g.fillStyle = gr; g.fill();
    g.save(); g.clip();
    g.strokeStyle = 'rgba(255,255,255,0.22)'; g.lineWidth = 3; g.beginPath(); g.moveTo(128, 252); g.lineTo(128, 10); g.stroke();   // tulang daun
    g.lineWidth = 1.6; g.strokeStyle = 'rgba(255,255,255,0.12)';
    for (let i = 1; i < 9; i++) { const y = 252 - i * 26; g.beginPath(); g.moveTo(128, y); g.lineTo(128 - 100 * w, y - 40); g.moveTo(128, y); g.lineTo(128 + 100 * w, y - 40); g.stroke(); }
    for (let i = 0; i < 500; i++) { g.fillStyle = Math.random() < 0.5 ? 'rgba(0,0,0,0.04)' : 'rgba(255,255,255,0.04)'; g.fillRect(Math.random() * 256, Math.random() * 256, 3, 3); }
    g.restore(); g.lineWidth = 3; g.strokeStyle = 'rgba(20,40,15,0.5)'; g.stroke();
    const t = new THREE.CanvasTexture(cv); t.encoding = THREE.sRGBEncoding; t.anisotropy = 8; return t;
  }
  const leafA = leafTex(0.55, '#2c5528', '#6fa150'), leafB = leafTex(0.8, '#386a2e', '#8bbf62');
  const leafGeo = (() => {                 // helai daun melengkung (lipatan tengah + ujung merunduk)
    const g = new THREE.PlaneGeometry(1, 1, 2, 5); g.translate(0, 0.5, 0);
    const P = g.attributes.position;
    for (let i = 0; i < P.count; i++) { const x = P.getX(i), y = P.getY(i); P.setZ(i, Math.abs(x) * 0.35 - y * y * 0.3); }
    g.computeVertexNormals(); return g;
  })();
  const _q1 = new THREE.Quaternion(), _q2 = new THREE.Quaternion(), _m = new THREE.Matrix4(), _s = new THREE.Vector3(),
        _n = new THREE.Vector3(), _u = new THREE.Vector3(), _c = new THREE.Color(), Yv = new THREE.Vector3(0, 1, 0), Zv = new THREE.Vector3(0, 0, 1);
  // gen(i) -> { p: posisi pangkal, dir: arah daun, face: arah sisi atas daun, len, wid }
  function leafCloud(parent, tex, n, gen) {
    const mat = new THREE.MeshStandardMaterial({ map: tex, alphaTest: 0.5, side: THREE.DoubleSide, roughness: 0.55 });
    const mesh = new THREE.InstancedMesh(leafGeo, mat, n);
    for (let i = 0; i < n; i++) {
      const L = gen(i), d = L.dir.normalize(); _q1.setFromUnitVectors(Yv, d);
      _n.copy(Zv).applyQuaternion(_q1); _u.copy(L.face).addScaledVector(d, -d.dot(L.face));
      let roll = 0; if (_u.lengthSq() > 1e-4) { _u.normalize(); roll = Math.atan2(d.dot(_n.clone().cross(_u)), _n.dot(_u)); }
      _q2.setFromAxisAngle(d, roll + (Math.random() - 0.5) * 0.7).multiply(_q1);
      _m.compose(L.p, _q2, _s.set(L.wid, L.len, 1)); mesh.setMatrixAt(i, _m);
      const v = 0.78 + Math.random() * 0.32; mesh.setColorAt(i, _c.setRGB(v * (0.9 + Math.random() * 0.15), v, v * (0.82 + Math.random() * 0.15)));
    }
    mesh.frustumCulled = false; mesh.castShadow = true;
    mesh.customDepthMaterial = new THREE.MeshDepthMaterial({ depthPacking: THREE.RGBADepthPacking, map: tex, alphaTest: 0.5 });
    parent.add(mesh); return mesh;
  }
  const R3 = (x, y, z) => new THREE.Vector3(x, y, z);

  // tanaman pot: batang tipis + daun menyebar ke atas dan keluar
  function potPlant(x, z, s) {
    const g = new THREE.Group(); g.position.set(x, 0, z);
    const pot = new THREE.Mesh(new THREE.CylinderGeometry(0.32 * s, 0.25 * s, 0.6 * s, 32), mRose); pot.position.y = 0.3 * s; pot.castShadow = true;
    const soil = new THREE.Mesh(new THREE.CircleGeometry(0.3 * s, 24), mEsp); soil.rotation.x = -Math.PI / 2; soil.position.y = 0.6 * s + 0.005;
    const stem = new THREE.Mesh(new THREE.CylinderGeometry(0.015 * s, 0.03 * s, 0.9 * s, 6), mEsp); stem.position.y = 1.05 * s;
    g.add(pot, soil, stem);
    leafCloud(g, leafA, 130, () => { const a = Math.random() * 6.283, h = 0.2 + Math.random() * 0.7;
      return { p: R3(Math.cos(a) * 0.02 * s, (0.6 + h) * s, Math.sin(a) * 0.02 * s),
               dir: R3(Math.cos(a) * (0.9 - h * 0.35), 0.3 + h * 0.5, Math.sin(a) * (0.9 - h * 0.35)), face: R3(0, 1, 0),
               len: (0.25 + Math.random() * 0.2) * s, wid: (0.16 + Math.random() * 0.08) * s }; });
    group.add(g); colliders.push({ x, z, r: 0.45 * s });
  }
  [[-8.3, 10.2], [8.3, 10.2], [-12.8, 3.4], [12.8, 3.4], [-4.1, -12.55], [4.1, -12.55]].forEach(([x, z]) => potPlant(x, z, 1.6));

  // tanaman gantung berbentuk kerucut terbalik dari cincin plafon (seperti referensi) + bak tanaman di lantai
  function hangingPlant(x, z) {
    const R = 1.8, H = 2.4, top = 3.3;
    // rumah cincin di plafon + lampu cincin
    const housing = new THREE.Mesh(new THREE.CylinderGeometry(2.05, 2.05, 0.3, 64), mOat); housing.position.set(x, 3.45, z); group.add(housing);
    const under = new THREE.Mesh(new THREE.CircleGeometry(1.95, 64), new THREE.MeshStandardMaterial({ color: 0x2a211d, roughness: 0.8 }));
    under.rotation.x = Math.PI / 2; under.position.set(x, top - 0.005, z); group.add(under);
    ringLight(x, z, 2.0, 2.08, top - 0.004, true);
    // kerucut dedaunan (menggantung & bergoyang pelan)
    const hang = new THREE.Group(); hang.position.set(x, top, z); group.add(hang); sway.push(hang);
    const core = new THREE.Mesh(new THREE.ConeGeometry(R * 0.88, H, 40, 1, true), new THREE.MeshStandardMaterial({ color: 0x2b4425, roughness: 0.9, side: THREE.DoubleSide }));
    core.rotation.x = Math.PI; core.position.y = -H / 2; hang.add(core);
    leafCloud(hang, leafA, 1100, () => { const t = 1 - Math.sqrt(1 - Math.random()), th = Math.random() * 6.283, rr = R * Math.pow(1 - t, 0.85) * (0.85 + Math.random() * 0.2);
      const rad = R3(Math.cos(th), 0, Math.sin(th));
      return { p: R3(rad.x * rr, -t * H, rad.z * rr), dir: rad.clone().multiplyScalar(0.45).add(R3((Math.random() - .5) * .5, -0.9, (Math.random() - .5) * .5)),
               face: rad, len: (0.4 + Math.random() * 0.4) * (0.7 + (1 - t) * 0.5), wid: 0.22 + Math.random() * 0.12 }; });
    // sulur menjuntai dari tepi cincin
    const specs = [];
    for (let k = 0; k < 14; k++) {
      const a = Math.random() * 6.283, r0 = R * (0.92 + Math.random() * 0.12), len = 0.6 + Math.random() * 0.9, ph = Math.random() * 6;
      const curve = new THREE.CatmullRomCurve3([0, 0.33, 0.66, 1].map(u => R3(Math.cos(a) * (r0 + u * 0.05) + Math.sin(u * 3 + ph) * 0.05, -0.02 - u * len, Math.sin(a) * (r0 + u * 0.05))));
      hang.add(new THREE.Mesh(new THREE.TubeGeometry(curve, 12, 0.008, 5), new THREE.MeshStandardMaterial({ color: 0x2f4a26, roughness: 0.8 })));
      for (let j = 1; j <= 7; j++) specs.push({ p: curve.getPoint(j / 8), dir: R3(Math.cos(a) * 0.4 + (Math.random() - .5) * .6, -0.8, Math.sin(a) * 0.4 + (Math.random() - .5) * .6),
        face: R3(Math.cos(a), 0, Math.sin(a)), len: 0.12 + Math.random() * 0.07, wid: 0.1 + Math.random() * 0.05 });
    }
    leafCloud(hang, leafB, specs.length, i => specs[i]);
    // bak tanaman di lantai dengan daun rendah
    const basin = new THREE.Mesh(new THREE.CylinderGeometry(1.0, 1.05, 0.3, 48), mWall); basin.position.set(x, 0.15, z); basin.castShadow = basin.receiveShadow = true; group.add(basin);
    const low = new THREE.Group(); low.position.set(x, 0.3, z); group.add(low);
    leafCloud(low, leafA, 110, () => { const a = Math.random() * 6.283, r = Math.sqrt(Math.random()) * 0.8;
      return { p: R3(Math.cos(a) * r, 0, Math.sin(a) * r), dir: R3(Math.cos(a) * 0.7, 0.8, Math.sin(a) * 0.7), face: R3(0, 1, 0), len: 0.3 + Math.random() * 0.25, wid: 0.2 }; });
    colliders.push({ x, z, r: 1.15 });
    const pl = new THREE.PointLight(0xdff2c8, 0.5, 7, 1.5); pl.position.set(x, 1.6, z); group.add(pl);   // pantulan hijau lembut
  }

  // ---- Pulau bulat: bench melingkar (dua busur dengan celah di sisi) ----
  function bench(x, z, ri, ro, a0, a1) {
    arc(x, z, ri, ro, a0, a1, 0, 0.45, mOat); arc(x, z, ri - 0.03, ro + 0.03, a0, a1, 0.45, 0.05, mWall);
    addCollider(x, z, (ri + ro) / 2, a0, a1, 0.3);
  }
  function station(x, z, ri) { bench(x, z, ri, ri + 0.45, 25, 155); bench(x, z, ri, ri + 0.45, 205, 335); ringLight(x, z, ri + 0.7, ri + 0.77, 0.01, false); }
  station(0, 5.5, 2.0); hangingPlant(0, 5.5);   // depan-tengah: tanaman gantung (ubah koordinat di sini untuk memindahkan)

  // ---- Pedestal + objek digital berputar ----
  const pedestals = [];
  [[0, -5.5], [-9.5, -3], [9.5, -3], [0, -11]].forEach(([x, z], i) => {
    station(x, z, 1.7);
    const base = new THREE.Mesh(new THREE.CylinderGeometry(0.4, 0.45, 1.0, 32), mWall); base.position.set(x, 0.5, z);
    const cap = new THREE.Mesh(new THREE.CylinderGeometry(0.46, 0.46, 0.06, 32), mRose); cap.position.set(x, 1.03, z);
    base.castShadow = cap.castShadow = base.receiveShadow = true; group.add(base, cap);
    colliders.push({ x, z, r: 0.5 });
    const geo = [new THREE.IcosahedronGeometry(0.28, 1), new THREE.TorusKnotGeometry(0.2, 0.07, 80, 12),
                 new THREE.OctahedronGeometry(0.3), new THREE.TorusGeometry(0.25, 0.08, 12, 32)][i];
    const obj = new THREE.Mesh(geo, new THREE.MeshStandardMaterial({ color: [0xCF7D65, 0x6B6D43, 0x99B4AA, 0xABA66F][i],
      roughness: 0.25, metalness: 0.5, wireframe: i === 0 }));
    obj.position.set(x, 1.55, z); group.add(obj); spinners.push(obj);
    pedestals.push({ position: { x, y: 1.06, z } });
  });

  // ---- Karya digital prosedural (canvas, palet referensi) ----
  function rng(s) { return () => { s |= 0; s = s + 0x6D2B79F5 | 0; let t = Math.imul(s ^ s >>> 15, 1 | s);
    t = t + Math.imul(t ^ t >>> 7, 61 | t) ^ t; return ((t ^ t >>> 14) >>> 0) / 4294967296; }; }
  const PAL = ['#F6E6D6', '#E1B8A2', '#CF7D65', '#99B4AA', '#6B6D43'];
  function makeArt(idx, w, h) {
    const cv = document.createElement('canvas'); cv.width = 768; cv.height = Math.round(768 * h / w);
    const g = cv.getContext('2d'), W = cv.width, H = cv.height, R = rng(idx * 97 + 11), style = idx % 4;
    const grd = g.createLinearGradient(0, 0, W, H);
    const dark = idx % 2; grd.addColorStop(0, PAL[dark ? 4 : 0]); grd.addColorStop(1, PAL[dark ? 3 : 1]);
    g.fillStyle = grd; g.fillRect(0, 0, W, H);
    if (style === 0) { for (let i = 0; i < 14; i++) { g.globalAlpha = 0.25 + R() * 0.5; g.fillStyle = PAL[1 + (R() * 3 | 0)];
      g.beginPath(); g.arc(R() * W, R() * H, 30 + R() * 120, 0, 7); g.fill(); } }
    else if (style === 1) { for (let k = 0; k < 9; k++) { g.globalAlpha = 0.7; g.strokeStyle = PAL[1 + (k % 4)]; g.lineWidth = 6;
      g.beginPath(); for (let x = 0; x <= W; x += 8) { const y = H * (0.15 + k * 0.09) + Math.sin(x * 0.012 + k + idx) * 40 * (0.5 + R() * 0.2);
      x ? g.lineTo(x, y) : g.moveTo(x, y); } g.stroke(); } }
    else if (style === 2) { for (let y = 0; y < H; y += 24) for (let x = 0; x < W; x += 24) {
      const d = Math.hypot(x - W / 2, y - H / 2) / (W / 2); g.globalAlpha = 0.9; g.fillStyle = PAL[2 + (idx % 2)];
      g.beginPath(); g.arc(x + 12, y + 12, Math.max(1, 11 * (1 - d) + R() * 2), 0, 7); g.fill(); } }
    else { for (let i = 0; i < 12; i++) { g.globalAlpha = 0.8; g.strokeStyle = PAL[1 + i % 4]; g.lineWidth = 5;
      g.beginPath(); g.arc(W * 0.5, H * 0.55, 20 + i * 24, Math.PI * (0.1 * i), Math.PI * (1 + 0.1 * i)); g.stroke(); } }
    g.globalAlpha = 1; return srgb(new THREE.CanvasTexture(cv));
  }
  function makeSign(w, h) { // papan judul pameran
    const cv = document.createElement('canvas'); cv.width = 1024; cv.height = Math.round(1024 * h / w);
    const g = cv.getContext('2d'); g.fillStyle = '#F6E6D6'; g.fillRect(0, 0, 1024, cv.height);
    g.fillStyle = '#6B6D43'; g.textAlign = 'center'; g.font = 'bold 84px Georgia, serif'; g.fillText('PAMERAN DIGITAL', 512, cv.height * 0.5);
    g.fillStyle = '#CF7D65'; g.fillRect(312, cv.height * 0.58, 400, 5);
    g.fillStyle = '#6B6D43'; g.font = '36px Georgia, serif'; g.fillText('Koleksi seni generatif', 512, cv.height * 0.8);
    return srgb(new THREE.CanvasTexture(cv));
  }

  const TITLES = ['Aliran Senja', 'Gelombang Pasir', 'Halftone Mawar', 'Orbit Sunyi', 'Kabut Pagi', 'Arus Tenang',
                  'Titik Cahaya', 'Cincin Waktu', 'Bunga Digital', 'Riak Kopi', 'Pola Beludru', 'Pusaran Susu'];
  const DESCS = ['Lingkaran transparan bertumpuk membentuk senja yang lembut.', 'Garis gelombang berlapis dari palet tanah dan mawar.',
    'Titik halftone yang mengecil menjauhi pusat kanvas.', 'Busur-busur konsentris mengorbit satu titik tenang.',
    'Gradasi susu dan oat dengan bayangan lingkaran samar.', 'Aliran garis sinusoidal yang bergerak perlahan.',
    'Matriks titik cahaya dengan variasi acak halus.', 'Cincin berlapis seperti lingkaran tahun pada kayu.',
    'Bentuk bunga abstrak dari lingkaran dan warna dusty rose.', 'Riak halus dari warna cokelat kopi.',
    'Tekstur titik berlapis menyerupai kain beludru.', 'Pusaran krem yang larut dalam kegelapan espresso.'];

  // ---- Penempatan bingkai: (pusat busur, radius, sudut) menghadap pusat busur ----
  const srgb = t => { t.encoding = THREE.sRGBEncoding; t.anisotropy = 8; return t; };
  const artMat = tx => new THREE.MeshStandardMaterial({ map: tx, emissive: 0xffffff, emissiveMap: tx, emissiveIntensity: 0.22, roughness: 0.35 });
  const wallSlots = []; let n = 0;
  function place(cx, cz, r, deg, w, h, kind) {
    const a = rad(deg), nx = -Math.cos(a), nz = -Math.sin(a), rotY = Math.atan2(nx, nz);
    const px = cx + r * Math.cos(a), pz = cz + r * Math.sin(a);
    wallSlots.push({ position: { x: px, y: 1.7, z: pz }, rotationY: rotY, width: w, height: h });
    const frame = new THREE.Mesh(new THREE.BoxGeometry(w + 0.12, h + 0.12, 0.05), kind === 'sign' ? mTaupe : mFrame);
    frame.position.set(px + nx * 0.03, 1.7, pz + nz * 0.03); frame.rotation.y = rotY; frame.castShadow = true; group.add(frame);
    const art = new THREE.Mesh(new THREE.PlaneGeometry(w, h),
      artMat(kind === 'sign' ? makeSign(w, h) : makeArt(n, w, h)));
    art.position.set(px + nx * 0.06, 1.7, pz + nz * 0.06); art.rotation.y = rotY; group.add(art);
    if (kind === 'sign') { art.userData = { title: 'Selamat Datang', desc: 'Jelajahi 14 karya seni digital generatif. Semua karya dibuat prosedural lewat canvas.', no: 'PAPAN JUDUL' }; }
    else { art.userData = { title: TITLES[n % 12] + (n > 11 ? ' II' : ''), desc: DESCS[n % 12], no: 'KARYA ' + String(n + 1).padStart(2, '0') + ' / 14' }; n++; }
    artworks.push(art);
    art.userData.fi = frames.length;
    frames.push({ art, box: frame, w, h, kind, base: art.material.map, meta: Object.assign({}, art.userData), custom: false, scaleK: 1, aspect: null, dataURL: null, colorHex: null });
  }
  [150, 180, 210, 240, 300, 330, 0, 30].forEach(d => place(0, 0, 14.68, d, 2.4, 1.5, 'art'));       // dinding luar
  place(0, 0, 14.68, 270, 4.0, 1.5, 'sign');
  [190, 215, 240].forEach(d => place(0, 5, 4.73, d, 1.8, 1.1, 'art'));                    // dalam busur kiri
  [20, 45, 70].forEach(d => place(0, -5, 4.73, d, 1.8, 1.1, 'art'));                        // dalam busur kanan

  // ---- Pencahayaan ----
  group.add(new THREE.HemisphereLight(0xfff4ea, 0x7A6258, 0.3));
  // spotlight plafon (bershadow lembut)
  [[0, 3.4, 11, 0, 0, 5], [-9, 3.4, -3, -9.5, 0, -3], [9, 3.4, -3, 9.5, 0, -3], [0, 3.4, -8, 0, 0, -6], [0, 3.4, 1, 0, 0, 0]].forEach(([x, y, z, tx, ty, tz]) => {
    const sp = new THREE.SpotLight(0xffe6cc, 0.9, 30, Math.PI / 2.4, 0.85, 1.2);
    sp.position.set(x, y, z); sp.target.position.set(tx, ty, tz); group.add(sp, sp.target);
    sp.castShadow = true; sp.shadow.mapSize.set(1024, 1024); sp.shadow.bias = -0.0006; sp.shadow.camera.near = 0.5; sp.shadow.camera.far = 30; });
  [[-10, 2.6, 7], [10, 2.6, 7], [-10, 2.6, -8], [10, 2.6, -8], [0, 2.6, -12]].forEach(p => {   // fill hangat tanpa bayangan
    const l = new THREE.PointLight(0xffe2c4, 0.35, 10, 1.6); l.position.set(...p); group.add(l); });

  scene.add(group);
  // ---- API foto: pasang gambar ke bingkai (ukuran bingkai menyesuaikan rasio foto) ----
  // src: URL / objectURL / dataURL. Contoh: room.setFrameImage(0, 'foto.jpg', 'Judul')
  function setFrameImage(i, src, title) {
    const f = frames[i]; if (!f) return Promise.resolve(false);
    return new Promise(resolve => {
      const img = new Image();
      img.onerror = () => resolve(false);
      img.onload = () => {
        const k = Math.min(1, 1600 / Math.max(img.width, img.height));   // batasi ukuran texture
        const cv = document.createElement('canvas');
        cv.width = Math.round(img.width * k); cv.height = Math.round(img.height * k);
        cv.getContext('2d').drawImage(img, 0, 0, cv.width, cv.height);
        const tex = srgb(new THREE.CanvasTexture(cv));
        const old = f.art.material.map; if (old && old !== f.base) old.dispose();
        f.art.material.map = f.art.material.emissiveMap = tex; f.art.material.needsUpdate = true;
        // fit foto ke dalam slot tanpa terpotong / melar
        f.aspect = img.width / img.height; f.dataURL = cv.toDataURL('image/jpeg', 0.85); fitFrame(i);
        f.art.userData.title = title || 'Foto Baru';
        f.art.userData.desc = 'Foto yang ditambahkan pengunjung.';
        f.custom = true; resolve(true);
      };
      img.src = src;
    });
  }
  // kembalikan bingkai ke karya bawaan
  function resetFrame(i) {
    const f = frames[i]; if (!f) return;
    const old = f.art.material.map; if (old && old !== f.base) old.dispose();
    f.art.material.map = f.art.material.emissiveMap = f.base; f.art.material.needsUpdate = true;
    f.aspect = null; f.dataURL = null; f.scaleK = 1; fitFrame(i);
    Object.assign(f.art.userData, f.meta, { year: '', link: '' }); f.custom = false;
  }

  // ---- API editor ----
  function fitFrame(i) {   // sesuaikan ukuran bingkai: rasio foto (atau slot) x skala pengguna
    const f = frames[i], ar = f.aspect || f.w / f.h;
    const nw = (ar > f.w / f.h ? f.w : f.h * ar) * f.scaleK, nh = (ar > f.w / f.h ? f.w / ar : f.h) * f.scaleK;
    f.art.scale.set(nw / f.w, nh / f.h, 1); f.box.scale.set((nw + 0.12) / (f.w + 0.12), (nh + 0.12) / (f.h + 0.12), 1);
  }
  function setFrameColor(i, hex) {
    const f = frames[i]; if (!f) return; f.colorHex = hex; f.box.material = new THREE.MeshStandardMaterial({ color: hex, roughness: 0.5 });
  }
  function swapFrames(i, j) {   // tukar isi dua bingkai (foto, teks, ukuran)
    const a = frames[i], b = frames[j]; if (!a || !b) return;
    ['aspect', 'dataURL', 'custom', 'scaleK'].forEach(k => { [a[k], b[k]] = [b[k], a[k]]; });
    [a.art.material.map, b.art.material.map] = [b.art.material.map, a.art.material.map];
    a.art.material.emissiveMap = a.art.material.map; b.art.material.emissiveMap = b.art.material.map;
    a.art.material.needsUpdate = b.art.material.needsUpdate = true;
    ['title', 'desc', 'year', 'link'].forEach(k => { [a.art.userData[k], b.art.userData[k]] = [b.art.userData[k], a.art.userData[k]]; });
    fitFrame(i); fitFrame(j);
  }
  const THEMES = {   // wall: dinding luar, accent: dinding S, oat: bench & rumah cincin, base: baseboard, cap: tutup pedestal & pot
    bunga: { wall: 0xF6E6D6, oat: 0xE1B8A2, floor: 0xB5AB86, accent: 0x99B4AA, base: 0x6B6D43, cap: 0xCF7D65, frame: 0x6B6D43, runner: 0xE1B8A2 },
    milk:  { wall: 0xFBF7F4, oat: 0xE8D8CE, floor: 0x80706a, accent: 0xFBF7F4, base: 0x7A6258, cap: 0xB98F88, frame: 0x3B2F2A, runner: 0xd8c2b6 },
    blush: { wall: 0xF4E4DD, oat: 0xE3C9BE, floor: 0x8f736c, accent: 0xF4E4DD, base: 0x7A6258, cap: 0xB98F88, frame: 0x3B2F2A, runner: 0xd8c2b6 },
    cocoa: { wall: 0xD9C6BC, oat: 0xB99F94, floor: 0x5a4640, accent: 0xD9C6BC, base: 0x3B2F2A, cap: 0xB98F88, frame: 0x3B2F2A, runner: 0xB99F94 },
    dark:  { wall: 0x8a766d, oat: 0x6d5a52, floor: 0x3b2f2a, accent: 0x8a766d, base: 0x3B2F2A, cap: 0xB98F88, frame: 0xE8D8CE, runner: 0x6d5a52 } };
  function setTheme(nm) {
    const t = THEMES[nm] || THEMES.bunga;
    mWall.color.setHex(t.wall); mOat.color.setHex(t.oat); mFloor.color.setHex(t.floor); mSage.color.setHex(t.accent);
    mTaupe.color.setHex(t.base); mRose.color.setHex(t.cap); mFrame.color.setHex(t.frame); mRunner.color.setHex(t.runner);
  }
  setTheme('bunga');

  return { group, spawnPoint: { x: 0, y: 1.6, z: 19 }, wallSlots, pedestals, colliders, artworks, spinners, frames, setFrameImage, resetFrame, fitFrame, setFrameColor, swapFrames, setTheme, sway };
}

/* ============================================================
   Demo / jelajah: kamera orang pertama, tabrakan, info karya
   ============================================================ */
const $ = id => document.getElementById(id);

const scene = new THREE.Scene();
scene.background = new THREE.Color(0x3d3f2a);
scene.fog = new THREE.Fog(0x3d3f2a, 30, 60);
const camera = new THREE.PerspectiveCamera(70, innerWidth / innerHeight, 0.1, 100);
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setPixelRatio(Math.min(devicePixelRatio, 2));
renderer.setSize(innerWidth, innerHeight);
renderer.toneMapping = THREE.ACESFilmicToneMapping;
renderer.toneMappingExposure = 1.15;
renderer.shadowMap.enabled = true;
renderer.shadowMap.type = THREE.PCFSoftShadowMap;
renderer.outputEncoding = THREE.sRGBEncoding;
document.body.appendChild(renderer.domElement);

// Environment map prosedural (studio hangat) untuk pantulan & cahaya ambient yang lebih nyata
const envScene = new THREE.Scene();
envScene.add(new THREE.Mesh(new THREE.BoxGeometry(40, 14, 40), new THREE.MeshBasicMaterial({ color: 0xcdbfb5, side: THREE.BackSide })));
[[0, 6.8, 0, 14, 0.2, 14, 3.5], [-12, 3, 0, 0.2, 5, 14, 1.6], [12, 3, 0, 0.2, 5, 14, 1.6], [0, 3, -12, 14, 5, 0.2, 1.6]].forEach(([x, y, z, w, h, d, i]) => {
  const m = new THREE.Mesh(new THREE.BoxGeometry(w, h, d), new THREE.MeshBasicMaterial({ color: new THREE.Color(i, i * 0.95, i * 0.88) }));
  m.position.set(x, y, z); envScene.add(m); });
scene.environment = new THREE.PMREMGenerator(renderer).fromScene(envScene, 0.05).texture;

const room = buildExhibitionRoom(scene);
room.group.traverse(o => { if (o.material && o.material.envMapIntensity !== undefined) o.material.envMapIntensity = 0.55; });
const pos = new THREE.Vector3(room.spawnPoint.x, room.spawnPoint.y, room.spawnPoint.z);
let yaw = 0, pitch = 0, locked = false, drag = false, lx = 0, ly = 0;
const keys = {};
const startEl = document.getElementById('start'), infoEl = document.getElementById('info');

startEl.addEventListener('click', () => { startEl.style.display = 'none'; renderer.domElement.requestPointerLock && renderer.domElement.requestPointerLock(); });
document.addEventListener('pointerlockchange', () => {
  locked = document.pointerLockElement === renderer.domElement;
  if (!locked && !drag && !edit) startEl.style.display = 'flex'; });
function look(dx, dy) { yaw -= dx * 0.0022; pitch = Math.max(-1.2, Math.min(1.2, pitch - dy * 0.0022)); }
document.addEventListener('mousemove', e => { if (locked) look(e.movementX, e.movementY); else if (drag) { edit ? orbit(e.clientX - lx, e.clientY - ly) : look(e.clientX - lx, e.clientY - ly); lx = e.clientX; ly = e.clientY; } });
renderer.domElement.addEventListener('pointerdown', e => { if (!locked) { drag = true; lx = e.clientX; ly = e.clientY; } });
addEventListener('pointerup', () => drag = false);
addEventListener('keydown', e => keys[e.code] = true);
addEventListener('keyup', e => keys[e.code] = false);
addEventListener('resize', () => { camera.aspect = innerWidth / innerHeight; camera.updateProjectionMatrix(); renderer.setSize(innerWidth, innerHeight); });

const ray = new THREE.Raycaster(), center = new THREE.Vector2(0, 0);
let clock = new THREE.Clock(), shown = null;

/* ---- Upload foto ke bingkai ---- */
const picker = document.getElementById('picker');
let pickTarget = -1, moved = false;
renderer.domElement.addEventListener('pointerdown', () => moved = false);
document.addEventListener('mousemove', () => { if (drag) moved = true; });

function openPicker(idx) {
  if (VIEW) return;            // idx >= 0: satu bingkai; -1: banyak foto sekaligus
  pickTarget = idx; picker.multiple = idx < 0; picker.value = '';
  if (document.pointerLockElement) document.exitPointerLock();
  picker.click();
}
async function addFiles(fileList, idx) {
  if (VIEW) return;
  const files = [...fileList].filter(f => f && f.type.startsWith('image/'));
  if (!files.length) return;
  let i = idx;
  for (const file of files) {
    if (idx < 0) {                    // isi bingkai kosong berikutnya (papan judul dilewati)
      i = room.frames.findIndex(f => !f.custom && f.kind !== 'sign');
      if (i < 0) { alert('Semua bingkai sudah terisi foto. Arahkan ke bingkai lalu tekan E untuk mengganti.'); break; }
    }
    const url = URL.createObjectURL(file);
    await room.setFrameImage(i, url, file.name.replace(/\.[^.]+$/, ''));
    URL.revokeObjectURL(url);
    if (idx >= 0) break;
  }
  shown = null; if (edit) select(sel);
}
picker.addEventListener('change', () => addFiles(pickTarget >= 0 ? [picker.files[0]] : picker.files, pickTarget));
document.getElementById('addBtn').addEventListener('click', e => { e.stopPropagation(); openPicker(-1); });
renderer.domElement.addEventListener('click', () => { if (!edit && shown && (locked || !moved)) openPicker(shown.userData.fi); });
addEventListener('dragover', e => e.preventDefault());
addEventListener('drop', e => {       // seret file gambar ke layar
  e.preventDefault();
  const fs = e.dataTransfer.files; if (!fs.length) return;
  shown ? addFiles([fs[0]], shown.userData.fi) : addFiles(fs, -1);
});
addEventListener('keydown', e => {
  if (/INPUT|TEXTAREA|SELECT/.test(e.target.tagName)) return;
  if (e.code === 'KeyU') openPicker(-1);
  if (!shown || VIEW) return;
  const u = shown.userData, i = u.fi;
  if (e.code === 'KeyE') openPicker(i);
  if (e.code === 'KeyR') { room.resetFrame(i); shown = null; }
  if (e.code === 'KeyT') {
    if (document.pointerLockElement) document.exitPointerLock();
    const t = prompt('Judul karya:', u.title); if (t) u.title = t;
    const d = prompt('Deskripsi:', u.desc); if (d) u.desc = d;
    shown = null;
  }
});

/* ---- Editor (mode edit, pilih bingkai, tema, simpan/muat) ---- */
const VIEW = new URLSearchParams(location.search).has('view');
let edit = false, oAz = 0.6, oPol = 0.95, oDist = 34, sel = -1;
const frameSelect = $('frameSelect');
populateFrameSelect();

function populateFrameSelect() {
  frameSelect.innerHTML = '<option value="-1">-- Pilih karya --</option>';

  room.frames.forEach((f, i) => {
    if (f.kind === 'sign') return;

    const option = document.createElement('option');
    option.value = i;

    const title = f.art.userData.title || `Karya ${i + 1}`;
    option.textContent = `${f.art.userData.no || 'KARYA'} — ${title}`;

    frameSelect.appendChild(option);
  });
}

frameSelect.addEventListener('change', e => {
  const i = Number(e.target.value);

  if (i < 0) {
    select(-1);
    return;
  }

  select(i);
});
const isField = e => /INPUT|TEXTAREA|SELECT/.test(e.target.tagName);
function orbit(dx, dy) { oAz -= dx * 0.005; oPol = Math.max(0.2, Math.min(1.45, oPol - dy * 0.005)); }
renderer.domElement.addEventListener('wheel', e => { if (edit) { e.preventDefault(); oDist = Math.max(8, Math.min(60, oDist + e.deltaY * 0.03)); } }, { passive: false });

const hl = new THREE.LineSegments(new THREE.EdgesGeometry(new THREE.BoxGeometry(1, 1, 1)), new THREE.LineBasicMaterial({ color: 0xff7a9a, depthTest: false }));
hl.visible = false; hl.renderOrder = 10; scene.add(hl);
function syncSel() {
  const f = room.frames[sel]; hl.visible = edit && !!f; if (!hl.visible) return;
  const b = f.box; hl.position.copy(b.position); hl.rotation.copy(b.rotation);
  hl.scale.set((f.w + 0.12) * b.scale.x + 0.15, (f.h + 0.12) * b.scale.y + 0.15, 0.2);
}
function select(i) {
  sel = i;

  const f = room.frames[i];

  $('noSel').style.display = f ? 'none' : 'block';
  $('selForm').style.display = f ? 'block' : 'none';

  frameSelect.value = f ? i : '-1';

  if (!f) return;

  const u = f.art.userData;

  $('fTitle').value = u.title || '';
  $('fYear').value = u.year || '';
  $('fDesc').value = u.desc || '';
  $('fLink').value = u.link || '';

  $('fScale').value = f.scaleK;
  $('fColor').value = f.colorHex || '#6b6d43';
  $('selName').textContent = u.no || `KARYA ${i + 1}`;

  syncSel();
}
function setMode(on) {
  edit = on; $('editor').style.display = on ? 'block' : 'none'; $('toolbar').classList.toggle('shift', on);
  $('modeBtn').textContent = on ? 'MODE JELAJAH' : 'MODE EDIT'; $('dot').style.display = on ? 'none' : 'block';
  if (on) { startEl.style.display = 'none'; if (document.pointerLockElement) document.exitPointerLock(); }
  else { select(-1); startEl.style.display = 'flex'; }
}
$('modeBtn').onclick = e => { e.stopPropagation(); setMode(!edit); };
$('goEdit').onclick = e => { e.stopPropagation(); setMode(true); };
addEventListener('keydown', e => {
  if (e.code === 'Tab' && !VIEW && !isField(e)) { e.preventDefault(); setMode(!edit); }
  if (e.code === 'KeyL' && !isField(e) && shown && shown.userData.link) window.open(shown.userData.link, '_blank');
});
// klik bingkai di mode edit
renderer.domElement.addEventListener('click', e => {
  if (!edit || moved) return;
  const r = renderer.domElement.getBoundingClientRect();
  ray.setFromCamera(new THREE.Vector2((e.clientX - r.left) / r.width * 2 - 1, -(e.clientY - r.top) / r.height * 2 + 1), camera);
  const h = ray.intersectObjects(room.group.children, true)[0]; if (!h) return select(-1);
  const o = h.object, i = o.userData.fi !== undefined ? o.userData.fi : room.frames.findIndex(f => f.box === o);
  select(i);
});
// kolom edit
[['fTitle', 'title'], ['fYear', 'year'], ['fDesc', 'desc'], ['fLink', 'link']].forEach(([id, k]) =>
  $(id).addEventListener('input', e => { const f = room.frames[sel]; if (f) f.art.userData[k] = e.target.value; }));
$('fScale').addEventListener('input', e => { const f = room.frames[sel]; if (f) { f.scaleK = +e.target.value; room.fitFrame(sel); } });
$('fColor').addEventListener('input', e => room.setFrameColor(sel, e.target.value));
$('bPhoto').onclick = () => openPicker(sel);
$('bBulk').onclick = () => openPicker(-1);
$('bReset').onclick = () => { room.resetFrame(sel); select(sel); };
function swap(d) {
  const F = room.frames, n = F.length; if (!F[sel] || F[sel].kind === 'sign') return;
  let j = sel; do { j = (j + d + n) % n; } while (F[j].kind === 'sign');
  room.swapFrames(sel, j); select(j);
}
$('bPrev').onclick = () => swap(-1); $('bNext').onclick = () => swap(1);
// tema & cahaya
const lights = []; room.group.traverse(o => { if (o.isLight) lights.push([o, o.intensity]); });
let theme = 'bunga';
document.querySelectorAll('[data-theme]').forEach(b => b.onclick = () => { theme = b.dataset.theme; room.setTheme(theme); });
$('lightRange').addEventListener('input', e => lights.forEach(([l, b]) => l.intensity = b * e.target.value));
// simpan / muat JSON
function exportData() {
  return { v: 1, theme, light: +$('lightRange').value, frames: room.frames.map(f => { const u = f.art.userData;
    return { title: u.title, desc: u.desc, year: u.year || '', link: u.link || '', scaleK: f.scaleK, color: f.colorHex, img: f.dataURL }; }) };
}
async function importData(d) {
  theme = d.theme || 'milk'; room.setTheme(theme); $('lightRange').value = d.light || 1; $('lightRange').dispatchEvent(new Event('input'));
  for (let i = 0; i < d.frames.length && i < room.frames.length; i++) {
    const x = d.frames[i], f = room.frames[i];
    if (x.img) await room.setFrameImage(i, x.img, x.title);
    Object.assign(f.art.userData, { title: x.title, desc: x.desc, year: x.year, link: x.link });
    f.scaleK = x.scaleK || 1; room.fitFrame(i); if (x.color) room.setFrameColor(i, x.color);
  }
  shown = null; if (edit) select(sel);
}
$('bSave').onclick = () => { const a = document.createElement('a');
  a.href = URL.createObjectURL(new Blob([JSON.stringify(exportData())], { type: 'application/json' })); a.download = 'exhibition.json'; a.click(); };
$('bLoad').onclick = () => $('jsonPicker').click();
$('jsonPicker').onchange = e => { const fl = e.target.files[0]; if (fl) fl.text().then(t => importData(JSON.parse(t))); };
// muat otomatis exhibition.json (saat di-hosting); ?view=1 = mode pengunjung tanpa editor
fetch('exhibition.json').then(r => r.ok ? r.json() : null).then(d => d && importData(d)).catch(() => {});
if (VIEW) ['toolbar', 'editor', 'goEdit'].forEach(id => $(id).remove());

(function animate() {
  requestAnimationFrame(animate);
  const dt = Math.min(clock.getDelta(), 0.05), t = clock.elapsedTime;
  // gerak: maju/mundur & geser relatif arah pandang
  const f = (keys.KeyW || keys.ArrowUp ? 1 : 0) - (keys.KeyS || keys.ArrowDown ? 1 : 0);
  const s = (keys.KeyD ? 1 : 0) - (keys.KeyA ? 1 : 0);
  if (keys.ArrowLeft) yaw += 1.6 * dt; if (keys.ArrowRight) yaw -= 1.6 * dt;
  const sp = (keys.ShiftLeft ? 5 : 3) * dt * (edit ? 0 : 1);
  pos.x += (-Math.sin(yaw) * f + Math.cos(yaw) * s) * sp;
  pos.z += (-Math.cos(yaw) * f - Math.sin(yaw) * s) * sp;
  // tabrakan: dorong keluar dari titik penghalang
  for (let it = 0; it < 2; it++) for (const c of room.colliders) {
    const dx = pos.x - c.x, dz = pos.z - c.z, rr = c.r + 0.3, d2 = dx * dx + dz * dz;
    if (d2 < rr * rr && d2 > 1e-6) { const d = Math.sqrt(d2); pos.x = c.x + dx / d * rr; pos.z = c.z + dz / d * rr; } }
  const pr = Math.hypot(pos.x, pos.z); if (pr > 21) { pos.x *= 21 / pr; pos.z *= 21 / pr; }
  if (edit) { camera.position.set(oDist * Math.sin(oPol) * Math.sin(oAz), 1.5 + oDist * Math.cos(oPol), oDist * Math.sin(oPol) * Math.cos(oAz)); camera.lookAt(0, 1.5, 0); }
  else { camera.position.copy(pos); camera.position.y = 1.6; camera.rotation.set(pitch, yaw, 0, 'YXZ'); }
  room.sway.forEach((g, i) => { g.rotation.z = Math.sin(t * 0.7 + i) * 0.012; g.rotation.x = Math.cos(t * 0.5 + i) * 0.01; });
  room.spinners.forEach((o, i) => { o.rotation.y = t * 0.6 + i; o.rotation.x = t * 0.3; o.position.y = 1.55 + Math.sin(t * 1.2 + i) * 0.05; });
  // karya yang sedang dilihat
  ray.setFromCamera(center, camera);
  const hit = ray.intersectObjects(room.artworks)[0];
  const cur = !edit && hit && hit.distance < 7 ? hit.object : null;
  if (cur !== shown) { shown = cur;
    if (cur) { const u = cur.userData; infoEl.querySelector('h2').textContent = u.title + (u.year ? ' · ' + u.year : ''); infoEl.querySelector('p').textContent = u.desc; infoEl.querySelector('small').textContent = u.no + (VIEW ? '' : '  ·  E: ganti foto  ·  R: reset  ·  T: ubah teks') + (u.link ? '  ·  L: buka tautan' : ''); }
    infoEl.classList.toggle('on', !!cur); }
  syncSel();
  renderer.render(scene, camera);
})();
</script>
</body>
</html>
