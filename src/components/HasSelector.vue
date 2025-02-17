<script setup>
import { ref } from 'vue'

const selectedExample = ref('form')
const showDescription = ref(true)

const oldCode = `
/* Eski yöntem - JavaScript ile kontrol gerekir */

// CSS
.form-group.has-error {
  border-color: red;
}

.card.has-image {
  grid-column: span 2;
}

.menu-item.has-submenu {
  padding-right: 2rem;
}

// JavaScript
document.querySelectorAll('.form-group input').forEach(input => {
  input.addEventListener('input', function() {
    const formGroup = this.closest('.form-group');
    if (this.validity.valid) {
      formGroup.classList.remove('has-error');
    } else {
      formGroup.classList.add('has-error');
    }
  });
});

document.querySelectorAll('.card').forEach(card => {
  if (card.querySelector('img')) {
    card.classList.add('has-image');
  }
});

document.querySelectorAll('.menu-item').forEach(item => {
  if (item.querySelector('.submenu')) {
    item.classList.add('has-submenu');
  }
});`

const newCode = `
/* :has seçicisi ile JavaScript'e gerek kalmadan ebeveyn element seçimi */

/* Form validasyonu için */
.form-group:has(input:invalid) {
  border-color: red;
}

/* Resimli kartlar için */
.card:has(img) {
  grid-column: span 2;
}

/* Alt menülü öğeler için */
.menu-item:has(> .submenu) {
  padding-right: 2rem;
}

/* Diğer kullanım örnekleri */
/* Başlık ve paragraf içeren makaleler */
article:has(h2 + p) {
  margin-bottom: 2rem;
}

/* Geniş resim içeren galeri */
.gallery:has(img[data-type="wide"]) {
  grid-template-columns: 1fr;
}

/* Boş içerik kontrolü */
.content:not(:has(p)) {
  display: none;
}`

const htmlCode = `
<!-- Form Validasyon Örneği -->
<div class="form-group" :has(input:invalid)>
  <label>E-posta</label>
  <input type="email" required>
  <span class="error">Geçerli bir e-posta adresi giriniz</span>
</div>

<!-- Kart Grid Örneği -->
<div class="card" :has(img)>
  <img src="image.jpg" alt="Kart Görseli">
  <div class="content">...</div>
</div>

<!-- Menü Örneği -->
<nav class="menu">
  <div class="menu-item" :has(> .submenu)>
    Dropdown
    <div class="submenu">...</div>
  </div>
</nav>`

const examples = {
  form: {
    title: 'Form Validasyonu',
    items: [
      {
        label: 'Ad Soyad',
        type: 'text',
        required: true,
        pattern: '[A-Za-z ]{3,}',
        error: 'En az 3 karakter giriniz'
      },
      {
        label: 'E-posta',
        type: 'email',
        required: true,
        error: 'Geçerli bir e-posta adresi giriniz'
      },
      {
        label: 'Telefon',
        type: 'tel',
        pattern: '[0-9]{10}',
        error: '10 haneli telefon numarası giriniz'
      }
    ]
  },
  cards: {
    title: 'Kart Grid',
    items: [
      {
        title: 'Resimli Kart',
        content: 'Bu kart bir resim içerdiği için 2 kolon kaplar',
        hasImage: true,
        image: 'https://picsum.photos/400/200?random=1'
      },
      {
        title: 'Resimsiz Kart',
        content: 'Bu kart resim içermediği için 1 kolon kaplar',
        hasImage: false
      },
      {
        title: 'Geniş Kart',
        content: 'Bu kart da resimli olduğu için 2 kolon kaplar',
        hasImage: true,
        image: 'https://picsum.photos/400/200?random=2'
      }
    ]
  },
  menu: {
    title: 'Dinamik Menü',
    items: [
      {
        label: 'Ana Sayfa',
        hasSubmenu: false
      },
      {
        label: 'Ürünler',
        hasSubmenu: true,
        submenu: ['Elektronik', 'Giyim', 'Kitap']
      },
      {
        label: 'Hakkımızda',
        hasSubmenu: false
      },
      {
        label: 'Hizmetler',
        hasSubmenu: true,
        submenu: ['Danışmanlık', 'Eğitim', 'Destek']
      }
    ]
  }
}
</script>

<template>
  <div class="feature-section">
    <h2>:has Seçicisi</h2>
    <p class="description">
      :has seçicisi, içeriğine göre ebeveyn elementleri seçmenize olanak tanır.
      Bu özellik sayesinde, JavaScript kullanmadan içeriğe bağlı stil değişiklikleri yapabilirsiniz.
      Aşağıdaki örneklerde form validasyonu, dinamik grid ve menü yapıları için :has kullanımını görebilirsiniz.
    </p>

    <div class="example">
      <h3>HTML Yapısı</h3>
      <pre><code>{{ htmlCode }}</code></pre>

      <h3>CSS (Eski Yöntem)</h3>
      <pre><code>{{ oldCode }}</code></pre>

      <h3>CSS (:has ile)</h3>
      <pre><code>{{ newCode }}</code></pre>

      <div class="demo">
        <h3>Canlı Demo</h3>
        
        <!-- Örnek Seçici -->
        <div class="example-selector">
          <button 
            v-for="(example, key) in examples"
            :key="key"
            :class="['example-button', { active: selectedExample === key }]"
            @click="selectedExample = key"
          >
            {{ example.title }}
          </button>
        </div>

        <!-- Form Validasyon Demo -->
        <div v-if="selectedExample === 'form'" class="form-demo">
          <form @submit.prevent>
            <div 
              v-for="item in examples.form.items"
              :key="item.label"
              class="form-group"
            >
              <label>{{ item.label }}</label>
              <input 
                :type="item.type"
                :required="item.required"
                :pattern="item.pattern"
              >
              <span class="error">{{ item.error }}</span>
            </div>
            <button type="submit">Gönder</button>
          </form>
        </div>

        <!-- Kart Grid Demo -->
        <div v-if="selectedExample === 'cards'" class="cards-demo">
          <div class="card-grid">
            <div 
              v-for="card in examples.cards.items"
              :key="card.title"
              class="card"
              :class="{ 'has-image': card.hasImage }"
            >
              <img 
                v-if="card.hasImage"
                :src="card.image" 
                :alt="card.title"
              >
              <div class="card-content">
                <h4>{{ card.title }}</h4>
                <p>{{ card.content }}</p>
              </div>
            </div>
          </div>
        </div>

        <!-- Menü Demo -->
        <div v-if="selectedExample === 'menu'" class="menu-demo">
          <nav class="menu">
            <div 
              v-for="item in examples.menu.items"
              :key="item.label"
              class="menu-item"
              :class="{ 'has-submenu': item.hasSubmenu }"
            >
              {{ item.label }}
              <div v-if="item.hasSubmenu" class="submenu">
                <a 
                  v-for="subItem in item.submenu"
                  :key="subItem"
                  href="#"
                >
                  {{ subItem }}
                </a>
              </div>
            </div>
          </nav>
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

.demo {
  margin-top: 2rem;
}

/* Örnek Seçici */
.example-selector {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
}

.example-button {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 4px;
  background: #f1f5f9;
  color: #475569;
  cursor: pointer;
  transition: all 0.3s ease;
}

.example-button.active {
  background: #3b82f6;
  color: white;
}

/* Form Demo Stilleri */
.form-demo {
  max-width: 500px;
  margin: 0 auto;
}

.form-group {
  margin-bottom: 1.5rem;
  position: relative;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  color: #1f2937;
}

.form-group input {
  width: 100%;
  padding: 0.75rem;
  border: 2px solid #e5e7eb;
  border-radius: 4px;
  transition: all 0.3s ease;
}

.form-group:has(input:invalid) {
  border-color: #ef4444;
}

.form-group:has(input:invalid) .error {
  display: block;
}

.error {
  display: none;
  position: absolute;
  bottom: -20px;
  left: 0;
  color: #ef4444;
  font-size: 0.875rem;
}

/* Kart Grid Stilleri */
.card-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
}

.card {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.card:has(img) {
  grid-column: span 2;
}

.card img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.card-content {
  padding: 1.5rem;
}

/* Menü Stilleri */
.menu {
  background: white;
  border-radius: 8px;
  padding: 0.5rem;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.menu-item {
  position: relative;
  padding: 0.75rem 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.menu-item:has(> .submenu) {
  padding-right: 2.5rem;
}

.menu-item:has(> .submenu)::after {
  content: '▼';
  position: absolute;
  right: 1rem;
  top: 50%;
  transform: translateY(-50%);
  font-size: 0.75rem;
  color: #6b7280;
}

.submenu {
  position: absolute;
  top: 100%;
  left: 0;
  background: white;
  border-radius: 4px;
  padding: 0.5rem;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  min-width: 200px;
  display: none;
}

.menu-item:hover .submenu {
  display: block;
}

.submenu a {
  display: block;
  padding: 0.5rem 1rem;
  color: #4b5563;
  text-decoration: none;
  transition: all 0.3s ease;
}

.submenu a:hover {
  background: #f3f4f6;
}

/* Responsive Ayarlamalar */
@media (max-width: 768px) {
  .example-selector {
    flex-direction: column;
  }

  .card-grid {
    grid-template-columns: 1fr;
  }

  .card:has(img) {
    grid-column: auto;
  }

  .menu-item:has(> .submenu) {
    padding-right: 1rem;
  }

  .submenu {
    position: static;
    box-shadow: none;
    padding-left: 1rem;
  }
}
</style> 