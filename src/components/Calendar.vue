<template>
  <div id="calendar">
    <vue-cal selected-date="2020-08-30"
            :time-from="8 * 60"
            :time-step="30"
            :disable-views="['years', 'year', 'month', 'week']"
            :activeView="'day'"
            :on-event-click="onEventClick"
            :hideViewSelector="true"
            :dragToCreateEvent="dragToCreateEvent"
            :events="events"
            :split-days="splitDays"
            :sticky-split-labels="stickySplitLabels"
            :min-cell-width="minCellWidth"
            :min-split-width="minSplitWidth">
    </vue-cal>
    <div :class="{ active: isPopOpen === true }" id="popup">
      <div @click="closePop" class="pop_bg"></div>
      <div class="content">
        <div>{{ popupText }}</div>
        <div class="btns">
          <div @click="orderCase('y')">Y</div>
          <div @click="orderCase('n')">N</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import VueCal from 'vue-cal'
// import 'vue-cal/dist/drag-and-drop.js'
import 'vue-cal/dist/vuecal.css'

export default {
  name: "Calendar",
  components: {
    VueCal
  },
  props: {
    msg: String,
  },
  data: function() {
    return {
      stickySplitLabels: false,
      minCellWidth: 400,
      minSplitWidth: 0,
      isPopOpen: false,
      dragToCreateEvent: false,
      popupText: '是否確定預約',
      splitDays: [
        { id: 1, class: 'user1', label: 'Andy' },
        { id: 2, class: 'user2', label: '許湘雅', hide: false },
      ],
      events: [
        {
          start: '2020-08-30 11:00',
          end: '2020-08-30 12:00',
          title: 'Andy',
          content: '<i class="v-icon material-icons">花式按摩</i>',
          class: 'available',
          split: 1
        },
        {
          start: '2020-08-30 13:00',
          end: '2020-08-30 15:00',
          title: 'Andy',
          content: '<i class="v-icon material-icons">花式美體</i>',
          class: 'available',
          split: 1
        },
        {
          start: '2020-08-30 18:30',
          end: '2020-08-30 20:30',
          title: 'Crossfit',
          content: '<i class="v-icon material-icons">fitness_center</i>',
          class: 'unavailable',
          split: 2
        },
      ],
    }
  },
  mounted: function getOrderTime() {
    let events = this.events
    axios
      .get("https://l-beta.styleup.biz/fakeapi.php")
      .then(function (response) {
        console.log("succes");
        console.log(events)
        response.data.forEach((order)=>{
          events.push({
            start: `${order.date} ${order.startTime}`,
            end: `${order.date} ${order.endTime}`,
            title: '不接單時段',
            class: 'unavailable',
            split: 2
          })
        })
      })
      .catch(function (error) {
        console.log("Errr");
        console.log(error);
      });
  },
  computed: {
  },
  methods: {
    onEventClick: function(event,e) {
      this.selectedEvent = event
      this.isPopOpen = !this.isPopOpen
      e.preventDefault()
      console.log(this.selectedEvent.class)
    },
    closePop: function() {
      this.isPopOpen = !this.isPopOpen
    },
    orderCase: function(type) {
      type === 'y' ?this.selectedEvent.class = 'ordered' :console.log('no')
      this.isPopOpen = !this.isPopOpen
    },
  },
};
</script>

<style lang="scss">
  #calendar {
    max-width: 500px;
    margin: 0 auto;
  }
  #popup {
    visibility: hidden;
    opacity: 0;
    position: fixed;
    top: 50%;
    left: 50%;
    display: flex;
    z-index: 2;
    &.active {
      visibility: visible;
      opacity: 1;
      .pop_bg {
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        background-color: rgba($color: #000000, $alpha: 0.7);
      }
    }
    .content {
      display: flex;
      flex-direction: column;
      transform: translateX(-50%);
      border-radius: 5px;
      background-color: #FFFFFF;
      z-index: 1;
      padding: 10px 20px;

      .btns {
        display: flex;
        justify-content: space-around;
      }
    }
  }
  .vuecal__cell-split {
    .split-label {
      color: #FFFFFF;
      font-size: 26px;
      line-height: 40px;
    }
    &.user1 .split-label {
      background-color: rgba(253, 136, 136, 0.9);
    }
    &.user2 .split-label {
      background-color: rgba(240, 221, 85, 0.9);
    }
  }

  .vuecal__event {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    border-radius: 5px;
    &.available {
      background-color: rgba(253, 136, 136, 0.9);
    }
    &.unavailable {
      background-color: #F2F2F4;
      pointer-events: none;
    }
    &.ordered {
      background-color: #9B98F6;
      pointer-events: none;
    }
    .vuecal__event-time { display: none; }
    // border: 1px solid rgb(144, 210, 190);
  }
</style>