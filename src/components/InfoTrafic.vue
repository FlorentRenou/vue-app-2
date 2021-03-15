<template>

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

</template>

<script>
    const apiURL_1 = 'https://data.nantesmetropole.fr/api/records/1.0/search/?dataset=244400404_info-trafic-tan-temps-reel&q=&sort=date_debut'

    export default {
        name: 'InfoTrafic',
        data() {
            return {
            infos: [],
            arrets: new Set(),
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
                    //console.log(this.infos.records[0].fields.listes_arrets)
                    await this.infos.records.forEach((r) => {
                        //console.log(r.fields.listes_arrets);
                        const listes_arrets = JSON.parse(r.fields.listes_arrets);
                        if (listes_arrets && listes_arrets.LISTE_ARRETS.length > 0){
                            listes_arrets.LISTE_ARRETS.forEach( (a) => {
                                console.log(a.CODES);
                                const codes = a.CODES.split(",");
                                console.log(codes);
                                codes.forEach((c) => {
                                    const code = c.slice(0,-1);
                                    this.arrets.add(code);
                                })
                            })
                        }
                    })
                    localStorage.setItem('listes_arrets', JSON.stringify([arrets]));
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
