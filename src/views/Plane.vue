<template>
    <div class="about">
        <div class="container">
            <form @submit.prevent="doWork">
                <div class="row">
                    <div class="col-md-6">
                        <h3>Journey Details</h3>


                        <div class="form-group">
                            <label for="journey_from_time"> Plane Name </label>
                            <input type="text" class="form-control" id="journey_from_time"  v-model="plane_name" aria-describedby="emailHelp" placeholder="Time 18:00">
                        </div>
                        <img :src="previewImage" class="uploading-image" alt=" " style="width: 100px" />
                        <div class="form-group">

                            <label for="journey_to_time"> Plane Image </label>

                            <input type="file" class="form-control" id="journey_to_time" @change="uploadImage" aria-describedby="emailHelp" placeholder="Time 18:00">
                        </div>
                    </div>
                </div>
                <button type="submit" class="btn btn-primary">Submit</button>
            </form>
        </div>

    </div>
</template>

<script>
    import axios from "axios";

    export default {
        name: "Plane",
        data(){
            return{
                plane_name:'',
                plane_image:'',
                previewImage:'https://image.shutterstock.com/image-vector/image-icon-260nw-445357855.jpg',
            }
        },
        methods:{
            uploadImage(e){
                const image = e.target.files[0];
                this.plane_image = e.target.files[0];
                const reader = new FileReader();
                reader.readAsDataURL(image);
                reader.onload = e =>{
                    this.previewImage = e.target.result;
                    console.log(this.previewImage);
                };
            },
            doWork(){
                let formData = new FormData();
                formData.append('plane_name' ,this.plane_name);
                formData.append('plane_image' ,this.plane_image);
                console.log(this.plane_image)
                axios.post('http://127.0.0.1:8000/api/plane/store',formData).then(response=>{
                    console.log(response)
                        this.journey_date='',
                        this.return_date=''

                })
            }
        },

    }
</script>

<style scoped>

</style>