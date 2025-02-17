<script setup>
import { ref } from "vue";

const currentPage = ref(1);
const transitionType = ref("slide");

const oldCode = `
/* JavaScript ile sayfa geçişleri */
function changePage(direction) {
  const currentPage = document.querySelector('.current');
  const nextPage = direction === 'next' 
    ? currentPage.nextElementSibling 
    : currentPage.previousElementSibling;
    
  if (nextPage) {
    currentPage.classList.remove('current');
    nextPage.classList.add('current');
  }
}`;

const newCode = `
/* View Transition Types kullanımı */
document.startViewTransition({
  update: () => updatePage(nextPageIndex),
  types: ['slide', 'forwards']
});

/* CSS ile geçiş stillerini özelleştirme */
@view-transition[slide] {
  from { transform: translateX(100%); }
  to { transform: translateX(0); }
}

@view-transition[fade] {
  from { opacity: 0; }
  to { opacity: 1; }
}`;

const pages = [
  { id: 1, title: "Ana Sayfa", content: "Ana sayfa içeriği burada yer alır." },
  { id: 2, title: "Hakkımızda", content: "Hakkımızda sayfası içeriği." },
  { id: 3, title: "İletişim", content: "İletişim bilgileri burada." },
];

const changePage = async (newPage) => {
  if (!document.startViewTransition) {
    currentPage.value = newPage;
    return;
  }

  const direction = newPage > currentPage.value ? "forwards" : "backwards";

  await document.startViewTransition({
    update: () => {
      currentPage.value = newPage;
    },
    types: [transitionType.value, direction],
  }).finished;
};
</script>

<template>
  <div class="feature-section">
    <h2>View Transition Types</h2>
    <p class="description">
      View Transition Types, farklı sayfa geçişleri için özel animasyon türleri
      tanımlamamızı sağlar. Örneğin, ileri ve geri gezinme için farklı
      animasyonlar kullanabiliriz.
    </p>

    <div class="example">
      <h3>Eski Yöntem</h3>
      <pre><code>{{ oldCode }}</code></pre>

      <h3>Yeni Yöntem (View Transition Types)</h3>
      <pre><code>{{ newCode }}</code></pre>

      <div class="demo">
        <h3>Canlı Demo</h3>
        <div class="demo-container">
          <div class="controls">
            <div class="transition-controls">
              <label>Geçiş Türü:</label>
              <select v-model="transitionType">
                <option value="slide">Slide</option>
                <option value="fade">Fade</option>
                <option value="zoom">Zoom</option>
              </select>
            </div>
            <div class="navigation">
              <button
                @click="changePage(currentPage - 1)"
                :disabled="currentPage === 1"
              >
                Önceki
              </button>
              <span>Sayfa {{ currentPage }} / {{ pages.length }}</span>
              <button
                @click="changePage(currentPage + 1)"
                :disabled="currentPage === pages.length"
              >
                Sonraki
              </button>
            </div>
          </div>

          <div class="page-container">
            <div class="page" :class="transitionType">
              <h3>{{ pages[currentPage - 1].title }}</h3>
              <p>{{ pages[currentPage - 1].content }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.feature-section {
  margin-bottom: 3rem;
}

.description {
  margin: 1rem 0;
  line-height: 1.6;
}

pre {
  background: #f8f9fa;
  padding: 1rem;
  border-radius: 4px;
  overflow-x: auto;
  margin: 1rem 0;
}

code {
  font-family: monospace;
  white-space: pre;
}

.demo {
  margin-top: 2rem;
}

.demo-container {
  background: #f8f9fa;
  padding: 2rem;
  border-radius: 8px;
}

.controls {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}

.transition-controls {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.transition-controls select {
  padding: 0.5rem;
  border-radius: 4px;
  border: 1px solid #d1d5db;
}

.navigation {
  display: flex;
  align-items: center;
  gap: 1rem;
}

button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  background: #3b82f6;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
}

button:disabled {
  background: #d1d5db;
  cursor: not-allowed;
}

.page-container {
  background: white;
  border-radius: 8px;
  padding: 2rem;
  min-height: 300px;
  view-transition-name: page;
}

.page {
  view-transition-name: page-content;
}

/* View Transition Animations */
::view-transition-old(page-content),
::view-transition-new(page-content) {
  animation: none;
  mix-blend-mode: normal;
}

.page.slide {
  animation: slide-in 0.3s ease;
}

.page.fade {
  animation: fade-in 0.3s ease;
}

.page.zoom {
  animation: zoom-in 0.3s ease;
}

@keyframes slide-in {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

@keyframes fade-in {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes zoom-in {
  from {
    transform: scale(0.8);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}
</style>
