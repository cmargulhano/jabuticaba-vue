<template>
  <v-layout justify-left>
    <user-list/>
  </v-layout>
</template>

<script>
import userList from '~/components/user-list.vue'
import Vuex from 'vuex'

export default {
  layout: 'complex',
  components: {
    userList,
  },
  data () {
    return {}
  }
}
</script>

<style>
</style>
