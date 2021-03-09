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
    const apiURL = 'https://data.nantesmetropole.fr/api/records/1.0/search/?dataset=244400404_info-trafic-tan-temps-reel&q=&sort=date_debut'

    export default {
        name: 'InfoTrafic',
        data() {
            return {
            infos: []
            }
        },
        created: function () {
            this.fetchData()
        },
        methods: {
            fetchData: async function () {
                try {
                    const response = await this.axios.get(apiURL)
                    this.infos = response.data
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
