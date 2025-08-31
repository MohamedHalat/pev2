<script lang="ts" setup>
import { inject, computed } from "vue"
import Plan from "@/components/Plan.vue"
import MainLayout from "../layouts/MainLayout.vue"

const props = defineProps<{
  planSource?: string
  planQuery?: string
  title?: string
}>()

const planData = inject("planData")

const source = computed(() => props.planSource || planData[0])
const query = computed(() => props.planQuery || planData[1])
const viewTitle = computed(() => props.title || planData[2])
</script>

<template>
  <MainLayout :title="viewTitle">
    <Plan
      v-if="source"
      :plan-source="source"
      :plan-query="query"
    />
  </MainLayout>
</template>
