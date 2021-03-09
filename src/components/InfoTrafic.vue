<template>
    <div id="demo">
  		<div class="container-fluid">
            <div class="row">
                <div class="col-md-12">
                    <h3 class="text-center">
                        Info-trafic TAN temps réel
                    </h3>
                </div>
            </div>
            <br>
            <div class="bar">
                <input type="text" v-model="searchString" placeholder="Entrer votre Arrêt" />
            </div>
            <br>
            <div class="row" v-for="record in infos.records" >
                <div class="col-md-12">
                    <h3 class="text-center">
                        {{record.fields.intitule}}
                    </h3>
                    <div class="row">
                        <div class="col-md-6">
                            <div class="text-danger text-left" >
                                 Date de début : {{record.fields.date_debut}}
                            </div>
                        </div>
                        <div class="col-md-6">
                            <div class="text-success text-right" v-if="record.fields.date_fin">
                              Date de fin : {{record.fields.date_fin}}
                            </div>
                        </div>
                    </div>
                    <p class="text-info">
                        {{record.fields.resume}}
                    </p>
                    <br>
                </div>
            </div>
  	    </div>
  	</div>
</template>

<script>
    const apiURL_1 = 'https://data.nantesmetropole.fr/api/records/1.0/search/?dataset=244400404_info-trafic-tan-temps-reel&q=&sort=date_debut'
    const apiURL_2 = 'https://data.nantesmetropole.fr/api/records/1.0/search/?dataset=244400404_tan-arrets&q=&rows=10&refine.stop_id='

    export default {
        name: 'InfoTrafic',
        data() {
            return {
            infos: [],
            arrets_1: [],
            arrets_2: [],
            searchString: ""
            }
        },
        created: function () {
            this.fetchData()
        },
        methods: {
            fetchData: async function () {
                try {

                    const response = await this.axios.get(apiURL_1);
                    this.infos = response.data;
                    infos.forEach(data => arrets_1 = data.fields.listes_arrets);
                    console.log(arrets_1);

                    //arrets_1.forEach(data => {
                    //    const reponse2 = await this.axios.get(apiURL_2 + data);
                    //    arrets_2 = arrets_2 + reponse2.data;
                    //}

                } catch (error) {
                    console.log(error);
                }
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
