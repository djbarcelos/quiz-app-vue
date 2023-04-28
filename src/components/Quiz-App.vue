<template>
  <!-- Start Game -->
  <section v-show="gameState">
    <!-- points -->
    <span>Acertos: {{ hits }} | Erros: {{ misses }}</span>
    <!-- Question -->
    <transition name="nested" duration="550" appear>
      <div class="card outer" v-show="!notification.visible">
        <span>{{ questionCurr.question }}</span>
        <div class="options inner">
          <div
            class="option"
            v-for="(item, i) in questionCurr.answers"
            :key="i"
            @click="validateAnswer(item.correct)"
          >
            <span>{{ i + 1 }}</span>
            <p>{{ item.text }}</p>
          </div>
        </div>
      </div>
    </transition>
  </section>
  <!-- End Game -->
  <section v-show="!gameState">
    <transition name="nested" duration="350" appear>
      <div class="card outer" v-show="!gameState">
        <div class="inner">
          Você acertou {{ hits }} perguntas de um total {{ hits + misses }}.
        </div>
        <button class="btn-game inner" @click="startGame">JOGAR</button>
      </div>
    </transition>
  </section>
  <!-- Notifications -->
  <section>
    <transition name="slide" mode="out-in" appear>
      <div
        class="notification"
        :class="notification.type"
        v-if="notification.visible"
      >
        <div>{{ notification.text }}</div>
      </div>
    </transition>
  </section>
</template>

<script>
import { questions } from "@/questions";

export default {
  data() {
    return {
      questionCurr: {},
      questions,
      exception: [],
      gameState: false,
      hits: 0,
      misses: 0,
      notification: {
        visible: false,
      },
    };
  },
  methods: {
    startGame() {
      this.gameState = !this.gameState;
      this.exception = [];
      this.hits = 0;
      this.misses = 0;
      this.raffleNewQuestion();
    },
    validateAnswer(response) {
      if (response) {
        this.notification = {
          visible: true,
          type: "hits",
          text: "Parabéns resposta certa!!",
        };
        this.hits++;
      } else {
        this.notification = {
          visible: true,
          type: "misses",
          text: "Que pena resposta errada.",
        };
        this.misses++;
      }
    },
    raffleNewQuestion() {
      const index = Math.floor(Math.random() * this.questions.length);
      if (this.exception.includes(index)) {
        return this.raffleNewQuestion();
      }
      this.exception.push(index);
      this.shuffle(this.questions[index].answers);
      this.questionCurr = this.questions[index];
    },
    shuffle(array) {
      var indice_atual = array.length,
        valor_temporario,
        indice_aleatorio;

      while (0 !== indice_atual) {
        indice_aleatorio = Math.floor(Math.random() * indice_atual);
        indice_atual -= 1;

        valor_temporario = array[indice_atual];
        array[indice_atual] = array[indice_aleatorio];
        array[indice_aleatorio] = valor_temporario;
      }
      return array;
    },
  },
  watch: {
    misses() {
      if (this.misses === 3) {
        return (this.gameState = false);
      }
      this.raffleNewQuestion();
    },
    hits() {
      this.raffleNewQuestion();
    },
    notification() {
      setTimeout(() => {
        this.notification.visible = false;
      }, 1000);
    },
  },
};
</script>

<style>
.card {
  background-color: #fff;
  width: 80%;
  margin: 20px auto;
  border-radius: 10px;
  padding: 20px;
}

.btn-game {
  font-size: 24px;
  border-radius: 10px;
  height: 50px;
  width: 200px;
  margin-top: 50px;
  background: linear-gradient(to right, #008de3, #22a5c2);
  color: #fff;
  font-weight: 600;
  border: 0;
  cursor: pointer;
}

.options {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.option {
  background-color: lightgreen;
  box-sizing: border-box;
  height: 100px;
  width: 350px;
  margin: 20px 20px;
  border-radius: 10px;
  font-size: 24px;
  cursor: pointer;
  user-select: none;
  display: flex;
  align-items: center;
}

.option p {
  display: flex;
  margin: 0 auto;
}

.option span {
  background-color: green;
  height: 100%;
  width: 20%;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px 0 0 10px;
}

.notification {
  position: absolute;
  top: 10px;
  right: 20px;
  width: 300px;
  margin: 20px auto;
  border-radius: 10px;
  padding: 20px;
}

.hits {
  background-color: rgb(10, 164, 10);
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
}

.misses {
  background-color: rgb(193, 10, 10);
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* min-width */
@media screen and (min-width: 600px) {
  .card {
    width: 60%;
  }

  .option {
    margin: 40px 100px;
    font-size: 2rem;
  }
}

/* Animation */

@keyframes slide-in {
  from {
    transform: translateY(40px);
  }
  to {
    transform: translateY(0);
  }
}

@keyframes slide-out {
  from {
    transform: translateY(0);
  }
  to {
    transform: translateY(40px);
  }
}

.slide-enter-active,
.slide-leave-active {
  animation: slide-in 0.5s ease;
  transition: opacity 0.5s;
}

.slide-enter-from,
.slide-leave-to {
  opacity: 0;
}

/* */

.nested-enter-active,
.nested-leave-active {
  transition: all 0.3s ease-in-out;
}

.nested-leave-active {
  transition-delay: 0.25s;
}

.nested-enter-from,
.nested-leave-to {
  transform: translateY(30px);
  opacity: 0;
}

.nested-enter-active .inner,
.nested-leave-active .inner {
  transition: all 0.3s ease-in-out;
}

.nested-enter-active .inner {
  transition-delay: 0.25s;
}

.nested-enter-from .inner,
.nested-leave-to .inner {
  transform: translateX(30px);
  opacity: 0.001;
}
</style>