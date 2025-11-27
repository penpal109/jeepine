<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>XXX公司活动能量充能系统</title>记得修改公司名字
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body, html {
            width: 100%;
            height: 100%;
            overflow: hidden;
            font-family: "Microsoft YaHei", sans-serif;
        }
        
        /* 背景图设置 - 可在此更换背景图 */
        .background {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
            background-image: url('https://youke1.picui.cn/s1/2025/11/25/692514d18daae.jpg');
            background-size: cover;
            background-position: center center;
        }
        
        /* 粒子效果容器 */
        #particles-js {
            position: absolute;
            width: 100%;
            height: 100%;
            z-index: 2;
        }
        
        /* 机器图片容器 */
        .machine-container {
            position: absolute;
            z-index: 3;
            /* 机器图位置设置 - 可在此调整机器图位置 */
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 1330px;  /* 可调整机器图宽度 */
            height: 519px;  /* 可调整机器图高度 */
            /* 机器图位置微调 - 可在此调整机器图X、Y偏移 */
            margin-left: 0px;  /* X轴偏移 */
            margin-top: 100px;   /* Y轴偏移 */
        }
        
        /* 机器图片 */
        .machine-image {
            width: 100%;
            height: 100%;
            /* 机器图默认透明度设置 - 可在此调整机器图默认透明度 */
            opacity: 0.5;
            position: relative;
        }
        
        /* 机器图片点亮遮罩 */
        .machine-mask {
            position: absolute;
            top: 0;
            left: 0;
            width: 0%;
            height: 100%;
            background-image: url('https://youke1.picui.cn/s1/2025/11/25/69251c3132147.png');
            background-size: cover;
            opacity: 1;
            transition: width 0.5s ease;
            z-index: 4;
        }
        
        /* 进度显示 */
        .progress-display {
            position: absolute;
            z-index: 5;
            /* 进度显示位置 - 在机器正下方 */
            top: calc(50% + 380px);  /* 可调整进度显示与机器的垂直距离 */
            left: 50%;
            transform: translateX(-50%);
            /* 进度显示样式设置 - 可在此调整颜色、字体、字号 */
            color: #ffffff;
            font-size: 50px;
            font-weight: bold;
            text-shadow: 0 0 10px rgba(0, 150, 255, 0.8);
            text-align: center;
        }
        
        /* 输入框容器 */
        .input-container {
            position: absolute;
            z-index: 6;
            /* 输入框位置设置 */
            right: 100px;  /* 距离右边距 */
            bottom: 50px; /* 距离底部高度 */
            /* 输入框整体样式设置 */
            opacity: 0.05;  /* 整体透明度 */
            background-color: #000000;  /* 整体颜色 */
            padding: 15px;
            border-radius: 8px;
            width: 200px;  /* 整体大小宽度 */
        }
        
        /* 输入框 */
        .progress-input {
            width: 100%;
            padding: 8px;
            background-color: transparent;
            border: 1px solid #555;
            border-radius: 4px;
            color: white;
            text-align: center;
            font-size: 18px;
        }
        
        /* 浮动进度效果 */
        .progress-float {
            position: absolute;
            z-index: 7;
            color: #4fc3f7;
            font-size: 36px;
            font-weight: bold;
            text-shadow: 0 0 15px rgba(79, 195, 247, 0.9);
            animation: floatUp 3s forwards;
        }
        
        @keyframes floatUp {
            0% {
                opacity: 1;
                transform: translateY(0);
            }
            100% {
                opacity: 0;
                transform: translateY(-100px);
            }
        }
        
        /* 礼花效果容器 */
        #confetti-js {
            position: absolute;
            width: 100%;
            height: 100%;
            z-index: 8;
            pointer-events: none;
            display: none;
        }
        
        /* 底部版权信息 */
        .footer {
            position: absolute;
            z-index: 9;
            bottom: 20px;
            width: 100%;
            text-align: center;
            /* 版权信息样式设置 */
            color: #000000;
            opacity: 0.78;  /* 透明度 */
            font-size: 14px;  /* 字号 */
        }
    </style>
</head>
<body>
    <!-- 背景图片 -->
    <div class="background"></div>
    
    <!-- 粒子效果容器 -->
    <div id="particles-js"></div>
    
    <!-- 机器图片容器 -->
    <div class="machine-container">
        <img src="https://youke1.picui.cn/s1/2025/11/25/69251c3132147.png" alt="晶品机器" class="machine-image">
        <div class="machine-mask"></div>
    </div>
    
    <!-- 进度显示 -->
    <div class="progress-display">0%</div>
    
    <!-- 输入框 -->
    <div class="input-container">
        <input type="number" class="progress-input" value="0" min="0" max="100" placeholder="输入进度值">
    </div>
    
    <!-- 礼花效果容器 -->
    <canvas id="confetti-js"></canvas>
    
    <!-- 底部版权信息 -->
    <div class="footer">© 2025 XXX公司 市场宣传组 | PHOENIX</div>记得修改公司名字
    
    <!-- 引入粒子效果库 -->
    <script src="https://cdn.jsdelivr.net/particles.js/2.0.0/particles.min.js"></script>
    <script>
        // 初始化变量
        let currentProgress = 0;
        let isCharging = false;
        
        // 音频文件设置 - 可在此更换音效链接
        const chargeSound = new Audio('D:\工作\11月\2025年会\能量动画粒子音效8秒.mp3');
        
        // 初始化粒子效果
        particlesJS('particles-js', {
            particles: {
                number: {
                    value: 80,  // 粒子数量 - 可调整
                    density: {
                        enable: true,
                        value_area: 800
                    }
                },
                color: {
                    value: "#4fc3f7"  // 粒子颜色 - 可调整
                },
                shape: {
                    type: "circle"
                },
                opacity: {
                    value: 0.3,  // 粒子透明度 - 可调整
                    random: true,
                    anim: {
                        enable: true,
                        speed: 0.5,
                        opacity_min: 0.1,
                        sync: false
                    }
                },
                size: {
                    value: 3,  // 粒子大小 - 可调整
                    random: true
                },
                line_linked: {
                    enable: false
                },
                move: {
                    enable: true,
                    speed: 1,  // 粒子移动速度 - 可调整
                    direction: "center",  // 粒子移动方向 - 向中心汇集
                    random: true,
                    straight: false,
                    out_mode: "out",
                    bounce: false,
                    attract: {
                        enable: true,
                        rotateX: 600,
                        rotateY: 1200
                    }
                }
            },
            interactivity: {
                detect_on: "canvas",
                events: {
                    onhover: {
                        enable: false
                    },
                    onclick: {
                        enable: false
                    },
                    resize: true
                }
            },
            retina_detect: true
        });
        
        // 输入框事件监听
        document.querySelector('.progress-input').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                const value = parseInt(this.value);
                if (!isNaN(value) && value > 0 && value <= 100 && currentProgress < 100) {
                    addProgress(value);
                    this.value = 0;  // 重置输入框
                }
            }
        });
        
        // 添加进度函数
        function addProgress(value) {
            if (currentProgress >= 100 || isCharging) return;
            
            isCharging = true;
            
            // 播放充能音效
            chargeSound.play();
            
            // 加速粒子效果
            particlesJS('particles-js', {
                particles: {
                    number: {
                        value: 150,  // 增加粒子数量
                        density: {
                            enable: true,
                            value_area: 800
                        }
                    },
                    color: {
                        value: "#ffffff"  // 改变粒子颜色为白色
                    },
                    opacity: {
                        value: 0.8,  // 增加粒子透明度
                        random: true,
                        anim: {
                            enable: true,
                            speed: 1,  // 加快动画速度
                            opacity_min: 0.3,
                            sync: false
                        }
                    },
                    size: {
                        value: 5,  // 增加粒子大小
                        random: true
                    },
                    move: {
                        enable: true,
                        speed: 3,  // 加快粒子移动速度
                        direction: "center",
                        random: true,
                        straight: false,
                        out_mode: "out",
                        bounce: false,
                        attract: {
                            enable: true,
                            rotateX: 600,
                            rotateY: 1200
                        }
                    }
                },
                interactivity: {
                    detect_on: "canvas",
                    events: {
                        onhover: {
                            enable: false
                        },
                        onclick: {
                            enable: false
                        },
                        resize: true
                    }
                },
                retina_detect: true
            });
            
            // 显示浮动进度效果
            const floatElement = document.createElement('div');
            floatElement.className = 'progress-float';
            floatElement.textContent = `+${value}%`;
            floatElement.style.left = '50%';
            floatElement.style.top = '50%';
            floatElement.style.transform = 'translateX(-50%)';
            document.body.appendChild(floatElement);
            
            // 3秒后恢复粒子效果并更新进度
            setTimeout(() => {
                // 移除浮动元素
                floatElement.remove();
                
                // 计算新进度
                const newProgress = Math.min(currentProgress + value, 100);
                currentProgress = newProgress;
                
                // 更新进度显示
                updateProgressDisplay();
                
                // 恢复粒子效果
                particlesJS('particles-js', {
                    particles: {
                        number: {
                            value: 80,
                            density: {
                                enable: true,
                                value_area: 800
                            }
                        },
                        color: {
                            value: "#4fc3f7"
                        },
                        opacity: {
                            value: 0.3,
                            random: true,
                            anim: {
                                enable: true,
                                speed: 0.5,
                                opacity_min: 0.1,
                                sync: false
                            }
                        },
                        size: {
                            value: 3,
                            random: true
                        },
                        move: {
                            enable: true,
                            speed: 1,
                            direction: "center",
                            random: true,
                            straight: false,
                            out_mode: "out",
                            bounce: false,
                            attract: {
                                enable: true,
                                rotateX: 600,
                                rotateY: 1200
                            }
                        }
                    },
                    interactivity: {
                        detect_on: "canvas",
                        events: {
                            onhover: {
                                enable: false
                            },
                            onclick: {
                                enable: false
                            },
                            resize: true
                        }
                    },
                    retina_detect: true
                });
                
                isCharging = false;
                
                // 如果进度达到100%，触发完成效果
                if (currentProgress >= 100) {
                    triggerCompletion();
                }
            }, 3000);
        }
        
        // 更新进度显示
        function updateProgressDisplay() {
            const progressDisplay = document.querySelector('.progress-display');
            const machineMask = document.querySelector('.machine-mask');
            
            // 更新进度文字
            progressDisplay.textContent = `${currentProgress}%`;
            
            // 更新机器点亮遮罩
            machineMask.style.width = `${currentProgress}%`;
        }
        
        // 触发完成效果
        function triggerCompletion() {
            // 播放完成音效
            completeSound.play();
            
            // 显示礼花效果
            const confettiCanvas = document.getElementById('confetti-js');
            confettiCanvas.style.display = 'block';
            
            // 简单的礼花效果实现
            const confettiSettings = { target: 'confetti-js', max: 150, size: 1.5, animate: true, props: ['circle', 'square', 'triangle', 'line'], colors: [[165,104,246],[230,61,135],[0,199,228],[253,214,126]], clock: 25, rotate: true, width: window.innerWidth, height: window.innerHeight, start_from_edge: true, respawn: false };
            const confetti = new ConfettiGenerator(confettiSettings);
            confetti.render();
            
            // 3秒后隐藏礼花效果
            setTimeout(() => {
                confettiCanvas.style.display = 'none';
                confetti.clear();
            }, 3000);
        }
        
        // 简单的礼花效果实现
        class ConfettiGenerator {
            constructor(settings) {
                this.settings = settings;
                this.canvas = document.getElementById(settings.target);
                this.ctx = this.canvas.getContext('2d');
                this.particles = [];
                this.w = window.innerWidth;
                this.h = window.innerHeight;
                
                this.canvas.width = this.w;
                this.canvas.height = this.h;
                
                this.createParticles();
                this.animate = this.animate.bind(this);
            }
            
            createParticles() {
                for (let i = 0; i < this.settings.max; i++) {
                    this.particles.push({
                        x: Math.random() * this.w,
                        y: Math.random() * this.h - this.h,
                        color: this.settings.colors[Math.floor(Math.random() * this.settings.colors.length)],
                        shape: this.settings.props[Math.floor(Math.random() * this.settings.props.length)],
                        size: Math.random() * this.settings.size + 0.5,
                        speed: Math.random() * 10 + 5,
                        rotation: Math.random() * 360,
                        rotationSpeed: Math.random() * 10 + 2
                    });
                }
            }
            
            drawParticles() {
                for (let i = 0; i < this.particles.length; i++) {
                    const p = this.particles[i];
                    
                    this.ctx.save();
                    this.ctx.translate(p.x, p.y);
                    this.ctx.rotate(p.rotation * Math.PI / 180);
                    this.ctx.fillStyle = `rgb(${p.color[0]}, ${p.color[1]}, ${p.color[2]})`;
                    
                    if (p.shape === 'circle') {
                        this.ctx.beginPath();
                        this.ctx.arc(0, 0, p.size, 0, 2 * Math.PI);
                        this.ctx.fill();
                    } else if (p.shape === 'square') {
                        this.ctx.fillRect(-p.size, -p.size, p.size * 2, p.size * 2);
                    } else if (p.shape === 'triangle') {
                        this.ctx.beginPath();
                        this.ctx.moveTo(0, -p.size);
                        this.ctx.lineTo(-p.size, p.size);
                        this.ctx.lineTo(p.size, p.size);
                        this.ctx.closePath();
                        this.ctx.fill();
                    } else if (p.shape === 'line') {
                        this.ctx.fillRect(-p.size/2, -p.size*2, p.size, p.size*4);
                    }
                    
                    this.ctx.restore();
                }
            }
            
            updateParticles() {
                for (let i = 0; i < this.particles.length; i++) {
                    const p = this.particles[i];
                    
                    p.y += p.speed;
                    p.rotation += p.rotationSpeed;
                    
                    if (p.y > this.h) {
                        if (this.settings.respawn) {
                            p.y = -10;
                            p.x = Math.random() * this.w;
                        } else {
                            this.particles.splice(i, 1);
                            i--;
                        }
                    }
                }
                
                if (this.particles.length === 0 && this.settings.animate) {
                    this.clear();
                }
            }
            
            animate() {
                if (this.settings.animate) {
                    requestAnimationFrame(this.animate);
                }
                
                this.ctx.clearRect(0, 0, this.w, this.h);
                this.drawParticles();
                this.updateParticles();
            }
            
            render() {
                this.animate();
            }
            
            clear() {
                this.ctx.clearRect(0, 0, this.w, this.h);
                this.particles = [];
            }
        }
        
        // 窗口大小调整时更新画布尺寸
        window.addEventListener('resize', function() {
            const confettiCanvas = document.getElementById('confetti-js');
            confettiCanvas.width = window.innerWidth;
            confettiCanvas.height = window.innerHeight;
        });
    </script>
</body>
</html>
