<template>
  <div class="trips">
    <div class="dashboard-section-heading">
      <div class="section-heading__inner">
        <span class="section-heading__title">Travel Portal / </span>
        <span class="section-heading__title section-heading__title__current"
          >Trips</span
        >
      </div>
    </div>

    <div class="main-padding trips-container">
      <div class="row">
        <div class="col-12">
          <div class="trips-card__wrapper">
            <div class="tile">
              <div class="tile__body">
                <h3 class="trips-card__title">Upcoming Trips</h3>
                <p v-if="upcomingLoading" class="tile__body-no-result">
                  Loading...
                </p>
                <div v-else-if="upcomingTrips.length > 0">
                  <booking-trip
                    v-for="trip in upcomingTrips"
                    :flight="trip"
                    :key="trip.id"
                  />
                </div>
                <!--              <p v-else class="tile__body-no-result">-->
                <!--                No Upcoming Trips, Get Away Today!-->
                <!--              </p>-->
              </div>
            </div>
            <div v-if="upcomingTrips.length === 0" class="tile--no-result">
              <p class="tile--no-result__text">You have no upcoming trips</p>
            </div>
          </div>
        </div>

        <div class="col-12">
          <div class="trips-card__wrapper">
            <div class="tile">
              <div class="tile__body">
                <h3 class="trips-card__title">Previous Trips</h3>
                <p v-if="pastLoading" class="tile__body-no-result">
                  Loading...
                </p>
                <div v-else-if="pastTrips.length > 0">
                  <booking-trip
                    v-for="trip in pastTrips"
                    :flight="trip"
                    :key="trip.id"
                  />
                </div>
                <!--              <p v-else="" class="tile__body-no-result">-->
                <!--                No Previous Trips, Start Booking-->
                <!--              </p>-->
              </div>
            </div>
            <div v-if="pastTrips.length === 0" class="tile--no-result">
              <p class="tile--no-result__text">You have no previous trips</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import api from "../../utils/http";
import BookingTrip from "../BookingTrip";

export default {
  data() {
    return {
      upcomingLoading: true,
      upcomingTrips: [],
      pastLoading: true,
      pastTrips: [],
      searchHistory: []
    };
  },
  components: {
    BookingTrip
  },
  delimiters: ["{{", "}}"],
  methods: {
    processTrips(data) {
      return data.map(o => {
        const innerData = o.data;
        delete o.data;
        o.flights = o.flights.map(f => {
          const innerData = f.data;
          delete f.data;
          return { ...f, ...innerData };
        });
        return { ...o, ...innerData };
      });
    }
  },
  created() {
    api
      .get("/bookings/?kind=upcoming")
      .then(response => {
        this.upcomingTrips = this.processTrips(response.data);
      })
      .finally(() => (this.upcomingLoading = false));
    api
      .get("/bookings/?kind=past")
      .then(response => {
        this.pastTrips = this.processTrips(response.data);
      })
      .finally(() => (this.pastLoading = false));
  }
};
</script>

<style scoped></style>
