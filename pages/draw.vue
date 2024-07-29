<template>
  <v-container fill-height>
    <v-row align="center" justify="center">
      <v-col cols="12" md="8">
        <v-card class="elevation-3 pa-5" outlined>
          <v-card-title class="headline mb-3 text-center">
            Secret Santa Draw
          </v-card-title>
          <v-card-subtitle class="mb-4 text-center">
            Click the button below to perform the Secret Santa draw.
          </v-card-subtitle>
          <v-card-actions class="justify-center">
            <v-btn @click="performDraw" color="primary" large>
              Perform Draw
            </v-btn>
          </v-card-actions>
          <v-list>
            <v-list-item
              v-for="(assignment, index) in assignments"
              :key="index"
            >
              <v-list-item-content>
                <v-list-item-title>
                  {{ assignment.participant }} is assigned to
                  {{ assignment.assignedTo }}
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
import { ref, onMounted } from "vue";
import { useFetch, useState } from "#app";
import { useRouter } from "vue-router";


// Define global state for assignments
const assignmentsStore = useState("assignments", () => []);
const participantsStore = useState("participants", () => []);
const router = useRouter();

const performDraw = async () => {
  const participants = [...participantsStore.value];
  if (participants.length < 2) {
    alert("Not enough participants to perform a draw.");
    return;
  }

  // Shuffle participants
  const shuffled = participants.sort(() => 0.5 - Math.random());

  // Assign each participant to another
  const newAssignments = participants.map((participant, index) => ({
    participant: participant.name,
    assignedTo: shuffled[(index + 1) % shuffled.length].name,
  }));

  assignmentsStore.value = newAssignments;

  // Post assignments to API
  try {
    const { data, error } = await useFetch("https://reqres.in/api/users", {
      method: "POST",
      body: JSON.stringify({
        assignment: newAssignments.reduce(
          (acc, { participant, assignedTo }) => {
            acc[participant] = assignedTo;
            return acc;
          },
          {}
        ),
      }),
      headers: {
        "Content-Type": "application/json",
      },
    });

    if (error.value) {
      console.error("Error posting assignments:", error.value);
      alert("There was an error posting the assignments.");
      return;
    }

    // Redirect to results page
    router.push("/results");
  } catch (err) {
    console.error("Unexpected error:", err);
    alert("There was an unexpected error.");
  }
};
</script>

<style scoped>
.draw-page {
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

v-btn {
  background-color: #3f51b5;
  color: white;
}

v-btn:hover {
  background-color: #ff4081;
  color: white;
}
</style>
