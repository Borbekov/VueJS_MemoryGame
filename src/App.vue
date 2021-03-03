<template>
  <div class="wrapper">
    <transition-group v-if="!isWin" name="card" tag="div" class="table">
      <div v-for="card in cards" :key="card.id" class="card">
        <div v-if="card.show && !card.founded || defaultOpen" v-html="card.icon" class="icon" :class="{'invisible': card.founded}"></div>
        <div v-if="!card.show && !card.founded && !defaultOpen" class="icon closed" @click="openCard(card)"></div>
      </div>
    </transition-group>
    <div v-else class="table">
      <div class="win">
        <p>YOU WIN!</p>
        <p>SCORE IS {{formattedElapsedTime}}</p>
      </div>
    </div>
    <div v-if="!isGame">
      <button v-if="!isWin" @click="play">
        PLAY
    </button>
    <button v-else @click="replay">
        PLAY AGAIN
    </button>
    </div>
    <div v-else class="timer">{{formattedElapsedTime}}</div>
  </div>
</template>

<script>
import _shuffle from "lodash.shuffle";
export default {
  data: function () {
    return {
      defaultOpen: true,
      cards: [
        {
          id: 1,
          icon: '<i class="ri-home-4-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 2,
          icon: '<i class="ri-home-4-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 3,
          icon: '<i class="ri-mail-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 4,
          icon: '<i class="ri-mail-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 5,
          icon: '<i class="ri-chat-1-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 6,
          icon: '<i class="ri-chat-1-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 7,
          icon: '<i class="ri-bug-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 8,
          icon: '<i class="ri-bug-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 9,
          icon: '<i class="ri-tv-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 10,
          icon: '<i class="ri-tv-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 11,
          icon: '<i class="ri-gamepad-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 12,
          icon: '<i class="ri-gamepad-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 13,
          icon: '<i class="ri-bluetooth-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 14,
          icon: '<i class="ri-bluetooth-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 15,
          icon: '<i class="ri-battery-2-charge-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 16,
          icon: '<i class="ri-battery-2-charge-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 17,
          icon: '<i class="ri-heart-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 18,
          icon: '<i class="ri-heart-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 19,
          icon: '<i class="ri-gitlab-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 20,
          icon: '<i class="ri-gitlab-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 21,
          icon: '<i class="ri-vuejs-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 22,
          icon: '<i class="ri-vuejs-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 23,
          icon: '<i class="ri-twitter-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 24,
          icon: '<i class="ri-twitter-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 25,
          icon: '<i class="ri-compass-3-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 26,
          icon: '<i class="ri-compass-3-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 27,
          icon: '<i class="ri-map-pin-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 28,
          icon: '<i class="ri-map-pin-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 29,
          icon: '<i class="ri-rocket-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 30,
          icon: '<i class="ri-rocket-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 31,
          icon: '<i class="ri-police-car-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 32,
          icon: '<i class="ri-police-car-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 33,
          icon: '<i class="ri-truck-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 34,
          icon: '<i class="ri-truck-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 35,
          icon: '<i class="ri-alert-line"></i>',
          show: false,
          founded: false
        },
        {
          id: 36,
          icon: '<i class="ri-alert-line"></i>',
          show: false,
          founded: false
        },
      ],
      selectedCards: [],
      count: 0,
      fiveSec: null,
      elapsedTime: 0,
      timer: 0,
      isGame: false,
      isWin: false
    }
  },
  computed: {
    formattedElapsedTime() {
      const date = new Date(null);
      date.setSeconds(this.elapsedTime / 1000);
      const utc = date.toUTCString();
      return utc.substr(utc.indexOf(":") - 2, 8);
    }
  },
  methods: {
    openCard(selectedCard) {
      if (this.selectedCards.length < 2 && this.isGame) {
        this.cards.forEach(card => {
          card.id === selectedCard.id && (
            card.show = true,
            this.selectedCards.push(card.icon)
          )
        })

        //wait 5 sec if there is one card
        this.selectedCards.length === 1 && (
          this.fiveSec = setTimeout(() => (
            this.waitToFiveSec()
          ), 5000)
        )

        //check selected cards and clear timer for 5 sec
        this.selectedCards.length === 2 && (
          this.checkSelectedCards(),
          clearTimeout(this.fiveSec)
        )
      }
    },
    //function to close selected one card
    waitToFiveSec() {
      let checkedIcon = this.selectedCards[0]
      this.cards.forEach(card => {
        card.icon === checkedIcon && (
          card.show = false,
          this.selectedCards.length = 0
        )
      })
    },
    //actions for selected cards
    checkSelectedCards() {
      let checkedIcon = null
      if (this.selectedCards[0] === this.selectedCards[1]) {
        checkedIcon = this.selectedCards[0]
      }
      this.cards.forEach(card => {
        card.icon === checkedIcon ? (
          setTimeout(() => {
            card.founded = true
            this.count += 1 
            this.selectedCards.length = 0
          }, 1000)
        ) : (
          setTimeout(() => {
            card.show = false
            this.selectedCards.length = 0
          }, 1000)
        )
      })
    },
    start() {
      this.timer = setInterval(() => {
        this.elapsedTime += 1000;
      }, 1000);
      this.isGame = true
    },
    stop() {
      clearInterval(this.timer)
      this.isGame = false
      this.isWin = true
    },
    reset() {
      this.elapsedTime = 0;
    },
    play() {
      this.defaultOpen = false
      this.cards = _shuffle(this.cards)
      this.start()
    },
    replay() {
      this.cards.forEach(card => (
        card.show = false,
        card.founded = false
      ))
      this.count = 0
      this.isWin = false
      this.reset()
    }
  },
  watch: {
    count() {
      if (this.count === 36) {
        this.stop()
      }
    }
  }
}
</script>

<style lang="scss">
body {
  width: 100%;
  height: 100%;
  background: #F3F3F3;
}
.wrapper {
  display: flex;
  flex-flow: column;
  align-items: center;
  .table {
    display: flex;
    flex-wrap: wrap;
    width: 720px;
    margin-top: 10px;
    .card {
      width: 100px;
      height: 100px;
      margin: 10px;
      .icon {
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 40px;
        border-radius: 8px;
        border: 1px solid rgba(0, 0, 0, 0.32);
        background: #FFFFFF;
        cursor: pointer;
        &.closed {
          background: #e1b382;
        }
        &.invisible {
          opacity: 0;
          cursor: default;
        }
      }
    }
    .card-move {
      transition: transform 1.5s;
    }
    .win {
      height: 720px;
      width: 100%;
      background: linear-gradient(#A1FFCE, #FAFFD1);
      font-size: 50px;
      font-weight: 600;
      display: flex;
      flex-flow: column;
      align-items: center;
      justify-content: center;
    }
  }
  button {
    margin-top: 30px;
    padding: 10px 20px;
    background: #FFF;
    cursor: pointer;
  }
  .timer {
    margin-top: 30px;
    font-size: 20px;
    cursor: default;
  }
}
</style>