<template>
  <div class="datepicker">
    {{ startDate }} - {{ endDate }}
    <input
      type="text"
      v-if="type === 'single'"
      v-model="selectedDate"
      @click="toggleDatepicker"
      placeholder="Select a date"
    />
    <div v-else>
      <input
        type="text"
        v-model="startDate"
        @click="toggleStartDatepicker"
        placeholder="Start Date"
      />
      hasta
      <input type="text" v-model="endDate" @click="toggleEndDatepicker" placeholder="End Date" />
    </div>

    <div v-if="isDatepickerVisible" class="datepicker-container">
      <div class="preset-buttons">
          <button @click="setLastNDays(7)">Últimos 7 días</button>
          <button @click="setLastNDays(30)">Últimos 30 días</button>
          <button @click="setLastNDays(60)">Últimos 60 días</button>
          <button @click="setLastNDays(90)">Últimos 90 días</button>
        </div>
      <div class="calendar">
        <div class="calendar-header">
          <button @click="goToPreviousMonth">&lt;</button>
          <div @click="toggleMonthSelector" class="current-month">{{ currentMonth }}</div>
          <div @click="toggleYearSelector" class="current-year">{{ currentYear }}</div>
          <button @click="goToNextMonth">&gt;</button>
        </div>
        <div class="calendar-days">
          <div v-for="day in daysOfWeek" :key="day" class="day">{{ day }}</div>
          <div v-for="blank in firstDayOffset" :key="blank" class="day blank-day"></div>
          <div
            v-for="date in daysInMonth"
            :key="date"
            class="day"
            :class="{
              selected: isDateSelected(getFormattedDate(date)) && type === 'single',
              'start-date': isStartDate(getFormattedDate(date)) && type === 'range',
              'end-date':
                isEndDate(getFormattedDate(date)) &&
                type === 'range' &&
                !isStartDate(getFormattedDate(date)), // No aplicar si también es start-date
              'in-range':
                isDateInRange(getFormattedDate(date)) &&
                type === 'range' &&
                !isStartDate(getFormattedDate(date)) &&
                !isEndDate(getFormattedDate(date)), // No aplicar si también es start-date o end-date
              disabled: isDateDisabled(getFormattedDate(date))
            }"
            @click="selectDate(date)"
          >
            {{ date }}
          </div>
        </div>
        <div class="button-container">
          <button class="accept-button" @click="acceptDate">Aceptar</button>
          <button class="cancel-button" @click="cancelDate">Cancelar</button>
        </div>
      </div>
    </div>
    <div v-if="isMonthSelectorVisible" class="month-selector">
      <div class="month-list">
        <div v-for="(month, index) in months" :key="month" @click="selectMonth(index)">
          {{ month.substr(0, 3) }}
          <!-- Mostrar solo las primeras 3 letras del mes -->
        </div>
        <button class="close-button" @click="closeMonthSelector">X</button>
      </div>
    </div>
    <div v-if="isYearSelectorVisible" class="year-selector">
      <div class="year-list">
        <div v-for="year in displayedYears" :key="year" @click="selectYear(year)">
          {{ year }}
        </div>
      </div>
      <button @click="previousYears">&lt;</button>
      <button @click="nextYears">&gt;</button>
      <button class="close-button" @click="closeYearSelector">X</button>
    </div>
  </div>
</template>

<script>
import moment from 'moment'

export default {
  props: {
    type: {
      type: String,
      default: 'single'
    },
    allowBefore: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      selectedDate: null,
      startDate: null,
      endDate: null,
      isDatepickerVisible: false,
      currentDate: moment(),
      isMonthSelectorVisible: false,
      months: moment.months(),
      isYearSelectorVisible: false,
      displayedYears: [],
      currentYearIndex: 0
    }
  },
  computed: {
    currentMonth() {
      return this.currentDate.format('MMMM')
    },
    currentYear() {
      return this.currentDate.format('YYYY')
    },
    currentMonthNumber() {
      return this.currentDate.format('M')
    },
    daysOfWeek() {
      return ['Dom', 'Lun', 'Mar', 'Mie', 'Jue', 'Vie', 'Sab']
    },
    firstDayOffset() {
      const firstDayOfMonth = this.currentDate.clone().date(1).day()
      return Array(firstDayOfMonth).fill(null)
    },
    daysInMonth() {
      const totalDaysInMonth = this.currentDate.clone().endOf('month').date()
      return Array.from({ length: totalDaysInMonth }, (_, i) => i + 1)
    }
  },
  methods: {
    getFormattedDate(day) {
      const formattedDate = moment(
        `${day}/${this.currentMonthNumber}/${this.currentYear}`,
        'D/M/YYYY'
      )
      return formattedDate.format('DD/MM/YYYY')
    },
    toggleDatepicker() {
      this.isDatepickerVisible = !this.isDatepickerVisible
    },
    toggleStartDatepicker() {
      this.isDatepickerVisible = !this.isDatepickerVisible
      if (this.startDate) {
        this.startDate = moment(this.startDate, 'DD/MM/YYYY').format('DD/MM/YYYY')
      }
    },

    toggleEndDatepicker() {
      this.isDatepickerVisible = !this.isDatepickerVisible
      if (this.endDate) {
        this.endDate = moment(this.endDate, 'DD/MM/YYYY').format('DD/MM/YYYY')
      }
    },
    goToPreviousMonth() {
      this.currentDate = this.currentDate.clone().subtract(1, 'month')
    },
    goToNextMonth() {
      this.currentDate = this.currentDate.clone().add(1, 'month')
    },
    isDateDisabled(date) {
      if (this.type === 'range') {
        if (this.allowBefore) {
          return false
        } else {
          const selectedDate = this.currentDate.clone().date(date)
          return selectedDate.isBefore(moment(), 'day')
        }
      } else {
        return this.allowBefore ? false : date < moment().date()
      }
    },
    // selectDate(date) {
    //   const formattedDate = moment(
    //     `${date}/${this.currentMonthNumber}/${this.currentYear}`,
    //     'DD/MM/YYYY'
    //   )
    //   formattedDate.format('DD/MM/YYYY')

    //   if (this.type === 'single') {
    //     this.selectedDate = formattedDate
    //     this.isDatepickerVisible = false
    //   } else if (this.type === 'range') {
    //     if (!this.startDate) {
    //       this.startDate = formattedDate
    //     } else if (this.startDate && !this.endDate) {
    //       const isAfter = moment(formattedDate, 'DD/MM/YYYY').isSameOrAfter(this.startDate, 'day')
    //       if (isAfter) {
    //         this.endDate = formattedDate
    //         this.isDatepickerVisible = false
    //       } else {
    //         this.endDate = this.startDate
    //         this.startDate = formattedDate
    //       }
    //     } else {
    //       this.startDate = formattedDate
    //       this.endDate = null
    //     }
    //   }
    // },
    selectDate(date) {
      const formattedDate = moment(
        `${date}/${this.currentMonthNumber}/${this.currentYear}`,
        'DD/MM/YYYY'
      )

      if (this.type === 'single') {
        this.selectedDate = formattedDate.format('DD/MM/YYYY')
        this.isDatepickerVisible = false
      } else if (this.type === 'range') {
        if (!this.startDate) {
          this.startDate = formattedDate.format('DD/MM/YYYY')
        } else if (this.startDate && !this.endDate) {
          const isAfter = moment(formattedDate, 'DD/MM/YYYY').isSameOrAfter(
            moment(this.startDate, 'DD/MM/YYYY'),
            'day'
          )
          if (isAfter) {
            this.endDate = formattedDate.format('DD/MM/YYYY')
            this.isDatepickerVisible = false
          } else {
            this.endDate = this.startDate
            this.startDate = formattedDate.format('DD/MM/YYYY')
          }
        } else {
          this.startDate = formattedDate.format('DD/MM/YYYY')
          this.endDate = null
        }
      }
    },
    isDateSelected(date) {
      return this.type === 'single' && date === this.selectedDate
    },
    isStartDate(date) {
      return this.type === 'range' && date === this.startDate
    },
    isEndDate(date) {
      return this.type === 'range' && date === this.endDate
    },
    isDateInRange(date) {
      if (this.type === 'range' && this.startDate !== null && this.endDate !== null) {
        const formattedStartDate = moment(this.startDate, 'DD/MM/YYYY')
        const formattedEndDate = moment(this.endDate, 'DD/MM/YYYY')
        const formattedDate = moment(date, 'DD/MM/YYYY')

        return formattedDate.isBetween(formattedStartDate, formattedEndDate, null, '[]')
      }
      return false
    },
    toggleMonthSelector() {
      this.isMonthSelectorVisible = !this.isMonthSelectorVisible
    },
    selectMonth(index) {
      this.currentDate = this.currentDate.clone().month(index).date(1)
      this.isMonthSelectorVisible = false
    },
    toggleYearSelector() {
      this.isYearSelectorVisible = !this.isYearSelectorVisible
      if (this.isYearSelectorVisible) {
        this.generateYearList()
      }
    },
    generateYearList() {
      const currentYear = this.currentDate.year()
      this.displayedYears = Array.from({ length: 9 }, (_, index) => currentYear - 4 + index)
      this.currentYearIndex = 4
    },
    selectYear(year) {
      this.currentDate = this.currentDate.clone().year(year).date(1)
      this.isYearSelectorVisible = false
    },
    previousYears() {
      const firstYear = this.displayedYears[0] - 9
      const newYears = Array.from({ length: 9 }, (_, index) => firstYear + index)
      this.displayedYears = newYears
    },
    nextYears() {
      const lastYear = this.displayedYears[8] + 1
      const newYears = Array.from({ length: 9 }, (_, index) => lastYear + index)
      this.displayedYears = newYears
    },
    acceptDate() {
      // Realiza alguna acción al aceptar la fecha
      // Por ejemplo, puedes emitir un evento para informar a un componente padre sobre la fecha seleccionada
      this.$emit('date-selected', this.selectedDate)

      // Cierra el modal
      this.isDatepickerVisible = false
    },

    cancelDate() {
      // Cierra el modal sin realizar ninguna acción adicional
      this.startDate = null
      this.endDate = null
      this.isDatepickerVisible = false
    },

    closeDatepicker() {
      // Cierra el modal de fecha
      this.isDatepickerVisible = false
    },

    closeMonthSelector() {
      // Cierra el modal del selector de meses
      this.isMonthSelectorVisible = false
    },

    closeYearSelector() {
      // Cierra el modal del selector de años
      this.isYearSelectorVisible = false
    },
    setLastNDays(numDays) {
      const endDate = moment().format('DD/MM/YYYY') // Fecha de hoy
      const startDate = moment()
        .subtract(numDays - 1, 'days')
        .format('DD/MM/YYYY')

      this.startDate = startDate
      this.endDate = endDate
    }
  }
}
</script>
<style scoped>
.datepicker {
  position: relative;
}
.datepicker-container {
  position: absolute;
  background-color: white;
  border: 1px solid #ccc;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
  padding: 10px;
  z-index: 1000;
}
.calendar {
  display: flex;
  flex-direction: column;
}
.calendar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.current-month {
  font-weight: bold;
}
.calendar-days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}
.day {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  cursor: pointer;
  position: relative;
}
.day.blank-day {
  visibility: hidden;
}
.day.disabled {
  color: #ccc;
  cursor: not-allowed;
}
.day.selected {
  background-color: #a0d3e8; /* Estilo para la fecha seleccionada en modo single */
}
.day.start-date::after,
.day.end-date::after,
.day.in-range::after {
  content: '';
  display: block;
  width: 30px; /* Ancho del círculo */
  height: 30px; /* Altura del círculo */
  background-color: #69a0a6; /* Color del círculo */
  border-radius: 50%; /* Para hacerlo circular */
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: -1; /* Asegura que esté detrás del contenido */
}

.day.start-date::after {
  background-color: #69a0a6; /* Fondo para la fecha de inicio en modo rango */
}

.day.end-date::after {
  background-color: #2e4a4e; /* Fondo para la fecha de fin en modo rango */
}

.day.in-range::after {
  background-color: #f0f0f0; /* Fondo para las fechas dentro del rango en modo rango */
}

.month-selector {
  position: absolute;
  background-color: white;
  border: 1px solid #ccc;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
  padding: 10px;
  z-index: 1001;
}
.month-list {
  display: flex;
  flex-wrap: wrap;
  height: 150px;
  width: 150px;
  max-height: 200px;
  overflow-y: auto;
}
.month-list div {
  display: flex;
  justify-content: center; /* Centrar horizontalmente */
  align-items: center; /* Centrar verticalmente */
  padding: 5px;
  cursor: pointer;
  flex: 1 0 33.33%;
  text-align: center;
  font-size: 10px; /* Tamaño de fuente 10 */
  border: 1px solid #ccc; /* Bordes delgados */
  background-color: #f9f9f9; /* Fondo tenue */
}
.month-list div:hover {
  background-color: #f0f0f0;
}
.month-list {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* Mostrar en 3 columnas */
  grid-template-rows: repeat(4, 1fr); /* Mostrar en 4 filas */
  max-height: 200px;
  overflow-y: auto;
}

.month-list div {
  padding: 5px;
  cursor: pointer;
  flex: 1 0 33.33%;
  text-align: center;
  font-size: 10px; /* Tamaño de fuente 10 */
}
/***YEAR */
.year-selector {
  position: absolute;
  background-color: white;
  border: 1px solid #ccc;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
  padding: 10px;
  z-index: 1001;
}
.year-list {
  display: flex;
  flex-wrap: wrap;
  height: 150px;
  width: 150px;
  max-height: 200px;
  overflow-y: auto;
}
.year-list div {
  display: flex;
  justify-content: center; /* Centrar horizontalmente */
  align-items: center; /* Centrar verticalmente */
  padding: 5px;
  cursor: pointer;
  flex: 1 0 33.33%;
  text-align: center;
  font-size: 10px; /* Tamaño de fuente 10 */
  border: 1px solid #ccc; /* Bordes delgados */
  background-color: #f9f9f9; /* Fondo tenue */
}
.year-list div:hover {
  background-color: #f0f0f0;
}
.year-list {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* Mostrar en 3 columnas */
  grid-template-rows: repeat(4, 1fr); /* Mostrar en 4 filas */
  max-height: 200px;
  overflow-y: auto;
}

.year-list div {
  padding: 5px;
  cursor: pointer;
  flex: 1 0 33.33%;
  text-align: center;
  font-size: 10px; /* Tamaño de fuente 10 */
}
.preset-buttons {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-right: 20px; /* Espacio entre los botones y el calendario */
}
.preset-buttons button {
  margin-bottom: 5px;
}
</style>
