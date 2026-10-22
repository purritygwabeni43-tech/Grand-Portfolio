<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue'
import portrait from '../assets/purrity-art.png'

const sections = [
  { id: 'home', label: 'Home', number: '01', icon: '⌂', building: 'SAFEHOUSE', color: 'mint' },
  { id: 'about', label: 'About', number: '02', icon: '◈', building: 'THE HANGOUT', color: 'peach' },
  { id: 'projects', label: 'Projects', number: '03', icon: '▣', building: 'WORKSHOP', color: 'blue' },
  { id: 'certifications', label: 'Certifications', number: '04', icon: '✦', building: 'THE ACADEMY', color: 'gold' },
  { id: 'contact', label: 'Contact', number: '05', icon: '✉', building: 'THE OFFICE', color: 'pink' },
]

const profile = {
  name: 'Purrity Gwabeni',
  role: 'Creative developer',
  location: 'Your city, your timezone',
  tagline: 'Building things worth exploring.',
  about:
    'I’m a curious creative who loves turning bold ideas into thoughtful digital experiences. This is my little corner of the city — take a look around.',
}

const projects = [
  { name: 'Project One', type: 'WEB EXPERIENCE', year: '2025', tone: 'project-sunset', mark: '01' },
  { name: 'Project Two', type: 'CREATIVE DEVELOPMENT', year: '2025', tone: 'project-violet', mark: '02' },
  { name: 'Project Three', type: 'DIGITAL PRODUCT', year: '2024', tone: 'project-cyan', mark: '03' },
]

const certificates = [
  { name: 'Certificate One', issuer: 'Add issuing organization', year: '2025', mark: '01' },
  { name: 'Certificate Two', issuer: 'Add issuing organization', year: '2024', mark: '02' },
  { name: 'Certificate Three', issuer: 'Add issuing organization', year: '2024', mark: '03' },
]

const isLoading = ref(true)
const activeSection = ref('home')
const activeCertificate = ref(0)
const activePanel = computed(() => sections.find((section) => section.id === activeSection.value))
const activeCertificateData = computed(() => certificates[activeCertificate.value])

function enterCity() {
  isLoading.value = false
}

function returnToLobby() {
  activeSection.value = 'home'
  isLoading.value = true
}

function visitSection(id) {
  activeSection.value = id
  isLoading.value = false
}

function changeCertificate(direction) {
  activeCertificate.value =
    (activeCertificate.value + direction + certificates.length) % certificates.length
}

function handleMapKeys(event) {
  if (isLoading.value && event.key === 'Enter') {
    event.preventDefault()
    enterCity()
    return
  }

  if (isLoading.value || !['ArrowDown', 'ArrowRight', 'ArrowUp', 'ArrowLeft'].includes(event.key)) return

  event.preventDefault()
  const direction = ['ArrowDown', 'ArrowRight'].includes(event.key) ? 1 : -1
  const currentIndex = sections.findIndex((section) => section.id === activeSection.value)
  const nextIndex = (currentIndex + direction + sections.length) % sections.length
  activeSection.value = sections[nextIndex].id
}

onMounted(() => document.addEventListener('keydown', handleMapKeys))
onUnmounted(() => document.removeEventListener('keydown', handleMapKeys))
</script>

<template>
  <main class="portfolio-shell">
    <header class="topbar">
      <a class="brand" href="#" aria-label="Purrity Gwabeni portfolio home" @click.prevent="visitSection('home')">
        <span class="brand-mark">PG</span>
        <span class="brand-name">Purrity <b>Gwabeni</b></span>
      </a>

      <nav class="top-nav" aria-label="Portfolio sections">
        <button
          v-for="section in sections"
          :key="section.id"
          class="nav-link"
          :class="{ 'is-current': activeSection === section.id }"
          type="button"
          @click="visitSection(section.id)"
        >
          {{ section.label }}
        </button>
      </nav>

      <div class="topbar-actions">
        <a class="availability" href="#contact" @click.prevent="visitSection('contact')">
          <span class="availability-dot"></span>
          AVAILABLE FOR WORK
        </a>
        <button class="lobby-button" type="button" aria-label="Return to lobby loading screen" title="Return to lobby" @click="returnToLobby">
          <span aria-hidden="true">↩</span> <span class="lobby-label">LOBBY</span>
        </button>
      </div>
    </header>

    <section class="city-screen" aria-label="Portfolio city map">
      <div class="city-sky"></div>
      <div class="city-sun"></div>
      <div class="city-horizon">
        <span v-for="n in 14" :key="n" class="horizon-block" :class="`horizon-block-${n}`"></span>
      </div>

      <div class="city-headline">
        <span class="eyebrow"><span class="eyebrow-line"></span> WELCOME TO MY WORLD</span>
        <h1>One city.<br /><span>Many stories.</span></h1>
        <p>Pick a building, take a look around.<br />Your next favorite thing is just around the corner.</p>
      </div>

      <div class="map-label map-label-west">VICE CITY<br /><small>EST. ALWAYS IN MOTION</small></div>
      <div class="map-label map-label-east">OCEAN DRIVE <span>→</span></div>

      <div class="city-ground">
        <div class="street street-back"></div>
        <div class="street street-front"></div>
        <div class="street-line"></div>
        <div class="sidewalk sidewalk-back"></div>
        <div class="sidewalk sidewalk-front"></div>
        <div class="palm palm-one"><i></i></div>
        <div class="palm palm-two"><i></i></div>
        <div class="palm palm-three"><i></i></div>

        <button
          v-for="(section, index) in sections"
          :key="section.id"
          class="building"
          :class="[`building-${section.color}`, `building-${index + 1}`, { 'building-active': activeSection === section.id }]"
          type="button"
          :aria-label="`Enter ${section.label} section`"
          @click="visitSection(section.id)"
        >
          <span class="building-sign">{{ section.building }}</span>
          <span class="building-roof"></span>
          <span class="building-body">
            <span v-for="n in 6" :key="n" class="building-window"></span>
            <span class="building-door"></span>
          </span>
          <span class="building-caption"><b>{{ section.number }}</b> {{ section.label }}</span>
        </button>

        <div class="walkway"></div>
        <div class="player" :class="{ 'player-walking': !isLoading }" :style="{ '--player-position': `${10 + sections.findIndex((section) => section.id === activeSection) * 20}%` }">
          <img :src="portrait" alt="" />
          <span class="player-shadow"></span>
        </div>
        <div class="player-tag" :style="{ left: `${10 + sections.findIndex((section) => section.id === activeSection) * 20}%` }"><span class="tag-dot"></span> YOU ARE HERE</div>
      </div>

      <div class="map-coordinates"><span>MAP 01</span><span>26° 07' N&nbsp; 80° 08' W</span></div>

      <aside class="mission-card" aria-label="Current portfolio section">
        <div class="mission-topline"><span>NOW EXPLORING</span><span class="mission-signal">● LIVE</span></div>
        <div class="mission-content">
          <span class="mission-icon">{{ activePanel.icon }}</span>
          <div><small>{{ activePanel.number }} / 05</small><strong>{{ activePanel.label }}</strong></div>
        </div>
        <div class="mission-progress"><span :style="{ width: `${(Number(activePanel.number) / 5) * 100}%` }"></span></div>
        <div class="mission-foot"><span>SELECT A BUILDING</span><span>↗</span></div>
      </aside>

      <div class="scroll-hint"><span class="scroll-mouse"></span> SCROLL TO EXPLORE</div>

      <section v-if="activeSection !== 'home'" class="section-panel" :key="activeSection" aria-live="polite">
        <button class="panel-close" type="button" aria-label="Return to city map" @click="visitSection('home')">×</button>
        <template v-if="activeSection === 'about'">
          <span class="panel-kicker">THE PERSON BEHIND THE CONTROLS</span>
          <h2>A little about <em>me.</em></h2>
          <div class="about-panel-body">
            <img :src="portrait" alt="Illustrated portrait of Purrity Gwabeni" />
            <div>
              <h3>{{ profile.name }}</h3>
              <p class="about-role">{{ profile.role }} <span>·</span> {{ profile.location }}</p>
              <p>{{ profile.about }}</p>
              <div class="skill-list"><span>DESIGN</span><span>DEVELOPMENT</span><span>IDEAS</span></div>
            </div>
          </div>
        </template>

        <template v-else-if="activeSection === 'projects'">
          <span class="panel-kicker">SELECTED SIDE QUESTS</span>
          <h2>Made with <em>purpose.</em></h2>
          <div class="project-grid">
            <article v-for="project in projects" :key="project.name" class="project-card">
              <div class="project-art" :class="project.tone"><span>{{ project.mark }}</span><b>YOUR<br />PROJECT</b><i>↗</i></div>
              <div class="project-meta"><div><h3>{{ project.name }}</h3><span>{{ project.type }}</span></div><small>{{ project.year }}</small></div>
              <p>Add a short description and link to this project.</p>
            </article>
          </div>
        </template>

        <template v-else-if="activeSection === 'certifications'">
          <span class="panel-kicker">SKILLS UNLOCKED</span>
          <h2>Proof of <em>progress.</em></h2>
          <div class="certificate-carousel">
            <button class="carousel-arrow" type="button" aria-label="Previous certificate" @click="changeCertificate(-1)">←</button>
            <article class="certificate-card">
              <div class="certificate-stamp">PG<br /><span>VERIFIED</span></div>
              <div class="certificate-info">
                <span class="certificate-overline">CERTIFICATE {{ activeCertificateData.mark }} / 0{{ certificates.length }}</span>
                <h3>{{ activeCertificateData.name }}</h3>
                <p>{{ activeCertificateData.issuer }}</p>
                <span class="certificate-year">ISSUED {{ activeCertificateData.year }}</span>
              </div>
              <div class="certificate-ribbon">✦</div>
            </article>
            <button class="carousel-arrow" type="button" aria-label="Next certificate" @click="changeCertificate(1)">→</button>
          </div>
          <div class="carousel-dots" aria-label="Certificate position">
            <button v-for="(certificate, index) in certificates" :key="certificate.name" :aria-label="`Show certificate ${index + 1}`" :class="{ active: activeCertificate === index }" type="button" @click="activeCertificate = index"></button>
          </div>
        </template>

        <template v-else>
          <span class="panel-kicker">YOUR NEXT MISSION STARTS HERE</span>
          <h2>Let’s make something <em>great.</em></h2>
          <p class="contact-copy">Have a project in mind, an opportunity, or just want to say hello? My inbox is open.</p>
          <a class="contact-cta" href="mailto:your.email@example.com">GET IN TOUCH <span>↗</span></a>
          <p class="contact-note">Remember to update the email address before publishing.</p>
          <div class="contact-details"><span>BASED IN <b>{{ profile.location }}</b></span><span>STATUS <b><i></i> OPEN TO OPPORTUNITIES</b></span></div>
        </template>
      </section>
    </section>

    <section class="home-strip" aria-label="Introduction">
      <div class="home-intro">
        <span class="strip-label">PLAYER ONE <i></i> READY</span>
        <h2>{{ profile.tagline }}</h2>
        <p>Hey, I’m <strong>{{ profile.name }}</strong> — welcome to my portfolio.</p>
      </div>
      <div class="strip-divider"></div>
      <div class="strip-stat"><span>01</span><b>CURIOUS<br />BY NATURE</b></div>
      <div class="strip-stat"><span>02</span><b>MAKING<br />GOOD THINGS</b></div>
      <button class="start-exploring" type="button" @click="visitSection('projects')"><span>START EXPLORING</span><b>↗</b></button>
    </section>

    <footer class="site-footer"><span>© {{ new Date().getFullYear() }} PURRITY GWABENI</span><span>MADE WITH A LITTLE VICE CITY MAGIC <b>✦</b></span><span>PRESS <kbd>↑</kbd><kbd>↓</kbd> TO EXPLORE</span></footer>

    <div v-if="isLoading" class="loading-screen">
      <div class="loading-grain"></div>
      <div class="loading-content">
        <div class="loading-main">
          <div class="loading-portrait-frame">
            <img :src="portrait" alt="Illustrated portrait of Purrity Ginaben" />
            <span>LOADING SCREEN ART</span>
          </div>
          <div class="loading-copy">
            <span class="loading-overline">SAN FIERRO · LOS SANTOS · PORTFOLIO</span>
            <h2>Grand<br /><em>Portfolio</em></h2>
            <h3>{{ profile.name }}</h3>
            <p>{{ profile.role }} — welcome to the neighbourhood. Walk the streets,<br class="desktop-break" /> step into every building and explore the work.</p>
            <div class="loading-progress" role="progressbar" aria-label="Loading progress" aria-valuemin="0" aria-valuemax="100" aria-valuenow="100"><span></span></div>
            <div class="loading-status"><em>The radar in the corner tracks every district.</em><span>100%</span></div>
            <button class="enter-button" type="button" @click="enterCity"><span class="play-icon">▷</span><span>Press Enter / Start Game</span></button>
          </div>
        </div>
        <div class="loading-bottom"><span>© {{ new Date().getFullYear() }} PURRITY GWABENI</span><span>EXPLORE THE CITY · DISCOVER WHAT’S POSSIBLE</span><span>ALL SYSTEMS READY</span></div>
      </div>
    </div>
  </main>
</template>
