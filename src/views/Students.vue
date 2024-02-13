<script setup lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';

interface Student {
  gender: string;
  name: {
    title?: string;
    first?: string;
    last?: string;
  };
  email: string;
  id: string;
}
// Use a ref to hold the students array
const students = ref<Student[]>([]);

// Fetch students from the API
const fetchStudents = async () => {
  try {
    const response = await axios.get('https://randomuser.me/api/?results=10');
    students.value = transformStudentData(response);
    //students.value = response.data;
  } catch (error) {
    console.error("There was an error fetching the students: ", error);
  }
};

const transformStudentData = (data: any): Student[] => {
  return data.data.results.map((item: any) => ({
    id: item.id.value,
    name: item.name,
    email: item.email
  }));
};

// Use onMounted lifecycle hook to fetch data when the component mounts
onMounted(() => {
  fetchStudents();
});
</script>


<template>
  <div class="container">
    <h1>Students Page</h1>
    <div v-if="!students || students.length == 0">No Student Available</div>
    <ul class="list-group" v-if="students && students.length > 0">
      <li class="list-group-item" v-for="student in students" :key="student.id">{{ `${student?.name?.title} ${student?.name?.first} ${student?.name?.last}`  }}</li>
    </ul>
  </div>
</template>

<style scoped>
</style>
