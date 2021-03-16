<template>
  <div id="app">
    <div v-if="counter===1">
      <p class="text1">Лучшие астрологи и экстрасенсы Румынии</p>
      <div class="hr"/>
      <p class="text2">Точность прогноза: 97%</p>
      <div class="face">
        <img src="../src/assets/img/woman1.png" alt="" class="woman">
        <img src="../src/assets/img/icon1.svg" alt="" class="bg-icon">
        <img src="../src/assets/img/blik.png" alt="" class="blik">
      </div>
      <p class="text3">
        Вас беспокоит вопрос о том,<br><span>когда Вы покинете этот Мир и при каких обстоятельствах</span>?
      </p>

      <div class="btn btn-yes">
        Да
      </div>
      <div class="btn btn-no">
        Нет
      </div>
      <p class="text1">Онлайн предсказание</p>

      <div class="block-hands">
        <img src="../src/assets/img/hands.png" alt="" class="hands">
        <p class="text4">
          Позвольте нам раскрыть эту волнующую тайну и<br><span>с точностью определить дату и время вашей смерти,</span>
          а
          также предшествующую этому событию причину
        </p>
      </div>

      <div class="yoga">
        <p class="text4">
          Многие не верят предсказаниям и мы решили доказать каждому,<br><span>что прогноз может изменить жизнь любого человека!</span>
        </p>
      </div>

      <div class="question-start">
        <div class="question-title">
          Боитесь ли вы умереть?
        </div>
        <div class="btn btn-yes" @click="nextQuestion">
          Да
        </div>
        <div class="btn btn-no" @click="nextQuestion">
          Нет
        </div>
        <div class="question-counter text1">
          Вопрос {{ counter }} - 5
        </div>
      </div>
    </div>
    <div class="question" v-if="counter===2">
      <img src="../src/assets/img/eye-1.svg" alt="" class="eye1">
      <img src="../src/assets/img/eye.svg" alt="" class="eye2">
      <div class="question-header">
        <span>Мы расскажем Вам не только подробности вашей смерти, но также поможем Вам избежать этой ужасной даты и продлить
        вашу жизнь на многие годы.</span>
      </div>
      <div class="question-title">
        Когда Вы чувствуете себя наиболее комфортно?
      </div>
      <div class="btn btn-yes" @click="nextQuestion">
        Утро
      </div>
      <div class="btn btn-no" @click="nextQuestion">
        День
      </div>
      <div class="btn btn-yes" @click="nextQuestion">
        Вечер
      </div>
      <div class="btn btn-no" @click="nextQuestion">
        Ночь
      </div>
      <div class="question-counter text1">
        Вопрос {{ counter }} - 5
      </div>
    </div>

    <div class="question" v-if="counter===3 && !loading">
      <img src="../src/assets/img/eye-1.svg" alt="" class="eye1">
      <img src="../src/assets/img/eye.svg" alt="" class="eye2">
      <div class="question-header">
        <span>Уже совсем скоро Вы узнаете много интересного о своем будущем!</span>
      </div>
      <div class="question-title">
        Укажите свою дату рождения:
      </div>
      <div class="selects">
        <div class="btn btn-select" @click="chooseDay = true" :class="{'error': dayError}">
          {{ isDayChoosen ? choosenDay : 'День' }}
        </div>
        <div class="select-items" v-if="chooseDay">
          <div class="select-item" v-for="(day,i) in days" :key="i" @click="selectDay(day)">{{ day }}</div>
        </div>
      </div>
      <div class="selects">
        <div class="btn btn-select" @click="chooseMonth = true" :class="{'error': monthError}">
          {{ isMonthChoosen ? choosenMonth : 'Месяц' }}
        </div>
        <div class="select-items" v-if="chooseMonth">
          <div class="select-item" v-for="(month,i) in months" :key="i" @click="selectMonth(month)">{{ month }}</div>
        </div>
      </div>
      <div class="selects">
        <div class="btn btn-select" @click="chooseYear = true" :class="{'error': yearError}">
          {{ isYearChoosen ? choosenYear : 'Год' }}
        </div>
        <div class="select-items" v-if="chooseYear">
          <div class="select-item" v-for="(year,i) in years" :key="i" @click="selectYear(year)">{{ year }}</div>
        </div>
      </div>
      <button class="btn" @click="goNext">
        Далее
      </button>
      <div class="question-counter text1">
        Вопрос {{ counter }} - 5
      </div>
    </div>

    <div class="question loading" v-if="loading">
      <img src="../src/assets/img/loading.svg" alt="">
    </div>

    <div class="runes" v-if="counter===1">
      <img src="../src/assets/img/rune1.svg" alt="">
      <img src="../src/assets/img/eye-1.svg" alt="" class="eye1">
      <p class="text5">
        Вы, конечно, умрете.<br>
        И все, с кем вы знакомы,<br>
        тоже однажды умрут.
      </p>
      <img src="../src/assets/img/rune2.svg" alt="">
      <img src="../src/assets/img/eye.svg" alt="" class="eye2">
    </div>

    <div class="question" v-if="counter===4">
      <img src="../src/assets/img/eye-1.svg" alt="" class="eye1">
      <img src="../src/assets/img/eye.svg" alt="" class="eye2">
      <div class="question-header">
        <span>Смерть родного человека – одно из тяжелейших испытаний в жизни каждого из нас!</span>
      </div>
      <div class="question-title">
        Снятся ли Вам умершие люди?
      </div>

      <button class="btn" @click="nextQuestion">
        Да
      </button>
      <button class="btn" @click="nextQuestion">
        Нет
      </button>
      <button class="btn" @click="nextQuestion">
        Иногда
      </button>
      <div class="question-counter text1">
        Вопрос {{ counter }} - 5
      </div>
    </div>

    <div class="question" v-if="counter===5 && !audioLoading">
      <img src="../src/assets/img/eye-1.svg" alt="" class="eye1">
      <img src="../src/assets/img/eye.svg" alt="" class="eye2">
      <div class="question-header fifth">
        <span>{{ fifthText }}</span>
      </div>
      <div class="question-title">
        Запись, которую Вы услышите, может шокировать людей с неокрепшей психикой. Вы готовы узнать, что ждет именно
        Вас?
      </div>

      <button class="btn" @click="goAudio">
        Да
      </button>
      <button class="btn" @click="goAudio">
        Затрудняюсь ответить
      </button>
      <div class="question-counter text1">
        Вопрос {{ counter }} - 5
      </div>
    </div>

    <div class="question loading audio" v-if="audioLoading">
      <img src="../src/assets/img/microphone.svg" alt="">
      <img src="../src/assets/img/audio.svg" alt="">
      <div class="text6">
        <div class="perc"/>
        <span>Запись сообщения</span>
      </div>
    </div>

    <div class="question lastPage" v-if="endPage">
      <div class="question-header fifth end">
        <span>Спасибо за Ваши ответы!<br>
          <span>Мы подготовили для Вас персональную аудио запись с Вашим прогнозом.</span>
        </span>
      </div>
      <div class="text7">
        Вы можете узнать, как повлиять на события, которые ожидают вас в ближайшем будущем.
      </div>

      <div class="block-hands">
        <span>Первое значимое событие может произойти уже 14.02.2021,</span> Вам надо быть готовым, что бы последствия
        не оказались необратимыми.
      </div>

      <div class="text7">
        Нажмите на кнопку ниже прямо сейчас и наберите наш номер телефона. Прослушайте важную информацию!
      </div>

      <div class="btn btn-call">
        Позвонить и прослушать
      </div>
    </div>


    <div class="footer" v-if="counter===1">
      TERMENI SI CONDITII: ACESTA ESTE UN SERVICIU DE DIVERTISMENT. PRIN FOLOSIREA LUI DECLARATI CA AVETI 18 ANI
      IMPLINITI,
    </div>
  </div>

</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      counter: 1,
      chooseMonth: false,
      choosenMonth: '',
      isMonthChoosen: false,
      months: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'],
      chooseDay: false,
      choosenDay: '',
      isDayChoosen: false,
      chooseYear: false,
      choosenYear: '',
      isYearChoosen: false,
      age: null,
      disabledButton: false,
      dayError: false,
      monthError: false,
      yearError: false,
      loading: false,
      audioLoading: false,
      endPage: false
    }
  },
  methods: {
    nextQuestion() {
      this.counter++
    },
    selectMonth(month) {
      this.choosenMonth = month
      this.chooseMonth = false
      this.isMonthChoosen = true
      this.monthError = false
    },
    selectDay(day) {
      this.choosenDay = day
      this.chooseDay = false
      this.isDayChoosen = true
      this.dayError = false
    },
    selectYear(year) {
      this.choosenYear = year
      this.chooseYear = false
      this.isYearChoosen = true
      this.yearError = false
      this.age = new Date().getFullYear() - year
    },
    notChoosen() {
      this.disabledButton = true
    },
    goNext() {
      if (this.isValid) {
        this.loading = true
        setTimeout(() => {
          this.counter++
          this.loading = false
        }, 2000)
      } else if (!this.isDayChoosen) this.dayError = true
      else if (!this.isMonthChoosen) this.monthError = true
      else if (!this.isYearChoosen) this.yearError = true
    },
    goAudio() {
      this.audioLoading = true
      setTimeout(() => {
        this.counter++
        this.endPage = true
        this.audioLoading = false
      }, 5000)
    }
  },
  components: {},
  computed: {
    days() {
      const arr = []
      for (let i = 1; i < 32; i++) arr.push(i)
      return arr
    },
    years() {
      const arr = []
      for (let i = 1940; i < 2004; i++) arr.push(i)
      return arr
    },
    isValid() {
      return this.isDayChoosen && this.isMonthChoosen && this.isYearChoosen
    },
    fifthText() {
      if (this.age <= 35) return 'По вам скучает очень близкий человек, которого больше нет в мире живых.'
      else if (this.age > 35 && this.age <= 45) return 'По вам скучает очень близкий человек, которого больше нет в мире живых. Возможно это дедушка или бабушка.'
      return 'По вам скучает очень близкий человек, которого больше нет в мире живых. Возможно это кто-то из Ваших родителей.'
    }
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Bad+Script&display=swap');

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

h1, h2, h3, h4, h5 {
  font-size: 0;
  font-weight: unset;
}

body {
  background-color: #202024;
}

#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  overflow: hidden;
}

.text1 {
  font-size: 12px;
  line-height: 14px;
  text-align: center;
  letter-spacing: 0.1em;
  color: rgba(255, 255, 255, 0.6);
  padding-top: 21px;
  padding-bottom: 11px;
  font-weight: 300;
}

.text2 {
  font-weight: 300;
  font-size: 20px;
  line-height: 23px;
  text-align: center;
  letter-spacing: 0.1em;
  color: rgba(255, 255, 255, 0.6);
  margin-top: 7px;
}

.text3 {
  font-weight: normal;
  font-size: 16px;
  line-height: 25px;
  text-align: center;
  color: #F6C866;
  padding: 16px;
  margin-top: 20px;
}

.text3 span {
  text-transform: uppercase;
}

.text4 {
  font-weight: normal;
  font-size: 16px;
  line-height: 25px;
  text-align: center;
  color: #FFFFFF;
}

.text4 span {
  font-size: 20px;
  color: #F6C866;
}

.hr {
  width: 100%;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.face {
  margin-top: 60px;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
}

.woman {
  position: relative;
  z-index: 1;
  width: 137px;
}

.bg-icon {
  position: absolute;
  z-index: -1;
  opacity: 0.2;
}

.blik {
  position: absolute;
  z-index: -1;
  width: 130%;
}

.btn {
  width: 179px;
  height: 40px;
  background: linear-gradient(90deg, rgba(246, 200, 102, 0.9) -6.2%, rgba(254, 173, 53, 0.9) 100%);
  border-radius: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  font-size: 14px;
  line-height: 16px;
  color: #ffffff;
}

.btn.error {
  border: 2px solid #EE5353;
}

.btn + .btn {
  margin-top: 20px;
}

.block-hands {
  border: 1px solid #FFFFFF;
  box-sizing: border-box;
  border-radius: 3px;
  margin: 70px 33px;
  padding: 58px 23px 32px;
  position: relative;
}

.hands {
  position: absolute;
  top: 0;
  margin: 0 auto;
  width: 78px;
  left: 50%;
  transform: translate(-50%, -50%);
}

.yoga {
  width: 100%;
  position: relative;
  height: 330px;
  background-image: url('../src/assets/img/image 7.jpg');
  background-position: 50% 50%;
  background-size: cover;
  padding: 100px 50px 80px;
}

.yoga .text4 {
  margin: 0 auto;
  position: relative;
  z-index: 2;
}

.yoga:before {
  content: '';
  z-index: 1;
  position: absolute;
  left: 0;
  top: 0;
  opacity: 0.7;
  width: 100%;
  height: 100%;
  background: #000000;
}

.yoga img {
  position: absolute;
  width: 100%;
  top: 0;
  left: 0;
  z-index: -1;
}

.question-start {
  padding-top: 53px;
}

.question-title {
  font-weight: normal;
  font-size: 16px;
  line-height: 19px;
  text-align: center;
  text-transform: uppercase;
  color: #F6C866;
  margin-bottom: 27px;
}

.runes {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 30px 0;
  margin: 30px 0;
}

.text5 {
  font-family: Bad Script, serif;
  font-style: normal;
  font-weight: normal;
  font-size: 20px;
  line-height: 28px;
  color: #FFFFFF;
  margin: 37px 0 33px;
}

.eye1, .eye2 {
  position: absolute;
  opacity: 0.2;
}

.eye1 {
  top: 0;
  right: 0;
  transform: translateX(33%);
}

.eye2 {
  bottom: 0;
  left: 0;
  transform: translateX(-33%);
}

.footer {
  font-size: 7px;
  line-height: 9px;
  text-align: center;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: #9D9D9D;
  padding: 0 13px;
}

.question {
  position: relative;
  padding-bottom: 25px;
}

.question.loading {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.question.loading.audio {
  flex-direction: column;
}

.question-header {
  font-family: Bad Script, serif;
  font-weight: normal;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  color: #FFFFFF;
  opacity: 0.6;
  height: 124px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  margin-bottom: 36px;
}

.question-header span {
  width: 235px;
}

.question-header.fifth {
  opacity: 1;
}

.question-header.fifth > span {
  width: 259px;
  height: 84px;
  color: #202024;
  background-color: #ffffff;
  border-radius: 5px;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 15px;
}

.question-header.fifth > span:after {
  content: '';
  position: absolute;
  width: 0;
  height: 0;
  border-style: solid;
  border-width: 12px 7px 4px;
  border-color: #ffffff transparent transparent transparent;
  bottom: -16px;
  right: 20px;
}

.question .question-title {
  margin-bottom: 40px;
  padding: 0 30px;
}

.question .question-counter {
  margin-top: 36px;
}

.question .eye1 {
  top: 65px;
}

.question .eye2 {
  bottom: 25px;
}

.btn-select {
  background: linear-gradient(90deg, rgba(228, 228, 228, 0.9) -6.2%, rgba(203, 203, 203, 0.9) 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  color: #202024;
  position: relative;
}

.btn-select:after {
  content: '';
  position: absolute;
  width: 0;
  height: 0;
  border-style: solid;
  border-width: 5px 7px 8px;
  border-color: #202024 transparent transparent transparent;
  right: 10px;
  top: 18px;
}

.selects {
  position: relative;
  margin-bottom: 20px;
}


.select-items {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 179px;
  height: 200px;
  background: rgb(203, 203, 203);
  overflow: scroll;
  z-index: 3;
  border-radius: 25px;
}

.select-item {
  padding: 10px;
  border-bottom: 1px solid #202024;
}

.perc::after {
  font-weight: 300;
  font-size: 20px;
  line-height: 23px;
  letter-spacing: 0.1em;
  color: rgba(255, 255, 255, 0.6);
  content: counter(count) "%";
  animation: counter 5s linear infinite alternate;
  counter-reset: count 0;
}

@keyframes counter {
  0% {
    counter-increment: count 0;
  }
  10% {
    counter-increment: count 10;
  }
  20% {
    counter-increment: count 20;
  }
  30% {
    counter-increment: count 30;
  }
  40% {
    counter-increment: count 40;
  }
  50% {
    counter-increment: count 50;
  }
  60% {
    counter-increment: count 60;
  }
  70% {
    counter-increment: count 70;
  }
  80% {
    counter-increment: count 80;
  }
  90% {
    counter-increment: count 90;
  }
  100% {
    counter-increment: count 100;
  }
}

.text6 > span {
  font-weight: 300;
  font-size: 12px;
  line-height: 14px;
  letter-spacing: 0.1em;
  color: rgba(255, 255, 255, 0.6);
}

.question-header.fifth.end {
  border: none;
}


.question-header.fifth.end > span {
  font-family: Roboto, sans-serif;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 18px;
  flex-direction: column;
}

.question-header.fifth.end > span > span {
  font-weight: 700;
}

.text7 {
  font-style: normal;
  font-weight: 300;
  font-size: 14px;
  line-height: 16px;
  color: #FFFFFF;
  width: 220px;
}

.lastPage {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-bottom: 15px;
}

.lastPage .block-hands {
  padding: 13px 15px 8px;
  font-size: 16px;
  line-height: 25px;
  color: #F6C866;
  margin: 20px 30px;
}

.lastPage .block-hands span {
  text-transform: uppercase;
  font-weight: 700;
}

.end{
  margin-bottom: 10px;
}

.btn-call{
  width: 240px;
  background: linear-gradient(90deg, rgba(76, 217, 100, 0.9) -6.2%, rgba(50, 185, 73, 0.9) 100%);
  margin-top: 20px;
}
</style>
