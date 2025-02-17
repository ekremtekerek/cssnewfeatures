<script setup>
import { ref } from 'vue'

const selectedExample = ref('navigation')
const showNewSyntax = ref(true)

const oldCode = `
/* Eski yöntem - SCSS/SASS benzeri iç içe yazım */
.nav {
  background: #1a1a1a;
  padding: 1rem;

  & .nav-list {
    display: flex;
    gap: 1rem;

    & .nav-item {
      position: relative;

      & .nav-link {
        color: white;
        text-decoration: none;

        &:hover {
          color: #3b82f6;
        }
      }

      & .dropdown {
        position: absolute;
        display: none;

        & .dropdown-item {
          padding: 0.5rem 1rem;
        }
      }

      &:hover .dropdown {
        display: block;
      }
    }
  }
}`

const newCode = `
/* Yeni CSS Nesting sözdizimi */
.nav {
  background: #1a1a1a;
  padding: 1rem;

  .nav-list {
    display: flex;
    gap: 1rem;

    .nav-item {
      position: relative;

      .nav-link {
        color: white;
        text-decoration: none;

        :hover {
          color: #3b82f6;
        }
      }

      .dropdown {
        position: absolute;
        display: none;

        .dropdown-item {
          padding: 0.5rem 1rem;
        }
      }

      :hover .dropdown {
        display: block;
      }
    }
  }
}

/* Kart Örneği */
.card {
  background: white;
  border-radius: 8px;
  overflow: hidden;

  .card-header {
    padding: 1rem;
    background: #f8f9fa;

    .card-title {
      font-size: 1.25rem;
      color: #1a1a1a;

      :hover {
        color: #3b82f6;
      }
    }
  }

  .card-body {
    padding: 1rem;

    .card-text {
      color: #4b5563;
      line-height: 1.6;
    }
  }

  .card-footer {
    padding: 1rem;
    background: #f8f9fa;
    border-top: 1px solid #e5e7eb;

    .card-actions {
      display: flex;
      gap: 1rem;

      .button {
        padding: 0.5rem 1rem;
        border-radius: 4px;
        
        &.primary {
          background: #3b82f6;
          color: white;
        }

        &.secondary {
          background: #6b7280;
          color: white;
        }
      }
    }
  }
}`

const htmlCode = `
<!-- Navigasyon Örneği -->
<nav class="nav">
  <ul class="nav-list">
    <li class="nav-item">
      <a href="#" class="nav-link">Ana Sayfa</a>
    </li>
    <li class="nav-item">
      <a href="#" class="nav-link">Ürünler</a>
      <div class="dropdown">
        <a href="#" class="dropdown-item">Elektronik</a>
        <a href="#" class="dropdown-item">Giyim</a>
        <a href="#" class="dropdown-item">Kitap</a>
      </div>
    </li>
    <li class="nav-item">
      <a href="#" class="nav-link">Hakkımızda</a>
    </li>
  </ul>
</nav>

<!-- Kart Örneği -->
<div class="card">
  <div class="card-header">
    <h3 class="card-title">Kart Başlığı</h3>
  </div>
  <div class="card-body">
    <p class="card-text">Kart içeriği buraya gelecek...</p>
  </div>
  <div class="card-footer">
    <div class="card-actions">
      <button class="button primary">Kaydet</button>
      <button class="button secondary">İptal</button>
    </div>
  </div>
</div>`

const examples = {
  navigation: {
    title: 'Navigasyon Menüsü',
    items: [
      { title: 'Ana Sayfa', link: '#' },
      { 
        title: 'Ürünler', 
        link: '#',
        dropdown: [
          { title: 'Elektronik', link: '#' },
          { title: 'Giyim', link: '#' },
          { title: 'Kitap', link: '#' }
        ]
      },
      { title: 'Hakkımızda', link: '#' },
      { title: 'İletişim', link: '#' }
    ]
  },
  card: {
    title: 'Kart Bileşeni',
    items: [
      {
        title: 'Vue.js ile CSS Nesting',
        content: 'CSS Nesting özelliği ile daha düzenli ve okunabilir stil kodları yazabilirsiniz.',
        actions: [
          { type: 'primary', label: 'Detaylar' },
          { type: 'secondary', label: 'Kapat' }
        ]
      },
      {
        title: 'Modern CSS Özellikleri',
        content: 'Yeni CSS özellikleri ile daha güçlü ve esnek tasarımlar oluşturabilirsiniz.',
        actions: [
          { type: 'primary', label: 'İncele' },
          { type: 'secondary', label: 'Geri' }
        ]
      }
    ]
  }
}
</script>

<template>
  <div class="feature-section">
    <h2>CSS Nesting (İç İçe Yerleştirme)</h2>
    <p class="description">
      CSS Nesting, stil kurallarını iç içe yazmanıza olanak tanır. Bu sayede, SCSS/SASS gibi
      ön işlemcilere ihtiyaç duymadan, daha organize ve bakımı kolay CSS kodları yazabilirsiniz.
      Aşağıdaki örneklerde, eski ve yeni sözdizimini karşılaştırabilir ve canlı örnekleri inceleyebilirsiniz.
    </p>

    <div class="example">
      <h3>HTML Yapısı</h3>
      <pre><code>{{ htmlCode }}</code></pre>

      <h3>CSS (Eski Yöntem - SCSS/SASS)</h3>
      <pre><code>{{ oldCode }}</code></pre>

      <h3>CSS (Yeni Nesting Sözdizimi)</h3>
      <pre><code>{{ newCode }}</code></pre>

      <div class="demo">
        <h3>Canlı Demo</h3>
        
        <!-- Kontroller -->
        <div class="controls">
          <div class="syntax-control">
            <label>
              <input 
                type="checkbox" 
                v-model="showNewSyntax"
              > Yeni Sözdizimini Kullan
            </label>
          </div>
          <div class="example-control">
            <label>Örnek:</label>
            <select v-model="selectedExample">
              <option value="navigation">Navigasyon Menüsü</option>
              <option value="card">Kart Bileşeni</option>
            </select>
          </div>
        </div>

        <!-- Ana Demo Alanı -->
        <div class="demo-container">
          <!-- Navigasyon Örneği -->
          <template v-if="selectedExample === 'navigation'">
            <nav class="nav" :class="{ 'use-new-syntax': showNewSyntax }">
              <ul class="nav-list">
                <li 
                  v-for="item in examples.navigation.items"
                  :key="item.title"
                  class="nav-item"
                >
                  <a :href="item.link" class="nav-link">{{ item.title }}</a>
                  <div v-if="item.dropdown" class="dropdown">
                    <a 
                      v-for="dropItem in item.dropdown"
                      :key="dropItem.title"
                      :href="dropItem.link"
                      class="dropdown-item"
                    >
                      {{ dropItem.title }}
                    </a>
                  </div>
                </li>
              </ul>
            </nav>
          </template>

          <!-- Kart Örneği -->
          <template v-if="selectedExample === 'card'">
            <div 
              v-for="item in examples.card.items"
              :key="item.title"
              class="card"
              :class="{ 'use-new-syntax': showNewSyntax }"
            >
              <div class="card-header">
                <h3 class="card-title">{{ item.title }}</h3>
              </div>
              <div class="card-body">
                <p class="card-text">{{ item.content }}</p>
              </div>
              <div class="card-footer">
                <div class="card-actions">
                  <button 
                    v-for="action in item.actions"
                    :key="action.label"
                    :class="['button', action.type]"
                  >
                    {{ action.label }}
                  </button>
                </div>
              </div>
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

.syntax-control, .example-control {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.syntax-control input[type="checkbox"] {
  width: 1.2rem;
  height: 1.2rem;
  cursor: pointer;
}

.example-control select {
  padding: 0.5rem;
  border-radius: 4px;
  border: 1px solid #d1d5db;
}

.demo-container {
  margin-top: 2rem;
}

/* Navigasyon Stilleri - Eski Syntax */
.nav {
  background: #1a1a1a;
  padding: 1rem;
  border-radius: 8px;
}

.nav .nav-list {
  display: flex;
  gap: 1rem;
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav .nav-item {
  position: relative;
}

.nav .nav-link {
  color: white;
  text-decoration: none;
  padding: 0.5rem 1rem;
  display: block;
}

.nav .nav-link:hover {
  color: #3b82f6;
}

.nav .dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  background: white;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  display: none;
  min-width: 200px;
}

.nav .nav-item:hover .dropdown {
  display: block;
}

.nav .dropdown-item {
  display: block;
  padding: 0.5rem 1rem;
  color: #1a1a1a;
  text-decoration: none;
}

.nav .dropdown-item:hover {
  background: #f8f9fa;
}

/* Navigasyon Stilleri - Yeni Syntax */
.nav.use-new-syntax {
  background: #1a1a1a;
  padding: 1rem;
  border-radius: 8px;

  .nav-list {
    display: flex;
    gap: 1rem;
    list-style: none;
    margin: 0;
    padding: 0;

    .nav-item {
      position: relative;

      .nav-link {
        color: white;
        text-decoration: none;
        padding: 0.5rem 1rem;
        display: block;

        :hover {
          color: #3b82f6;
        }
      }

      .dropdown {
        position: absolute;
        top: 100%;
        left: 0;
        background: white;
        border-radius: 4px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        display: none;
        min-width: 200px;

        .dropdown-item {
          display: block;
          padding: 0.5rem 1rem;
          color: #1a1a1a;
          text-decoration: none;

          :hover {
            background: #f8f9fa;
          }
        }
      }

      :hover .dropdown {
        display: block;
      }
    }
  }
}

/* Kart Stilleri - Eski Syntax */
.card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  margin-bottom: 1rem;
}

.card .card-header {
  padding: 1rem;
  background: #f8f9fa;
  border-bottom: 1px solid #e5e7eb;
}

.card .card-title {
  margin: 0;
  font-size: 1.25rem;
  color: #1a1a1a;
}

.card .card-body {
  padding: 1rem;
}

.card .card-text {
  margin: 0;
  color: #4b5563;
  line-height: 1.6;
}

.card .card-footer {
  padding: 1rem;
  background: #f8f9fa;
  border-top: 1px solid #e5e7eb;
}

.card .card-actions {
  display: flex;
  gap: 1rem;
}

.card .button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.card .button.primary {
  background: #3b82f6;
  color: white;
}

.card .button.primary:hover {
  background: #2563eb;
}

.card .button.secondary {
  background: #6b7280;
  color: white;
}

.card .button.secondary:hover {
  background: #4b5563;
}

/* Kart Stilleri - Yeni Syntax */
.card.use-new-syntax {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  margin-bottom: 1rem;

  .card-header {
    padding: 1rem;
    background: #f8f9fa;
    border-bottom: 1px solid #e5e7eb;

    .card-title {
      margin: 0;
      font-size: 1.25rem;
      color: #1a1a1a;
    }
  }

  .card-body {
    padding: 1rem;

    .card-text {
      margin: 0;
      color: #4b5563;
      line-height: 1.6;
    }
  }

  .card-footer {
    padding: 1rem;
    background: #f8f9fa;
    border-top: 1px solid #e5e7eb;

    .card-actions {
      display: flex;
      gap: 1rem;

      .button {
        padding: 0.5rem 1rem;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        transition: background 0.3s ease;

        &.primary {
          background: #3b82f6;
          color: white;

          :hover {
            background: #2563eb;
          }
        }

        &.secondary {
          background: #6b7280;
          color: white;

          :hover {
            background: #4b5563;
          }
        }
      }
    }
  }
}
</style> 