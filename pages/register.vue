<template>
    <v-container class="registration-page" fill-height>
        <v-row align="center" justify="center">
            <v-col cols="12" md="8">
                <v-card class="elevation-3 pa-5" outlined>
                    <v-card-title class="headline mb-3 text-center">
                        Register Participants
                    </v-card-title>
                    <v-form ref="form" @submit.prevent="registerParticipants">
                        <v-row v-for="(participant, index) in participants" :key="index" align="center">
                            <v-col cols="12" sm="5">
                                <v-text-field v-model="participant.name" label="Participant Name"
                                    :rules="[rules.required, rules.uniqueName]" outlined></v-text-field>
                            </v-col>
                            <v-col cols="12" sm="5">
                                <v-text-field v-model="participant.email" label="Participant Email"
                                    :rules="[rules.required, rules.email, rules.uniqueEmail]" outlined></v-text-field>
                            </v-col>
                            <v-col cols="12" sm="2" class="text-center">
                                <v-btn icon @click="removeParticipant(index)">
                                    <v-icon color="error">mdi-delete</v-icon>
                                </v-btn>
                            </v-col>
                        </v-row>
                        <v-card-actions class="justify-center mt-4">
                            <v-btn @click="addParticipant" color="secondary">Add Participant</v-btn>
                            <v-btn type="submit" color="primary">Submit</v-btn>
                        </v-card-actions>
                    </v-form>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import { useState } from "#app";

//Variables
const participants = ref([{ name: "", email: "" }]);
const router = useRouter();
const form = ref(null);
const stateParticipants = useState('participants', () => []);

/**
 * Function to validation rules while submitting the form
 */
const rules = {
    required: (value) => !!value || "Required.",
    email: (value) => /.+@.+\..+/.test(value) || "Invalid email.",
    uniqueName: (value) => {
        const names = participants.value.map((p) => p.name);
        return (
            names.filter((n) => n === value).length <= 1 || "Name must be unique."
        );
    },
    uniqueEmail: (value) => {
        const emails = participants.value.map((p) => p.email);
        return (
            emails.filter((e) => e === value).length <= 1 || "Email must be unique."
        );
    },
};

/**
 *  Function to add new participants
 */
const addParticipant = () => {
    console.log(rules);
    participants.value.push({ name: "", email: "" });
};

/**
 *  Function to remove the fields
 * @param index get index of fields
 */
const removeParticipant = (index) => {
    participants.value.splice(index, 1);
};

/**
 *  Function to register the participants
 */
const registerParticipants = async () => {
    const isValid = await form.value.validate();

    if (isValid.valid == true) {
        const participantsList = []
        try {
            for (let participant of participants.value) {
              const {data} = await useFetch("https://reqres.in/api/users", {
                    method: "POST",
                    body: {
                        name: participant.name,
                        email: participant.email,
                        createdAt: new Date().toISOString(),
                    },
                });
                participantsList.push(data.value)
            }
            stateParticipants.value = participantsList
            router.push("/draw");
        } catch (error) {
            console.error("Error submitting participant:", error);
        }
    }
};
</script>

<style scoped>
.registration-page {
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

v-text-field {
    margin-bottom: 16px;
}
</style>
