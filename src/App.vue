<template>
  p
  <roleServiceVue></roleServiceVue>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import roleServiceVue from './components/role-service.vue';
export default defineComponent({
  name: 'App',

  components: {
    roleServiceVue,
  },

  data() {
    return {
      //
    };
  },
});
</script>
<style></style>
