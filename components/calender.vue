<template>
  <div class="calendar" :class="{ collapsed: isCollapsed }">
    <div class="header" @click="isCollapsed = !isCollapsed">
	  <!-- <div>上一周</div> -->
      <div class="prev">&lt;</div>
      <div class="title">{{ year }}年{{ month }}月</div>
      <div class="next">&gt;</div>
	  <!-- <div>下一周</div> -->
    </div>
    <div class="weekdays">
      <div class="weekday" v-for="day in weekdays">{{ day }}</div>
    </div>
    <div class="days">
      <div
        class="day"
        v-for="(day, index) in days"
        :class="{ today: isToday(day), selected: isSelected(day) }"
        @click="selectDay(day)"
        v-show="!isCollapsed || index < 7"
      >
        {{ day }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "calendar",
  props: {
    defaultDate: {
      type: Date,
      default: () => new Date(),
    },
  },
  data() {
    const today = this.defaultDate;
    return {
      year: today.getFullYear(),
      month: today.getMonth() + 1,
      selectedDay: today.getDate(),
      isCollapsed: true,
    };
  },
  computed: {
	  days() {
	    const firstDayOfMonth = new Date(this.year, this.month - 1, 1);
	    const lastDayOfMonth = new Date(this.year, this.month, 0);
	    const daysInMonth = lastDayOfMonth.getDate();
	    const days = [];
	    for (let i = 1; i <= daysInMonth; i++) {
	      days.push(i);
	    }
	    const emptyDays = (firstDayOfMonth.getDay() + 6) % 7;
	    for (let i = 0; i < emptyDays; i++) {
	      days.unshift(null);
	    }
	  if (this.isCollapsed) {
	    const today = new Date();
	    const firstDayOfWeek = new Date(this.year, this.month - 1, today.getDate() - today.getDay() + 1);
	    const lastDayOfWeek = new Date(this.year, this.month - 1, today.getDate() - today.getDay() + 7);
	    const firstDayIndex = days.indexOf(firstDayOfWeek.getDate());
	    const lastDayIndex = days.indexOf(lastDayOfWeek.getDate());
	    const lastDayOfMonth = new Date(this.year, this.month, 0).getDate();
		const nowWeek = [];
		for (let i = 0; i < days.slice(firstDayIndex).length; i++) {
			nowWeek.push(days.slice(firstDayIndex)[i])
		}
		var d = [1,2,3,4,5,6]
		for (let i = 0; i < d.length; i++) {
			nowWeek.push(d[i])
		}		
		return nowWeek.slice(0,7)
	  } else {
		  while (days.length<42){
		  	days.push(null)
		  }
	    return days;
	  }

	  },

    weekdays() {
      return ["日", "一", "二", "三", "四", "五", "六"];
    },
  },
  methods: {
    prevMonth() {
      if (this.month === 1) {
        this.year--;
        this.month = 12;
      } else {
        this.month--;
      }
    },
    nextMonth() {
      if (this.month === 12) {
        this.year++;
        this.month = 1;
      } else {
        this.month++;
      }
    },
    isToday(day) {
      const today = new Date();
      return (
        this.year === today.getFullYear() &&
        this.month === today.getMonth() + 1 &&
        day === today.getDate()
      );
    },
    isSelected(day) {
      return day === this.selectedDay;
    },
    selectDay(day) {
      if (day !== null) {
        this.selectedDay = day;
      }
    },
  },
};
</script>
<style>
	.calendar {
	  width: 80%;
	  height: 300px;
	  background-color: #f2f2f2;
	  margin-bottom: 20px;
	  display: flex;
	  flex-direction: column;
	  overflow: hidden;
	  transition: height 0.3s linear;
	}
	
	.calendar.collapsed {
	  height: 150px;
	}
	
	.header {
	  display: flex;
	  justify-content: center;
	  align-items: center;
	  height: 40px;
	  font-size: 20px;
	  font-weight: bold;
	  cursor: pointer;
	}
	
	.prev,
	.next {
	  width: 40px;
	  height: 40px;
	  line-height: 40px;
	  text-align: center;
	  cursor: pointer;
	}
	
	.weekdays {
	  display: flex;
	  height: 40px;
	  line-height: 40px;
	  text-align: center;
	}
	
	.weekday {
	  flex: 1;
	}
	
	.days {
	  display: flex;
	  flex-wrap: wrap;
	  justify-content: space-between;
	  padding: 10px;
	}
	
	.day {
	  width: calc(100% / 7);
	  height: 40px;
	  line-height: 40px;
	  text-align: center;
	  cursor: pointer;
	}
	
	.today {
	  background-color: #f60;
	  color: #fff;
	  border-radius: 20px;
	}
	
	.selected {
	  background-color: #f60;
	  color: #fff;
	  border-radius: 20px;
	}
</style>
