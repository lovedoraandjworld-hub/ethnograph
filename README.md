<!doctype html>
<html lang="ru">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>ЭТНОКОД — цифровой музей культуры России</title>
<script src="https://api-maps.yandex.ru/2.1/?apikey=9d08746c-663c-48f8-a58c-ada00fcc5d04&lang=ru_RU" type="text/javascript"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<style>
:root{--bg:#0d1414;--bg2:#101b1c;--panel:#151c1b;--panel2:#1b211e;--gold:#b79057;--gold2:#d6ba86;--cream:#e7dcc5;--muted:#a69d8b;--line:rgba(214,186,134,.22);--green:#2a4b43;--shadow:0 22px 65px rgba(0,0,0,.35)}
*{box-sizing:border-box}html{scroll-behavior:smooth}body{margin:0;background:radial-gradient(circle at 50% 0,#182827 0,#0d1414 45%,#080d0d 100%);color:var(--cream);font-family:Inter,"Segoe UI",Arial,sans-serif}.serif{font-family:Georgia,"Times New Roman",serif}
.header{position:sticky;top:0;z-index:1000;height:78px;padding:0 34px;display:flex;align-items:center;gap:34px;border-bottom:1px solid var(--line);background:rgba(8,13,13,.92);backdrop-filter:blur(18px)}
.brand{display:flex;align-items:center;gap:12px;min-width:210px}.brand-mark{width:40px;height:40px;border:1px solid var(--gold);border-radius:50%;display:grid;place-items:center;color:var(--gold2);font-size:20px}.brand-title{font-family:Georgia,serif;letter-spacing:.08em;font-size:18px}.brand-sub{font-size:11px;color:var(--muted);line-height:1.2}.nav{display:flex;gap:26px;align-items:center}.nav a{color:#c8bfad;text-decoration:none;font-size:14px}.nav a.active,.nav a:hover{color:var(--gold2)}.spacer{flex:1}.search{width:min(360px,29vw);height:42px;border:1px solid var(--line);border-radius:10px;background:#101615;padding:0 14px;color:var(--cream);outline:none}.header .link-btn{border:0;background:none;color:var(--gold2);cursor:pointer;font-size:14px}.header .primary{border:1px solid var(--gold);background:transparent;color:var(--gold2);padding:11px 16px;border-radius:8px;cursor:pointer}
.hero{min-height:640px;padding:64px 40px 42px;position:relative;overflow:hidden;border-bottom:1px solid var(--line)}.hero:before{content:"";position:absolute;inset:0;background:radial-gradient(ellipse at 65% 60%,rgba(183,144,87,.12),transparent 48%),linear-gradient(180deg,rgba(21,40,39,.28),rgba(7,12,12,.72));pointer-events:none}.hero-grid{position:relative;z-index:1;max-width:1480px;margin:auto;display:grid;grid-template-columns:420px 1fr;gap:36px;align-items:center}.hero-copy h1{font-size:50px;line-height:1.06;margin:0 0 18px;font-weight:500;color:#efe5d1}.hero-copy p{font-size:17px;line-height:1.55;color:#c8bfad;max-width:390px}.actions{display:flex;gap:12px;flex-wrap:wrap;margin-top:26px}.btn{border-radius:9px;padding:12px 18px;border:1px solid var(--line);background:#151d1b;color:#e8dcc4;cursor:pointer;font-weight:600}.btn.gold{background:linear-gradient(180deg,#856236,#6d4e2d);border-color:#a7804d}.btn.ghost{background:transparent}.stats-strip{display:grid;grid-template-columns:repeat(4,1fr);margin-top:42px;border:1px solid var(--line);border-radius:12px;overflow:hidden}.stat{padding:17px 10px;text-align:center;border-right:1px solid var(--line)}.stat:last-child{border-right:0}.stat b{font:24px Georgia,serif;color:#d6ba86}.stat span{display:block;margin-top:4px;font-size:11px;color:#918977}.map-art{min-height:520px;border-radius:28px;position:relative;background:linear-gradient(145deg,#263a32,#152927 38%,#10201f);box-shadow:inset 0 0 0 1px rgba(255,255,255,.03),var(--shadow);overflow:hidden}.map-art:after{content:"";position:absolute;inset:30px;background:radial-gradient(circle at 50% 55%,rgba(128,116,71,.30),transparent 52%);filter:blur(2px)}.russia-shape{position:absolute;inset:68px 50px 54px 35px;background:linear-gradient(135deg,#5b6638,#827541 55%,#4b5732);clip-path:polygon(2% 43%,11% 32%,17% 23%,26% 25%,33% 12%,42% 20%,49% 18%,56% 27%,65% 22%,74% 28%,84% 20%,94% 27%,100% 40%,96% 54%,90% 57%,87% 71%,77% 73%,71% 85%,59% 82%,52% 93%,39% 88%,31% 77%,19% 81%,12% 66%,5% 61%);box-shadow:0 25px 50px rgba(0,0,0,.4)}.river{position:absolute;width:65%;height:4px;background:rgba(174,208,194,.42);left:16%;top:54%;transform:rotate(7deg);filter:blur(.5px)}.artifact{position:absolute;z-index:3;width:70px;height:70px;border-radius:18px;background:radial-gradient(circle at 35% 25%,#d0a967,#71522f 62%,#352719);display:grid;place-items:center;font-size:34px;box-shadow:0 13px 25px rgba(0,0,0,.48),inset 0 0 18px rgba(255,255,255,.08)}.artifact small{position:absolute;top:74px;white-space:nowrap;color:#eee0c8;font-size:11px;text-shadow:0 2px 5px #000}.a1{left:17%;top:45%}.a2{left:33%;top:59%}.a3{left:50%;top:38%}.a4{left:65%;top:54%}.a5{left:78%;top:36%}.a6{left:84%;top:64%}.hero-tip{position:absolute;z-index:4;bottom:18px;left:50%;transform:translateX(-50%);padding:10px 22px;border:1px solid var(--line);border-radius:30px;background:rgba(10,17,16,.76);color:#bda77e;font-size:13px}
.section{max-width:1480px;margin:0 auto;padding:68px 40px}.section-head{display:flex;justify-content:space-between;gap:20px;align-items:flex-end;margin-bottom:26px}.section h2{font:38px Georgia,serif;margin:0;color:#ecdfc5}.section p.lead{margin:8px 0 0;color:#a9a18f;max-width:720px;line-height:1.6}.map-layout{display:grid;grid-template-columns:1fr 360px;gap:20px}.map-container{height:620px;border:1px solid var(--line);border-radius:18px;overflow:hidden;background:#161d1b}.sidebar{display:flex;flex-direction:column;gap:16px}.panel{background:linear-gradient(180deg,rgba(29,35,32,.96),rgba(18,24,23,.96));border:1px solid var(--line);border-radius:16px;padding:18px;box-shadow:0 14px 30px rgba(0,0,0,.18)}.panel h3{margin:0 0 13px;color:#d2b783;font-size:16px;letter-spacing:.03em}.input,.select{width:100%;height:42px;border:1px solid rgba(214,186,134,.16);border-radius:9px;background:#0f1615;color:#e8dcc7;padding:0 12px;outline:none}.filter-row{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:8px}.object-list{list-style:none;margin:0;padding:0;max-height:335px;overflow:auto}.object-list li{display:flex;gap:12px;align-items:center;padding:12px 5px;border-bottom:1px solid rgba(214,186,134,.09);cursor:pointer}.object-list li:hover{background:rgba(183,144,87,.05)}.emoji-big{font-size:26px}.obj-info{flex:1}.obj-name{font-weight:600}.obj-meta{font-size:12px;color:#948c7b;margin-top:4px}
.about-grid{display:grid;grid-template-columns:1.2fr .8fr;gap:20px}.mission-card{padding:30px;border-radius:18px;background:linear-gradient(135deg,#2c251c,#171b18);border:1px solid var(--line)}.mission-card h3{font:30px Georgia,serif;margin:0 0 12px}.steps{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-top:18px}.step{padding:16px;border:1px solid rgba(214,186,134,.14);border-radius:12px;background:rgba(255,255,255,.02)}.step b{color:#d2b783}.step span{display:block;color:#9e9684;font-size:13px;margin-top:7px;line-height:1.45}.rank-list{display:grid;gap:8px}.rank-row{display:grid;grid-template-columns:34px 1fr auto;align-items:center;padding:11px;border-radius:9px;background:#101615;border:1px solid rgba(214,186,134,.10)}.rank-pos{font:20px Georgia,serif;color:#ba9b69}.rank-user b{display:block}.rank-user small{color:#8f8778}.rank-score{color:#d6ba86;font-weight:700}
.partners{display:grid;grid-template-columns:repeat(5,1fr);gap:12px}.partner{height:88px;border:1px dashed rgba(214,186,134,.25);border-radius:12px;display:grid;place-items:center;color:#9f947f;background:rgba(255,255,255,.015);text-align:center;font-weight:600}.footer{border-top:1px solid var(--line);padding:28px 40px;color:#8e8778;text-align:center}
.modal-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.74);backdrop-filter:blur(7px);z-index:2000;padding:24px;overflow:auto}.modal-overlay.active{display:flex;align-items:flex-start;justify-content:center}.modal{margin:auto;width:min(1180px,100%);background:linear-gradient(180deg,#1c211e,#111614);border:1px solid rgba(214,186,134,.28);border-radius:18px;box-shadow:0 35px 90px rgba(0,0,0,.5);position:relative;padding:30px}.modal-close{position:absolute;right:20px;top:15px;border:0;background:transparent;color:#c8b38c;font-size:32px;cursor:pointer}.object-modal-grid{display:grid;grid-template-columns:1.25fr .85fr;gap:28px}.breadcrumbs{font-size:12px;color:#a58c64;margin-bottom:14px}.modal h2{font:36px Georgia,serif;margin:0}.modal-region{color:#b5a991;margin:5px 0 15px}.tags{display:flex;gap:8px;flex-wrap:wrap;margin:12px 0 18px}.tag{font-size:11px;border:1px solid rgba(214,186,134,.22);padding:6px 9px;border-radius:6px;color:#bba982}.modal-3d{height:330px;border-radius:13px;background:#0b0f0e;overflow:hidden;border:1px solid rgba(214,186,134,.12);position:relative}.modal-3d-label{position:absolute;left:50%;bottom:10px;transform:translateX(-50%);font-size:11px;color:#aa9b80;background:rgba(0,0,0,.55);padding:6px 12px;border-radius:20px;z-index:2}.details h3{margin:16px 0 8px;color:#d6ba86;letter-spacing:.05em;font-size:15px}.details p{color:#b8af9d;line-height:1.65;margin:0}.specs{display:grid;gap:8px;margin-top:8px}.spec{display:grid;grid-template-columns:150px 1fr;gap:12px;font-size:13px;border-bottom:1px solid rgba(214,186,134,.08);padding-bottom:8px}.spec span:first-child{color:#8f8777}.origin{margin-top:16px;padding:14px;border:1px solid rgba(214,186,134,.14);border-radius:10px;background:#101514}.voice{margin-top:14px;border:1px solid rgba(214,186,134,.14);border-radius:10px;padding:14px;display:flex;align-items:center;gap:12px}.voice-play{width:42px;height:42px;border-radius:50%;display:grid;place-items:center;background:#b38c53;color:#101413;cursor:pointer;font-weight:900}.wave{flex:1;height:24px;background:repeating-linear-gradient(90deg,rgba(183,144,87,.5) 0 2px,transparent 2px 5px);mask:linear-gradient(180deg,transparent 0,#000 28%,#000 72%,transparent 100%)}.similar{margin-top:20px}.similar-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px}.similar-card{padding:12px;border:1px solid rgba(214,186,134,.12);border-radius:10px;background:#111715;display:flex;align-items:center;gap:10px}.similar-icon{width:50px;height:50px;border-radius:8px;background:#34271b;display:grid;place-items:center;font-size:26px}.modal-actions{display:flex;gap:10px;margin-top:18px}.add-form{display:grid;gap:10px}.add-form input,.add-form textarea,.add-form select{width:100%;background:#0e1413;border:1px solid rgba(214,186,134,.16);color:#ece0ca;border-radius:8px;padding:12px}.add-form textarea{min-height:90px}.form-row{display:grid;grid-template-columns:1fr 1fr;gap:10px}.upload-area{border:1px dashed rgba(214,186,134,.35);border-radius:10px;padding:22px;text-align:center;color:#a89d88;cursor:pointer}.upload-area input{display:none}.ai-status{padding:10px;border-radius:8px;background:#101a18;color:#9fc4b4;font-size:13px}
@media(max-width:1050px){.nav,.search{display:none}.hero-grid,.map-layout,.about-grid,.object-modal-grid{grid-template-columns:1fr}.hero-copy{max-width:720px}.map-art{min-height:430px}.partners{grid-template-columns:repeat(2,1fr)}}
@media(max-width:650px){.header{padding:0 16px}.hero,.section{padding-left:18px;padding-right:18px}.hero-copy h1{font-size:38px}.stats-strip{grid-template-columns:repeat(2,1fr)}.stat:nth-child(2){border-right:0}.stat:nth-child(-n+2){border-bottom:1px solid var(--line)}.steps{grid-template-columns:1fr}.filter-row,.form-row,.similar-grid{grid-template-columns:1fr}.modal{padding:22px}.partners{grid-template-columns:1fr}}

/* ===== PIXEL / ETHNO GAME UI OVERRIDES ===== */
@import url('https://fonts.googleapis.com/css2?family=Pixelify+Sans:wght@400;500;600;700&family=Press+Start+2P&display=swap');
:root{--pixel:#d8c18e;--pixel-dark:#0b100d;--pixel-green:#273522;--pixel-line:#736344}
body{font-family:'Pixelify Sans','Courier New',monospace;background-color:#0b0f0d;background-image:linear-gradient(rgba(255,255,255,.018) 1px,transparent 1px),linear-gradient(90deg,rgba(255,255,255,.018) 1px,transparent 1px),radial-gradient(circle at 50% 20%,#253428 0,#131b16 38%,#080b09 82%);background-size:6px 6px,6px 6px,auto;image-rendering:pixelated}
body:before{content:"";position:fixed;inset:0;pointer-events:none;z-index:9999;background:repeating-linear-gradient(0deg,rgba(255,255,255,.018) 0 1px,transparent 1px 3px);mix-blend-mode:soft-light;opacity:.35}
.serif,.brand-title,.hero-copy h1,.section h2,.mission-card h3,.modal h2,.stat b,.rank-pos{font-family:'Pixelify Sans','Courier New',monospace!important;letter-spacing:.03em}
.header{height:76px;border-bottom:2px solid #4d432f;background:rgba(9,13,11,.96);backdrop-filter:none;box-shadow:0 6px 0 rgba(0,0,0,.25)}
.brand-mark{border-radius:2px;border:2px solid #8e754b;box-shadow:3px 3px 0 #332b1e;background:#151b15}
.nav a,.header .link-btn,.header .primary,.search,.btn,.input,.select,.panel,.mission-card,.step,.rank-row,.partner,.modal,.tag,.origin,.voice,.similar-card,.map-container,.stats-strip{border-radius:2px!important}
.header .primary,.btn.gold{background:#6e542d;border:2px solid #a98a52;box-shadow:3px 3px 0 #2b2115;color:#f1dfb6;text-transform:uppercase}
.header .primary:hover,.btn.gold:hover{transform:translate(1px,1px);box-shadow:2px 2px 0 #2b2115}
.search,.input,.select,.add-form input,.add-form textarea,.add-form select{font-family:'Pixelify Sans','Courier New',monospace!important;background:#0e1511!important;border:2px solid #4e4633!important;color:#e8d6ac!important;border-radius:2px!important}
.hero{min-height:650px;background:linear-gradient(rgba(12,17,13,.2),rgba(8,12,9,.35));border-bottom:2px solid #4d432f}
.hero-copy h1{font-size:54px;text-transform:uppercase;text-shadow:4px 4px 0 #19150e,6px 6px 0 rgba(0,0,0,.45)}
.hero-copy p,.section p.lead,.details p{font-size:16px}
.map-art{border-radius:2px;border:3px solid #584d35;box-shadow:8px 8px 0 #090b09;min-height:520px;background-color:#16231d;background-image:linear-gradient(45deg,rgba(191,159,89,.07) 25%,transparent 25%),linear-gradient(-45deg,rgba(191,159,89,.07) 25%,transparent 25%),linear-gradient(45deg,transparent 75%,rgba(191,159,89,.05) 75%),linear-gradient(-45deg,transparent 75%,rgba(191,159,89,.05) 75%);background-size:16px 16px;background-position:0 0,0 8px,8px -8px,-8px 0}
.russia-shape{filter:saturate(.78) contrast(1.15);box-shadow:10px 10px 0 rgba(0,0,0,.34);background:repeating-linear-gradient(0deg,#65673c 0 6px,#757047 6px 12px,#505c35 12px 18px)}
.artifact{border-radius:2px;border:2px solid #a98a52;background:#6c4e2c;box-shadow:5px 5px 0 #1e1911;font-size:30px}.artifact small{font-family:'Pixelify Sans',monospace;text-shadow:2px 2px 0 #000}
.hero-tip{border-radius:2px;border:2px solid #66583b;background:#101610;box-shadow:3px 3px 0 #050705}
.panel,.mission-card,.modal{border:2px solid #554a33;box-shadow:6px 6px 0 rgba(0,0,0,.34);background:linear-gradient(180deg,#1c231b,#10150f)}
.panel h3,.details h3{font-family:'Press Start 2P','Pixelify Sans',monospace;font-size:11px;line-height:1.5}
.object-list li{border-bottom:1px dashed #504832}.object-list li:hover{background:#25271a}
.stats-strip{border:2px solid #5b4e35}.stat{border-right:2px solid #4d432f}.stat b{font-size:27px}
.step,.rank-row,.partner,.similar-card,.origin,.voice{border:2px solid #4f4633;background:#111710;box-shadow:3px 3px 0 #080a08}
.modal-overlay{backdrop-filter:none;background:rgba(3,5,4,.88)}
.modal-close{font-family:'Press Start 2P',monospace;font-size:18px}
.modal-3d{border-radius:2px;border:3px solid #493f2d;box-shadow:5px 5px 0 #070907}
.tag{border:2px solid #5f5236;background:#171b12}.voice-play{border-radius:2px;background:#806038;box-shadow:3px 3px 0 #20180f}
.upload-area{border:2px dashed #8c7549!important;border-radius:2px!important;background:#0d130e!important;box-shadow:inset 0 0 0 3px rgba(255,255,255,.015);font-family:'Press Start 2P','Pixelify Sans',monospace!important;font-size:10px!important;line-height:1.8!important}
.upload-area:hover{background:#171d13!important}
.ai-status{border:2px solid #5c5138!important;border-radius:2px!important;background:#141a11!important;color:#d9c38e!important;box-shadow:3px 3px 0 #090b08!important}
#previewContainer img{image-rendering:auto;border-radius:2px!important;border:2px solid #6e5a39;box-shadow:4px 4px 0 #090b08}
.pixel-badge{display:inline-flex;align-items:center;gap:8px;padding:8px 10px;border:2px solid #5e5237;background:#10150f;color:#c8ae73;font-family:'Press Start 2P','Pixelify Sans',monospace;font-size:9px;box-shadow:3px 3px 0 #080a08;margin:10px 0}
.pixel-badge:before{content:'◆';color:#c99f54}.gemini-ok:before{content:'✦';color:#8bd0a0}
@media(max-width:900px){.hero-copy h1{font-size:40px}.nav{display:none}.header{gap:12px}.search{display:none}}

</style>
</head>
<body>
<header class="header">
  <div class="brand"><div class="brand-mark">✣</div><div><div class="brand-title">ЭТНОКОД</div><div class="brand-sub">цифровой музей<br>культуры России</div></div></div>
  <nav class="nav"><a class="active" href="#mapSection">Карта</a><a href="#collection">Коллекции</a><a href="#about">О проекте</a><a href="#ranking">Рейтинг</a><a href="#partners">Партнёры</a></nav>
  <div class="spacer"></div><input class="search" id="headerSearch" placeholder="Поиск по предметам, местам, людям…"><button class="link-btn" id="loginBtn">Войти</button><button class="primary" id="openAddModalBtn">Поделиться предметом</button>
</header>
<section class="hero" id="about"><div class="hero-grid">
  <div class="hero-copy"><h1 class="serif">Открой культуру<br>своей страны</h1><p>ЭТНОКОД собирает семейные истории, предметы быта и локальные традиции в единую живую карту культурного наследия России.</p><div class="actions"><button class="btn gold" id="joinBtn">Как участвовать?</button><button class="btn ghost" onclick="document.getElementById('mapSection').scrollIntoView()">Открыть карту</button></div>
    <div class="stats-strip"><div class="stat"><b id="heroItems">0</b><span>предметов</span></div><div class="stat"><b>1 342</b><span>населённых пункта</span></div><div class="stat"><b>68</b><span>регионов</span></div><div class="stat"><b>3 528</b><span>историй</span></div></div>
  </div>
  <div class="map-art"><div class="russia-shape"></div><div class="river"></div><div class="artifact a1">🏛<small>Санкт‑Петербург</small></div><div class="artifact a2">🏺<small>Приволжский ФО</small></div><div class="artifact a3">🧵<small>Уральский ФО</small></div><div class="artifact a4">🪕<small>Сибирский ФО</small></div><div class="artifact a5">🐴<small>Якутия</small></div><div class="artifact a6">🛖<small>Дальний Восток</small></div><div class="hero-tip">Нажмите на регион или предмет, чтобы узнать больше</div></div>
</div></section>
<section class="section" id="mapSection"><div class="section-head"><div><h2>Интерактивная карта</h2><p class="lead">Исследуйте предметы по регионам, находите личные истории и сохраняйте интересные объекты в свою коллекцию.</p></div><button class="btn" id="openCollectionBtn">Избранное: <span id="savedCount">0</span></button></div>
<div class="map-layout"><div class="map-container"><div id="map" style="width:100%;height:100%"></div></div><aside class="sidebar"><div class="panel"><h3>Поиск и фильтры</h3><input class="input" id="searchInput" placeholder="Название, описание, место"><div class="filter-row"><select class="select" id="categoryFilter"><option value="">Все категории</option></select><select class="select" id="regionFilter"><option value="">Все регионы</option></select></div></div><div class="panel"><h3>Объекты на карте</h3><ul class="object-list" id="objectList"></ul></div></aside></div></section>
<section class="section" id="collection"><div class="about-grid"><div class="mission-card"><h3>Сохраняем не вещи — сохраняем память</h3><p class="lead">Главная ценность проекта — не музейная витрина, а связь предмета с конкретной семьёй, местом и человеком.</p><div class="steps"><div class="step"><b>01. Добавьте</b><span>Фотографию предмета, место и краткую историю.</span></div><div class="step"><b>02. Расскажите</b><span>Запишите голос потомка семьи или владельца.</span></div><div class="step"><b>03. Сохраните</b><span>После модерации объект появится на общей карте.</span></div></div><div class="actions"><button class="btn gold" onclick="openAddModal()">Поделиться предметом</button></div></div>
<div class="panel" id="ranking"><h3>Рейтинг участников</h3><div class="rank-list"><div class="rank-row"><div class="rank-pos">1</div><div class="rank-user"><b>Анна Петрова</b><small>Нижегородская область</small></div><div class="rank-score">1280</div></div><div class="rank-row"><div class="rank-pos">2</div><div class="rank-user"><b>Михаил Орлов</b><small>Республика Карелия</small></div><div class="rank-score">1055</div></div><div class="rank-row"><div class="rank-pos">3</div><div class="rank-user"><b>Елена Смирнова</b><small>Архангельская область</small></div><div class="rank-score">970</div></div><div class="rank-row"><div class="rank-pos">4</div><div class="rank-user"><b>Илья Сафонов</b><small>Татарстан</small></div><div class="rank-score">820</div></div></div></div></div></section>
<section class="section" id="partners"><div class="section-head"><div><h2>Партнёры проекта</h2><p class="lead">Здесь можно разместить реальные организации, музеи, архивы и образовательные партнёрства проекта.</p></div></div><div class="partners"><div class="partner">Музей‑партнёр</div><div class="partner">Региональный архив</div><div class="partner">Университет</div><div class="partner">Культурный фонд</div><div class="partner">Медиа‑партнёр</div></div></section>
<footer class="footer">ЭТНОКОД · цифровой музей культуры России · 2026</footer>

<div class="modal-overlay" id="objectModal"><div class="modal"><button class="modal-close" onclick="closeModal('objectModal')">×</button><div class="breadcrumbs" id="modalBreadcrumbs"></div><div class="object-modal-grid"><div><h2 id="modalTitle">Предмет</h2><div class="modal-region" id="modalRegion"></div><div class="tags" id="modalTags"></div><div class="modal-3d" id="modal3dContainer"><div class="modal-3d-label">3D-модель · перетащите мышью, чтобы вращать и колёсиком приблизить</div></div><div class="similar"><h3 style="color:#d6ba86">Похожие предметы</h3><div class="similar-grid" id="similarGrid"></div></div></div><div class="details"><h3>ИСТОРИЯ ПРЕДМЕТА</h3><p id="modalDescription"></p><h3>ХАРАКТЕРИСТИКИ</h3><div class="specs" id="modalSpecs"></div><div class="origin"><h3 style="margin-top:0">ПРОИСХОЖДЕНИЕ</h3><div id="modalOrigin"></div></div><h3>ГОЛОС ПРЕДМЕТА</h3><div class="voice"><div class="voice-play" id="modalVoiceBtn">▶</div><div style="min-width:0"><b id="voiceTitle">История от потомка семьи</b><div class="wave"></div></div><span id="voiceTime" style="color:#8f8778;font-size:12px">0:00 / 1:48</span></div><div class="modal-actions"><button class="btn gold" id="modalSaveBtn">В избранное</button><button class="btn ghost" id="modalShareBtn">Поделиться</button></div></div></div></div></div>
<div class="modal-overlay" id="addModal"><div class="modal" style="max-width:760px"><button class="modal-close" onclick="closeModal('addModal')">×</button><h2>Поделиться семейным предметом</h2><div class="pixel-badge gemini-ok">PROVOD.AI · МУЛЬТИМОДАЛЬНЫЙ АНАЛИЗ</div><p class="lead">Загрузите фотографию: ИИ через provod.ai распознает предмет, предложит музейное описание и автоматически заполнит карточку. Перед отправкой все поля можно исправить вручную.</p><form class="add-form" id="addObjectForm"><div class="upload-area" id="uploadArea">[ ЗАГРУЗИТЬ ФОТО ПРЕДМЕТА ]<input type="file" id="fileInput" accept="image/*"><div id="previewContainer"></div></div><div class="ai-status" id="aiStatus">ЛОКАЛЬНЫЙ ИИ: ОЖИДАНИЕ ИЗОБРАЖЕНИЯ...</div><input id="addName" placeholder="Название предмета *" required><div class="form-row"><select id="addCategory" required><option value="">Категория *</option><option>Архитектура</option><option>Ремесло</option><option>Одежда</option><option>Быт</option><option>Музыка</option><option>Искусство</option><option>Инструменты</option></select><input id="addRegion" placeholder="Регион *" required></div><div class="form-row"><input id="addPeriod" placeholder="Период / датировка"><input id="addMaterial" placeholder="Материал"></div><div class="form-row"><input id="addTechnique" placeholder="Техника изготовления"><input id="addPurpose" placeholder="Назначение"></div><div class="form-row"><input type="number" step="any" id="addLat" placeholder="Широта" required><input type="number" step="any" id="addLng" placeholder="Долгота" required></div><textarea id="addDescription" placeholder="История предмета и его связь с вашей семьёй *" required></textarea><input id="addOwner" placeholder="Кто добавил / происхождение"><input id="addVoice" placeholder="Ссылка на запись голоса потомка семьи (необязательно)"><button class="btn gold" type="submit">Отправить на модерацию</button></form></div></div>
<div class="modal-overlay" id="collectionModal"><div class="modal" style="max-width:720px"><button class="modal-close" onclick="closeModal('collectionModal')">×</button><h2>Моя коллекция</h2><ul class="object-list" id="collectionList" style="max-height:520px"></ul></div></div>
<div class="modal-overlay" id="loginModal"><div class="modal" style="max-width:520px"><button class="modal-close" onclick="closeModal('loginModal')">×</button><h2>Личный кабинет</h2><p class="lead">В демонстрационной версии вход имитируется. Здесь можно подключить реальную авторизацию.</p><div class="add-form"><input placeholder="E‑mail"><input type="password" placeholder="Пароль"><button class="btn gold" onclick="closeModal('loginModal')">Войти</button></div></div></div>
<script>
const culturalObjects=[
{id:1,name:'Кижский погост',category:'Архитектура',region:'Республика Карелия',lat:62.0678,lng:35.2231,emoji:'⛪',description:'Ансамбль деревянного зодчества XVIII–XIX веков. Для проекта важна не только архитектура, но и семейные воспоминания жителей о праздниках, ремёслах и жизни вокруг Кижей.',saves:14,shape:'building',period:'XVIII–XIX века',material:'дерево',technique:'рубка, деревянное зодчество',purpose:'храмовый ансамбль',owner:'Передано в цифровой архив местными исследователями'},
{id:2,name:'Хохломская чаша',category:'Ремесло',region:'Нижегородская область',lat:56.8584,lng:44.5222,emoji:'🥣',description:'Предмет домашнего быта с характерной золотистой росписью. В семье Смирновых похожая чаша хранилась как память о бабушке и использовалась только по большим праздникам.',saves:28,shape:'bowl',period:'конец XIX — начало XX века',material:'липа, лак, краска',technique:'хохломская роспись',purpose:'сервировка и хранение',owner:'Добавил Александр Смирнов, потомок семьи'},
{id:3,name:'Татарский ичиг',category:'Одежда',region:'Республика Татарстан',lat:55.7887,lng:49.1221,emoji:'👢',description:'Мягкие кожаные сапоги с цветной мозаикой. Семейная история связывает эту пару с мастерской прадеда, который шил обувь для жителей Казани.',saves:9,shape:'boot',period:'начало XX века',material:'кожа',technique:'кожаная мозаика, ручной шов',purpose:'праздничная обувь',owner:'Добавила семья Хабибуллиных'},
{id:4,name:'Русская печь',category:'Быт',region:'Вологодская область',lat:59.2205,lng:39.8915,emoji:'🔥',description:'Центр традиционного дома: печь обогревала, кормила и собирала семью. Голосовая история владельца рассказывает о рецептах и зимних вечерах в деревенском доме.',saves:42,shape:'cube',period:'конец XIX века',material:'кирпич, глина',technique:'кладка, обмазка',purpose:'обогрев и приготовление пищи',owner:'Добавлено Марией Лебедевой'},
{id:5,name:'Резной сундук',category:'Быт',region:'Нижегородская область',lat:56.3269,lng:44.0059,emoji:'🧰',description:'Сундук изготовлен в Нижнем Новгороде в конце XIX века в мастерской купца Ивана Михайловича. Использовался в купеческом быту для хранения одежды и ценностей. Геометрическая и растительная резьба типична для нижегородского Поволжья.',saves:54,shape:'chest',period:'1880–1890-е гг.',material:'липа, сосна',technique:'резьба, сборка на деревянных шипах',purpose:'хранение одежды и ценных вещей',owner:'Передан в цифровой архив семьёй Смирновых; добавил Александр Смирнов'}];
let savedIds=new Set(),yMap=null,mapMarkers=[],currentObjectId=null,threeScene,threeCamera,threeRenderer,threeMesh,animationFrameId;
function filtered(){const q=(document.getElementById('searchInput').value||'').toLowerCase(),c=document.getElementById('categoryFilter').value,r=document.getElementById('regionFilter').value;return culturalObjects.filter(o=>(o.name+o.description+o.region).toLowerCase().includes(q)&&(!c||o.category===c)&&(!r||o.region===r))}
function populateFilters(){const c=[...new Set(culturalObjects.map(o=>o.category))],r=[...new Set(culturalObjects.map(o=>o.region))];categoryFilter.innerHTML='<option value="">Все категории</option>'+c.map(x=>`<option>${x}</option>`).join('');regionFilter.innerHTML='<option value="">Все регионы</option>'+r.map(x=>`<option>${x}</option>`).join('')}
function renderList(){objectList.innerHTML=filtered().map(o=>`<li onclick="focusObject(${o.id})"><span class="emoji-big">${o.emoji}</span><div class="obj-info"><div class="obj-name">${o.name}</div><div class="obj-meta">${o.region} · ${o.category}</div></div><span style="color:#9c8d75;font-size:12px">★ ${o.saves}</span></li>`).join('')||'<li>Ничего не найдено</li>';heroItems.textContent=culturalObjects.length.toLocaleString('ru-RU')}
function initMap(){
  if(typeof ymaps==='undefined'){
    console.error('Яндекс Карты не загрузились. Проверь API-ключ и разрешённый домен в кабинете разработчика Яндекс.');
    const el=document.getElementById('map');
    if(el) el.innerHTML='<div style="height:100%;display:grid;place-items:center;padding:30px;text-align:center;color:#d6ba86;background:#101716">Яндекс Карты не загрузились.<br><small style="color:#948c7b;margin-top:8px">Проверьте API-ключ и разрешите домен GitHub Pages в кабинете Яндекс API.</small></div>';
    return;
  }
  ymaps.ready(()=>{
    yMap=new ymaps.Map('map',{
      center:[57.8,43],
      zoom:5,
      controls:['zoomControl','fullscreenControl','geolocationControl']
    },{
      suppressMapOpenBlock:true
    });
    renderMarkers();
  });
}
function renderMarkers(){
  if(!yMap)return;
  yMap.geoObjects.removeAll();
  mapMarkers=[];
  filtered().forEach(o=>{
    const placemark=new ymaps.Placemark([o.lat,o.lng],{
      iconCaption:o.name,
      hintContent:o.name,
      balloonContentHeader:`<b>${o.name}</b>`,
      balloonContentBody:`<div style="font-family:Arial,sans-serif"><div style="margin:6px 0;color:#666">${o.region} · ${o.category}</div><div style="margin:8px 0">${o.description}</div><button onclick="openObjectModal(${o.id})" style="padding:8px 12px;border:0;background:#7a5b34;color:white;cursor:pointer">Открыть карточку</button></div>`
    },{
      preset:'islands#darkGreenIcon',
      iconColor:'#8a6b3f'
    });
    placemark.events.add('click',()=>{ currentObjectId=o.id; });
    yMap.geoObjects.add(placemark);
    mapMarkers.push(placemark);
  });
}
function focusObject(id){
  const o=culturalObjects.find(x=>x.id===id);
  if(!o)return;
  if(yMap){
    yMap.setCenter([o.lat,o.lng],8,{duration:400});
  }
  openObjectModal(id);
}
function openObjectModal(id){const o=culturalObjects.find(x=>x.id===id);if(!o)return;currentObjectId=id;modalTitle.textContent=o.name;modalRegion.textContent=`${o.region} · ${o.period}`;modalBreadcrumbs.textContent=`${o.region}  ›  ${o.name}  ›  Предмет`;modalTags.innerHTML=[o.category,o.material,o.technique].map(x=>`<span class="tag">${x}</span>`).join('');modalDescription.textContent=o.description;modalSpecs.innerHTML=`<div class="spec"><span>Место создания</span><span>${o.region}</span></div><div class="spec"><span>Период</span><span>${o.period}</span></div><div class="spec"><span>Материал</span><span>${o.material}</span></div><div class="spec"><span>Техника</span><span>${o.technique}</span></div><div class="spec"><span>Назначение</span><span>${o.purpose}</span></div>`;modalOrigin.textContent=o.owner;similarGrid.innerHTML=culturalObjects.filter(x=>x.id!==id).slice(0,3).map(x=>`<div class="similar-card" onclick="openObjectModal(${x.id})"><div class="similar-icon">${x.emoji}</div><div><b>${x.name}</b><div class="obj-meta">${x.region}</div></div></div>`).join('');modalSaveBtn.textContent=savedIds.has(id)?'В избранном':'В избранное';objectModal.classList.add('active');init3D(o.shape)}
function closeModal(id){document.getElementById(id).classList.remove('active');if(animationFrameId)cancelAnimationFrame(animationFrameId);if(window.speechSynthesis)window.speechSynthesis.cancel()}
function init3D(shape){
  const c=modal3dContainer;
  if(animationFrameId) cancelAnimationFrame(animationFrameId);
  const oldCanvas=c.querySelector('canvas');
  if(oldCanvas) oldCanvas.remove();
  if(threeRenderer){
    try{ threeRenderer.dispose(); }catch(e){}
  }
  threeScene=new THREE.Scene();
  threeScene.background=new THREE.Color(0x0b0f0e);
  const w=c.clientWidth||640,h=c.clientHeight||330;
  threeCamera=new THREE.PerspectiveCamera(36,w/h,.1,1000);
  threeCamera.position.set(0,1.35,8.1);
  threeRenderer=new THREE.WebGLRenderer({antialias:true,alpha:true});
  threeRenderer.setPixelRatio(Math.min(window.devicePixelRatio||1,2));
  threeRenderer.setSize(w,h);
  c.prepend(threeRenderer.domElement);

  const hemi=new THREE.HemisphereLight(0xf6eddc,0x111513,1.15);
  threeScene.add(hemi);
  const key=new THREE.DirectionalLight(0xf2d6a2,1.1);
  key.position.set(5,8,6);
  threeScene.add(key);
  const rim=new THREE.DirectionalLight(0x6f8f88,.45);
  rim.position.set(-5,3,-4);
  threeScene.add(rim);

  const pedestal=new THREE.Mesh(
    new THREE.CylinderGeometry(2.55,2.85,.42,48),
    new THREE.MeshStandardMaterial({color:0x2a2117,roughness:.85,metalness:.08})
  );
  pedestal.position.y=-2.15;
  threeScene.add(pedestal);

  const floorGlow=new THREE.Mesh(
    new THREE.CircleGeometry(3.15,48),
    new THREE.MeshBasicMaterial({color:0x3d3120,transparent:true,opacity:.28})
  );
  floorGlow.rotation.x=-Math.PI/2;
  floorGlow.position.y=-1.92;
  threeScene.add(floorGlow);

  function mesh(g,m,x=0,y=0,z=0,rx=0,ry=0,rz=0){
    const n=new THREE.Mesh(g,m);
    n.position.set(x,y,z); n.rotation.set(rx,ry,rz); n.castShadow=false; n.receiveShadow=false;
    return n;
  }
  function mat(color,rough=.7,metal=.08){ return new THREE.MeshStandardMaterial({color,roughness:rough,metalness:metal}); }
  function woodTone(){ return mat(0x765032,.72,.06); }
  function darkWood(){ return mat(0x553521,.8,.04); }
  function metal(){ return mat(0x8f744a,.38,.42); }
  function brick(){ return mat(0xb99472,.92,.02); }
  function leather(){ return mat(0x6e3626,.82,.03); }
  function paintGold(){ return mat(0xc09b47,.55,.15); }
  function felt(){ return mat(0x4c2618,.96,.01); }

  function addDecorativeBands(obj,width,height,depth,count=5){
    for(let i=0;i<count;i++){
      const y=-height/2 + (i+1)*(height/(count+1));
      const band=mesh(new THREE.BoxGeometry(width+.03,.035,depth+.03),paintGold(),0,y,0);
      obj.add(band);
    }
  }

  function buildChurch(){
    const g=new THREE.Group();
    const wood=woodTone(), roof=darkWood(), brass=paintGold();
    const base=mesh(new THREE.BoxGeometry(3.7,.35,2.5),wood,0,-1.35,0);
    g.add(base);
    const hall=mesh(new THREE.BoxGeometry(2.8,2.0,1.9),wood,0,-.15,0);
    g.add(hall);
    addDecorativeBands(hall,2.8,2.0,1.9,6);
    const porch=mesh(new THREE.BoxGeometry(1.2,1.15,1.0),wood,0,-.55,1.38);
    g.add(porch);
    const porchRoof=mesh(new THREE.ConeGeometry(.95,.8,4),roof,0,.35,1.38,0,Math.PI/4,0);
    g.add(porchRoof);
    const sideWingL=mesh(new THREE.BoxGeometry(.95,1.15,1.15),wood,-1.5,-.55,0);
    const sideWingR=mesh(new THREE.BoxGeometry(.95,1.15,1.15),wood,1.5,-.55,0);
    g.add(sideWingL,sideWingR);
    const mainRoof=mesh(new THREE.ConeGeometry(1.8,1.35,4),roof,0,1.25,0,0,Math.PI/4,0);
    g.add(mainRoof);
    const tower=mesh(new THREE.CylinderGeometry(.52,.72,2.3,8),wood,0,1.25,0);
    g.add(tower);
    const towerRoof=mesh(new THREE.ConeGeometry(.9,1.25,8),roof,0,2.85,0);
    g.add(towerRoof);
    const dome=mesh(new THREE.SphereGeometry(.46,18,18),brass,0,3.55,0);
    dome.scale.set(.9,1.2,.9);
    g.add(dome);
    const crossV=mesh(new THREE.CylinderGeometry(.03,.03,.55,8),brass,0,4.1,0);
    const crossH=mesh(new THREE.CylinderGeometry(.03,.03,.28,8),brass,0,4.15,0,0,0,Math.PI/2);
    g.add(crossV,crossH);
    const fenceMat=mat(0x8b6843,.9,.04);
    for(let i=0;i<6;i++){
      const z=-1.35 + i*.54;
      g.add(mesh(new THREE.BoxGeometry(.05,.45,.05),fenceMat,-2.15,-1.45,z));
      g.add(mesh(new THREE.BoxGeometry(.05,.45,.05),fenceMat,2.15,-1.45,z));
    }
    g.add(mesh(new THREE.BoxGeometry(4.35,.05,.05),fenceMat,0,-1.25,-1.38));
    g.add(mesh(new THREE.BoxGeometry(4.35,.05,.05),fenceMat,0,-1.25,1.38));
    g.position.y=-.1;
    return g;
  }

  function buildBowl(){
    const g=new THREE.Group();
    const pts=[];
    [[0,.0],[.18,.12],[.42,.18],[.72,.42],[.96,.88],[1.06,1.28],[1.0,1.5],[.68,1.62],[.22,1.58],[.07,1.5],[0,1.44]].forEach(p=>pts.push(new THREE.Vector2(p[0],p[1])));
    const bowl=mesh(new THREE.LatheGeometry(pts,56),mat(0x191515,.82,.05),0,-1.15,0,Math.PI,0,0);
    g.add(bowl);
    const inner=mesh(new THREE.LatheGeometry([new THREE.Vector2(0,.08),new THREE.Vector2(.12,.1),new THREE.Vector2(.42,.22),new THREE.Vector2(.68,.62),new THREE.Vector2(.8,.96),new THREE.Vector2(.74,1.05)],56),mat(0x612915,.88,.02),0,-.3,0,Math.PI,0,0);
    inner.scale.set(1,1.12,1);
    g.add(inner);
    const foot=mesh(new THREE.CylinderGeometry(.52,.68,.26,36),darkWood(),0,-1.82,0);
    g.add(foot);
    const ring=mesh(new THREE.TorusGeometry(1.02,.06,12,48),paintGold(),0,.28,0,Math.PI/2,0,0);
    g.add(ring);
    for(let i=0;i<8;i++){
      const petal=mesh(new THREE.SphereGeometry(.14,10,10),paintGold(),Math.cos(i*Math.PI/4)*.63,-.36,Math.sin(i*Math.PI/4)*.63);
      petal.scale.set(1,.55,.28); g.add(petal);
    }
    g.rotation.x=.1;
    return g;
  }

  function buildBoot(){
    const g=new THREE.Group();
    const body=leather();
    const sole=mat(0x1c120d,.96,.01);
    const trim=paintGold();
    const soleBase=mesh(new THREE.BoxGeometry(2.35,.28,1.1),sole,0,-1.73,.1);
    soleBase.rotation.z=-.03; g.add(soleBase);
    const toe=mesh(new THREE.SphereGeometry(.56,16,16),body,.96,-1.46,.12);
    toe.scale.set(1.28,.62,.9); g.add(toe);
    const mid=mesh(new THREE.BoxGeometry(1.28,.72,1.0),body,.02,-1.25,.06);
    mid.rotation.z=-.09; g.add(mid);
    const heel=mesh(new THREE.BoxGeometry(.56,.46,.95),sole,-.9,-1.56,0);
    g.add(heel);
    const shaft=mesh(new THREE.CylinderGeometry(.45,.58,2.15,20),body,-.08,.0,0,0,0,.06);
    g.add(shaft);
    const cuff=mesh(new THREE.TorusGeometry(.56,.06,12,42),trim,-.08,.95,0,Math.PI/2,0,0);
    g.add(cuff);
    for(let i=0;i<4;i++){
      const stripe=mesh(new THREE.TorusGeometry(.44+.04*i,.015,8,32),trim,-.02,-.18+.28*i,0,Math.PI/2,0,0);
      stripe.scale.z=.72; g.add(stripe);
    }
    const sideDecor=mesh(new THREE.SphereGeometry(.17,12,12),trim,.35,-.2,.46);
    sideDecor.scale.set(.7,1.4,.2); g.add(sideDecor);
    g.rotation.z=-.16;
    return g;
  }

  function buildStove(){
    const g=new THREE.Group();
    const b=brick(), metalTone=metal();
    const body=mesh(new THREE.BoxGeometry(3.0,2.4,2.05),b,0,-.45,0);
    g.add(body);
    const upper=mesh(new THREE.BoxGeometry(2.0,.55,2.0),b,-.4,.95,0);
    g.add(upper);
    const chimney=mesh(new THREE.BoxGeometry(.55,2.1,.55),b,.95,2.0,0);
    g.add(chimney);
    const ledge=mesh(new THREE.BoxGeometry(.95,.28,1.3),b,1.02,-.18,.32);
    g.add(ledge);
    const archShape=new THREE.Shape();
    archShape.moveTo(-.52,-.45); archShape.lineTo(-.52,.0); archShape.absarc(0,0,.52,Math.PI,0,false); archShape.lineTo(.52,-.45); archShape.lineTo(-.52,-.45);
    const firebox=mesh(new THREE.ExtrudeGeometry(archShape,{depth:.12,bevelEnabled:false}),mat(0x443226,.98,0),0,-.8,1.04);
    firebox.rotation.y=Math.PI; g.add(firebox);
    const fireInner=mesh(new THREE.PlaneGeometry(.88,.78),mat(0x17120f,1,0),0,-.48,1.01);
    g.add(fireInner);
    const ironPlate=mesh(new THREE.CylinderGeometry(.36,.36,.06,28),metalTone,.0,.78,.74,Math.PI/2,0,0);
    const ironPlate2=mesh(new THREE.CylinderGeometry(.26,.26,.05,28),metalTone,-.62,.78,.74,Math.PI/2,0,0);
    g.add(ironPlate,ironPlate2);
    const bench=mesh(new THREE.BoxGeometry(1.2,.22,1.25),b,-1.1,-.15,-.15);
    g.add(bench);
    return g;
  }

  function buildChest(){
    const g=new THREE.Group();
    const wood=woodTone(), trim=metal();
    const box=mesh(new THREE.BoxGeometry(2.8,1.55,1.75),wood,0,-.82,0);
    g.add(box);
    const lid=mesh(new THREE.CylinderGeometry(.88,.88,2.8,28,1,false,0,Math.PI),wood,0,.16,0,0,0,Math.PI/2);
    lid.scale.z=1.0; g.add(lid);
    const frontPlate=mesh(new THREE.BoxGeometry(2.82,.18,.08),trim,0,-.36,.91);
    const frontPlate2=mesh(new THREE.BoxGeometry(2.82,.18,.08),trim,0,-1.0,.91);
    const v1=mesh(new THREE.BoxGeometry(.12,1.52,.08),trim,-1.0,-.8,.91);
    const v2=mesh(new THREE.BoxGeometry(.12,1.52,.08),trim,0,-.8,.91);
    const v3=mesh(new THREE.BoxGeometry(.12,1.52,.08),trim,1.0,-.8,.91);
    g.add(frontPlate,frontPlate2,v1,v2,v3);
    const handle=mesh(new THREE.TorusGeometry(.22,.03,8,24),trim,0,-.72,.98,0,0,0);
    g.add(handle);
    const sideH=mesh(new THREE.TorusGeometry(.18,.025,8,24),trim,-1.46,-.72,0,0,Math.PI/2,0);
    const sideH2=mesh(new THREE.TorusGeometry(.18,.025,8,24),trim,1.46,-.72,0,0,Math.PI/2,0);
    g.add(sideH,sideH2);
    return g;
  }

  const modelMap={building:buildChurch,bowl:buildBowl,boot:buildBoot,cube:buildStove,chest:buildChest};
  threeMesh=(modelMap[shape]||buildChest)();
  threeScene.add(threeMesh);

  let fitBox=new THREE.Box3().setFromObject(threeMesh);
  let fitSize=new THREE.Vector3();
  fitBox.getSize(fitSize);
  const maxDim=Math.max(fitSize.x,fitSize.y,fitSize.z)||1;
  const scale=Math.min(1.55,4.6/maxDim);
  threeMesh.scale.setScalar(scale);
  fitBox=new THREE.Box3().setFromObject(threeMesh);
  const fitCenter=new THREE.Vector3();
  fitBox.getCenter(fitCenter);
  threeMesh.position.x-=fitCenter.x;
  threeMesh.position.z-=fitCenter.z;
  fitBox=new THREE.Box3().setFromObject(threeMesh);
  const targetBottom=-1.45;
  threeMesh.position.y+=targetBottom-fitBox.min.y;

  let drag=false,px=0,py=0,rotY=0,rotX=.16,targetZoom=8.1;
  const cv=threeRenderer.domElement;
  cv.style.cursor='grab';
  cv.onmousedown=e=>{drag=true;px=e.clientX;py=e.clientY;cv.style.cursor='grabbing'};
  cv.onmousemove=e=>{
    if(!drag) return;
    rotY+=(e.clientX-px)*.012;
    rotX+=(e.clientY-py)*.01;
    rotX=Math.max(-.45,Math.min(.55,rotX));
    px=e.clientX; py=e.clientY;
  };
  cv.onwheel=e=>{
    e.preventDefault();
    targetZoom=Math.max(5.2,Math.min(10.2,targetZoom + Math.sign(e.deltaY)*.35));
  };
  window.onmouseup=()=>{drag=false;cv.style.cursor='grab'};

  function anim(){
    animationFrameId=requestAnimationFrame(anim);
    if(!drag) rotY+=.0045;
    threeMesh.rotation.y += (rotY-threeMesh.rotation.y)*.12;
    threeMesh.rotation.x += (rotX-threeMesh.rotation.x)*.12;
    threeCamera.position.z += (targetZoom-threeCamera.position.z)*.08;
    threeCamera.lookAt(0,0.35,0);
    threeRenderer.render(threeScene,threeCamera);
  }
  anim();
}

function openAddModal(){addModal.classList.add('active')}
openAddModalBtn.onclick=openAddModal;joinBtn.onclick=openAddModal;loginBtn.onclick=()=>loginModal.classList.add('active');
modalSaveBtn.onclick=()=>{const o=culturalObjects.find(x=>x.id===currentObjectId);if(savedIds.has(o.id)){savedIds.delete(o.id);o.saves=Math.max(0,o.saves-1)}else{savedIds.add(o.id);o.saves++}savedCount.textContent=savedIds.size;modalSaveBtn.textContent=savedIds.has(o.id)?'В избранном':'В избранное';renderList()};
modalShareBtn.onclick=()=>navigator.clipboard?.writeText(location.href).then(()=>modalShareBtn.textContent='Ссылка скопирована');
modalVoiceBtn.onclick=()=>{const o=culturalObjects.find(x=>x.id===currentObjectId);if(!o)return;if('speechSynthesis'in window){speechSynthesis.cancel();const u=new SpeechSynthesisUtterance(`История от потомка семьи. ${o.description}`);u.lang='ru-RU';u.rate=.9;speechSynthesis.speak(u);modalVoiceBtn.textContent='■';u.onend=()=>modalVoiceBtn.textContent='▶'}};
openCollectionBtn.onclick=()=>{const arr=culturalObjects.filter(o=>savedIds.has(o.id));collectionList.innerHTML=arr.length?arr.map(o=>`<li onclick="openObjectModal(${o.id});closeModal('collectionModal')"><span class="emoji-big">${o.emoji}</span><div class="obj-info"><div class="obj-name">${o.name}</div><div class="obj-meta">${o.region}</div></div></li>`).join(''):'<li>Пока ничего не сохранено</li>';collectionModal.classList.add('active')};
searchInput.oninput=()=>{renderList();renderMarkers()};categoryFilter.onchange=regionFilter.onchange=()=>{renderList();renderMarkers()};headerSearch.oninput=e=>{searchInput.value=e.target.value;renderList();renderMarkers();document.getElementById('mapSection').scrollIntoView({behavior:'smooth'})};
let localVisionPipe=null;
async function getLocalVision(){
  if(localVisionPipe)return localVisionPipe;
  aiStatus.textContent='ЛОКАЛЬНЫЙ ИИ: ПЕРВАЯ ЗАГРУЗКА МОДЕЛИ...';
  const {pipeline}=await import('https://cdn.jsdelivr.net/npm/@huggingface/transformers@3.7.2');
  localVisionPipe=await pipeline('image-to-text','Xenova/vit-gpt2-image-captioning',{dtype:'q8'});
  return localVisionPipe;
}
function fileToDataUrl(file){return new Promise((resolve,reject)=>{const r=new FileReader();r.onload=()=>resolve(String(r.result));r.onerror=reject;r.readAsDataURL(file)})}
function inferFieldsFromCaption(caption){
  const s=(caption||'').toLowerCase();
  let category='Быт', material='Требует уточнения', technique='Требует уточнения', purpose='Семейная реликвия';
  const has=(...w)=>w.some(x=>s.includes(x));
  if(has('shoe','boot','dress','shirt','coat','hat','clothing','garment')) category='Одежда';
  else if(has('instrument','guitar','violin','accordion','drum','music')) category='Музыка';
  else if(has('painting','picture','art','sculpture','icon')) category='Искусство';
  else if(has('tool','hammer','axe','saw','knife')) category='Инструменты';
  else if(has('building','house','church','tower','architecture')) category='Архитектура';
  else if(has('carved','craft','handmade','pottery','embroidery','woven')) category='Ремесло';
  if(has('wood','wooden')) material='дерево';
  else if(has('metal','iron','steel','copper','brass')) material='металл';
  else if(has('ceramic','pottery','clay')) material='керамика / глина';
  else if(has('fabric','cloth','textile','wool','cotton')) material='текстиль';
  else if(has('leather')) material='кожа';
  if(has('carved','carving')) technique='резьба';
  else if(has('painted','painting')) technique='роспись';
  else if(has('embroidered','embroidery')) technique='вышивка';
  else if(has('woven','weaving')) technique='ткачество';
  if(has('chest','box')) purpose='хранение вещей';
  else if(has('bowl','plate','cup','pot')) purpose='бытовая утварь';
  else if(category==='Одежда') purpose='элемент одежды';
  else if(category==='Музыка') purpose='музыкальный инструмент';
  let name='Семейный предмет';
  if(has('chest','box')) name='Старинный сундук';
  else if(has('bowl')) name='Деревянная или керамическая чаша';
  else if(has('plate')) name='Традиционная тарелка';
  else if(has('cup','mug')) name='Старинная кружка';
  else if(has('pot')) name='Глиняный горшок';
  else if(has('boot','shoe')) name='Традиционная обувь';
  else if(has('dress','shirt','coat','garment')) name='Предмет традиционной одежды';
  else if(has('guitar')) name='Струнный музыкальный инструмент';
  else if(has('violin')) name='Скрипка';
  else if(has('accordion')) name='Гармонь / аккордеон';
  else if(has('painting','picture')) name='Картина';
  else if(has('icon')) name='Икона';
  else if(has('tool','hammer','axe','saw')) name='Старинный инструмент';
  else if(has('building','house')) name='Традиционная постройка';
  else if(has('church')) name='Храмовая постройка';

  let human='ИИ выполнил предварительное распознавание предмета по фотографии. ';
  human+=`Предположительная категория: ${category.toLowerCase()}. `;
  human+=material!=='Требует уточнения' ? `Вероятный материал: ${material}. ` : 'Материал требует уточнения. ';
  human+=technique!=='Требует уточнения' ? `Предполагаемая техника изготовления: ${technique}. ` : 'Техника изготовления требует уточнения. ';
  human+=`Возможное назначение: ${purpose}. `;
  human+='Описание сформировано автоматически и должно быть проверено участником перед публикацией.';
  return {name,category,region:'Требует уточнения',period:'Требует уточнения',material,technique,purpose,description:human,lat:55.751244,lng:37.618423};
}
async function analyzeWithLocalAI(file){
  aiStatus.style.opacity='.9';
  try{
    const dataUrl=await fileToDataUrl(file);
    const pipe=await getLocalVision();
    aiStatus.textContent='ЛОКАЛЬНЫЙ ИИ: АНАЛИЗИРУЮ ИЗОБРАЖЕНИЕ...';
    const out=await pipe(dataUrl,{max_new_tokens:48});
    const caption=(out?.[0]?.generated_text||'').trim();
    const data=inferFieldsFromCaption(caption);
    addName.value=data.name;
    addCategory.value=data.category;
    addRegion.value=data.region;
    addPeriod.value=data.period;
    addMaterial.value=data.material;
    addTechnique.value=data.technique;
    addPurpose.value=data.purpose;
    addDescription.value=data.description;
    addLat.value=data.lat;
    addLng.value=data.lng;
    aiStatus.textContent='ЛОКАЛЬНЫЙ ИИ: ГОТОВО · ОБЪЕКТ РАСПОЗНАН · ОПИСАНИЕ СФОРМИРОВАНО НА РУССКОМ · ПРОВЕРЬТЕ ПОЛЯ';
    aiStatus.style.opacity='1';
  }catch(err){
    console.error(err);
    aiStatus.textContent=`ЛОКАЛЬНЫЙ ИИ: ОШИБКА · ${String(err?.message||err)} · МОЖНО ЗАПОЛНИТЬ ВРУЧНУЮ`;
    aiStatus.style.opacity='1';
  }
}
uploadArea.onclick=()=>fileInput.click();
fileInput.onchange=async e=>{const f=e.target.files[0];if(!f)return;const r=new FileReader();r.onload=ev=>previewContainer.innerHTML=`<img src="${ev.target.result}" style="max-width:180px;margin-top:12px">`;r.readAsDataURL(f);await analyzeWithLocalAI(f)};
addObjectForm.onsubmit=e=>{e.preventDefault();culturalObjects.unshift({id:Date.now(),name:addName.value,category:addCategory.value,region:addRegion.value,lat:+addLat.value,lng:+addLng.value,emoji:'✦',description:addDescription.value,saves:0,shape:'cube',period:addPeriod.value||'не указан',material:addMaterial.value||'не указан',technique:addTechnique.value||'не указана',purpose:addPurpose.value||'семейная реликвия',owner:addOwner.value||'Добавлено участником проекта'});populateFilters();renderList();renderMarkers();closeModal('addModal');addObjectForm.reset();previewContainer.innerHTML='';aiStatus.textContent='ОБЪЕКТ ОТПРАВЛЕН НА МОДЕРАЦИЮ';};
window.onload=()=>{populateFilters();renderList();initMap()};
</script>
</body>
</html>
