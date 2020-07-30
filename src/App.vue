<template>
  <div id="app">

        <div class="adding-selecting-box">
        <select class="select-option" v-model="selectedKey">
        <option value="" disabled>Select a Driver</option>
        <option v-for="driver in drivers" :value="driver">{{ driver.name }}</option>
        </select>
        <select class="select-option" v-model="selectedKey.newTask">
        <option value="" disabled>Select a Task</option>
        <option v-for="task in tasks" :value="task">{{ task }}</option>
        </select>
        <input v-model="selectedKey.location" type="text"  placeholder="Type a location">
        <date-picker 
        v-model="selectedKey.time"
        lang="en"
        range 
        type="time"
        value-type="format"
        format="H"
        placeholder="Select time">
        </date-picker>
        <date-picker
        v-model="selectedKey.days"
        lang="en"
        value-type="format"
        :format="momentFormat"
        placeholder="Select day and week">
        </date-picker>
        <button v-on:click="addTask">Add</button>
        </div>
        <div class="main-calendar-box">
        <ol>
        <li 
        v-bind:info="task.info"
        v-bind:timerange="task.timerange"
        v-bind:daysweek="task.daysweek"
        v-bind:location="task.location"
        v-for="(task, index) in selectedKey.drivertasks"
        >
        <span>{{task.info}} from {{task.timerange[0]}} to {{task.timerange[1]}} at {{ task.daysweek}}. Location: {{ task.location }}</span>
        <button v-on:click="deletetask">Delete Task</button>
        <button v-on:click="opencomp">Update Task</button>
        <editdrivers v-if="openComponent" v-bind:task="task"></editdrivers>
        </li>
        </ol>
        </div>

        
  
  </div>
</template>

<script>
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
import * as moment from "moment/moment";
import EditDrivers from './components/EditDrivers';
export default {
  name: 'app',
  components: {

    DatePicker,
    'editdrivers': EditDrivers

    },

  data () {
    return {

      openComponent: false,

      momentFormat: {

      stringify: (date) => {
        return date ? moment(date).format('E [day] WW [week]') : ''
      },

      parse: (value) => {
        return value ? moment(value, 'E [day] WW [week]').toDate() : null
      }
    },

       drivers: [

            {
                name: 'Adam Anderson',
                key: 'Driver1',
                newTask: '',
                time: '',
                days: '',
                location: '',
                drivertasks: []
                
            },
            {
                name: 'John Doe',
                key: 'Driver2',
                newTask: '',
                time: '',
                days: '',
                location: '',
                drivertasks: [
                ]
            },
            {
                name: 'Emily Smith',
                key: 'Driver3',
                newTask: '',
                time: '',
                days: '',
                location: '',
                drivertasks: [
                ]
            }
        ],
        selectedKey: '',
        tasks: [

            'Pickup',
            'Dropoff',
            'Other'

        ],
        date: ''
     
    }
  },
  methods: {

  

    addTask() {
    
    var task = this.selectedKey.newTask;
    var time = this.selectedKey.time;
    var location = this.selectedKey.location;
    var days = this.selectedKey.days;


    var startingPoint = this.selectedKey.time[0];
    var endingPoint = this.selectedKey.time[1];
    var arrayPush = [];
    while(startingPoint <= endingPoint){
     arrayPush.push(startingPoint++);
    }

    var daysonearray = this.selectedKey.drivertasks.map(function (item) {
	  return item.daysweek;
    });

    var timearray = this.selectedKey.drivertasks.map(function (item) {
	  return item.timerange;
    });

    var onearray = [];

    var result = [];

    timearray.forEach((timearray) => {
    onearray.push(timearray);
    });

    daysonearray.forEach(function (k, i) {

    result[k] = onearray[i]
    });

    console.log(result[onearray]);

    if(!daysonearray.includes(this.selectedKey.days) && !timearray.includes(arrayPush)){

    this.selectedKey.drivertasks.push({
        timerange: time,
        daysweek: days,
        info: task,
        location: location
      })


    } else if(daysonearray.includes(this.selectedKey.days)) {
      
      if(confirm('At this time, driver has a task! Do you want to overwrite it?')){
      
      var index = daysonearray.indexOf(this.selectedKey.days);

      this.selectedKey.drivertasks.splice(index, 1);

      this.selectedKey.drivertasks.push({
        timerange: time,
        daysweek: days,
        info: task,
        location: location
      })
  

    } else {
        console.log('I did not make a changes!');
    }
  } else {
    return false;
  };

  },
  deletetask(index) {
    
    this.selectedKey.drivertasks.splice(index, 1)

  },

  opencomp() {

    this.openComponent = !this.openComponent
  }

}
}

</script>
<style>

#app {

  display: flex;
  justify-content: space-between;
  flex-direction: column;
  margin-top: 150px;
}

ol li {

  font-family: 'Cormorant Garamond', serif;
  font-size: 20px;
  padding-bottom: 10px;
  
}

ol li span{

  padding-right: 40px;
  
}

.adding-selecting-box {
  width: 1300px;
  display: flex;
  justify-content: space-between;
  margin: 0 auto;

}

.main-calendar-box {

  width: 1330px;
  margin: 40px auto 0 auto;

}

.select-option {

    padding: 8px;
    color: grey;
    background: transparent;
    border: 1px solid grey;
    font-family: 'Cormorant Garamond', serif;
    font-size: 15px;
    border-radius: 5px;
    cursor: pointer;
    appearance: none;
    -webkit-appearance: none;
    -moz-appearance: none;
    background-image: url('data:image/svg+xml;utf8, <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%grey" width="18px" height="18px"><path d="M0 0h24v24H0z" fill="none"/><path d="M16.59 8.59L12 13.17 7.41 8.59 6 10l6 6 6-6z"/></svg>');
    background-position: right 10px top 50%;
    background-repeat: no-repeat;
    background-size: 30px;
    padding-right: 60px;
}

.select-option:focus,
.select-option:hover {

    outline: none;
}

input {

  background: transparent;
  border: 1px solid grey;
  font-family: 'Cormorant Garamond', serif;
  font-size: 15px;
  cursor: pointer;
  color: grey;
  height: 16px;
  padding: 8px;
  border-radius: 5px;
  width: 200px;
 
}

button {

  background-color: grey;
  border: none;
  color: white;
  padding: 2px 15px;
  font-family: 'Cormorant Garamond', serif;
  font-size: 21px;
  text-transform:uppercase;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
}

</style>

