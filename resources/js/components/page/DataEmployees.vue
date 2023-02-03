<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">

                    <div class="card-header">
                        <h4 class="card-title"> Data Employees</h4>
                        <div class="card-tools">
                            <button type="button" class="btn btn-success mb-3" @click="addModal"><i
                                    class="fas fa-plus pr-2"></i>Add</button>
                        </div>
                    </div>

                    <!-- Modal Add Add/Update-->
                    <div class="modal fade" id="modalAdd" tabindex="-1" role="dialog" aria-labelledby="modalAdd1"
                        aria-hidden="true">
                        <div class="modal-dialog modal-dialog-centered" role="document">
                            <div class="modal-content">
                                <div class="modal-header">
                                    <h5 class="modal-title" id="exampleModalLongTitle" v-show="!statusmodal">Add
                                        Employee</h5>
                                    <h5 class="modal-title" id="exampleModalLongTitle" v-show="statusmodal">Edit
                                        Employee</h5>
                                    <button type="button" class="close" data-bs-dismiss="modal" aria-label="Close">
                                        <span aria-hidden="true">&times;</span>
                                    </button>
                                </div>
                                <form @submit.prevent="statusmodal ? editData() : saveData()">
                                    <div class="modal-body">
                                        <div class="form-group">
                                            <input type="text" v-model="form.nama" class="form-control"
                                                :class="{ 'is-invalid': form.errors.has('nama') }" placeholder="Name">
                                            <has-error :form="form" field="nama"></has-error>
                                        </div>
                                        <div class="form-group">
                                            <input type="email" v-model="form.email" class="form-control"
                                                :class="{ 'is-invalid': form.errors.has('email') }" placeholder="Email">
                                            <has-error :form="form" field="email"></has-error>
                                        </div>
                                        <div class="form-group">
                                            <input type="date" v-model="form.tanggal_lahir" class="form-control"
                                                :class="{ 'is-invalid': form.errors.has('tanggal_lahir') }"
                                                placeholder="DoB">
                                            <has-error :form="form" field="tanggal_lahir"></has-error>
                                        </div>
                                        <div class="form-group">
                                            <input type="text" v-model="form.alamat" class="form-control"
                                                :class="{ 'is-invalid': form.errors.has('alamat') }"
                                                placeholder="Address">
                                            <has-error :form="form" field="alamat"></has-error>
                                        </div>
                                    </div>
                                    <div class="modal-footer">
                                        <button type="button" class="btn btn-secondary"
                                            data-bs-dismiss="modal">Close</button>
                                        <button type="submit" class="btn btn-primary" v-show="!statusmodal"
                                            :disabled="disabled"><i v-show="loading" class="fa fa-spinner fa-spin"></i>
                                            Save</button>
                                        <button type="submit" class="btn btn-primary" v-show="statusmodal"
                                            :disabled="disabled"><i v-show="loading" class="fa fa-spinner fa-spin"></i>
                                            Save Change</button>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                    <!-- End Modal Add/Update-->

                    <!-- Modal Add Info-->
                    <div class="modal fade" id="modalInfo" tabindex="-1" role="dialog" aria-labelledby="modalInfo1"
                        aria-hidden="true">
                        <div class="modal-dialog modal-dialog-centered" role="document">
                            <div class="modal-content">
                                <div class="modal-header">
                                    <h5 class="modal-title" id="exampleModalLongTitle">Info
                                        Employee</h5>
                                    <button type="button" class="close" data-bs-dismiss="modal" aria-label="Close">
                                        <span aria-hidden="true">&times;</span>
                                    </button>
                                </div>
                                <form @submit.prevent="infoData()">
                                    <div class="modal-body">
                                        <div class="form-group">
                                            <input type="text" v-model="form.nama" class="form-control" readonly>
                                        </div>
                                        <div class="form-group">
                                            <input type="email" v-model="form.email" class="form-control" readonly>
                                        </div>
                                        <div class="form-group">
                                            <input type="date" v-model="form.tanggal_lahir" class="form-control"
                                                placeholder="DoB" readonly>
                                        </div>
                                        <div class="form-group">
                                            <input type="text" v-model="form.alamat" class="form-control"
                                                placeholder="Address" readonly>
                                        </div>
                                    </div>
                                    <div class="modal-footer">
                                        <button type="button" class="btn btn-secondary"
                                            data-bs-dismiss="modal">Close</button>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                    <!-- End Modal Info-->

                    <div class="card-body">
                        <div class="form-group">
                            <div class="table-responsive">
                                <table class="table">
                                    <tr>
                                        <th>Nama</th>
                                        <th>Email</th>
                                        <th>Action</th>
                                    </tr>
                                    <tr v-for="item in employees" :key="item.messages">
                                        <td>{{ item.nama }}</td>
                                        <td>{{ item.email }}</td>
                                        <td><a href="#" @click="editModal(item)"><i class="fas fa-edit"></i></a> | <a
                                                href="#" @click="deleteData(item.id)"><i class="fas fa-trash-alt"
                                                    style="color:red"></i></a> | <a href="#"
                                                @click="modalInfoEmp(item)"><i class="fas fa-info"
                                                    style="color:yellowgreen"></i></a></td>
                                    </tr>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Swal from 'sweetalert2'

export default {
    data() {
        return {
            loading: false,
            disabled: false,
            employees: {},
            statusmodal: false,
            form: new Form({
                id: "",
                nama: "",
                email: "",
                tanggal_lahir: "",
                alamat: ""
            })
        }
    },
    methods: {

        addModal() {
            this.statusmodal = false
            this.form.reset()
            $('#modalAdd').modal('show')
        },

        editModal(item) {
            this.statusmodal = true
            this.form.reset()
            $('#modalAdd').modal('show')
            this.form.fill(item)
        },

        modalInfoEmp(item) {
            $('#modalInfo').modal('show')
            this.form.fill(item)
        },

        loadData() {
            this.$Progress.start()
            axios.get('api/employee').then(({ data }) => (this.employees = data))
            this.$Progress.finish()
        },

        saveData() {
            this.$Progress.start()
            this.loading = true
            this.disabled = true
            this.form.post('api/employee').then(() => {
                Fire.$emit('refreshData')
                $('#modalAdd').modal('hide')
                Toast.fire({
                    icon: 'success',
                    title: 'Your work has been saved'
                })
                this.$Progress.finish()
                this.loading = false
                this.disabled = false
            })
                .catch(() => {
                    this.$Progress.fail()
                    this.loading = false
                    this.disabled = false
                })
        },

        editData() {
            this.$Progress.start()
            this.loading = true
            this.disabled = true
            this.form.put('api/employee/' + this.form.id).then(() => {
                Fire.$emit("refreshData")
                $('#modalAdd').modal('hide')
                Toast.fire({
                    icon: 'success',
                    title: 'Data has been changed'
                })
                this.$Progress.finish()
                this.loading = false
                this.disabled = false
            })
                .catch(() => {
                    this.$Progress.fail()
                    this.loading = false
                    this.disabled = false
                })
        },

        deleteData(id) {
            Swal.fire({
                title: 'Are you sure want to delete this?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#EE4B2B',
                cancelButtonColor: 'grey',
                confirmButtonColor: 'Delete'
            }).then(result => {
                if (result.value) {
                    this.form.delete('api/employee/' + id).then(() => {
                        Swal.fire(
                            'Deleted',
                            'Your data has been changed',
                            'success'
                        )
                        Fire.$emit('refreshData')
                    }).catch(() => {
                        Swal.fire(
                            'Failed',
                            'Fail to remove data',
                            'warning'
                        )
                    })
                }
            })
        }


    },
    created() {
        this.loadData()
        Fire.$on('refreshData', () => { this.loadData() })
    }
}
</script>
