<script setup>
import { ref } from 'vue'

const selectedExample = ref('responsive-card')
const containerWidth = ref(800)
const selectedTheme = ref('light')

const oldCode = `
/* Eski yöntem - Tekrar eden media query'ler */
@media (max-width: 768px) {
  .card {
    flex-direction: column;
  }
}

@media (max-width: 768px) {
  .sidebar {
    display: none;
  }
}

@media (prefers-color-scheme: dark) {
  .card {
    background: #1a1a1a;
    color: white;
  }
}

@media (prefers-reduced-motion: reduce) {
  .card {
    transition: none;
  }
}`

const newCode = `
/* Custom Media Queries tanımlamaları */
@custom-media --mobile (max-width: 768px);
@custom-media --tablet (min-width: 769px) and (max-width: 1024px);
@custom-media --desktop (min-width: 1025px);
@custom-media --dark-mode (prefers-color-scheme: dark);
@custom-media --reduced-motion (prefers-reduced-motion: reduce);
@custom-media --touch (hover: none) and (pointer: coarse);
@custom-media --high-contrast (prefers-contrast: high);

/* Responsive Card */
.card {
  padding: 1rem;
  transition: transform 0.3s ease;

  @media (--mobile) {
    flex-direction: column;
    padding: 0.5rem;
  }

  @media (--tablet) {
    flex-direction: row;
    gap: 1rem;
  }

  @media (--desktop) {
    max-width: 1200px;
    margin: 0 auto;
  }

  @media (--dark-mode) {
    background: #1a1a1a;
    color: white;
  }

  @media (--reduced-motion) {
    transition: none;
  }
}

/* Responsive Grid */
.grid {
  display: grid;
  gap: 1rem;

  @media (--mobile) {
    grid-template-columns: 1fr;
  }

  @media (--tablet) {
    grid-template-columns: repeat(2, 1fr);
  }

  @media (--desktop) {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* Touch Optimizasyonu */
.button {
  padding: 0.5rem 1rem;

  @media (--touch) {
    padding: 1rem 2rem;
    font-size: 1.1rem;
  }
}

/* Yüksek Kontrast Modu */
.text {
  color: #666;

  @media (--high-contrast) {
    color: #000;
  }
}`

const htmlCode = `
<!-- Responsive Card -->
<div class="card">
  <div class="card-image">
    <img src="image.jpg" alt="Card Image">
  </div>
  <div class="card-content">
    <h3>Card Title</h3>
    <p>Card description goes here...</p>
    <button class="button">Learn More</button>
  </div>
</div>

<!-- Responsive Grid -->
<div class="grid">
  <div class="grid-item">
    <img src="item1.jpg" alt="Item 1">
    <h4>Grid Item 1</h4>
  </div>
  <div class="grid-item">
    <img src="item2.jpg" alt="Item 2">
    <h4>Grid Item 2</h4>
  </div>
  <div class="grid-item">
    <img src="item3.jpg" alt="Item 3">
    <h4>Grid Item 3</h4>
  </div>
</div>

<!-- Touch-Optimized Buttons -->
<div class="button-group">
  <button class="button primary">Primary Action</button>
  <button class="button secondary">Secondary Action</button>
</div>`

const examples = {
  'responsive-card': {
    title: 'Responsive Kart',
    items: [
      {
        title: 'Modern Web Tasarımı',
        description: 'Responsive ve modern web tasarımı teknikleri ile projelerinizi geliştirin.',
        image: 'https://picsum.photos/400/300?random=1',
        tags: ['Web', 'Tasarım', 'CSS']
      },
      {
        title: 'CSS Yenilikleri',
        description: 'En yeni CSS özelliklerini keşfedin ve projelerinizde kullanın.',
        image: 'https://picsum.photos/400/300?random=2',
        tags: ['CSS', 'Frontend', 'Web']
      }
    ]
  },
  'responsive-grid': {
    title: 'Responsive Grid',
    items: [
      {
        title: 'Grid Öğesi 1',
        image: 'https://picsum.photos/300/200?random=3',
        category: 'Kategori 1'
      },
      {
        title: 'Grid Öğesi 2',
        image: 'https://picsum.photos/300/200?random=4',
        category: 'Kategori 2'
      },
      {
        title: 'Grid Öğesi 3',
        image: 'https://picsum.photos/300/200?random=5',
        category: 'Kategori 3'
      },
      {
        title: 'Grid Öğesi 4',
        image: 'https://picsum.photos/300/200?random=6',
        category: 'Kategori 4'
      }
    ]
  },
  'touch-buttons': {
    title: 'Dokunmatik Butonlar',
    items: [
      { type: 'primary', label: 'Ana Eylem' },
      { type: 'secondary', label: 'İkincil Eylem' },
      { type: 'success', label: 'Başarılı' },
      { type: 'danger', label: 'Tehlike' }
    ]
  }
}
</script>

<template>
  <div class="feature-section">
    <h2>Custom Media Queries</h2>
    <p class="description">
      Custom Media Queries, medya sorgularını yeniden kullanılabilir değişkenler olarak tanımlamanıza olanak tanır.
      Bu sayede, responsive tasarımlarınızı daha organize ve bakımı kolay hale getirebilirsiniz.
      Aşağıdaki örneklerde, farklı ekran boyutları, tema tercihleri ve cihaz özellikleri için özelleştirilmiş
      medya sorgularının nasıl kullanıldığını görebilirsiniz.
    </p>

    <div class="example">
      <h3>HTML Yapısı</h3>
      <pre><code>{{ htmlCode }}</code></pre>

      <h3>CSS (Eski Yöntem)</h3>
      <pre><code>{{ oldCode }}</code></pre>

      <h3>CSS (Custom Media Queries)</h3>
      <pre><code>{{ newCode }}</code></pre>

      <div class="demo">
        <h3>Canlı Demo</h3>
        
        <!-- Kontroller -->
        <div class="controls">
          <div class="width-control">
            <label>Konteyner Genişliği:</label>
            <input 
              type="range" 
              v-model="containerWidth" 
              min="320" 
              max="1200" 
              step="10"
            >
            <span>{{ containerWidth }}px</span>
          </div>
          <div class="theme-control">
            <label>Tema:</label>
            <select v-model="selectedTheme">
              <option value="light">Açık Tema</option>
              <option value="dark">Koyu Tema</option>
            </select>
          </div>
          <div class="example-control">
            <label>Örnek:</label>
            <select v-model="selectedExample">
              <option value="responsive-card">Responsive Kart</option>
              <option value="responsive-grid">Responsive Grid</option>
              <option value="touch-buttons">Dokunmatik Butonlar</option>
            </select>
          </div>
        </div>

        <!-- Ana Demo Alanı -->
        <div 
          class="demo-container"
          :style="{ maxWidth: containerWidth + 'px' }"
          :class="[`theme-${selectedTheme}`]"
        >
          <!-- Responsive Kart -->
          <template v-if="selectedExample === 'responsive-card'">
            <div 
              v-for="item in examples['responsive-card'].items"
              :key="item.title"
              class="card"
            >
              <div class="card-image">
                <img :src="item.image" :alt="item.title">
              </div>
              <div class="card-content">
                <h3>{{ item.title }}</h3>
                <p>{{ item.description }}</p>
                <div class="card-tags">
                  <span 
                    v-for="tag in item.tags"
                    :key="tag"
                    class="tag"
                  >
                    {{ tag }}
                  </span>
                </div>
                <button class="button">Detaylar</button>
              </div>
            </div>
          </template>

          <!-- Responsive Grid -->
          <template v-if="selectedExample === 'responsive-grid'">
            <div class="grid">
              <div 
                v-for="item in examples['responsive-grid'].items"
                :key="item.title"
                class="grid-item"
              >
                <img :src="item.image" :alt="item.title">
                <h4>{{ item.title }}</h4>
                <span class="category">{{ item.category }}</span>
              </div>
            </div>
          </template>

          <!-- Dokunmatik Butonlar -->
          <template v-if="selectedExample === 'touch-buttons'">
            <div class="button-group">
              <button 
                v-for="item in examples['touch-buttons'].items"
                :key="item.type"
                :class="['button', item.type]"
              >
                {{ item.label }}
              </button>
            </div>
          </template>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
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

/* Demo Styles */
.demo {
  margin-top: 2rem;
}

.controls {
  margin-bottom: 2rem;
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 8px;
  display: flex;
  gap: 2rem;
  align-items: center;
}

.width-control, .theme-control, .example-control {
  display: flex;
  align-items: center;
  gap: 1rem;
}

input[type="range"] {
  width: 200px;
}

select {
  padding: 0.5rem;
  border-radius: 4px;
  border: 1px solid #d1d5db;
}

.demo-container {
  margin: 0 auto;
  transition: max-width 0.3s ease;
  width: 100%;
}

/* Responsive Card Styles */
.card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  overflow: hidden;
  margin-bottom: 1rem;
  display: flex;
  transition: transform 0.3s ease;
}

@media (max-width: 768px) {
  .card {
    flex-direction: column;
  }
}

.card-image {
  flex: 0 0 40%;
}

.card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-content {
  flex: 1;
  padding: 1.5rem;
}

.card-content h3 {
  margin: 0 0 1rem 0;
  font-size: 1.5rem;
  color: #1a1a1a;
}

.card-content p {
  margin: 0 0 1rem 0;
  color: #4b5563;
  line-height: 1.6;
}

.card-tags {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.tag {
  background: #e5e7eb;
  color: #4b5563;
  padding: 0.25rem 0.75rem;
  border-radius: 9999px;
  font-size: 0.875rem;
}

/* Responsive Grid Styles */
.grid {
  display: grid;
  gap: 1.5rem;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}

.grid-item {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.grid-item img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.grid-item h4 {
  margin: 1rem;
  color: #1a1a1a;
}

.category {
  display: inline-block;
  margin: 0 1rem 1rem;
  padding: 0.25rem 0.75rem;
  background: #e5e7eb;
  color: #4b5563;
  border-radius: 9999px;
  font-size: 0.875rem;
}

/* Button Styles */
.button-group {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.button {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.3s ease;
}

@media (hover: none) and (pointer: coarse) {
  .button {
    padding: 1rem 2rem;
    font-size: 1.1rem;
  }
}

.button.primary {
  background: #3b82f6;
  color: white;
}

.button.primary:hover {
  background: #2563eb;
}

.button.secondary {
  background: #6b7280;
  color: white;
}

.button.secondary:hover {
  background: #4b5563;
}

.button.success {
  background: #10b981;
  color: white;
}

.button.success:hover {
  background: #059669;
}

.button.danger {
  background: #ef4444;
  color: white;
}

.button.danger:hover {
  background: #dc2626;
}

/* Theme Styles */
.theme-light {
  background: white;
  color: #1a1a1a;
}

.theme-dark {
  background: #1a1a1a;
  color: white;
}

.theme-dark .card {
  background: #2a2a2a;
}

.theme-dark .card-content h3 {
  color: white;
}

.theme-dark .card-content p {
  color: #d1d5db;
}

.theme-dark .tag {
  background: #374151;
  color: #d1d5db;
}

.theme-dark .grid-item {
  background: #2a2a2a;
}

.theme-dark .grid-item h4 {
  color: white;
}

.theme-dark .category {
  background: #374151;
  color: #d1d5db;
}

/* Responsive Adjustments */
@media (max-width: 640px) {
  .controls {
    flex-direction: column;
    align-items: stretch;
  }

  .button-group {
    flex-direction: column;
  }

  .button {
    width: 100%;
  }
}
</style> 