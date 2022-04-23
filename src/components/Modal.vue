<template>
  <div
    class="modal__container"
    tabindex="-1"
    @click.self="close"
    @keydown.esc="close"
  >
    <div class="modal">
      <span class="modal__close" @click="close">X</span>
      <div class="modal__content">
        <span>TYPE: {{ post.type }}</span>
        <span>NAME: {{ post.name }}</span>
        <span>HITS: {{ post.hits }}</span>
        <span>BANDWIDTH: {{ post.bandwidth }}</span>
      </div>
    </div>
  </div>  
</template>
<script>

export default {
  props: {
    post: {
      type: Object
    }
  },
  mounted () {
    this.$el.focus()
  },
  methods: {
    close () {
      this.$emit('close')
    }
  },
}
</script>

<style>
</style>
