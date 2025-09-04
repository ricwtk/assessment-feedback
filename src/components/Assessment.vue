<template>
  <!-- <div class="p-6 max-w-4xl mx-auto"> -->
  <Panel>
    <template #header>
      <h1 class="text-2xl font-bold mb-6">{{ assessment.name }}</h1>
    </template>

    <div class="flex flex-col gap-2" v-if="assessment.criteria.length>0">
      <CriteriaTable
        v-for="c in assessment.criteria"
        :key="c.name"
        :criteria="c"
        :feedback="feedback[c.name]"
      />
    </div>

    <div class="p-4" :class="assessment.criteria.length > 0 ? 'mt-6' : ''">
      <h2 class="">Total Score for <span class="font-bold">{{ assessment.name }}</span>: <Tag>{{ feedback.overall.score < 1 ? Math.round(feedback.overall.score*10000)/100 : feedback.overall.score }}</Tag></h2>
      <p class="text-gray-700" v-if="feedback.overall.comment">
        <span class="font-medium flex flex-col">
          <span class="font-bold">Overall Comment</span>
          <span>
            {{ feedback.overall.comment }}
          </span>
        </span> 
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