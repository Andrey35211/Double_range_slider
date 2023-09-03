<template>

  <div id="slider">
    <div class="bar-left">
      <p @click="showFirstComponent"
         :class="{ 'v-switch-active': firstComponentVisible, 'v-switch-active-color': !firstComponentVisible }">Все
        года</p>
      <p @click="showSecondComponent"
         :class="{ 'v-switch-active': !firstComponentVisible, 'v-switch-active-color': firstComponentVisible }">
        Месяцы</p>
    </div>

    <div class="bar-right">
      <div class="MultiRangeSliderContainer" v-if="firstComponentVisible">
        <MultiRangeSlider
            :min="0"
            :max="91"
            :minValue="yearMinValue"
            :maxValue="yearMaxValue"
            :labels="getAllYearNames"
            :min-caption="dateMinCaption"
            :max-caption="dateMaxCaption"
            :step="1"
            @input="updateDayValues"
        />
      </div>

      <div class="MultiRangeSliderContainerMonth" v-else>
        <MultiRangeSlider
            :min="0"
            :max="26"
            :labels="getElementsBetweenForMonth"
            :minValue="monthMinValue"
            :maxValue="monthMaxValue"
            :min-caption="monthMinCaption"
            :max-caption="monthMaxCaption"
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
  components: {MultiRangeSlider},

  data() {
    return {
      monthMinValue: 0,
      // 3 года + все месяцы между ними
      monthMaxValue: 26,
      yearMinValue: 0,
      // 8 годов + все месяцы между ними
      yearMaxValue: 91,
      firstComponentVisible: true,
      StartYear: 2014,
      EndYear: 2021,
    };
  },
  methods: {
    updateWeekValues(e) {
      this.monthMinValue = e.minValue;
      this.monthMaxValue = e.maxValue;
    },
    updateDayValues(e) {
      this.yearMinValue = e.minValue;
      this.yearMaxValue = e.maxValue;
    },
    showFirstComponent: function () {
      this.firstComponentVisible = true;
    },
    showSecondComponent: function () {
      this.firstComponentVisible = false;
    },
  },
  computed: {
    monthNames() {
      return ["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"];
    },
    getAllYearNames() {
      const years = [];
      for (let i = this.StartYear; i <= this.EndYear; i++) {
        years.push(String(i));
      }
      return years;
    },
    shortMonthNames() {
      const months = this.monthNames;
      const shortenedMonths = [];

      for (let i = 0; i < months.length; i++) {
        const month = months[i];
        const shortenedMonth = month.substring(0, 3).toLowerCase();
        shortenedMonths.push(shortenedMonth);
        shortenedMonths.join(' ').split(' ')
      }
      return shortenedMonths;
    },
    getElementsBetweenForMonth() {

      const yearMin = this.dateMinCaption.split(" ")[1];
      const yearMax = this.dateMaxCaption.split(" ")[1];
      const elements = [];
      const years = []
      let isBetween = false;
      const months = this.shortMonthNames;
      for (let i = yearMin; i <= yearMax; i++) {
        years.push(String(i));
      }
      // если в массиве пришло более 3 лет, то вставить все года и слово "месяцы" между ними
      if (years.length > 3) {
        for (let i = 0; i < years.length; i++) {
          const year = years[i];
          elements.push(year);
          if (!(year === yearMax)) {
            elements.push("месяцы")
          }
        }
      }
      // вставить месяцы между годами, если в массиве пришло 3 года или менее 3 лет
      if (years.length <= 3) {
        for (let i = 0; i < years.length; i++) {
          const year = years[i];

          if (year === yearMin) {
            isBetween = true;
          }
          if (isBetween) {

            elements.push(year);

            if (!(year === yearMax)) {
              for (let j = 0; j < months.length; j++) {
                elements.push(months[j]);
              }
            }
          }
          if (year === yearMax) {
            break
          }
        }
      }

      if (yearMin === yearMax) {

        for (let j = 0; j < months.length; j++) {
          elements.push(months[j]);
        }

        elements.push(yearMin);
      }
      return elements;
    },

    dateMinCaption() {

      const year = this.duplicateArrayElementsForYears[this.yearMinValue]
      const month = this.insertEmptyArraysForMonth[this.yearMinValue]
      return month + " " + year
    },
    dateMaxCaption() {
      const year = this.duplicateArrayElementsForYears[this.yearMaxValue]
      const month = this.insertEmptyArraysForMonth[this.yearMaxValue]
      return month + " " + year
    },
    monthMinCaption() {
      const year = this.duplicateYearForMonthCaption[this.monthMinValue]
      const month = this.insertEmptyArraysForMonth[this.monthMinValue]
      return month + " " + year
    },
    monthMaxCaption() {
      const year = this.duplicateYearForMonthCaption[this.monthMaxValue]
      const month = this.insertEmptyArraysForMonth[this.monthMaxValue]
      return month + " " + year
    },
    duplicateArrayElementsForYears() {
      const duplicatedArray = [];
      const months = this.shortMonthNames;
      this.getAllYearNames.forEach((element) => {
        for (let i = 0; i < months.length + 1; i++) {
          duplicatedArray.push(element);
        }
      });

      return duplicatedArray;
    },
    insertEmptyArraysForMonth() {
      const years = this.getAllYearNames;
      const newArray = [];
      for (let i = 0; i < years.length - 1; i++) {
        newArray.push([]);
        newArray.push(...this.monthNames);
      }
      newArray.push([]);
      return newArray;
    },
    duplicateYearForMonthCaption() {
      const yearMin = this.dateMinCaption.split(" ")[1];
      const yearMax = this.dateMaxCaption.split(" ")[1];
      const duplicatedArray = [];
      const years = []
      for (let i = yearMin; i <= yearMax; i++) {
        years.push(String(i));
      }
      const months = this.shortMonthNames;
      years.forEach((element) => {
        for (let i = 0; i < months.length ; i++) {
          duplicatedArray.push(element);
        }
      });
      console.log(duplicatedArray)
      return duplicatedArray;
    },
  }
}
</script>

<style>

</style>
