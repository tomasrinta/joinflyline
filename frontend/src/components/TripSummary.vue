<template>
  <div class="summary__inner">
    <div class="summary__heading-wrapper">
      <h3 class="summary__heading">
        Trip Summary
        <span
          >{{ flightToBook.nightsInDest }} Night{{
            flightToBook.nightsInDest > 1 ? "s" : ""
          }}
          Stay in {{ flightToBook.cityTo }}</span
        >
      </h3>
    </div>
    <!-- Flights -->
    <div class="summary__flights">
      <div class="row">
        <div class="col-12 col-md-6">
          <div class="summary__departure">
            <h3
              class="box-title"
              style="font-family: dona-bold; font-size: 1rem;"
            >
              Departure
            </h3>
            <booking-flight-details
              v-for="flight in departureFlights"
              :key="flight.id"
              :flight="flight"
            />
          </div>
        </div>
        <div class="col-12 col-md-6">
          <div class="summary__return">
            <h3
              class="box-title"
              style="font-family: dona-bold; font-size: 1rem;"
            >
              Return
            </h3>
            <booking-flight-details
              v-for="flight in returnFlights"
              :key="flight.id"
              :flight="flight"
            />
          </div>
        </div>
      </div>
    </div>
    <!-- End flights -->
  </div>
</template>

<script>
import BookingFlightDetails from "./BookingFlightDetails";
import Vuex from "vuex";

export default {
  name: "TripSummary",
  components: {
    BookingFlightDetails
  },
  computed: {
    ...Vuex.mapGetters("search", [
      "flightToBook",
      "departureFlights",
      "returnFlights"
    ])
  }
};
</script>
