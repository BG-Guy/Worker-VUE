<template>
  <div class="page-content-container" v-if="!isLoading">
    <div class="breadcrumbs" v-if="gigsToShow">
      <span @click="this.$router.push('/')"> FIIVERR ></span>
      <span @click="this.$router.push('/tag')">SERVICES ></span>
      <span> {{ breadcrumbsToShow.toUpperCase() }} ></span>
      <h1>
        {{
          breadcrumbsToShow
            .split(" ")
            .map((word) => word.split("")[0].toUpperCase() + word.substring(1))
            .join(" ")
        }}
      </h1>
      <p>{{ gigsToShow.length }} Services Available</p>
    </div>
    <gigTabsCarousel></gigTabsCarousel>
    <div class="gig-filters">
      <filtereEplore> </filtereEplore>
    </div>
    <ul class="gig-list grid">
      <li class="gig-preview" v-for="gig in gigsToShow" :key="gig._id">
        <gigsPreview :gig="gig" />
      </li>
    </ul>
  </div>
</template>

<script>
import { ref } from "vue";
import { Carousel, Navigation, Slide } from "vue3-carousel";
import gigsPreview from "../components/gigs-preview.vue";
import gigTabsCarousel from "../components/gigs-tabs-carousel.vue";
import filtereEplore from "../components/filter-explore.vue";

export default {
  data() {
    return {
      fullscreenLoading: false,
      filterBy: {
        seller: "",
        price: ref([80, 150]),
        sortBy: "",
      },
    };
  },
  components: {
    gigTabsCarousel,
    gigsPreview,
    Carousel,
    Slide,
    Navigation,
    filtereEplore,
  },
  created() {},
  computed: {
    gigs() {
      return this.$store.getters.gigs;
    },
    isLoading() {
      return this.$store.getters.isLoading;
    },
    gigsToShow() {
      const category = this.$route.params.gig;
      if (!category) return this.gigs;

      const gigsToDisplay = this.gigs.filter((gig) => {
        return gig.category.some((tab) => category.includes(tab));
      });
      return gigsToDisplay;
    },
    breadcrumbsToShow() {
      if (this.$route.params.gig) return this.$route.params.gig;
      return "all gigs";
    },
  },

  methods: {
    setFilter() {
      const filterBy = JSON.parse(JSON.stringify(this.filterBy));
      this.$store.dispatch({ type: "setFilter", filterBy });
    },
    capSentence() {
      return this.breadcrumbsToShow
        .split(" ")
        .map((word) => word.split("")[0].toUpperCase() + word.substring(1))
        .join(" ");
    },
  },
};
</script>
