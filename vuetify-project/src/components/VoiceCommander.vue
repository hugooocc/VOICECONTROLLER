<script setup>
import { ref, watch } from 'vue';
// 1. Importa useTheme de Vuetify per a la Tasca 2
import { useTheme } from 'vuetify'; 
import { useSpeechRecognition } from '@/composables/useSpeechRecognition';

// 2. Injecta el servei de tema de Vuetify
const theme = useTheme();

const { isListening, transcript, interimTranscript, error, start } = useSpeechRecognition();

const uiMessage = ref("Prem el botó per començar...");
const statusColor = ref("primary");
// 3. Estat del Snackbar per a la Tasca 3
const snackbar = ref(false); 
const snackbarText = ref('');
const snackbarTimeout = 3000; // 3 segons

// Funció per restablir l'estat inicial (Tasca 1)
const resetState = () => {
  uiMessage.value = "Esperant comanda...";
  statusColor.value = "primary";
};

// --- Lògica de reacció a la veu (Tasca 1, 2, 3) ---
watch(transcript, (newText) => {
  const command = newText.toLowerCase().trim();
  let commandRecognized = false; // Flag per a la Tasca 3 (Snackbar)
  
  // Lògica de les comandes de l'exercici (Part 1)
  if (command.includes('saluda')) {
    uiMessage.value = "Hola! Benvingut a l'aplicació.";
    statusColor.value = "success";
    alert("Hola!"); 
    commandRecognized = true;
  } 
  else if (command.includes('ajuda')) {
    uiMessage.value = "Aquesta és una prova de concepte.";
    statusColor.value = "info";
    commandRecognized = true;
  }
  // Tasca 1: La comanda "Esborra"
  else if (command.includes('esborra') || command.includes('borrar')) {
    resetState();
    commandRecognized = true;
  }
  // Tasca 2: Control del Tema (Dark/Light Mode)
  else if (command.includes('fosc')) {
    theme.global.name.value = 'dark';
    uiMessage.value = "Tema canviat a **FOSC**.";
    statusColor.value = "info";
    commandRecognized = true;
  } 
  else if (command.includes('clar')) {
    theme.global.name.value = 'light';
    uiMessage.value = "Tema canviat a **CLAR**.";
    statusColor.value = "info";
    commandRecognized = true;
  }
  
  // Si la comanda NO és reconeguda (Tasca 3)
  if (!commandRecognized) {
    uiMessage.value = `Comanda no reconeguda: "${newText}"`;
    statusColor.value = "warning";
    
    // Mostra el v-snackbar
    snackbarText.value = `Comanda no vàlida: "${newText}"`;
    snackbar.value = true;
  }
  
  // Reinicia l'estat a "Esperant comanda..." un cop finalitzada l'execució
  // La lògica de l'exercici original (Part 1) reinicia a cada `transcript` nou.
  // Es podria millorar per fer el reset després d'un temps, però seguim el patró.
  if (commandRecognized) {
      setTimeout(() => {
        resetState();
      }, 3500); // Es reinicia l'estat de la targeta després de 3.5s 
  }
});

// Aquesta lògica assegura que, si l'API no funciona, l'estat es reinicia.
watch(error, (newError) => {
  if (newError) {
    statusColor.value = "error";
  }
});

</script>

<template>
  <v-container class="d-flex justify-center align-center fill-height">
    <v-card width="400" :color="statusColor" variant="tonal">
      <v-card-item>
        <v-card-title class="text-h5 text-center">Control per Veu</v-card-title>
      </v-card-item>

      <v-card-text class="text-center py-4">
        <div class="mb-4">
          <v-icon 
            :icon="isListening ? 'mdi-microphone' : 'mdi-microphone-off'" 
            size="64"
            :class="{'text-red animate-pulse': isListening}"
          ></v-icon>
        </div>
        
        <p class="text-h6 font-weight-bold">{{ isListening ? 'Escoltant...' : 'En espera' }}</p>
        
        <p v-if="interimTranscript" class="text-caption text-grey">
            Detectant: {{ interimTranscript }}
        </p>
        
        <p class="mt-2 text-body-1" v-html="uiMessage"></p>
        
        <v-alert v-if="error" type="error" class="mt-3" density="compact">{{ error }}</v-alert>
      </v-card-text>

      <v-card-actions class="justify-center pb-4">
        <v-btn 
          variant="elevated" color="primary" size="large"
          @click="start" :disabled="isListening"
        >
          Escolta
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-container>
  
  <v-snackbar
    v-model="snackbar"
    :timeout="snackbarTimeout"
    color="error"
    location="bottom"
  >
    {{ snackbarText }}
    <template v-slot:actions>
      <v-btn
        color="white"
        variant="text"
        @click="snackbar = false"
      >
        Tanca
      </v-btn>
    </template>
  </v-snackbar>
</template>

<style scoped>
.animate-pulse { animation: pulse 1.5s infinite; }
@keyframes pulse {
  0% { opacity: 1; transform: scale(1); }
  50% { opacity: 0.5; transform: scale(1.1); }
  100% { opacity: 1; transform: scale(1); }
}
</style>