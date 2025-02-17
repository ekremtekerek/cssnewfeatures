<script setup>
import { ref } from "vue";

const selectedExample = ref("basic");
const showDescription = ref(true);

const oldCode = `
/* Eski Yöntem - JavaScript ile Popover */
const popover = document.querySelector('.popover');
const trigger = document.querySelector('.trigger');
let isOpen = false;

// Açma/Kapama Kontrolü
trigger.addEventListener('click', () => {
  isOpen = !isOpen;
  popover.style.display = isOpen ? 'block' : 'none';
  
  if (isOpen) {
    const rect = trigger.getBoundingClientRect();
    popover.style.top = rect.bottom + window.scrollY + 'px';
    popover.style.left = rect.left + window.scrollX + 'px';
  }
});

// Dışarı Tıklama Kontrolü
document.addEventListener('click', (e) => {
  if (!popover.contains(e.target) && !trigger.contains(e.target)) {
    isOpen = false;
    popover.style.display = 'none';
  }
});

// Scroll ve Resize Olayları
window.addEventListener('scroll', updatePosition);
window.addEventListener('resize', updatePosition);

function updatePosition() {
  if (isOpen) {
    const rect = trigger.getBoundingClientRect();
    popover.style.top = rect.bottom + window.scrollY + 'px';
    popover.style.left = rect.left + window.scrollX + 'px';
  }
}`;

const newCode = `
/* Yeni Yöntem - Popover API */

<!-- HTML -->
<button popovertarget="menu">Menüyü Aç</button>
<div id="menu" popover>Menü İçeriği</div>

/* CSS */
[popover] {
  /* Varsayılan Stiller */
  padding: 1rem;
  border: none;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

/* Açılış Animasyonu */
[popover]:popover-open {
  animation: slide-in 0.2s ease;
}

/* Konumlandırma */
[popover].top {
  position: fixed;
  top: anchor(bottom);
  left: anchor(start);
}

/* Farklı Konumlar */
[popover].bottom { bottom: anchor(top); }
[popover].start { left: anchor(end); }
[popover].end { right: anchor(start); }

/* Otomatik Konumlandırma */
[popover].auto {
  top: anchor(bottom);
  left: anchor(start);
  margin: 8px;
}`;

const examples = {
  basic: {
    title: "Temel Popover",
    description: "En basit popover kullanımı",
    code: `
<button popovertarget="basic-popover">Temel Popover</button>
<div id="basic-popover" popover>
  <h3>Temel Popover İçeriği</h3>
  <p>Bu bir temel popover örneğidir.</p>
</div>`,
  },
  menu: {
    title: "Menü Popover",
    description: "Dropdown menü olarak kullanım",
    code: `
<button popovertarget="menu-popover">Menü</button>
<div id="menu-popover" popover class="menu">
  <div class="menu-item">Profil</div>
  <div class="menu-item">Ayarlar</div>
  <div class="menu-item">Çıkış</div>
</div>`,
  },
  tooltip: {
    title: "Tooltip Popover",
    description: "Tooltip olarak kullanım",
    code: `
<button 
  popovertarget="tooltip-popover"
  onmouseenter="document.getElementById('tooltip-popover').showPopover()"
  onmouseleave="document.getElementById('tooltip-popover').hidePopover()"
>
  Bilgi
</button>
<div id="tooltip-popover" popover class="tooltip">
  Yardımcı bilgi metni
</div>`,
  },
  form: {
    title: "Form Popover",
    description: "Form içeren popover",
    code: `
<button popovertarget="form-popover">Formu Aç</button>
<form id="form-popover" popover>
  <input type="text" placeholder="Kullanıcı adı">
  <input type="password" placeholder="Şifre">
  <button type="submit">Giriş</button>
  <button popovertarget="form-popover" popovertargetaction="hide">
    İptal
  </button>
</form>`,
  },
};

const positions = ["top", "right", "bottom", "left"];
const selectedPosition = ref("bottom");
</script>

<template>
  <div class="feature-section">
    <h2>Popover API</h2>
    <p class="description">
      Popover API, açılır menüler, ipuçları ve modallar gibi öğeleri kolayca
      oluşturmanızı sağlar. JavaScript kullanmadan, sadece HTML öznitelikleri
      ile popover davranışını kontrol edebilirsiniz.
    </p>

    <div class="comparison">
      <div class="code-section">
        <h3>Eski Yöntem</h3>
        <div class="code-block">
          <pre><code>{{ oldCode }}</code></pre>
        </div>
      </div>

      <div class="code-section">
        <h3>Yeni Yöntem (Popover API)</h3>
        <div class="code-block">
          <pre><code>{{ newCode }}</code></pre>
        </div>
      </div>
    </div>

    <div class="demo">
      <h3>İnteraktif Örnekler</h3>

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

      <!-- Temel Popover -->
      <div v-if="selectedExample === 'basic'" class="example-container">
        <h4>{{ examples.basic.title }}</h4>
        <p>{{ examples.basic.description }}</p>

        <div class="demo-area">
          <button popovertarget="basic-popover" class="trigger-button">
            Temel Popover'ı Aç
          </button>
          <div id="basic-popover" popover class="basic-popover">
            <h3>Temel Popover İçeriği</h3>
            <p>Bu bir temel popover örneğidir.</p>
            <button popovertarget="basic-popover" popovertargetaction="hide">
              Kapat
            </button>
          </div>
        </div>
      </div>

      <!-- Menü Popover -->
      <div v-if="selectedExample === 'menu'" class="example-container">
        <h4>{{ examples.menu.title }}</h4>
        <p>{{ examples.menu.description }}</p>

        <div class="demo-area">
          <button popovertarget="menu-popover" class="trigger-button">
            Menüyü Aç
          </button>
          <div id="menu-popover" popover class="menu-popover">
            <div class="menu-item">
              <i class="fas fa-user"></i>
              <span>Profil</span>
            </div>
            <div class="menu-item">
              <i class="fas fa-cog"></i>
              <span>Ayarlar</span>
            </div>
            <div class="menu-item">
              <i class="fas fa-bell"></i>
              <span>Bildirimler</span>
            </div>
            <div class="menu-item danger">
              <i class="fas fa-sign-out-alt"></i>
              <span>Çıkış</span>
            </div>
          </div>
        </div>
      </div>

      <!-- Tooltip Popover -->
      <div v-if="selectedExample === 'tooltip'" class="example-container">
        <h4>{{ examples.tooltip.title }}</h4>
        <p>{{ examples.tooltip.description }}</p>

        <div class="demo-area">
          <!-- Pozisyon Kontrolleri -->
          <div class="position-controls">
            <button
              v-for="pos in positions"
              :key="pos"
              :class="['position-button', { active: selectedPosition === pos }]"
              @click="selectedPosition = pos"
            >
              {{ pos }}
            </button>
          </div>

          <button popovertarget="tooltip-popover" class="trigger-button info">
            Bilgi için Tıkla
          </button>
          <div
            id="tooltip-popover"
            popover
            :class="['tooltip-popover', selectedPosition]"
          >
            <div class="tooltip-content">
              <i class="fas fa-info-circle"></i>
              <p>
                Bu bir bilgi baloncuğudur. Farklı pozisyonlarda
                görüntülenebilir.
              </p>
            </div>
          </div>
        </div>
      </div>

      <!-- Form Popover -->
      <div v-if="selectedExample === 'form'" class="example-container">
        <h4>{{ examples.form.title }}</h4>
        <p>{{ examples.form.description }}</p>

        <div class="demo-area">
          <button popovertarget="form-popover" class="trigger-button">
            Hızlı Giriş
          </button>
          <form id="form-popover" popover class="form-popover">
            <h3>Giriş Yap</h3>
            <div class="form-group">
              <label>E-posta</label>
              <input type="email" placeholder="ornek@mail.com" />
            </div>
            <div class="form-group">
              <label>Şifre</label>
              <input type="password" placeholder="••••••••" />
            </div>
            <div class="form-actions">
              <button type="submit" class="submit-button">Giriş Yap</button>
              <button
                type="button"
                popovertarget="form-popover"
                popovertargetaction="hide"
                class="cancel-button"
              >
                İptal
              </button>
            </div>
          </form>
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
  color: #374151;
}

/* Kod Karşılaştırma */
.comparison {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  margin: 2rem 0;
}

@media (max-width: 768px) {
  .comparison {
    grid-template-columns: 1fr;
  }
}

.code-block {
  background: #1e1e1e;
  border-radius: 8px;
  overflow: hidden;
}

pre {
  margin: 0;
  padding: 1.5rem;
  overflow-x: auto;
}

code {
  font-family: "Fira Code", monospace;
  font-size: 0.875rem;
  line-height: 1.6;
}

/* Örnek Seçici */
.example-selector {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
  flex-wrap: wrap;
}

.example-button {
  padding: 0.75rem 1.5rem;
  border: 2px solid #3b82f6;
  border-radius: 8px;
  background: transparent;
  color: #3b82f6;
  cursor: pointer;
  transition: all 0.3s ease;
}

.example-button:hover {
  background: #eff6ff;
}

.example-button.active {
  background: #3b82f6;
  color: white;
}

/* Demo Alanı */
.example-container {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 2rem;
}

.demo-area {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 200px;
  background: #f9fafb;
  border-radius: 8px;
  padding: 2rem;
  margin-top: 1rem;
}

/* Trigger Butonları */
.trigger-button {
  padding: 0.75rem 1.5rem;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.trigger-button:hover {
  background: #2563eb;
}

.trigger-button.info {
  background: #10b981;
}

.trigger-button.info:hover {
  background: #059669;
}

/* Popover Stilleri */
[popover] {
  padding: 1.5rem;
  border: none;
  border-radius: 8px;
  background: white;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
    0 2px 4px -1px rgba(0, 0, 0, 0.06);
}

[popover]:popover-open {
  animation: slide-in 0.2s ease;
}

/* Temel Popover */
.basic-popover {
  min-width: 300px;
}

.basic-popover h3 {
  margin: 0 0 1rem 0;
  color: #1f2937;
}

.basic-popover p {
  margin: 0 0 1rem 0;
  color: #4b5563;
}

/* Menü Popover */
.menu-popover {
  min-width: 200px;
  padding: 0.5rem;
}

.menu-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 1rem;
  color: #374151;
  cursor: pointer;
  border-radius: 6px;
  transition: all 0.2s ease;
}

.menu-item:hover {
  background: #f3f4f6;
}

.menu-item.danger {
  color: #ef4444;
}

.menu-item.danger:hover {
  background: #fef2f2;
}

/* Tooltip Popover */
.tooltip-popover {
  padding: 0.75rem 1rem;
  max-width: 250px;
}

.tooltip-content {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
  color: #4b5563;
}

.tooltip-content i {
  color: #3b82f6;
  font-size: 1.25rem;
}

/* Form Popover */
.form-popover {
  min-width: 320px;
}

.form-popover h3 {
  margin: 0 0 1.5rem 0;
  color: #1f2937;
}

.form-group {
  margin-bottom: 1rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  color: #374151;
}

.form-group input {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  transition: all 0.3s ease;
}

.form-group input:focus {
  outline: none;
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}

.form-actions {
  display: flex;
  gap: 1rem;
  margin-top: 1.5rem;
}

.submit-button {
  flex: 1;
  padding: 0.75rem;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.submit-button:hover {
  background: #2563eb;
}

.cancel-button {
  padding: 0.75rem;
  background: #f3f4f6;
  color: #374151;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.cancel-button:hover {
  background: #e5e7eb;
}

/* Pozisyon Kontrolleri */
.position-controls {
  position: absolute;
  top: 1rem;
  right: 1rem;
  display: flex;
  gap: 0.5rem;
}

.position-button {
  padding: 0.5rem 1rem;
  background: #f3f4f6;
  color: #374151;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.position-button:hover {
  background: #e5e7eb;
}

.position-button.active {
  background: #3b82f6;
  color: white;
}

/* Animasyonlar */
@keyframes slide-in {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Responsive */
@media (max-width: 640px) {
  .example-selector {
    flex-direction: column;
  }

  .example-button {
    width: 100%;
  }

  .form-popover {
    min-width: 100%;
  }
}
</style>
