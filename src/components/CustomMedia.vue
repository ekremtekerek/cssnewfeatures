<script setup>
import { ref, watch } from 'vue'

const containerWidth = ref(800)
const selectedBreakpoint = ref('desktop')
const gridColumns = ref(3)

// Container genişliği değiştiğinde grid kolonlarını güncelle
watch(containerWidth, (newWidth) => {
  if (newWidth <= 640) {
    gridColumns.value = 1
  } else if (newWidth <= 1024) {
    gridColumns.value = 2
  } else {
    gridColumns.value = 3
  }
})

const breakpoints = {
  mobile: {
    width: 640,
    label: 'Mobil'
  },
  tablet: {
    width: 1024,
    label: 'Tablet'
  },
  desktop: {
    width: 1200,
    label: 'Masaüstü'
  }
}

const oldCode = `
/* Eski yöntem - Her seferinde tekrar eden medya sorguları */
@media (max-width: 640px) {
  .blog-card { width: 100%; }
}
@media (min-width: 641px) and (max-width: 1024px) {
  .blog-card { width: 50%; }
}
@media (min-width: 1025px) {
  .blog-card { width: 33.333%; }
}

@media (max-width: 640px) {
  .header { padding: 1rem; }
}
@media (min-width: 641px) and (max-width: 1024px) {
  .header { padding: 2rem; }
}
@media (min-width: 1025px) {
  .header { padding: 3rem; }
}`

const newCode = `
/* @custom-media ile yeniden kullanılabilir medya sorguları */
@custom-media --mobile (max-width: 640px);
@custom-media --tablet (min-width: 641px) and (max-width: 1024px);
@custom-media --desktop (min-width: 1025px);
@custom-media --dark-theme (prefers-color-scheme: dark);
@custom-media --light-theme (prefers-color-scheme: light);
@custom-media --touch-device (hover: none) and (pointer: coarse);
@custom-media --retina-screen (-webkit-min-device-pixel-ratio: 2);

/* Blog kartları için kullanım */
.blog-card {
  @media (--mobile) {
    width: 100%;
    margin-bottom: 1rem;
  }
  @media (--tablet) {
    width: 50%;
    padding: 0.5rem;
  }
  @media (--desktop) {
    width: 33.333%;
    padding: 1rem;
  }
  @media (--dark-theme) {
    background: #2a2a2a;
    color: white;
  }
  @media (--touch-device) {
    padding: 1.5rem;
    font-size: 1.1rem;
  }
  @media (--retina-screen) {
    border: 0.5px solid #eee;
  }
}`

const htmlCode = `
<div class="blog-container">
  <article class="blog-card">
    <img src="blog-image.jpg" alt="Blog">
    <div class="content">
      <h2>Blog Başlığı</h2>
      <p>Blog içeriği burada yer alır...</p>
      <div class="meta">
        <span class="date">15 Mart 2024</span>
        <span class="category">CSS</span>
      </div>
    </div>
  </article>
</div>`

const examples = {
  blog: {
    title: 'Blog Kartları',
    items: [
      {
        title: 'Modern CSS Özellikleri',
        content: 'CSS dünyasındaki en yeni gelişmeleri ve özellikleri keşfedin.',
        image: 'https://picsum.photos/400/300?random=1',
        date: '15 Mart 2024',
        category: 'CSS'
      },
      {
        title: 'Responsive Tasarım İpuçları',
        content: 'Mobil uyumlu web siteleri için en iyi pratikler ve teknikler.',
        image: 'https://picsum.photos/400/300?random=2',
        date: '14 Mart 2024',
        category: 'Web Tasarım'
      },
      {
        title: '@custom-media Kullanımı',
        content: 'Medya sorgularını daha etkili ve yeniden kullanılabilir hale getirin.',
        image: 'https://picsum.photos/400/300?random=3',
        date: '13 Mart 2024',
        category: 'CSS'
      }
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
    </p>

    <div class="example">
      <h3>HTML Yapısı</h3>
      <pre><code>{{ htmlCode }}</code></pre>

      <h3>CSS (Eski Yöntem)</h3>
      <pre><code>{{ oldCode }}</code></pre>

      <h3>CSS (@custom-media ile)</h3>
      <pre><code>{{ newCode }}</code></pre>

      <div class="demo">
        <h3>Canlı Demo</h3>
        
        <!-- Kontroller -->
        <div class="controls">
          <div class="width-control">
            <label>Container Genişliği:</label>
            <input 
              type="range" 
              v-model="containerWidth" 
              :min="320" 
              :max="1200" 
              step="10"
            >
            <span>{{ containerWidth }}px</span>
          </div>
          <div class="breakpoint-control">
            <label>Ekran Boyutu:</label>
            <div class="breakpoint-buttons">
              <button 
                v-for="(bp, key) in breakpoints"
                :key="key"
                :class="['breakpoint-button', { active: selectedBreakpoint === key }]"
                @click="selectedBreakpoint = key; containerWidth = bp.width"
              >
                {{ bp.label }}
                <span class="breakpoint-width">{{ bp.width }}px</span>
              </button>
            </div>
          </div>
        </div>

        <!-- Ana Demo Alanı -->
        <div 
          class="demo-container"
          :style="{
            maxWidth: containerWidth + 'px',
            width: '100%'
          }"
        >
          <div 
            class="blog-grid"
            :style="{
              gridTemplateColumns: `repeat(${gridColumns}, 1fr)`
            }"
          >
            <article 
              v-for="item in examples.blog.items"
              :key="item.title"
              class="blog-card"
            >
              <div class="card-image">
                <img :src="item.image" :alt="item.title">
              </div>
              <div class="card-content">
                <h3>{{ item.title }}</h3>
                <p>{{ item.content }}</p>
                <div class="card-meta">
                  <span class="date">{{ item.date }}</span>
                  <span class="category">{{ item.category }}</span>
                </div>
              </div>
            </article>
          </div>

          <!-- Breakpoint Göstergesi -->
          <div class="breakpoint-indicator">
            <div class="current-width">
              Geçerli Genişlik: {{ containerWidth }}px
            </div>
            <div class="active-breakpoint">
              Aktif Breakpoint: 
              <span v-if="containerWidth <= 640">Mobile (≤ 640px)</span>
              <span v-else-if="containerWidth <= 1024">Tablet (641px - 1024px)</span>
              <span v-else>Desktop (≥ 1025px)</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
/* Custom Media Queries */
@custom-media --mobile (max-width: 640px);
@custom-media --tablet (min-width: 641px) and (max-width: 1024px);
@custom-media --desktop (min-width: 1025px);
@custom-media --dark-theme (prefers-color-scheme: dark);
@custom-media --light-theme (prefers-color-scheme: light);
@custom-media --touch-device (hover: none) and (pointer: coarse);
@custom-media --retina-screen (-webkit-min-device-pixel-ratio: 2);

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
  padding: 1.5rem;
  background: #f8f9fa;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.width-control {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.width-control input[type="range"] {
  flex: 1;
  height: 6px;
  -webkit-appearance: none;
  background: #e5e7eb;
  border-radius: 3px;
  outline: none;
}

.width-control input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 20px;
  height: 20px;
  background: #3b82f6;
  border-radius: 50%;
  cursor: pointer;
  transition: background 0.3s ease;
}

.width-control input[type="range"]::-webkit-slider-thumb:hover {
  background: #2563eb;
}

.width-control span {
  min-width: 80px;
  text-align: right;
  font-family: monospace;
}

.breakpoint-control {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.breakpoint-buttons {
  display: flex;
  gap: 1rem;
}

.breakpoint-button {
  flex: 1;
  padding: 0.75rem 1rem;
  border: none;
  border-radius: 4px;
  background: white;
  color: #4b5563;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.breakpoint-button.active {
  background: #3b82f6;
  color: white;
}

.breakpoint-width {
  font-size: 0.875rem;
  opacity: 0.8;
}

.demo-container {
  margin: 0 auto;
  transition: max-width 0.3s ease;
  width: 100%;
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.blog-grid {
  display: grid;
  gap: 2rem;
  width: 100%;
  transition: all 0.3s ease;
}

.blog-card {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: transform 0.3s ease;
  display: flex;
  flex-direction: column;
}

.blog-card:hover {
  transform: translateY(-4px);
}

.card-image {
  position: relative;
  padding-top: 56.25%; /* 16:9 aspect ratio */
}

.card-image img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-content {
  padding: 1.5rem;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.card-content h3 {
  margin: 0 0 1rem 0;
  font-size: 1.25rem;
  color: #1a1a1a;
}

.card-content p {
  flex: 1;
  margin: 0 0 1rem 0;
  color: #4b5563;
  line-height: 1.6;
}

.card-meta {
  margin-top: auto;
  padding-top: 1rem;
  border-top: 1px solid #e5e7eb;
  display: flex;
  justify-content: space-between;
  color: #6b7280;
  font-size: 0.875rem;
}

.category {
  background: #e5e7eb;
  padding: 0.25rem 0.75rem;
  border-radius: 9999px;
}

.breakpoint-indicator {
  margin-top: 2rem;
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 4px;
  font-size: 0.875rem;
  color: #4b5563;
  display: flex;
  justify-content: space-between;
}

/* Responsive Adjustments */
@media (max-width: 640px) {
  .controls {
    padding: 1rem;
  }

  .breakpoint-buttons {
    flex-direction: column;
  }

  .breakpoint-indicator {
    flex-direction: column;
    gap: 0.5rem;
    text-align: center;
  }

  .demo-container {
    padding: 1rem;
  }

  .blog-grid {
    gap: 1rem;
  }
  
  .card-content {
    padding: 1rem;
  }
}

@media (max-width: 768px) {
  .blog-grid {
    grid-template-columns: 1fr;
  }

  .demo-container {
    padding: 1rem;
  }

  .card-content {
    padding: 1.25rem;
  }

  .card-content h3 {
    font-size: 1.2rem;
  }

  .card-meta {
    flex-direction: column;
    gap: 0.5rem;
    align-items: flex-start;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  .blog-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1025px) {
  .blog-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* Blog Grid ve Card Styles */
.blog-grid {
  display: grid;
  gap: 2rem;
  width: 100%;
}

@media (--mobile) {
  .blog-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  .demo-container {
    padding: 1rem;
  }

  .blog-card {
    width: 100%;
  }
}

@media (--tablet) {
  .blog-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 1.75rem;
  }

  .blog-card {
    width: 100%;
  }
}

@media (--desktop) {
  .blog-grid {
    grid-template-columns: repeat(3, 1fr);
  }

  .blog-card {
    width: 100%;
  }
}

.blog-card {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: transform 0.3s ease;
  display: flex;
  flex-direction: column;
}

demo-container {
  width: 100%;
  transition: max-width 0.3s ease;
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  overflow: hidden;
}

@media (--dark-theme) {
  .blog-card {
    background: #2a2a2a;
    color: white;
  }
}

@media (--touch-device) {
  .blog-card {
    padding: 1.5rem;
    font-size: 1.1rem;
  }
}

@media (--retina-screen) {
  .blog-card {
    border: 0.5px solid #eee;
  }
}
</style> 