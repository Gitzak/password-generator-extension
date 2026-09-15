<script setup lang="ts">
import { computed, ref } from 'vue'

const password = ref('')
const length = ref(16)

const options = ref({
  lowercase: true,
  uppercase: true,
  numbers: true,
  symbols: true,
})

const toastVisible = ref(false)
const toastMessage = ref('')

const spotlightX = ref(190)
const spotlightY = ref(120)
const spotlightVisible = ref(0)

let toastTimeout: number | null = null

const LOWERCASE = 'abcdefghijklmnopqrstuvwxyz'
const UPPERCASE = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
const NUMBERS = '0123456789'
const SYMBOLS = '!@#$%^&*()_+-=[]{}'

const getRandomIndex = (max: number) => {
  if (max <= 0) {
    throw new Error('Max must be greater than 0')
  }

  const maxUint32 = 0xffffffff
  const limit = maxUint32 - (maxUint32 % max)

  const random = new Uint32Array(1)

  do {
    crypto.getRandomValues(random)
  } while (random[0] >= limit)

  return random[0] % max
}

const generatePassword = () => {
  const selectedSets: string[] = []

  if (options.value.lowercase) {
    selectedSets.push(LOWERCASE)
  }

  if (options.value.uppercase) {
    selectedSets.push(UPPERCASE)
  }

  if (options.value.numbers) {
    selectedSets.push(NUMBERS)
  }

  if (options.value.symbols) {
    selectedSets.push(SYMBOLS)
  }

  if (selectedSets.length === 0) {
    password.value = ''
    return
  }

  const characters = selectedSets.join('')
  const result: string[] = []

  // Ensure at least one character from each selected category
  for (const set of selectedSets) {
    result.push(set[getRandomIndex(set.length)])
  }

  // Fill the rest of the password
  while (result.length < length.value) {
    result.push(
      characters[getRandomIndex(characters.length)]
    )
  }

  // Shuffle securely
  for (let i = result.length - 1; i > 0; i--) {
    const j = getRandomIndex(i + 1)

      ;[result[i], result[j]] = [result[j], result[i]]
  }

  password.value = result.join('')
}

const showToast = (message: string) => {
  toastMessage.value = message
  toastVisible.value = true

  if (toastTimeout) {
    clearTimeout(toastTimeout)
  }

  toastTimeout = window.setTimeout(() => {
    toastVisible.value = false
  }, 3000)
}

const copyPassword = async () => {
  if (!password.value) return

  try {
    await navigator.clipboard.writeText(password.value)

    showToast('Password copied')
  } catch {
    showToast('Unable to copy password')
  }
}

const strength = computed(() => {
  let score = 0

  if (length.value >= 12) {
    score++
  }

  if (length.value >= 16) {
    score++
  }

  if (options.value.lowercase) {
    score++
  }

  if (options.value.uppercase) {
    score++
  }

  if (options.value.numbers) {
    score++
  }

  if (options.value.symbols) {
    score++
  }

  if (score <= 2) {
    return 'Weak'
  }

  if (score <= 4) {
    return 'Medium'
  }

  return 'Strong'
})

const spotlightStyle = computed(() => ({
  '--spot-x': `${spotlightX.value}px`,
  '--spot-y': `${spotlightY.value}px`,
  '--spot-opacity': spotlightVisible.value.toString(),
}))

const handleMouseMove = (event: MouseEvent) => {
  const element = event.currentTarget as HTMLElement
  const rect = element.getBoundingClientRect()

  spotlightX.value = event.clientX - rect.left
  spotlightY.value = event.clientY - rect.top

  spotlightVisible.value = 1
}

const handleMouseLeave = () => {
  spotlightVisible.value = 0
}

generatePassword()
</script>

<template>
  <main class="app" :style="spotlightStyle" @mousemove="handleMouseMove" @mouseleave="handleMouseLeave">
    <div class="toast" :class="{ show: toastVisible }">
      {{ toastMessage }}
    </div>

    <header class="header">
      <span class="eyebrow">
        SECURE TOOL
      </span>

      <h1>Password Generator</h1>
    </header>

    <section class="password-card">
      <div class="password-value">
        {{ password || 'Select at least one option' }}
      </div>

      <button class="copy-button" type="button" :disabled="!password" @click="copyPassword">
        Copy
      </button>
    </section>

    <div class="strength">
      <span>Password strength</span>

      <strong :class="strength.toLowerCase()">
        {{ strength }}
      </strong>
    </div>

    <section class="settings">
      <div class="setting-title">
        <span>Length</span>

        <strong>
          {{ length }}
        </strong>
      </div>

      <input v-model.number="length" class="range" type="range" min="8" max="64" step="1" @input="generatePassword" />

      <div class="options">
        <label>
          <span>Uppercase</span>

          <input v-model="options.uppercase" type="checkbox" @change="generatePassword" />
        </label>

        <label>
          <span>Lowercase</span>

          <input v-model="options.lowercase" type="checkbox" @change="generatePassword" />
        </label>

        <label>
          <span>Numbers</span>

          <input v-model="options.numbers" type="checkbox" @change="generatePassword" />
        </label>

        <label>
          <span>Symbols</span>

          <input v-model="options.symbols" type="checkbox" @change="generatePassword" />
        </label>
      </div>
    </section>

    <button class="generate-button" type="button" @click="generatePassword">
      Generate new password
    </button>

    <p class="privacy">
      Generated locally. Nothing leaves your browser.
    </p>

    <p class="developer">
      Developed by
      <span>GitZak</span>
      ·
      <a href="https://github.com/Gitzak" target="_blank" rel="noopener noreferrer">
        GitHub
      </a>
    </p>
  </main>
</template>