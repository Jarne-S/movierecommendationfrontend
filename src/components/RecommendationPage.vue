<template>
  <div class="center">
    <h1 class="titel">
      AI powered movie recommendations.
    </h1>

    <!-- BUTTONS CHANGING LOCAL HOST PORT OR NEW RECOMMENDATION -->
    <div class="buttons">
      <button v-if="localhostConfirmed" @click="removeRecommendation(), removeUserinput(), changeLocalPort()">
      Change local host port
      </button>

      <button v-if="showmovie" @click="removeRecommendation(), makeAnotherRecommendation()">
        Make another recommendation
      </button>
    </div>

    <!-- USER INPUT LOCAL HOST PORT -->
    <div v-if="localhostConfirmed == false">
      Enter API local host port:
      <input type="text" v-model="localhostport" class="padding" >
      <button @click="assignLocalport(), showinput()">
        Confirm local host port.
      </button>
    </div>

    <!-- USER INPUT MOVIE RECOMMENDATION -->
    <div v-if="showuserinputfield">
      Input for movie recommendation:
      <input type="text" v-model="userinput" class="padding breedte"/>
      <button @click="getRecommendation()">
        Get Recommendation
      </button>
    </div>

    <!-- MOVIE RECOMMENDATION TONEN -->
    <div v-if="showmovie" class="filmoverzicht">
      <img v-bind:src="posterurl" class="poster" v-bind:alt="titel + ' poster'"/>
      <div class="links">
        <h2>
        {{ titel }}
        </h2>

        <p>
          Jaartal: {{jaartal}}
        </p>

        <p>
          Genre('s): {{ genres }}
        </p>

        <p>
          Looptijd: {{ looptijd }}
        </p>

        <p>
          Beschrijving: {{ beschrijving }}
        </p>

        <p>
          Beschikbaarheid:
        </p>

        <ul>
          <li v-for="(dienst, index) in beschikbaarheid" :key="'dienst' + index">
            {{ dienst }}
          </li>
        </ul>

        <iframe width="600" height="338" v-bind:src="'https://www.youtube.com/embed/' + youtubeidentifier" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
        </iframe>
      </div>
    </div>

  </div>
</template>

<script>

export default {
  name: 'RecommendationPage',
  methods:{
    assignLocalport(){
      this.localhostConfirmed = true;
      console.log(this.localhostport)
    },
    showinput(){
      this.showuserinputfield = true;
    },
    getRecommendation(){
      fetch("https://localhost:"+ this.localhostport +"/MovieRecommendation/" + this.userinput)
      .then((response) => response.json())
      .then((data)=>{
        console.log(data);
        this.titel = data.titel;
        this.jaartal = data.jaartal;
        this.beschrijving = data.plot;
        this.looptijd = data.looptijd;
        this.genres = data.genres;
        this.beschikbaarheid = data.beschikbaarheid;
        this.posterurl = data.posterURL;
        this.youtubeidentifier = data.youTubeTrailerIdentifier;
        this.showuserinputfield = false;
        this.showmovie = true;
      });
    },
    changeLocalPort(){
      this.localhostConfirmed = false;
    },
    makeAnotherRecommendation(){
      this.showuserinputfield = true;
    },
    removeRecommendation(){
      this.showmovie = false;
    },
    removeUserinput(){
      this.showuserinputfield = false;
    }
  },
  data(){
    return{
      localhostport : "",
      localhostConfirmed : false,
      showuserinputfield : false,
      userinput : "",
      showmovie : false,
      titel : "",
      jaartal : 0,
      beschrijving : "",
      looptijd : "",
      genres: "",
      beschikbaarheid: [],
      posterurl : "",
      youtubeidentifier : ""
    }
  },
}
</script>


<style scoped>
.filmoverzicht{
  margin-top: 2.5vh;
  display:flex;
  column-gap: 2vw;
  align-content: center;
  justify-content: center;
}
.buttons{
  margin-bottom: 20px;
  display:flex;
  align-content: center;
  column-gap: 5vw;
  text-align: center;
  justify-content: center;
}
.poster{
  height:  75vh;
}
.titel{
  text-align: center;
}
.center{
  text-align: center;
  justify-content: center;
}
.links{
  text-align: left;
  width: 35vw;
}
.padding{
  margin-left: 5px;
  margin-right: 15px;
}
.breedte{
  width: 300px;
}
</style>