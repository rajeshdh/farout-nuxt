<template>  
  <main>
    <div class="flex flex-col mx-4 items-center bg-white-200 px-2 rounded-tb rounded-lr border-solid border-4 border-gray-400 w-screen">
    <div class="full-height single xs-border-left xs-border-right" :style="`min-height:calc(100vh - ${navbarheight}px);margin-top:${navbarheight}px`">
      <div class="xs-mt2 xs-p2 bcg-item">
        <div class="item xs-block xs-full-height">
          <div class="feat-wrapper" v-if="theThumb"><transition appear name="fade"><img class="w-full sm:w-2/3 md:w-3/5 xl:w-2/5 flex mx-auto p-4 pt-8 border-double border-4 border-gray-600" :src="thumbnail" :alt="title"></transition></div>
          <h1 class="xs-py3 main-title my-24 xl:italic">{{title}}</h1>
          <no-ssr>
            <div class="xs-mt-5 bold">
            <ul class="list-unstyled xs-flex xs-flex-align-center">
             <li class="xs-inline-block xs-mr1" v-if="this.$store.state.theCategory"><div class="tag fill-gray-darker xs-border">
  <nuxt-link :to="`/category/${this.$store.state.theCategory.toLowerCase()}`" class="tag__link text-black">{{this.$store.state.theCategory}}</nuxt-link>
</div></li> 
<li class="xs-inline-block">{{ date }}</li>
              </ul> 
             </div>
          </no-ssr>
          <div class="xs-py3 post-content text-gray-lighter">
            <div v-html="$md.render(body)"></div>
          </div>
        </div>
      </div>

    </div>
    </div>
    
  </main>
</template>



<script>
import MdWrapper from "~/components/MdWrapper";

export default {
  async asyncData({ params, app, payload, route, store }) {
    let post = await import("~/content/blog/posts/" + params.slug + ".json");
    console.log(post);
   
   await store.commit("SET_TITLE", post.title);
     await store.commit("SET_THUMB", post.thumbnail);
     await store.commit("SET_CRUMB", 'Blog');
   await   store.commit("SET_POSTCAT", post.category);
    await store.commit("paginateOff", false);
    return post;
  },
   transition (to, from) {
    if (!from) { return 'slide-left' } else {return 'slide-right'}
  },
  head() {
    return {
      title: this.title + " | " + this.$store.state.siteInfo.sitename
    };
  },
  
  data() {
    return {};
  },
  methods: {
    onResize(event) {
      this.navHeight();
      console.log(this.$store.state.navheight);
      console.log("slug resize");
    },
    navHeight() {
      var height = document.getElementById("navbar").clientHeight;
      this.$store.commit("SET_NAVHEIGHT", height);
    }
  },
  updated() {
    if (process.browser) {
      this.$nextTick(() => {
        this.navHeight();
        console.log(this.$store.state.navheight);
        console.log("slug updated");
      });
    }
  },
  mounted() {
    if (process.browser) {
      this.$nextTick(() => {
        this.navHeight();
        window.addEventListener("resize", this.onResize);
        console.log(this.$store.state.navheight);
        console.log("slug mounted");
      });
    }
  },
  beforeDestroy() {
    // Unregister the event listener before destroying this Vue instance
    window.removeEventListener("resize", this.onResize);
  },

  computed: {
    theThumb() {
      return this.$store.state.theThumbnail;
    },
    allBlogPosts() {
      return this.$store.state.blogPosts;
    },
    navbarheight() {
      return this.$store.state.navheight;
    }
  },
  components: {
    MdWrapper
  }
};
</script>
