<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue'
import portrait from '../assets/purrity-art.png'
import fitWellThumbnail from '../assets/fitwell.png'
import marketPulseThumbnail from '../assets/market pulse.png'
import modernTechThumbnail from '../assets/modern tech.png'
import rentosphereThumbnail from '../assets/rentosphere.png'
import youthCodeAchievement from '../assets/Modern YouthCode Achievement Certificate.png'
import youthCodeExcellence from '../assets/YouthCode Project Certificate of Excellence (1).png'
import cybersecurityCertificatePreview from '../assets/cyber cert.png'
import dataScienceCertificatePreview from '../assets/Data science cert.png'
import cybersecurityCertificate from '../assets/Introduction_to_Cybersecurity_certificate_purritygwabeni43-gmail-com_3bab2a32-d610-4bd4-909e-62d9d6c8f545.pdf'
import dataScienceCertificate from '../assets/Introduction_to_Data_Science_certificate_purritygwabeni43-gmail-com_35f5a10e-b6ce-4c0e-b532-9d52f1f511d9.pdf'

const sections = [
  { id: 'home', label: 'Home', number: '01', icon: '⌂', building: 'SAFEHOUSE', color: 'mint' },
  { id: 'about', label: 'About', number: '02', icon: '◈', building: 'THE HANGOUT', color: 'peach' },
  { id: 'projects', label: 'Projects', number: '03', icon: '▣', building: 'WORKSHOP', color: 'blue' },
  { id: 'certifications', label: 'Certifications', number: '04', icon: '✦', building: 'THE ACADEMY', color: 'gold' },
  { id: 'contact', label: 'Contact', number: '05', icon: '✉', building: 'THE OFFICE', color: 'pink' },
]

const profile = {
  name: 'Purrity Gwabeni',
  role: 'Fullstack developer',
  tagline: 'I code what I see.',
  about:
    'I’m a fullstack developer who enjoys shaping clear ideas into useful, polished web experiences. I work across the stack with JavaScript, Python, and PHP, and love learning the tools that make good products feel effortless.',
}

const skills = ['HTML', 'CSS', 'JavaScript', 'Vue.js', 'Python', 'PHP', 'Node.js', 'MySQL', 'Git']

const continents = [
  { name: 'Africa', code: 'africa' },
  { name: 'Asia', code: 'asia' },
  { name: 'Europe', code: 'europe' },
  { name: 'North America', code: 'north-america' },
  { name: 'South America', code: 'south-america' },
  { name: 'Oceania', code: 'oceania' },
  { name: 'Antarctica', code: 'antarctica' },
]

const countryCodesByContinent = {
  Africa:
    'DZ AO BJ BW BF BI CV CM CF TD KM CG CD CI DJ EG GQ ER SZ ET GA GM GH GN GW KE LS LR LY MG MW ML MR MU MA MZ NA NE NG RW ST SN SC SL SO ZA SS SD TZ TG TN UG EH ZM ZW',
  Asia:
    'AF AM AZ BH BD BT BN KH CN CY GE IN ID IR IQ IL JP JO KZ KW KG LA LB MY MV MN MM NP KP OM PK PS PH QA SA SG KR LK SY TW TJ TH TL TR TM AE UZ VN YE',
  Europe:
    'AX AL AD AT BY BE BA BG HR CZ DK EE FO FI FR DE GI GR GG HU IS IE IM IT JE XK LV LI LT LU MT MD MC ME NL MK NO PL PT RO RU SM RS SK SI ES SJ SE CH UA GB VA',
  'North America':
    'AI AG AW BS BB BZ BM BQ VG CA KY CR CU CW DM DO SV GL GD GP GT HT HN JM MQ MX MS NI PA PR BL KN LC MF PM VC SX TT TC VI US',
  'South America': 'AR BO BR CL CO EC FK GF GY PY PE SR UY VE',
  Oceania:
    'AS AU CX CC CK FJ PF GU KI MH FM NR NC NZ NU NF MP PW PG PN WS SB TK TO TV UM VU WF',
  Antarctica: '',
}

const countryNames = new Intl.DisplayNames(['en'], { type: 'region' })
const countriesByContinent = Object.fromEntries(
  Object.entries(countryCodesByContinent).map(([continent, codes]) => [
    continent,
    (codes ? codes.split(' ') : [])
      .map((code) => ({ code, name: countryNames.of(code) }))
      .filter((country) => country.name && country.name !== country.code)
      .sort((first, second) => first.name.localeCompare(second.name)),
  ]),
)
const countries = [...['ZA', 'US', 'GB', 'JP', 'AU'].map((code) => {
  const continent = Object.entries(countryCodesByContinent).find(([, codes]) =>
    codes.split(' ').includes(code),
  )?.[0]

  return { code, name: countryNames.of(code), continent }
}), { code: 'AQ', name: 'Antarctica', continent: 'Antarctica' }]

const worldThemes = {
  Africa: { accent: '#e9ad55', skyTop: '#28372d', skyMid: '#6b6241', horizon: '#c48c58', ground: '#a56a4a', building: '#bc8863', glow: 'rgba(255,205,118,.42)' },
  Asia: { accent: '#f08d6c', skyTop: '#282b45', skyMid: '#62536a', horizon: '#c87c69', ground: '#985b55', building: '#b77d76', glow: 'rgba(255,160,131,.4)' },
  Europe: { accent: '#87b8ef', skyTop: '#1d3042', skyMid: '#45687a', horizon: '#9a9fa0', ground: '#777f86', building: '#96989a', glow: 'rgba(160,208,255,.38)' },
  'North America': { accent: '#65c9be', skyTop: '#19323c', skyMid: '#397174', horizon: '#c38368', ground: '#a25f51', building: '#b98368', glow: 'rgba(118,237,215,.38)' },
  'South America': { accent: '#8bcf86', skyTop: '#193a38', skyMid: '#3c7861', horizon: '#d29a62', ground: '#9d674a', building: '#b98561', glow: 'rgba(169,238,133,.38)' },
  Oceania: { accent: '#68c7d3', skyTop: '#193743', skyMid: '#377988', horizon: '#e0a77a', ground: '#b26f59', building: '#c08b6b', glow: 'rgba(127,231,240,.4)' },
  Antarctica: { accent: '#a9c9f2', skyTop: '#1a2e45', skyMid: '#577c96', horizon: '#b3c8d1', ground: '#738fa4', building: '#a3b6c1', glow: 'rgba(190,229,255,.45)' },
}

const projects = [
  {
    name: 'FitWell',
    type: 'FITNESS COACHING WEBSITE',
    year: 'LIVE',
    tone: 'project-sunset',
    image: fitWellThumbnail,
    mark: '01',
    description: 'A fitness coaching website for training, nutrition, and wellness support.',
    link: 'https://fittwell-coach.netlify.app/',
  },
  {
    name: 'Market Pulse',
    type: 'PRICE MONITORING DASHBOARD',
    year: 'LIVE',
    tone: 'project-violet',
    image: marketPulseThumbnail,
    mark: '02',
    description: 'A dashboard for monitoring retail goods and digital asset prices.',
    link: 'https://market-pulse-11gq.onrender.com/',
  },
  {
    name: 'ModernTech HR Portal',
    type: 'HR OPERATIONS DASHBOARD',
    year: 'LIVE',
    tone: 'project-cyan',
    image: modernTechThumbnail,
    mark: '03',
    description: 'An HR dashboard for employee operations, attendance, and performance.',
    link: 'https://modern-tech-solutions00.netlify.app/',
  },
  {
    name: 'Rentosphere',
    type: 'RENTAL MARKETPLACE',
    year: 'LIVE',
    tone: 'project-sunset',
    image: rentosphereThumbnail,
    mark: '04',
    description: 'A rental marketplace for tools, trailers, appliances, and event equipment.',
    link: 'https://e-commerce-project-ca0q.onrender.com/',
  },
]

const certificates = [
  {
    name: 'YouthCode Software Development Project',
    issuer: 'Life Choices Academy · YouthCode',
    year: '2 OCT 2026',
    mark: '01',
    image: youthCodeAchievement,
    link: youthCodeAchievement,
  },
  {
    name: 'Coding Wizard — Certificate of Excellence',
    issuer: 'YouthCode Project',
    year: '2 OCT 2026',
    mark: '02',
    image: youthCodeExcellence,
    link: youthCodeExcellence,
  },
  {
    name: 'Introduction to Cybersecurity',
    issuer: 'Cisco Networking Academy',
    year: '31 AUG 2026',
    mark: '03',
    image: cybersecurityCertificatePreview,
    link: cybersecurityCertificate,
  },
  {
    name: 'Introduction to Data Science',
    issuer: 'Cisco Networking Academy',
    year: '6 AUG 2026',
    mark: '04',
    image: dataScienceCertificatePreview,
    link: dataScienceCertificate,
  },
]

const contactLinks = [
  { label: 'GitHub', href: 'https://github.com/purritygwabeni43-tech' },
  { label: 'LinkedIn', href: 'https://www.linkedin.com/in/purrity-gwabeni-24124a403' },
  { label: 'Email', href: 'mailto:purritygwabeni43@gmail.com' },
]
const email = 'purritygwabeni43@gmail.com'

const isLoading = ref(true)
const activeSection = ref('home')
const activeCertificate = ref(0)
const worldPickerOpen = ref(false)
const selectedContinent = ref('North America')
const selectedCountryCode = ref('')
const activePanel = computed(() => sections.find((section) => section.id === activeSection.value))
const activeCertificateData = computed(() => certificates[activeCertificate.value])
const selectedCountry = computed(() => countries.find((country) => country.code === selectedCountryCode.value))
const currentLocation = computed(() => selectedCountry.value?.name || selectedContinent.value)
const selectedContinentCode = computed(
  () => continents.find((continent) => continent.name === selectedContinent.value).code,
)
const worldThemeStyle = computed(() => ({
  '--gold': worldThemes[selectedContinent.value].accent,
  '--theme-sky-top': worldThemes[selectedContinent.value].skyTop,
  '--theme-sky-mid': worldThemes[selectedContinent.value].skyMid,
  '--theme-horizon': worldThemes[selectedContinent.value].horizon,
  '--theme-ground': worldThemes[selectedContinent.value].ground,
  '--theme-building': worldThemes[selectedContinent.value].building,
  '--theme-glow': worldThemes[selectedContinent.value].glow,
}))

function chooseCountry(code) {
  selectedCountryCode.value = code
  const country = countries.find((item) => item.code === code)
  if (country) selectedContinent.value = country.continent
}

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
  if (worldPickerOpen.value) return

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
  <main class="portfolio-shell" :style="worldThemeStyle">
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

      <div class="map-label map-label-west">{{ selectedCountry ? selectedContinent.toUpperCase() : 'VICE CITY' }}<br /><small>{{ selectedCountry ? 'WORLD TOUR' : 'EST. ALWAYS IN MOTION' }}</small></div>
      <div class="map-label map-label-east">{{ selectedCountry ? currentLocation.toUpperCase() : 'OCEAN DRIVE' }} <span>→</span></div>
      <button
        v-if="activeSection === 'home'"
        class="world-picker-trigger"
        type="button"
        aria-controls="world-picker"
        :aria-expanded="worldPickerOpen"
        :aria-label="`Choose a world location. Current location: ${currentLocation}`"
        @click="worldPickerOpen = true"
      >
        <svg viewBox="0 0 24 24" aria-hidden="true"><circle cx="12" cy="12" r="9" /><path d="M3 12h18M12 3a15 15 0 0 1 0 18M12 3a15 15 0 0 0 0 18M5.6 6.6h12.8M5.6 17.4h12.8" /></svg>
        <span>CHANGE LOCATION</span>
      </button>

      <div class="city-ground" :class="`architecture-${selectedContinentCode}`">
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
          <span v-if="selectedContinentCode === 'antarctica'" class="building-igloo" aria-hidden="true">
            <span class="igloo-entrance"></span>
          </span>
          <span class="building-caption"><b>{{ section.number }}</b> {{ section.label }}</span>
        </button>

        <div class="walkway"></div>
        <div class="player" :class="{ 'player-walking': !isLoading }" :style="{ '--player-position': `${10 + sections.findIndex((section) => section.id === activeSection) * 20}%` }">
          <img :src="portrait" alt="" />
          <span class="player-shadow"></span>
        </div>
        <div class="player-tag" :style="{ left: `${10 + sections.findIndex((section) => section.id === activeSection) * 20}%` }">YOU ARE HERE</div>
      </div>

      <div class="map-coordinates"><span>{{ selectedCountry ? `WORLD / ${selectedContinent.toUpperCase()}` : 'MAP 01' }}</span><span>{{ selectedCountry ? currentLocation.toUpperCase() : "26° 07' N  80° 08' W" }}</span></div>

      <aside class="mission-card" aria-label="Current portfolio section">
        <div class="mission-topline"><span>NOW EXPLORING</span></div>
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
              <p class="about-role">{{ profile.role }}</p>
              <p>{{ profile.about }}</p>
              <div class="skill-list"><span v-for="skill in skills" :key="skill">{{ skill }}</span></div>
            </div>
          </div>
        </template>

        <template v-else-if="activeSection === 'projects'">
          <span class="panel-kicker">SELECTED SIDE QUESTS</span>
          <h2>Made with <em>purpose.</em></h2>
          <div class="project-grid">
            <a v-for="project in projects" :key="project.name" class="project-card" :href="project.link" target="_blank" rel="noopener noreferrer">
              <div class="project-art" :class="project.tone"><img :src="project.image" :alt="`${project.name} website thumbnail`" loading="lazy" /><span>{{ project.mark }}</span><i>↗</i></div>
              <div class="project-meta"><div><h3>{{ project.name }}</h3><span>{{ project.type }}</span></div><small>{{ project.year }}</small></div>
              <p>{{ project.description }}</p>
            </a>
          </div>
        </template>

        <template v-else-if="activeSection === 'certifications'">
          <span class="panel-kicker">SKILLS UNLOCKED</span>
          <h2>Proof of <em>progress.</em></h2>
          <div class="certificate-carousel">
            <button class="carousel-arrow" type="button" aria-label="Previous certificate" @click="changeCertificate(-1)">←</button>
            <a
              class="certificate-card"
              :href="activeCertificateData.link"
              :aria-label="`Open ${activeCertificateData.name} certificate in a new tab`"
              target="_blank"
              rel="noopener noreferrer"
            >
              <div class="certificate-stamp">PG<br /><span>VERIFIED</span></div>
              <div class="certificate-info">
                <span class="certificate-overline">CERTIFICATE {{ activeCertificateData.mark }} / 0{{ certificates.length }}</span>
                <h3>{{ activeCertificateData.name }}</h3>
                <p>{{ activeCertificateData.issuer }}</p>
                <span class="certificate-year">ISSUED {{ activeCertificateData.year }}</span>
                <span class="certificate-open-label">HOVER TO PREVIEW · CLICK TO OPEN</span>
              </div>
              <div class="certificate-ribbon">✦</div>
              <img class="certificate-preview" :src="activeCertificateData.image" alt="" aria-hidden="true" />
            </a>
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
          <a class="contact-cta" :href="`mailto:${email}`">GET IN TOUCH <span>↗</span></a>
          <div class="contact-details">
            <span>TECH STACK <b>{{ skills.join(' · ') }}</b></span>
            <span>FIND ME <b><a v-for="(link, index) in contactLinks" :key="link.label" :href="link.href" target="_blank" rel="noopener noreferrer">{{ index ? ' · ' : '' }}{{ link.label }}</a></b></span>
          </div>
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

    <footer class="site-footer">
      <span>© {{ new Date().getFullYear() }} PURRITY GWABENI</span>
      <nav class="footer-socials" aria-label="Social links">
        <a
          v-for="link in contactLinks"
          :key="link.label"
          :href="link.href"
          :aria-label="link.label"
          :title="link.label"
          :target="link.label === 'Email' ? undefined : '_blank'"
          :rel="link.label === 'Email' ? undefined : 'noopener noreferrer'"
        >
          <svg v-if="link.label === 'GitHub'" viewBox="0 0 24 24" aria-hidden="true">
            <path fill="currentColor" d="M12 .9a11.1 11.1 0 0 0-3.51 21.63c.55.1.76-.24.76-.53v-2.08c-3.1.68-3.76-1.32-3.76-1.32-.5-1.3-1.24-1.65-1.24-1.65-1.02-.7.08-.69.08-.69 1.13.08 1.72 1.16 1.72 1.16 1 .1.86 2.36 3.43 1.67.1-.72.4-1.22.72-1.5-2.48-.28-5.08-1.24-5.08-5.52 0-1.22.44-2.22 1.16-3-.12-.29-.5-1.43.11-2.98 0 0 .95-.3 3.05 1.15a10.6 10.6 0 0 1 5.55 0c2.1-1.44 3.04-1.15 3.04-1.15.61 1.55.23 2.69.12 2.98.72.78 1.15 1.78 1.15 3 0 4.3-2.6 5.23-5.1 5.51.4.35.76 1.02.76 2.06V22c0 .3.2.64.77.53A11.1 11.1 0 0 0 12 .9Z"/>
          </svg>
          <svg v-else-if="link.label === 'LinkedIn'" viewBox="0 0 24 24" aria-hidden="true">
            <path fill="currentColor" d="M20.45 2H3.55C2.69 2 2 2.68 2 3.52v16.96c0 .84.69 1.52 1.55 1.52h16.9c.86 0 1.55-.68 1.55-1.52V3.52c0-.84-.69-1.52-1.55-1.52ZM7.93 18.34H4.96V9.5h2.97v8.84ZM6.44 8.3a1.72 1.72 0 1 1 0-3.44 1.72 1.72 0 0 1 0 3.44Zm11.9 10.04h-2.96v-4.3c0-1.03-.02-2.35-1.43-2.35-1.43 0-1.65 1.12-1.65 2.28v4.37H9.34V9.5h2.84v1.21h.04c.4-.7 1.36-1.43 2.8-1.43 3 0 3.56 1.98 3.56 4.56v4.5Z"/>
          </svg>
          <svg v-else viewBox="0 0 24 24" aria-hidden="true">
            <path fill="currentColor" d="M3.5 5h17A1.5 1.5 0 0 1 22 6.5v11a1.5 1.5 0 0 1-1.5 1.5h-17A1.5 1.5 0 0 1 2 17.5v-11A1.5 1.5 0 0 1 3.5 5Zm8.5 7.1L19.1 7H4.9l7.1 5.1Zm0 2.1L4 8.5v9.1h16V8.5l-8 5.7Z"/>
          </svg>
        </a>
      </nav>
      <span class="footer-credit">MADE WITH A LITTLE VICE CITY MAGIC <b>✦</b></span>
      <span class="footer-hint">PRESS <kbd>↑</kbd><kbd>↓</kbd> TO EXPLORE</span>
    </footer>

    <div
      v-if="worldPickerOpen"
      id="world-picker"
      class="world-picker-backdrop"
      @keydown.esc="worldPickerOpen = false"
    >
      <section class="world-picker-dialog" role="dialog" aria-modal="true" aria-labelledby="world-picker-title">
        <button class="world-picker-close" type="button" aria-label="Close world selector" @click="worldPickerOpen = false">×</button>
        <div class="globe-art" aria-hidden="true">
          <svg viewBox="0 0 240 240">
            <defs><clipPath id="globe-clip"><circle cx="120" cy="120" r="105" /></clipPath></defs>
            <circle class="globe-ocean" cx="120" cy="120" r="105" />
            <g class="globe-grid" clip-path="url(#globe-clip)">
              <ellipse cx="120" cy="120" rx="45" ry="105" />
              <ellipse cx="120" cy="120" rx="82" ry="105" />
              <ellipse cx="120" cy="120" rx="105" ry="43" />
              <ellipse cx="120" cy="120" rx="105" ry="76" />
              <path d="M15 120h210" />
              <path class="globe-land" d="m35 64 18-18 24-5 13 9 17-5 12 11-8 14-17 5-7 13-15 5-8 16-15-4-8-16-12-6-7-12Zm53 46 15 5 7 16-7 21-11 16-8-15 4-20-7-11Zm47-57 15-10 15 3 9 13 19 5 7 13-11 10-14-4-10 8-13-4-7 12-12-2-10-14 4-15-10-8Zm25 55 20-7 15 7 6 16-13 14-17-3-12-12Zm-5 37 14 7 6 16-12 8-12-12Zm-68-97 7-12 11-2 6 8-5 10-11 4Zm-57 91 19 2 9 10-6 12-17 2-9-12Z" />
            </g>
            <circle class="globe-highlight" cx="120" cy="120" r="105" />
          </svg>
          <span class="globe-orbit"></span>
          <span class="globe-caption">THE WORLD IS YOURS</span>
        </div>
        <div class="world-picker-content">
          <span class="panel-kicker">PICK YOUR NEXT DESTINATION</span>
          <h2 id="world-picker-title">Explore the <em>world.</em></h2>
          <p class="world-picker-copy">Choose a country to transform the city colors and map theme.</p>
          <div class="country-grid" aria-label="Choose a country">
            <button
              v-for="country in countries"
              :key="country.code"
              type="button"
              :class="{ 'is-selected': selectedCountryCode === country.code }"
              :aria-pressed="selectedCountryCode === country.code"
              @click="chooseCountry(country.code)"
            >{{ country.name }}</button>
          </div>
          <div class="world-picker-footer">
            <span>CURRENT DESTINATION: <b>{{ currentLocation.toUpperCase() }}</b></span>
            <button type="button" @click="worldPickerOpen = false">SET DESTINATION <span>↗</span></button>
          </div>
        </div>
      </section>
    </div>

    <div v-if="isLoading" class="loading-screen">
      <div class="loading-grain"></div>
      <div class="loading-content">
        <div class="loading-main">
          <div class="loading-portrait-frame">
            <img :src="portrait" alt="Illustrated portrait of Purrity Gwabeni" />
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
