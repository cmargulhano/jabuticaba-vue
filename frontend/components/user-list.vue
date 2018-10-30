<template>
  <v-card>
    <confirm ref="confirm"></confirm>
    <v-toolbar color="cyan" dark>
      <v-text-field class="ma-4" v-model="user.name" label="Name" :loading="adding"/>
      <v-text-field class="ma-4" type="email" v-model="user.email" label="E-mail" :loading="adding"/>
      <v-btn color="success" v-on:click="add()">Add User</v-btn>
    </v-toolbar>
    <v-progress-linear :indeterminate="true" v-if="loading"></v-progress-linear>
    <v-list two-line>
      <template v-for="(item, index) in items">
        <v-list-tile :key="item.email">
          <v-list-tile-content>
            <v-list-tile-title>{{item.name}}</v-list-tile-title>
            <v-list-tile-title>{{item.email}}</v-list-tile-title>
          </v-list-tile-content>
          <v-list-tile-action>
            <v-btn icon v-on:click="remove(index)">
              <v-icon color="grey lighten-1">delete</v-icon>
            </v-btn>
          </v-list-tile-action>
        </v-list-tile>
      </template>
    </v-list>
  </v-card>
</template>

<script>
import confirm from '~/components/confirm.vue'
import AppApi from '~apijs'
export default {
  components: {
    confirm
  },
  data () {
    return {
      user: {name:'', email: ''},
      adding: false,
      loading: false,
      items: [
      ]
    }
  },
  mounted () {
    this.loading = true
    AppApi.list_users().then(response => {
      const users = response.data.users
      this.items = users
      this.loading = false
    })
  },
  methods: {
    add() {
      if (!this.user.name || !this.user.email) {
        alert('Name or e-mail cannot be null')
        return
      }
      if (this.items.findIndex(_user => _user.email == this.user.email) >= 0) {
        alert('User already exists')
        return
      }
      this.adding = true
      AppApi.add_user(this.user).then(response => {
        const user = response.data
        this.items.push(user)
        this.user = {name:'', email: ''}
        this.adding = false
      })
    },
    remove(index) {
      this.$refs.confirm.open('Delete', 'Are you sure?', { color: 'red' }).then((confirm) => {
        if (confirm) {
          AppApi.remove_user(this.items[index]).then(response => {
          this.items.splice(index, 1);
          })
        }
      })
    },
  }
}
</script>

<style scoped>
</style>
