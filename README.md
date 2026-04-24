<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gemini Prompt Builder — Nectar Life</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Sans:opsz,wght@9..40,300;9..40,400;9..40,500;9..40,600&display=swap" rel="stylesheet">
<style>
  :root {
    --pink: #FCAFC0;
    --hot-pink: #F05380;
    --cream: #FCF4EE;
    --blush: #FFE3EC;
    --charcoal: #2e2020;
    --mid: #7a6060;
    --border: #edd8e0;
    --white: #ffffff;
    --radius: 16px;
    --radius-sm: 10px;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--cream);
    color: var(--charcoal);
    height: 100vh;
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }

  header {
    padding: 16px 24px;
    background: var(--white);
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    gap: 12px;
    flex-shrink: 0;
  }

  .logo-dot {
    width: 32px;
    height: 32px;
    background: linear-gradient(135deg, var(--pink), var(--hot-pink));
    border-radius: 50%;
    flex-shrink: 0;
  }

  header h1 {
    font-family: 'DM Serif Display', serif;
    font-size: 1.1rem;
    color: var(--charcoal);
  }

  header h1 span { color: var(--hot-pink); font-style: italic; }

  header p {
    font-size: 0.75rem;
    color: var(--mid);
    font-weight: 300;
  }

  #chatArea {
    flex: 1;
    overflow-y: auto;
    padding: 24px 16px;
    display: flex;
    flex-direction: column;
    gap: 16px;
    scroll-behavior: smooth;
  }

  .msg {
    display: flex;
    gap: 10px;
    max-width: 640px;
    animation: fadeUp 0.3s ease;
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(8px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .msg.user { align-self: flex-end; flex-direction: row-reverse; }
  .msg.assistant { align-self: flex-start; }

  .avatar {
    width: 28px;
    height: 28px;
    border-radius: 50%;
    flex-shrink: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.7rem;
    font-weight: 600;
    margin-top: 2px;
  }

  .msg.assistant .avatar {
    background: linear-gradient(135deg, var(--pink), var(--hot-pink));
    color: white;
  }

  .msg.user .avatar {
    background: var(--charcoal);
    color: white;
  }

  .bubble {
    padding: 12px 16px;
    border-radius: var(--radius);
    font-size: 0.875rem;
    line-height: 1.6;
    max-width: 520px;
  }

  .msg.assistant .bubble {
    background: var(--white);
    border: 1px solid var(--border);
    border-top-left-radius: 4px;
    color: var(--charcoal);
  }

  .msg.user .bubble {
    background: var(--hot-pink);
    color: white;
    border-top-right-radius: 4px;
  }

  .prompt-output {
    background: var(--charcoal);
    border-radius: var(--radius);
    padding: 16px;
    margin-top: 10px;
    border-top-left-radius: 4px;
    max-width: 520px;
  }

  .prompt-label {
    font-size: 0.65rem;
    font-weight: 600;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: var(--pink);
    margin-bottom: 10px;
  }

  .prompt-text {
    font-size: 0.825rem;
    line-height: 1.7;
    color: #f0e8ec;
    white-space: pre-wrap;
    word-break: break-word;
  }

  .copy-btn {
    margin-top: 12px;
    padding: 7px 14px;
    background: transparent;
    border: 1px solid var(--pink);
    border-radius: 100px;
    color: var(--pink);
    font-family: 'DM Sans', sans-serif;
    font-size: 0.72rem;
    font-weight: 600;
    letter-spacing: 1px;
    text-transform: uppercase;
    cursor: pointer;
    transition: all 0.2s;
  }

  .copy-btn:hover { background: var(--pink); color: var(--charcoal); }
  .copy-btn.copied { background: var(--pink); color: var(--charcoal); }

  .img-preview {
    width: 80px;
    height: 80px;
    object-fit: cover;
    border-radius: var(--radius-sm);
    border: 2px solid rgba(255,255,255,0.3);
  }

  .img-row { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 8px; }

  .typing .bubble {
    display: flex;
    align-items: center;
    gap: 5px;
    padding: 14px 18px;
  }

  .dot {
    width: 7px; height: 7px;
    background: var(--pink);
    border-radius: 50%;
    animation: bounce 1.2s infinite;
  }
  .dot:nth-child(2) { animation-delay: 0.2s; }
  .dot:nth-child(3) { animation-delay: 0.4s; }

  @keyframes bounce {
    0%, 60%, 100% { transform: translateY(0); }
    30% { transform: translateY(-6px); }
  }

  .input-area {
    background: var(--white);
    border-top: 1px solid var(--border);
    padding: 12px 16px;
    flex-shrink: 0;
  }

  .upload-strip {
    display: flex;
    gap: 8px;
    margin-bottom: 10px;
    flex-wrap: wrap;
  }

  .upload-btn {
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 6px 12px;
    border: 1px dashed var(--border);
    border-radius: 100px;
    font-size: 0.75rem;
    font-weight: 500;
    color: var(--mid);
    cursor: pointer;
    background: var(--cream);
    transition: all 0.2s;
    position: relative;
    overflow: hidden;
  }

  .upload-btn:hover { border-color: var(--hot-pink); color: var(--hot-pink); }
  .upload-btn.has-image { border-color: var(--hot-pink); color: var(--hot-pink); background: var(--blush); border-style: solid; }

  .upload-btn input[type="file"] {
    position: absolute;
    inset: 0;
    opacity: 0;
    cursor: pointer;
    font-size: 0;
  }

  .upload-icon { font-size: 0.9rem; }

  .paste-strip {
    display: none;
    gap: 8px;
    margin-bottom: 8px;
    flex-wrap: wrap;
    align-items: center;
  }

  .paste-strip.visible { display: flex; }

  .paste-thumb-wrap {
    position: relative;
    display: inline-flex;
  }

  .paste-thumb {
    width: 48px;
    height: 48px;
    object-fit: cover;
    border-radius: 8px;
    border: 1px solid var(--border);
  }

  .paste-remove {
    position: absolute;
    top: -5px;
    right: -5px;
    width: 16px;
    height: 16px;
    background: var(--charcoal);
    color: white;
    border-radius: 50%;
    font-size: 0.6rem;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    line-height: 1;
  }

  .paste-hint {
    font-size: 0.72rem;
    color: var(--mid);
    font-weight: 300;
  }

  .input-row {
    display: flex;
    gap: 10px;
    align-items: flex-end;
  }

  textarea#userInput {
    flex: 1;
    padding: 10px 14px;
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    font-family: 'DM Sans', sans-serif;
    font-size: 0.875rem;
    color: var(--charcoal);
    background: var(--cream);
    resize: none;
    outline: none;
    line-height: 1.5;
    min-height: 42px;
    max-height: 120px;
    transition: border-color 0.2s;
  }

  textarea#userInput:focus { border-color: var(--hot-pink); background: var(--white); }

  .send-btn {
    width: 42px;
    height: 42px;
    background: var(--hot-pink);
    border: none;
    border-radius: var(--radius-sm);
    color: white;
    font-size: 1rem;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    transition: background 0.2s, transform 0.1s;
  }

  .send-btn:hover { background: #d9406c; }
  .send-btn:active { transform: scale(0.95); }
  .send-btn:disabled { background: var(--border); cursor: not-allowed; }

  .welcome {
    align-self: center;
    text-align: center;
    padding: 40px 20px;
    max-width: 400px;
  }

  .welcome-icon {
    width: 56px;
    height: 56px;
    background: linear-gradient(135deg, var(--pink), var(--hot-pink));
    border-radius: 50%;
    margin: 0 auto 16px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.4rem;
  }

  .welcome h2 {
    font-family: 'DM Serif Display', serif;
    font-size: 1.4rem;
    margin-bottom: 8px;
  }

  .welcome p {
    font-size: 0.85rem;
    color: var(--mid);
    line-height: 1.6;
    font-weight: 300;
  }

  .welcome-steps {
    margin-top: 20px;
    display: flex;
    flex-direction: column;
    gap: 8px;
    text-align: left;
  }

  .step {
    display: flex;
    align-items: flex-start;
    gap: 10px;
    padding: 10px 14px;
    background: var(--white);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    font-size: 0.8rem;
    color: var(--charcoal);
  }

  .step-num {
    width: 20px;
    height: 20px;
    background: var(--hot-pink);
    color: white;
    border-radius: 50%;
    font-size: 0.65rem;
    font-weight: 700;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    margin-top: 1px;
  }
</style>
</head>
<body>

<header>
  <div class="logo-dot"></div>
  <div>
    <h1>Gemini Prompt <span>Builder</span></h1>
    <p>Nectar Life · AI Image Production</p>
  </div>
</header>

<div id="chatArea">
  <div class="welcome" id="welcome">
    <div class="welcome-icon">✨</div>
    <h2>Build your Gemini prompt</h2>
    <p>Upload or paste your reference and product images, then tell me what you want.</p>
    <div class="welcome-steps">
      <div class="step"><div class="step-num">1</div><span>Upload or paste a reference image showing the scene you want</span></div>
      <div class="step"><div class="step-num">2</div><span>Upload or paste the product image</span></div>
      <div class="step"><div class="step-num">3</div><span>Describe what you want — aspect ratio, angle, colors, anything specific</span></div>
      <div class="step"><div class="step-num">4</div><span>Copy the finished prompt into Gemini</span></div>
    </div>
  </div>
</div>

<div class="input-area">
  <div class="upload-strip">
    <label class="upload-btn" id="refLabel">
      <span class="upload-icon">🖼</span>
      <span id="refText">Reference image</span>
      <input type="file" accept="image/*" id="refInput" onchange="handleImage('ref', this)">
    </label>
    <label class="upload-btn" id="prodLabel">
      <span class="upload-icon">📦</span>
      <span id="prodText">Product image</span>
      <input type="file" accept="image/*" id="prodInput" onchange="handleImage('prod', this)">
    </label>
  </div>
  <div class="paste-strip" id="pasteStrip">
    <span class="paste-hint">Pasted:</span>
  </div>
  <div class="input-row">
    <textarea id="userInput" placeholder="Describe what you want — or paste an image with Ctrl+V..." rows="1"
      onkeydown="if(event.key==='Enter'&&!event.shiftKey){event.preventDefault();sendMessage()}"
      oninput="this.style.height='auto';this.style.height=this.scrollHeight+'px'"></textarea>
    <button class="send-btn" onclick="sendMessage()" id="sendBtn">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><line x1="22" y1="2" x2="11" y2="13"></line><polygon points="22 2 15 22 11 13 2 9 22 2"></polygon></svg>
    </button>
  </div>
</div>

<script>
  const SYSTEM_PROMPT = `You are a Gemini image generation prompt specialist for Nectar Life, a handcrafted bath and body brand.

Your job is to write precise, ready-to-use Gemini image prompts based on reference images and product images the user provides, plus their description.

CORE RULES — always apply unless the user explicitly says not to:
- Photorealistic, shot on camera, commercial product photography
- Preserve exact product colors — never interpret or change them
- No label text visible (Gemini will distort it anyway)
- Always ask for aspect ratio if the user hasn't specified one

RULES THE USER CAN TOGGLE — only apply if they mention them or if they're clearly relevant to the scene:
- No water displacement ring / no crater around product base
- No directional shadow — soft shadow wraps evenly all edges
- Foam and bubbles touch product edges naturally
- Bubbles are perfectly round, clear, iridescent — no foam clusters, no soap residue
- Bubbles vary dramatically in size from tiny to very large, distributed randomly
- Product hovers/floats above surface — soft diffused shadow slightly separated from base
- Overhead flat lay — 90 degrees, product fully flat

OUTPUT FORMAT:
- Write the prompt as a single clean paragraph, no bullet points, no headers
- Be specific about what you see in the reference image — scene, lighting, surface, atmosphere
- Be specific about the product — shape, color, texture, packaging details
- End with technical specs: aspect ratio, photorealistic, shot on camera

If images are uploaded, analyze them carefully. If no reference image is uploaded, ask for one or work from the description.
If the user wants to refine the prompt, update it based on their feedback.
Never add rules the user didn't ask for. Never remove rules without being asked.
Keep prompts tight — every word should earn its place.`;

  let conversationHistory = [];
  let refImageBase64 = null;
  let prodImageBase64 = null;
  let pastedImages = [];
  let isLoading = false;

  // Paste handler
  document.addEventListener('paste', (e) => {
    const items = e.clipboardData?.items;
    if (!items) return;
    for (const item of items) {
      if (item.type.startsWith('image/')) {
        e.preventDefault();
        const file = item.getAsFile();
        const reader = new FileReader();
        reader.onload = (ev) => {
          const base64 = ev.target.result.split(',')[1];
          addPastedImage(base64, ev.target.result);
        };
        reader.readAsDataURL(file);
      }
    }
  });

  function addPastedImage(base64, dataUrl) {
    pastedImages.push(base64);
    const strip = document.getElementById('pasteStrip');
    strip.classList.add('visible');
    const wrap = document.createElement('div');
    wrap.className = 'paste-thumb-wrap';
    const img = document.createElement('img');
    img.src = dataUrl;
    img.className = 'paste-thumb';
    const remove = document.createElement('div');
    remove.className = 'paste-remove';
    remove.textContent = '×';
    const idx = pastedImages.length - 1;
    remove.onclick = () => removePastedImage(idx, wrap);
    wrap.appendChild(img);
    wrap.appendChild(remove);
    strip.appendChild(wrap);
  }

  function removePastedImage(idx, el) {
    pastedImages.splice(idx, 1);
    el.remove();
    if (pastedImages.length === 0) {
      document.getElementById('pasteStrip').classList.remove('visible');
    }
  }

  function handleImage(type, input) {
    const file = input.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = (e) => {
      const base64 = e.target.result.split(',')[1];
      if (type === 'ref') {
        refImageBase64 = base64;
        document.getElementById('refText').textContent = '✓ Reference';
        document.getElementById('refLabel').classList.add('has-image');
      } else {
        prodImageBase64 = base64;
        document.getElementById('prodText').textContent = '✓ Product';
        document.getElementById('prodLabel').classList.add('has-image');
      }
    };
    reader.readAsDataURL(file);
  }

  function hideWelcome() {
    const w = document.getElementById('welcome');
    if (w) w.remove();
  }

  function addMessage(role, content, images = []) {
    hideWelcome();
    const chatArea = document.getElementById('chatArea');
    const msg = document.createElement('div');
    msg.className = `msg ${role}`;
    const avatar = document.createElement('div');
    avatar.className = 'avatar';
    avatar.textContent = role === 'user' ? 'K' : 'AI';
    const bubble = document.createElement('div');
    bubble.className = 'bubble';
    if (images.length > 0) {
      const imgRow = document.createElement('div');
      imgRow.className = 'img-row';
      images.forEach(src => {
        const img = document.createElement('img');
        img.src = src;
        img.className = 'img-preview';
        imgRow.appendChild(img);
      });
      bubble.appendChild(imgRow);
    }
    const text = document.createElement('span');
    text.textContent = content;
    bubble.appendChild(text);
    msg.appendChild(avatar);
    msg.appendChild(bubble);
    chatArea.appendChild(msg);
    chatArea.scrollTop = chatArea.scrollHeight;
  }

  function addPromptOutput(promptText) {
    const chatArea = document.getElementById('chatArea');
    const msg = document.createElement('div');
    msg.className = 'msg assistant';
    const avatar = document.createElement('div');
    avatar.className = 'avatar';
    avatar.textContent = 'AI';
    const wrapper = document.createElement('div');
    const intro = document.createElement('div');
    intro.className = 'bubble';
    intro.style.marginBottom = '8px';
    intro.textContent = "Here's your Gemini prompt:";
    const output = document.createElement('div');
    output.className = 'prompt-output';
    const label = document.createElement('div');
    label.className = 'prompt-label';
    label.textContent = 'Copy & paste into Gemini';
    const text = document.createElement('div');
    text.className = 'prompt-text';
    text.textContent = promptText;
    const copyBtn = document.createElement('button');
    copyBtn.className = 'copy-btn';
    copyBtn.textContent = 'COPY PROMPT';
    copyBtn.onclick = () => {
      navigator.clipboard.writeText(promptText).then(() => {
        copyBtn.textContent = 'COPIED ✓';
        copyBtn.classList.add('copied');
        setTimeout(() => {
          copyBtn.textContent = 'COPY PROMPT';
          copyBtn.classList.remove('copied');
        }, 2000);
      });
    };
    output.appendChild(label);
    output.appendChild(text);
    output.appendChild(copyBtn);
    wrapper.appendChild(intro);
    wrapper.appendChild(output);
    msg.appendChild(avatar);
    msg.appendChild(wrapper);
    chatArea.appendChild(msg);
    chatArea.scrollTop = chatArea.scrollHeight;
  }

  function addTyping() {
    const chatArea = document.getElementById('chatArea');
    const msg = document.createElement('div');
    msg.className = 'msg assistant typing';
    msg.id = 'typingIndicator';
    const avatar = document.createElement('div');
    avatar.className = 'avatar';
    avatar.textContent = 'AI';
    const bubble = document.createElement('div');
    bubble.className = 'bubble';
    bubble.innerHTML = '<div class="dot"></div><div class="dot"></div><div class="dot"></div>';
    msg.appendChild(avatar);
    msg.appendChild(bubble);
    chatArea.appendChild(msg);
    chatArea.scrollTop = chatArea.scrollHeight;
  }

  function removeTyping() {
    const t = document.getElementById('typingIndicator');
    if (t) t.remove();
  }

  async function sendMessage() {
    if (isLoading) return;
    const input = document.getElementById('userInput');
    const text = input.value.trim();
    if (!text && !refImageBase64 && !prodImageBase64 && pastedImages.length === 0) return;

    isLoading = true;
    document.getElementById('sendBtn').disabled = true;

    const displayImages = [];
    if (refImageBase64) displayImages.push(`data:image/jpeg;base64,${refImageBase64}`);
    if (prodImageBase64) displayImages.push(`data:image/jpeg;base64,${prodImageBase64}`);
    pastedImages.forEach(b => displayImages.push(`data:image/jpeg;base64,${b}`));

    const displayText = text || (displayImages.length > 0 ? 'Here are my images.' : '');
    addMessage('user', displayText, displayImages);

    const messageContent = [];

    if (refImageBase64) {
      messageContent.push({ type: 'image', source: { type: 'base64', media_type: 'image/jpeg', data: refImageBase64 } });
      messageContent.push({ type: 'text', text: '[Reference image — this is the scene/composition to match]' });
    }

    if (prodImageBase64) {
      messageContent.push({ type: 'image', source: { type: 'base64', media_type: 'image/jpeg', data: prodImageBase64 } });
      messageContent.push({ type: 'text', text: '[Product image — this is the product to place in the scene]' });
    }

    pastedImages.forEach((b, i) => {
      messageContent.push({ type: 'image', source: { type: 'base64', media_type: 'image/jpeg', data: b } });
      messageContent.push({ type: 'text', text: `[Pasted image ${i + 1}]` });
    });

    if (text) messageContent.push({ type: 'text', text });

    conversationHistory.push({ role: 'user', content: messageContent });

    // Clear inputs
    refImageBase64 = null;
    prodImageBase64 = null;
    pastedImages = [];
    document.getElementById('refText').textContent = 'Reference image';
    document.getElementById('prodText').textContent = 'Product image';
    document.getElementById('refLabel').classList.remove('has-image');
    document.getElementById('prodLabel').classList.remove('has-image');
    document.getElementById('refInput').value = '';
    document.getElementById('prodInput').value = '';
    const strip = document.getElementById('pasteStrip');
    strip.classList.remove('visible');
    strip.querySelectorAll('.paste-thumb-wrap').forEach(el => el.remove());
    input.value = '';
    input.style.height = 'auto';

    addTyping();

    try {
      const response = await fetch('/api/chat', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          system: SYSTEM_PROMPT,
          messages: conversationHistory
        })
      });

      const data = await response.json();
      removeTyping();

      if (!response.ok) {
        addMessage('assistant', `Error: ${data.error || 'Something went wrong.'}`);
        isLoading = false;
        document.getElementById('sendBtn').disabled = false;
        return;
      }

      const assistantText = data.content?.find(b => b.type === 'text')?.text || 'Something went wrong.';
      conversationHistory.push({ role: 'assistant', content: assistantText });

      const isPrompt = assistantText.length > 200 && !assistantText.trim().endsWith('?');
      if (isPrompt) {
        const lines = assistantText.split('\n').filter(l => l.trim());
        const promptLine = lines.find(l => l.length > 100) || assistantText;
        addPromptOutput(promptLine.trim());
      } else {
        addMessage('assistant', assistantText);
      }

    } catch (err) {
      removeTyping();
      addMessage('assistant', 'Connection error. Check your network and try again.');
    }

    isLoading = false;
    document.getElementById('sendBtn').disabled = false;
  }
</script>
</body>
</html>
