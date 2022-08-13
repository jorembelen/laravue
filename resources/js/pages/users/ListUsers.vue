
<script setup>
import axios from 'axios';
import { ref, onMounted, reactive } from 'vue';

    const users = ref([]);

    const form = reactive({
        name: '',
        email: '',
        password: '',
    })

    const getUsers = () => {
        axios.get('/api/users')
        .then((response) => {
            users.value = response.data;
        })
    }

    const createUser = () => {
        axios.post('/api/users', form)
        .then((response) => {
            users.value.unshift(response.data);
            form.name = '';
            form.email = '';
            form.password = '';
            $('#createUserModal').modal('hide');
        });
    }

    onMounted(() => {
        getUsers();
    });

</script>


<template>

    <div class="content-header">
        <div class="container-fluid">
            <div class="row mb-2">
                <div class="col-sm-6">
                    <h1 class="m-0">List Users</h1>
                </div>
                <div class="col-sm-6">
                    <ol class="breadcrumb float-sm-right">
                        <li class="breadcrumb-item"><a href="/">Home</a></li>
                        <li class="breadcrumb-item active">List Users</li>
                    </ol>
                </div>
            </div>
        </div>
    </div>


    <div class="content">
        <div class="container-fluid">
        <button type="button" class="btn btn-primary mb-2" data-toggle="modal" data-target="#createUserModal">
            Add New User
        </button>
          <div class="card">
            <div class="card-body">
                  <table  class="table table-bordered">
                <thead>
                    <tr>
                        <th>SN</th>
                        <th>Name</th>
                        <th>Email</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(user, index) in users" :key="user.id">
                        <td>{{ index + 1  }}</td>
                        <td>{{ user.name }}</td>
                        <td>{{ user.email }}</td>
                    </tr>
                </tbody>
            </table>
            </div>
          </div>

        </div>
    </div>

    <div id="createUserModal" class="modal fade">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h4 class="modal-title" id="myModalLabel">Update {{ $name }} ?</h4>
                    <button type="button" class="close" data-dismiss="modal">×</button>
                </div>
                <div class="modal-body">

                    <div class="form-group row">
                        <label for="create-username" class="col-md-4 ml-3 col-form-label">Name</label>
                        <div class="col-md-11 ml-3">
                            <input type="text" class="form-control mb-2" v-model="form.name" placeholder="Name">
                        </div>
                    </div>
                    <div class="form-group row">
                        <label for="create-username" class="col-md-4 ml-3 col-form-label">Email</label>
                        <div class="col-md-11 ml-3">
                            <input type="email" class="form-control mb-2" v-model="form.email" placeholder="email">
                        </div>
                    </div>
                    <div class="form-group row">
                        <label for="create-username" class="col-md-4 ml-3 col-form-label">Password</label>
                        <div class="col-md-11 ml-3">
                            <input type="password" class="form-control mb-2" v-model="form.password" placeholder="password">
                        </div>
                    </div>

                </div>
                <div class="modal-footer">
                    <button  class="btn btn-dark waves-effect waves-light" type="button" @click="createUser">Submit</button>
                    <a href="#" class="btn btn-danger waves-effect" data-dismiss="modal">Cancel</a>
                </div>
            </div>
            <!-- /.modal-content -->
        </div>
        <!-- /.modal-dialog -->
    </div>

</template>
