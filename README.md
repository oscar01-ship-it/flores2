<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flores Amarillas Para Ti 💛</title>

    <!-- Fuentes de Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Caveat:wght@600&family=Montserrat:wght@300;400;600&display=swap" rel="stylesheet">

    <style>
        /* ==========================================================================
           1. VARIABLES GLOBALES Y RESET
           ========================================================================== */
        :root {
            --bg-pastel: #FFFDD0;          /* Crema pastel */
            --bg-accent: #FFF9E6;          /* Degradado suave */
            --yellow-main: #FFD700;        /* Amarillo girasol / tulipán */
            --yellow-light: #FFEC8B;       /* Amarillo claro para destellos */
            --yellow-dark: #FFA500;        /* Naranja cálido para sombras de pétalos */
            --stem-green: #6B8E23;         /* Verde oliva suave */
            --stem-dark: #4B6B18;          /* Verde oscuro para profundidad */
            --center-brown: #5C3A21;       /* Centro de girasol */
            --text-main: #4A3E3D;          /* Marrón oscuro para texto */
            --text-soft: #7A6B69;          /* Marrón suave */
            --card-bg: #FFFFFF;            /* Fondo de tarjeta */
            --shadow: rgba(92, 58, 33, 0.12);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background: radial-gradient(circle at center, var(--bg-accent) 0%, var(--bg-pastel) 100%);
            font-family: 'Montserrat', sans-serif;
            color: var(--text-main);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            overflow-x: hidden;
            position: relative;
        }

        /* Canvas de Partículas de Fondo */
        #bgCanvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1;
        }

        /* Contenedor Principal */
        .main-container {
            position: relative;
            z-index: 10;
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
            padding: 20px;
            max-width: 600px;
            width: 100%;
        }

        /* Encabezados con animación fade-in */
        .header {
            opacity: 0;
            transform: translateY(-20px);
            animation: fadeInDown 1.2s cubic-bezier(0.16, 1, 0.3, 1) forwards 0.3s;
        }

        .header h1 {
            font-family: 'Caveat', cursive;
            font-size: 3rem;
            color: var(--text-main);
            margin-bottom: 8px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
        }

        .header p {
            font-size: 0.95rem;
            color: var(--text-soft);
            letter-spacing: 0.5px;
            margin-bottom: 25px;
        }

        /* ==========================================================================
           2. ESTILOS Y ANIMACIONES DEL RAMO (SVG)
           ========================================================================== */
        .bouquet-wrapper {
            position: relative;
            width: 320px;
            height: 420px;
            cursor: pointer;
            margin-bottom: 30px;
            transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        .bouquet-wrapper:hover {
            transform: scale(1.04) translateY(-5px);
        }

        .bouquet-svg {
            width: 100%;
            height: 100%;
            overflow: visible;
            filter: drop-shadow(0 15px 25px var(--shadow));
        }

        /* Animación de brote/apertura del ramo */
        .flower-group {
            transform-origin: 160px 300px;
            opacity: 0;
            transform: scale(0.2) rotate(-10deg);
            animation: bloom 1.5s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
        }

        .flower-1 { animation-delay: 0.6s; } /* Tulipán Izquierdo */
        .flower-2 { animation-delay: 0.8s; } /* Girasol Central */
        .flower-3 { animation-delay: 1.0s; } /* Tulipán Derecho */
        .flower-4 { animation-delay: 1.2s; } /* Girasol Secundario */

        .ribbon-group {
            opacity: 0;
            animation: fadeIn 1s ease forwards 1.4s;
        }

        /* ==========================================================================
           3. BOTÓN E INTERFAZ
           ========================================================================== */
        .btn-message {
            opacity: 0;
            transform: translateY(20px);
            animation: fadeInUp 1s ease forwards 1.6s;
            background: linear-gradient(135deg, var(--yellow-main) 0%, var(--yellow-dark) 100%);
            color: #4A3000;
            border: none;
            padding: 14px 32px;
            font-family: 'Montserrat', sans-serif;
            font-weight: 600;
            font-size: 0.95rem;
            letter-spacing: 1px;
            border-radius: 30px;
            cursor: pointer;
            box-shadow: 0 8px 20px rgba(255, 165, 0, 0.3);
            transition: all 0.3s ease;
            outline: none;
        }

        .btn-message:hover {
            transform: translateY(-2px);
            box-shadow: 0 12px 25px rgba(255, 165, 0, 0.45);
            background: linear-gradient(135deg, #FFE033 0%, #FF9900 100%);
        }

        .btn-message:active {
            transform: translateY(1px);
        }

        /* ==========================================================================
           4. TARJETA MODAL CON MENSAJE
           ========================================================================== */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(74, 62, 61, 0.35);
            backdrop-filter: blur(5px);
            -webkit-backdrop-filter: blur(5px);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 100;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.4s ease;
        }

        .modal-overlay.active {
            opacity: 1;
            pointer-events: auto;
        }

        .modal-card {
            background: var(--card-bg);
            padding: 40px 30px;
            border-radius: 24px;
            max-width: 440px;
            width: 88%;
            text-align: center;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
            transform: scale(0.8) translateY(20px);
            transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            position: relative;
            border: 2px solid rgba(255, 215, 0, 0.3);
        }

        .modal-overlay.active .modal-card {
            transform: scale(1) translateY(0);
        }

        .modal-card h2 {
            font-family: 'Caveat', cursive;
            font-size: 2.4rem;
            color: var(--yellow-dark);
            margin-bottom: 15px;
        }

        .modal-card p {
            font-size: 1rem;
            line-height: 1.6;
            color: var(--text-main);
            margin-bottom: 25px;
            font-weight: 400;
        }

        .btn-close {
            background: transparent;
            border: 1px solid var(--text-soft);
            color: var(--text-soft);
            padding: 8px 24px;
            border-radius: 20px;
            font-size: 0.85rem;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .btn-close:hover {
            background: var(--text-soft);
            color: #FFFFFF;
        }

        /* ==========================================================================
           5. KEYFRAMES DE ANIMACIÓN
           ========================================================================== */
        @keyframes bloom {
            0% {
                opacity: 0;
                transform: scale(0.2) rotate(-10deg);
            }
            70% {
                transform: scale(1.05) rotate(2deg);
            }
            100% {
                opacity: 1;
                transform: scale(1) rotate(0deg);
            }
        }

        @keyframes fadeInDown {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes fadeInUp {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes fadeIn {
            to {
                opacity: 1;
            }
        }

        /* ==========================================================================
           6. RESPONSIVE DESIGN
           ========================================================================== */
        @media (max-width: 480px) {
            .header h1 {
                font-size: 2.4rem;
            }
            .bouquet-wrapper {
                width: 270px;
                height: 360px;
            }
            .modal-card {
                padding: 30px 20px;
            }
            .modal-card h2 {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>

    <!-- Canvas para Partículas Flotantes -->
    <canvas id="bgCanvas"></canvas>

    <!-- Contenedor Principal de la Aplicación -->
    <main class="main-container">
        
        <header class="header">
            <h1>Un Ramo Especial 💛</h1>
            <p>Haz clic en el ramo para descubrir tu mensaje</p>
        </header>

        <!-- Ilustración Vectorial SVG del Ramo -->
        <div class="bouquet-wrapper" onclick="toggleModal(true)" aria-label="Ramo de flores amarillas">
            <svg class="bouquet-svg" viewBox="0 0 320 400" fill="none" xmlns="http://www.w3.org/2000/svg">
                <defs>
                    <!-- Degradados para pétalos y tallos -->
                    <linearGradient id="petalGrad" x1="0%" y1="0%" x2="0%" y2="100%">
                        <stop offset="0%" stop-color="#FFEC8B" />
                        <stop offset="60%" stop-color="#FFD700" />
                        <stop offset="100%" stop-color="#FFA500" />
                    </linearGradient>

                    <linearGradient id="stemGrad" x1="0%" y1="0%" x2="100%" y2="0%">
                        <stop offset="0%" stop-color="#4B6B18" />
                        <stop offset="100%" stop-color="#6B8E23" />
                    </linearGradient>

                    <radialGradient id="centerGrad" cx="50%" cy="50%" r="50%">
                        <stop offset="0%" stop-color="#3E2723" />
                        <stop offset="70%" stop-color="#5C3A21" />
                        <stop offset="100%" stop-color="#8D5B4C" />
                    </radialGradient>

                    <!-- Sombra suave interna -->
                    <filter id="softShadow" x="-10%" y="-10%" width="120%" height="120%">
                        <feDropShadow dx="0" dy="4" stdDeviation="4" flood-color="#5C3A21" flood-opacity="0.15" />
                    </filter>
                </defs>

                <!-- TALLOS Y HOJAS BASE -->
                <g class="stems" stroke="url(#stemGrad)" stroke-linecap="round">
                    <!-- Tallos curvos del ramo -->
                    <path d="M160 330 C155 270, 110 220, 90 150" stroke-width="7"/>
                    <path d="M160 330 C160 260, 160 200, 160 120" stroke-width="8"/>
                    <path d="M160 330 C165 270, 210 220, 230 160" stroke-width="7"/>
                    <path d="M160 330 C158 280, 135 230, 125 180" stroke-width="6"/>
                    
                    <!-- Hojas verdes decorativas -->
                    <path d="M140 260 Q100 240 115 210 Q145 230 140 260 Z" fill="#6B8E23" stroke="none"/>
                    <path d="M180 250 Q220 230 205 200 Q175 220 180 250 Z" fill="#4B6B18" stroke="none"/>
                </g>

                <!-- FLORES ANIMADAS -->
                
                <!-- 1. Tulipán Izquierdo -->
                <g class="flower-group flower-1" filter="url(#softShadow)">
                    <g transform="translate(90, 150)">
                        <!-- Hojas del tulipán -->
                        <path d="M-20 10 Q-35 -15 -10 -40 Q5 -15 -20 10 Z" fill="url(#petalGrad)"/>
                        <path d="M20 10 Q35 -15 10 -40 Q-5 -15 20 10 Z" fill="url(#petalGrad)"/>
                        <path d="M0 15 Q-25 -20 0 -50 Q25 -20 0 15 Z" fill="#FFD700"/>
                    </g>
                </g>

                <!-- 2. Girasol Secundario (Derecha) -->
                <g class="flower-group flower-4" filter="url(#softShadow)">
                    <g transform="translate(230, 160) scale(0.85)">
                        <!-- Pétalos circulares -->
                        <g fill="url(#petalGrad)">
                            <ellipse cx="0" cy="-35" rx="9" ry="22" />
                            <ellipse cx="25" cy="-25" rx="9" ry="22" transform="rotate(45)" />
                            <ellipse cx="35" cy="0" rx="9" ry="22" transform="rotate(90)" />
                            <ellipse cx="25" cy="25" rx="9" ry="22" transform="rotate(135)" />
                            <ellipse cx="0" cy="35" rx="9" ry="22" transform="rotate(180)" />
                            <ellipse cx="-25" cy="25" rx="9" ry="22" transform="rotate(225)" />
                            <ellipse cx="-35" cy="0" rx="9" ry="22" transform="rotate(270)" />
                            <ellipse cx="-25" cy="-25" rx="9" ry="22" transform="rotate(315)" />
                        </g>
                        <circle cx="0" cy="0" r="18" fill="url(#centerGrad)"/>
                    </g>
                </g>

                <!-- 3. Tulipán Central Secundario -->
                <g class="flower-group flower-3" filter="url(#softShadow)">
                    <g transform="translate(125, 175) scale(0.9) rotate(-15)">
                        <path d="M-18 8 Q-30 -12 -8 -35 Q3 -12 -18 8 Z" fill="url(#petalGrad)"/>
                        <path d="M18 8 Q30 -12 8 -35 Q-3 -12 18 8 Z" fill="url(#petalGrad)"/>
                        <path d="M0 12 Q-20 -15 0 -42 Q20 -15 0 12 Z" fill="#FFEC8B"/>
                    </g>
                </g>

                <!-- 4. Girasol Principal (Centro) -->
                <g class="flower-group flower-2" filter="url(#softShadow)">
                    <g transform="translate(160, 120)">
                        <!-- Capa trasera de pétalos -->
                        <g fill="#FFA500" opacity="0.8">
                            <ellipse cx="0" cy="-45" rx="10" ry="28" transform="rotate(15)" />
                            <ellipse cx="0" cy="-45" rx="10" ry="28" transform="rotate(60)" />
                            <ellipse cx="0" cy="-45" rx="10" ry="28" transform="rotate(105)" />
                            <ellipse cx="0" cy="-45" rx="10" ry="28" transform="rotate(150)" />
                            <ellipse cx="0" cy="-45" rx="10" ry="28" transform="rotate(195)" />
                            <ellipse cx="0" cy="-45" rx="10" ry="28" transform="rotate(240)" />
                            <ellipse cx="0" cy="-45" rx="10" ry="28" transform="rotate(285)" />
                            <ellipse cx="0" cy="-45" rx="10" ry="28" transform="rotate(330)" />
                        </g>
                        <!-- Capa principal de pétalos -->
                        <g fill="url(#petalGrad)">
                            <ellipse cx="0" cy="-45" rx="11" ry="30" />
                            <ellipse cx="0" cy="-45" rx="11" ry="30" transform="rotate(45)" />
                            <ellipse cx="0" cy="-45" rx="11" ry="30" transform="rotate(90)" />
                            <ellipse cx="0" cy="-45" rx="11" ry="30" transform="rotate(135)" />
                            <ellipse cx="0" cy="-45" rx="11" ry="30" transform="rotate(180)" />
                            <ellipse cx="0" cy="-45" rx="11" ry="30" transform="rotate(225)" />
                            <ellipse cx="0" cy="-45" rx="11" ry="30" transform="rotate(270)" />
                            <ellipse cx="0" cy="-45" rx="11" ry="30" transform="rotate(315)" />
                        </g>
                        <!-- Centro semillero -->
                        <circle cx="0" cy="0" r="24" fill="url(#centerGrad)"/>
                        <circle cx="0" cy="0" r="20" fill="none" stroke="#3E2723" stroke-width="2" stroke-dasharray="4,2"/>
                    </g>
                </g>

                <!-- LAZO DE ENVOLTORIO -->
                <g class="ribbon-group" filter="url(#softShadow)">
                    <!-- Papel envolvente pastel -->
                    <path d="M120 280 L160 340 L200 280 Q160 290 120 280 Z" fill="#FFFDF0" opacity="0.9"/>
                    <!-- Cinta o lazo -->
                    <path d="M135 295 C150 305, 170 305, 185 295" stroke="#FFA500" stroke-width="5" fill="none" stroke-linecap="round"/>
                    <circle cx="160" cy="300" r="5" fill="#FFA500"/>
                </g>
            </svg>
        </div>

        <button class="btn-message" onclick="toggleModal(true)">Ver Dedicatoria ✨</button>

    </main>

    <!-- Modal con el Mensaje Personalizado -->
    <div class="modal-overlay" id="messageModal" onclick="handleOverlayClick(event)">
        <div class="modal-card">
            <h2>Para Alguien Especial 🌻</h2>
            <p>
                Que estas flores amarillas iluminen tu día tanto como tú iluminas el de los demás. 
                Nunca olvides lo increíble que eres y lo mucho que vales. ¡Que tengas un día radiante y lleno de sonrisas! 💛✨
            </p>
            <button class="btn-close" onclick="toggleModal(false)">Guardar Flores 💛</button>
        </div>
    </div>

    <!-- ==========================================================================
       7. LÓGICA JAVASCRIPT (Partículas Canvas + Modal)
       ========================================================================== -->
    <script>
        /* --- Lógica del Modal --- */
        function toggleModal(show) {
            const modal = document.getElementById('messageModal');
            if (show) {
                modal.classList.add('active');
            } else {
                modal.classList.remove('active');
            }
        }

        function handleOverlayClick(event) {
            if (event.target.classList.contains('modal-overlay')) {
                toggleModal(false);
            }
        }

        /* --- Sistema de Partículas Ambientales (Canvas) --- */
        const canvas = document.getElementById('bgCanvas');
        const ctx = canvas.getContext('2d');

        let width, height;
        let particles = [];

        // Ajustar tamaño del canvas al cambiar dimensiones de ventana
        function resizeCanvas() {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
        }

        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        // Clase Partícula (Representa destellos suaves y pétalos cayendo)
        class Particle {
            constructor() {
                this.reset();
            }

            reset() {
                this.x = Math.random() * width;
                this.y = Math.random() * height - height; // Iniciar arriba fuera de pantalla
                this.size = Math.random() * 4 + 2;
                this.speedY = Math.random() * 1 + 0.5;
                this.speedX = Math.sin(Math.random() * Math.PI) * 0.5;
                this.opacity = Math.random() * 0.5 + 0.3;
                this.color = Math.random() > 0.3 ? '#FFD700' : '#FFEC8B'; // Tonos amarillos
                this.isSparkle = Math.random() > 0.5;
                this.pulseSpeed = Math.random() * 0.02 + 0.005;
            }

            update() {
                this.y += this.speedY;
                this.x += Math.sin(this.y * 0.01) * 0.5;

                // Efecto de pulso en opacidad para destellos
                if (this.isSparkle) {
                    this.opacity += Math.sin(Date.now() * this.pulseSpeed) * 0.01;
                }

                // Reiniciar cuando sale de la pantalla
                if (this.y > height + 20) {
                    this.reset();
                    this.y = -10;
                }
            }

            draw() {
                ctx.save();
                ctx.globalAlpha = Math.max(0, Math.min(1, this.opacity));
                ctx.fillStyle = this.color;

                if (this.isSparkle) {
                    // Dibujar pequeño destello en forma de estrella sutil
                    ctx.beginPath();
                    ctx.arc(this.x, this.y, this.size / 2, 0, Math.PI * 2);
                    ctx.fill();
                } else {
                    // Dibujar pequeño pétalo/óvalo cayendo
                    ctx.beginPath();
                    ctx.ellipse(this.x, this.y, this.size, this.size / 2, Math.PI / 4, 0, Math.PI * 2);
                    ctx.fill();
                }

                ctx.restore();
            }
        }

        // Inicializar conjunto de partículas
        function initParticles() {
            particles = [];
            const particleCount = Math.floor((width * height) / 18000); // Adaptable a densidad de pantalla
            for (let i = 0; i < Math.min(particleCount, 50); i++) {
                particles.push(new Particle());
            }
        }

        // Bucle de animación continuo
        function animateParticles() {
            ctx.clearRect(0, 0, width, height);
            
            particles.forEach(particle => {
                particle.update();
                particle.draw();
            });

            requestAnimationFrame(animateParticles);
        }

        initParticles();
        animateParticles();
    </script>
</body>
</html>
