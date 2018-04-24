<template>
   <section class="IndexPage min-h-screen pb-8">
   <div class="text-center container mx-auto max-w-md">
     <h1 class="font-thin py-8 px-2">Vue.js Contributors</h1>
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
   <div class="footer-text text-center container mx-auto max-w-md">
     <h4>Made by <a href="https://github.com/artdvp" class="link_ex" target="_blank">Artdvp</a> | Made with <label class="i-love">❤</label>&nbsp;<a class="link_ex" href="https://nuxtjs.org/" target="_blank">Nuxt.js</a></h4>
     <br>
      <h4>Inspiration from Frontend Developer Love Conference in Amsterdam</h4>
     </div>
  </section>
</template>

<script>
export default {
   async asyncData({ app, error }) {
    const contributors = await app.$axios.$get(
      "https://api.github.com/repos/vuejs/vue/contributors"
    );
    console.log(contributors);
    return { contributors };
  },
  head: {
    title: 'Vue.js contributos',
    meta: [{ hid: 'description', name: 'description', content: 'Welcome' }]
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
  font-family: Helvetica,Arial , sans-serif;
  font-size:  3rem;
}

.footer-text{
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
}
</style>
