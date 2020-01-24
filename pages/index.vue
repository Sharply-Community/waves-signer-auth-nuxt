<template>
  <div class="container">
    <div>
      <logo />
      <h1 class="title">
        Waves Signer
      </h1>
      <h2 class="subtitle">
        Demo project for Waves Signer Authentication
      </h2>
      <div class="links">
        <a
          @click.prevent="callSigner"
          href="#"
          target="_blank"
          class="button--green"
        >
          {{ loginButtonText }}
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import Waves from '@waves/signer'
import Provider from '@waves.exchange/provider-web'
import Logo from '~/components/Logo.vue'

// settings
const waves = new Waves({ NODE_URL: 'https://pool.testnet.wavesnodes.com' })

// setting the Waves.Exchange provider
const provider = new Provider('https://testnet.waves.exchange/signer/')

waves.setProvider(provider)

export default {
  components: {
    Logo
  },
  data() {
    return {
      loginButtonText: 'login',
      isAuthenticated: false
    }
  },
  methods: {
    async callSigner() {
      if (!this.isAuthenticated) {
        this.loginButtonText = 'logging in...'

        try {
          const userData = await waves.login()

          this.loginButtonText = `Signed in as ${userData.address}`

          this.isAuthenticated = true
        } catch (error) {
          this.loginButtonText = error.message
        }
      } else {
        try {
          await waves.logout()

          this.isAuthenticated = false

          this.loginButtonText = 'Login'
        } catch (error) {
          this.loginButtonText = error.message
        }
      }
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
