<template>
<div id="app">
  <b-form-group label="Please, select Males/Females and click on Select button">
    <b-form-checkbox-group id="checkbox-group-1" v-model="selectedSexes">
      <b-form-checkbox value="males">Males</b-form-checkbox>
      <b-form-checkbox value="females">Females</b-form-checkbox>
      <b-button @click="updateTable">Select</b-button>
    </b-form-checkbox-group>    
  </b-form-group>
  
  <div>Selected: <strong>{{ selectedSexes }}</strong></div>
  
  <div class="container">
    <div class="row">
      <b-table
        :sort-by.sync="sortBy"
        :fields="fields1"
        :items="itemsTable"
        striped hover
        >
        </b-table>     
    </div>    
    <div class="row">      
      <b-table
        :fields="fields2"
        striped hover :items="itemsTable2">
      </b-table>
    </div>

    <b-form-group label="Population Scaling: Enter factors a and b in the fields bellow, and click on Calculate Population Scaling Button">
      <b-container>
        <b-row class="scaling">          
          <b-col md="auto">
            <b-row>Factor A <b-form-input v-model="factorA" placeholder="Enter factor a"></b-form-input></b-row>
            <b-row>Factor B <b-form-input v-model="factorB" placeholder="Enter factor b"></b-form-input></b-row>

            <b-row>{{ this.errorMessage }}</b-row>

            <b-row><b-button @click="computePopulationScaling">Calculate Population Scaling</b-button></b-row>

          </b-col>
          <b-col col lg="1" >
          </b-col>
          
          <b-col class="resutls">
            <b-row class="justify-content-md-center">
              Scaling Factors
            </b-row>
            
            <b-row>
              <b-col>Males ({{this.totalPop["Males"]}})</b-col>
              <b-col>{{ this.scalingFactorMales }}</b-col>
            </b-row>

            <b-row>
              <b-col>Females ({{this.totalPop["Females"]}})</b-col>
              <b-col>{{ this.scalingFactorFemales }}</b-col>
            </b-row>

            <b-row>
              <b-col>All ({{this.totalPop["All"]}})</b-col>
              <b-col>{{ this.scalingFactorAll }}</b-col>
            </b-row>
          </b-col>          
        </b-row>
      </b-container>     
    </b-form-group>        
  </div>  
</div>

</template>

<script>

import jsonData from './assets/planets_males.json';
//import Vue from 'vue'
//import BootstrapVue from 'bootstrap-vue'

var malesOnPlanets = {}
var femalesOnPlanets = {}
var planets = []
var populationOnPlanets = [] // array of items to show on table
var totalPopulation = {} // map of items  to show on foot table
var rowForTotalPopulation = [] // array of items for tota

export default {
    name: 'app',
    mounted(){
        console.log("mounted")
    },
    created() {
        console.log("created")
        const arr = jsonData.map(p => p.Planet)
        const setPlanets = new Set(arr)
        planets = Array.from(setPlanets)

        // Build hash maps for males and females from planet to number of individuals
        planets.forEach(function(planet){
            var malesByPlanet   = jsonData.filter(item => item.Planet === planet && item.Males === 1)
            var femalesByPlanet = jsonData.filter(item => item.Planet === planet && item.Males === 0)
            
            malesOnPlanets[planet]   = malesByPlanet.length
            femalesOnPlanets[planet] = femalesByPlanet.length
        })
        
        console.log(malesOnPlanets)
        console.log(femalesOnPlanets)
        console.log(planets)

        planets.forEach(function(planet){
            populationOnPlanets.push({"Planet" :planet,
                                      "Males"  :malesOnPlanets[planet],
                                      "Females":femalesOnPlanets[planet]})
        })

        totalPopulation["Males"]   = jsonData.filter(item => item.Males === 1).length
        totalPopulation["Females"] = jsonData.filter(item => item.Males === 0).length
        var totalpop = totalPopulation["Males"] + totalPopulation["Females"];
        totalPopulation["All"] = totalpop
        
        rowForTotalPopulation.push({"Set of Planets":"Solar System",
                                    "Total Males":totalPopulation["Males"],
                                    "Total Females":totalPopulation["Females"],
                                    "Total Population":totalpop})
        //populationOnPlanets.push({"Planet":"Z All Planets","Males":totalPopulation["Males"], "Females":totalPopulation["Females"]})
        //rowForTotalPopulation.push({"Total":"All Planets","Males":totalPopulation["Males"], "Females":totalPopulation["Females"]})
    },
    methods: {
        updateTable(){
            console.log("udate table")
            
            var fieldsOnchange = []
            var fieldsOnchange2 = []
            fieldsOnchange.push({ key: 'Planet',  sortable: true })
            fieldsOnchange2.push({ key: 'Set of Planets'})
            
            if (this.selectedSexes.includes('males'))
            {
                fieldsOnchange.push({key: 'Males', sortable: true})
                fieldsOnchange2.push({key: 'Total Males'})
            }
            if (this.selectedSexes.includes('females'))
            {
                fieldsOnchange.push({key:'Females', sortable: true})
                fieldsOnchange2.push({key: 'Total Females'})
            }

            if (this.selectedSexes.includes('males') && this.selectedSexes.includes('females') ){
                fieldsOnchange2.push({key: 'Total Population'})
            }
            
            this.fields1 = fieldsOnchange
            this.fields2 = fieldsOnchange2
        },
        computePopulationScaling(){
            
            console.log(this.factorA)
            console.log(this.factorB)

            if(this.factorA === null || this.factorB === null){

                this.errorMessage = "Enter Not Null Factors"
                this.scalingFactorMales   = "Not calculated"
                this.scalingFactorFemales = "Not calculated"
                this.scalingFactorAll     = "Not calculated"                

            }
            else
                {            
                    // check inputs are numbers in valid range
                    console.log(isNaN(this.factorA))
                    console.log(isNaN(this.factorB))

                    var badCondition0 = isNaN(this.factorA) || isNaN(this.factorB)
                    var badCondition1 = this.factorA < 2 || this.factorA > 10000
                    var badCondition2 = this.factorB < 2 || this.factorB > 10000
                    
                    if( badCondition0 || badCondition1 || badCondition2){
                        this.errorMessage = "Enter Numerical float Factors in range [2-10000]"
                        this.scalingFactorMales   = "Not calculated"
                        this.scalingFactorFemales = "Not calculated"
                        this.scalingFactorAll     = "Not calculated"                                                                              
                    }                
                    else {

                        var numMales   = totalPopulation["Males"]
                        var numFemales = totalPopulation["Females"]
                        var numAll     = totalPopulation["All"]

                        var denominator = (this.factorB - 1) * (this.factorB - 1)
                        
                        this.scalingFactorMales   = (this.factorA * numMales   ) / denominator
                        this.scalingFactorFemales = (this.factorA * numFemales ) / denominator
                        this.scalingFactorAll     = (this.factorA * numAll     ) / denominator 
                        this.errorMessage = "Entered Factors were OK -> Scaling Factors Calculated OK"
                    }
                }            
        }                
    },
    data() {
        return {
            sortBy: 'Planet',
            fields1: [
                { key: 'Planet',  sortable: true }]
            ,
            fields2 : [{ key: 'Set of Planets'}],
            typeOfPeopleSelected: "",
            planets: [],
            malesOnPlanets:{},
            femalesOnPlanets:{},
            itemsTable: populationOnPlanets,
            itemsTable2: rowForTotalPopulation,
            totalPop: totalPopulation,
            selectedSexes: [],
            factorA: null,
            factorB: null,
            scalingFactorMales: null,
            scalingFactorFemales: null,
            scalingFactorAll: null,
            errorMessage: "----"
        }
    },
    computed: {
        malesTotal() {
            //totalPopulation["Males"]   = jsonData.filter(item => item.Males === 1).length
            //totalPopulation["Females"] = jsonData.filter(item => item.Males === 0).length
            //totalPopulation["Total"]   = totalPopulation["Males"] + totalPopulation["Females"]
            
            return jsonData.filter(item => item.Males === 1).length
        },
        femalesTotal() {
            return jsonData.filter(item => item.Males === 0).length
        },
        allTotal() {
            return 100;
        }
        
        
    }
    
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
