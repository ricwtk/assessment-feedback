<template>
  <!-- <div class="mb-6 p-4 rounded-2xl bg-white"> -->
  <Card>
    <template #title>
      <h3 class="text-lg font-semibold mb-2">{{ criteria.name }}</h3>
    </template>

    <template #content>
      <div class="flex flex-col gap-2">
        <div class="flex flex-row justify-stretch gap-2 p-2 border-b border-gray-300 ">
          <div v-for="l in criteria.levels" class="grow flex-1 text-center">{{ l.score[0] }} - {{ l.score[1] }}</div>
        </div>
        <div class="flex flex-row justify-stretch gap-2">
          <div v-for="l in criteria.levels" class="grow flex-1 text-center"
            :class="[
              'grow text-center p-2',
              feedback.score >= l.score[0] && feedback.score <= l.score[1] ? 'bg-green-100' : ''
            ]"
          >{{ l.comment }}</div>
        </div>
      </div>
      <!-- <DataTable :value="criteria.levels" stripedRows responsiveLayout="scroll">
        <Column field="level" header="Level" />
        <Column field="description" header="Description" />
        <Column field="score" header="Score">
          <template #body="slotProps">
            <span
              :class="[
                'px-2 py-1 rounded-md',
                slotProps.data.score === criteria.score
                  ? 'bg-green-100 text-green-700 font-semibold'
                  : 'text-gray-600'
              ]"
            >
              {{ slotProps.data.score }}
            </span>
          </template>
        </Column>
      </DataTable> -->
  
      <div class="mt-3">
        <p class="font-medium">
          Score: <span class="text-blue-600 font-bold">{{ feedback.score }}</span>
        </p>
        <p class="mt-1 text-gray-700">
          <span class="font-medium">Comment:</span> {{ feedback.comment || "—" }}
        </p>
      </div>
    </template>
  </Card>
  <!-- </div> -->
</template>

<script setup>
const props = defineProps({
  criteria: {
    type: Object,
    required: true,
  },
  feedback: {
    type: Object,
    required: true,
  },
});
</script>