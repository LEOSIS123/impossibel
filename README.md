<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Impossible - Enhanced Audio Player</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');
  body {
    background-color: #121212;
    color: #eee;
    font-family: 'Roboto', Arial, sans-serif;
    margin: 0;
    padding: 0;
    display: flex;
    flex-direction: column;
    min-height: 100vh;
  }
  header {
    text-align: center;
    padding: 1.5rem 1rem 1rem 1rem;
    background: linear-gradient(90deg, #ff8c00, #ffa500);
    color: #121212;
    box-shadow: 0 2px 8px rgba(255, 165, 0, 0.5);
  }
  header h1 {
    margin: 0;
    font-weight: 700;
    font-size: 2.5rem;
    letter-spacing: 1.5px;
  }
  header h2 {
    margin: 0.3rem 0 0 0;
    font-weight: 400;
    font-size: 1.2rem;
    opacity: 0.85;
  }
  main {
    flex: 1;
    display: flex;
    flex-direction: column;
    max-width: 720px;
    margin: 1rem auto 2rem auto;
    padding: 0 1rem;
  }
  #controls {
    display: flex;
    align-items: center;
    gap: 1rem;
    margin-bottom: 0.75rem;
  }
  #playPauseBtn {
    background-color: #ffa500;
    border: none;
    color: #121212;
    font-weight: 700;
    font-size: 1.1rem;
    padding: 0.6rem 1.2rem;
    cursor: pointer;
    border-radius: 6px;
    box-shadow: 0 4px 8px rgba(255, 165, 0, 0.6);
    transition: background-color 0.3s ease;
  }
  #playPauseBtn:hover {
    background-color: #ffb733;
  }
  #progressContainer {
    flex: 1;
    display: flex;
    align-items: center;
    gap: 0.75rem;
  }
  #progressBar {
    width: 100%;
    -webkit-appearance: none;
    height: 8px;
    border-radius: 4px;
    background: #444;
    cursor: pointer;
    box-shadow: inset 0 1px 2px rgba(0,0,0,0.5);
  }
  #progressBar::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 18px;
    height: 18px;
    background: #ffa500;
    border-radius: 50%;
    cursor: pointer;
    box-shadow: 0 0 6px #ffb733;
    margin-top: -5px;
    transition: background-color 0.3s ease;
  }
  #progressBar::-webkit-slider-thumb:hover {
    background-color: #ffb733;
  }
  #currentTime, #duration {
    font-family: 'Courier New', Courier, monospace;
    font-size: 0.95rem;
    width: 45px;
    text-align: center;
    user-select: none;
  }
  #volumeContainer {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    margin-top: 0.75rem;
  }
  #volumeLabel {
    font-size: 1rem;
    user-select: none;
  }
  #volumeSlider {
    width: 120px;
    -webkit-appearance: none;
    height: 6px;
    border-radius: 3px;
    background: #444;
    cursor: pointer;
    box-shadow: inset 0 1px 2px rgba(0,0,0,0.5);
  }
  #volumeSlider::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 16px;
    height: 16px;
    background: #ffa500;
    border-radius: 50%;
    cursor: pointer;
    box-shadow: 0 0 6px #ffb733;
    margin-top: -5px;
    transition: background-color 0.3s ease;
  }
  #volumeSlider::-webkit-slider-thumb:hover {
    background-color: #ffb733;
  }
  #lyricsContainer {
    flex: 1;
    overflow-y: auto;
    background-color: #282828;
    border-radius: 8px;
    padding: 1rem;
    margin-top: 1.5rem;
    font-size: 1.1rem;
    line-height: 1.5;
    box-shadow: inset 0 0 10px #000;
  }
  .lyric-line {
    padding: 0.3rem 0.6rem;
    color: #ddd;
    transition: background-color 0.3s ease, color 0.3s ease;
  }
  .lyric-line.active {
    background-color: #ff6b6b;
    color: #121212;
    font-weight: 700;
    border-radius: 5px;
    box-shadow: 0 0 8px #ff6b6b;
  }
  #appleMusicEmbed {
    margin-top: 2rem;
    border-radius: 12px;
    overflow: hidden;
    max-width: 660px;
    width: 100%;
    height: 175px;
    box-shadow: 0 0 15px rgba(255, 165, 0, 0.7);
    align-self: center;
  }
  @media (max-width: 480px) {
    main {
      margin: 1rem 0.5rem 2rem 0.5rem;
    }
    #playPauseBtn {
      font-size: 1rem;
      padding: 0.5rem 1rem;
    }
    #volumeSlider {
      width: 100px;
    }
  }
</style>
</head>
<body>
<header>
  <h1>Impossible</h1>
  <h2>Artist Name</h2>
</header>
<main>
  <div id="controls">
    <button id="playPauseBtn">Play</button>
    <div id="progressContainer">
      <span id="currentTime">0:00</span>
      <input type="range" id="progressBar" min="0" max="100" value="0" step="0.1" />
      <span id="duration">0:00</span>
    </div>
  </div>
  <div id="volumeContainer">
    <label for="volumeSlider" id="volumeLabel">Volume</label>
    <input type="range" id="volumeSlider" min="0" max="1" step="0.01" value="1" />
  </div>
  <div id="lyricsContainer"></div>
  <audio id="audio" preload="metadata" src="imposibel.mp3"></audio>

  <div id="appleMusicEmbed">
    <iframe allow="autoplay *; encrypted-media *; fullscreen *; clipboard-write" frameborder="0" height="175" style="width:100%;max-width:660px;overflow:hidden;border-radius:10px;" sandbox="allow-forms allow-popups allow-same-origin allow-scripts allow-storage-access-by-user-activation allow-top-navigation-by-user-activation" src="https://embed.music.apple.com/id/song/impossible/589034898"></iframe>
  </div>
</main>
<script>
  const audio = document.getElementById('audio');
  const playPauseBtn = document.getElementById('playPauseBtn');
  const progressBar = document.getElementById('progressBar');
  const currentTimeLabel = document.getElementById('currentTime');
  const durationLabel = document.getElementById('duration');
  const volumeSlider = document.getElementById('volumeSlider');
  const lyricsContainer = document.getElementById('lyricsContainer');

  const lyrics = [
    { time: 0, text: "I remember years ago" },
    { time: 5, text: "Someone told me I should take" },
    { time: 10, text: "Caution when it comes to love, I did" },
    { time: 15, text: "And you were strong and I was not" },
    { time: 20, text: "My illusion, my mistake" },
    { time: 25, text: "I was careless I forgot, I did" },
    { time: 30, text: "And now" },
    { time: 35, text: "When all is done, there is nothing to say" },
    { time: 40, text: "You have gone and so effortlessly" },
    { time: 45, text: "You have won, you can go ahead tell them" },
    { time: 50, text: "Tell them all I know now" },
    { time: 55, text: "Shout it from the rooftops" },
    { time: 60, text: "Write it on the skyline" },
    { time: 65, text: "All we had is gone now" },
    { time: 70, text: "Tell them I was happy" },
    { time: 75, text: "And my heart is broken" },
    { time: 80, text: "All my scars are open" },
    { time: 85, text: "Tell them what I hoped would be" },
    { time: 90, text: "Impossible, impossible" },
    { time: 95, text: "Impossible, impossible" },
    { time: 100, text: "Falling out of love is hard" },
    { time: 105, text: "Falling for betrayal is worse" },
    { time: 110, text: "Broken trust and broken hearts, I know, I know" },
    { time: 115, text: "And thinking all you need is there" },
    { time: 120, text: "Building faith on love and words" },
    { time: 125, text: "Empty promises will wear, I know, I know" },
    { time: 130, text: "And now" },
    { time: 135, text: "When all is done there is nothing to say" },
    { time: 140, text: "And if you're done with embarrassing me" },
    { time: 145, text: "On your own you can go ahead tell them" },
    { time: 150, text: "Tell them all I know now" },
    { time: 155, text: "Shout it from the rooftops" },
    { time: 160, text: "Write it on the skyline" },
    { time: 165, text: "All we had is gone now" },
    { time: 170, text: "Tell them I was happy" },
    { time: 175, text: "And my heart is broken" },
    { time: 180, text: "All my scars are open" },
    { time: 185, text: "Tell them what I hoped would be" },
    { time: 190, text: "Impossible, impossible" },
    { time: 195, text: "Impossible, impossible" },
    { time: 200, text: "I remember years ago" },
    { time: 205, text: "Someone told me I should take" },
    { time: 210, text: "Caution when it comes to love, I did" },
    { time: 215, text: "Tell them all I know now" },
    { time: 220, text: "Shout it from the rooftops" },
    { time: 225, text: "Write it on the skyline" },
    { time: 230, text: "All we had is gone now" },
    { time: 235, text: "Tell them I was happy" },
    { time: 240, text: "And my heart is broken" },
    { time: 245, text: "Oh, what I hoped would be" },
    { time: 250, text: "Impossible (impossible), impossible (impossible)" },
    { time: 255, text: "Impossible, impossible" },
    { time: 260, text: "Impossible (impossible), impossible (impossible)" },
    { time: 265, text: "Impossible, impossible" }
  ];

  let currentLyricIndex = -1;

  // Populate lyrics container
  lyrics.forEach((line, index) => {
    const div = document.createElement('div');
    div.textContent = line.text;
    div.classList.add('lyric-line');
    div.dataset.index = index;
    lyricsContainer.appendChild(div);
  });

  function formatTime(seconds) {
    const m = Math.floor(seconds / 60);
    const s = Math.floor(seconds % 60);
    return `${m}:${s.toString().padStart(2, '0')}`;
  }

  function updateLyrics(currentTime) {
    let newIndex = -1;
    for (let i = 0; i < lyrics.length; i++) {
      const t = lyrics[i].time;
      const nextT = i + 1 < lyrics.length ? lyrics[i + 1].time : Infinity;
      if (currentTime >= t && currentTime < nextT) {
        newIndex = i;
        break;
      }
    }
    if (newIndex !== currentLyricIndex) {
      if (currentLyricIndex !== -1) {
        const oldLine = lyricsContainer.querySelector(`.lyric-line[data-index="${currentLyricIndex}"]`);
        if (oldLine) oldLine.classList.remove('active');
      }
      if (newIndex !== -1) {
        const newLine = lyricsContainer.querySelector(`.lyric-line[data-index="${newIndex}"]`);
        if (newLine) {
          newLine.classList.add('active');
          // Scroll to keep active lyric visible
          const containerHeight = lyricsContainer.clientHeight;
          const lineOffsetTop = newLine.offsetTop;
          const lineHeight = newLine.offsetHeight;
          const scrollTop = lyricsContainer.scrollTop;
          if (lineOffsetTop < scrollTop || lineOffsetTop + lineHeight > scrollTop + containerHeight) {
            lyricsContainer.scrollTop = lineOffsetTop - containerHeight / 2 + lineHeight / 2;
          }
        }
      }
      currentLyricIndex = newIndex;
    }
  }

  playPauseBtn.addEventListener('click', () => {
    if (audio.paused) {
      audio.play();
      playPauseBtn.textContent = 'Pause';
    } else {
      audio.pause();
      playPauseBtn.textContent = 'Play';
    }
  });

  audio.addEventListener('timeupdate', () => {
    const currentTime = audio.currentTime;
    progressBar.value = currentTime;
    currentTimeLabel.textContent = formatTime(currentTime);
    updateLyrics(currentTime);
  });

  audio.addEventListener('loadedmetadata', () => {
    progressBar.max = audio.duration;
    durationLabel.textContent = formatTime(audio.duration);
  });

  progressBar.addEventListener('input', () => {
    audio.currentTime = progressBar.value;
    updateLyrics(audio.currentTime);
  });

  volumeSlider.addEventListener('input', () => {
    audio.volume = volumeSlider.value;
  });

  // Initialize volume
  audio.volume = volumeSlider.value;
</script>
</body>
</html>
# impossibel
