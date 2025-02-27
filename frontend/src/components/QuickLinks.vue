<template>
  <div class="search-top-result">
    <div
      v-for="(label, sortBy) in columns"
      :key="sortBy"
      class="search-quick-filter"
      :class="{ applied: sort === sortBy }"
      @click="sortResultsBy(sortBy)"
    >
      <div class="search-quick-filter-filter-name">
        {{ label }}
      </div>
      <div class="search-quick-filter-filter-price-duration">
        <span class="small" v-if="data && data[sortBy]"
          >${{ data[sortBy].price }} -
          {{ secs2hm(data[sortBy].duration) }}</span
        >
        <span class="small" v-if="label === 'Low-Cost'"
          >Southwest and more</span
        >
        <span class="small" v-if="label === 'Legacy'"
          >American Airlines and more</span
        >
      </div>

      <div class="border-line w-75 mx-auto"></div>
      <div class="loading-animation-topsearch">
        <div class="animate-element-topresult"></div>
      </div>
    </div>
    <template v-if="!user.anonymous">
      <div
        class="search-quick-filter"
        :class="{ applied: form.singleCarrier }"
        @click="toggleSingleCarrier"
      >
        <div class="search-quick-filter-filter-name">
          Single Carrier
        </div>
        <div class="search-quick-filter-filter-price-duration">
          <span class="small">{{
            singleCarrierAirlines
              ? `${airlineCodes[singleCarrierAirlines[0]]} and more`
              : ""
          }}</span>
        </div>
        <div class="border-line w-75 mx-auto"></div>
        <div class="loading-animation-topsearch">
          <div class="animate-element-topresult"></div>
        </div>
      </div>
      <div
        v-for="(label, kind) in { legacy: 'Legacy', lowcost: 'Low-cost' }"
        :key="kind"
        class="search-quick-filter"
        :class="{ applied: airlinesFilter === kind }"
        @click="applyAirlinesFilter(kind)"
      >
        <div class="search-quick-filter-filter-name">{{ label }} airlines</div>
        <div class="search-quick-filter-filter-price-duration">
          <span class="small">{{
            airlineFilters[kind] ? `${airlineFilters[kind]} and more` : ""
          }}</span>
        </div>

        <div class="border-line w-75 mx-auto"></div>
        <div class="loading-animation-topsearch">
          <div class="animate-element-topresult"></div>
        </div>
      </div>
    </template>
  </div>
</template>

<script>
import { secs2hm, pick, stripAirlines } from "../utils/utils";
import {
  legacyAirlines,
  lowcostAirlines,
  airlineCodes
} from "../utils/airlineCodes";
import Vuex from "vuex";

export default {
  props: ["data", "sort", "airlinesFilter"],
  delimiters: ["{{", "}}"],
  data() {
    return {
      airlineCodes
    };
  },
  computed: {
    ...Vuex.mapState("user", ["user"]),
    ...Vuex.mapState("search", ["form"]),
    ...Vuex.mapGetters("search", ["singleCarrierAirlines"]),
    airlineFilters() {
      const legacySet = new Set(legacyAirlines);
      const lowcostSet = new Set(lowcostAirlines);
      const legacyAvailable = this.form.airlines.filter(o =>
        legacySet.has(o.code)
      );
      const lowcostAvailable = this.form.airlines.filter(o =>
        lowcostSet.has(o.code)
      );
      return {
        legacy:
          legacyAvailable.length > 0
            ? stripAirlines(pick(legacyAvailable).name)
            : null,
        lowcost:
          lowcostAvailable.length > 0
            ? stripAirlines(pick(lowcostAvailable).name)
            : null
      };
    },
    columns() {
      const additionalCol =
        this.user && this.user.anonymous
          ? { date: "Earliest", quality: "Recommended" }
          : {};
      return {
        price: "Cheapest",
        duration: "Quickest",
        ...additionalCol
      };
    }
  },
  methods: {
    secs2hm,
    ...Vuex.mapActions("search", [
      "sortResultsBy",
      "applyAirlinesFilter",
      "toggleSingleCarrier"
    ])
  }
};
</script>
