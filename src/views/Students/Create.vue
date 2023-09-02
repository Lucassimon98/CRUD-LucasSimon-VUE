<template>
    
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Add Students</h4>
            </div>
            <div class="card-body">

                <ul class="alert alert-warning" v-if="Object.keys(this.errorList).length > 0">
                    <li class="mb-0 ms-3" v-for="(error, index) in this.errorList" :key="index">
                        {{ error[0] }}
                    </li>
                </ul>

                <div class="mb-3">
                    <label for="">Name</label>
                    <input type="text" v-model="model.student.name" class="form-control">
                </div>
                <div class="mb-3">
                    <label for="">Course</label>
                    <input type="text" v-model="model.student.course" class="form-control">
                </div>
                <div class="mb-3">
                    <label for="">Email</label>
                    <input type="text" v-model="model.student.email" class="form-control">
                </div>
                <div class="mb-3">
                    <label for="">Phone</label>
                    <input type="text" v-model="model.student.phone" class="form-control">
                </div>
                <div class="mb-3">
                    <button type="button" @click="saveStudent" class="btn btn-primary">Save</button>
                </div>
            </div>
        </div>
    </div>    
  
</template>

<script>
import axios from 'axios'
export default {
    name: 'studentCreate',
    data() {
        return {
            errorList: '',
            model: {
                student: {
                    name: '',
                    email: '',
                    course: '',
                    phone: ''
                }
            }
        }
    },
    methods: {


        saveStudent() {
            var $this = this;
            axios.post('http://127.0.0.1:8000/api/students', this.model.student).then(res => {
                console.log(res)

                this.model.student = {
                    name: '',
                    email: '',
                    course: '',
                    phone: ''
                }
                this.errorList = '';
            })
            .catch(function (error) {
                if (error.response) {
                    if(error. response.status == 422) {
                        $this.errorList = error.response.data.errors
                    }
                console.error(error.response.data);
                // console.error(error.response.status);
                // console.error(error.response.headers);
                } else if (error.request) {
                    console.error(error.request);
                } else {
                    console.error('Error', error.message);
                }
            })
        }
    }
}
</script>