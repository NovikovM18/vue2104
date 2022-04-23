<template>
  
  <div class="post">
    <div @click="modalOpen = true">
      {{ post.name }}
    </div>
  </div>

  <modal
    :post="post"
    v-if="modalOpen"
    @close="modalOpen = false"
  />

</template>

<script>
import Modal from './Modal.vue';
export default {
  components: { Modal },
  props: {
    post: {
      type: Object
    }
  },
  data () {
    return {
      modalOpen: false
    }
  }
}
</script>

<style>
</style>