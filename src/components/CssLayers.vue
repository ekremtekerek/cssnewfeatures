<script setup>
import { ref } from 'vue'

const selectedTheme = ref('light')
const selectedFramework = ref('bootstrap')

const oldCode = `
/* Eski yöntem - CSS öncelik sorunları */
.button {
  /* Temel stiller */
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
}

/* Framework stilleri - yüksek spesifiklik */
.framework-button.primary {
  background: blue !important;
  color: white !important;
}

/* Tema stilleri - override etmek zor */
.theme-dark .button.primary {
  background: #2a2a2a !important;
  color: #ffffff !important;
}`

const newCode = `
/* CSS Layers ile düzenli stil katmanları */
@layer base, framework, theme, custom;

@layer base {
  .button {
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    transition: all 0.3s ease;
  }
}

@layer framework {
  .button.primary {
    background: #007bff;
    color: white;
  }
  
  .button.secondary {
    background: #6c757d;
    color: white;
  }

  /* Tailwind Stilleri */
  .bg-blue-500 {
    background-color: #3b82f6;
    color: white;
    padding: 0.5rem 1rem;
    border-radius: 0.375rem;
  }

  .bg-blue-500:hover {
    background-color: #1d4ed8;
  }

  .bg-gray-500 {
    background-color: #6b7280;
    color: white;
    padding: 0.5rem 1rem;
    border-radius: 0.375rem;
  }

  .bg-gray-500:hover {
    background-color: #4b5563;
  }

  .bg-green-500 {
    background-color: #22c55e;
    color: white;
    padding: 0.5rem 1rem;
    border-radius: 0.375rem;
  }

  .bg-green-500:hover {
    background-color: #15803d;
  }

  .bg-red-500 {
    background-color: #ef4444;
    color: white;
    padding: 0.5rem 1rem;
    border-radius: 0.375rem;
  }

  .bg-red-500:hover {
    background-color: #b91c1c;
  }

  .text-white {
    color: white;
  }

  .px-4 {
    padding-left: 1rem;
    padding-right: 1rem;
  }

  .py-2 {
    padding-top: 0.5rem;
    padding-bottom: 0.5rem;
  }

  .rounded {
    border-radius: 0.375rem;
  }
}

@layer theme {
  /* Açık tema */
  .theme-light .button.primary {
    background: #0056b3;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }
  
  /* Koyu tema */
  .theme-dark .button.primary {
    background: #2a2a2a;
    color: #ffffff;
  }
}

@layer custom {
  .button.custom {
    background: linear-gradient(45deg, #ff6b6b, #ff8e53);
    color: white;
  }
}
`

const htmlCode = `
<!-- Temel Buton Yapısı -->
<div class="button-container">
  <!-- Framework Butonları -->
  <div class="framework-buttons">
    <button class="button primary">Primary Button</button>
    <button class="button secondary">Secondary Button</button>
  </div>

  <!-- Tema Butonları -->
  <div class="theme-light">
    <button class="button primary">Light Theme Button</button>
  </div>
  <div class="theme-dark">
    <button class="button primary">Dark Theme Button</button>
  </div>

  <!-- Özel Butonlar -->
  <div class="custom-buttons">
    <button class="button custom">Custom Button</button>
  </div>
</div>

<!-- Framework Entegrasyonu -->
<div class="framework-integration">
  <!-- Bootstrap Butonları -->
  <div class="bootstrap-buttons">
    <button class="btn btn-primary">Bootstrap Primary</button>
    <button class="btn btn-secondary">Bootstrap Secondary</button>
  </div>

  <!-- Tailwind Butonları -->
  <div class="tailwind-buttons">
    <button class="bg-blue-500 hover:bg-blue-700">Tailwind Primary</button>
    <button class="bg-gray-500 hover:bg-gray-700">Tailwind Secondary</button>
  </div>
</div>`

const frameworks = {
  bootstrap: {
    name: 'Bootstrap',
    buttons: [
      { type: 'primary', label: 'Primary' },
      { type: 'secondary', label: 'Secondary' },
      { type: 'success', label: 'Success' },
      { type: 'danger', label: 'Danger' }
    ]
  },
  tailwind: {
    name: 'Tailwind',
    buttons: [
      { type: 'blue', label: 'Primary' },
      { type: 'gray', label: 'Secondary' },
      { type: 'green', label: 'Success' },
      { type: 'red', label: 'Danger' }
    ]
  }
}

const customButtons = [
  { type: 'gradient', label: 'Gradient' },
  { type: 'outline', label: 'Outline' },
  { type: 'neon', label: 'Neon' },
  { type: 'glass', label: 'Glass' }
]
</script>

<template>
  <div class="feature-section">
    <h2>CSS Layers (Katmanlar)</h2>
    <p class="description">
      CSS Layers, stil kurallarının önceliğini yönetmek için güçlü bir yöntemdir.
      Katmanlar sayesinde, farklı kaynaklardan gelen stilleri (framework, tema, özel stiller)
      düzenli bir şekilde organize edebilir ve öncelik çakışmalarını engelleyebilirsiniz.
    </p>

    <div class="example">
      <h3>HTML Yapısı</h3>
      <pre><code>{{ htmlCode }}</code></pre>

      <h3>CSS (Eski Yöntem)</h3>
      <pre><code>{{ oldCode }}</code></pre>

      <h3>CSS (Layers ile)</h3>
      <pre><code>{{ newCode }}</code></pre>

      <div class="demo">
        <h3>Canlı Demo</h3>
        
        <!-- Kontroller -->
        <div class="controls">
          <div class="theme-control">
            <label>Tema:</label>
            <select v-model="selectedTheme">
              <option value="light">Açık Tema</option>
              <option value="dark">Koyu Tema</option>
            </select>
          </div>
          <div class="framework-control">
            <label>Framework:</label>
            <select v-model="selectedFramework">
              <option value="bootstrap">Bootstrap</option>
              <option value="tailwind">Tailwind</option>
            </select>
          </div>
        </div>

        <!-- Ana Demo Alanı -->
        <div 
          class="demo-container"
          :class="[`theme-${selectedTheme}`]"
        >
          <!-- Temel Butonlar -->
          <section class="demo-section">
            <h4>Temel Butonlar</h4>
            <div class="button-group">
              <button class="button primary">Primary</button>
              <button class="button secondary">Secondary</button>
            </div>
          </section>

          <!-- Framework Butonları -->
          <section class="demo-section">
            <h4>{{ frameworks[selectedFramework].name }} Butonları</h4>
            <div class="button-group">
              <template v-if="selectedFramework === 'bootstrap'">
                <button 
                  v-for="btn in frameworks.bootstrap.buttons"
                  :key="btn.type"
                  :class="['btn', `btn-${btn.type}`]"
                >
                  {{ btn.label }}
                </button>
              </template>
              <template v-else>
                <button 
                  v-for="btn in frameworks.tailwind.buttons"
                  :key="btn.type"
                  :class="[
                    `bg-${btn.type}-500`,
                    `hover:bg-${btn.type}-700`,
                    'text-white',
                    'px-4',
                    'py-2',
                    'rounded'
                  ]"
                >
                  {{ btn.label }}
                </button>
              </template>
            </div>
          </section>

          <!-- Özel Butonlar -->
          <section class="demo-section">
            <h4>Özel Butonlar</h4>
            <div class="button-group">
              <button 
                v-for="btn in customButtons"
                :key="btn.type"
                :class="['button', `custom-${btn.type}`]"
              >
                {{ btn.label }}
              </button>
            </div>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Base Layer */
@layer base {
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

  .button {
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.3s ease;
  }
}

/* Framework Layer */
@layer framework {
  .button.primary {
    background: #007bff;
    color: white;
  }

  .button.secondary {
    background: #6c757d;
    color: white;
  }

  /* Bootstrap Stilleri */
  .btn {
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  .btn-primary {
    background: #007bff;
    color: white;
  }

  .btn-secondary {
    background: #6c757d;
    color: white;
  }

  .btn-success {
    background: #28a745;
    color: white;
  }

  .btn-danger {
    background: #dc3545;
    color: white;
  }

  /* Tailwind Stilleri */
  .bg-blue-500 {
    background-color: #3b82f6;
    color: white;
    padding: 0.5rem 1rem;
    border-radius: 0.375rem;
  }

  .bg-blue-500:hover {
    background-color: #1d4ed8;
  }

  .bg-gray-500 {
    background-color: #6b7280;
    color: white;
    padding: 0.5rem 1rem;
    border-radius: 0.375rem;
  }

  .bg-gray-500:hover {
    background-color: #4b5563;
  }

  .bg-green-500 {
    background-color: #22c55e;
    color: white;
    padding: 0.5rem 1rem;
    border-radius: 0.375rem;
  }

  .bg-green-500:hover {
    background-color: #15803d;
  }

  .bg-red-500 {
    background-color: #ef4444;
    color: white;
    padding: 0.5rem 1rem;
    border-radius: 0.375rem;
  }

  .bg-red-500:hover {
    background-color: #b91c1c;
  }

  .text-white {
    color: white;
  }

  .px-4 {
    padding-left: 1rem;
    padding-right: 1rem;
  }

  .py-2 {
    padding-top: 0.5rem;
    padding-bottom: 0.5rem;
  }

  .rounded {
    border-radius: 0.375rem;
  }
}

/* Theme Layer */
@layer theme {
  .theme-light {
    background: #ffffff;
    color: #1a1a1a;
  }

  .theme-light .button.primary {
    background: #0056b3;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }

  .theme-dark {
    background: #1a1a1a;
    color: #ffffff;
  }

  .theme-dark .button.primary {
    background: #2a2a2a;
    border: 1px solid #4a4a4a;
  }
}

/* Custom Layer */
@layer custom {
  .button.custom-gradient {
    background: linear-gradient(45deg, #ff6b6b, #ff8e53);
    color: white;
  }

  .button.custom-outline {
    background: transparent;
    border: 2px solid currentColor;
    color: #007bff;
  }

  .button.custom-neon {
    background: #2a2a2a;
    color: #00ff00;
    box-shadow: 0 0 10px #00ff00;
    text-shadow: 0 0 5px #00ff00;
  }

  .button.custom-glass {
    background: rgba(255, 255, 255, 0.2);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.3);
    color: #1a1a1a;
  }
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

.theme-control, .framework-control {
  display: flex;
  align-items: center;
  gap: 1rem;
}

select {
  padding: 0.5rem;
  border-radius: 4px;
  border: 1px solid #d1d5db;
}

.demo-container {
  padding: 2rem;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.demo-section {
  margin-bottom: 2rem;
}

.demo-section h4 {
  margin-bottom: 1rem;
  color: currentColor;
}

.button-group {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
}

/* Responsive */
@media (max-width: 768px) {
  .controls {
    flex-direction: column;
    align-items: stretch;
  }

  .button-group {
    flex-direction: column;
  }

  .button, .btn {
    width: 100%;
  }
}
</style> 