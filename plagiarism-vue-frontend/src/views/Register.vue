<template>
    <div class="container login">
        <div class="container col-lg-7">
            <div class="card">
                <div class="card-body">
                    <h2 class="card-title">Register</h2>
                    <p class="card-text">to collect detection statistic and have access to API</p>
                    <div class="alert alert-danger" role="alert" v-if="error">
                        <p>{{error.message}}</p>
                    </div>
                    <form>
                        <div class="mb-3">
                            <label for="exampleInputEmail1" class="form-label">Username</label>
                            <input type="text" class="form-control" id="exampleInputEmail1"
                                   aria-describedby="emailHelp" v-model="username">
                            <div v-if="error && error.details && error.details.login" style="margin-top: 5px;color:red;">
                                <p>{{error.details.login}}</p>
                            </div>
                        </div>
                        <div class="mb-3">
                            <label for="exampleInputPassword1" class="form-label">Password</label>
                            <input type="password" class="form-control" id="exampleInputPassword1" v-model="password1">
                            <div v-if="error && error.details && error.details.password" style="margin-top: 5px;color:red;">
                                <p>{{error.details.password}}</p>
                            </div>
                        </div>
                        <div class="mb-3">
                            <label for="exampleInputPassword2" class="form-label">Password again</label>
                            <input type="password" class="form-control" id="exampleInputPassword2" v-model="password2">
                        </div>
                        <div class="row align-items-start">
                            <div class="col">
                                <button type="button" class="btn btn-success full-width" v-on:click="submitRegister()">
                                    Register
                                </button>
                            </div>
                        </div>
                    </form>
                </div>

            </div>

        </div>
    </div>
</template>

<script>

    export default {
        data() {
            return {
                username: '',
                password1: '',
                password2: '',
                error: null,
            }
        },
        methods: {
            submitRegister() {
                if (this.password1 === this.password2) {
                    this.axios
                        .post('http://localhost:8080/auth/register', {
                            login: this.username,
                            password: this.password1
                        })
                        .then(response => this.$router.push('/login'))
                        .catch(error=>{
                            try {
                                this.error = {
                                    message: error.response.data.message,
                                    details: JSON.parse(error.response.data.details)
                                }
                            } catch (e) {
                                this.error = {
                                    message: error.response.data.message
                                }
                            }
                        })
                }
                else {
                    this.error = {message: 'Passwords are not equal'}
                }
            }

        }
    }
</script>

<style scoped>
    .card {
        margin-top: 50px;
    }

    .full-width {
        width: 100%;
    }
</style>