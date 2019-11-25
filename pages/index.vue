<template>
  <div class="container">
    <div>
      <h1 class="mb-5">もぐらたたき風ゲーム</h1>
      <h2 class="judge" v-if="timeOut">{{ gameFinish() }}</h2>
      <table class="table">
        <tbody>
          <tr>
            <td id="a1" @click="touch()"></td><td id="a2" @click="touch()"></td><td id="a3" @click="touch()"></td>
          </tr>
          <tr>
            <td id="a4" @click="touch()"></td><td id="a5" @click="touch()"></td><td id="a6" @click="touch()"></td>
          </tr>
          <tr>
            <td id="a7" @click="touch()"></td><td id="a8" @click="touch()"></td><td id="a9" @click="touch()"></td>
          </tr>
        </tbody>
      </table>
      <div class="judge" id="message"></div>
      <div class="links">
        <a target="_blank" class="button--green" v-if="state=='ready'" @click="signal()">ゲームを始めます</a>
        <a target="_blank" class="button--green" v-if="state=='finished'" @click="reset()">もう一度プレーする</a>
      </div>
    </div>
  </div>
</template>

<script>
  import Logo from '~/components/Logo.vue'

  export default {
    components: {
      Logo
    },
    
    data() {
      const ids = ["ai", "a2", "a3", "a4", "a5", "a6", "a7", "a8", "a9"]
      return {
        ids: ids,
        masu: "",
        state: "ready",
        id: "",
        timeOut: ""
      }
    },    

    methods: {
      signal(){
        if(this.state == "ready"){
          this.state = "started"
          setTimeout(this.gameFinish, 10000)
          this.id = setInterval(this.moguraOutput, 1200)
        }
      },

      moguraOutput(){
        //20％ぐらいの確率でモグラを召喚したい
        this.masu = document.getElementById(this.ids[Math.floor(Math.random()*9)]);
        if(this.masu.innerHTML ==  ""){
          this.masu.innerHTML = "も"
          setTimeout(this.moguraInput, 900)
        }
      },

      moguraInput(){
        //出したモグラを隠す処理
        this.masu.innerHTML = ""
      },
      
      moguraHit(e){
        //モグラを叩いたときの処理
        if(e.target.innerHTML == "も"){
          document.getElementById("message").innerHTML = "○ あたり！"
        }else{
          document.getElementById("message").innerHTML = "× はずれ！"
        }
      },

      gameFinish(){
        this.state = "finished"
        clearInterval(this.id)
        this.timeOut = true
        return "ゲーム終了〜！！"
      },

      reset(){
        this.state = "ready"
        this.timeOut = ""
        document.getElementById("a1").innerHTML = ""
        document.getElementById("a2").innerHTML = ""
        document.getElementById("a3").innerHTML = ""
        document.getElementById("a4").innerHTML = ""
        document.getElementById("a5").innerHTML = ""
        document.getElementById("a6").innerHTML = ""
        document.getElementById("a7").innerHTML = ""
        document.getElementById("a8").innerHTML = ""
        document.getElementById("a9").innerHTML = ""
        document.getElementById("message").innerHTML = ""
        this.signal()
      },

      touch(){
        if(this.state == "started"){
          document.getElementById("a1").addEventListener('click', this.moguraHit)
          document.getElementById("a2").addEventListener('click', this.moguraHit)
          document.getElementById("a3").addEventListener('click', this.moguraHit)
          document.getElementById("a4").addEventListener('click', this.moguraHit)
          document.getElementById("a5").addEventListener('click', this.moguraHit)
          document.getElementById("a6").addEventListener('click', this.moguraHit)
          document.getElementById("a7").addEventListener('click', this.moguraHit)
          document.getElementById("a8").addEventListener('click', this.moguraHit)
          document.getElementById("a9").addEventListener('click', this.moguraHit)
        }
      }
      
    },
  
  }

</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.table td{
  width: 100px;
	height: 100px;
	font-size: 50px;
	border: 2px solid #ccc;
	text-align:center;
}

.judge {
  color: red;
  font-size:200%;
	font-weight:bold;
}

/* .title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
} */
</style>
