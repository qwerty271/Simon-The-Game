<template>
  <div class="main-content" id="main-content">
    <h1 class="main-content__title">Simon The Game</h1>
    <div class="simon">
      <ul class="simon__colors">
        <li
          class="simon__color simon_blue"
          v-on:click="
            {
              clickPlayer(1);
            }
          "
          :class="{ simon_blueActive: blue }"
        ></li>
        <li
          class="simon__color simon_red"
          v-on:click="
            {
              clickPlayer(2);
            }
          "
          :class="{ simon_redActive: red }"
        ></li>
        <li
          class="simon__color simon_yellow"
          v-on:click="
            {
              clickPlayer(3);
            }
          "
          :class="{ simon_yellowActive: yellow }"
        ></li>
        <li
          class="simon__color simon_green"
          v-on:click="
            {
              clickPlayer(4);
            }
          "
          :class="{ simon_greenActive: green }"
        ></li>
      </ul>
    </div>
    <div class="game-info">
      <p class="game-info__round">Раунд: {{ round }}</p>
      <button
        class="game-info__button"
        type="button"
        v-on:click="
          {
            startGame();
          }
        "
      >
        Старт
      </button>
      <p v-bind:class="{ message_hidden: message }">
        Вы проиграли, попробуйте еще раз
      </p>
      <ul class="game-info__levels">
        <li class="game-info__level">
          <p class="game-info__title">Уровень сложности:</p>
        </li>
        <li class="game-info__level">
          <input
            class="game-info__level_input"
            type="radio"
            v-model="radio"
            v-bind:value="1500"
            id="1"
          /><label class="game-info__level_label" for="1">Легкий</label>
        </li>
        <li class="game-info__level">
          <input
            class="game-info__level_input"
            type="radio"
            value="1000"
            v-model="radio"
            id="2"
          /><label class="game-info__level_label" for="2">Средний</label>
        </li>
        <li class="game-info__level">
          <input
            class="game-info__level_input"
            type="radio"
            value="400"
            v-model="radio"
            id="3"
          /><label class="game-info__level_label" for="3">Сложный</label>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "main-content",
  data() {
    return {
      round: 0,
      roundCount: 0,
      score: [],
      message: true,
      clicks: 0,
      radio: 1500,
      blue: 0,
      red: 0,
      yellow: 0,
      green: 0,
    };
  },
  methods: {
    startGame() {
      this.message = true;
      this.round += 1;
      this.play();
    },
    play() {
      if (this.round === this.roundCount) {
        this.roundCount = 0;
      } else {
        if (this.score.length < this.round) {
          this.score.push(this.getRandomInt(1, 5));
        }
        this.sequenceGneration(this.score[this.roundCount]);
        this.roundCount += 1;
        setTimeout(() => this.play(), this.radio);
      }
    },
    getRandomInt(min, max) {
      min = Math.ceil(min);
      max = Math.floor(max);
      return Math.floor(Math.random() * (max - min)) + min;
    },
    sequenceGneration(number) {
      let audio_1 = new Audio("http://davidwills.us/cmis102a/Raptor2/beep.wav");
      let audio_2 = new Audio("http://mrclan.com/fastdl/tfc/sound/beep.wav");
      let audio_3 = new Audio(
        "http://orteil.dashnet.org/cookieclicker/snd/buy4.mp3"
      );
      let audio_4 = new Audio(
        "http://www.soundescapestudios.com/SESAudio/SES%20Site%20Sounds/BoinksBoings/Boink-single-high-01.wav"
      );

      switch (number) {
        case 1:
          audio_1.play();
          this.blue += 1;
          setTimeout(() => (this.blue -= 1), 300);
          break;
        case 2:
          audio_2.play();
          this.red += 1;
          setTimeout(() => (this.red -= 1), 300);
          break;
        case 3:
          audio_3.play();
          this.yellow += 1;
          setTimeout(() => (this.yellow -= 1), 300);
          break;
        case 4:
          audio_4.play();
          this.green += 1;
          setTimeout(() => (this.green -= 1), 300);
          break;
        default:
          break;
      }
    },
    clickPlayer(number) {
      this.clicks += 1;
      if (
        this.score[this.clicks - 1] === number &&
        this.score.length === this.clicks &&
        this.score.length === this.round
      ) {
        this.sequenceGneration(number);
        this.clicks = 0;
        setTimeout(() => this.startGame(), 800);
      } else if (this.score[this.clicks - 1] === number) {
        this.sequenceGneration(number);
      } else {
        this.message = false;
        this.round = 0;
        this.clicks = 0;
        this.score = [];
        this.roundCount = 0;
      }
    },
  },
};
</script>

<style>
.main-content {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.main-content__title {
  font-size: 30px;
  margin: 30px auto 0;
  text-align: center;
  width: 100%;
}

.simon {
  width: 300px;
  height: 300px;
  border-radius: 150px;
  margin: 50px 50px 0 0;
}

.simon__colors {
  display: flex;
  flex-wrap: wrap;
  list-style: none;
  padding: 0px;
  margin: 0;
}

.simon__color {
  width: 150px;
  height: 150px;
}

.simon__color:hover {
  border: 2px solid black;
  box-sizing: border-box;
}

.simon_blue {
  background-color: rgba(0, 0, 255, 0.5);
  border-radius: 150px 0 0 0;
}

.simon_blueActive {
  background-color: rgba(0, 0, 255, 1);
}

.simon_red {
  background-color: rgba(255, 0, 0, 0.5);
  border-radius: 0 150px 0 0;
}

.simon_redActive {
  background-color: rgba(255, 0, 0, 1);
}

.simon_yellow {
  background-color: rgba(255, 255, 0, 0.5);
  border-radius: 0 0 0 150px;
}

.simon_yellowActive {
  background-color: rgba(255, 255, 0, 1);
}

.simon_green {
  background-color: rgba(0, 128, 0, 0.5);
  border-radius: 0 0 150px 0;
}

.simon_greenActive {
  background-color: rgba(0, 128, 0, 1);
}

.game-info {
  margin: 50px 0 0 0;
}

.game-info__title {
  font-size: 20px;
  font-weight: bold;
  margin: 0;
}

.game-info__levels {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin: 30px 0 0 0;
}

.game-info__level_label {
  font-size: 16px;
  margin-left: 5px;
}

.game-info__button {
  padding: 10px 25px;
  font-size: 18px;
  border: 0;
  border-radius: 5px;
  background-color: dodgerblue;
}

.game-info__button:hover {
  cursor: pointer;
  background-color: rgba(30, 144, 255, 0.5);
}

.game-info__round {
  font-size: 20px;
  font-weight: bold;
}

.message {
  font-size: 18px;
}

.message_hidden {
  position: fixed;
  visibility: hidden;
}
</style>
