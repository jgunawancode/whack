<template>
  <h1>Whack-a-mole! <span class="score">{{ score }}</span></h1>
  <button @click="startGame">Start!</button>

  <div class="game">
    <div 
      v-for="mole in moles"
      :key="mole.id"
      class="hole">
      <div class="mole" 
          :class="{ peek: mole.isPeeking }"
          @click="bonk(mole)">
      </div>
    </div>    
  </div>
</template>

<script>
export default {
  data() {
    return {
      lastMoleId: 0,
      moles: [{
        id: 1,
        isPeeking: false
      },{
        id: 2,
        isPeeking: false
      },{
        id: 3,
        isPeeking: false
      },{
        id: 4,
        isPeeking: false
      },{
        id: 5,
        isPeeking: false
      },{
        id: 6,
        isPeeking: false
      }],      
      score: 0,
      timeUp: false
    }
  },
  methods: {
    bonk(mole) {
      this.score++;
      mole.isPeeking = false;
    },
    getRandomMole() {
      const moleId = Math.floor(Math.random() * this.moles.length);
      if (moleId === 0 || this.lastMoleId === moleId) {
        if (moleId === 0)
          console.log(`Mole #${moleId}: Who are you?`);
        else
          console.log(`Mole #${moleId}: I'm peeping already!`);
        return this.getRandomMole();
      }

      this.lastMoleId = moleId;
      console.log(`The mole id is: ${moleId}`);
      let mole = this.moles.find(x => x.id === moleId);
      console.log(mole);
      return mole;
    },
    peep() {
      const time = this.randomTime(500,1000);
      
      let mole = this.getRandomMole();
      mole.isPeeking = true;

      setTimeout(() => {        
        mole.isPeeking = false;
        if (!this.timeUp) this.peep();
      }, time);
    },
    randomTime(min, max) {
      return Math.round(Math.random() * (max - min) + min);
    },
    startGame() {
      this.lastMoleId = 0;
      this.score = 0;
      this.timeUp = false;
      this.peep();
      setTimeout(() => this.timeUp = true, 10000);
    }
  }
}
</script>

<style>
html {
    box-sizing: border-box;
    font-size: 10px;
    background: #ffc600;
  }
  
  *, *:before, *:after {
    box-sizing: inherit;
  }
  
  body {
    padding: 0;
    margin: 0;
    font-family: 'Amatic SC', cursive;
  }
  
  h1 {
    text-align: center;
    font-size: 10rem;
    line-height: 1;
    margin-bottom: 0;
  }
  
  .score {
    background: rgba(255,255,255,0.2);
    padding: 0 3rem;
    line-height: 1;
    border-radius: 1rem;
  }
  
  .game {
    width: 600px;
    height: 400px;
    display: flex;
    flex-wrap: wrap;
    margin: 0 auto;
  }
  
  .hole {
    flex: 1 0 33.33%;
    overflow: hidden;
    position: relative;
  }
  
  .hole:after {
    display: block;
    background: url('./assets/dirt.svg') bottom center no-repeat;
    background-size: contain;
    content: '';
    width: 100%;
    height:70px;
    position: absolute;
    z-index: 2;
    bottom: -30px;
  }
  
  .mole {
    background: url('./assets/mole.svg') bottom center no-repeat;
    background-size: 60%;
    position: absolute;
    top: 100%;    
    width: 100%;
    height: 100%;
    transition:all 0.4s;
  }
  
  .mole.peek {
    top: 0;
  }
</style>
