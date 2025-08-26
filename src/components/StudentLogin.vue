<template>
  <div class="flex flex-col gap-2">
    <InputGroup>
      <InputGroupAddon><i class="pi pi-warehouse"></i></InputGroupAddon>
      <Select placeholder="Assessment" v-model="selectedCourse" :options="courselist" optionLabel="display"></Select>
    </InputGroup>

    <div class="flex flex-row gap-2">
      <InputGroup>
        <InputGroupAddon><i class="pi pi-user"></i></InputGroupAddon>
        <Select placeholder="Yourself" v-model="selectedPerson" :options="personlist" optionLabel="display"></Select>
      </InputGroup>

      <InputGroup>
        <InputGroupAddon><i class="pi pi-key"></i></InputGroupAddon>
        <InputText :useGrouping="false" placeholder="Pin" v-model="providedPin"/>
      </InputGroup>
    </div>

    <Button class="self-end" @click="showfeedback">Show Feedback</Button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps(["courses"])
const courselist = computed(() => props.courses.map(c => {
  c.display = `${c.code} ${c.name} (${c.assessment})`
  return c
}))
const personlist = computed(() => {
  if (selectedCourse.value == "") { return [] }
  else {
    selectedCourse.value.students.sort((a, b) => a.id.localeCompare(b.id))
    return selectedCourse.value.students.map(s => {
      s.display = `${s.id} ${s.name}`
      return s
    })
  }
})

const selectedCourse = ref("")
const selectedPerson = ref("")
const providedPin = ref("")

const emit = defineEmits(["showfeedback"])
const showfeedback = () => {
  emit("showfeedback", {
    course: { 
      key: selectedCourse.value.courseKey, 
      name: selectedCourse.value.name, 
      code: selectedCourse.value.code,
      assessment: selectedCourse.value.assessment,
      lecturers: selectedCourse.value.lecturers.map(l => ({ name: l.name, email: l.email }))
    },
    student: { 
      id: selectedPerson.value.id,
      name: selectedPerson.value.name
    },
    pin: providedPin.value
  })
}
</script>