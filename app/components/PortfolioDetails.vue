<script setup lang="ts">
import { onBeforeUnmount, onMounted, reactive, ref } from 'vue'

const contributionChartFailed = ref(false)
const githubChartViewport = ref<HTMLElement | null>(null)
let githubChartResizeObserver: ResizeObserver | undefined

function showRecentGitHubActivity() {
  const viewport = githubChartViewport.value
  if (viewport) viewport.scrollLeft = Math.max(0, viewport.scrollWidth - viewport.clientWidth)
}
const skillGroups = [
  {
    title: 'Web development',
    description: 'Interfaces, application logic, and databases.',
    skills: [
      'HTML',
      'CSS',
      'JavaScript',
      'TypeScript',
      'Bootstrap',
      'Tailwind CSS',
      'Three.js',
      'GSAP',
      'Vite',
      'Vue',
      'Nuxt',
      'React',
      'Next.js',
      'PHP',
      'Laravel',
      'Node.js',
      'MySQL',
      'PostgreSQL',
      'MongoDB',
      'Supabase',
    ],
  },
  {
    title: 'Mobile & AI',
    description: 'Mobile experiences and image-analysis experiments.',
    skills: [
      'Flutter',
      'React Native',
      'Capacitor',
      'Dart',
      'Firebase',
      'Python',
      'Flask',
      'PyTorch',
      'Keras',
      'OpenCV',
      'TensorFlow',
    ],
  },
  {
    title: 'Foundations & tools',
    description: 'The tools behind the development process.',
    skills: [
      'Java',
      'Git',
      'GitHub',
      'VS Code',
      'Docker',
      'Postman',
      'Figma',
      'Vercel',
      'Cisco Networking',
    ],
  },
]

const activeSkillGroup = ref(0)
const mobileToolkit = ref(false)
let toolkitMedia: MediaQueryList | undefined
function updateToolkitViewport() {
  mobileToolkit.value = toolkitMedia?.matches ?? false
}
const toolkitKeyboardNavigation = ref(false)
const skillTabLabels = ['Web', 'Mobile & AI', 'Tools']

function selectSkillGroup(index: number, event: MouseEvent) {
  toolkitKeyboardNavigation.value = event.detail === 0
  activeSkillGroup.value = index
}

function handleSkillTabKey(event: KeyboardEvent, index: number) {
  let nextIndex: number
  switch (event.key) {
    case 'ArrowRight': nextIndex = (index + 1) % skillGroups.length; break
    case 'ArrowLeft': nextIndex = (index + skillGroups.length - 1) % skillGroups.length; break
    case 'Home': nextIndex = 0; break
    case 'End': nextIndex = skillGroups.length - 1; break
    default: return
  }
  event.preventDefault()
  toolkitKeyboardNavigation.value = true
  activeSkillGroup.value = nextIndex
  const tab = event.currentTarget as HTMLButtonElement
  tab.parentElement?.querySelectorAll<HTMLButtonElement>('[role="tab"]')[nextIndex]?.focus()
}

const skillColors: Record<string, string> = {
  HTML: '#e34f26', CSS: '#1572b6', JavaScript: '#d6b800', TypeScript: '#3178c6',
  Bootstrap: '#7952b3', 'Tailwind CSS': '#06b6d4', 'Three.js': 'var(--text)', GSAP: '#88ce02',
  Vite: '#a66bff', Vue: '#42b883', Nuxt: '#00b97d', React: '#27a9c6', 'Next.js': 'var(--text)',
  PHP: '#8993be', Laravel: '#ff2d20', 'Node.js': '#5fa04e', MySQL: '#1685a9',
  PostgreSQL: '#487ca8', MongoDB: '#47a248', Supabase: '#3ecf8e', Flutter: '#42a5f5',
  'React Native': '#27a9c6', Capacitor: '#53b9ff', Dart: '#0175c2', Firebase: '#f5a800',
  Python: '#4b8bbe', Flask: 'var(--text)', PyTorch: '#ee4c2c', Keras: '#d00000',
  OpenCV: '#df5656', TensorFlow: '#ff8f00', Java: '#e07628',
  Git: '#f05032', GitHub: 'var(--text)', 'VS Code': '#168bd2', Docker: '#2496ed',
  Postman: '#ff6c37', Figma: '#a66bff', Vercel: 'var(--text)', 'Cisco Networking': '#049fd9',
}

/* ========================================
  CERTIFICATIONS
======================================== */

const certifications = [
  {
    title: 'CCNA: Introduction to Networks',
    issuer: 'Davao del Norte State College · Cisco Networking Academy',
    date: 'May 2025',
  },
  {
    title: 'CCNA: Switching, Routing, and Wireless Essentials',
    issuer: 'Davao del Norte State College · Cisco Networking Academy',
    date: 'Dec 2025',
  },
]

/* ========================================
  TECH STACK ICONS
======================================== */

const techIconBase = 'https://cdn.jsdelivr.net/npm/simple-icons@latest/icons'

// Map each stack label to its Simple Icons slug.
// No public brand mark for a tool? Leave it out — it falls back to initials.
const techIconSlugs: Record<string, string> = {
  Python: 'python',
  Capacitor: 'capacitor',
  Dart: 'dart',
  'Tailwind CSS': 'tailwindcss',
  'Three.js': 'threedotjs',
  GSAP: 'gsap',
  Vite: 'vite',
  Supabase: 'supabase',
  Flask: 'flask',
  Vue: 'vuedotjs',
  Nuxt: 'nuxtdotjs',
  React: 'react',
  'React Native': 'react',
  'Next.js': 'nextdotjs',
  'Node.js': 'nodedotjs',
  TypeScript: 'typescript',
  PostgreSQL: 'postgresql',
  MongoDB: 'mongodb',
  Keras: 'keras',
  Docker: 'docker',
  Vercel: 'vercel',
  PyTorch: 'pytorch',
  OpenCV: 'opencv',
  TensorFlow: 'tensorflow',
  Flutter: 'flutter',
  Firebase: 'firebase',
  Laravel: 'laravel',
  PHP: 'php',
  MySQL: 'mysql',
  HTML: 'html5',
  CSS: 'css',
  JavaScript: 'javascript',
  Bootstrap: 'bootstrap',
  // Oracle doesn't license the Java coffee-cup mark for open reuse,
  // so this uses OpenJDK's Duke mark instead — the accepted open substitute.
  Java: 'openjdk',
  Git: 'git',
  GitHub: 'github',
  Postman: 'postman',
  Figma: 'figma',
  'Cisco Networking': 'cisco',
}

function techIconUrl(tech: string) {
  if (tech === 'VS Code') return 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vscode/vscode-plain.svg'
  const slug = techIconSlugs[tech]
  return slug ? `${techIconBase}/${slug}.svg` : null
}

function techInitials(tech: string) {
  return tech.replace(/[^A-Za-z]/g, '').slice(0, 2).toUpperCase()
}

const brandIcons: Record<string, string> = {
  HTML: 'html5', CSS: 'css3', JavaScript: 'javascript', TypeScript: 'typescript', Bootstrap: 'bootstrap',
  'Tailwind CSS': 'tailwindcss', Vite: 'vitejs', Vue: 'vuejs', Nuxt: 'nuxtjs', React: 'react',
  PHP: 'php', Laravel: 'laravel', 'Node.js': 'nodejs', MySQL: 'mysql', PostgreSQL: 'postgresql',
  MongoDB: 'mongodb', Supabase: 'supabase', Flutter: 'flutter', 'React Native': 'react', Dart: 'dart',
  Firebase: 'firebase', Python: 'python', PyTorch: 'pytorch', Keras: 'keras', OpenCV: 'opencv',
  TensorFlow: 'tensorflow', Java: 'java', Git: 'git', 'VS Code': 'vscode',
  Docker: 'docker', Postman: 'postman', Figma: 'figma',
}
const brokenBrandIcons = ref<Record<string, boolean>>({})
function brandIconUrl(tech: string) {
  const slug = brandIcons[tech]
  return slug && !brokenBrandIcons.value[tech] ? `https://cdn.jsdelivr.net/gh/devicons/devicon@v2.16.0/icons/${slug}/${slug}-original.svg` : null
}

/* ========================================
  SOCIAL / CONTACT LINKS
======================================== */

/* ========================================
  CONTACT FORM — FORMSPREE
======================================== */

const FORMSPREE_ENDPOINT = 'https://formspree.io/f/xnpqwgor'

const contactForm = reactive({
  name: '',
  email: '',
  message: '',
})

type FormStatus = 'idle' | 'sending' | 'success' | 'error'

const formStatus = ref<FormStatus>('idle')
const formErrorMessage = ref('')

async function submitContactForm() {
  if (formStatus.value === 'sending') return

  formStatus.value = 'sending'
  formErrorMessage.value = ''

  try {
    const response = await fetch(FORMSPREE_ENDPOINT, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        Accept: 'application/json',
      },
      body: JSON.stringify({
        name: contactForm.name,
        email: contactForm.email,
        message: contactForm.message,
      }),
    })

    if (response.ok) {
      formStatus.value = 'success'
      contactForm.name = ''
      contactForm.email = ''
      contactForm.message = ''
      return
    }

    const data = await response.json().catch(() => null)

    formErrorMessage.value =
      data?.errors?.[0]?.message ??
      'Something went wrong sending that — try again, or email me directly.'
    formStatus.value = 'error'
  } catch (error) {
    console.warn('Contact form submission failed.', error)
    formErrorMessage.value =
      'Could not reach the server — check your connection and try again.'
    formStatus.value = 'error'
  }
}


onMounted(() => {
  toolkitMedia = window.matchMedia('(max-width: 720px)')
  updateToolkitViewport()
  toolkitMedia.addEventListener('change', updateToolkitViewport)
  // Lazy hydration can attach listeners after an SSR icon has already failed.
  for (const image of document.querySelectorAll<HTMLImageElement>('.toolkit-brand[data-skill]')) {
    const skill = image.dataset.skill
    if (skill && image.complete && image.naturalWidth === 0) brokenBrandIcons.value[skill] = true
  }
  const contributionImage = githubChartViewport.value?.querySelector<HTMLImageElement>('.github-chart')
  if (contributionImage?.complete && contributionImage.naturalWidth === 0) contributionChartFailed.value = true
  if (githubChartViewport.value && !contributionChartFailed.value) {
    githubChartResizeObserver = new ResizeObserver(showRecentGitHubActivity)
    githubChartResizeObserver.observe(githubChartViewport.value)
    showRecentGitHubActivity()
  }
})

onBeforeUnmount(() => {
  toolkitMedia?.removeEventListener('change', updateToolkitViewport)
  githubChartResizeObserver?.disconnect()
})
</script>

<template>
<!-- ==================================
          SKILLS & CERTIFICATIONS
      =================================== -->

      <section
        id="skills"
        class="skills-section page-width"
        aria-labelledby="skills-title"
      >
        <div class="section-heading">
          <div class="section-heading-bottom">
            <h2 id="skills-title">Skills &amp; certifications.</h2>
            <p>Core toolkit across web, mobile, and AI — backed by certifications along the way.</p>
          </div>
        </div>

        <div class="toolkit-categories" :class="{ 'toolkit-instant': toolkitKeyboardNavigation }">
          <div class="toolkit-tabs" role="tablist" aria-label="Stack categories">
            <button
              v-for="(group, index) in skillGroups"
              :id="`skill-tab-${index}`"
              :key="group.title"
              class="toolkit-tab"
              type="button"
              role="tab"
              :aria-selected="activeSkillGroup === index"
              :aria-controls="`skill-panel-${index}`"
              :tabindex="activeSkillGroup === index ? 0 : -1"
              @click="selectSkillGroup(index, $event)"
              @keydown="handleSkillTabKey($event, index)"
            >{{ skillTabLabels[index] }}</button>
          </div>
          <div class="toolkit-panels">
            <div
              v-for="(group, index) in skillGroups"
              :id="`skill-panel-${index}`"
              :key="group.title"
              class="toolkit-panel"
              :class="{ 'is-active': activeSkillGroup === index }"
              :role="mobileToolkit ? 'tabpanel' : undefined"
              :aria-labelledby="mobileToolkit ? `skill-tab-${index}` : undefined"
              :aria-hidden="mobileToolkit ? activeSkillGroup !== index : undefined"
              :inert="mobileToolkit && activeSkillGroup !== index"
              :tabindex="mobileToolkit ? (activeSkillGroup === index ? 0 : -1) : undefined"
            >
              <ul class="toolkit-grid" :aria-label="group.title">
                <li v-for="skill in group.skills" :key="skill" class="toolkit-item" :class="{ 'toolkit-monochrome': ['GitHub', 'Vercel', 'Next.js', 'Three.js', 'Flask'].includes(skill) }">
                  <div class="toolkit-button">
                    <img v-if="brandIconUrl(skill)" class="toolkit-brand" :data-skill="skill" :src="brandIconUrl(skill)!" alt="" width="26" height="26" loading="lazy" decoding="async" @error="brokenBrandIcons[skill] = true" />
                    <span v-else-if="techIconUrl(skill)" class="token-icon toolkit-icon" :style="{ '--tech-icon': `url('${techIconUrl(skill)}')`, '--skill-color': skillColors[skill] || 'var(--text)' }" aria-hidden="true" />
                    <span v-else class="toolkit-icon toolkit-initial" aria-hidden="true">{{ techInitials(skill) }}</span>
                    <span>{{ skill }}</span>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>

        <!-- Certifications -->

        <div class="credentials">
          <p class="eyebrow muted">
            CERTIFICATIONS
          </p>

          <ul class="credential-list">
            <li
              v-for="cert in certifications"
              :key="cert.title"
              class="credential-item"
            >
              <span
                class="token-icon credential-icon"
                :style="{ '--tech-icon': `url('${techIconUrl('Cisco Networking')}')` }"
                aria-hidden="true"
              />

              <div class="credential-text">
                <p class="credential-title">
                  {{ cert.title }}
                </p>

                <p class="credential-meta mono muted">
                  {{ cert.issuer }} · {{ cert.date }}
                </p>
              </div>
            </li>
          </ul>
        </div>
      </section>

      <!-- Experience & education -->

      <section
        id="experience"
        class="experience-section page-width"
        aria-labelledby="experience-title"
      >
        <div class="section-heading">
          <div class="section-heading-bottom">
            <h2 id="experience-title">
              Experience &amp; education.
            </h2>
            <p>Technical operations experience alongside my Information Technology degree.</p>
          </div>
        </div>

        <article class="background-row" aria-labelledby="role-title">
          <div class="background-meta mono muted">
            <img class="background-logo" src="/images/cruisechef-logo-128.webp" alt="" width="64" height="64" loading="lazy" decoding="async" />
            <p>September 2026–Present</p>
            <p>Remote</p>
          </div>
          <div class="background-content">
            <h3 id="role-title">Technical Staff</h3>
            <p class="background-subtitle">Cruisechef International Academy</p>
            <div class="background-description">
              <p>Support the academy's admin dashboard, reservations, automated email workflows, and account troubleshooting for staff and trainees, with 200+ reservations on record.</p>
              <p>Maintain social media channels and the content calendar, and contribute to planning an in-house trainee account management application.</p>
            </div>
          </div>
        </article>

        <article class="background-row" aria-labelledby="education-title">
          <div class="background-meta mono muted">
            <img class="background-logo" src="/images/dnsc-seal-128.webp" alt="" width="64" height="64" loading="lazy" decoding="async" />
            <p>Expected June 2027</p>
            <p>Fourth year</p>
          </div>
          <div class="background-content">
            <h3 id="education-title">BS Information Technology</h3>
            <p class="background-subtitle">Davao del Norte State College</p>
            <p class="background-description">Panabo City, Philippines</p>
          </div>
        </article>
      </section>

      <!-- ==================================
          CONTACT
      =================================== -->

      <section class="github-section page-width" aria-labelledby="github-title">
        <div class="github-panel">
          <div class="github-panel-heading">
            <div>
              <h2 id="github-title">GitHub activity</h2>
              <p class="github-description">A snapshot of my public contributions over the past year.</p>
            </div>
          </div>
          <div v-if="!contributionChartFailed" ref="githubChartViewport" class="github-chart-wrap" tabindex="0" role="region" aria-label="GitHub contributions, latest months first; scroll horizontally to view the full year">
            <img class="github-chart" src="https://ghchart.rshah.org/0878a8/Laurenceonly" alt="GitHub contribution activity for Laurenceonly over the past year" width="663" height="104" loading="lazy" @load="showRecentGitHubActivity" @error="contributionChartFailed = true" />
          </div>
          <p v-else class="github-chart-fallback">The contribution chart is unavailable right now. Visit my GitHub profile to see my activity.</p>
          <a class="github-profile-link" href="https://github.com/Laurenceonly" target="_blank" rel="noopener noreferrer">View GitHub profile</a>
        </div>
      </section>

      <section
        id="contact"
        class="contact-section page-width"
        aria-labelledby="contact-title"
      >
        <div>
          <h2 id="contact-title">
            Let's connect.
          </h2>

          <div class="contact-columns">
            <form
              class="contact-form"
              :action="FORMSPREE_ENDPOINT"
              method="post"
              @submit.prevent="submitContactForm"
            >
              <p class="mono muted contact-form-label">
                Send a message
              </p>

              <div class="form-field">
                <label class="form-label mono" for="contact-name">
                  Name
                </label>

                <input
                  id="contact-name"
                  v-model="contactForm.name"
                  class="form-input"
                  type="text"
                  name="name"
                  autocomplete="name"
                  required
                />
              </div>

              <div class="form-field">
                <label class="form-label mono" for="contact-email">
                  Email
                </label>

                <input
                  id="contact-email"
                  v-model="contactForm.email"
                  class="form-input"
                  type="email"
                  name="email"
                  autocomplete="email"
                  required
                />
              </div>

              <div class="form-field">
                <label class="form-label mono" for="contact-message">
                  Message
                </label>

                <textarea
                  id="contact-message"
                  v-model="contactForm.message"
                  class="form-input form-textarea"
                  name="message"
                  rows="4"
                  required
                ></textarea>
              </div>

              <div class="form-actions">
                <button
                  type="submit"
                  class="form-submit"
                  :disabled="formStatus === 'sending'"
                >
                  {{ formStatus === 'sending' ? 'Sending…' : 'Send message' }}
                </button>

                <p
                  v-if="formStatus === 'success'"
                  class="form-status is-success"
                  role="status"
                >
                  Message sent — I'll get back to you soon.
                </p>

                <p
                  v-else-if="formStatus === 'error'"
                  class="form-status is-error"
                  role="alert"
                >
                  {{ formErrorMessage }}
                </p>
              </div>
            </form>

            <div class="contact-bottom">
              <div class="social-links-group">
                <p class="mono muted social-links-label">
                  Elsewhere
                </p>
                <a class="github-profile-link" href="https://github.com/Laurenceonly" target="_blank" rel="noopener noreferrer">GitHub</a>
              </div>

              <p>
                For development projects, collaborations, or a conversation
                about building something together, feel free to reach out.
              </p>
            </div>
          </div>
        </div>
      </section>
</template>
