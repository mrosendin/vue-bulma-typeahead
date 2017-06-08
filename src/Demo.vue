<template>
  <div id="demo">
    <section class="hero is-primary">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">
            Vue Bulma Typeahead Demo
          </h1>
        </div>
      </div>
    </section>

    <div class="container">

      <div class="notification is-danger" v-if="error">
        <button class="delete" @click="error = ''"></button>
        {{ error }}
      </div>

      <div class="columns">
        <div class="column is-4 is-offset-4">
          <label class="label">Name</label>
          <p class="control">
            <!-- Start Typeahead Component -->
            <typeahead></typeahead>
            <!-- End Typeahead Component -->
          </p>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import Typeahead from './Typeahead.vue'
import axios from 'axios';

export default {
  name: 'demo',
  components: { Typeahead },
  data () {
    return {
      error: ''
    }
  }
}
</script>

<style scoped>
.hero {
  margin-bottom: 35px;
}
</style>
