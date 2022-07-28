<script>
  import {reactive, ref, onMounted} from 'vue'
export default {
  setup() {

  let index = ref(0)
  let current = reactive({})
  let audio = new Audio()
  let state = ref(false)

  const data = reactive([
    {source: require('./assets/001.mp3'), surah: 'Al Fatiha', reader: 'Salman Laatibi'},
    {source: require('./assets/112.mp3'), surah: 'Al Ikhlas', reader: 'Salman Laatibi'},
    {source: require('./assets/113.mp3'), surah: 'Al Falaq', reader: 'Salman Laatibi'},
    {source: require('./assets/114.mp3'), surah: 'Al Nas', reader: 'Salman Laatibi'}
  ])

  onMounted(()=> {
    current = data
    audio.src = current[index.value].source
  })

  function rutop() {
    if(state.value == false) {
      audio.play()
      state.value = true
    }
    else {
      audio.pause()
      state.value = false
    }
  }
  function next() {
    if(index.value === data.length-1) {
      index.value = 0
      audio.src = current[index.value].source
      audio.play()
      state.value = true
    }
    else {
      index.value++
      audio.src = current[index.value].source
      audio.play()
      state.value = true
    }
  }
  function previous() {
    if(index.value <= 0) {
      index.value = data.length-1
      audio.src = current[index.value].source
      audio.play()
      state.value = true
    }
    else {
      index.value--
      audio.src = current[index.value].source
      audio.play()
      state.value = true
    }
  }

  return {
    rutop,
    data,
    index,
    current,
    next,
    previous,
    state,
    stop
  }
  },
}
</script>

<template>
  <section>
    <h2>Quran Player</h2>
    <div class="quran-player">
      <div class="logic">
          <div class="surah-name">{{data[index].surah}}</div>
          <div class="reader">{{data[index].reader}}</div> 
          <div class="controls">
          <button @click="previous">Prev</button>
          <button class="pl-tp" @click="rutop">{{state ? 'Stop': 'Play'}}</button>
          <button @click="next">Next</button>
      </div>
    </div>
    </div>
  </section>
</template>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Poppins', sans-serif;
  }
  section {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
  }
  section h2 {
    margin-bottom: 50px;
    color: #495C83;
  }
  section .quran-player {
    background-color: #eee;
    height: 350px;
    width: 260px;
    border-radius: 5px;
  }
  section .quran-player .logic {
    position: absolute;
    top: 60%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
  }
  section .quran-player .surah-name {
    margin: 10px auto;
    background-color: #fff;
    padding: 5px 0;
    width: 100px;
    cursor: default;
    border-radius: 5px;
  }
  section .quran-player .reader {
    background-color: #fff;
    padding: 5px 0;
    cursor: default;
  }
  section .quran-player .controls {
    margin: 20px 0;
  }
  section .quran-player .controls button {
    margin: 0 5px;
    border: none;
    color: #fff;
    padding: 5px 0;
    width: 60px;
    background-color: #495C83;
    border-radius: 5px;
    cursor: pointer;
  }
  section .quran-player .controls button.pl-tp {
    height: 40px;
    line-height: 2.4;
  }
</style>
