<template>
  <div class="about">
    <div class="container">
      <form @submit.prevent="doWork">
        <div class="form-group">
          <label for="exampleFormControlSelect1">Class</label>
          <select class="form-control"  id="exampleFormControlSelect1" v-model="class_id">
            <option v-for="(class_item2 , class_index2) in class_list" :key="'return_class'+class_index2" :value="class_item2.id" >{{class_item2.name}}</option>
          </select>
        </div>

        <div class="row">
          <div class="col-md-6">
            <h3>Journey Details</h3>
            <div class="form-group">
              <label for="exampleFormControlSelect1">Journey Plane</label>
              <select class="form-control"  v-model="journey_plane_id" >
                <option v-for="(plane_item2 , plane_index2) in plane_list" :key="'return_plane'+plane_index2" :value="plane_item2.id">{{plane_item2.name}}</option>
              </select>
            </div>

            <div class="form-group">
              <label for="journey_date">Journey Date</label>
              <VueDatePicker  class="form-control" id="journey_date"
                              v-model="journey_date"
                              format="YYYY-MM-DD"
                              required />

            </div>

            <div class="form-group">
              <label for="journey_from_time"> from time </label>
              <input type="text" class="form-control" id="journey_from_time"  v-model="journey_from_time" aria-describedby="emailHelp" placeholder="Time 18:00">
              <small>Time format 18:00 (24 hour)</small>
            </div>

            <div class="form-group">
              <label for="journey_to_time"> to time </label>
              <input type="text" class="form-control" id="journey_to_time" v-model="journey_to_time" aria-describedby="emailHelp" placeholder="Time 18:00">
              <small>Time format 18:00 (24 hour)</small>
            </div>

            <div class="form-group">
              <label for="journey_price"> Price </label>
              <input type="text" class="form-control" id="journey_price" v-model="journey_price" aria-describedby="emailHelp" placeholder="Cost">
            </div>
          </div>

          <div class="col-md-6">
            <h3>Return Details</h3>
            <div class="form-group">
              <label for="exampleFormControlSelect1">Return Plane</label>
              <select class="form-control" v-model="return_plane_id">
                <option v-for="(plane_item , plane_index) in plane_list" :key="'return_plane'+plane_index" :value="plane_item.id">{{plane_item.name}}</option>
              </select>
            </div>

            <div class="form-group">
              <label for="return_date">Return Date</label>
              <VueDatePicker  class="form-control" id="return_date"
                              v-model="return_date"
                              format="YYYY-MM-DD"
                              required />

            </div>

            <div class="form-group">
              <label for="return_from_time"> from time </label>
              <input type="text" class="form-control" id="return_from_time" v-model="return_from_time" aria-describedby="emailHelp" placeholder="Time 18:00">
              <small>Time format 18:00 (24 hour)</small>
            </div>

            <div class="form-group">
              <label for="return_to_time"> to time </label>
              <input type="text" class="form-control" id="return_to_time" v-model="return_to_time" aria-describedby="emailHelp" placeholder="Time 18:00">
              <small>Time format 18:00 (24 hour)</small>
            </div>

            <div class="form-group">
              <label for="return_price"> Price </label>
              <input type="text" class="form-control" id="return_price" v-model="return_price" aria-describedby="emailHelp" placeholder="Cost">
            </div>
          </div>
        </div>



        <button type="submit" class="btn btn-primary">Submit</button>
      </form>
    </div>

  </div>
</template>

<script>
  import { VueDatePicker } from '@mathieustan/vue-datepicker';
  import '@mathieustan/vue-datepicker/dist/vue-datepicker.min.css';
  import axios from "axios";
  export default {
    name: "Activity",
    components : {
      VueDatePicker,
    },
    data(){
      return{
        plane_list:[],
        class_list:[],
        date: new Date(),
        journey_date:this.date,
        return_date:this.date,
        class_id:'',
        journey_plane_id:'',
        return_plane_id:'',

        journey_from_time:'',
        journey_to_time:'',
        return_from_time:'',
        return_to_time:'',
        return_price:'',
        journey_price:''
      }
    },
    methods:{
      getClass(){
        let self = this;
        axios.get('http://127.0.0.1:8000/api/all-class')
                .then(function (response) {
                  self.class_list = response.data.result
                  console.log(self.class_list);
                })
                .catch(function (error) {
                  console.log(error);
                })
                .then(function () {
                  // always executed
                });
      },
      getPlane(){
        let self = this;
        axios.get('http://127.0.0.1:8000/api/all-planes')
                .then(function (response) {
                  self.plane_list = response.data.result
                  console.log(self.plane_list);
                })
                .catch(function (error) {
                  console.log(error);
                })
                .then(function () {
                  // always executed
                });
      },

      doWork(){
        axios.post('http://127.0.0.1:8000/api/ticket/store',{

          journey_date:this.journey_date,
          return_date:this.return_date,
          class_id:this.class_id,
          journey_plane_id:this.journey_plane_id,
          return_plane_id:this.return_plane_id,

          journey_from_time:this.journey_from_time,
          journey_to_time:this.journey_to_time,

          return_from_time:this.return_from_time,
          return_to_time:this.return_to_time,

          return_price:this.return_price,
          journey_price:this.journey_price
        }).then(response=>{

          // this.$toast.success({
          //   title:'Success!',
          //   message:'Operation successful'
          // })
          this.journey_date='',
          this.return_date='',
          this.class_id='',
          this.journey_plane_id='',
          this.return_plane_id='',

          this.journey_from_time='',
          this.journey_to_time='',

          this.return_from_time='',
          this.return_to_time='',

          this.return_price='',
          this.journey_pric='',
          console.log(response)
        })
      }

    },
    mounted() {
      this.getPlane()
      this.getClass()
    }

  }
</script>
