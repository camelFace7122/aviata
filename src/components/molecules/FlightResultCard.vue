<template>
  <article class="flight-result-card">
    <div class="flight-result-card__main-block">
      <div class="flight-result-card__main-block-row">
        <div class="flight-result-card__head">
          <AirlineLogo :src="logoSrc" :name="airportName" />
          <p class="text-small">{{ baggageText }}</p>
        </div>
        <div class="flight-result-card__times">
          <Timer
            :day="this.timesData.depDay"
            :time="this.timesData.depTime"
            :iso="this.timesData.depTimeIso"
          />
          <DurationIndicator
            class="flight-result-card__duration-block"
            :duration="this.timesData.travelTime"
            :dep-code="this.timesData.depCode"
            :arr-code="this.timesData.arrCode"
          >
            {{ travelText }}
          </DurationIndicator>
          <Timer
            :day="this.timesData.arrDay"
            :time="this.timesData.arrTime"
            :iso="this.timesData.arrTimeIso"
          />
        </div>
      </div>
      <DurationIndicator
        class="flight-result-card__duration-block"
        :duration="this.timesData.travelTime"
        :dep-code="this.timesData.depCode"
        :arr-code="this.timesData.arrCode"
      >
        {{ travelText }}
      </DurationIndicator>
      <div class="flight-result-card__helper-links">
        <CustomButton type="link">Детали перелета</CustomButton>
        <CustomButton type="link">Условия тарифа</CustomButton>
        <p class="text-grey text-small">{{ refundableText }}</p>
      </div>
    </div>
    <div class="flight-result-card__secondary-block">
      <div class="flight-result-card__secondary-left-block">
        <p class="flight-result-card__price">{{ price }} <span>₸</span></p>
        <CustomButton>Выбрать</CustomButton>
        <p class="flight-result-card__info-text text-small">
          Цена за всех пассажиров
        </p>
      </div>
      <div class="flight-result-card__secondary-right-block">
        <div class="flight-result-card__additional-controls">
          <p class="text-small">{{ baggageText }}</p>
          <CustomButton type="small">+ Добавить багаж</CustomButton>
        </div>
        <p class="flight-result-card__info-text text-small">
          Цена за всех пассажиров
        </p>
      </div>
    </div>
  </article>
</template>

<script>
import AirlineLogo from "@/components/atoms/AirlineLogo";
import DurationIndicator from "@/components/atoms/DurationIndicator";
import Timer from "@/components/atoms/Timer";
import CustomButton from "@/components/atoms/CustomButton";

export default {
  name: "FlightResultCard",
  components: {
    AirlineLogo,
    DurationIndicator,
    Timer,
    CustomButton,
  },
  props: {
    flight: {
      type: Object,
      default: () => ({}),
    },
  },
  computed: {
    baggageText() {
      switch (
        this.flight.itineraries[0][0].segments[0].baggage_options[0].value
      ) {
        case 0: {
          return "Нет багажа";
        }
        case 1: {
          return "1PC";
        }
        case 2: {
          return "2PC";
        }
        default: {
          return `${this.flight.itineraries[0][0].segments[0].baggage_options[0].value} кг`;
        }
      }
    },
    price() {
      return this.flight.itineraries[0][0].price.amount;
    },
    timesData() {
      const segments = this.flight.itineraries[0][0].segments;
      const segmentsCount = segments.length;
      const depTimeString = segments[0].dep_time;
      const arrTimeString = segments[segmentsCount - 1].arr_time;
      let layover = "";
      if (segmentsCount > 1) {
        layover = segments[0].airport_dest;
      }
      return {
        depDay:
          depTimeString.split(" ")[0] +
          " " +
          depTimeString.split(" ")[1] +
          " " +
          depTimeString.split(" ")[2],
        depTime: depTimeString.split(" ")[3],
        depTimeIso: segments[0].dep_time_iso,
        depCode: segments[0].origin_code,
        arrDay:
          arrTimeString.split(" ")[0] +
          " " +
          arrTimeString.split(" ")[1] +
          " " +
          arrTimeString.split(" ")[2],
        arrTime: arrTimeString.split(" ")[3],
        arrTimeIso: segments[segmentsCount - 1].arr_time_iso,
        arrCode: segments[segmentsCount - 1].dest_code,
        layover: layover,
        travelTime: this.secondsToDhm(this.flight.itineraries[0][0].traveltime),
      };
    },
    travelText() {
      if (this.timesData.layover) {
        console.log(this.timesData.layover, this.timesData.travelTime);
        return (
          "через " + this.timesData.layover + ", " + this.timesData.travelTime
        );
      }
      return this.timesData.travelTime;
    },
    refundableText() {
      if (this.flight.refundable) {
        return "возвратный";
      }
      return "невозвратный";
    },
    airportName() {
      return this.flight.itineraries[0][0].carrier_name;
    },
    logoSrc() {
      return `https://aviata.kz/static/airline-logos/80x80/${this.flight.validating_carrier}.png`;
    },
  },
  methods: {
    secondsToDhm(seconds) {
      seconds = Number(seconds);
      var d = Math.floor(seconds / (3600 * 24));
      var h = Math.floor((seconds % (3600 * 24)) / 3600);
      var m = Math.floor((seconds % 3600) / 60);

      var dDisplay = d > 0 ? d + " дн" : "";
      var hDisplay = h > 0 ? h + " ч" : "";
      var mDisplay = m > 0 ? m + " м" : "";
      return `${dDisplay} ${hDisplay} ${mDisplay}`;
    },
  },
};
</script>

<style lang="scss" scoped>
.flight-result-card {
  display: block;
  background-color: $c-white;
  border-radius: 4px;
  overflow: hidden;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.15);
  max-width: 880px;
  @media (min-width: 820px) {
    display: flex;
  }

  &__main-block {
    padding-bottom: 28px;
    @media (min-width: 480px) {
      padding-bottom: 16px;
    }
    @media (min-width: 820px) {
      flex: 1;
    }
  }
  &__main-block-row {
    display: block;
    @media (min-width: 580px) {
      display: flex;
      padding: 20px 20px 10px;
    }
    @media (min-width: 580px) {
      display: flex;
      padding: 20px 20px 10px;
    }
    @media (min-width: 820px) {
      padding: 40px 40px 20px;
    }
  }

  &__head {
    display: flex;
    justify-content: space-between;
    padding: 18px 20px;
    @media (min-width: 580px) {
      padding: 0;
    }
    p.text-small {
      @media (min-width: 480px) {
        display: none;
      }
    }
  }

  &__duration-block {
    margin: 0 auto;
    display: block;
    @media (min-width: 480px) {
      display: none;
    }
  }

  &__times {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 9px 20px;
    margin: 0 auto;
    @media (min-width: 480px) {
      justify-content: center;
    }
    @media (min-width: 580px) {
      padding: 0;
    }
    @media (min-width: 820px) {
      margin: 0 0 0 15px;
    }
    .flight-result-card__duration-block {
      display: none;
      width: 200px;
      padding: 0;
      margin: 0 30px;
      @media (min-width: 480px) {
        display: block;
      }
      @media (min-width: 640px) {
        width: 230px;
      }
      @media (min-width: 780px) {
        width: 300px;
      }
      @media (min-width: 820px) {
        width: 170px;
      }
    }
  }

  &__secondary-block {
    background-color: $c-bg-light;
    text-align: center;
    padding: 17px;
    @media (min-width: 480px) {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
    }
    @media (min-width: 820px) {
      display: block;
      padding: 12px 20px;
    }
    & > * {
      @media (min-width: 480px) {
        flex: 0 0 210px;
      }
    }
  }

  &__info-text {
    color: #707276;
    margin-top: 14px;
  }

  &__secondary-left-block {
    @media (min-width: 640px) {
      display: flex;
      align-items: center;
      flex: 1;
    }
    @media (min-width: 820px) {
      display: block;
    }
    .flight-result-card__info-text {
      display: block;
      @media (min-width: 480px) {
        display: none;
      }
    }
  }

  &__secondary-right-block {
    display: none;
    @media (min-width: 480px) {
      display: block;
    }
    @media (min-width: 820px) {
      display: flex;
      flex-direction: column-reverse;
    }
    & .flight-result-card__info-text {
      @media (min-width: 820px) {
        margin-top: 8px;
        margin-bottom: 12px;
      }
    }
  }

  &__price {
    margin: 0;
    font-size: 26px;
    line-height: 30px;
    margin-bottom: 14px;
    @media (min-width: 640px) {
      margin-bottom: 0;
      margin-left: 20px;
      order: 2;
    }
    @media (min-width: 820px) {
      display: block;
      margin-bottom: 14px;
      margin-left: 0;
    }
    span {
      font-size: 19px;
      line-height: 21px;
    }
  }

  &__additional-controls {
    display: none;
    align-items: center;
    @media (min-width: 480px) {
      display: flex;
      flex: 0 0 210px;
    }
    @media (min-width: 820px) {
      display: flex;
      width: 210px;
      flex: 1;
    }
    & > * + * {
      margin-left: 6px;
    }
    & > p {
      flex: 0 0 70px;
      width: 70px;
    }
  }
  &__helper-links {
    display: none;
    padding: 20px 20px 0;
    @media (min-width: 480px) {
      display: flex;
    }
    @media (min-width: 820px) {
      padding: 20px 40px 0;
    }
    & > button + button {
      margin-left: 23px;
    }
    & > .text-small {
      margin-left: 45px;
    }
  }
  & .text-grey {
    color: #707276;
  }
}
</style>
