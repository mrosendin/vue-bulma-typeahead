<template>
  <div id="typeahead">
    <input v-model="query" class="input" type="text" placeholder="Search for users...">
  </div>
</template>

<script>
export default {
  name: 'typeahead',
  props: {
    source: {
      type: Array,
      default: () => { return [] }
    }
  },
  data () {
    return {
      query: ''
    }
  },
  watch: {
    query: (newValue, oldValue) => {
      console.log(`Old value: ${oldValue}, new value: ${newValue}`)
    }
  },
  methods: {}
}
</script>

<style lang="sass">
@import "~bulma/sass/utilities/_all"
@import "~bulma/sass/elements/form.sass"
</style>
