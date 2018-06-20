<template>
  <section class="Details flex content-end flex-wrap h-screen">
      <div class="text-center container mx-auto max-w-md pt-8">
        <div class="group p-4 bg-white rounded w-full shadow-lg select-none overflow-hidden mx-auto">
        <div class="py-8">
            <img class="w-64 h-64 rounded-full shadow-lg" :src="`https://avatars1.githubusercontent.com/u/${contributor.id}?v=4&s=400`" alt="Avatar">
        </div>
        <h2 class="text-5xl font-normal pb-6 px-2"><a class="text_ex hover:text-black" :href="`https://www.github.com/${$route.params.contributor}`" target="_blank">{{ $route.params.contributor }}</a></h2>
        <p class="text-2xl text-grey-darker text-base">{{ contributor.name }}</p>
        <ul class="list-reset py-8 flex text-center font-thin text-xl">
            <li class="flex-1">
                <span class="inline-block bg-grey-lighter rounded-full px-3 py-1 text-md font-semibold text-grey-darker mr-2">
                    {{ contributor.public_repos }} Public Repos
                </span>
            </li>
            <li class="flex-1">
                <span class="inline-block bg-grey-lighter rounded-full px-3 py-1 text-md font-semibold text-grey-darker mr-2">
                    {{ contributor.followers }} followers
                </span>
           </li>
            <li class="flex-1">
                <span class="inline-block bg-grey-lighter rounded-full px-3 py-1 text-md font-semibold text-grey-darker mr-2">
                    {{ contributor.following }} following
                </span>
            </li>
        </ul>

        <br>
        <nuxt-link to="/" class="no-underline hover:bg-black text-grey hover:text-white bg-white text-grey-darkest py-1 px-2 border border-grey-light rounded shadow">Back</nuxt-link>
        </div>
      </div>
  </section>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      contributor: {}
    }
  },
  async asyncData({ app, params, error }) {
    const { data } = await axios.get(`https://api.github.com/users/${params.contributor}`)
    const contributor = data
    return { contributor }
  },
  created() {
    if (process.browser) {
      this.getContributor()
    }
  },
  methods: {
    async getContributor() {
      let rx = await axios
        .get(`https://api.github.com/users/${this.$route.params.contributor}`)
        .then(res => {
          let con = res.data
          this.contributor = con
        })
    }
  },
  head() {
    return {
      title: `${this.contributor.login} contributions`
    }
  }
}
</script>

<style>
.back-list {
  font-size: 36px;
  font-weight: 300;
  text-decoration: none;
  border: 1px solid #000;
  width: 50px;
  height: 50px;
  line-height: 50px;
  text-align: center;
}

.Details {
  background-image: linear-gradient(236deg, #acd1e5, #7ced8f);
}

.text_ex {
  text-decoration: none;
  color: green;
}
</style>
