<script>

import axios from 'axios' 
import Multiselect from '@vueform/multiselect'

export default {
    name: 'UserEditView',
    components: {
      Multiselect,
    },
    data() {
        return {
            model: {
                user: {
                    id: '',
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
            axios.put(`http://localhost:8080/v1/users/${this.$route.params.id}`, this.model.user).then(response => {
                alert('User successfully updated!')
                this.$router.push('/users')
            }).catch(function (error) {
                if (error.code === 'ERR_BAD_REQUEST') {
                    thisView.setErrors(error.response.data.errors)
                }
                console.error('Error', error)
            })
        },
        setErrors(errors) {
            this.errors = errors
        },
        getUserById() {
            axios.get(`http://localhost:8080/v1/users/${this.$route.params.id}`).then(response => {
                this.model.user = response.data
                if (!this.model.user.userDetail) {
                    this.model.user.userDetail = {
                        firstName: '',
                    }
                }
                console.log('USER', this.model.user)
            }).catch(function (error) {
                console.log('Error', error)
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
        this.getUserById()
    }
}
</script>

<style src="@vueform/multiselect/themes/default.css"></style>

<template>
  <div class="users">
    <div class="card">
        <div class="card-header">
            <h4>
                Edit user                
            </h4>
        </div>
        <div class="card-body">
            <div class="mb-3">
                <p v-if="errors.length">
                    <b>Please correct the following error(s):</b>
                    <ul>
                    <li v-for="error in errors">{{ error.field }}: {{ error.defaultMessage }}</li>
                    </ul>
                </p>
            </div>
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
                <input type="date" v-model="model.user.userDetail.birthDay" name="birthDay" id="birthDay" class="form-control">
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
