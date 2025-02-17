<script setup>
import { ref } from 'vue'

const selectedExample = ref('tooltip')
const isTooltipVisible = ref(false)
const isDropdownVisible = ref(false)
const isModalVisible = ref(false)
const isPopoverVisible = ref(false)
const selectedPosition = ref('bottom')

const oldCode = `
/* Eski Yöntem - JavaScript ile Konumlandırma */
function positionElement(anchor, element, position) {
  const anchorRect = anchor.getBoundingClientRect();
  const elementRect = element.getBoundingClientRect();
  
  switch(position) {
    case 'top':
      element.style.top = anchorRect.top - elementRect.height + 'px';
      element.style.left = anchorRect.left + (anchorRect.width - elementRect.width) / 2 + 'px';
      break;
    case 'bottom':
      element.style.top = anchorRect.bottom + 'px';
      element.style.left = anchorRect.left + (anchorRect.width - elementRect.width) / 2 + 'px';
      break;
    case 'left':
      element.style.top = anchorRect.top + (anchorRect.height - elementRect.height) / 2 + 'px';
      element.style.left = anchorRect.left - elementRect.width + 'px';
      break;
    case 'right':
      element.style.top = anchorRect.top + (anchorRect.height - elementRect.height) / 2 + 'px';
      element.style.left = anchorRect.right + 'px';
      break;
  }
}`

const newCode = `
/* Yeni Yöntem - Anchor Positioning API */

/* Tooltip */
.tooltip {
  position: fixed;
  top: anchor(bottom);
  left: anchor(center);
  transform: translateX(-50%);
}

/* Dropdown */
.dropdown {
  position: fixed;
  top: anchor(bottom);
  left: anchor(start);
  width: anchor(width);
}

/* Modal */
.modal {
  position: fixed;
  top: anchor(center);
  left: anchor(center);
  transform: translate(-50%, -50%);
}

/* Popover */
.popover {
  position: fixed;
  top: anchor(top);
  left: anchor(end);
  height: anchor(height);
}`

const examples = {
  tooltip: {
    title: 'Tooltip Konumlandırma',
    description: 'Farklı yönlerde tooltip konumlandırma örneği. Butonun üzerine gelerek veya tıklayarak tooltipleri görüntüleyebilirsiniz.'
  },
  dropdown: {
    title: 'Dropdown Menü',
    description: 'Açılır menü konumlandırma örneği. Menü butona göre konumlandırılır ve genişliği butona göre ayarlanır.'
  },
  modal: {
    title: 'Modal Pencere',
    description: 'Modal pencere konumlandırma örneği. Modal, ekranın merkezinde konumlandırılır.'
  },
  popover: {
    title: 'Popover',
    description: 'Popover konumlandırma örneği. İçerik, referans elemana göre konumlandırılır.'
  }
}

const positions = [
  { value: 'top', label: 'Üst' },
  { value: 'right', label: 'Sağ' },
  { value: 'bottom', label: 'Alt' },
  { value: 'left', label: 'Sol' }
]

const menuItems = [
  { id: 1, label: 'Profil', icon: '👤' },
  { id: 2, label: 'Ayarlar', icon: '⚙️' },
  { id: 3, label: 'Mesajlar', icon: '✉️' },
  { id: 4, label: 'Çıkış Yap', icon: '🚪' }
]
</script>

<template>
  <div class="feature-section">
    <h2>Anchor Positioning API</h2>
    <p class="description">
      Anchor Positioning API, elementleri referans noktalarına göre otomatik olarak konumlandırmanızı sağlar.
      JavaScript kullanmadan, sadece CSS ile karmaşık konumlandırma senaryolarını çözebilirsiniz.
    </p>

    <div class="code-comparison">
      <div class="code-block">
        <h3>Eski Yöntem</h3>
        <pre><code>{{ oldCode }}</code></pre>
      </div>
      <div class="code-block">
        <h3>Yeni Yöntem (Anchor Positioning)</h3>
        <pre><code>{{ newCode }}</code></pre>
      </div>
    </div>

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

    <div class="example-description">
      {{ examples[selectedExample].description }}
    </div>

    <div class="demo-container">
      <!-- Tooltip Örneği -->
      <div v-if="selectedExample === 'tooltip'" class="demo-section">
        <div class="position-controls">
          <button 
            v-for="pos in positions"
            :key="pos.value"
            :class="['position-button', { active: selectedPosition === pos.value }]"
            @click="selectedPosition = pos.value"
          >
            {{ pos.label }}
          </button>
        </div>

        <div class="tooltip-demo">
          <button 
            class="anchor-button"
            @mouseenter="isTooltipVisible = true"
            @mouseleave="isTooltipVisible = false"
          >
            Tooltip Butonu
            <div 
              v-if="isTooltipVisible"
              class="tooltip"
              :class="selectedPosition"
            >
              Bu bir tooltip örneğidir
              <div class="tooltip-arrow"></div>
            </div>
          </button>
        </div>
      </div>

      <!-- Dropdown Örneği -->
      <div v-if="selectedExample === 'dropdown'" class="demo-section">
        <div class="dropdown-demo">
          <button 
            class="dropdown-button"
            @click="isDropdownVisible = !isDropdownVisible"
          >
            Menüyü Aç
            <div 
              v-if="isDropdownVisible"
              class="dropdown-menu"
            >
              <div 
                v-for="item in menuItems"
                :key="item.id"
                class="menu-item"
              >
                <span class="menu-icon">{{ item.icon }}</span>
                {{ item.label }}
              </div>
            </div>
          </button>
        </div>
      </div>

      <!-- Modal Örneği -->
      <div v-if="selectedExample === 'modal'" class="demo-section">
        <button 
          class="modal-button"
          @click="isModalVisible = !isModalVisible"
        >
          Modal'ı Aç
        </button>
        <div 
          v-if="isModalVisible"
          class="modal-overlay"
          @click="isModalVisible = false"
        >
          <div 
            class="modal-content"
            @click.stop
          >
            <div class="modal-header">
              <h3>Modal Başlığı</h3>
              <button 
                class="close-button"
                @click="isModalVisible = false"
              >
                ×
              </button>
            </div>
            <div class="modal-body">
              <p>Bu bir modal içeriğidir. Anchor positioning ile otomatik olarak merkezlenir.</p>
            </div>
            <div class="modal-footer">
              <button 
                class="primary-button"
                @click="isModalVisible = false"
              >
                Tamam
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Popover Örneği -->
      <div v-if="selectedExample === 'popover'" class="demo-section">
        <div class="popover-demo">
          <div class="content-area">
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
            <button 
              class="popover-button"
              @click="isPopoverVisible = !isPopoverVisible"
            >
              Bilgi
              <div 
                v-if="isPopoverVisible"
                class="popover"
              >
                <div class="popover-header">
                  <h4>Detaylı Bilgi</h4>
                  <button 
                    class="close-button"
                    @click="isPopoverVisible = false"
                  >
                    ×
                  </button>
                </div>
                <div class="popover-body">
                  <p>Bu bir popover içeriğidir. İçerik referans elemana göre konumlandırılır.</p>
                </div>
              </div>
            </button>
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
  color: #374151;
}

.code-comparison {
  display: flex;
  flex-direction: column;
  gap: 2rem;

}

.code-block {
  background: #1e1e1e;
  border-radius: 8px;
  padding: 1.5rem;
}

.code-block h3 {
  color: #e5e7eb;
  margin-top: 0;
  margin-bottom: 1rem;
}

pre {
  margin: 0;
  overflow-x: auto;
}

code {
  font-family: 'Fira Code', monospace;
  font-size: 0.875rem;
  line-height: 1.6;
}

.example-selector {
  display: flex;
  gap: 1rem;
  margin: 2rem 0;
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

.example-description {
  color: #6b7280;
  margin-bottom: 2rem;
  font-style: italic;
}

.demo-container {
  background: white;
  border-radius: 8px;
  padding: 2rem;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.demo-section {
  position: relative;
  min-height: 300px;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Position Controls */
.position-controls {
  position: absolute;
  top: 1rem;
  right: 1rem;
  display: flex;
  gap: 0.5rem;
}

.position-button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  background: #f3f4f6;
  color: #374151;
  cursor: pointer;
  transition: all 0.3s ease;
}

.position-button:hover {
  background: #e5e7eb;
}

.position-button.active {
  background: #3b82f6;
  color: white;
}

/* Tooltip Styles */
.tooltip-demo {
  position: relative;
  display: inline-block;
}

.anchor-button {
  padding: 0.75rem 1.5rem;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.tooltip {
  position: absolute;
  background: #1f2937;
  color: white;
  padding: 0.75rem 1rem;
  border-radius: 6px;
  font-size: 0.875rem;
  white-space: nowrap;
  z-index: 1000;
}

.tooltip.top {
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%) translateY(-10px);
  margin-bottom: 10px;
}

.tooltip.bottom {
  top: 100%;
  left: 50%;
  transform: translateX(-50%) translateY(10px);
  margin-top: 10px;
}

.tooltip.left {
  right: 100%;
  top: 50%;
  transform: translateY(-50%) translateX(-10px);
  margin-right: 10px;
}

.tooltip.right {
  left: 100%;
  top: 50%;
  transform: translateY(-50%) translateX(10px);
  margin-left: 10px;
}

.tooltip-arrow {
  position: absolute;
  width: 0;
  height: 0;
  border: 8px solid transparent;
}

.tooltip.top .tooltip-arrow {
  bottom: -8px;
  left: 50%;
  transform: translateX(-50%);
  border-top-color: #1f2937;
}

.tooltip.bottom .tooltip-arrow {
  top: -8px;
  left: 50%;
  transform: translateX(-50%);
  border-bottom-color: #1f2937;
}

.tooltip.left .tooltip-arrow {
  right: -8px;
  top: 50%;
  transform: translateY(-50%);
  border-left-color: #1f2937;
}

.tooltip.right .tooltip-arrow {
  left: -8px;
  top: 50%;
  transform: translateY(-50%);
  border-right-color: #1f2937;
}

/* Dropdown Styles */
.dropdown-demo {
  position: relative;
  display: inline-block;
}

.dropdown-button {
  padding: 0.75rem 1.5rem;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  min-width: 200px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);
  margin-top: 0.5rem;
  z-index: 1000;
}

.menu-item {
  padding: 0.75rem 1rem;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  color: #374151;
  cursor: pointer;
  transition: all 0.2s ease;
}

.menu-item:hover {
  background: #f3f4f6;
}

.menu-icon {
  font-size: 1.25rem;
}

/* Modal Styles */
.modal-button {
  padding: 0.75rem 1.5rem;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  position: relative;
  background: white;
  border-radius: 8px;
  width: 90%;
  max-width: 500px;
  z-index: 1001;
  margin: auto;
}

.modal-header {
  padding: 1.5rem;
  border-bottom: 1px solid #e5e7eb;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-header h3 {
  margin: 0;
  color: #1f2937;
}

.modal-body {
  padding: 1.5rem;
}

.modal-footer {
  padding: 1.5rem;
  border-top: 1px solid #e5e7eb;
  display: flex;
  justify-content: flex-end;
}

.close-button {
  background: none;
  border: none;
  font-size: 1.5rem;
  color: #6b7280;
  cursor: pointer;
  padding: 0.25rem;
  transition: all 0.2s ease;
}

.close-button:hover {
  color: #1f2937;
}

.primary-button {
  padding: 0.75rem 1.5rem;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.primary-button:hover {
  background: #2563eb;
}

/* Popover Styles */
.popover-demo {
  position: relative;
  display: inline-block;
}

.content-area {
  max-width: 600px;
  margin: 0 auto;
  text-align: center;
}

.popover-button {
  padding: 0.75rem 1.5rem;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 1rem;
}

.popover {
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);
  width: 300px;
  z-index: 1000;
  margin-top: 1rem;
}

.popover-header {
  padding: 1rem;
  border-bottom: 1px solid #e5e7eb;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.popover-header h4 {
  margin: 0;
  color: #1f2937;
}

.popover-body {
  padding: 1rem;
}

/* Responsive Styles */
@media (max-width: 768px) {
  .code-comparison {
    grid-template-columns: 1fr;
  }

  .example-selector {
    flex-direction: column;
  }

  .example-button {
    width: 100%;
  }

  .position-controls {
    position: static;
    margin-bottom: 1rem;
  }

  .demo-section {
    min-height: 400px;
  }

  .modal-content {
    width: 95%;
  }
}
</style> 