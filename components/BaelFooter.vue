<template>

  <footer class="fill-gray-lighter xs-text-6 md-text-5">
    <div class="r no-gap">
      <div
        v-if="pagination"
        class="bg-white px-4 py-3 flex items-center justify-between border-t border-gray-200 sm:px-6"
      >
        <div class="flex-1 flex justify-between sm:hidden">
          <nuxt-link
            :to="`?page=${prevpage}`"
            :class="{ 'opacity-50 cursor-not-allowed': this.prevpage <= 0 }"
            class="relative inline-flex items-center px-4 py-2 border border-gray-300 text-sm leading-5 font-medium rounded-md text-gray-700 bg-white hover:text-gray-500 focus:outline-none focus:shadow-outline-blue focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150"
          >Previous</nuxt-link>
          <nuxt-link
            :to="`?page=${nextpage}`"
            no-prefetch
            :class="{ 'opacity-50 cursor-not-allowed': this.queryParam >= this.totalpages }"
            class="ml-3 relative inline-flex items-center px-4 py-2 border border-gray-300 text-sm leading-5 font-medium rounded-md text-gray-700 bg-white hover:text-gray-500 focus:outline-none focus:shadow-outline-blue focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150"
          >Next</nuxt-link>
        </div>
        <div class="bg-white px-4 py-3 flex items-center justify-between border-t border-gray-200 sm:px-6">
          <div>
            <p class="text-sm leading-5 text-gray-700">
              Showing
              <span class="font-medium">{{ 1 + this.$store.state.gridOffset}}</span>
              to
              <span class="font-medium">{{this.pageCount}}</span>
              of
              <span class="font-medium">{{this.$store.state.resultsnum}}</span>
              results
            </p>
          </div>
          <div>
            <nav class="relative z-0 inline-flex shadow-sm">
              <nuxt-link
                tag="button"
                :to="`?page=${prevpage}`"
                :class="{ 'opacity-50 cursor-not-allowed': this.prevpage <= 0 }"
                :disabled="this.prevpage <= 0"
                class="relative inline-flex items-center px-2 py-2 rounded-l-md border border-gray-300 bg-white text-sm leading-5 font-medium text-gray-500 hover:text-gray-400 focus:z-10 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-100 active:text-gray-500 transition ease-in-out duration-150"
                aria-label="Previous"
              >
                <svg class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                  <path
                    fill-rule="evenodd"
                    d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
                    clip-rule="evenodd"
                  />
                </svg>
              </nuxt-link>

              <nuxt-link
                tag="button"
                :to="`?page=${nextpage}`"
                no-prefetch
                :class="{ 'opacity-50 cursor-not-allowed': this.queryParam >= this.totalpages }"
                :disabled="this.queryParam >= this.totalpages"
                class="-ml-px relative inline-flex items-center px-2 py-2 rounded-r-md border border-gray-300 bg-white text-sm leading-5 font-medium text-gray-500 hover:text-gray-400 focus:z-10 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-100 active:text-gray-500 transition ease-in-out duration-150"
                aria-label="Next"
              >
                <svg class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                  <path
                    fill-rule="evenodd"
                    d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
                    clip-rule="evenodd"
                  />
                </svg>
              </nuxt-link>
            </nav>
          </div>
        </div>
      </div>
      <div id="footer" class="w-full bg-blue-900">
        <div class="flex flex-wrap text-center text-white">
<div class="container bg-grey-lighter p-8">
    <div class="sm:flex mb-4">
  <div class="sm:w-1/4 h-auto">
       <div class="xs-text-left xs-p2 xs-border" :class="signupAboutSize">
       
            <div v-show="siteDescription" class="footer__heading my-6 ml-3 text-xl font-semibold ">About</div>
            <p v-show="siteDescription">{{siteDescription}}</p>
          </div>
        
           
  </div>
  <div class="sm:w-1/4 h-auto sm:mt-0 mt-8">
      
                <div class="" :class="signupAboutSize">
          <div class="item">
            <div v-show="connectData" class="footer__heading xs-mb2 my-6 text-xl font-semibold">CONNECT</div>
            <ul class="list-unstyled">
              <li v-show="connectData" v-for="(c,i) in connectData" :key="i">
                <a :href="c.url">{{c.name}}</a> 
              </li>
            </ul>
          </div>
        </div>

  </div>
  <div class="sm:w-1/4 h-auto sm:mt-0 mt-8">
              <div class="" :class="signupAboutSize">
          <div class="item">
            <div class="footer__heading my-6 ml-3 text-xl font-semibold ">Deploy</div>

            <a  
              href="https://app.netlify.com/start/deploy?repository=https://github.com/jake-101/bael-template"
            >
              <img
                style="height:29px;width:auto;"
                src="~/assets/deploy.svg"
                title="Deploy to Netlify"
              />
            </a>
          </div>
        </div>
       
  </div>
 <div class="sm:w-1/2 sm:mt-0 mt-8 h-auto">
      <div v-if="signupBoolean" class="c-25 xs-text-left xs-p2 xs-border">
          <div v-if="!sent" class="">
            <div class="footer__heading my-6 ml-3 text-xl font-semibold">Newsletter Signup</div>
            <form @submit.prevent="processForm" action="/.netlify/functions/app" name="mailinglist">
              <input
                type="email"
                v-model="emaildata.email"
                class="text-input text-input--small xs-mb1 xs-mr2"
                placeholder="you@email.com"
              />
              <button type="submit" class="button button--transparent button--small text-blue bg-blue-800 rounded-lg py-2 px-5 shadow-lg text-green-100 text-sm hover:bg-blue-900">Submit</button>
            </form>
          </div>
          <div class="item" v-else>
            <div class="xs-mb2">{{emaildata.email}} has been added to our newsletter.</div>
          </div>
        </div>
 </div>  

</div>
</div>
  </div>  
   <!-- Start footer bottom -->

        <div class="pt-4 md:flex md:items-center md:justify-center " style="border-top:1px solid white ">
           <div class="item xs-text-6 text-white ">
          <a href="https://github.com/jake-101/bael-template">Bael</a>, An open source design by
          <a href="https://jake101.com">jake101</a>
        </div>
            </div>


        <!-- end container -->
    </div>
</div>
        
     
   
  </footer>


</template>

<script>
export default {
  props: ["pagination"],
  watchQuery: ["page"],
  data() {
    return {
      emaildata: {
        email: "",
      },
      sent: false,
    };
  },
  methods: {
    async processForm() {
      try {
        const sendgrid = await this.$axios.post(
          `${process.env.API_URL}/.netlify/functions/app`,
          this.emaildata
        );
        console.log("Processed!");
        this.sent = true;
      } catch (e) {
        console.log(e);
      }
    },
  },
  computed: {
    nextCheck() {
      if (this.nextpage > this.queryParam) {
        return true;
      } else {
        return false;
      }
    },
    pageCount() {
      var tp =
        this.$store.state.gridNumPosts * 1 + this.$store.state.gridOffset * 1;
      if (tp > this.$store.state.resultsnum) {
        return this.$store.state.resultsnum;
      } else {
        return tp + 1;
      }
    },
    signupAboutSize: function () {
      return {
        "c-25": this.signupBoolean,
        "c-4": !this.signupBoolean,
      };
    },
    prevpage() {
      var prev = Number(this.queryParam) - 1;
      return prev;
    },
    totalpages() {
      var res = this.$store.state.resultsnum;
      var total = Math.ceil(res / 12);
      return total;
    },
    nextpage() {
      var next = Number(this.queryParam) + 1;
      return next;
    },
    queryParam() {
      if (this.$route.query.page == null) {
        return 1;
      } else {
        return Number(this.$route.query.page);
      }
    },

    connectData() {
      return this.$store.state.connect.connectlinks;
    },
    siteDescription() {
      return this.$store.state.siteInfo.sitedescription;
    },
    signupBoolean() {
      return this.$store.state.siteInfo.emailsignup;
    },
  },
};
</script>

<style scoped>
.text-input {
  max-width: 100%;
}

.footer__heading {
  max-width: 100%;
}
</style>
 