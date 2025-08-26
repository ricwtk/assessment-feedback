<template>
  <!-- <div class="p-6 max-w-4xl mx-auto"> -->
  <Panel>
    <template #header>
      <h1 class="text-2xl font-bold mb-6">{{ assessment.name }}</h1>
    </template>

    <div class="flex flex-col gap-2">
      <CriteriaTable
        v-for="c in assessment.criteria"
        :key="c.name"
        :criteria="c"
        :feedback="feedback[c.name]"
      />
    </div>

    <div class="mt-6 p-4">
      <h2 class="">Total Score for <span class="font-bold">{{ assessment.name }}</span>: <Tag>{{ feedback.overall.score }}</Tag></h2>
      <p class="text-gray-700">
        <span class="font-medium">Overall Comment: {{ feedback.overall.comment }}</span> 
      </p>
    </div>
  </Panel>
  <!-- </div> -->
</template>

<script setup>
const props = defineProps({
  assessment: {
    type: Object,
    required: true,
  },
  feedback: {
    type: Object,
    required: true,
  }
});
</script>