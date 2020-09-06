<template>
  <nav ref="navBar" id="navbar" class="sm-border-b-0 z-50">
    <div class="flex justify-between flex-wrap bg-purple-500 p-6">
      <div class="c-4 xs-text-left xs-p2 sm-border-r">
        <div class="item">
          <nuxt-link class="sitename" to="/" exact>{{headerSiteName}}</nuxt-link>
        </div>
      </div>

      <div
        v-if="blogtitle"
        style="z-index:55;"
        class="c-12 xs-border-t xs-border-b xs-p2 xs-text-6 titlebar"
      >
        <div class="item">
          <nuxt-link to="/" exact>Home</nuxt-link>
          <span v-show="crumb">
            &nbsp;
            <span class="text-gray-lightest">></span>
            &nbsp; {{thecrumb}}
          </span> &nbsp;
          <span class="text-gray-lightest">></span>
          &nbsp; {{blogtitle}}
        </div>
      </div>
      
      
       <div class="relative mx-auto text-gray-600 lg:block hidden">
        <VueFuse
          class="border-2 border-gray-300 bg-white h-10 pl-2 pr-8 rounded-lg text-sm focus:outline-none"
          placeholder="Search..."
          :compResults="compResults"
          :keys="keys"
          :list="allPosts"
          event-name="searchChanged"
        /> 
        
    
        <button type="submit" class="absolute right-0 top-0 mt-3 mr-2">
          <svg
            class="text-gray-600 h-4 w-4 fill-current"
            xmlns="http://www.w3.org/2000/svg"
            version="1.1"
            id="Capa_1"
            x="0px"
            y="0px"
            viewBox="0 0 56.966 56.966"
            style="enable-background:new 0 0 56.966 56.966;"
            xml:space="preserve"
            width="512px"
            height="512px"
          >
            <path
              d="M55.146,51.887L41.588,37.786c3.486-4.144,5.396-9.358,5.396-14.786c0-12.682-10.318-23-23-23s-23,10.318-23,23  s10.318,23,23,23c4.761,0,9.298-1.436,13.177-4.162l13.661,14.208c0.571,0.593,1.339,0.92,2.162,0.92  c0.779,0,1.518-0.297,2.079-0.837C56.255,54.982,56.293,53.08,55.146,51.887z M23.984,6c9.374,0,17,7.626,17,17s-7.626,17-17,17  s-17-7.626-17-17S14.61,6,23.984,6z"
            />
          </svg>
        </button>
        
      </div>
     </div> 
  </nav>
</template>
<script>
import VueFuse from "~/components/VueFuse";
export default {
  props: ["blogtitle", "posts", "thecrumb"],
  data() {
    return {
      results: [],
      keys: [
        {
          name: "title",
          weight: 0.3,
        },
        {
          name: "body",
          weight: 0.7,
        },
      ],

      compResults: [],
    };
  },
  components: { VueFuse },
  computed: {
    allPosts() {
      let posts = this.$store.state.blogPosts;
      let pages = this.$store.state.allPages;
      let both = posts.concat(pages);
      return both;
    },
    headerSiteName() {
      return this.$store.state.siteInfo.sitename;
    },
    componentResults() {
      return this.$store.state.results;
    },
    crumb() {
      return this.$store.state.theCrumb;
    },
  },

  methods: {
    navHeight() {
      var height = document.getElementById("navbar").clientHeight;
      console.log(height);
      this.$store.commit("SET_NAVHEIGHT", height - 1);
    },
  },

  mounted() {
    this.$on("searchChanged", (results) => {
      this.compResults = results;
    });
  },
};
</script>
<style>
.titlebar .item {
  overflow-x: none;
}
.results {
  padding-left: 0;
  transform: translateY(17px);
  width: 30vw;
}
.results li {
  list-style-type: none;
  margin-left: 0;
}
nav {
  position: fixed;
  background: white;
  top: 0;
  left: 0;
  right: 0;
}
.sitename {
  color: #000;
  font-family: "Archivo Black", sans-serif;
  text-transform: uppercase;
  font-weight: 400;
  font-size: 18px;
}
@media only screen and (max-width: 40rem) {
  .results {
    width: 94vw;
  }
}
</style>  