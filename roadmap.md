# Roadmap — Инструкция по развитию и интеграции 3D-моделей

## 📌 Текущий статус проекта (Фаза 1 завершена)
- [x] Запущен локальный HTTP-сервер на порту 8000 (`http://localhost:8000`).
- [x] Обе фичи (Акселерометр и PPG-пульсометр) объединены на **одной странице** (single-page scrollytelling).
- [x] Строгий минималистичный дизайн в **черно-белых тонах** (high-contrast monochrome).
- [x] Встроенная интерактивная **3D-модель разобранных часов (Exploded View)** на Three.js прямо в шапке сайта.
- [x] Ползунок ручной разборки слоев (Sapphire Glass $\to$ Screen $\to$ PCB/MEMS $\to$ Battery $\to$ PPG $\to$ Housing).
- [x] Вращение мышью и тачем на 360°.

---

## 🎯 Инструкция: Как интегрировать высокополигональную 3D-модель (.GLTF / .GLB) для ВАУ-эффекта на защите

Сейчас в шапке работает процедурная 3D-модель на Three.js (не требует внешних файлов). Если ваша команда хочет подключить фотореалистичную готовую CAD/3D-модель (из Blender, Sketchfab или Free3D):

### Шаг 1. Подготовка модели в Blender
1. Скачайте модель фитнес-часов (формат `.fbx`, `.obj` или `.blend`).
2. В Blender разделите модель на **отдельные логические объекты (Objects)**:
   - `Layer_01_Glass` (защитное стекло)
   - `Layer_02_Screen` (экран)
   - `Layer_03_Motherboard` (плата с чипом акселерометра)
   - `Layer_04_Battery` (аккумулятор)
   - `Layer_05_PPG_Sensor` (датчик пульса со светодиодами)
   - `Layer_06_Case` (корпус с ремешками)
3. Убедитесь, что центр (Origin) каждого объекта находится в центре детали.
4. Экспортируйте в формате **`.glb`** (File $\to$ Export $\to$ glTF 2.0 (`.glb`)). Положите файл в папку проекта, например: `watch_exploded.glb`.

---

### Шаг 2. Подключение `GLTFLoader` в `index.html`
Добавьте скрипт загрузчика Three.js в `<head>`:
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/loaders/GLTFLoader.js"></script>
```

---

### Шаг 3. Загрузка и привязка к скроллу / ползунку
Замените процедурные меши в блоке `init3DExplodedView` следующим кодом:

```javascript
const loader = new THREE.GLTFLoader();
const parts = [];

loader.load('watch_exploded.glb', (gltf) => {
    const model = gltf.scene;
    scene.add(model);

    // Центрируем модель
    const box = new THREE.Box3().setFromObject(model);
    const center = box.getCenter(new THREE.Vector3());
    model.position.sub(center);

    // Находим ключевые детали по именам из Blender
    model.traverse((child) => {
        if (child.isMesh) {
            // Настраиваем премиальные материалы (матовый металл, стекло)
            child.material.metalness = 0.8;
            child.material.roughness = 0.2;

            // Запоминаем исходную позицию Y для разборки
            parts.push({
                mesh: child,
                initialY: child.position.y,
                // Коэффициент смещения в зависимости от названия
                offsetMultiplier: getLayerMultiplier(child.name)
            });
        }
    });
});

// Функция смещения слоев
function getLayerMultiplier(name) {
    if (name.includes('Glass')) return 3.5;
    if (name.includes('Screen')) return 2.0;
    if (name.includes('Motherboard')) return 0.8;
    if (name.includes('Battery')) return -0.5;
    if (name.includes('PPG')) return -2.0;
    if (name.includes('Case')) return -3.2;
    return 0;
}

// Привязка к ползунку или скроллу
function updateExplode(progress) { // progress от 0.0 до 1.0
    parts.forEach(p => {
        p.mesh.position.y = p.initialY + p.offsetMultiplier * progress;
    });
}
```

---

### Шаг 4. Scrollytelling (Разборка по мере скролла страницы)
Чтобы часы плавно разбирались, когда пользователь просто крутит колесико мыши:

```javascript
window.addEventListener('scroll', () => {
    const scrollY = window.scrollY;
    const heroHeight = document.getElementById('hero').offsetHeight;

    // Вычисляем процент скролла первого экрана от 0 до 1
    const progress = Math.min(1, Math.max(0, scrollY / (heroHeight * 0.7)));

    // Обновляем разборку 3D-модели
    updateExplode(progress);

    // Синхронизируем значение ползунка
    slider.value = progress;
    sliderVal.textContent = Math.round(progress * 100) + '%';
});
```

---

## 💡 Сценарий презентации для жюри (Pitch Strategy)

1. **0:00 – 0:30 (Введение & ВАУ-эффект с 3D)**:
   - Откройте сайт на `http://localhost:8000`.
   - Покажите собранные часы.
   - Потяните ползунок «Exploded View» или начните скроллить: часы эффектно разделяются на слои.
   - Сформулируйте тезис: *"Фитнес-часы кажутся простым экраном на руке, но внутри скрыты сложнейшие микроэлектромеханические и оптические системы. Давайте разберем две главные."*

2. **0:30 – 1:30 (Акселерометр и шаги)**:
   - Проскролльте до раздела **01 / The MEMS Accelerometer**.
   - Переключитесь на сцену 2 и 3: покажите инерционную массу, зазор пластин $d$ и формулу $C = \varepsilon A / d$.
   - Подчеркните: *"Часы не видят наши ноги. Они фиксируют инерцию микроскопического грузика при каждом взмахе руки."*
   - Покажите сцену 5 с графиком и порогом распознавания.

3. **1:30 – 2:30 (Оптический датчик PPG и пульс)**:
   - Перейдите к разделу **02 / Optical Photoplethysmography**.
   - Покажите сцену 2 (разрез кожи) и сцену 3 (пульсовая волна).
   - Объясните главный научный факт: *"Зеленый свет выбран не случайно — гемоглобин крови сильнее всего поглощает спектр ~530 нм. В систолу сосуд расширяется, поглощает больше света, и фотодиод фиксирует спад отражения."*
   - Покажите формулу $\text{BPM} = 60 / \Delta t$.

4. **2:30 – 3:00 (Ответы на вопросы и код)**:
   - Укажите жюри, что проект собран на чистом веб-стеке без тяжелых библиотек, с математически достоверными кривыми сигналов и адаптирован под презентацию.
