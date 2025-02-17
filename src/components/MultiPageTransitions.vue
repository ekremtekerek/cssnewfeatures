<script setup>
import { ref } from "vue";

const currentPage = ref("home");
const selectedEffect = ref("fade");
const isTransitioning = ref(false);

const pages = {
  home: {
    title: "Ana Sayfa",
    content:
      "Modern web geliştirme araçları ve teknolojileri hakkında güncel bilgiler.",
    cards: [
      { title: "Vue.js", desc: "Progressive JavaScript Framework" },
      { title: "CSS Transitions", desc: "Modern geçiş efektleri" },
      { title: "Web APIs", desc: "Modern tarayıcı özellikleri" },
    ],
  },
  about: {
    title: "Hakkımızda",
    content:
      "Web teknolojileri konusunda uzmanlaşmış ekibimizle hizmetinizdeyiz.",
    cards: [
      { title: "Ekibimiz", desc: "Uzman geliştiriciler" },
      { title: "Misyonumuz", desc: "Modern web deneyimi" },
      { title: "Vizyonumuz", desc: "Yenilikçi çözümler" },
    ],
  },
  blog: {
    title: "Blog",
    content: "Web geliştirme dünyasından en son haberler ve makaleler.",
    cards: [
      { title: "View Transitions", desc: "Yeni API özellikleri" },
      { title: "CSS Container", desc: "Container Queries" },
      { title: "Modern Layout", desc: "Grid ve Flexbox" },
    ],
  },
  contact: {
    title: "İletişim",
    content: "Bizimle iletişime geçin ve projelerinizi konuşalım.",
    cards: [
      { title: "E-posta", desc: "info@example.com" },
      { title: "Telefon", desc: "+90 123 456 7890" },
      { title: "Adres", desc: "İstanbul, Türkiye" },
    ],
  },
};

const effects = {
  fade: {
    name: "Fade Efekti",
    description:
      "Sayfa içeriği solarak kaybolur ve yeni içerik belirerek görünür.",
  },
  slide: {
    name: "Slide Efekti",
    description: "Sayfa içeriği yatay olarak kayar.",
  },
  scale: {
    name: "Scale Efekti",
    description: "Sayfa içeriği küçülerek kaybolur ve büyüyerek görünür.",
  },
  rotate: {
    name: "Rotate Efekti",
    description: "Sayfa içeriği dönerek kaybolur ve görünür.",
  },
};

const changePage = async (page) => {
  if (currentPage.value === page || isTransitioning.value) return;

  isTransitioning.value = true;

  if (!document.startViewTransition) {
    currentPage.value = page;
    isTransitioning.value = false;
    return;
  }

  try {
    const transition = document.startViewTransition(() => {
      currentPage.value = page;
    });

    await transition.finished;
  } catch (error) {
    console.error("Geçiş hatası:", error);
  } finally {
    isTransitioning.value = false;
  }
};

const changeEffect = (effect) => {
  if (isTransitioning.value) return;
  selectedEffect.value = effect;
};

const oldCode = `/* Eski Yöntem - Sayfa Geçişleri */

// index.html
<div class="page">
  <header class="header">
    <nav class="nav">...</nav>
  </header>
  <main class="content">
    <!-- Sayfa İçeriği -->
  </main>
</div>

// app.js
function navigateToPage(url) {
  // 1. Geçiş animasyonunu başlat
  const page = document.querySelector('.page');
  page.classList.add('page-exit');

  // 2. Animasyon bitince yeni sayfaya git
  page.addEventListener('animationend', () => {
    window.location.href = url;
  });
}

// styles.css
.page {
  opacity: 1;
  transform: translateY(0);
  transition: all 0.3s ease-out;
}

.page-exit {
  opacity: 0;
  transform: translateY(20px);
}`;

const newCode = `/* Yeni Yöntem - View Transitions API */

// index.html
<div class="page">
  <header class="header">
    <nav class="nav">...</nav>
  </header>
  <main class="content">
    <!-- Sayfa İçeriği -->
  </main>
</div>

// app.js
async function navigateToPage(url) {
  // 1. API desteğini kontrol et
  if (!document.startViewTransition) {
    window.location.href = url;
    return;
  }

  // 2. Geçiş animasyonunu başlat
  const transition = document.startViewTransition(async () => {
    // Yeni sayfayı yükle ve DOM'u güncelle
    const response = await fetch(url);
    const html = await response.text();
    document.body.innerHTML = html;
  });

  // 3. Geçişi tamamla
  await transition.finished;
}

// styles.css
@keyframes slide-from-right {
  from { transform: translateX(100%); }
  to   { transform: translateX(0); }
}

::view-transition-old(root) {
  animation: fade-out 0.3s ease;
}

::view-transition-new(root) {
  animation: slide-from-right 0.3s ease;
}`;
</script>

<template>
  <div class="feature-section">
    <h2>Çok Sayfalı Görüntüleme Geçişleri</h2>
    <p class="description">
      View Transitions API ile sayfa geçişlerini yönetmek artık çok daha kolay.
      Bu API sayesinde sayfalar arası geçişlerde profesyonel animasyonlar
      oluşturabilir ve kullanıcı deneyimini geliştirebilirsiniz.
    </p>

    <div class="example">
      <div class="code-comparison">
        <div class="code-block">
          <h3>Eski Yöntem</h3>
          <div class="code-header">
            <span class="code-label">Geleneksel JavaScript ve CSS</span>
          </div>
          <pre><code>{{ oldCode }}</code></pre>
        </div>

        <div class="code-block">
          <h3>Yeni Yöntem</h3>
          <div class="code-header">
            <span class="code-label">View Transitions API</span>
          </div>
          <pre><code>{{ newCode }}</code></pre>
        </div>
      </div>

      <div class="demo">
        <h3>İnteraktif Demo</h3>
        <div class="demo-container">
          <!-- Tarayıcı Simülasyonu -->
          <div class="browser-frame">
            <div class="browser-header">
              <div class="browser-actions">
                <span class="action red"></span>
                <span class="action yellow"></span>
                <span class="action green"></span>
              </div>
              <div class="browser-address">
                <span>https://example.com/{{ currentPage }}</span>
              </div>
            </div>

            <div class="browser-content">
              <!-- Navigasyon -->
              <nav class="demo-nav">
                <button
                  v-for="(page, key) in pages"
                  :key="key"
                  @click="changePage(key)"
                  :class="['nav-link', { active: currentPage === key }]"
                >
                  {{ page.title }}
                </button>
              </nav>

              <!-- Sayfa İçeriği -->
              <div class="page-wrapper" :class="selectedEffect">
                <div class="page-content">
                  <h3>{{ pages[currentPage].title }}</h3>
                  <p>{{ pages[currentPage].content }}</p>

                  <div class="card-grid">
                    <div
                      v-for="(card, index) in pages[currentPage].cards"
                      :key="index"
                      class="card"
                    >
                      <h4>{{ card.title }}</h4>
                      <p>{{ card.desc }}</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Geçiş Efektleri Seçimi -->
          <div class="transitions-control">
            <h4>Geçiş Efektini Seçin</h4>
            <div class="effects-grid">
              <button
                v-for="(effect, key) in effects"
                :key="key"
                @click="changeEffect(key)"
                :class="['effect-button', { active: selectedEffect === key }]"
              >
                <span class="effect-name">{{ effect.name }}</span>
                <div class="effect-preview" :class="key"></div>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* View Transitions */
::view-transition-old(page),
::view-transition-new(page) {
  animation: none;
  mix-blend-mode: normal;
}

/* Fade Effect */
.fade ::view-transition-old(page) {
  animation: fade-out 0.5s ease forwards !important;
}

.fade ::view-transition-new(page) {
  animation: fade-in 0.5s ease forwards !important;
}

/* Slide Effect */
.slide ::view-transition-old(page) {
  animation: slide-out 0.5s ease forwards !important;
}

.slide ::view-transition-new(page) {
  animation: slide-in 0.5s ease forwards !important;
}

/* Scale Effect */
.scale ::view-transition-old(page) {
  animation: scale-out 0.5s ease forwards !important;
}

.scale ::view-transition-new(page) {
  animation: scale-in 0.5s ease forwards !important;
}

/* Rotate Effect */
.rotate ::view-transition-old(page) {
  animation: rotate-out 0.5s ease forwards !important;
}

.rotate ::view-transition-new(page) {
  animation: rotate-in 0.5s ease forwards !important;
}

/* Animation Keyframes */
@keyframes fade-out {
  to {
    opacity: 0;
  }
}

@keyframes fade-in {
  from {
    opacity: 0;
  }
}

@keyframes slide-out {
  to {
    transform: translateX(-100%);
    opacity: 0;
  }
}

@keyframes slide-in {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
}

@keyframes scale-out {
  to {
    transform: scale(0.8);
    opacity: 0;
  }
}

@keyframes scale-in {
  from {
    transform: scale(1.2);
    opacity: 0;
  }
}

@keyframes rotate-out {
  to {
    transform: rotate(-10deg) scale(0.95);
    opacity: 0;
  }
}

@keyframes rotate-in {
  from {
    transform: rotate(10deg) scale(0.95);
    opacity: 0;
  }
}

/* Effect Preview Animations */
.effect-preview.fade::after {
  animation: preview-fade 2s infinite;
}

.effect-preview.slide::after {
  animation: preview-slide 2s infinite;
}

.effect-preview.scale::after {
  animation: preview-scale 2s infinite;
}

.effect-preview.rotate::after {
  animation: preview-rotate 2s infinite;
}

@keyframes preview-fade {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.3;
  }
}

@keyframes preview-slide {
  0%,
  100% {
    transform: translateX(0);
  }
  50% {
    transform: translateX(233%);
  }
}

@keyframes preview-scale {
  0%,
  100% {
    transform: scale(1);
  }
  50% {
    transform: scale(0.5);
  }
}

@keyframes preview-rotate {
  0%,
  100% {
    transform: rotate(0);
  }
  50% {
    transform: rotate(180deg);
  }
}

/* Diğer stiller buraya gelecek */

.feature-section {
  margin-bottom: 3rem;
}

.description {
  margin: 1rem 0;
  line-height: 1.6;
  color: #374151;
}

/* Kod Blokları */
.code-comparison {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  margin: 2rem 0;
}

.code-block {
  background: #1e1e1e;
  border-radius: 8px;
  overflow: hidden;
}

.code-header {
  background: #2d2d2d;
  padding: 1rem;
  border-bottom: 1px solid #404040;
}

.code-label {
  font-size: 0.875rem;
  color: #9ca3af;
}

pre {
  margin: 0;
  padding: 1.5rem;
  background: #1e1e1e;
  overflow-x: auto;
}

code {
  font-family: "Fira Code", monospace;
  font-size: 0.875rem;
  line-height: 1.6;
  color: #d4d4d4;
}

/* Syntax Highlighting */
.code-block .comment {
  color: #6a9955;
}
.code-block .keyword {
  color: #569cd6;
}
.code-block .string {
  color: #ce9178;
}
.code-block .function {
  color: #dcdcaa;
}
.code-block .class {
  color: #4ec9b0;
}
.code-block .selector {
  color: #d7ba7d;
}
.code-block .property {
  color: #9cdcfe;
}

/* Tarayıcı Çerçevesi */
.browser-frame {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  margin-bottom: 2rem;
}

.browser-header {
  background: #f3f4f6;
  padding: 0.75rem 1rem;
  display: flex;
  align-items: center;
  gap: 1rem;
}

.browser-actions {
  display: flex;
  gap: 0.5rem;
}

.action {
  width: 12px;
  height: 12px;
  border-radius: 50%;
}

.action.red {
  background: #ef4444;
}
.action.yellow {
  background: #f59e0b;
}
.action.green {
  background: #10b981;
}

.browser-address {
  flex: 1;
  background: white;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  font-size: 0.875rem;
  color: #6b7280;
}

.browser-content {
  padding: 1.5rem;
}

/* Navigasyon */
.demo-nav {
  display: flex;
  gap: 1rem;
  margin-bottom: 1.5rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid #e5e7eb;
}

.nav-link {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  background: #f3f4f6;
  color: #4b5563;
  cursor: pointer;
  transition: all 0.2s ease;
}

.nav-link:hover {
  background: #e5e7eb;
}

.nav-link.active {
  background: #3b82f6;
  color: white;
}

/* Sayfa İçeriği */
.page-wrapper {
  position: relative;
  view-transition-name: page;
}

.page-content {
  padding: 1rem 0;
}

.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
  margin-top: 1.5rem;
}

.card {
  background: #f9fafb;
  padding: 1.5rem;
  border-radius: 8px;
  border: 1px solid #e5e7eb;
  transition: all 0.3s ease;
}

.card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

/* Geçiş Efektleri Kontrolü */
.transitions-control {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.effects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  margin-top: 1rem;
}

.effect-button {
  background: #f9fafb;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  padding: 1rem;
  cursor: pointer;
  transition: all 0.2s ease;
}

.effect-button:hover {
  background: #f3f4f6;
}

.effect-button.active {
  border-color: #3b82f6;
  background: #eff6ff;
}

.effect-name {
  display: block;
  margin-bottom: 0.5rem;
  color: #374151;
  font-weight: 500;
}

.effect-preview {
  height: 4px;
  background: #e5e7eb;
  border-radius: 2px;
  overflow: hidden;
  position: relative;
}

.effect-preview::after {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 30%;
  height: 100%;
  background: #3b82f6;
  border-radius: 2px;
}

/* Diğer stiller buraya gelecek */
</style>
