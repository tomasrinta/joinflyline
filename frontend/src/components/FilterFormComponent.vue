<template>
  <div class="result__filters-aside">
    <div class="result__filters-info" v-if="user.anonymous">
      <div class="result__filters-count">
        Available Results : {{ searchResults.length }}
      </div>
      <router-link :to="{ name: 'sign-in' }" class="result__filters-info-link">
        Sign up to view all
      </router-link>
    </div>
    <form action="" class="bg-white flex-grow-1 result__filters-form">
      <overlay-component
        v-if="user.anonymous"
        :link="{ name: 'get-started' }"
        label="Start Trial"
        class="text-center"
      >
        <h5 class="overlay-component__heading">
          Join for Free to View Flight Filters
        </h5>
        <p class="overlay-component__text">
          Sign up for your free 14 day trial of either Basic or Premium to see
          all that FlyLine has to offer.
        </p>
        <template #footer>
          <br />
          <br />
          <p>
            Want to learn more about FlyLine memberships? <br />
            Click
            <router-link :to="{ name: 'membership-explained' }"
              >HERE</router-link
            >
          </p>
        </template>
      </overlay-component>
      <div class="search-filter">
        <div class="search-filter__header">
          <div class="search-quick-filter-filter-name right-pane">
            <span>Flight Filters</span>
            <h3 class="filter-sidebar__title">
              <span @click="clearFiltersAndUpdate">Clear Filters</span>
            </h3>
          </div>
        </div>
        <div
          class="search-filter__body"
          :class="{ 'search-filter__body--logged': !user.anonymous }"
        >
          <collapse :collapsed="airlineCollapse" title="Filter By Airline">
            <airline-filter
              :data="filterableAirlines"
              @select="toggleAirline"
            />
          </collapse>
          <collapse title="Filter By Time">
            <time-filter />
          </collapse>
          <collapse title="Filter By Stops">
            <max-stops-filter :data="form.maxStops" @select="setMaxStops" />
          </collapse>
          <collapse title="Filter By Price">
            <price-filter @select="setPriceRange" />
          </collapse>
          <button
            type="button"
            class="btn btn-primary w-100"
            @click="search({ clearFilters: false, saveSearch: false })"
          >
            Update Result
          </button>
        </div>
        <p class="search-filter__bottom_text">
          Additional baggage fees may apply.
        </p>
      </div>
    </form>
  </div>
</template>

<script>
import Collapse from "./Collapse";
import AirlineFilter from "./AirlineFilter";
import TimeFilter from "./TimeFilter";
import MaxStopsFilter from "./MaxStopsFilter";
import PriceFilter from "./PriceFilter";
import OverlayComponent from "./OverlayComponent";
import Vuex from "vuex";

export default {
  props: {
    airlineCollapse: {
      type: Boolean,
      default: false,
      required: false
    }
  },
  name: "FilterFormComponent",
  methods: {
    ...Vuex.mapActions("search", [
      "sortResultsBy",
      "loadMore",
      "search",
      "clearFiltersAndUpdate"
    ]),
    ...Vuex.mapMutations("search", [
      "toggleAirline",
      "setMaxStops",
      "setPriceRange"
    ])
  },
  computed: {
    ...Vuex.mapState("search", ["form", "searchResults"]),
    ...Vuex.mapState("user", ["user"]),
    ...Vuex.mapGetters("search", [
      "cityFromTo",
      "airlineNames",
      "filterableAirlines",
      "quickFiltersData",
      "finalResults"
    ])
  },
  components: {
    Collapse,
    AirlineFilter,
    TimeFilter,
    MaxStopsFilter,
    PriceFilter,
    OverlayComponent
  }
};
</script>
<style>
form {
  position: relative;
}
</style>
