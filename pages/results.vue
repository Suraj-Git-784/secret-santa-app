<template>
    <v-container fill-height>
      <v-row align="center" justify="center">
        <v-col cols="12" md="8">
          <v-card class="elevation-3 pa-5" outlined>
            <v-card-title class="headline mb-3 text-center">
              Secret Santa Results
            </v-card-title>
            <v-card-subtitle class="mb-4 text-center">
              Here are your Secret Santa assignments.
            </v-card-subtitle>
            <v-list v-if="assignments && assignments.length">
              <v-list-item v-for="(assignment, index) in assignments" :key="index">
                <v-list-item-content>
                  <v-list-item-title>
                    {{ assignment.participant }} is assigned to {{ assignment.assignedTo }}
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import { useFetch, useState } from '#app'
  
  // Global state for assignments
  const assignmentsStore = useState('assignments')
  
  // Local state for assignments
  const assignments = ref([])
  
  /**
   *  Function to get the assignments
   */
  onMounted(async () => {
    if (assignmentsStore.value.length === 0) {
      const { data, error } = await useFetch('https://reqres.in/api/users', {
        method: 'GET',
        headers: {
          'Content-Type': 'application/json'
        }
      })
  
      if (error.value) {
        console.error('Error fetching assignments:', error.value)
        return
      }
  
      const fetchedAssignments = data.value?.assignment || []
      assignmentsStore.value = fetchedAssignments
    }
  
    assignments.value = assignmentsStore.value
  })
  </script>
  
  <style scoped>
  .results-page {
    margin-top: 50px;
  }
  
  v-card {
    backdrop-filter: blur(10px);
    background-color: rgba(255, 255, 255, 0.8);
    border-radius: 20px;
  }
  
  .headline {
    color: #ff4081;
  }
  
  v-list-item {
    margin-bottom: 10px;
  }
  </style>
  