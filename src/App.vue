<template>
  <div class="w-full max-w-5xl flex flex-col gap-2 m-2">
    <TopBar />
    
    <Panel>
      <template #header>
        <div class="flex flex-row gap-2 items-center">
          <Tag>Your Information</Tag>
        </div>
      </template>
      <div class="flex flex-col gap-2">
        <StudentLogin :courses="courses" @showfeedback="showfeedback"/>
        <Message severity="error" v-if="loginerror"><strong>Error</strong> {{ loginerror }}</Message>
      </div>
    </Panel>

    <Panel v-if="student.id !== ''">
      <template #header>
        <div class="flex flex-row gap-2 items-center">
          <Tag>Assessment Feedback</Tag>
        </div>
      </template>
      <div class="flex flex-col gap-2">
        <Message severity="success">The following is the feedback for {{ student.name }} ({{ student.id }}).</Message>  
        <Assessment v-if="feedback" v-for="asm in feedback.rubrics" :assessment="asm" :feedback="feedback.feedback[asm.name]"></Assessment>
      </div>
    </Panel>
    
  </div>
</template>

<script setup>
import { API_BASE_URL } from './config';
import { ref } from "vue"

const courses = ref([])
async function fetchCourses() {
  try {
    const response = await fetch(`${API_BASE_URL}/courses`);
    const data = await response.json();
    console.log("Courses:", data);
    courses.value = data
    courses.value.sort((a, b) => a.code.localeCompare(b.code))
  } catch (error) {
    console.error("Error fetching courses:", error);
  }
}

fetchCourses();

const loginerror = ref(null)
const feedback = ref(null)
const loading = ref(false);
async function getStudentFeedback(courseKey, studentId, pin) {
  loginerror.value = null
  feedback.value = null
  loading.value = true
  try {
    const response = await fetch(
      `${API_BASE_URL}/course/${courseKey}/feedback/${studentId}`,
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ pin }),
      }
    );

    if (!response.ok) {
      const errorData = await response.json();
      loginerror.value = errorData.error || "Unknown error";
      throw new Error(errorData.error || "Failed to fetch feedback");
    }

    feedback.value = await response.json();
    console.log(feedback.value)
  } catch (err) {
    loginerror.value = err.message
    console.error("Error fetching feedback:", err.message);
    return null;
  } finally {
    loading.value = false;
  }
}

const showfeedback = (login) => {
  student.value.id = login.student.id
  student.value.name = login.student.name
  assessment.value.key = login.course.key
  assessment.value.code = login.course.code
  assessment.value.name = login.course.name
  assessment.value.assessment = login.course.assessment
  assessment.value.lecturers = login.course.lecturers.map(l => ({ name: l.name, email: l.email}))
  getStudentFeedback(login.course.key, login.student.id, login.pin);
}

const student = ref({
  id: "", name: ""
})

const assessment = ref({
  key: "",
  code: "",
  name: "",
  assessment: "",
  lecturers: []
})

</script>