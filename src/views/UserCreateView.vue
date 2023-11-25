<script>

import axios from 'axios' 
import Multiselect from '@vueform/multiselect'

export default {
    name: 'UserCreateView',
    components: {
      Multiselect,
    },
    data() {
        return {
            model: {
                user: {
                    username: '',
                    password: '',
                    email: '',
                    userDetail: {
                        firstName: '',
                        lastName: '',
                        age: '',
                        birthDay: ''
                    },
                    roles: []
                }
            },
            roles: []
        }
    },
    methods: {
        saveUser() {
            axios.post('http://localhost:8080/v1/users', this.model.user).then(response => {
                alert('User successfully create!')
                this.model.user = {
                    username: '',
                    email: '',
                    userDetail: {
                        firstName: '',
                        lastName: '',
                        age: '',
                        birthDay: ''
                    },
                    roles: []
                }
            }).catch(function (error) {
                console.error('Error', error)
            })
        },
        getRoles() {
            axios.get('http://localhost:8080/v1/roles').then(response => {
                this.roles = response.data.map(role => {
                    return {
                        value: role.id,
                        label: role.name
                    }
                })                          
            }).catch(function (error) {
                console.log('E1', error)
            })                
        }
    },
    mounted() {        
        this.getRoles()
    }
}
</script>

<style src="@vueform/multiselect/themes/default.css"></style>

<template>
  <div class="users">
    <div class="card">
        <div class="card-header">
            <h4>
                Add user                
            </h4>
        </div>
        <div class="card-body">
            <div class="mb-3">
                <label for="username" class="form-label">Username</label>
                <input type="text" v-model="model.user.username" name="username" id="username" class="form-control">
            </div>
            <div class="mb-3">
                <label for="password" class="form-label">Password</label>
                <input type="password" v-model="model.user.password" name="username" id="password" class="form-control">
            </div>
            <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input type="email" v-model="model.user.email" name="email" id="email" class="form-control">
            </div>
            <div class="mb-3">
                <label for="firstName" class="form-label">First Name</label>
                <input type="text" v-model="model.user.userDetail.firstName" name="firstName" id="firstName" class="form-control">
            </div>
            <div class="mb-3">
                <label for="lastName" class="form-label">Last Name</label>
                <input type="text" v-model="model.user.userDetail.lastName" name="lastName" id="lastName" class="form-control">
            </div>
            <div class="mb-3">
                <label for="age" class="form-label">Age</label>
                <input type="text" v-model="model.user.userDetail.age" name="age" id="age" class="form-control">
            </div>
            <div class="mb-3">
                <label for="birthDay" class="form-label">Birth Day</label>
                <input type="text" v-model="model.user.userDetail.birthDay" name="birthDay" id="birthDay" class="form-control">
            </div>
            <div class="mb-3">
                <label class="form-label">Roles</label>
                <Multiselect
                    mode="multiple"
                    v-model="model.user.roles"
                    :options="roles"
                />
            </div>           
            <div class="mb-3">
                <button type="button" @click="saveUser" class="btn btn-primary">Save</button>
                <RouterLink to="/users" class="btn btn-secondary">Back</RouterLink>
            </div>            
        </div>
    </div>
  </div>
</template>
