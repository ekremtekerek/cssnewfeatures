<script setup>
import { ref } from 'vue'

const containerWidth = ref(800)
const selectedLayout = ref('grid')

const oldCode = `
/* Eski yöntem - Medya sorguları ile responsive tasarım */
@media (max-width: 768px) {
  .card {
    flex-direction: column;
  }
}

@media (max-width: 480px) {
  .card-title {
    font-size: 1.2rem;
  }
}`

const newCode = `
/* Container Queries ile bileşen bazlı responsive tasarım */
.container {
  container-type: inline-size;
  container-name: card-container;
}

@container card-container (max-width: 400px) {
  .card {
    flex-direction: column;
  }
  
  .card-title {
    font-size: 1.2rem;
  }
}

/* Farklı container'lar için farklı stiller */
@container sidebar (min-width: 200px) {
  .widget {
    grid-template-columns: repeat(2, 1fr);
  }
}`

const products = [
  {
    id: 1,
    title: 'Vue.js T-Shirt',
    price: '29.99',
    image: 'https://picsum.photos/200/200?random=1',
    description: 'Rahat ve şık Vue.js logolu t-shirt'
  },
  {
    id: 2,
    title: 'Vue.js Hoodie',
    price: '49.99',
    image: 'https://picsum.photos/200/200?random=2',
    description: 'Kışlık Vue.js logolu hoodie'
  },
  {
    id: 3,
    title: 'Vue.js Şapka',
    price: '19.99',
    image: 'https://picsum.photos/200/200?random=3',
    description: 'Spor Vue.js logolu şapka'
  }
]

const blogPosts = [
  {
    id: 1,
    title: 'Vue 3 Composition API',
    date: '2024-03-15',
    image: 'https://picsum.photos/300/200?random=4',
    excerpt: 'Vue 3 Composition API ile daha modüler ve yeniden kullanılabilir kod yazımı'
  },
  {
    id: 2,
    title: 'Container Queries Kullanımı',
    date: '2024-03-16',
    image: 'https://picsum.photos/300/200?random=5',
    excerpt: 'Container Queries ile bileşen bazlı responsive tasarım nasıl yapılır?'
  }
]

const widgets = [
  {
    id: 1,
    title: 'Hava Durumu',
    type: 'weather',
    data: { temp: 22, condition: 'Güneşli' }
  },
  {
    id: 2,
    title: 'Son Aktiviteler',
    type: 'activities',
    data: ['Yeni yorum', 'Yeni beğeni', 'Yeni takipçi']
  }
]

const htmlCode = `
<!-- Grid Layout Örneği -->
<div class="product-grid">
  <div class="product-card-container" style="container-type: inline-size;">
    <div class="product-card">
      <img src="product-image.jpg" alt="Ürün">
      <div class="product-info">
        <h4>Ürün Başlığı</h4>
        <p>Ürün açıklaması</p>
        <div class="product-price">
          <span>$29.99</span>
          <button>Sepete Ekle</button>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- Sidebar Layout Örneği -->
<div class="sidebar-layout">
  <main class="main-content">
    <article class="blog-post-container" style="container-type: inline-size;">
      <div class="blog-post">
        <img src="post-image.jpg" alt="Blog">
        <div class="post-content">
          <h4>Blog Başlığı</h4>
          <time>2024-03-15</time>
          <p>Blog içeriği...</p>
        </div>
      </div>
    </article>
  </main>
  <aside class="sidebar" style="container-type: inline-size;">
    <div class="widget">
      <h5>Widget Başlığı</h5>
      <div class="widget-content">...</div>
    </div>
  </aside>
</div>

<!-- Dashboard Layout Örneği -->
<div class="dashboard-layout">
  <div class="dashboard-card-container area-1" style="container-type: inline-size;">
    <div class="dashboard-card">
      <img src="card-image.jpg" alt="Kart">
      <div class="card-content">
        <h4>Kart Başlığı</h4>
        <p>Kart içeriği...</p>
      </div>
    </div>
  </div>
</div>`
</script>

<template>
  <div class="feature-section">
    <h2>Container Queries</h2>
    <p class="description">
      Container Queries, bileşenlerin kendi container'larının boyutuna göre stil almasını sağlar.
      Bu özellik sayesinde, her bileşen kendi container'ının boyutuna göre responsive olabilir.
      Aşağıda üç farklı layout türü için HTML, CSS ve canlı örnekler bulabilirsiniz.
    </p>

    <div class="example">
      <h3>HTML Yapısı</h3>
      <pre><code>{{ htmlCode }}</code></pre>

      <h3>CSS (Eski Yöntem)</h3>
      <pre><code>{{ oldCode }}</code></pre>

      <h3>CSS (Container Queries)</h3>
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
              min="300" 
              max="1200" 
              step="50"
            >
            <span>{{ containerWidth }}px</span>
          </div>
          <div class="layout-control">
            <label>Yerleşim:</label>
            <select v-model="selectedLayout">
              <option value="grid">Grid</option>
              <option value="sidebar">Kenar Çubuğu</option>
              <option value="dashboard">Dashboard</option>
            </select>
          </div>
        </div>

        <!-- Ana Demo Alanı -->
        <div 
          class="demo-container"
          :style="{ maxWidth: containerWidth + 'px' }"
        >
          <!-- Grid Layout -->
          <div v-if="selectedLayout === 'grid'" class="product-grid">
            <div 
              v-for="product in products" 
              :key="product.id"
              class="product-card-container"
              style="container-type: inline-size;"
            >
              <div class="product-card">
                <img :src="product.image" :alt="product.title">
                <div class="product-info">
                  <h4>{{ product.title }}</h4>
                  <p>{{ product.description }}</p>
                  <div class="product-price">
                    <span>${{ product.price }}</span>
                    <button>Sepete Ekle</button>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Sidebar Layout -->
          <div v-if="selectedLayout === 'sidebar'" class="sidebar-layout">
            <main class="main-content">
              <article 
                v-for="post in blogPosts" 
                :key="post.id"
                class="blog-post-container"
                style="container-type: inline-size;"
              >
                <div class="blog-post">
                  <img :src="post.image" :alt="post.title">
                  <div class="post-content">
                    <h4>{{ post.title }}</h4>
                    <time>{{ post.date }}</time>
                    <p>{{ post.excerpt }}</p>
                    <button>Devamını Oku</button>
                  </div>
                </div>
              </article>
            </main>
            <aside class="sidebar" style="container-type: inline-size;">
              <div 
                v-for="widget in widgets"
                :key="widget.id"
                class="widget"
              >
                <h5>{{ widget.title }}</h5>
                <!-- Hava Durumu Widget -->
                <div v-if="widget.type === 'weather'" class="weather-widget">
                  <span class="temp">{{ widget.data.temp }}°C</span>
                  <span class="condition">{{ widget.data.condition }}</span>
                </div>
                <!-- Aktiviteler Widget -->
                <div v-if="widget.type === 'activities'" class="activities-widget">
                  <ul>
                    <li v-for="(activity, index) in widget.data" :key="index">
                      {{ activity }}
                    </li>
                  </ul>
                </div>
              </div>
            </aside>
          </div>

          <!-- Dashboard Layout -->
          <div v-if="selectedLayout === 'dashboard'" class="dashboard-layout">
            <div 
              v-for="(product, index) in products" 
              :key="product.id"
              class="dashboard-card-container"
              style="container-type: inline-size;"
              :class="'area-' + (index + 1)"
            >
              <div class="dashboard-card">
                <img :src="product.image" :alt="product.title">
                <div class="card-content">
                  <h4>{{ product.title }}</h4>
                  <p>{{ product.description }}</p>
                  <div class="card-footer">
                    <span>${{ product.price }}</span>
                    <button>Detaylar</button>
                  </div>
                </div>
              </div>
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

/* Kontroller */
.controls {
  margin-bottom: 2rem;
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 8px;
  display: flex;
  gap: 2rem;
  align-items: center;
}

.width-control, .layout-control {
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

/* Demo Container */
.demo-container {
  margin: 0 auto;
  transition: max-width 0.3s ease;
  width: 100%;
}

/* Product Grid Layout */
.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
}

.product-card {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.product-card img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.product-info {
  padding: 1rem;
}

.product-price {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 1rem;
}

@container (max-width: 400px) {
  .product-card {
    flex-direction: column;
  }

  .product-info {
    padding: 0.5rem;
  }

  .product-price {
    flex-direction: column;
    gap: 0.5rem;
  }
}

/* Sidebar Layout */
.sidebar-layout {
  display: grid;
  grid-template-columns: 1fr 300px;
  gap: 2rem;
}

.main-content {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.blog-post {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.blog-post img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.post-content {
  padding: 1rem;
}

.sidebar {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.widget {
  background: white;
  padding: 1rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

@container (max-width: 300px) {
  .widget {
    padding: 0.5rem;
  }

  .widget h5 {
    font-size: 0.875rem;
  }
}

/* Dashboard Layout */
.dashboard-layout {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(2, 1fr);
  gap: 1rem;
  min-height: 600px;
}

.area-1 {
  grid-area: 1 / 1 / 2 / 3;
}

.area-2 {
  grid-area: 1 / 3 / 2 / 5;
}

.area-3 {
  grid-area: 2 / 1 / 3 / 5;
}

.dashboard-card {
  height: 100%;
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  display: flex;
}

.dashboard-card img {
  width: 40%;
  object-fit: cover;
}

.card-content {
  flex: 1;
  padding: 1rem;
  display: flex;
  flex-direction: column;
}

.card-footer {
  margin-top: auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

@container (max-width: 500px) {
  .dashboard-card {
    flex-direction: column;
  }

  .dashboard-card img {
    width: 100%;
    height: 200px;
  }
}

/* Genel Stiller */
button {
  padding: 0.5rem 1rem;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.3s ease;
}

button:hover {
  background: #2563eb;
}

h4 {
  margin: 0 0 0.5rem 0;
  color: #1f2937;
}

p {
  margin: 0 0 1rem 0;
  color: #4b5563;
}

time {
  color: #6b7280;
  font-size: 0.875rem;
}

.weather-widget {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
}

.temp {
  font-size: 2rem;
  font-weight: bold;
  color: #1f2937;
}

.condition {
  color: #6b7280;
}

.activities-widget ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.activities-widget li {
  padding: 0.5rem 0;
  border-bottom: 1px solid #e5e7eb;
}

.activities-widget li:last-child {
  border-bottom: none;
}
</style> 