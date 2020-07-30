
<template>
<div class="main-update-component">
<div class="update-coomponent">
<input class="input-p-one" v-model="task.info">
<date-picker class="picker" 
v-model="task.timerange" 
lang="en"
range 
type="time"
value-type="format"
format="H"
placeholder="Select time""
></date-picker>
<date-picker class="picker" v-model="task.daysweek"
lang="en"
value-type="format"
:format="momentFormat"
placeholder="Select day and week"
></date-picker>
<input class="input-p" v-model="task.location">
<button v-on:click="savechanges">Save</button>
</div>
</div>
</template>

<script>
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
import * as moment from "moment/moment";

export default {

  components: { DatePicker },

  props: ['task', 'selectedKey'],


  
  data () {
    return {

      openedComp: true,
      momentFormat: {

      stringify: (date) => {
        return date ? moment(date).format('E [day] WW [week]') : ''
      },

      parse: (value) => {
        return value ? moment(value, 'E [day] WW [week]').toDate() : null
      }
    },

  }

},
methods: {

    savechanges() {

        var daysonearray = selectedKey.drivertasks.map(function (item) {
	    return item.daysweek;
        });

        if(daysonearray.includes(this.selectedKey.days)) {

           if(confirm('At this time, driver has a task! Do you want to delete it?')){
      
           var index = daysonearray.indexOf(this.selectedKey.days);

           this.selectedKey.drivertasks.splice(index, 1);

      } else {
        console.log('I did not make a changes!');
      }
        }
    }
}

}

</script>
<style>
.update-coomponent {
    border: 1px solid grey;
    height: 300px;
    width: 400px;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    text-align: center;
   
    
}

.update-coomponent .input-p {
    margin: 20px auto 20px auto;
    
}

.update-coomponent .input-p-one {
    margin: 20px auto 0 auto;
    
}

.update-coomponent .picker {
    margin: 20px auto 0 auto;
    
}

.update-coomponent button {
    width: 130px;
    height: 40px;
    border-radius: 5px;
    margin: 0 auto;
    
}

.main-update-component {
    display: flex;
    justify-content: center;
    margin-top: 30px;
}
</style>