<template >

  <div id="slider">
    <div class="bar-left">
      <p @click="showFirstComponent" :class="{ 'v-switch-active': firstComponentVisible, 'v-switch-active-color': !firstComponentVisible }" >Все года</p>
      <p @click="showSecondComponent" :class="{ 'v-switch-active': !firstComponentVisible, 'v-switch-active-color': firstComponentVisible }">Месяцы</p>
    </div>

    <div class="bar-right">
      <div class="MultiRangeSliderContainer" v-if="firstComponentVisible">
        <MultiRangeSlider
            :min="0"
            :max="91"
            :minValue="dayBarMinValue"
            :maxValue="dayBarMaxValue"
            :labels="getAllYearNames"
            :min-caption="dayMinCaption"
            :max-caption="dayMaxCaption"
            :step="1"
            @input="updateDayValues"
        />
      </div>

      <div class="MultiRangeSliderContainer" v-else >
        <MultiRangeSlider
            :min="0"
            :max="91"
            :labels="getElementsBetween"
            :minValue="barMinValue"
            :maxValue="barMaxValue"
            :min-caption="dayMinCaption"
            :max-caption="dayMaxCaption"
            @input="updateWeekValues"
        />
      </div>
    </div>
  </div>

</template>



<script>

import MultiRangeSlider from "multi-range-slider-vue";
import "./style.css"
import "./../../node_modules/multi-range-slider-vue/MultiRangeSliderBlack.css";
import "./../../node_modules/multi-range-slider-vue/MultiRangeSliderBarOnly.css";

export default {
  el: "slider",
  name: "App",
  components: { MultiRangeSlider },

  data() {
    return {
      barMinValue: 0,
      barMaxValue: 91,
      dayBarMinValue: 0,
      dayBarMaxValue: 91,
      wBarMinValue: 1,
      wBarMaxValue: 8,
      hBarMinValue: 120,
      hBarMaxValue: 600,
      sBarMinValue: 0,
      sBarMaxValue: 100,
      oBarMinValue: 10,
      oBarMaxValue: 90,
      firstComponentVisible: true,
    };
  },
  mounted() {
    const element = document.querySelector('.max-caption');
    element.classList.add('speech')
    const element1 = document.querySelector('.min-caption');
    element1.classList.add('speech')
  },

  methods: {
    updateWeekValues(e) {
      this.wBarMinValue = e.minValue;
      this.wBarMaxValue = e.maxValue;
    },
    updateDayValues(e) {
      this.dayBarMinValue = e.minValue;
      this.dayBarMaxValue = e.maxValue;
    },
    showFirstComponent: function() {
    this.firstComponentVisible = true;
    },
    showSecondComponent: function() {
    this.firstComponentVisible = false;
    },
  },


  computed: {

    monthNames() {
      return ["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"];
    },

    yearNames() {
      return [
        "2014",
        "2021"
      ];
    },
    getAllYearNames() {
      let years = [];
      for (let i = 2014; i <= 2021; i++) {
        years.push(String(i));
      }
      return years;
    },
    shortMonthNames() {
      let months = this.monthNames;
      let shortenedMonths = [];

      for (let i = 0; i < months.length; i++) {
        let month = months[i];
        let shortenedMonth = month.substring(0, 3).toLowerCase();
        shortenedMonths.push(shortenedMonth);
      }
      return shortenedMonths;
    },
    getElementsBetween() {
      let yearMin = this.dayMinCaption.split(" ")[0];
      let yearMax = this.dayMaxCaption.split(" ")[1];
      console.log(yearMax)
      let elements = [];
      let isBetween = false;

      for (let i = 0; i < this.insertMonthIntoArrayYear.length; i++) {
        let year = this.insertMonthIntoArrayYear[i];
        if (year === yearMin) {
          isBetween = true;
        }
        if (isBetween) {
          elements.push(year);
        }
        if (year === yearMax) {
          break;
        }
      }
      return elements;
    },

    dayMinCaption() {
      let year = this.duplicateArrayElementsForYears[this.dayBarMinValue]
      let month = this.insertEmptyArraysForMonth[this.dayBarMinValue]
      return year + " " + month
    },
    dayMaxCaption() {
      let year = this.duplicateArrayElementsForYears[this.dayBarMaxValue]
      let month = this.insertEmptyArraysForMonth[this.dayBarMaxValue]
      return month + " " + year
    },
    duplicateArrayElementsForYears() {
      const duplicatedArray = [];
      this.getAllYearNames.forEach((element) => {
        for (let i = 0; i < 13; i++) {
          duplicatedArray.push(element);
        }
      });
      return duplicatedArray;
    },
    insertEmptyArraysForMonth() {
      const newArray = [];
      for (let i = 0; i < 7; i++) {
        newArray.push([]);
        newArray.push(...this.monthNames);
      }
      newArray.push([]);
      return newArray;
    },
    insertMonthIntoArrayYear() {
      let years = this.getAllYearNames;
      let months = this.shortMonthNames;
      let newArray = [];
        for (let i = 0; i < years.length; i++) {
          newArray.push(years[i]);
          if (i < years.length - 1) {
            for (let j = 0; j < months.length; j++) {
              newArray.push(months[j]);
            }
          }
        }
      return newArray;
    },
  }
}
</script>

<style>

</style>
