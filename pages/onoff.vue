<template>
  <div>
    <h1>
      {{ isOn ? 'On' : 'Off' }}
    </h1>
    <v-button @click="isOn = !isOn">
      {{ isOn ? 'click to turn off' : 'click to turn on' }}
    </v-button>
  </div>
</template>
<script>
  export default {
    name: 'OnOff',
    data() {
      return {
        isOn: true,
      };
    }
  }
</script>
