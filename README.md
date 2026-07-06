<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Drop</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,600&family=JetBrains+Mono:wght@400;500&family=Inter:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --desk: #1B1B1F;
    --desk-dark: #131316;
    --paper: #F3ECD9;
    --paper-dim: #E8E0C8;
    --brass: #B8935A;
    --brass-dim: #8C7040;
    --ink: #2B2E38;
    --ink-soft: #5B5E68;
    --success: #6E8F6C;
  }

  * { box-sizing: border-box; }

  body {
    margin: 0;
    min-height: 100vh;
    background:
      radial-gradient(ellipse 900px 500px at 50% -10%, #26262c 0%, transparent 60%),
      var(--desk);
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'Inter', sans-serif;
    color: var(--ink);
    padding: 40px 20px;
    overflow: hidden;
  }

  /* subtle wood-grain desk texture */
  body::before {
    content: "";
    position: fixed;
    inset: 0;
    background-image: repeating-linear-gradient(
      95deg,
      rgba(255,255,255,0.012) 0px,
      rgba(255,255,255,0.012) 1px,
      transparent 1px,
      transparent 3px
    );
    pointer-events: none;
  }

  .scene {
    position: relative;
    width: 100%;
    max-width: 460px;
  }

  /* the box + slot the paper disappears into */
  .box {
    position: relative;
    height: 34px;
    margin: 0 auto -1px;
    width: 92%;
    background: linear-gradient(180deg, #2A2A30, #1E1E23);
    border-radius: 10px 10px 0 0;
    box-shadow: inset 0 2px 6px rgba(0,0,0,0.5);
    z-index: 3;
  }

  .slot {
    position: absolute;
    top: 10px;
    left: 50%;
    transform: translateX(-50%);
    width: 64%;
    height: 8px;
    background: var(--desk-dark);
    border-radius: 4px;
    box-shadow: 0 1px 0 rgba(255,255,255,0.05), inset 0 2px 4px rgba(0,0,0,0.8);
  }

  .slot::after {
    content: "";
    position: absolute;
    inset: 0;
    border-radius: 4px;
    background: linear-gradient(90deg, transparent, var(--brass-dim) 20%, var(--brass) 50%, var(--brass-dim) 80%, transparent);
    opacity: 0.35;
  }

  .eyebrow {
    text-align: center;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.22em;
    color: var(--brass);
    text-transform: uppercase;
    margin-bottom: 14px;
    opacity: 0.85;
  }

  .card {
    position: relative;
    background: var(--paper);
    border-radius: 3px 3px 14px 14px;
    padding: 40px 34px 30px;
    box-shadow:
      0 1px 0 rgba(255,255,255,0.4) inset,
      0 30px 60px -20px rgba(0,0,0,0.7),
      0 10px 20px -10px rgba(0,0,0,0.5);
    z-index: 2;
    transition: transform 0.65s cubic-bezier(.55,0,.4,1), opacity 0.55s ease 0.15s;
  }

  /* torn/deckled top edge of the paper, just under the slot */
  .card::before {
    content: "";
    position: absolute;
    top: -3px;
    left: 0;
    right: 0;
    height: 6px;
    background: var(--paper);
    clip-path: polygon(
      0% 100%, 3% 30%, 7% 90%, 11% 20%, 15% 80%, 19% 10%, 23% 70%,
      27% 25%, 31% 95%, 35% 15%, 39% 75%, 43% 30%, 47% 85%, 51% 20%,
      55% 70%, 59% 10%, 63% 90%, 67% 25%, 71% 80%, 75% 15%, 79% 70%,
      83% 30%, 87% 90%, 91% 20%, 95% 75%, 100% 100%
    );
  }

  h1 {
    font-family: 'Fraunces', serif;
    font-weight: 600;
    font-size: 28px;
    letter-spacing: -0.01em;
    margin: 0 0 6px;
    color: var(--ink);
  }

  .sub {
    font-size: 13.5px;
    color: var(--ink-soft);
    margin: 0 0 26px;
    line-height: 1.5;
  }

  textarea {
    width: 100%;
    min-height: 150px;
    resize: vertical;
    background: transparent;
    border: none;
    border-top: 1px solid rgba(43,46,56,0.15);
    padding-top: 18px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 14px;
    line-height: 1.7;
    color: var(--ink);
    outline: none;
  }

  textarea::placeholder { color: rgba(43,46,56,0.35); }

  /* faint ruled lines, like a notepad */
  textarea {
    background-image: repeating-linear-gradient(
      to bottom,
      transparent,
      transparent 27px,
      rgba(43,46,56,0.08) 28px
    );
    background-position-y: 18px;
  }

  .footer-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-top: 20px;
  }

  .hint {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    color: rgba(43,46,56,0.4);
    letter-spacing: 0.03em;
  }

  button {
    font-family: 'Inter', sans-serif;
    font-weight: 500;
    font-size: 14px;
    color: var(--paper);
    background: var(--ink);
    border: none;
    padding: 12px 22px;
    border-radius: 7px;
    cursor: pointer;
    transition: background 0.2s ease, transform 0.15s ease;
    letter-spacing: 0.01em;
  }

  button:hover { background: #3a3e4a; }
  button:active { transform: scale(0.97); }
  button:focus-visible {
    outline: 2px solid var(--brass);
    outline-offset: 2px;
  }

  button:disabled {
    background: #8a8d94;
    cursor: default;
  }

  /* status line beneath, replaces alert() */
  .status {
    text-align: center;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    letter-spacing: 0.05em;
    color: var(--success);
    margin-top: 18px;
    min-height: 16px;
    opacity: 0;
    transform: translateY(-4px);
    transition: opacity 0.4s ease, transform 0.4s ease;
  }

  .status.show {
    opacity: 1;
    transform: translateY(0);
  }

  .status.error { color: #C4726B; }

  /* the sent state: card slides up into the slot */
  .card.sent {
    transform: translateY(-220px);
    opacity: 0;
  }

  @media (prefers-reduced-motion: reduce) {
    .card { transition: opacity 0.3s ease; }
    .card.sent { transform: none; }
  }
</style>
</head>
<body>

  <div class="scene">
    <div class="eyebrow">Anonymous · Unsigned</div>
    <div class="box"><div class="slot"></div></div>

    <div class="card" id="card">
      <h1>Paste here</h1>
      <p class="sub">Only.</p>

      <textarea id="message" placeholder="Paste here" aria-label="Your message"></textarea>

      <div class="footer-row">
        <span class="hint" id="charcount">0 characters</span>
        <button onclick="send()" id="sendBtn">Slip it in</button>
      </div>

      <div class="status" id="status"></div>
    </div>
  </div>

<script>
  const webhook = "https://discord.com/api/webhooks/1523477602684375090/F2sQa1nbnHTXehRWRjs41Nf-KmOBI0chnmCBlpJAyFMUpCofJyqtsVfLMVMqFeNvhnR9";

  const textarea = document.getElementById("message");
  const charcount = document.getElementById("charcount");
  const status = document.getElementById("status");
  const card = document.getElementById("card");
  const btn = document.getElementById("sendBtn");

  textarea.addEventListener("input", () => {
    charcount.textContent = `${textarea.value.length} characters`;
  });

  function setStatus(text, isError) {
    status.textContent = text;
    status.classList.toggle("error", !!isError);
    status.classList.add("show");
  }

  async function send() {
    const message = textarea.value.trim();

    if (!message) {
      setStatus("Nothing to send yet.", true);
      return;
    }

    btn.disabled = true;
    setStatus("Sending...");

    try {
      await fetch(webhook, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ content: `New submission:\n${message}` })
      });

      card.classList.add("sent");
      setTimeout(() => {
        setStatus("Sent. Nothing left on this page.");
      }, 500);

      setTimeout(() => {
        textarea.value = "";
        charcount.textContent = "0 characters";
        card.classList.remove("sent");
        btn.disabled = false;
      }, 1400);

    } catch (err) {
      setStatus("Couldn't send that. Try again.", true);
      btn.disabled = false;
    }
  }
</script>

</body>
</html>
