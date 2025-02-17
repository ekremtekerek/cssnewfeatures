<script setup>
import { ref } from 'vue'

const currentView = ref('list')
const showModal = ref(false)
const selectedTransition = ref('fade')
const items = ref([
  { id: 1, title: 'Kart 1', description: 'Vue.js ile web geliştirme', image: 'https://picsum.photos/200?random=1' },
  { id: 2, title: 'Kart 2', description: 'Modern CSS özellikleri', image: 'https://picsum.photos/200?random=2' },
  { id: 3, title: 'Kart 3', description: 'JavaScript framework\'leri', image: 'https://picsum.photos/200?random=3' },
  { id: 4, title: 'Kart 4', description: 'Responsive tasarım', image: 'https://picsum.photos/200?random=4' },
  { id: 5, title: 'Kart 5', description: 'Web animasyonları', image: 'https://picsum.photos/200?random=5' },
  { id: 6, title: 'Kart 6', description: 'Frontend teknolojileri', image: 'https://picsum.photos/200?random=6' },
])

const oldCode = `
/* JavaScript ile sayfa geçişi animasyonu */
function navigateToPage(url) {
  // Mevcut sayfayı gizle
  document.body.style.opacity = 0;
  
  // Yeni sayfayı yükle
  setTimeout(() => {
    window.location.href = url;
  }, 300);
}

/* CSS ile basit geçiş efekti */
body {
  transition: opacity 0.3s;
}`

const newCode = `
/* View Transitions API kullanımı */
document.startViewTransition(() => {
  // DOM değişiklikleri
});

/* CSS ile geçiş stillerini özelleştirme */
::view-transition-old(root) {
  animation: fade-out 0.5s ease-out;
}

::view-transition-new(root) {
  animation: fade-in 0.5s ease-in;
}

/* Özel geçiş efektleri */
::view-transition-old(header) {
  animation: slide-out 0.5s ease;
}

::view-transition-new(header) {
  animation: slide-in 0.5s ease;
}`

const transitions = {
  fade: `
.view-content.fade::view-transition-old {
  animation: fade-out 0.5s ease-out forwards !important;
}
.view-content.fade::view-transition-new {
  animation: fade-in 0.5s ease-in forwards !important;
}`,
  slide: `
.view-content.slide::view-transition-old {
  animation: slide-out 0.5s ease forwards !important;
}
.view-content.slide::view-transition-new {
  animation: slide-in 0.5s ease forwards !important;
}`,
  scale: `
.view-content.scale::view-transition-old {
  animation: scale-out 0.5s ease forwards !important;
}
.view-content.scale::view-transition-new {
  animation: scale-in 0.5s ease forwards !important;
}`
}

const changeView = async (newView) => {
  if (!document.startViewTransition) {
    currentView.value = newView
    return
  }

  const transition = document.startViewTransition(() => {
    currentView.value = newView
  })

  try {
    await transition.finished
  } catch (error) {
    console.error('Geçiş hatası:', error)
  }
}

const toggleModal = async (show) => {
  if (!document.startViewTransition) {
    showModal.value = show
    return
  }

  const transition = document.startViewTransition(() => {
    showModal.value = show
  })

  try {
    await transition.finished
  } catch (error) {
    console.error('Modal geçiş hatası:', error)
  }
}

const applyTransition = async (type) => {
  if (!document.startViewTransition) {
    selectedTransition.value = type
    return
  }

  const transition = document.startViewTransition(() => {
    selectedTransition.value = type
  })

  try {
    await transition.finished
  } catch (error) {
    console.error('Geçiş stili değişim hatası:', error)
  }
}
</script>

<template>
  <div class="feature-section">
    <h2>View Transitions API</h2>
    <p class="description">
      View Transitions API, modern web uygulamalarında sayfa geçişlerini ve DOM değişikliklerini
      yönetmek için güçlü bir araçtır. Bu API sayesinde karmaşık animasyonları kolayca
      uygulayabilir ve kullanıcı deneyimini geliştirebilirsiniz.
    </p>

    <div class="example">
      <h3>Eski Yöntem vs Yeni Yöntem</h3>
      <div class="code-comparison">
        <div class="code-block">
          <h4>Eski Yöntem</h4>
          <pre><code>{{ oldCode }}</code></pre>
        </div>
        <div class="code-block">
          <h4>Yeni Yöntem (View Transitions)</h4>
          <pre><code>{{ newCode }}</code></pre>
        </div>
      </div>

      <h3>Geçiş Efektleri</h3>
      <div class="transition-types">
        <button 
          v-for="type in ['fade', 'slide', 'scale']" 
          :key="type"
          @click="applyTransition(type)"
          :class="{ active: selectedTransition === type }">
          {{ type.charAt(0).toUpperCase() + type.slice(1) }} Geçişi
        </button>
      </div>

      <pre class="transition-code"><code>{{ transitions[selectedTransition] }}</code></pre>

      <div class="demo">
        <h3>İnteraktif Örnekler</h3>
        <div class="demo-container">
          <div class="view-demo">
            <div class="view-controls">
              <button @click="changeView('list')" :class="{ active: currentView === 'list' }">
                📋 Liste Görünümü
              </button>
              <button @click="changeView('grid')" :class="{ active: currentView === 'grid' }">
                📑 Grid Görünümü
              </button>
            </div>

            <div class="view-content" :class="[currentView, selectedTransition]">
              <div v-for="item in items" :key="item.id" class="item">
                <div class="item-image">
                  <img :src="item.image" :alt="item.title">
                </div>
                <div class="item-content">
                  <h4>{{ item.title }}</h4>
                  <p>{{ item.description }}</p>
                  <button @click="toggleModal(true)" class="details-btn">
                    Detayları Gör
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal -->
    <div v-if="showModal" class="modal" @click.self="toggleModal(false)">
      <div class="modal-content">
        <h3>View Transitions Detayları</h3>
        <div class="modal-body">
          <p>View Transitions API'nin sunduğu özellikler:</p>
          <ul>
            <li>Otomatik DOM değişikliği algılama</li>
            <li>Özelleştirilebilir geçiş efektleri</li>
            <li>Performans optimizasyonu</li>
            <li>Kolay entegrasyon</li>
          </ul>
        </div>
        <button @click="toggleModal(false)" class="close-btn">Kapat</button>
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
  display: grid;
  gap: 2rem;
}

/* View Controls */
.view-controls {
  display: flex;
  gap: 1rem;
  margin-bottom: 1rem;
}

.view-controls button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  background: #e5e7eb;
  cursor: pointer;
  transition: all 0.3s ease;
}

.view-controls button.active {
  background: #3b82f6;
  color: white;
}

/* View Content */
.view-content {
  display: grid;
  gap: 1rem;
  view-transition-name: view-content;
}

.view-content.list {
  grid-template-columns: 1fr;
}

.view-content.grid {
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
}

.item {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: transform 0.3s ease;
}

.item img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.item-content {
  padding: 1rem;
}

/* Modal */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  view-transition-name: modal;
}

.modal-content {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  max-width: 500px;
  width: 90%;
}

/* View Transition Effects */
/* Fade efekti */
.view-content.fade::view-transition-old {
  animation: fade-out 0.5s ease-out forwards !important;
}
.view-content.fade::view-transition-new {
  animation: fade-in 0.5s ease-in forwards !important;
}

/* Slide efekti */
.view-content.slide::view-transition-old {
  animation: slide-out 0.5s ease forwards !important;
}
.view-content.slide::view-transition-new {
  animation: slide-in 0.5s ease forwards !important;
}

/* Scale efekti */
.view-content.scale::view-transition-old {
  animation: scale-out 0.5s ease forwards !important;
}
.view-content.scale::view-transition-new {
  animation: scale-in 0.5s ease forwards !important;
}

/* Animasyonlar */
@keyframes fade-out {
  from { opacity: 1; }
  to { opacity: 0; }
}

@keyframes fade-in {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slide-out {
  from { 
    transform: translateX(0);
    opacity: 1;
  }
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
  to { 
    transform: translateX(0);
    opacity: 1;
  }
}

@keyframes scale-out {
  from { 
    transform: scale(1);
    opacity: 1;
  }
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
  to { 
    transform: scale(1);
    opacity: 1;
  }
}

.item:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}

.item-image {
  position: relative;
  overflow: hidden;
}

.item-image img {
  transition: transform 0.3s ease;
}

.item:hover .item-image img {
  transform: scale(1.05);
}

.details-btn {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.details-btn:hover {
  background: #2563eb;
}

.modal-body {
  margin: 1rem 0;
}

.modal-body ul {
  margin-left: 1.5rem;
}

.modal-body li {
  margin: 0.5rem 0;
}

.close-btn {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.close-btn:hover {
  background: #dc2626;
}

.code-comparison {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  margin: 2rem 0;
}

.code-block {
  background: #f8f9fa;
  padding: 1.5rem;
  border-radius: 8px;
  border: 1px solid #e9ecef;
}

.code-block h4 {
  margin-top: 0;
  margin-bottom: 1rem;
  color: #3b82f6;
}

.transition-code {
  background: #f8f9fa;
  padding: 1.5rem;
  border-radius: 8px;
  border: 1px solid #e9ecef;
  margin: 1rem 0;
}

/* Transition Types */
.transition-types {
  display: flex;
  gap: 1rem;
  margin: 1rem 0;
}

.transition-types button {
  padding: 0.5rem 1rem;
  border: 2px solid #3b82f6;
  border-radius: 4px;
  background: transparent;
  color: #3b82f6;
  cursor: pointer;
  transition: all 0.3s ease;
}

.transition-types button.active {
  background: #3b82f6;
  color: white;
}
</style>
