<template>
  <div class="feature-section">
    <h2>Scroll-Driven Animations</h2>
    <p class="description">
      Scroll-Driven Animations, sayfa kaydırma hareketlerine bağlı animasyonları
      JavaScript kullanmadan sadece CSS ile oluşturmanıza olanak tanır. Bu
      özellik sayesinde performanslı ve akıcı scroll animasyonları
      oluşturabilirsiniz.
    </p>

    <div class="example">
      <h3>Eski Yöntem (JavaScript ile)</h3>
      <pre><code>{{ oldCode }}</code></pre>

      <h3>Yeni Yöntem (Scroll-Driven Animations)</h3>
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

        <!-- Progress Bar Demo -->
        <div
          v-if="selectedExample === 'progress'"
          class="progress-demo demo-container"
        >
          <div class="demo-header">
            <h4>Scroll Progress Örneği</h4>
            <p>
              Container içinde kaydırarak progress bar'ın ilerleyişini
              gözlemleyin.
            </p>
          </div>
          <div class="demo-content">
            <div class="progress-container">
              <div class="progress-bar"></div>
            </div>
            <div class="scroll-content">
              <div v-for="n in 10" :key="n" class="content-block">
                <h5>Bölüm {{ n }}</h5>
                <p>
                  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed
                  do eiusmod tempor incididunt ut labore et dolore magna aliqua.
                </p>
              </div>
            </div>
          </div>
        </div>

        <!-- Paralaks Demo -->
        <div
          v-if="selectedExample === 'parallax'"
          class="parallax-demo demo-container"
        >
          <div class="demo-header">
            <h4>Paralaks Efekt Örneği</h4>
            <p>Container içinde kaydırarak paralaks efektini gözlemleyin.</p>
          </div>
          <div class="demo-content">
            <div class="parallax-scene">
              <div class="parallax-bg">
                <img
                  :src="examples.parallax.items[0].image"
                  :alt="examples.parallax.items[0].title"
                />
              </div>
              <div class="parallax-element">
                <div class="parallax-content">
                  <h4>{{ examples.parallax.items[0].title }}</h4>
                  <p>{{ examples.parallax.items[0].description }}</p>
                </div>
              </div>
            </div>
            <div class="parallax-scene">
              <div class="parallax-bg">
                <img
                  :src="examples.parallax.items[1].image"
                  :alt="examples.parallax.items[1].title"
                />
              </div>
              <div class="parallax-element">
                <div class="parallax-content">
                  <h4>{{ examples.parallax.items[1].title }}</h4>
                  <p>{{ examples.parallax.items[1].description }}</p>
                </div>
              </div>
            </div>
            <div class="parallax-scene">
              <div class="parallax-bg">
                <img
                  :src="examples.parallax.items[2].image"
                  :alt="examples.parallax.items[2].title"
                />
              </div>
              <div class="parallax-element">
                <div class="parallax-content">
                  <h4>{{ examples.parallax.items[2].title }}</h4>
                  <p>{{ examples.parallax.items[2].description }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Cards Demo -->
        <div
          v-if="selectedExample === 'cards'"
          class="cards-demo demo-container"
        >
          <div class="demo-header">
            <h4>Fade ve Rotate Efektleri</h4>
            <p>
              Container içinde kaydırarak kartların görünüm efektlerini
              gözlemleyin.
            </p>
          </div>
          <div class="demo-content">
            <div class="cards-grid">
              <div
                v-for="(card, index) in examples.cards.items"
                :key="card.title"
                class="fade-element"
              >
                <img :src="card.image" :alt="card.title" />
                <div class="card-content">
                  <h4>{{ card.title }}</h4>
                  <p>{{ card.content }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Navbar Demo -->
        <div
          v-if="selectedExample === 'navbar'"
          class="navbar-demo demo-container"
        >
          <div class="demo-header">
            <h4>Scroll Navbar Örneği</h4>
            <p>Container içinde kaydırarak navbar'ın değişimini gözlemleyin.</p>
          </div>
          <div class="demo-content">
            <nav class="navbar">
              <div class="nav-content">
                <span class="logo">Logo</span>
                <div class="nav-links">
                  <a v-for="link in examples.navbar.links" :key="link" href="#">
                    {{ link }}
                  </a>
                </div>
              </div>
            </nav>
            <div class="page-content">
              <div v-for="n in 5" :key="n" class="content-block">
                <h5>Bölüm {{ n }}</h5>
                <p>
                  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed
                  do eiusmod tempor incididunt ut labore et dolore magna aliqua.
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Video Bölümü -->
    <div class="video-section">
      <h3>Örnek Video</h3>
      <video src="/airbnb.mp4" class="demo-video" controls autoplay muted loop>
        Tarayıcınız video elementini desteklemiyor.
      </video>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const selectedExample = ref("progress");

const oldCode = `
/* Eski yöntem - JavaScript ile scroll animasyonları */

// CSS
.parallax {
  transform: translateY(0);
  opacity: 1;
  transition: all 0.5s;
}

.progress-bar {
  width: 0;
  transition: width 0.3s;
}

// JavaScript
window.addEventListener('scroll', () => {
  // Scroll progress
  const scrolled = window.scrollY;
  const maxScroll = document.documentElement.scrollHeight - window.innerHeight;
  const progress = (scrolled / maxScroll) * 100;
  document.querySelector('.progress-bar').style.width = progress + '%';
  
  // Paralaks efekt
  const elements = document.querySelectorAll('.parallax');
  elements.forEach(el => {
    const rect = el.getBoundingClientRect();
    const scrollPercent = rect.top / window.innerHeight;
    el.style.transform = 'translateY(' + (scrollPercent * 50) + 'px)';
    el.style.opacity = 1 - Math.abs(scrollPercent);
  });
});`;

const newCode = `
/* Scroll-Driven Animations ile JavaScript'e gerek kalmadan */

/* Scroll Progress Bar */
@keyframes scroll-progress {
  from { width: 0; }
  to { width: 100%; }
}

.progress-container {
  position: sticky;
  top: 0;
  z-index: 10;
}

.progress-bar {
  height: 4px;
  background: #3b82f6;
  transform-origin: 0 50%;
  animation: scroll-progress auto linear;
  animation-timeline: scroll(nearest block);
}

/* Paralaks Efekt */
.parallax-scene {
  container-type: inline-size;
  container-name: parallax;
}

.parallax-bg {
  position: fixed;
  width: 100%;
  height: 100%;
}

@keyframes parallax-content {
  from { transform: translateY(0); }
  to { transform: translateY(calc(-30vh * var(--speed))); }
}

.parallax-content {
  position: relative;
  animation: parallax-content auto linear;
  animation-timeline: scroll(nearest block);
  animation-range: entry 0% exit 100%;
}

/* Fade ve Rotate Efektleri */
@keyframes fade-in {
  from { 
    opacity: 0;
    transform: translateY(50px) rotateX(20deg);
  }
  to { 
    opacity: 1;
    transform: translateY(0) rotateX(0);
  }
}

.fade-element {
  view-timeline-name: --card;
  view-timeline-axis: block;
  animation: fade-in both linear;
  animation-timeline: --card;
  animation-range: entry 20% cover 40%;
}

/* Scroll Navbar */
@keyframes navbar-scroll {
  from { 
    background: transparent;
    backdrop-filter: none;
    transform: translateY(0);
  }
  to { 
    background: rgba(255, 255, 255, 0.9);
    backdrop-filter: blur(8px);
    transform: translateY(-5px);
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  }
}

.navbar {
  position: sticky;
  top: 0;
  animation: navbar-scroll both linear;
  animation-timeline: scroll(nearest block);
  animation-range: 0px 100px;
}

/* Rotating Card */
@keyframes rotate-card {
  from { transform: rotateX(0deg); }
  to { transform: rotateX(360deg); }
}

.rotating-card {
  animation: rotate-card auto linear;
  animation-timeline: scroll();
  animation-range: entry exit;
}`;

const examples = {
  progress: {
    title: "Scroll Progress",
    description: "Sayfa scroll durumunu gösteren progress bar",
  },
  parallax: {
    title: "Paralaks Efekt",
    description: "Scroll ile hareket eden paralaks elementler",
    items: [
      {
        title: "Dağlar",
        description:
          "Yüksek dağların muhteşem manzarası, arka planda kayarak hareket ediyor.",
        image: "https://picsum.photos/1200/800?random=1",
        speed: 0.7,
      },
      {
        title: "Orman",
        description:
          "Yemyeşil ormanın derinlikleri, paralaks efektiyle daha etkileyici.",
        image: "https://picsum.photos/1200/800?random=2",
        speed: 0.5,
      },
      {
        title: "Nehir",
        description:
          "Akan nehrin huzur veren görüntüsü, scroll ile birlikte akıyor.",
        image: "https://picsum.photos/1200/800?random=3",
        speed: 0.3,
      },
    ],
  },
  cards: {
    title: "Fade ve Rotate Efektleri",
    description: "Scroll ile görünüm alanına giren kartlar",
    items: [
      {
        title: "Modern Animasyonlar",
        content: "CSS ile güçlü ve performanslı animasyonlar oluşturun.",
        image: "https://picsum.photos/400/300?random=4",
        color: "#3b82f6",
      },
      {
        title: "Scroll Timeline",
        content: "Scroll hareketine bağlı animasyonları kolayca kontrol edin.",
        image: "https://picsum.photos/400/300?random=5",
        color: "#10b981",
      },
      {
        title: "View Timeline",
        content: "Elementlerin görünürlüğüne göre animasyonlar tanımlayın.",
        image: "https://picsum.photos/400/300?random=6",
        color: "#8b5cf6",
      },
      {
        title: "Performans",
        content: "JavaScript kullanmadan yüksek performanslı animasyonlar.",
        image: "https://picsum.photos/400/300?random=7",
        color: "#ef4444",
      },
      {
        title: "Performans",
        content: "JavaScript kullanmadan yüksek performanslı animasyonlar.",
        image: "https://picsum.photos/400/300?random=7",
        color: "#ef4444",
      },
      {
        title: "Performans",
        content: "JavaScript kullanmadan yüksek performanslı animasyonlar.",
        image: "https://picsum.photos/400/300?random=7",
        color: "#ef4444",
      },
      {
        title: "Performans",
        content: "JavaScript kullanmadan yüksek performanslı animasyonlar.",
        image: "https://picsum.photos/400/300?random=7",
        color: "#ef4444",
      },
      {
        title: "Performans",
        content: "JavaScript kullanmadan yüksek performanslı animasyonlar.",
        image: "https://picsum.photos/400/300?random=7",
        color: "#ef4444",
      },
      {
        title: "Performans",
        content: "JavaScript kullanmadan yüksek performanslı animasyonlar.",
        image: "https://picsum.photos/400/300?random=7",
        color: "#ef4444",
      },
    ],
  },
  navbar: {
    title: "Scroll Navbar",
    description: "Scroll durumuna göre değişen navbar",
    links: ["Ana Sayfa", "Özellikler", "Örnekler", "Dokümantasyon", "İletişim"],
    content: [
      {
        title: "Modern Web Tasarımı",
        text: "Scroll-driven animasyonlar ile modern ve etkileyici web siteleri oluşturun.",
      },
      {
        title: "Performans Odaklı",
        text: "JavaScript yerine CSS kullanarak daha performanslı animasyonlar elde edin.",
      },
      {
        title: "Kolay Kullanım",
        text: "Basit ve anlaşılır syntax ile hızlıca animasyonlar oluşturun.",
      },
    ],
  },
};
</script>

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

/* Demo Container Styles */
.demo-container {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.demo-header {
  padding: 1.5rem;
  background: #f8f9fa;
  border-bottom: 1px solid #e5e7eb;
}

.demo-header h4 {
  margin: 0 0 0.5rem 0;
  color: #1f2937;
}

.demo-header p {
  margin: 0;
  color: #6b7280;
}

.demo-content {
  height: 500px;
  overflow-y: auto;
  scroll-behavior: smooth;
  position: relative;
  container-type: inline-size;
  container-name: demo;
}

/* Progress Bar Demo */
.progress-demo .progress-container {
  position: sticky;
  top: 0;
  left: 0;
  width: 100%;
  height: 4px;
  background: #e5e7eb;
  z-index: 10;
}

.progress-demo .progress-bar {
  height: 100%;
  background: #3b82f6;
  transform-origin: 0 50%;
  animation: scroll-progress auto linear;
  animation-timeline: scroll(nearest block);
}

/* Content Blocks */
.content-block {
  padding: 2rem;
  border-bottom: 1px solid #e5e7eb;
}

.content-block:last-child {
  border-bottom: none;
}

.content-block h5 {
  margin: 0 0 1rem 0;
  color: #1f2937;
}

.content-block p {
  margin: 0;
  color: #4b5563;
  line-height: 1.6;
}

/* Paralaks Demo */
.parallax-demo .demo-content {
  height: 600px;
  overflow-y: auto;
  overflow-x: hidden;
  perspective: 1px;
  transform-style: preserve-3d;
}

.parallax-scene {
  position: relative;
  height: 400px;
  margin-bottom: 2rem;
  transform-style: preserve-3d;
  container-type: inline-size;
  container-name: parallax;
}

.parallax-bg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transform: translateZ(-1px) scale(2);
}

.parallax-bg img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.parallax-element {
  position: relative;
  transform-style: preserve-3d;
  z-index: 2;
}

.parallax-content {
  position: relative;
  padding: 2rem;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(8px);
  border-radius: 8px;
  color: white;
  text-align: center;
  animation: parallax-content auto linear;
  animation-timeline: scroll(nearest block);
  animation-range: entry 0% exit 100%;
}

/* Cards Demo */
.cards-demo .demo-content {
  height: 600px;
  padding: 2rem;
  perspective: 1000px;
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
}

.fade-element {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  opacity: 0;
  transform: translateY(50px) rotateX(20deg);
  view-timeline-name: --card;
  view-timeline-axis: block;
  animation: fade-in both linear;
  animation-timeline: --card;
  animation-range: entry 20% cover 40%;
}

.fade-element img {
  width: 100%;
  height: 180px;
  object-fit: cover;
}

.card-content {
  padding: 1.5rem;
}

/* Navbar Demo */
.navbar-demo .demo-content {
  height: 600px;
  overflow-y: auto;
}

.navbar-demo .navbar {
  position: sticky;
  top: 0;
  width: 100%;
  padding: 1rem;
  z-index: 10;
  animation: navbar-scroll both linear;
  animation-timeline: scroll(nearest block);
  animation-range: 0px 100px;
}

.navbar-demo .nav-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
}

.navbar-demo .logo {
  font-weight: bold;
  font-size: 1.5rem;
}

.navbar-demo .nav-links {
  display: flex;
  gap: 2rem;
}

.navbar-demo .nav-links a {
  color: inherit;
  text-decoration: none;
  transition: color 0.3s ease;
}

/* Keyframe Animations */
@keyframes scroll-progress {
  from {
    transform: scaleX(0);
  }
  to {
    transform: scaleX(1);
  }
}

@keyframes parallax-content {
  from {
    transform: translateY(0);
  }
  to {
    transform: translateY(calc(-30vh * var(--speed)));
  }
}

@keyframes fade-in {
  from {
    opacity: 0;
    transform: translateY(50px) rotateX(20deg);
  }
  to {
    opacity: 1;
    transform: translateY(0) rotateX(0);
  }
}

@keyframes navbar-scroll {
  from {
    background: transparent;
    backdrop-filter: none;
    transform: translateY(0);
  }
  to {
    background: rgba(255, 255, 255, 0.9);
    backdrop-filter: blur(8px);
    transform: translateY(-5px);
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  }
}

/* Responsive Adjustments */
@media (max-width: 768px) {
  .example-selector {
    flex-direction: column;
  }

  .nav-content {
    flex-direction: column;
    gap: 1rem;
  }

  .nav-links {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }

  .parallax-element h4 {
    font-size: 1.5rem;
    bottom: 1rem;
    left: 1rem;
  }
}

/* Video Bölümü Stilleri */
.video-section {
  margin-top: 3rem;
  padding-top: 2rem;
  border-top: 1px solid #e5e7eb;
}

.demo-video {
  width: 100%;
  max-width: 800px;
  margin: 1rem auto;
  display: block;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
</style>
