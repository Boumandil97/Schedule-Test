<template>
  <div class="w-full h-full bg-gray-200 flex items-center justify-center">
    <div class="flex flex-col">
      <div class="flex">
        <div
          class="bg-white w-24 text-center py-8 border-l border-t border-black"
        >
          Person
        </div>
        <div
          class="bg-white w-24 text-center py-8 border-l border-t border-black"
          v-for="(cell, index) in cells"
          :key="index"
          :class="{ 'border-r': index === 6 }"
        >
          {{ cell.name }}
        </div>
      </div>
      <div v-for="user in users" :key="user.name" class="flex">
        <div
          class="bg-white w-24 text-center py-8 border-l border-t border-black"
        >
          {{ user.name }}
        </div>

        <div
          class="relative w-24 text-center py-8 border-l border-t border-black flex items-center justify-center select-none"
          v-for="(cell, index) in user.cells"
          :key="index"
          :class="{
            'border-r': index === 6,
            'bg-transparent': cell.name === 'Sat' || cell.name === 'Sun',
            'bg-white': cell.name !== 'Sat' && cell.name !== 'Sun',
          }"
          @mousedown="handleMouseDown(cell.name, user.name, user.occupiedCells)"
          @mousemove="handleMouseEnter(cell.name, user.occupiedCells)"
          @mouseup="handleMouseUp(user.name, user.occupiedCells)"
        >
          <div
            v-if="
              user.occupiedCells.includes(cell.name) &&
              user.occupiedCells[0] === cell.name
            "
            :class="`bg-blue-500 text-white capitalize font-bold h-[80%] absolute inset-0 z-40 my-auto rounded-xl flex items-center justify-center`"
            :style="`width: ${user.occupiedCells.length * 6}rem`"
          >
            {{ user.taskName }}
          </div>
        </div>
      </div>
      <PopUp
        :showingPopup="showingPopup"
        :user="selectedData.user"
        :date="selectedData.date"
        :exit="exit"
      />
    </div>
  </div>
</template>

<script>
import PopUp from "./PopUp.vue";

export default {
  name: "ScheduleTable",
  components: {
    PopUp,
  },
  data() {
    return {
      cells: [],
      dragging: false,
      showingPopup: false,
      cellsHolder: [],
      selectedData: {},

      users: [
        {
          name: "Anton",
          taskName: "task1",
          occupiedCells: [],
        },
        {
          name: "Oleg",
          taskName: "task2",
          occupiedCells: [],
        },
        {
          name: "Ivan",
          taskName: "task3",
          occupiedCells: [],
        },
        {
          name: "Petr",
          taskName: "task3",
          occupiedCells: [],
        },
      ],
    };
  },
  mounted() {
    console.log(this.users);
  },
  created() {
    const currentDate = new Date();
    for (let i = 0; i < 7; i++) {
      const date = new Date(currentDate);
      date.setDate(currentDate.getDate() + i);

      this.cells = [
        { name: "Mon" },
        { name: "Tue" },
        { name: "Wed" },
        { name: "Thu" },
        { name: "Fri" },
        { name: "Sat" },
        { name: "Sun" },
      ];
    }

    this.users.map((user) => {
      user.cells = this.cells;
    });
  },
  methods: {
    handleMouseDown(cell, name, occupiedCells) {
      if (
        occupiedCells.filter(
          (item) => item === cell && cell !== "Sat" && cell !== "Sun"
        ).length > 0
      ) {
        occupiedCells = occupiedCells.filter((item) => item !== cell);
      } else {
        cell !== "Sat" && cell !== "Sun" && occupiedCells.push(cell);
      }
      this.dragging = true;
    },
    handleMouseEnter(event, occupiedCells) {
      if (this.dragging && event !== "Sat" && event !== "Sun") {
        if (occupiedCells.filter((item) => item === event).length > 0) {
          occupiedCells = occupiedCells.filter((item) => item !== event);
        } else {
          occupiedCells.push(event);
        }
        console.log(occupiedCells);
      } else return;
    },
    handleMouseUp(userName, schedule) {
      this.showingPopup = true;
      this.selectedData = {
        ...this.selectedData,

        user: userName,
        date: `${schedule[0]} to ${schedule[schedule.length - 1]}`,
      };
      this.dragging = false;
    },
    handleTaskNameChange(event) {
      this.selectedData = {
        ...this.selectedData,
        taskName: event.target.value,
      };
    },
    exit() {
      this.showingPopup = false;
    },
  },
};
</script>
