<script lang="ts" setup>
import { computed,getCurrentInstance } from 'vue';

const usevuetify = () => {
  const instance = getCurrentInstance()
  if (!instance) {
    throw new Error(`useVuetify should be called in setup().`)
  }
  return instance.proxy.$vuetify
}

const answer = computed(function()  { 
  const vuetify = usevuetify()
  console.log(vuetify)
  
  return vuetify.display.xs ? 450 : '250';
})
</script>

<template>
  <v-app>
    <v-card
        width="400"
        :height="answer"
        title="This is a title"
        subtitle="This is a subtitle"
        text="This is content"
      >
    </v-card>
  </v-app>
</template>


