<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Fancy Text / Unicode Generator</title>
  <style>
    :root { --bg-color:#ffffff; --text-color:#0b1220; --btn-bg:#f8fafc; --btn-border:#cbd5e1; }
    body { max-width:900px; margin:28px auto; padding:20px; color:var(--text-color); background-color:var(--bg-color); font-family:system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial; transition:0.3s; }
    h1 { text-align:center; font-size:24px; margin-bottom:12px; }
    p { text-align:center; color:#334; }
    textarea { width:100%; min-height:120px; padding:10px; font-size:16px; border-radius:8px; border:1px solid #cbd5e1; margin-bottom:10px; }
    .row, .settings { display:flex; flex-wrap:wrap; gap:10px; justify-content:center; margin-top:10px; }
    button, select { padding:8px 12px; border-radius:8px; border:1px solid var(--btn-border); background:var(--btn-bg); cursor:pointer; transition:0.2s; }
    button:hover, select:hover { transform:scale(1.05); background:#e2e8f0; }
    .output { white-space:pre-wrap; word-break:break-word; padding:12px; border-radius:8px; border:1px dashed #cbd5e1; min-height:80px; margin-top:10px; background:#f4f4f4; font-size:18px; }
  </style>
</head>
<body>
  <h1>Fancy Text / Unicode Generator</h1>
  <p>Type your text below and see it transformed. Change the theme or font.</p>

  <div class="settings">
    <button id="toggleTheme">Toggle Theme (Light/Dark)</button>
    <select id="fontSelect"></select>
  </div>

  <textarea id="input">Type your text here...</textarea>

  <div class="row">
    <button id="copy">Copy</button>
    <button id="clear">Clear</button>
  </div>

  <div class="output" id="output"></div>

  <script>
    const fontMaps = {
      smallCaps: {'A':'ᴀ','B':'ʙ','C':'ᴄ','D':'ᴅ','E':'ᴇ','F':'ꜰ','G':'ɢ','H':'ʜ','I':'ɪ','J':'ᴊ','K':'ᴋ','L':'ʟ','M':'ᴍ','N':'ɴ','O':'ᴏ','P':'ᴘ','Q':'q','R':'ʀ','S':'s','T':'ᴛ','U':'ᴜ','V':'ᴠ','W':'ᴡ','X':'x','Y':'ʏ','Z':'ᴢ','a':'ᴀ','b':'ʙ','c':'ᴄ','d':'ᴅ','e':'ᴇ','f':'ꜰ','g':'ɢ','h':'ʜ','i':'ɪ','j':'ᴊ','k':'ᴋ','l':'ʟ','m':'ᴍ','n':'ɴ','o':'ᴏ','p':'ᴘ','q':'q','r':'ʀ','s':'s','t':'ᴛ','u':'ᴜ','v':'ᴠ','w':'ᴡ','x':'x','y':'ʏ','z':'ᴢ'},
      bold: {'A':'𝗔','B':'𝗕','C':'𝗖','D':'𝗗','E':'𝗘','F':'𝗙','G':'𝗚','H':'𝗛','I':'𝗜','J':'𝗝','K':'𝗞','L':'𝗟','M':'𝗠','N':'𝗡','O':'𝗢','P':'𝗣','Q':'𝗤','R':'𝗥','S':'𝗦','T':'𝗧','U':'𝗨','V':'𝗩','W':'𝗪','X':'𝗫','Y':'𝗬','Z':'𝗭','a':'𝗮','b':'𝗯','c':'𝗰','d':'𝗱','e':'𝗲','f':'𝗳','g':'𝗴','h':'𝗵','i':'𝗶','j':'𝗷','k':'𝗸','l':'𝗹','m':'𝗺','n':'𝗻','o':'𝗼','p':'𝗽','q':'𝗾','r':'𝗿','s':'𝘀','t':'𝘁','u':'𝘂','v':'𝘃','w':'𝘄','x':'𝘅','y':'𝘆','z':'𝘇'},
      italic: {'A':'𝐴','B':'𝐵','C':'𝐶','D':'𝐷','E':'𝐸','F':'𝐹','G':'𝐺','H':'𝐻','I':'𝐼','J':'𝐽','K':'𝐾','L':'𝐿','M':'𝑀','N':'𝑁','O':'𝑂','P':'𝑃','Q':'𝑄','R':'𝑅','S':'𝑆','T':'𝑇','U':'𝑈','V':'𝑉','W':'𝑊','X':'𝑋','Y':'𝑌','Z':'𝑍','a':'𝑎','b':'𝑏','c':'𝑐','d':'𝑑','e':'𝑒','f':'𝑓','g':'𝑔','h':'ℎ','i':'𝑖','j':'𝑗','k':'𝑘','l':'𝑙','m':'𝑚','n':'𝑛','o':'𝑜','p':'𝑝','q':'𝑞','r':'𝑟','s':'𝑠','t':'𝑡','u':'𝑢','v':'𝑣','w':'𝑤','x':'𝑥','y':'𝑦','z':'𝑧'},
      bubble: {'A':'Ⓐ','B':'Ⓑ','C':'Ⓒ','D':'Ⓓ','E':'Ⓔ','F':'Ⓕ','G':'Ⓖ','H':'Ⓗ','I':'Ⓘ','J':'Ⓙ','K':'Ⓚ','L':'Ⓛ','M':'Ⓜ','N':'Ⓝ','O':'Ⓞ','P':'Ⓟ','Q':'Ⓠ','R':'Ⓡ','S':'Ⓢ','T':'Ⓣ','U':'Ⓤ','V':'Ⓥ','W':'Ⓦ','X':'Ⓧ','Y':'Ⓨ','Z':'Ⓩ','a':'ⓐ','b':'ⓑ','c':'ⓒ','d':'ⓓ','e':'ⓔ','f':'ⓕ','g':'ⓖ','h':'ⓗ','i':'ⓘ','j':'ⓙ','k':'ⓚ','l':'ⓛ','m':'ⓜ','n':'ⓝ','o':'ⓞ','p':'ⓟ','q':'ⓠ','r':'ⓡ','s':'ⓢ','t':'ⓣ','u':'ⓤ','v':'ⓥ','w':'ⓦ','x':'ⓧ','y':'ⓨ','z':'ⓩ'},
      monospace: {'A':'𝙰','B':'𝙱','C':'𝙲','D':'𝙳','E':'𝙴','F':'𝙵','G':'𝙶','H':'𝙷','I':'𝙸','J':'𝙹','K':'𝙺','L':'𝙻','M':'𝙼','N':'𝙽','O':'𝙾','P':'𝙿','Q':'𝚀','R':'𝚁','S':'𝚂','T':'𝚃','U':'𝚄','V':'𝚅','W':'𝚆','X':'𝚇','Y':'𝚈','Z':'𝚉','a':'𝚊','b':'𝚋','c':'𝚌','d':'𝚍','e':'𝚎','f':'𝚏','g':'𝚐','h':'𝚑','i':'𝚒','j':'𝚓','k':'𝚔','l':'𝚕','m':'𝚖','n':'𝚗','o':'𝚘','p':'𝚙','q':'𝚚','r':'𝚛','s':'𝚜','t':'𝚝','u':'𝚞','v':'𝚟','w':'𝚠','x':'𝚡','y':'𝚢','z':'𝚣'},
      squared: {'A':'🄰','B':'🄱','C':'🄲','D':'🄳','E':'🄴','F':'🄵','G':'🄶','H':'🄷','I':'🄸','J':'🄹','K':'🄺','L':'🄻','M':'🄼','N':'🄽','O':'🄾','P':'🄿','Q':'🅀','R':'🅁','S':'🅂','T':'🅃','U':'🅄','V':'🅅','W':'🅆','X':'🅇','Y':'🅈','Z':'🅉','a':'🄰','b':'🄱','c':'🄲','d':'🄳','e':'🄴','f':'🄵','g':'🄶','h':'🄷','i':'🄸','j':'🄹','k':'🄺','l':'🄻','m':'🄼','n':'🄽','o':'🄾','p':'🄿','q':'🅀','r':'🅁','s':'🅂','t':'🅃','u':'🅄','v':'🅅','w':'🅆','x':'🅇','y':'🅈','z':'🅉'},
      circled: {'A':'Ⓐ','B':'Ⓑ','C':'Ⓒ','D':'Ⓓ','E':'Ⓔ','F':'Ⓕ','G':'Ⓖ','H':'Ⓗ','I':'Ⓘ','J':'Ⓙ','K':'Ⓚ','L':'Ⓛ','M':'Ⓜ','N':'Ⓝ','O':'Ⓞ','P':'Ⓟ','Q':'Ⓠ','R':'Ⓡ','S':'Ⓢ','T':'Ⓣ','U':'Ⓤ','V':'Ⓥ','W':'Ⓦ','X':'Ⓧ','Y':'Ⓨ','Z':'Ⓩ','a':'ⓐ','b':'ⓑ','c':'ⓒ','d':'ⓓ','e':'ⓔ','f':'ⓕ','g':'ⓖ','h':'ⓗ','i':'ⓘ','j':'ⓙ','k':'ⓚ','l':'ⓛ','m':'ⓜ','n':'ⓝ','o':'ⓞ','p':'ⓟ','q':'ⓠ','r':'ⓡ','s':'ⓢ','t':'ⓣ','u':'ⓤ','v':'ⓥ','w':'ⓦ','x':'ⓧ','y':'ⓨ','z':'ⓩ'},
      parenthesized: {'A':'⒜','B':'⒝','C':'⒞','D':'⒟','E':'⒠','F':'⒡','G':'⒢','H':'⒣','I':'⒤','J':'⒥','K':'⒦','L':'⒧','M':'⒨','N':'⒩','O':'⒪','P':'⒫','Q':'⒬','R':'⒭','S':'⒮','T':'⒯','U':'⒰','V':'⒱','W':'⒲','X':'⒳','Y':'⒴','Z':'⒵','a':'⒜','b':'⒝','c':'⒞','d':'⒟','e':'⒠','f':'⒡','g':'⒢','h':'⒣','i':'⒤','j':'⒥','k':'⒦','l':'⒧','m':'⒨','n':'⒩','o':'⒪','p':'⒫','q':'⒬','r':'⒭','s':'⒮','t':'⒯','u':'⒰','v':'⒱','w':'⒲','x':'⒳','y':'⒴','z':'⒵'},
      fraktur: {'A':'𝔄','B':'𝔅','C':'ℭ','D':'𝔇','E':'𝔈','F':'𝔉','G':'𝔊','H':'ℌ','I':'ℑ','J':'𝔍','K':'𝔎','L':'𝔏','M':'𝔐','N':'𝔑','O':'𝔒','P':'𝔓','Q':'𝔔','R':'ℜ','S':'𝔖','T':'𝔗','U':'𝔘','V':'𝔙','W':'𝔚','X':'𝔛','Y':'𝔜','Z':'ℨ','a':'𝔞','b':'𝔟','c':'𝔠','d':'𝔡','e':'𝔢','f':'𝔣','g':'𝔤','h':'𝔥','i':'𝔦','j':'𝔧','k':'𝔨','l':'𝔩','m':'𝔪','n':'𝔫','o':'𝔬','p':'𝔭','q':'𝔮','r':'𝔯','s':'𝔰','t':'𝔱','u':'𝔲','v':'𝔳','w':'𝔴','x':'𝔵','y':'𝔶','z':'𝔷'}
    };

    const fontSelect = document.getElementById('fontSelect');
    for (const f in fontMaps) {
      const opt = document.createElement('option');
      opt.value = f;
      opt.textContent = f;
      fontSelect.appendChild(opt);
    }

    const input = document.getElementById('input');
    const output = document.getElementById('output');

    function convertText(text, font) {
      const map = fontMaps[font] || fontMaps['smallCaps'];
      return Array.from(text).map(c => map[c] || c).join('');
    }

    function updateOutput() {
      const selectedFont = fontSelect.value || 'smallCaps';
      output.textContent = convertText(input.value, selectedFont);
    }

    input.addEventListener('input', updateOutput);
    fontSelect.addEventListener('change', updateOutput);

    document.getElementById('copy').addEventListener('click', async () => {
      try { await navigator.clipboard.writeText(output.textContent); alert('Copied!'); }
      catch(e) { alert('Copy failed.'); }
    });

    document.getElementById('clear').addEventListener('click', () => { input.value=''; updateOutput(); });

    document.getElementById('toggleTheme').addEventListener('click', () => {
      if(document.body.style.backgroundColor==='black'){
        document.body.style.backgroundColor='#ffffff'; document.body.style.color='#0b1220';
      } else {
        document.body.style.backgroundColor='black'; document.body.style.color='white';
      }
    });

    fontSelect.value='smallCaps';
    updateOutput();
  </script>
</body>
</html>
