<script setup lang="ts">
import { ref, computed } from 'vue'
import countries from '../mocks/countryCodes.js'
import { useRouter } from 'vue-router'

const selected = ref(null)
const options = ref(countries)

const computedOptions = computed(() => countries)

const baseSelectOptions = ref(
  countries.map((country) => ({
    name: country.label,
    value: country.value,
  }))
)
const selectedOption = ref()

setTimeout(() => {
  selectedOption.value = baseSelectOptions.value[0]
}, 500)

const links = computed(() => {
  const router = useRouter()
  const excludes = ['Home', '404']
  return router.options.routes.filter(
    (r) => r.path.startsWith('/') && !excludes.includes(r.name as string)
  )
})
</script>

<template>
  <div>
    <div class="section">
      <h2 class="title">Basic</h2>
      <v-select v-model="selected" :options="options" />
    </div>

    <div class="section w-full flex flex-col items-center">
      <div>
        <h2 class="title">Use Cases</h2>
        <ul class="list-disc">
          <li v-for="link in links" :key="link.name">
            <router-link class="text-blue-400" :to="link.path">
              {{ link.name }}
            </router-link>
          </li>
        </ul>
      </div>
    </div>

    <div class="section">
      <h2 class="title">Custom</h2>
      <ul>
        <li>searchable: false</li>
        <li>clearable: false</li>
      </ul>
      <v-select
        v-model="selected"
        :options="computedOptions"
        :clearable="false"
        :searchable="false"
      />
    </div>
  </div>
</template>

<style scoped></style>
