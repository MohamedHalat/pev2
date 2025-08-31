<script lang="ts" setup>
import { ref, computed, provide } from "vue"
import AboutView from "./views/AboutView.vue"
import HomeView from "./views/HomeView.vue"
import NotFoundView from "./views/NotFoundView.vue"
import PlanView from "./views/PlanView.vue"

const routes = {
  "/": HomeView,
  "/about": AboutView,
  "/plan": PlanView,
}

const currentPath = ref("/")
provide("currentPath", currentPath)

import samples from "./samples"

const planId = ref<number | null>(null)

const currentView = computed(() => {
  const path = currentPath.value
  const match = path.match(/^\/plans\/(\d+)$/)
  if (match) {
    planId.value = Number(match[1])
    return PlanView
  }
  planId.value = null
  return routes[path] || NotFoundView
})

const planData = ["", "", ""]
provide("planData", planData)

const currentProps = computed(() => {
  if (planId.value !== null) {
    const sample = samples[planId.value]
    if (sample) {
      return {
        planSource: sample[1],
        planQuery: sample[2],
        title: sample[0],
      }
    }
  }
  return {}
})

function setPlanData(name, plan, query) {
  planData[0] = plan
  planData[1] = query
  planData[2] = name
  currentPath.value = "/plan"
}
provide("setPlanData", setPlanData)
window.setPlanData = setPlanData

window.onpopstate = () => {
  currentPath.value = window.location.pathname
}
</script>

<template>
  <component :is="currentView" v-bind="currentProps" />
</template>
