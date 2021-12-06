<template>
  <div id="app" class="app">
    <div class="app__inner">
      <div>
        <Filters
          v-model="selectedTariffs"
          class="app__filters"
          name="tariffs"
          :options="tariffOptions"
          @reset="resetSelectedTariffs"
        />
        <Filters
          v-model="selectedAirlines"
          class="app__filters"
          name="airlines"
          :options="airlines"
          @reset="resetSelectedAirlines"
        />
        <CustomButton
          type="link"
          class="reset-btn"
          @click.native="resetAllFilters"
          >Сбросить все фильтры</CustomButton
        >
      </div>
      <div>
        <FlightResultCard
          v-for="flight in filteredTickets"
          :key="flight.id"
          :flight="flight"
          class="app__card"
        />
      </div>
    </div>
  </div>
</template>

<script>
import CustomButton from "@/components/atoms/CustomButton";
import FlightResultCard from "@/components/molecules/FlightResultCard";
import Filters from "@/components/molecules/Filters";

import results from "@/data/results.json";

export default {
  name: "App",
  components: {
    CustomButton,
    FlightResultCard,
    Filters,
  },
  data() {
    return {
      results: [],
      tariffOptions: [
        ["direct", "Только прямые"],
        ["baggage", "Только с багажом"],
        ["refundable", "Только возвратные"],
      ],
      selectedTariffs: [],
      selectedAirlines: ["all"],
      filteredTickets: [],
    };
  },
  computed: {
    airlines() {
      if (this.results.airlines) {
        return [["all", "Все"], ...Object.entries(this.results.airlines)];
      }
      return [];
    },
  },
  watch: {
    selectedTariffs: {
      deep: true,
      handler() {
        this.filterTickets();
      },
    },
    selectedAirlines: {
      deep: true,
      handler() {
        this.filterTickets();
      },
    },
  },
  mounted() {
    this.results = results;
    this.filterTickets();
  },
  methods: {
    setSelectedTariffs(selected) {
      this.selectedTariffs = [...selected];
    },
    setSelectedAirlines(selected) {
      this.selectedAirlines = [...selected];
    },
    filterTickets() {
      this.filteredTickets = this.results.flights;
      if (this.selectedTariffs.includes("direct")) {
        this.filteredTickets = this.filteredTickets.filter((flight) => {
          return flight.itineraries[0][0].segments.length === 1;
        });
      }
      if (this.selectedTariffs.includes("baggage")) {
        this.filteredTickets = this.filteredTickets.filter((flight) => {
          return (
            flight.itineraries[0][0].segments[0].baggage_options[0].value !== 0
          );
        });
      }
      if (this.selectedTariffs.includes("refundable")) {
        this.filteredTickets = this.filteredTickets.filter((flight) => {
          return flight.refundable;
        });
      }
      if (this.selectedAirlines.includes("all")) {
        this.filteredTickets = [...this.filteredTickets];
      } else {
        this.filteredTickets = this.filteredTickets.filter((flight) => {
          const hasMatch = this.selectedAirlines.some((airline) => {
            return airline === flight.validating_carrier;
          });
          return hasMatch;
        });
      }
    },
    resetAllFilters() {
      this.selectedTariffs = [];
      this.selectedAirlines = ["all"];
    },
    resetSelectedTariffs() {
      this.selectedTariffs = [];
    },
    resetSelectedAirlines() {
      this.selectedAirlines = ["all"];
    },
  },
};
</script>

<style lang="scss" scoped>
.app {
  padding: 20px;
  background-color: #d7d7d7;
  height: auto;
  & .reset-btn {
    margin-top: 13px;
    margin-bottom: 23px;
  }
  &__filters {
    & + & {
      margin-top: 12px;
    }
  }
  &__card {
    & + & {
      margin-top: 12px;
    }
  }
  &__inner {
    @media (min-width: 1170px) {
      display: flex;
      & > * + * {
        margin-left: 20px;
      }
    }
  }
}
</style>
