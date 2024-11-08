<script>
import { mapActions, mapState, mapWritableState } from 'pinia';
import { useMainStore } from '../stores/main';
import PlaylistCard from '../components/PlaylistCard.vue'


export default {
  components: {
    PlaylistCard
  },
  computed: {
    ...mapState(useMainStore, ['AIsuggestion', 'fetchedPlaylists', 'currentWeather']),
  },
  methods: {
    ...mapActions(useMainStore, ['fetchWeather', 'enhanceSearchQuery', 'getModalTitle']),
    regenerate() {

      const title = this.getModalTitle()
      Swal.fire({
        title: title,
        timer: 3500,
        timerProgressBar: true,
        didOpen: () => {
          Swal.showLoading()
        },
      }).then((result) => {
        /* Read more about handling dismissals below */
        if (result.dismiss === Swal.DismissReason.timer) {
          console.log('I was closed by the timer')
        }
      })

      this.enhanceSearchQuery(this.currentWeather.weather[0].description)
    },
  },
  created() {
    if (Object.keys(this.currentWeather).length === 0) {
      this.$router.push('/')
    }
  }

}
</script>

<template>
  <div class="second-main-page flex flex-col justify-around items-center px-4 pb-8"
    :class="fetchedPlaylists.length === 0 ? 'h-[91vh]' : ''">
    <img @click.prevent="$router.push('/')" src="/images/arrow-91-128.png"
      class="absolute top-[25px] left-[25px] w-[50px] h-[50px] cursor-pointer hover:scale-110 transition-all" />
    <div class="text">
      <p class=" text-theme_red font-bold text-[3rem] border-red-300 border-b-4 mb-4 mt-8 italic text-center">Generated
        Playlists</p>
      <p class="text-theme_red opacity-70 font-bold text-[1rem] mb-4 italic text-center">Psst.. the AI suggested: " <span
          class="text-red-600 text-[1.3rem]">{{ AIsuggestion }}</span> "</p>
      <div class="start-button text-center">
        <button @click.prevent="regenerate"
          class="bg-theme_red px-4 p-2 mb-6 mt-2 text-[1.5rem] w-[15rem] rounded-3xl text-white font-semibold drop-shadow-md hover:scale-110 hover:bg-red-300 transition-all italic">REGENERATE!</button>
      </div>
    </div>

    <!-- PLAYLIST AREA -->
    <div class="playlist-area w-full p-4 flex justify-center items-center gap-8 flex-wrap bg-transparent mb-[0.2rem]"
      :class="fetchedPlaylists.length === 0 ? 'mt-[12rem]' : ''">
      <!-- PLAYLIST CARD -->
      <PlaylistCard class="" v-for="pl in fetchedPlaylists" :pl="pl" />
      <div v-if="fetchedPlaylists.length === 0" class="no-data">
        <p class="text-theme_red font-bold text-[2rem] mb-[10rem] opacity-70 italic">~ Uh oh, no playlist for you... click
          regenerate! ~</p>
      </div>
    </div>
  </div>
</template>