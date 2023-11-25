<script>

import axios from 'axios' 

export default {
    name: 'UserListView',
    data() {
        return {
            users: []
        }
    },
    methods: {
        getUsers() {
            axios.get('http://localhost:8080/v1/users?detailed=true').then(res => {
                axios.get('http://localhost:8080/v1/roles').then(rolesResponse => {
                    const roles = rolesResponse.data
                    this.users = res.data.map(user => {
                        user.roles = user.roles.map(roleId => {
                            const r = roles.find(i => roleId == i.id).name
                            return r
                        })                        
                        return user
                    })           
                    console.log('CALLED')         
                }).catch(function (error) {
                    console.log('E1', error)
                })                
            }).catch(function (error) {
                console.log('E2', error)
                //ToDo
            })
        },
        deleteUserById(id) {
            axios.deleteUserById(`http://localhost:8080/v1/users/${id}`).then(res => {
                console.log(res)
            }).catch(function (error) {

            })
        }
    },
    mounted() {        
        this.getUsers()
    }
}
</script>
<template>
  <div class="users">
    <div class="card">
        <div class="card-header">
            <h4>
                Users
                <RouterLink to="/users/create" class="btn btn-primary float-end">Add User</RouterLink>
            </h4>
        </div>
        <div class="card-body">
            <table class="table table-bordered">
                <thead>
                    <tr>
                        <th>#</th><th>Username</th><th>Email</th><th>First Name</th><th>Last Name</th><th>Age</th><th>Birth Day</th><th>Roles</th><th></th>
                    </tr>
                </thead>
                <tbody v-if="users.length > 0">
                    <tr v-for="(user, index) in this.users" key="index">
                        <td>{{ index+1 }}</td>
                        <td>{{ user.username }}</td>
                        <td>{{ user.email }}</td>
                        <td>{{ (user.userDetail && user.userDetail.firstName) || "N/A" }}</td>
                        <td>{{ (user.userDetail && user.userDetail.lastName) || "N/A" }}</td>
                        <td>{{ (user.userDetail && user.userDetail.age) || "N/A" }}</td>
                        <td>{{ (user.userDetail && user.userDetail.birthDay) || "N/A" }}</td>
                        <td>{{ user.roles.join(', ') }}</td>
                        <td>
                            <RouterLink :to="{ path: `/users/${user.id}/edit` }" class="btn btn-success">Edit</RouterLink>
                            <button type="button" @click="deleteUserById(user.id)" class="btn btn-danger">Delete</button>
                        </td>
                    </tr>
                </tbody>
                <tbody v-else>
                    <tr>
                        <td colspan="9">There are no users.</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
  </div>
</template>
