<template>
  <div class="box">
    <span class="f-foreground">command not found: {{ $store.state.input }}! 😵‍💫</span>
    <span class="f-foreground">Try typing 'help' to see known commands 😎</span>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'ReturnHelp'
})
</script>
