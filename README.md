<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Цифровой этнограф — карта культурного наследия</title>

    <script src="https://api-maps.yandex.ru/2.1/?apikey=9d08746c-663c-48f8-a58c-ada00fcc5d04&lang=ru_RU"></script>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #f5f0eb;
            color: #2d2a24;
        }

        /* ===== ШАПКА ===== */
        .header {
            background: linear-gradient(135deg, #2d1f14, #4a3426);
            color: #f5ede4;
            padding: 15px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
            position: sticky;
            top: 0;
            z-index: 1000;
        }

        .header h1 {
            font-size: 24px;
            font-weight: 700;
        }

        .header h1 span {
            color: #e6c9a8;
        }

        .header-actions {
            display: flex;
            gap: 12px;
            align-items: center;
            flex-wrap: wrap;
        }

        .btn {
            padding: 9px 20px;
            border: none;
            border-radius: 30px;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: inline-flex;
            align-items: center;
            gap: 6px;
        }

        .btn-primary { background: #e6c9a8; color: #2d1f14; }
        .btn-primary:hover { background: #dbb58c; transform: translateY(-1px); }
        
        .btn-secondary { background: transparent; color: #f5ede4; border: 2px solid #e6c9a8; }
        .btn-secondary:hover { background: #e6c9a8; color: #2d1f14; }

        .btn-success { background: #2d7d46; color: #fff; }
        .btn-success:hover { background: #236a39; }

        .btn-danger { background: #c0392b; color: #fff; }
        .btn-danger:hover { background: #a93226; }

        .btn-outline { background: transparent; border: 2px solid #ddd; color: #555; }
        .btn-outline:hover { border-color: #999; background: #f5f5f5; }

        /* ===== ОСНОВНОЙ КОНТЕНТ ===== */
        .main-wrapper {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
        }

        .banner {
            background: linear-gradient(135deg, #4a3426, #6b4f3a);
            color: #f5ede4;
            padding: 25px 35px;
            border-radius: 16px;
            margin-bottom: 25px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 20px;
        }

        .banner h2 { font-size: 26px; }
        .banner h2 span { color: #e6c9a8; }
        .banner p { font-size: 15px; opacity: 0.9; max-width: 600px; margin-top: 5px; }

        .content-grid {
            display: grid;
            grid-template-columns: 1fr 380px;
            gap: 25px;
        }

        @media (max-width: 900px) {
            .content-grid { grid-template-columns: 1fr; }
        }

        .map-container {
            background: #fff;
            border-radius: 16px;
            overflow: hidden;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
            height: 550px;
            position: relative;
        }

        #map { width: 100%; height: 100%; }

        .sidebar {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .card {
            background: #fff;
            border-radius: 16px;
            padding: 20px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
        }

        .card h3 {
            font-size: 16px;
            color: #6b4f3a;
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .search-box {
            width: 100%;
            padding: 10px 14px;
            border: 2px solid #e8e0d8;
            border-radius: 10px;
            font-size: 14px;
            margin-bottom: 10px;
            outline: none;
        }
        .search-box:focus { border-color: #6b4f3a; }

        .filter-row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
            margin-bottom: 12px;
        }

        .filter-row select {
            width: 100%;
            padding: 9px 12px;
            border: 2px solid #e8e0d8;
            border-radius: 10px;
            font-size: 13px;
            background: #fff;
            outline: none;
        }
        .filter-row select:focus { border-color: #6b4f3a; }

        .object-list {
            list-style: none;
            max-height: 380px;
            overflow-y: auto;
        }

        .object-list li {
            padding: 12px;
            border-bottom: 1px solid #f0ebe6;
            cursor: pointer;
            border-radius: 8px;
            display: flex;
            align-items: center;
            gap: 12px;
            transition: background 0.2s;
        }

        .object-list li:hover { background: #f8f3ef; }
        .object-list li .emoji-big { font-size: 28px; }
        .object-list li .info { flex: 1; }
        .object-list li .info .name { font-weight: 600; font-size: 14px; }
        .object-list li .info .region { font-size: 12px; color: #888; }

        .stats {
            display: flex;
            gap: 10px;
        }
        .stats .stat-item {
            flex: 1;
            text-align: center;
            background: #f8f3ef;
            padding: 12px 8px;
            border-radius: 12px;
        }
        .stats .stat-item .number { font-size: 22px; font-weight: 700; color: #2d1f14; }
        .stats .stat-item .label { font-size: 11px; color: #888; margin-top: 2px; }

        /* ===== МОДАЛЬНЫЕ ОКНА ===== */
        .modal-overlay {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.65);
            backdrop-filter: blur(4px);
            z-index: 2000;
            justify-content: center;
            align-items: center;
        }

        .modal-overlay.active { display: flex; }

        .modal {
            background: #fff;
            border-radius: 20px;
            max-width: 700px;
            width: 92%;
            max-height: 90vh;
            overflow-y: auto;
            padding: 30px 35px;
            position: relative;
            animation: modalFade 0.3s ease;
        }

        @keyframes modalFade {
            from { opacity: 0; transform: scale(0.95); }
            to { opacity: 1; transform: scale(1); }
        }

        .modal-close {
            position: absolute;
            top: 15px; right: 20px;
            font-size: 28px;
            cursor: pointer;
            background: none; border: none; color: #aaa;
        }
        .modal-close:hover { color: #333; }

        .modal h2 { font-size: 24px; color: #2d1f14; margin-bottom: 4px; }
        .modal .modal-region { color: #888; font-size: 14px; margin-bottom: 15px; }

        .modal-image {
            width: 100%; height: 200px;
            background: #f0ebe6; border-radius: 12px;
            display: flex; align-items: center; justify-content: center;
            font-size: 60px; margin-bottom: 15px; overflow: hidden;
        }
        .modal-image img { width: 100%; height: 100%; object-fit: cover; }

        .modal-3d-container {
            width: 100%; height: 240px;
            background: #111; border-radius: 12px;
            margin: 15px 0; position: relative; overflow: hidden;
        }

        .modal-3d-label {
            position: absolute; bottom: 10px; left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.7); color: #fff;
            padding: 4px 14px; border-radius: 20px; font-size: 12px;
        }

        .modal-actions {
            display: flex; gap: 10px; margin-top: 20px; flex-wrap: wrap;
        }

        /* ===== ФОРМА ДОБАВЛЕНИЯ С ИИ ===== */
        .add-form input, .add-form textarea, .add-form select {
            width: 100%; padding: 10px 14px;
            border: 2px solid #e8e0d8; border-radius: 10px;
            font-size: 14px; margin-bottom: 12px; font-family: inherit; outline: none;
        }
        .add-form input:focus, .add-form textarea:focus, .add-form select:focus { border-color: #6b4f3a; }
        .add-form textarea { min-height: 80px; resize: vertical; }

        .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }

        .upload-area {
            border: 2px dashed #e8e0d8; border-radius: 12px;
            padding: 20px; text-align: center; cursor: pointer;
            margin-bottom: 12px; color: #777; transition: 0.2s;
        }
        .upload-area:hover { border-color: #6b4f3a; background: #faf8f5; }
        .upload-area input[type="file"] { display: none; }

        .ai-status-bar {
            background: #f0f4f8; border-radius: 10px; padding: 10px 14px;
            margin-bottom: 12px; font-size: 13px; color: #555;
            display: flex; align-items: center; gap: 10px;
        }

        /* ===== TOAST УВЕДОМЛЕНИЯ ===== */
        .toast-container {
            position: fixed; bottom: 20px; right: 20px;
            z-index: 3000; display: flex; flex-direction: column; gap: 10px;
        }
        .toast {
            background: #2d1f14; color: #fff; padding: 12px 20px;
            border-radius: 10px; font-size: 14px; box-shadow: 0 4px 12px rgba(0,0,0,0.2);
            animation: toastIn 0.3s ease;
        }
        @keyframes toastIn { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
    </style>
</head>
<body>

    <header class="header">
        <div>
            <h1>Цифровой <span>этнограф</span></h1>
            <div style="font-size: 13px; opacity: 0.8;">Платформа сохранении культурного наследия России</div>
        </div>
        <div class="header-actions">
            <button class="btn btn-secondary" id="openCollectionBtn">⭐ Избранное (<span id="savedCount">0</span>)</button>
            <button class="btn btn-primary" id="openAddModalBtn">➕ Добавить объект (ИИ)</button>
        </div>
    </header>

    <div class="main-wrapper">
        <div class="banner">
            <div>
                <h2>Интерактивная карта <span>наследия</span></h2>
                <p>Исследуйте этнографические объекты, слушайте аудиогиды и просматривайте 3D-модели традиционных предметов.</p>
            </div>
            <div class="btn btn-primary" style="pointer-events: none;">ИИ Yandex Vision & GPT</div>
        </div>

        <div class="content-grid">
            <div class="map-container">
                <div id="map"></div>
            </div>

            <div class="sidebar">
                <div class="card">
                    <h3>🔍 Поиск и фильтры</h3>
                    <input type="text" class="search-box" id="searchInput" placeholder="Поиск по названию или описанию..." />
                    <div class="filter-row">
                        <select id="categoryFilter">
                            <option value="">Все категории</option>
                        </select>
                        <select id="regionFilter">
                            <option value="">Все регионы</option>
                        </select>
                    </div>
                </div>

                <div class="card">
                    <h3>📊 Статистика</h3>
                    <div class="stats">
                        <div class="stat-item">
                            <div class="number" id="statTotal">0</div>
                            <div class="label">Объектов</div>
                        </div>
                        <div class="stat-item">
                            <div class="number" id="statRegions">0</div>
                            <div class="label">Регионов</div>
                        </div>
                        <div class="stat-item">
                            <div class="number" id="statSaves">0</div>
                            <div class="label">Сохранений</div>
                        </div>
                    </div>
                </div>

                <div class="card">
                    <h3>📜 Список объектов</h3>
                    <ul class="object-list" id="objectList"></ul>
                </div>
            </div>
        </div>
    </div>

    <div class="modal-overlay" id="objectModal">
        <div class="modal">
            <button class="modal-close" onclick="closeModal('objectModal')">&times;</button>
            <h2 id="modalTitle">Название объекта</h2>
            <div class="modal-region" id="modalRegion">Регион, Категория</div>
            
            <div class="modal-image" id="modalImage">🏰</div>

            <p id="modalDescription" style="line-height: 1.6; color: #444; margin-bottom: 15px;"></p>

            <div class="modal-3d-container" id="modal3dContainer">
                <div class="modal-3d-label">🧊 Интерактивная 3D-модель (Вращайте мышью)</div>
            </div>

            <div class="modal-actions">
                <button class="btn btn-primary" id="modalAudioBtn">🔊 Слушать аудиогид</button>
                <button class="btn btn-secondary" id="modalSaveBtn" style="color: #2d1f14;">⭐ В избранное</button>
                <button class="btn btn-outline" id="modalReportBtn">🚩 Пожаловаться</button>
            </div>
        </div>
    </div>

    <div class="modal-overlay" id="addModal">
        <div class="modal">
            <button class="modal-close" onclick="closeModal('addModal')">&times;</button>
            <h2>Добавить этнографический объект</h2>
            <p style="font-size: 13px; color: #777; margin-bottom: 15px;">Загрузите фото — искусственный интеллект автоматически определит категорию и заполнит данные.</p>

            <form class="add-form" id="addObjectForm">
                <div class="upload-area" id="uploadArea">
                    📷 Нажмите или перетащите сюда изображение
                    <input type="file" id="fileInput" accept="image/*" />
                    <div id="previewContainer" style="margin-top: 10px;"></div>
                </div>

                <div class="ai-status-bar" id="aiStatus">
                    🤖 ИИ готов к анализу загруженного фото...
                </div>

                <input type="text" id="addName" placeholder="Название объекта *" required />
                
                <div class="form-row">
                    <select id="addCategory" required>
                        <option value="">Выберите категорию *</option>
                        <option value="Архитектура">Архитектура</option>
                        <option value="Ремесло">Ремесло</option>
                        <option value="Одежда">Одежда</option>
                        <option value="Быт">Быт</option>
                        <option value="Музыка">Музыка</option>
                    </select>
                    <input type="text" id="addRegion" placeholder="Регион (напр. Карелия) *" required />
                </div>

                <div class="form-row">
                    <input type="number" step="any" id="addLat" placeholder="Широта (Lat)" required />
                    <input type="number" step="any" id="addLng" placeholder="Долгота (Lng)" required />
                </div>

                <textarea id="addDescription" placeholder="Подробное описание..." required></textarea>

                <div style="display: flex; gap: 10px; justify-content: flex-end; margin-top: 10px;">
                    <button type="button" class="btn btn-outline" onclick="closeModal('addModal')">Отмена</button>
                    <button type="submit" class="btn btn-success">Опубликовать на карту</button>
                </div>
            </form>
        </div>
    </div>

    <div class="modal-overlay" id="collectionModal">
        <div class="modal">
            <button class="modal-close" onclick="closeModal('collectionModal')">&times;</button>
            <h2>⭐ Ваше избранное</h2>
            <p style="font-size: 13px; color: #777; margin-bottom: 15px;">Сохранённые объекты культурного наследия.</p>
            <ul class="object-list" id="collectionList"></ul>
        </div>
    </div>

    <div class="toast-container" id="toastContainer"></div>

    <script>
        // ============================================================
        // 1. ДАННЫЕ ОБЪЕКТОВ
        // ============================================================
        let culturalObjects = [
            {
                id: 1,
                name: "Кижский погост",
                category: "Архитектура",
                region: "Республика Карелия",
                lat: 62.0678,
                lng: 35.2231,
                emoji: "🕌",
                description: "Всемирно известный ансамбль деревянного зодчества, состоящий из двух церквей и колокольни XVIII-XIX веков, построенных без единого гвоздя.",
                saves: 14,
                shape: "building"
            },
            {
                id: 2,
                name: "Хохломская роспись",
                category: "Ремесло",
                region: "Нижегородская область",
                lat: 56.8584,
                lng: 44.5222,
                emoji: "🎨",
                description: "Старинный русский народный промысел, родившийся в XVII веке. Отличается золотистым орнаментом на черном или красном фоне.",
                saves: 28,
                shape: "bowl"
            },
            {
                id: 3,
                name: "Татарский ичиг",
                category: "Одежда",
                region: "Республика Татарстан",
                lat: 55.7887,
                lng: 49.1221,
                emoji: "👢",
                description: "Традиционные мягкие сапоги с красивой мозаикой из цветной кожи. Настоящее произведение национального декоративно-прикладного искусства.",
                saves: 9,
                shape: "boot"
            },
            {
                id: 4,
                name: "Русская печь",
                category: "Быт",
                region: "Вологодская область",
                lat: 59.2205,
                lng: 39.8915,
                emoji: "🧱",
                description: "Центральный элемент традиционного русского жилища, использовавшийся для обогрева, приготовления пищи и сна.",
                saves: 42,
                shape: "cube"
            }
        ];

        let savedIds = new Set();
        let yMap = null;
        let mapPlacemarks = [];
        let currentObjectId = null;
        let threeScene, threeCamera, threeRenderer, threeMesh, animationFrameId;

        // ============================================================
        // 2. ИНИЦИАЛИЗАЦИЯ И КАРТА YANDEX
        // ============================================================
        function initYandexMap() {
            if (typeof ymaps === 'undefined') {
                showToast('Ошибка загрузки Яндекс Карт', 'error');
                return;
            }
            ymaps.ready(() => {
                yMap = new ymaps.Map("map", {
                    center: [58.0000, 42.0000],
                    zoom: 5,
                    controls: ['zoomControl', 'fullscreenControl']
                });
                renderMarkers();
            });
        }

        function renderMarkers() {
            if (!yMap) return;
            yMap.geoObjects.removeAll();
            mapPlacemarks = [];

            const filtered = getFilteredObjects();

            filtered.forEach(obj => {
                const placemark = new ymaps.Placemark([obj.lat, obj.lng], {
                    balloonContentHeader: `<b>${obj.name}</b>`,
                    balloonContentBody: `<p>${obj.region}</p><button onclick="openObjectModal(${obj.id})" style="padding:4px 10px; background:#e6c9a8; border:none; border-radius:10px; cursor:pointer; margin-top:5px;">Открыть подробнее</button>`,
                    hintContent: obj.name
                }, {
                    preset: 'islands#brownIcon'
                });

                placemark.events.add('click', () => {
                    openObjectModal(obj.id);
                });

                yMap.geoObjects.add(placemark);
                mapPlacemarks.push(placemark);
            });
        }

        // ============================================================
        // 3. ФИЛЬТРАЦИЯ И ОТОБРАЖЕНИЕ СПИСКА
        // ============================================================
        function getFilteredObjects() {
            const search = document.getElementById('searchInput').value.toLowerCase();
            const cat = document.getElementById('categoryFilter').value;
            const reg = document.getElementById('regionFilter').value;

            return culturalObjects.filter(obj => {
                const matchSearch = obj.name.toLowerCase().includes(search) || obj.description.toLowerCase().includes(search);
                const matchCat = !cat || obj.category === cat;
                const matchReg = !reg || obj.region === reg;
                return matchSearch && matchCat && matchReg;
            });
        }

        function renderList() {
            const listEl = document.getElementById('objectList');
            const filtered = getFilteredObjects();
            listEl.innerHTML = '';

            if (filtered.length === 0) {
                listEl.innerHTML = '<li style="color:#888;">Ничего не найдено</li>';
                return;
            }

            filtered.forEach(obj => {
                const li = document.createElement('li');
                li.innerHTML = `
                    <span class="emoji-big">${obj.emoji}</span>
                    <div class="info">
                        <div class="name">${obj.name}</div>
                        <div class="region">${obj.region} • ${obj.category}</div>
                    </div>
                    <span style="font-size:12px; color:#888;">⭐ ${obj.saves}</span>
                `;
                li.onclick = () => {
                    if (yMap) yMap.setCenter([obj.lat, obj.lng], 8, { duration: 500 });
                    openObjectModal(obj.id);
                };
                listEl.appendChild(li);
            });

            updateStats();
        }

        function populateFilters() {
            const catSelect = document.getElementById('categoryFilter');
            const regSelect = document.getElementById('regionFilter');

            const categories = [...new Set(culturalObjects.map(o => o.category))];
            const regions = [...new Set(culturalObjects.map(o => o.region))];

            catSelect.innerHTML = '<option value="">Все категории</option>' + categories.map(c => `<option value="${c}">${c}</option>`).join('');
            regSelect.innerHTML = '<option value="">Все регионы</option>' + regions.map(r => `<option value="${r}">${r}</option>`).join('');
        }

        function updateStats() {
            document.getElementById('statTotal').innerText = culturalObjects.length;
            document.getElementById('statRegions').innerText = new Set(culturalObjects.map(o => o.region)).size;
            document.getElementById('statSaves').innerText = culturalObjects.reduce((acc, o) => acc + (o.saves || 0), 0);
            document.getElementById('savedCount').innerText = savedIds.size;
        }

        // ============================================================
        // 4. МОДАЛЬНЫЕ ОКНА И 3D THREE.JS
        // ============================================================
        function openObjectModal(id) {
            const obj = culturalObjects.find(o => o.id === id);
            if (!obj) return;

            currentObjectId = id;
            document.getElementById('modalTitle').innerText = obj.name;
            document.getElementById('modalRegion').innerText = `${obj.region} • Категория: ${obj.category}`;
            document.getElementById('modalDescription').innerText = obj.description;
            
            const imgContainer = document.getElementById('modalImage');
            if (obj.imageUrl) {
                imgContainer.innerHTML = `<img src="${obj.imageUrl}" alt="${obj.name}">`;
            } else {
                imgContainer.innerHTML = obj.emoji;
            }

            // Обновляем состояние кнопки Избранного
            const saveBtn = document.getElementById('modalSaveBtn');
            saveBtn.innerText = savedIds.has(id) ? '🌟 В избранном' : '⭐ В избранное';

            document.getElementById('objectModal').classList.add('active');
            init3DScene(obj.shape);
        }

        function closeModal(modalId) {
            document.getElementById(modalId).classList.remove('active');
            if (window.speechSynthesis) window.speechSynthesis.cancel();
            if (animationFrameId) cancelAnimationFrame(animationFrameId);
        }

        function init3DScene(shapeType) {
            const container = document.getElementById('modal3dContainer');
            // Очищаем старый canvas
            const oldCanvas = container.querySelector('canvas');
            if (oldCanvas) oldCanvas.remove();

            const width = container.clientWidth || 600;
            const height = container.clientHeight || 240;

            threeScene = new THREE.Scene();
            threeScene.background = new THREE.Color(0x1a1a2e);

            threeCamera = new THREE.PerspectiveCamera(45, width / height, 0.1, 1000);
            threeCamera.position.z = 5;

            threeRenderer = new THREE.WebGLRenderer({ antialias: true });
            threeRenderer.setSize(width, height);
            container.appendChild(threeRenderer.domElement);

            // Источники света
            const ambientLight = new THREE.AmbientLight(0xffffff, 0.7);
            threeScene.add(ambientLight);

            const dirLight = new THREE.DirectionalLight(0xe6c9a8, 0.8);
            dirLight.position.set(5, 5, 5);
            threeScene.add(dirLight);

            // Геометрия
            let geometry;
            if (shapeType === 'building') {
                geometry = new THREE.ConeGeometry(1.5, 2, 8);
            } else if (shapeType === 'bowl') {
                geometry = new THREE.CylinderGeometry(1.5, 0.8, 1, 16);
            } else if (shapeType === 'boot') {
                geometry = new THREE.BoxGeometry(1, 2, 1.5);
            } else {
                geometry = new THREE.BoxGeometry(1.5, 1.5, 1.5);
            }

            const material = new THREE.MeshStandardMaterial({
                color: 0xe6c9a8,
                roughness: 0.4,
                metalness: 0.2
            });

            threeMesh = new THREE.Mesh(geometry, material);
            threeScene.add(threeMesh);

            // Интерактивное вращение мышью
            let isDragging = false;
            let previousMousePosition = { x: 0, y: 0 };

            const canvas = threeRenderer.domElement;
            canvas.onmousedown = (e) => { isDragging = true; previousMousePosition = { x: e.clientX, y: e.clientY }; };
            canvas.onmousemove = (e) => {
                if (!isDragging) return;
                const deltaX = e.clientX - previousMousePosition.x;
                const deltaY = e.clientY - previousMousePosition.y;
                threeMesh.rotation.y += deltaX * 0.01;
                threeMesh.rotation.x += deltaY * 0.01;
                previousMousePosition = { x: e.clientX, y: e.clientY };
            };
            window.onmouseup = () => { isDragging = false; };

            function animate() {
                animationFrameId = requestAnimationFrame(animate);
                if (!isDragging) {
                    threeMesh.rotation.y += 0.008;
                }
                threeRenderer.render(threeScene, threeCamera);
            }
            animate();
        }

        // ============================================================
        // 5. КНОПКИ ДЕЙСТВИЙ (АУДИО, ИЗБРАННОЕ, ЖАЛОБЫ)
        // ============================================================
        document.getElementById('modalAudioBtn').onclick = () => {
            const obj = culturalObjects.find(o => o.id === currentObjectId);
            if (!obj) return;

            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel(); // сброс
                const text = `${obj.name}. ${obj.region}. ${obj.description}`;
                const utterance = new SpeechSynthesisUtterance(text);
                utterance.lang = 'ru-RU';
                utterance.rate = 0.9;
                window.speechSynthesis.speak(utterance);
                showToast('🔊 Воспроизведение аудиогида...');
            } else {
                showToast('Синтез речи не поддерживается вашим браузером', 'error');
            }
        };

        document.getElementById('modalSaveBtn').onclick = () => {
            if (!currentObjectId) return;
            const obj = culturalObjects.find(o => o.id === currentObjectId);
            if (!obj) return;

            if (savedIds.has(currentObjectId)) {
                savedIds.delete(currentObjectId);
                obj.saves = Math.max(0, (obj.saves || 1) - 1);
                document.getElementById('modalSaveBtn').innerText = '⭐ В избранное';
                showToast(`Удалено из коллекции: "${obj.name}"`);
            } else {
                savedIds.add(currentObjectId);
                obj.saves = (obj.saves || 0) + 1;
                document.getElementById('modalSaveBtn').innerText = '🌟 В избранном';
                showToast(`⭐ "${obj.name}" сохранено в коллекцию!`);
            }
            updateStats();
            renderList();
        };

        document.getElementById('modalReportBtn').onclick = () => {
            const reason = prompt('Опишите проблему или неточность в описании объекта:');
            if (reason && reason.trim()) {
                showToast('🚩 Жалоба отправлена модераторам');
            }
        };

        // Избранное (Коллекция)
        document.getElementById('openCollectionBtn').onclick = () => {
            const listEl = document.getElementById('collectionList');
            listEl.innerHTML = '';

            const savedObjects = culturalObjects.filter(o => savedIds.has(o.id));

            if (savedObjects.length === 0) {
                listEl.innerHTML = '<li style="color:#888;">У вас пока нет сохраненных объектов.</li>';
            } else {
                savedObjects.forEach(obj => {
                    const li = document.createElement('li');
                    li.innerHTML = `
                        <span class="emoji-big">${obj.emoji}</span>
                        <div class="info">
                            <div class="name">${obj.name}</div>
                            <div class="region">${obj.region}</div>
                        </div>
                        <button class="btn btn-outline" onclick="openObjectModal(${obj.id}); closeModal('collectionModal');" style="padding:4px 10px; font-size:12px;">Открыть</button>
                    `;
                    listEl.appendChild(li);
                });
            }
            document.getElementById('collectionModal').classList.add('active');
        };

        // ============================================================
        // 6. ФОРМА ДОБАВЛЕНИЯ И ИИ-РАСПОЗНАВАНИЕ
        // ============================================================
        document.getElementById('openAddModalBtn').onclick = () => {
            document.getElementById('addModal').classList.add('active');
        };

        const fileInput = document.getElementById('fileInput');
        const uploadArea = document.getElementById('uploadArea');

        uploadArea.onclick = () => fileInput.click();

        uploadArea.ondragover = (e) => { e.preventDefault(); uploadArea.style.borderColor = '#6b4f3a'; };
        uploadArea.ondragleave = () => { uploadArea.style.borderColor = '#e8e0d8'; };
        uploadArea.ondrop = (e) => {
            e.preventDefault();
            uploadArea.style.borderColor = '#e8e0d8';
            if (e.dataTransfer.files.length) {
                fileInput.files = e.dataTransfer.files;
                handleFileUpload(e.dataTransfer.files[0]);
            }
        };

        fileInput.onchange = (e) => {
            if (e.target.files.length) handleFileUpload(e.target.files[0]);
        };

        function handleFileUpload(file) {
            const reader = new FileReader();
            reader.onload = (e) => {
                document.getElementById('previewContainer').innerHTML = `<img src="${e.target.result}" style="max-height:120px; border-radius:8px;">`;
                simulateAIAnalysis(file.name);
            };
            reader.readAsDataURL(file);
        }

        function simulateAIAnalysis(fileName) {
            const status = document.getElementById('aiStatus');
            status.innerHTML = '🤖 <i>ИИ Yandex Vision анализирует изображение...</i>';

            setTimeout(() => {
                status.innerHTML = '✅ ИИ распознал объект и автоматически заполнил поля!';
                
                // Автозаполнение
                document.getElementById('addName').value = "Деревянная резная прялка";
                document.getElementById('addCategory').value = "Ремесло";
                document.getElementById('addRegion').value = "Архангельская область";
                document.getElementById('addLat').value = 64.5472;
                document.getElementById('addLng').value = 40.5602;
                document.getElementById('addDescription').value = "Традиционная северная резная прялка с солярными орнаментами и растительными узорами XIX века.";
            }, 1200);
        }

        document.getElementById('addObjectForm').onsubmit = (e) => {
            e.preventDefault();

            const name = document.getElementById('addName').value;
            const category = document.getElementById('addCategory').value;
            const region = document.getElementById('addRegion').value;
            const lat = parseFloat(document.getElementById('addLat').value);
            const lng = parseFloat(document.getElementById('addLng').value);
            const description = document.getElementById('addDescription').value;

            const previewImg = document.querySelector('#previewContainer img');
            const imageUrl = previewImg ? previewImg.src : null;

            const newObj = {
                id: Date.now(),
                name,
                category,
                region,
                lat,
                lng,
                emoji: "✨",
                imageUrl,
                description,
                saves: 0,
                shape: "cube"
            };

            culturalObjects.unshift(newObj);
            populateFilters();
            renderList();
            renderMarkers();

            if (yMap) yMap.setCenter([lat, lng], 8);

            closeModal('addModal');
            document.getElementById('addObjectForm').reset();
            document.getElementById('previewContainer').innerHTML = '';
            document.getElementById('aiStatus').innerHTML = '🤖 ИИ готов к анализу загруженного фото...';

            showToast('🎉 Новый объект успешно опубликован на карте!');
        };

        // ============================================================
        // 7. ПОИСК И ФИЛЬТРЫ
        // ============================================================
        document.getElementById('searchInput').oninput = () => { renderList(); renderMarkers(); };
        document.getElementById('categoryFilter').onchange = () => { renderList(); renderMarkers(); };
        document.getElementById('regionFilter').onchange = () => { renderList(); renderMarkers(); };

        // ============================================================
        // 8. УВЕДОМЛЕНИЯ TOAST
        // ============================================================
        function showToast(message, type = 'info') {
            const container = document.getElementById('toastContainer');
            const toast = document.createElement('div');
            toast.className = 'toast';
            if (type === 'error') toast.style.background = '#c0392b';
            toast.innerText = message;
            container.appendChild(toast);

            setTimeout(() => {
                toast.style.opacity = '0';
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        // ============================================================
        // 9. СТАРТ ПРИЛОЖЕНИЯ
        // ============================================================
        window.onload = () => {
            populateFilters();
            renderList();
            initYandexMap();
        };
    </script>
</body>
</html>
