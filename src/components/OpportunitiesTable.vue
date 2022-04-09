<template>
  <div class="tableContainer">

      <!-- custom header, add filter tag underneath -->
    <b-table hover small fixed :items="dispo" :fields="headers" >
        <template #table-colgroup="scope">
            <col
            v-for="field in scope.fields"
            :key="field.key"
            :style="{ width: field.key === 'foo' ? '180px' : '180px' }"
            >
        </template>

        <template slot="top-row" slot-scope="{ fields }">
            <td v-for="field in fields" :key="field.key">
                
                <div class="inputsContainer">
                    <!-- p sure the set focus is the thing slowing this site down -->
                    <div>
                        <!-- <input class="textInputs" v-model="filterHolder[field.key]" :placeholder="'***************'" 
                            v-on:keyup.enter="addTag(field.key)"> -->
                        <vSelect class="textInputs" :options="uniqueTerms[field.key]" style="border: none"
                            v-model="filterHolder[field.key]"
                            :value="filterHolder[field.key]"
                            @input="addTag(field.key)"
                            placeholder="**************"
                            >
                        </vSelect>
                        <!-- <Dropdown class="textInputs" 
                            :options="uniqueTerms[field.key]"
                            v-on:selected="validateSelection"
                            :disabled="false"
                            name=""
                            :maxItem="100"
                            placeholder="******">
                        </Dropdown> -->

                        <!-- 
                            
                            v-on:filter="getDropdownValues" -->
                        <!-- <button class="addButton" @click="columnToFocus[field.key] = !columnToFocus[field.key]">v</button> -->
                    </div>
                </div>
<!-- @blur="setFocusFalse(field.key)" @focus="setFocusTrue(field.key)" -->

<!-- :isFocused=columnToFocus[field.key] -->
                <!-- <div v-if="columnToFocus[field.key] == false"> -->
                    <!-- <SearchResults  :typeText="filterHolder[field.key]" :allColTerms="uniqueTerms[field.key]" /> -->
                    <!-- <p> I AM FOCUSED </p>
                </div> --> 

                <div class="tagsContainer">
                    <!-- <button v-if="filterDict[field.key] != undefined">{{ filterDict[field.key] }}</button> -->
                    <div class="tag" v-for="tag in filterDict[field.key]" :key="tag" >
                        <button class="removeTagButton" v-on:click="removeTag(field.key, tag)">x</button>
                        <ColumnTag :tagName=tag />
                    </div>
                </div>
            </td>
        </template>
    </b-table>

  </div>
</template>

<script>
//import * as csvdata from '../data.json';
import json from '../data.json'
import ColumnTag from './ColumnTag.vue'
// import SearchResults from './SearchResults.vue'
// import Dropdown from 'vue-simple-search-dropdown';
import vSelect from 'vue-select';
import 'vue-select/dist/vue-select.css';




export default{
    data(){
        return{
            myJson: json,
            dispo: json,
            headers: [],
            filterDict: [],
            filterHolder: [],
            columnToFocus: {},
            uniqueTerms: [],
            value:"",
            selected: { name: null, id: null },

            // list of column headers
            // column head/column filter variables dictionary
        }
    },
    components: {
        ColumnTag,
        // Dropdown,
        vSelect,
        // SearchResults
    },
    methods: {
        filterer(){
            console.log("filter called");
            const filtered = [];

            // For rows of table
                    // For possible filters:
                        // if the tag array exists:
                            // If item value is in tag array,
                                // Pass!
                            // Else: fail! + skip this value

            // Iterate through all of the rows.
            for (var j = 0; j < this.myJson.length; j++){

                var passesFilters = true;

                // Iterate through the headers
                for (var k = 0; k < this.headers.length; k++){

                    var colName = this.headers[k];
                    var myItem = this.myJson[j][colName];

                    var tagArray = this.filterDict[colName];

                    // If anyone's ever reading this code; Don't judge me

                    if (tagArray != undefined){
                        tagArray = tagArray.map(element => {
                        return element.toLowerCase();
                        });

                        if (myItem != undefined){
                            var isIn = tagArray.find(element => { if (myItem.toLowerCase().includes(element)) { return true; } });
                        }
                    }
                    
                    if (tagArray == undefined || myItem == undefined){
                        // console.log("missing value!");
                    } else if (tagArray.length == 0){
                        // console.log("empty tag array")
                    // } else if (tagArray.includes(myItem.toLowerCase())){
                    } else if (isIn){
                        // console.log("Found!");
                    } else {
                        // console.log("not found!");
                        passesFilters = false;
                    }

                    

                }

                if (passesFilters && !filtered.includes(this.myJson[j])){
                    filtered.push(this.myJson[j]);
                }


            }

            this.dispo = filtered;
            this.$forceUpdate();

        },
        addTag(key){

            // console.log("ADD TAG: ");
            console.log("tag: "+key);
            
            if (this.filterDict[key] == undefined){
                this.filterDict[key] = [];
            }

            var myWord = this.filterHolder[key];

            this.filterDict[key].push(myWord);
            this.filterHolder[key] = "";

            console.log(this.filterDict);
            this.filterer();
        },
        removeTag(fieldKey, tag){
            // console.log(fieldKey);
            // console.log(tag);

            var myFilterArr = this.filterDict[fieldKey];

            for (var m = 0; m < myFilterArr.length; m++){
                if (myFilterArr[m] == tag){
                    // console.log("found!");
                    // console.log(m);
                    myFilterArr.splice(m,1);

                    // console.log(myFilterArr);
                }
            }
            console.log(this.filterDict);
            this.filterer();
        },
        validateSelection(selected){
            this.addTag(selected);
        },
        setFocusTrue(key){
            // var newObj = this.columnToFocus;

            // Object.keys(newObj).forEach(key => {
            //     newObj[key] = false;
            // });
            // newObj[key] = true;

            // this.columnToFocus = newObj;
        
            this.$set(this.columnToFocus, key, true);
            console.log(this.columnToFocus[key])
        },
        setFocusFalse(key){
            this.columnToFocus[key] = false;
            console.log(this.columnToFocus[key])
        }
    },
    async mounted () {
        // This sets up the dictionary for the filters,
        // but also the focus tags
        // and now also the uniqueterms
        var uniqueDict = {};

        this.headers = Object.keys(json[0])
        for(var i = 0; i < this.headers.length; i++){
            var thisHead = this.headers[i];
            this.filterHolder.push({[thisHead] : ''});

            // piggybacking on this loop to set up the columnToFocus array
            this.columnToFocus[thisHead] = false;

            // also piggybacking on this loop to do unique terms
            // var oneSetOfTerms = {[thisHead]:[...new Set(json.map(item => item[thisHead]))]};
            // console.log(uniqueVals);
            // console.log(oneSetOfTerms)
            uniqueDict[thisHead] = [...new Set(json.map(item => item[thisHead]))].filter(n => n);

            
            // var uniqueVals = [...new Set(json.map(item => item[thisHead]))].filter(n => n)
            // uniqueDict[thisHead] = {};
            // var count = 0;
            // var myObj = {};
            // var myArr = [];
            // uniqueVals.forEach(element => {
            //     myObj = {"id":count,"name":element};
            //     myArr.push(myObj);
            //     // uniqueDict[thisHead][count] = element;
            //     count++;
            // });
            // uniqueDict[thisHead] = myArr;

            // console.log(myArr);
            // console.log(uniqueDict[thisHead])

            
        }

        this.uniqueTerms = uniqueDict;
        
        // console.log(this.columnFocus)
        // this.populateKeys(this.myJson);
    },
    computed: {
        tags(header) {
            const tags = this.filterDict[header];
            return tags;
        },
        myColor(){
        return "rgb(" + Math.floor(Math.random() * 255)
                + "," + Math.floor(Math.random() * 255) + ","
                + Math.floor(Math.random() * 255) + ")";
        }

        // filtered () {
        //     const filtered = this.myJson.filter(item => {
        //         return Object.keys(this.filterDict).every(key =>
        //             String(item[key]).includes(this.filterDict[key]))
        //     })
        //     return filtered.length > 0 ? filtered : [{
        //         id: '',
        //         issuedBy: '',
        //         issuedTo: ''
        //     }]
        // }
    }
}

</script>

<style>

b-table{
    padding-left: 5%;
    padding-right: 5%;
    border: 1px solid red;
}

td{
    overflow-wrap: break-word;
}

input{
    width: 80%;
}

.tableContainer{
    margin-left: 5%;
    text-align: center;
}

.textInputs{
    width: 100%;
    border: 2px solid black;

    font-weight: bold;
}

.inputsContainer{
    display: block;
    justify-content: left;
    width: 100%;
}

.tagsContainer{
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    margin-top: 5px;
}

.tag{
    display: flex;
    margin: 2%;
    margin-right: 7px;
    /* padding-left: 10%; */
    /* padding-right: 10%; */
    
    /* border: 1px solid black; */

    /* background-color: lightgrey; */
    /* border: 1px solid black; */

   /* background-color: expression("rgb(" + Math.floor(Math.random() * 255)
      + "," + Math.floor(Math.random() * 255) + ","
      + Math.floor(Math.random() * 255) + ")"); */

}

.removeTagButton{
    background-color: lightgrey;
    border: 2px solid grey;
    border-radius: 10px 0px 0px 10px;

    /* color: black;
    border: none;
    background-color: lightblue;

    margin-left: 5px; */
}

.addButton{
    border: 2px solid black;
    border-left: none;
}

/* .tableContainer{
    width: 100%;
    margin: 10px;
    padding: 0;
} */

/* Header width and column width have to be perfectly calibrated. 
Ideally they are calibrated the same way. */


</style>