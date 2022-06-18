<template>
  <div class="item">
    <div class="balance">
    Your current points: {{points}}   
    </div>
    <div class="btn">
      <router-link to="/game">Join game</router-link>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      count: 0
    }
  },
  computed: {
    points() {
      return 100
    }
  }
}
</script>