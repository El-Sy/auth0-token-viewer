<script lang="ts">
import { useAuth0 } from '@auth0/auth0-vue'
import { onBeforeMount, ref } from 'vue'

export default {
  setup() {
    const { loginWithRedirect, getAccessTokenSilently, isAuthenticated, user, logout } = useAuth0()
    const token = ref<string | null>(null)

    onBeforeMount(async () => {
      token.value = await getAccessTokenSilently()
      console.log(token.value)
    })

    return {
      login: () => {
        loginWithRedirect()
      },
      user,
      token,
      isAuthenticated,
      logout: () => {
        token.value = null
        logout({
          openUrl: false
        })
      }
    }
  }
}
</script>

<template>
  <main>
    <button v-if="!isAuthenticated" @click="login">Log in</button>
    <div v-if="user" >
      <label><b>User</b></label>
      <pre
      style="text-wrap: wrap;"
      >{{ user }}</pre>
    </div>
    <br />
    <div v-if="token" >
      <label><b>JWT Token</b></label>
      <textarea style="width: 100%" rows="15" v-model="token" readonly />
    </div>
    <br />
    <button v-if="isAuthenticated" @click="logout()">Logout</button>
  </main>
</template>
