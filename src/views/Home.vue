<template>
  <div class="home">
<!--    <h3>This is home page</h3>-->

    <div class="body-content m-5">
      <div class="d-flex justify-content-between">
        <div>
          <h1>Choose your flights <span class="lightgray">From From Place to To Place</span></h1>
        </div>
        <div class="d-flex">
          <div>
            <p class="lightgray p-2">Sort by:</p>
          </div>
          <div>
            <form action="">
              <select name="" id="" class="form-control" @change="priceOrder($event)">
                <option value="asc">Low to High</option>
                <option value="desc">High to Low</option>
              </select>
            </form>
          </div>
        </div>
      </div>
      <div class="departure-and-arrival d-flex">
        <div class="px-3">
          <i class="fas fa-plane-departure text-success"></i>
          <span class="lightgray">18.01.2020, Moscow</span>
        </div>
        <div class="px-3">
          <i class="fas fa-plane-arrival text-danger"></i>
          <span class="lightgray">18.01.2020, Moscow</span>
        </div>
      </div>
      <div class="container-fluid">
        <div class="row ">
          <div class="col-md-3 bg-white shadow p-3 sidenav m-3 rounded">
            <div class="container-fluid">
              <div class="menu-head d-flex justify-content-between">
                <div class="menu-head-title">
                  <h4>Filters</h4>
                </div>
                <div class="menu-head-title">
                  <a href="javascript:void(0)" v-on:click="menuToggle = ! menuToggle">Minimize <i class="fas fa-stream"></i></a>
                </div>
              </div>
              <div class="menu"  >
                <div class="accordion">
                  <div>
                    <div >
                      <h4 class="my-3">
                        <a class="text-left" data-toggle="collapse" data-target="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
                          <span class="lightgray">
                            Class
                          </span>
                          <span class="w-100" v-on:click="menuToggle = ! menuToggle"><icon class="fa fa-angle-down float-right"></icon></span>
                        </a>
                      </h4>
                    </div>

                    <div  class="collapse show" aria-labelledby="headingOne" data-parent="#accordionExample" v-if="menuToggle">
                      <div>
                        <ul class="list-group">
                          <li class="list-group-item border-none" v-for="(filter_class_item , filter_class_index) in class_list" :key="filter_class_index">
                            <div class="form-group form-check">
                              <input type="checkbox" class="form-check-input"  :value="filter_class_item.id" :id="filter_class_index" @click="getCheckValue(filter_class_item.id)" >
                              <label class="form-check-label" :for="filter_class_index">{{filter_class_item.name}}</label>
                            </div>
                          </li>
                        </ul>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="col-md-8 p-3 sidenav m-3 rounded">
            <div class="container-fluid">


              <!-- loop this -->
              <div class="row" v-for="(ticket_item , ticket_index) in ticket_list" :key="ticket_index">
                <div class="col-md-9">
                  <div class="rounded shadow py-5 px-2 d-flex align-items-center justify-content-between card">
                    <div class="d-flex align-items-center">
                      <div class="plane-status px-1 text-center">
                        <i class="fas fa-plane-departure text-success"></i>
                        <p class="lightgray">{{ticket_item.journey_date}}</p>
                      </div>
                      <div class="plane-type d-flex px-1">
                        <div class="px-3">
                          <div v-for="(plane_item , plane_index) in plane_list" :key="'journey_plane'+plane_index">
                            <h3 v-if="plane_item.id == ticket_item.journey_plane_id">
                              <div class="float-left">
                                <img :src="'http://127.0.0.1:8000/plane_image/'+plane_item.image" alt="" height="50px" width="50px">
                              </div>
                              <h3>{{plane_item.name}}</h3>
                            </h3>
                          </div>

                          <span v-for="(class_item , class_index) in class_list" :key="'journey_class'+class_index">
                            <span class="lightgray" v-if="class_item.id== ticket_item.class_id">{{class_item.name}}</span>
                          </span>
                          <small class="text-danger"> Last 5 min</small>
                        </div>
                      </div>
                      <div class="time">
                        <h3>{{ticket_item.journey_from_time}}</h3>
                        <h5 class="lightgray">MOS</h5>
                      </div>
                      <div class="range px-2">
                        <p class="lightgray text-center">
                          {{ticket_item.journey_to_time.split(":")[0] - ticket_item.journey_from_time.split(":")[0] +'H'}}

<!--                          {{ // ticket_item.journey_to_time.split(":")[1] - ticket_item.journey_from_time.split(":")[1] +'M'}}-->

                          {{(ticket_item.journey_to_time.split(":")[1] >ticket_item.journey_from_time.split(":")[1]) ?
                          ticket_item.journey_to_time.split(":")[1] - ticket_item.journey_from_time.split(":")[1] +'M' :
                          ticket_item.journey_from_time.split(":")[1]  - ticket_item.journey_to_time.split(":")[1]   +'M'}}

                        </p>
                        <div class="d-flex align-items-center">
                          <div style="height:10px; width:10px; background-color: black; border-radius: 50%;"></div>
                          <div style="height:2px; width:100px; background-color: black;"></div>
                          <div style="height:10px; width:10px; background-color: black;border-radius: 50%;"></div>
                        </div>
                        <p class="lightgray text-center">Direct Flight</p>
                      </div>
                      <div class="time">
                        <h3>{{ticket_item.journey_to_time}}</h3>
                        <h5 class="lightgray">AYT</h5>
                      </div>
                    </div>
                    <hr>
                    <div class="d-flex align-items-center">
                      <div class="plane-status px-1 text-center">
                        <i class="fas fa-plane-arrival text-danger"></i>
                        <p class="lightgray">{{ticket_item.return_date}}</p>
                      </div>
                      <div class="plane-type d-flex px-1">

                        <div class="px-3">
                          <div v-for="(plane_item2 , plane_index2) in plane_list" :key="'return_plane'+plane_index2">

                            <div v-if="plane_item2.id == ticket_item.return_plane_id">
                              <div class="float-left">
                                <img :src="'http://127.0.0.1:8000/plane_image/'+plane_item2.image" alt="" height="50px" width="50px">
                              </div>
                              <h3>{{plane_item2.name}}</h3>
                            </div>
                          </div>

                          <span v-for="(class_item2 , class_index2) in class_list" :key="'return_class'+class_index2">
                            <span class="lightgray" v-if="class_item2.id== ticket_item.class_id">{{class_item2.name}}</span>
                          </span>
                          <small class="text-danger"> Last 5 min</small>
                        </div>
                      </div>
                      <div class="time">
                        <h3>{{ticket_item.return_from_time}}</h3>
                        <h5 class="lightgray">AYT</h5>
                      </div>
                      <div class="range px-2">
                        <p class="lightgray text-center">3h 15m</p>
                        <div class="d-flex align-items-center">
                          <div style="height:10px; width:10px; background-color: black; border-radius: 50%;"></div>
                          <div style="height:2px; width:100px; background-color: black;"></div>
                          <div style="height:10px; width:10px; background-color: black;border-radius: 50%;"></div>
                        </div>
                        <p class="lightgray text-center">Direct Flight</p>
                      </div>
                      <div class="time">
                        <h3>{{ticket_item.return_to_time}}</h3>
                        <h5 class="lightgray">MOS</h5>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="col-md-3">
                  <div class="rounded shadow py-5 px-2 d-flex align-items-center justify-content-between card">
                    <h2>{{ticket_item.total_price}}$</h2>
                    <p class="lightgray">Flight Price</p>
                    <button class="btn btn-primary btn-lg btn-block my-2">SELECT</button>
                    <div class="btn-group">
                      <button type="button" class="btn-outline-primary btn-lg btn-block my-2 dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                        Show Detail
                      </button>
                      <div class="dropdown-menu">
                        <a class="dropdown-item" href="#">Action</a>
                        <a class="dropdown-item" href="#">Another action</a>
                        <a class="dropdown-item" href="#">Something else here</a>
                        <div class="dropdown-divider"></div>
                        <a class="dropdown-item" href="#">Separated link</a>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <!-- loop this -->


            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
// @ is an alias to /src

import axios from 'axios';
export default {
  name: 'Home',
  data(){
    return{
      menuToggle :true,
      ticket_list:[],
      class_list:[],
      plane_list:[],
      checkClass:[]
    }
  },
  methods:{


    async priceOrder(event){
      let self = this;
      console.log(event.target.value);
      // let selector = ;
      await axios.post('http://127.0.0.1:8000/api/ticket/ordering',{
        ordering: event.target.value,
      }).then(response=>{
        self.ticket_list = response.data.result
        console.log(response)
      })
    },

    async getCheckValue(id) {
      let self = this;
      // console.log(this.checkClass);
      if(this.checkClass.indexOf(id) !== -1){
        const index = this.checkClass.indexOf(id);
        if (index > -1) {
          this.checkClass.splice(index, 1);
        }

      } else{
        this.checkClass.push(id);
        // document.write("Value does not exists!")
      }


      // console.log(this.checkClass)
      await axios.post('http://127.0.0.1:8000/api/filter/class', {
        id_list: this.checkClass,
      }).then(response => {
        self.ticket_list = response.data.result
        // this.result = response.data.result
        console.log(response)
      })

    },

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

    getTicket(){
      let self = this;
      axios.get('http://127.0.0.1:8000/api/all-tickets')
              .then(function (response) {
                self.ticket_list = response.data.result
                console.log(self.ticket_list);
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
    }

  },
  mounted() {
    this.getClass()
    this.getTicket()
    this.getPlane()
  }
}
</script>
<style>

</style>
