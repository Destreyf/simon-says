<template>
  <div>
    <div class="bg-gray-800 pb-32">
      <header class="py-10">
        <div
          class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex justify-between"
        >
          <h1 class="text-3xl font-bold text-white">Simon Says</h1>
          <div>
            <h1
              v-if="sequence.length > 0"
              class="text-3xl font-bold text-white"
            >
              Level {{ sequence.length }}
            </h1>
            <h1
              v-if="sequence.length == 0"
              class="text-3xl font-bold text-white"
            >
              Press any key to start
            </h1>
          </div>
          <div class="w-40">
            <select
              id="colors"
              v-model="numberOfColors"
              class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm rounded-md"
            >
              <option :value="4">4 Colors</option>
              <option :value="5">5 Colors</option>
              <option :value="6">6 Colors</option>
              <option :value="7">7 Colors</option>
              <option :value="8">8 Colors</option>
              <option :value="9">9 Colors</option>
            </select>
          </div>
        </div>
      </header>
    </div>

    <main class="-mt-32">
      <div class="max-w-4xl mx-auto pb-12 px-4 sm:px-6 lg:px-8">
        <!-- Replace with your content -->
        <div class="bg-white rounded-lg shadow p-8">
          <div :class="'grid gap-8 ' + gridSize">
            <div
              v-for="(color, index) in colors"
              :key="color"
              :class="'cursor-pointer w-full aspect-w-1 aspect-h-1 ' + color"
              @click="click(index)"
            ></div>
          </div>
          <div class="w-full mt-8 grid gap-4 grid-cols-11" v-if="userSequence.length > 0">
            <div
              v-for="(selected, index) in userSequence"
              :key="index"
              :class="'relative place-self-center place-content-center text-white grid w-10 h-10 ' + colors[selected]"
            >
            <span>{{index + 1}}</span>
            </div>
          </div>
        </div>
        <!-- /End replace -->
      </div>
    </main>
  </div>
</template>

<script setup>
/* eslint-disable */
import { ref, watch } from "vue";
const numberOfColors = ref(4);

const sequence = ref([]);
const userSequence = ref([]);
const colors = ref([]);

const gridSize = ref("auto-cols-auto");

function shuffle(array) {
  let currentIndex = array.length,
    temporaryValue,
    randomIndex;

  // While there remain elements to shuffle...
  while (0 !== currentIndex) {
    // Pick a remaining element...
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex -= 1;

    // And swap it with the current element.
    temporaryValue = array[currentIndex];
    array[currentIndex] = array[randomIndex];
    array[randomIndex] = temporaryValue;
  }

  return array;
}

function playSound(name) {
  new Audio("sounds/" + name + ".mp3").play();
}

function genSequence() {
  userSequence.value = [];
  const nextItem = (Math.random() * 4) | 0;
  sequence.value = [...sequence.value, nextItem];
  playSound(nextItem);
}

function click(index) {
  userSequence.value.push(index);
}
const baseColors = [
  "bg-trueGray-300",
  "bg-red-300",
  "bg-orange-300",
  "bg-yellow-300",
  "bg-green-300",
  "bg-teal-300",
  "bg-cyan-300",
  "bg-blue-300",
  "bg-indigo-300",
  "bg-purple-300",
  "bg-pink-300",
  "bg-fuchsia-300",
];

const msg = "Hello world";

function reset(fail = false) {
  userSequence.value = [];
  sequence.value = [];
  if (fail) {
    playSound("fail");
  }

  if (numberOfColors.value > 4) {
    gridSize.value = "grid-cols-3 grid-rows-3";
  } else {
    gridSize.value = "grid-cols-2 grid-rows-2";
  }

  colors.value = shuffle(baseColors).slice(0, numberOfColors.value);
}

reset();

watch(numberOfColors, () => reset());
</script>
