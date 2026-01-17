// === PASSWORD CHECK ===
document.addEventListener('DOMContentLoaded', function() {
    const passwordOverlay = document.getElementById('password-overlay');
    const passwordInput = document.getElementById('password-input');
    const passwordSubmit = document.getElementById('password-submit');
    const passwordError = document.getElementById('password-error');
    const mainContent = document.getElementById('main-content');
    
    const correctPassword = "ghostin2026";
    
    passwordSubmit.addEventListener('click', checkPassword);
    passwordInput.addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            checkPassword();
        }
    });
    
    function checkPassword() {
        if (passwordInput.value === correctPassword) {
            passwordOverlay.classList.add('hidden');
            mainContent.classList.add('visible');
            initApp();
        } else {
            passwordError.classList.add('show');
            passwordInput.value = '';
            setTimeout(() => {
                passwordError.classList.remove('show');
            }, 2000);
        }
    }
});

// === MAIN APPLICATION ===
function initApp() {
    const canvas = document.getElementById('particles');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = document.documentElement.scrollHeight;
    
    const stars = [];
    for (let i = 0; i < 150; i++) {
        stars.push({
            x: Math.random() * canvas.width,
            y: Math.random() * canvas.height,
            radius: Math.random() * 1.5,
            opacity: Math.random() * 0.6 + 0.2,
            twinkleSpeed: Math.random() * 0.02
        });
    }
    
    let isPaused = false;
    
    function drawStars() {
        if (isPaused) return;
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        stars.forEach(star => {
            ctx.beginPath();
            ctx.arc(star.x, star.y, star.radius, 0, Math.PI * 2);
            ctx.fillStyle = `rgba(255, 255, 255, ${star.opacity})`;
            ctx.fill();
            star.opacity += star.twinkleSpeed;
            if (star.opacity > 0.8 || star.opacity < 0.2) star.twinkleSpeed *= -1;
        });
        requestAnimationFrame(drawStars);
    }
    drawStars();
    
    const observer = new IntersectionObserver(entries => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                const delay = entry.target.dataset.delay || 0;
                setTimeout(() => entry.target.classList.add('visible'), delay);
            }
        });
    }, { threshold: 0.3 });
    
    document.querySelectorAll('.line, .horizon, #fountain, .convo, .quote').forEach(el => observer.observe(el));
    
    const sunObs = new IntersectionObserver(e => {
        if (e[0].isIntersecting) document.getElementById('sun').classList.add('visible');
    }, { threshold: 0.5 });
    sunObs.observe(document.getElementById('dawn'));
    
    const fountainObs = new IntersectionObserver(e => {
        if (e[0].isIntersecting) document.getElementById('fountain').style.opacity = '1';
    }, { threshold: 0.3 });
    fountainObs.observe(document.getElementById('memory'));
    
    const heartObs = new IntersectionObserver(e => {
        if (e[0].isIntersecting) document.getElementById('finalHeart').classList.add('visible');
    }, { threshold: 0.5 });
    heartObs.observe(document.getElementById('continuation'));
    
    const symbols = ['✦', '☾', '☀', '∞', '○', '◇', '♡', '✶'];
    for (let i = 0; i < 25; i++) {
        const s = document.createElement('div');
        s.className = 'symbol ' + (Math.random() > 0.5 ? 'drift' : 'float');
        s.textContent = symbols[Math.floor(Math.random() * symbols.length)];
        s.style.left = Math.random() * 100 + '%';
        s.style.top = Math.random() * 100 + '%';
        s.style.animationDelay = Math.random() * 5 + 's';
        document.body.appendChild(s);
    }
    
    let audioPlaying = false;
    const audio = new Audio('ghostin.mp3');
    audio.loop = true;
    audio.volume = 0.3;
    
    document.getElementById('audioToggle').onclick = function() {
        audioPlaying = !audioPlaying;
        this.textContent = audioPlaying ? '♪' : '☾';
        if (audioPlaying) {
            audio.play().catch(e => console.log('Audio playback failed:', e));
        } else {
            audio.pause();
        }
    };
    
    document.getElementById('pauseToggle').onclick = function() {
        isPaused = !isPaused;
        this.textContent = isPaused ? '▶' : '❚❚';
        document.querySelectorAll('.symbol, .moon, .sun, .ripple, .song-title').forEach(el => {
            el.style.animationPlayState = isPaused ? 'paused' : 'running';
        });
        if (!isPaused) drawStars();
    };
    
    window.onresize = () => {
        canvas.width = window.innerWidth;
        canvas.height = document.documentElement.scrollHeight;
    };
}
