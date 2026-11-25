<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="referrer" content="strict-origin-when-cross-origin">
  <meta http-equiv="Content-Security-Policy" content="default-src 'none'; script-src 'unsafe-inline' https://www.youtube.com https://s.ytimg.com; frame-src https://www.youtube.com https://www.youtube-nocookie.com; style-src 'unsafe-inline'; img-src 'self' https://i.ytimg.com data:;">
  <title>CarrotTube Clip</title>
  <style>
    :root {
      --bg: #100f14;
      --surface: #1a1921;
      --border: #2e2c39;
      --orange: #ff6b1a;
      --orange-bright: #ff8c38;
      --leaf: #22c55e;
      --text: #f5f3ef;
      --subtext: #9d99a8;
    }
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      min-height: 100vh;
      background: radial-gradient(circle at top, rgba(255, 107, 26, 0.12), transparent 50%), var(--bg);
      color: var(--text);
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 20px;
      gap: 16px;
    }
    .topbar {
      width: min(960px, 100%);
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
      flex-wrap: wrap;
    }
    .brand {
      font-weight: 800;
      font-size: 18px;
      letter-spacing: -0.02em;
    }
    .brand span { color: var(--orange-bright); }
    .clip-pill {
      font-size: 12px;
      font-weight: 700;
      color: #4ade80;
      background: rgba(34, 197, 94, 0.14);
      border: 1px solid rgba(34, 197, 94, 0.3);
      padding: 5px 12px;
      border-radius: 999px;
    }
    .stage {
      position: relative;
      width: min(960px, 100%);
      aspect-ratio: 16 / 9;
      background: #000;
      border-radius: 16px;
      overflow: hidden;
      border: 1px solid var(--border);
      box-shadow: 0 24px 60px rgba(0, 0, 0, 0.6);
    }
    #player, #player iframe {
      width: 100%;
      height: 100%;
      border: 0;
    }
    .overlay {
      position: absolute;
      inset: 0;
      display: none;
      align-items: center;
      justify-content: center;
      background: rgba(12, 11, 16, 0.86);
      backdrop-filter: blur(8px);
      padding: 20px;
      z-index: 20;
    }
    .overlay.visible { display: flex; }
    .card {
      width: min(440px, 100%);
      background: var(--surface);
      border: 1px solid rgba(255, 107, 26, 0.38);
      border-radius: 16px;
      padding: 22px 24px;
      text-align: center;
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.65);
    }
    .kicker {
      display: inline-block;
      font-size: 11px;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.05em;
      color: var(--orange-bright);
      background: rgba(255, 107, 26, 0.15);
      padding: 4px 10px;
      border-radius: 999px;
      margin-bottom: 10px;
    }
    h2 { font-size: 20px; margin-bottom: 8px; }
    .sub { font-size: 13px; color: var(--subtext); margin-bottom: 18px; line-height: 1.45; }
    .note-box {
      display: none;
      background: #121117;
      border: 1px solid rgba(255, 255, 255, 0.08);
      border-left: 3px solid var(--orange);
      border-radius: 10px;
      padding: 12px 14px;
      text-align: left;
      margin: 12px 0 18px;
    }
    .note-box.visible { display: block; }
    .note-label {
      font-size: 10px;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.05em;
      color: var(--subtext);
      margin-bottom: 6px;
    }
    .note-body {
      font-size: 14px;
      line-height: 1.45;
      color: #fff;
      white-space: pre-wrap;
      word-break: break-word;
    }
    .actions {
      display: flex;
      gap: 10px;
      justify-content: center;
      flex-wrap: wrap;
    }
    .btn {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      text-decoration: none;
      border: none;
      border-radius: 9px;
      padding: 10px 16px;
      font-size: 13px;
      font-weight: 700;
      cursor: pointer;
      transition: filter 0.15s ease;
    }
    .btn:hover { filter: brightness(1.08); }
    .btn-primary { background: linear-gradient(135deg, var(--orange-bright), #f04e10); color: #fff; }
    .btn-secondary { background: #262430; color: var(--text); border: 1px solid var(--border); }
    .footer-note {
      font-size: 11px;
      color: var(--subtext);
      text-align: center;
    }
  </style>
</head>
<body>
  <div class="topbar">
    <div class="brand"><span>Carrot</span>Tube Clip</div>
    <div class="clip-pill" id="clipPill">Loading clip...</div>
  </div>

  <div class="stage">
    <div id="player"></div>

    <div class="overlay" id="overlay">
      <div class="card">
        <div class="kicker" id="overlayKicker">CarrotTube Clip</div>
        <h2 id="overlayTitle">End of clip</h2>
        <div class="note-box" id="noteBox">
          <div class="note-label">Note from link sender (not YouTube)</div>
          <div class="note-body" id="noteBody"></div>
        </div>
        <p class="sub" id="overlaySub">Replay this clip or continue watching on YouTube.</p>
        <div class="actions">
          <button type="button" class="btn btn-primary" id="replayBtn">Replay Clip</button>
          <a class="btn btn-secondary" id="ytLink" target="_blank" rel="noopener noreferrer">Continue on YouTube</a>
        </div>
      </div>
    </div>
  </div>

  <div class="footer-note">Powered by CarrotTube &bull; All clip info stays in your browser URL fragment (zero server tracking).</div>

  <script>
    (function () {
      'use strict';

      const VIDEO_ID_RE = /^[a-zA-Z0-9_-]{11}$/;

      function fmt(sec) {
        const r = Math.max(0, Math.floor(sec));
        const h = Math.floor(r / 3600);
        const m = Math.floor((r % 3600) / 60);
        const s = r % 60;
        const ps = s < 10 ? '0' + s : String(s);
        return h > 0 ? h + ':' + (m < 10 ? '0' + m : m) + ':' + ps : m + ':' + ps;
      }

      function sanitizeNote(raw) {
        if (typeof raw !== 'string') return '';
        return raw.replace(/[\u0000-\u001F\u007F\u202A-\u202E\u2066-\u2069]/g, '').trim().slice(0, 200);
      }

      const params = new URLSearchParams((location.hash || '').replace(/^#/, ''));
      const videoId = params.get('v') || '';
      const start = Math.max(0, Math.floor(Number(params.get('s') || 0)));
      const end = Math.floor(Number(params.get('e') || 0));
      const note = sanitizeNote(params.get('m') || params.get('msg') || '');

      const clipPill = document.getElementById('clipPill');
      const overlay = document.getElementById('overlay');
      const overlayKicker = document.getElementById('overlayKicker');
      const overlayTitle = document.getElementById('overlayTitle');
      const overlaySub = document.getElementById('overlaySub');
      const noteBox = document.getElementById('noteBox');
      const noteBody = document.getElementById('noteBody');
      const replayBtn = document.getElementById('replayBtn');
      const ytLink = document.getElementById('ytLink');

      if (!VIDEO_ID_RE.test(videoId) || !Number.isFinite(start) || !Number.isFinite(end) || end <= start) {
        clipPill.textContent = 'Invalid or missing clip link';
        overlayKicker.textContent = 'CarrotTube Universal Clip Player';
        overlayTitle.textContent = 'No clip loaded';
        overlaySub.textContent = 'Open a shared CarrotTube Universal Clip link (#v=...&s=...&e=...) to play a clip here.';
        replayBtn.style.display = 'none';
        ytLink.style.display = 'none';
        overlay.classList.add('visible');
        return;
      }

      const rangeText = `${fmt(start)} – ${fmt(end)} (${fmt(end - start)})`;
      clipPill.textContent = `Clip: ${rangeText}`;
      overlayKicker.textContent = `Clip • ${fmt(start)} – ${fmt(end)}`;
      ytLink.href = `https://www.youtube.com/watch?v=${encodeURIComponent(videoId)}&t=${end}s`;
      ytLink.textContent = `Continue on YouTube (${fmt(end)})`;

      if (note) {
        noteBody.textContent = note;
        noteBox.classList.add('visible');
        overlaySub.style.display = 'none';
      }

      let player = null;
      let pollTimer = null;
      let clipFinished = false;

      function showEndCard() {
        clipFinished = true;
        clearInterval(pollTimer);
        overlay.classList.add('visible');
      }

      function startPolling() {
        clearInterval(pollTimer);
        pollTimer = setInterval(() => {
          if (!player || clipFinished || typeof player.getCurrentTime !== 'function') return;
          if (player.getCurrentTime() >= end) {
            player.pauseVideo();
            showEndCard();
          }
        }, 100);
      }

      replayBtn.addEventListener('click', () => {
        if (!player) return;
        clipFinished = false;
        overlay.classList.remove('visible');
        player.seekTo(start, true);
        player.playVideo();
        startPolling();
      });

      window.onYouTubeIframeAPIReady = function () {
        player = new YT.Player('player', {
          videoId: videoId,
          playerVars: {
            start: start,
            end: end,
            rel: 0,
            modestbranding: 1,
            playsinline: 1,
            origin: window.location.origin
          },
          events: {
            onStateChange: function (e) {
              if (e.data === YT.PlayerState.PLAYING) {
                if (!clipFinished) startPolling();
              } else if (e.data === YT.PlayerState.ENDED) {
                showEndCard();
              }
            },
            onError: function () {
              clearInterval(pollTimer);
              overlayTitle.textContent = 'Embedding disabled by creator';
              overlaySub.style.display = 'block';
              overlaySub.textContent = 'This video cannot be played in an embedded player. You can still jump straight to the clip start on YouTube.';
              replayBtn.style.display = 'none';
              ytLink.href = `https://www.youtube.com/watch?v=${encodeURIComponent(videoId)}&t=${start}s`;
              ytLink.textContent = `Watch on YouTube from ${fmt(start)}`;
              overlay.classList.add('visible');
            }
          }
        });
      };

      const apiScript = document.createElement('script');
      apiScript.src = 'https://www.youtube.com/iframe_api';
      document.head.appendChild(apiScript);
    })();
  </script>
</body>
</html>
