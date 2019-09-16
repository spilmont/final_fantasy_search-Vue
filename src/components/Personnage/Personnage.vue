<template>
    <div>
        <h2 class="center-div">Personnage</h2>
        <div class="center-div marign-bottom">
        <input class="center-div right" type="text" v-model.lazy="name" @change="Reset" @keypress.enter="load" placeholder="entrer un nom de personnage">

        <button class="center-div pointeur" @click="load" >Rechercher</button>
            <div v-if="personnage && TotalPerso === 0">Aucun Personage Trouvé</div>
        </div>
        <!----------------------------------RECHERCHE DE PERSONNAGE-------------------------------------->

        <div class="flex arround profil hidden marign-bottom " id="profil" v-if="personnage && TotalPerso >0">

            <div v-if="(Pindex <= index && Pindex >= index-4)|| TotalPerso < 6 " class=" flex   chars left vertical-center  " v-for="(Res,index) in personnage.Results" :key="Res.id" :value="Pindex">
                <img class="right pointeur" @click="choice(index)" :src="Res.Avatar">
                <div class="column">{{Res.Name}}
                    <div class="white">{{Res.Server}}</div>
                </div>
            </div>
        </div>

        <div class="center-div marign-bottom" >
        <input class="pointeur" v-if="personnage && TotalPerso > 5" id="range" value="0"   type="range" min="0" max="45" step="5" @change="pagination">
    </div>

        <div class="flex  profil left  " v-if="profil">
            <img :src="profil.Character.Portrait" width="350" height="550">
            <!----------------------------------TANK CLASS-------------------------------------->
            <div class="left">
                <div class="marign-bottom marign-top">TANK</div>
                <div class="flex job-box wrap  marign-bottom  ">
            <span  v-for="(ClassJobs,index) in profil.Character.ClassJobs">
                  <div class="flex bottom left box"  v-if="index<4">
                <img class="right" :src="Jobs[index].Icon" width="40" height="40">{{Jobs[index].Job}}<br>lvl {{ClassJobs.Level }}<br>
                  </div>
              </span>
                </div>
            <!----------------------------------DPS MELLE CLASS-------------------------------------->

                <div class="marign-bottom">DPS MELLE</div>
                    <div class="flex job-box wrap  marign-bottom">
                    <span  v-for="(ClassJobs,index) in profil.Character.ClassJobs">
                  <div class="flex bottom left box" v-if="index<8 && index>3">
                <img class="right" :src="Jobs[index].Icon" width="40" height="40">{{Jobs[index].Job}}<br>lvl {{ClassJobs.Level }}<br>
                  </div>
              </span>
                </div>
            <!----------------------------------SOIGNEUR CLASS-------------------------------------->

                <div class="marign-bottom">SOIGNEUR</div>
                <div class="flex job-box wrap  marign-bottom">
                    <span v-for="(ClassJobs,index) in profil.Character.ClassJobs">
                  <div class="flex bottom left box" v-if="index<11 && index>7">
                <img class="right" :src="Jobs[index].Icon" width="40" height="40">{{Jobs[index].Job}}<br> lvl {{ClassJobs.Level }}<br>
                  </div>
              </span>
                </div>
            <!----------------------------------DPS DISTANCE CLASS-------------------------------------->

                <div class="marign-bottom">DPS  DISTANCE</div>
                <div class="flex job-box wrap  marign-bottom">
                    <span v-for="(ClassJobs,index) in profil.Character.ClassJobs">
                  <div class="flex bottom left box" v-if="index<18 && index>10">
                <img class="right" :src="Jobs[index].Icon" width="40" height="40">{{Jobs[index].Job}}<br> lvl {{ClassJobs.Level }}<br>
                  </div>
              </span>
                </div>
            <!----------------------------------METIER CLASS-------------------------------------->

                <div class="marign-bottom">METIER</div>
                <div class="flex job-box wrap  marign-bottom">
                    <span  v-for="(ClassJobs,index) in profil.Character.ClassJobs">
                  <div class="flex bottom left box" v-if="index>17 ">
                <img class="right" :src="Jobs[index].Icon" width="40" height="40">{{Jobs[index].Job}}<br> lvl {{ClassJobs.Level }}<br>
                  </div>
              </span>
                </div>
        </div>
        </div>
    </div>


</template>

<script>
    import './JobClass'

    export default {
        name: "Personnage",
        data() {
            return {
                personnage: '',
                profil: '',
                name: "",
                ID: '',
                Jobs: JobClass,
                Pindex:0,
                TotalPerso:0,

            }
        },
        methods: {
            load() {

                this.$personnage = this.$resource('character/search?lang=FR&name=' + this.name)

                this.$personnage.query().then((reponse) => {
                    this.personnage = reponse.data;
                    if(this.personnage){
                        this.TotalPerso = this.personnage.Results.length;
                    }
                }, (reponse) => {
                    console.log('erreur', reponse)
                })

            },
            choice(index) {

                this.ID = this.personnage.Results[index].ID
                if (this.ID) {

                    this.$profil = this.$resource('character/' + this.ID)


                    this.$profil.query().then((reponse) => {
                        this.profil = reponse.data;

                    }, (reponse) => {
                        console.log('erreur', reponse)
                    })
                }
            },
            pagination() {

              var value =   document.getElementById('range').value

               this.Pindex = value

            },
            Reset(){
                if(this.personnage !== this.name ){
                    this.profil ='';
                }
            }
            }


        }



</script>

<style>

    .flex {
        display: flex;
    }

    .arround {
        justify-content: space-around;
    }
    .center-div{
        text-align: center;
    }

    .white {
        color: white;
    }

    .column {
        flex-direction: column;
    }
    .profil{
        border: goldenrod  double 5px;
        border-radius: 5px;
    }
    .job-box{
        width: 100%
    }
    .wrap{
        flex-wrap: wrap;
    }
    .marign-bottom{
        margin-bottom: 15px;
    }
    .marign-top{
        margin-top: 10px;
    }
    .bottom{
        margin-bottom:  7px;
    }
    .left{
        margin-left: 15px;
    }
    .box{
        width: 145px;
    }

    .right{
        margin-right: 5px;
    }
    .hidden{
        overflow: hidden;
    }
    .vertical-center{
        align-items: center;
    }
    input[type=range] {
        background-color: transparent;
        -moz-appearance: none;
        background-color: transparent;
        margin: 3.85px 0;
    }

    input[type=range]::-moz-range-track{
        height: 16px;
        background-color: transparent;
        background-image: url("../../assets/textures/slide-bar.png");
         background-size: 100% 100%
    }

    input[type=range]::-moz-range-thumb{
        height: 30px;
        background-color: transparent;
        background-image: url("../../assets/textures/position-bar.png");
        background-size: 100% 100%;
        border: none;
    }

    input[type=text]{
        width: 20%;
        background-color: transparent;
        border: none;
        border-bottom: goldenrod solid  1px;
        border-top: goldenrod solid  1px;
        color: wheat;
    }

    button{
        background-color: transparent;
        color: wheat;
        border: goldenrod solid 1px;
    }

    .pointeur{
        cursor: pointer;
    }





</style>
