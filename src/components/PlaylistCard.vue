<script>
import { mapActions, mapState, mapWritableState } from 'pinia';
import { useMainStore } from '../stores/main';

export default {
  props: ['pl'],
  setup() {
    const store = useMainStore()
    return { store }
  },
  methods: {
    ...mapActions(useMainStore, ['fetchWeather', 'enhanceSearchQuery', 'fetchTracks', 'getModalTitle']),
    goToPage(url) {
      window.open(url, "_blank");
    },
    seeTracks(plName, url) {
      const title = this.store.getModalTitle()

      Swal.fire({
        title: title,
        timer: 2000,
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

      this.fetchTracks(plName, url)
    }

  },
  computed: {
    getSummary() {
      return this.pl.playlist_title?.length > 28
        ? this.pl.playlist_title.substring(0, 28) + '...'
        : this.pl.playlist_title
    }
  }
}
</script>

<template>
  <div class="playlist-card w-[300px] flex flex-col justify-start drop-shadow-xl">
    <div class="playlist-image overflow-hidden object-bottom rounded-t-xl w-[300px] h-[300px]">
      <img @click.prevent="goToPage(pl.playlist_link)" :src="pl.playlist_image"
        class=" object-cover h-[300px] w-[300px] cursor-pointer hover:scale-110 transition-all  " />
    </div>
    <div
      class="content-info w-full bg-theme_red px-4 pb-4 pt-2 flex flex-col justify-center items-center rounded-b-xl  relative">
      <p class="playlist-title font-bold text-white">{{ getSummary }}</p>
      <p class=" mb-4 text-white">by: {{ pl.playlist_owner }}</p>
      <button @click.prevent="seeTracks(pl.playlist_title, pl.playlist_tracks)"
        class=" bg-white px-4 py-2 text-[1rem] w-auto rounded-3xl text-theme_red font-semibold hover:scale-110 hover:bg-red-300 transition-all italic">See
        tracks</button>
    </div>
  </div>
</template>