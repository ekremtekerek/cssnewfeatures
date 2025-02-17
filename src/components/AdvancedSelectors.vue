<script setup>
import { ref } from 'vue'

const selectedExample = ref('has')
const showDescription = ref(true)

const oldCode = `
/* Eski yöntem - karmaşık seçiciler */
.form input:invalid {
  border-color: red;
}

.list li:first-child {
  font-weight: bold;
}

.text::selection {
  background: yellow;
}

/* JavaScript ile kontrol gerekir */
document.querySelectorAll('.parent').forEach(parent => {
  if (parent.querySelector('.special')) {
    parent.classList.add('has-special');
  }
});

/* Eski yöntem - :is() benzeri seçim için */
h1:hover, h2:hover, h3:hover {
  color: #3b82f6;
}

/* Eski yöntem - :where() benzeri seçim için */
.header .nav .link,
.footer .nav .link {
  color: blue;
}

/* Eski yöntem - :not() için */
.menu > *:not(.active) {
  opacity: 0.7;
}`

const newCode = `
/* Yeni gelişmiş seçiciler */

/* :has() seçicisi */
.parent:has(.special) {
  /* .special sınıfına sahip alt eleman içeren .parent */
  background: #e5e7eb;
}

/* :is() seçicisi */
:is(h1, h2, h3):hover {
  /* h1, h2 veya h3'ün hover durumu */
  color: #3b82f6;
}

/* :where() seçicisi */
:where(.header, .footer) .nav .link {
  /* Düşük özgüllükte stil tanımlama */
  color: blue;
}

/* :not() seçicisi */
.menu > :not(.active) {
  /* .active olmayan tüm direkt alt elemanlar */
  opacity: 0.7;
}`

const examples = {
  has: {
    title: ':has() Seçicisi',
    description: 'Belirli alt elemanlara sahip öğeleri seçer',
    items: [
      {
        title: 'Özel İçerikli Kart',
        content: 'Normal içerik',
        hasSpecial: true
      },
      {
        title: 'Normal Kart',
        content: 'Normal içerik',
        hasSpecial: false
      }
    ]
  },
  is: {
    title: ':is() Seçicisi',
    description: 'Birden fazla seçiciyi gruplar',
    items: [
      { tag: 'h1', content: 'Başlık 1' },
      { tag: 'h2', content: 'Başlık 2' },
      { tag: 'h3', content: 'Başlık 3' },
      { tag: 'p', content: 'Normal paragraf' }
    ]
  },
  where: {
    title: ':where() Seçicisi',
    description: 'Düşük özgüllükte gruplandırma yapar',
    items: [
      {
        type: 'section',
        title: 'Bölüm Başlığı',
        content: 'Bölüm içeriği'
      },
      {
        type: 'article',
        title: 'Makale Başlığı',
        content: 'Makale içeriği'
      }
    ]
  },
  not: {
    title: ':not() Seçicisi',
    description: 'Belirli koşulları sağlamayan öğeleri seçer',
    items: [
      'Liste Öğesi 1',
      'Liste Öğesi 2',
      'Liste Öğesi 3',
      'Liste Öğesi 4',
      'Liste Öğesi 5'
    ]
  }
}

const selectorDescriptions = {
  has: {
    title: ':has() Seçicisi',
    description: `
      :has() seçicisi, belirli alt elemanlara sahip öğeleri seçmenizi sağlar. 
      Bu, daha önce sadece JavaScript ile yapılabilen bir işlemi CSS ile yapmanıza olanak tanır.
      
      Avantajları:
      • JavaScript kodu yazmaya gerek kalmaz
      • Performans açısından daha verimlidir
      • Dinamik stil değişiklikleri daha kolay yapılır
      • Kod daha temiz ve bakımı daha kolaydır
    `,
    oldExample: `
      /* Eski Yöntem */
      // HTML: <div class="parent"><div class="child special"></div></div>
      
      // JavaScript ile kontrol
      document.querySelectorAll('.parent').forEach(parent => {
        if (parent.querySelector('.special')) {
          parent.classList.add('has-special');
        }
      });
      
      // CSS
      .parent.has-special {
        background: #e5e7eb;
      }
    `,
    newExample: `
      /* Yeni Yöntem */
      // Sadece CSS ile kontrol
      .parent:has(.special) {
        background: #e5e7eb;
      }
    `
  },
  is: {
    title: ':is() Seçicisi',
    description: `
      :is() seçicisi, birden fazla seçiciyi tek bir grupta toplamanızı sağlar.
      Kod tekrarını azaltır ve daha okunabilir bir CSS yazmanıza yardımcı olur.
      
      Avantajları:
      • Daha kısa ve öz kod yazımı
      • Bakımı daha kolay
      • Performans optimizasyonu
      • Seçici listelerini dinamik olarak değiştirme kolaylığı
    `,
    oldExample: `
      /* Eski Yöntem */
      header h1:hover,
      header h2:hover,
      header h3:hover {
        color: blue;
      }
      
      main h1:hover,
      main h2:hover,
      main h3:hover {
        color: blue;
      }
    `,
    newExample: `
      /* Yeni Yöntem */
      :is(header, main) :is(h1, h2, h3):hover {
        color: blue;
      }
    `
  },
  where: {
    title: ':where() Seçicisi',
    description: `
      :where() seçicisi, :is() seçicisine benzer ancak özgüllük değeri her zaman sıfırdır.
      Bu özellik, stil geçişlerini ve üzerine yazma işlemlerini kolaylaştırır.
      
      Avantajları:
      • Düşük özgüllük sayesinde kolay üzerine yazma
      • Daha esnek stil tanımları
      • Framework ve tema geliştirmede kolaylık
      • Stil çakışmalarını önleme
    `,
    oldExample: `
      /* Eski Yöntem */
      /* Özgüllük sorunlarını çözmek için !important kullanımı */
      section p { color: blue !important; }
      article p { color: blue !important; }
      
      /* veya uzun seçici zincirleri */
      body section p { color: blue; }
      body article p { color: blue; }
    `,
    newExample: `
      /* Yeni Yöntem */
      :where(section, article) p {
        color: blue;
      }
    `
  },
  not: {
    title: ':not() Seçicisi',
    description: `
      :not() seçicisi artık birden fazla seçiciyi destekler ve daha güçlü negatif seçimler yapmanızı sağlar.
      Özellikle liste öğeleri ve form elemanları için çok kullanışlıdır.
      
      Avantajları:
      • Birden fazla istisna tanımlayabilme
      • Daha esnek seçici kombinasyonları
      • Karmaşık durumları daha kolay yönetme
      • Kod tekrarını azaltma
    `,
    oldExample: `
      /* Eski Yöntem */
      /* Her durumu ayrı ayrı belirtmek gerekir */
      .list li {
        margin: 0.5rem 0;
      }
      .list li:first-child {
        margin-top: 0;
      }
      .list li:last-child {
        margin-bottom: 0;
      }
    `,
    newExample: `
      /* Yeni Yöntem */
      .list li:not(:first-child, :last-child) {
        margin: 0.5rem 0;
      }
    `
  }
}
</script>

<template>
  <div class="feature-section">
    <h2>Gelişmiş CSS Seçicileri</h2>
    <p class="description">
      Modern CSS'in getirdiği gelişmiş seçiciler, daha az JavaScript kodu yazmanızı sağlar
      ve stil tanımlamalarınızı daha güçlü hale getirir. Aşağıdaki örneklerde yeni seçicilerin
      kullanımını ve eski yöntemlerle karşılaştırmasını görebilirsiniz.
    </p>

    <div class="example">
      <h3>Eski Yöntem</h3>
      <pre><code>{{ oldCode }}</code></pre>

      <h3>Yeni Yöntem (Gelişmiş Seçiciler)</h3>
      <pre><code>{{ newCode }}</code></pre>

      <div class="demo">
        <h3>Canlı Demo</h3>
        
        <!-- Seçici Kontrolü -->
        <div class="selector-control">
          <button 
            v-for="(example, key) in examples"
            :key="key"
            :class="['selector-button', { active: selectedExample === key }]"
            @click="selectedExample = key"
          >
            {{ example.title }}
          </button>
        </div>

        <!-- :has() Demo -->
        <div v-if="selectedExample === 'has'" class="has-demo">
          <h4>{{ examples.has.title }}</h4>
          <p>{{ examples.has.description }}</p>
          
          <div class="cards">
            <div 
              v-for="(card, index) in examples.has.items"
              :key="index"
              class="card"
              :class="{ 'has-special': card.hasSpecial }"
            >
              <h5>{{ card.title }}</h5>
              <p>{{ card.content }}</p>
              <div v-if="card.hasSpecial" class="special">
                Özel İçerik
              </div>
            </div>
          </div>
        </div>

        <!-- :is() Demo -->
        <div v-if="selectedExample === 'is'" class="is-demo">
          <h4>{{ examples.is.title }}</h4>
          <p>{{ examples.is.description }}</p>
          
          <div class="headings">
            <component
              v-for="(item, index) in examples.is.items"
              :key="index"
              :is="item.tag"
            >
              {{ item.content }}
            </component>
          </div>
        </div>

        <!-- :where() Demo -->
        <div v-if="selectedExample === 'where'" class="where-demo">
          <h4>{{ examples.where.title }}</h4>
          <p>{{ examples.where.description }}</p>
          
          <div class="containers">
            <component
              v-for="(item, index) in examples.where.items"
              :key="index"
              :is="item.type"
              class="content-container"
            >
              <h5>{{ item.title }}</h5>
              <p>{{ item.content }}</p>
            </component>
          </div>
        </div>

        <!-- :not() Demo -->
        <div v-if="selectedExample === 'not'" class="not-demo">
          <h4>{{ examples.not.title }}</h4>
          <p>{{ examples.not.description }}</p>
          
          <ul class="list">
            <li 
              v-for="(item, index) in examples.not.items"
              :key="index"
            >
              {{ item }}
            </li>
          </ul>
        </div>
      </div>
    </div>

    <!-- Seçici Açıklamaları -->
    <div class="selector-descriptions">
      <div 
        v-for="(desc, key) in selectorDescriptions"
        :key="key"
        :class="['selector-description', { active: selectedExample === key }]"
      >
        <h3>{{ desc.title }}</h3>
        <div class="description-content">
          <p class="selector-info">{{ desc.description }}</p>
          
          <div class="code-comparison">
            <div class="code-block">
              <h4>Eski Yöntem</h4>
              <pre><code>{{ desc.oldExample }}</code></pre>
            </div>
            <div class="code-block">
              <h4>Yeni Yöntem</h4>
              <pre><code>{{ desc.newExample }}</code></pre>
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
  color: #374151;
}

pre {
  background: #1e1e1e;
  padding: 1.5rem;
  border-radius: 8px;
  overflow-x: auto;
  margin: 1rem 0;
  color: #d4d4d4;
}

code {
  font-family: 'Fira Code', monospace;
  font-size: 0.875rem;
  line-height: 1.6;
}

.demo {
  margin-top: 2rem;
}

/* Seçici Kontrol */
.selector-control {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
  flex-wrap: wrap;
}

.selector-button {
  padding: 0.75rem 1.5rem;
  border: 2px solid #3b82f6;
  border-radius: 8px;
  background: transparent;
  color: #3b82f6;
  cursor: pointer;
  transition: all 0.3s ease;
}

.selector-button:hover {
  background: #eff6ff;
}

.selector-button.active {
  background: #3b82f6;
  color: white;
}

/* :has() Demo */
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
  margin-top: 1rem;
}

.card {
  padding: 1.5rem;
  border-radius: 8px;
  background: white;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
}

.card:has(.special) {
  background: #f3f4f6;
  border: 2px solid #3b82f6;
}

.special {
  margin-top: 1rem;
  padding: 0.5rem;
  background: #3b82f6;
  color: white;
  border-radius: 4px;
  text-align: center;
}

/* :is() Demo */
.headings {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

:is(h1, h2, h3):hover {
  color: #3b82f6;
  cursor: pointer;
}

/* :where() Demo */
.containers {
  display: grid;
  gap: 1.5rem;
  margin-top: 1rem;
}

.content-container {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

:where(section, article) p {
  color: #4b5563;
  line-height: 1.6;
}

/* :not() Demo */
.list {
  list-style: none;
  padding: 0;
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.list li {
  padding: 1rem;
  border-bottom: 1px solid #e5e7eb;
}

.list li:not(:first-child, :last-child) {
  background: #f9fafb;
}

.list li:first-child {
  background: #3b82f6;
  color: white;
}

.list li:last-child {
  border-bottom: none;
  background: #f3f4f6;
}

/* Demo Başlıkları */
h4 {
  margin: 0 0 0.5rem 0;
  color: #1f2937;
}

/* Açıklamalar */
.demo p {
  color: #6b7280;
  margin-bottom: 1.5rem;
}

/* Syntax Highlighting */
.comment { color: #6a9955; }
.keyword { color: #569cd6; }
.string { color: #ce9178; }
.function { color: #dcdcaa; }
.selector { color: #d7ba7d; }

/* Responsive */
@media (max-width: 640px) {
  .selector-control {
    flex-direction: column;
  }
  
  .selector-button {
    width: 100%;
  }
  
  .cards {
    grid-template-columns: 1fr;
  }
}

/* Seçici Açıklamaları */
.selector-descriptions {
  margin: 2rem 0;
}

.selector-description {
  background: white;
  border-radius: 8px;
  margin-bottom: 1rem;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  display: none;
}

.selector-description.active {
  display: block;
}

.selector-description h3 {
  background: #f3f4f6;
  margin: 0;
  padding: 1rem 1.5rem;
  color: #1f2937;
  border-bottom: 1px solid #e5e7eb;
}

.description-content {
  padding: 1.5rem;
}

.selector-info {
  white-space: pre-line;
  color: #4b5563;
  line-height: 1.6;
  margin-bottom: 2rem;
}

.code-comparison {
  display: grid;
  grid-template-columns: 1fr;
  gap: 2rem;
  margin: 2rem 0;
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
  font-size: 1.2rem;
  border-bottom: 1px solid #374151;
  padding-bottom: 0.5rem;
}

.code-block h4 {
  margin: 0 0 0.5rem 0;
  color: #d4d4d4;
}

.code-block pre {
  margin: 0;
  padding: 1rem;
}
</style> 