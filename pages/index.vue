<template>
   <section class="IndexPage min-h-screen pb-8">
   <div class="text-center container mx-auto max-w-md">
     <h1 class="font-thin py-8 px-2">Vue.js Contributors</h1>
     <h2 class="font-thin text-green-darker">By <a href="https://github.com/artdvp/vue-nuxt-contributors" class="link_ex" target="_blank">Artdvp</a> &nbsp;<label class="i-love">❤</label> Vue</h2>
     <br>
     <div v-if="contributors.length === 0" class="loading">
            Loading...
     </div>
     <ul class="list-reset">
       <li v-for="contributor in contributors" :key="contributor.login" class="p-2">
         <nuxt-link :to="contributor.login" class="link flex items-center no-underline rounded-full p-1 border-2 bg-transparent hover:bg-green">
           <img class="w-16 h-16 rounded-full mr-4 bg-white" :src="`https://avatars1.githubusercontent.com/u/${contributor.id}?v=4&s=400`" alt="Avatar"/>
           <p class="flex-grow text-left text-white text-2xl font-light leading-none">{{ contributor.login }}</p>
           <p class="text-2xl text-white pr-4">
             {{ contributor.contributions }} <span class="font-thin">contributions</span>
           </p>
         </nuxt-link>
       </li>
     </ul>
   </div>
   <!-- <h4>Inspiration from Frontend Developer Love Conference in Amsterdam</h4> -->
  </section>
</template>

<script>
import axios from 'axios'

export default {
  //  async asyncData({ app, error }) {
  //   const contributors = await app.$axios.$get(
  //     "https://api.github.com/repos/vuejs/vue/contributors"
  //   );
  //   console.log(contributors);
  //   return { contributors };
  // },
  data() {
    return {
      bottom: false,
      pageOldCount: 0,
      pageCount: 1,
      contributors: []
    }
  },
  watch: {
    bottom(bottom) {
      if (bottom) {
        this.pageCount++
        this.getContribute()
      }
    }
  },
  created() {
    if (process.browser) {
      window.addEventListener('scroll', () => {
        this.bottom = this.bottomVisible()
      })
      this.getContribute()
    }
  },
  methods: {
    bottomVisible() {
      const scrollY = window.scrollY
      const visible = document.documentElement.clientHeight
      const pageHeight = document.documentElement.scrollHeight
      const bottomOfPage = visible + scrollY >= pageHeight
      return bottomOfPage || pageHeight < visible
    },
    async getContribute() {
      if (this.pageOldCount !== this.pageCount) {
        let _url = `https://api.github.com/repos/vuejs/vue/contributors?page=${this.pageCount}`

        const rx = await axios.get(_url).then(res => {
          let con = res.data
          if (con) {
            if (con.length > 0) {
              this.contributors.push(...con)
              this.pageOldCount = this.pageCount
              if (this.bottomVisible()) {
                this.getContribute()
              }
            } else {
              this.pageCount = this.pageOldCount
            }
          } else {
            this.pageCount = this.pageOldCount
          }
        })
      }
    }
  },
  head: {
    title: 'Vue.js contributos',
    meta: [{ hid: 'vue contributors', name: 'nuxtjs', content: 'vue contributors' }]
  }
}
</script>

<style>
.IndexPage {
  background-image: linear-gradient(236deg, #53e16b, #acd1e5);
}

.link {
  border-color: #fff;
  background-color: #34495e;
}

h1 {
  color: #34495e;
  font-family: Helvetica, Arial, sans-serif;
  font-size: 3rem;
}

.footer-text {
  padding-top: 2rem;
}

.footer-text a {
  text-decoration: none;
}

.i-love {
  color: red;
}

.link_ex {
  color: green;
  text-decoration: none;
}

.loading {
  color: white;
  text-align: center;
  font-size: 20px;
}
</style>
