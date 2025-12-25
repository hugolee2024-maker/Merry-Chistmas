# Merry-Chistmas
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>程序员的浪漫圣诞树</title>
    <script src="https://cdn.jsdelivr.net/npm/three@0.132.2/build/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.132.2/examples/js/controls/OrbitControls.js"></script>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background: #000;
        }
        .info {
            position: absolute;
            top: 10px;
            left: 10px;
            color: #fff;
            font-family: Arial, sans-serif;
        }
    </style>
</head>
<body>
    <div class="info">Merry Christmas</div>
    <script>
        // 初始化场景、相机、渲染器
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.z = 5;

        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // 轨道控制器，实现交互
        const controls = new OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;

        // 生成粒子数量
        const particleCount = 8000;
        const particles = new THREE.BufferGeometry();
        const positions = new Float32Array(particleCount * 3);
        const colors = new Float32Array(particleCount * 3);

        // 圣诞树形状参数
        const radius = 3;
        const height = 5;
        const segments = 16;
        const thetaStart = 0;
        const thetaLength = Math.PI * 2;

        // 生成圣诞树形状的粒子
        for (let i = 0; i < particleCount; i++) {
            const t = Math.random();
            const y = t * height;
            const r = radius * (1 - t);
            const theta = thetaStart + Math.random() * thetaLength;
            const x = r * Math.cos(theta);
            const z = r * Math.sin(theta);

            positions[i * 3] = x;
            positions[i * 3 + 1] = y;
            positions[i * 3 + 2] = z;

            // 颜色渐变，底部偏绿，顶部偏粉
            const color = new THREE.Color();
            color.lerpColors(new THREE.Color(0x228B22), new THREE.Color(0xFF69B4), t);
            colors[i * 3] = color.r;
            colors[i * 3 + 1] = color.g;
            colors[i * 3 + 2] = color.b;
        }

        particles.setAttribute('position', new THREE.BufferAttribute(positions, 3));
        particles.setAttribute('color', new THREE.BufferAttribute(colors, 3));

        // 粒子材质
        const material = new THREE.PointsMaterial({
            size: 0.05,
            vertexColors: true,
            transparent: true,
            opacity: 0.8
        });

        const particleSystem = new THREE.Points(particles, material);
        scene.add(particleSystem);

        // 顶部装饰球
        const sphereGeometry = new THREE.SphereGeometry(0.2, 16, 16);
        const sphereMaterial = new THREE.MeshBasicMaterial({ color: 0xFF69B4 });
        const sphere = new THREE.Mesh(sphereGeometry, sphereMaterial);
        sphere.position.y = height / 2;
        scene.add(sphere);

        // 窗口大小变化时调整
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });

        // 动画循环
        function animate() {
            requestAnimationFrame(animate);
            controls.update();
            renderer.render(scene, camera);
        }
        animate();
    </script>
</body>
</html>
