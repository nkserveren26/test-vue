<script lang="ts" setup>
import { computed,getCurrentInstance } from 'vue';

const usevuetify = () => {
  const instance = getCurrentInstance()
  if (!instance) {
    throw new Error(`vuetify instance doesn't exist`)
  }
  return instance.proxy.$vuetify
}

const getCardHeight = computed(function()  { 
  try {
    const vuetify = usevuetify();
    return vuetify.display.xs ? 450 : '250';
  } catch(err) {
    console.log(err);

  }
})
</script>

<template>
  <v-app>
    <v-card
        width="400"
        :height="getCardHeight"
        title="This is a title"
        subtitle="This is a subtitle"
        text="This is content"
      >
    </v-card>
  </v-app>
</template>


