<template>
  <v-container class="landing-page" fill-height>
    <v-row align="center" justify="center">
      <v-col cols="12" md="8">
        <v-card class="elevation-3 pa-5" outlined>
          <v-card-title class="headline mb-3 text-center">
            Welcome to the Secret Santa App
          </v-card-title>
          <v-card-subtitle class="mb-4 text-center">
            A fun and easy way to organize your Secret Santa gift exchange!
          </v-card-subtitle>
          <v-img 
            src="/secret-santa-hero.jpg"
            class="mb-4"
            height="200px"
            contain
            style="border-radius: 20px;"
          ></v-img>
          <v-card-actions class="justify-center">
            <v-btn color="primary" large to="/register">
              Register Participants
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
// No need for additional setup in this simple landing page
</script>

<style>
.landing-page {
  margin-top: 0; /* Adjusted to remove unnecessary space */
  background: linear-gradient(
      rgba(0, 0, 0, 0.5), 
      rgba(0, 0, 0, 0.5)
    ),
    url('https://img.freepik.com/free-photo/colorful-design-with-spiral-design_188544-9588.jpg') no-repeat center center fixed;
  background-size: cover;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

v-card {
  backdrop-filter: blur(10px);
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 20px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

.headline {
  color: #ff4081;
}

.text-center {
  text-align: center;
  color: #3f51b5;
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
