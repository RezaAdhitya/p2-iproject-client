<script>
import { mapActions, mapState, mapWritableState } from 'pinia';
import { useMainStore } from '../stores/main';

export default {
  data() {
    return {
      formData: {
        email: '',
        password: ''
      }
    }
  },
  methods: {
    ...mapActions(useMainStore, ['doLogin', 'callback', 'doSpotifyLogin', 'doGuestLogin']),
    login(formData) {
      this.doLogin(formData)
    },

    spotifyLogin() {
      this.doSpotifyLogin()
    },

    guestLogin() {
      this.doGuestLogin()
    }
  }
}
</script>

<template>
  <div class=" bg-theme_red h-[100vh] py-[3rem] px-[3rem] flex ">
    <div class="login bg-theme_red h-full w-[40%] p-[3rem] flex flex-col justify-center items-center">
      <div class="title font-bold text-[3rem] text-white">Login</div>
      <div class="login-form w-full ">
        <form @submit.prevent="login(formData)">
          <div class="form-content flex flex-col py-4">
            <label class="font-bold text-[1.5rem] text-white">Email</label>
            <input v-model="formData.email" type="text" class="h-12 px-4 mt-2 rounded-3xl shadow-xl" />
          </div>
          <div class="form-content flex flex-col pb-4">
            <label class="font-bold text-[1.5rem] text-white">Password</label>
            <input v-model="formData.password" type="password" autocomplete="on"
              class="h-12 px-4 mt-2 rounded-3xl shadow-xl" />
          </div>
          <div class="form-content flex justify-center items-center pb-6">
            <p class="text-white">Don't have an account? <a @click.prevent="this.$router.push('/register')" href="#"
                class="font-bold text-blue-300 hover:text-[1.3rem] transition-all">sign up</a> here!</p>
          </div>
          <div class="form-content flex justify-center items-center gap-8 mb-4">
            <button type="submit"
              class="bg-theme_red px-4 py-2 w-[8rem] border-white border-2 border-solid rounded-3xl text-white font-semibold drop-shadow-xl hover:scale-110 hover:bg-red-300 transition-all">Login</button>
          </div>
          <div class="form-content flex justify-center items-center gap-8 mb-4">
            <button @click.prevent="guestLogin"
              class="bg-theme_red px-4 py-2 w-[8rem] border-white border-2 border-solid rounded-3xl text-white font-semibold drop-shadow-xl hover:scale-110 hover:bg-red-300 transition-all">Guest
              Login</button>
          </div>
          <div class="form-content flex justify-center items-center gap-8 mb-4">
            <button @click.prevent="spotifyLogin"
              class=" bg-green-500 px-4 py-2 w-[10rem] border-white border-2 border-solid rounded-3xl text-white font-semibold drop-shadow-xl hover:scale-110 hover:bg-green-300 transition-all">Spotify
              Login</button>
          </div>
          <div class="form-content flex justify-center items-center gap-8">
            <GoogleLogin :callback="callback" class="pt-3" />
          </div>
        </form>
      </div>
    </div>
    <div
      class="banners bg-theme_red h-full w-[60%] flex justify-center items-center border-l-4 border-white border-solid">
      <img class="" src="/images/CHILLCLOUDs.png" />
    </div>
  </div>
</template>

<style scoped></style>