<template>
    <div class="container-fluid">
        <div class="row align-items-center justify-content-center">
            <div class="col col-12 col-sm-6 col-md-10 col-lg-6">
                <div class="card">
                  <div class="card-header" style="font-size: 24px; margin-bottom: 10px;">Register</div>


                    <div v-if="showMsg === 'error'" class="alert alert-danger" role="alert">
                    Invalid username or password. Please Try again.
                    </div>
                    <div class="p-2"> </div>

                    <div class="card-body">
                        <div class="row align-items-center justify-content-center" v-if="loading">
                            <!-- Creating the progress bar -->
                            <div class="progress">
                            <div
                                class="
                                progress-bar progress-bar-striped progress-bar-animated
                                "
                                role="progressbar"
                                aria-valuenow="75"
                                aria-valuemin="0"
                                aria-valuemax="100"
                                style="width: 75%"
                            ></div>
                            </div>
                        </div>
                   <!-- Input Field Section -->
                        <form v-else ref="form">
                            <div class="container-fuild"></div>

                                    <div class="form-group row justify-content-left py-2">
                                        <label class="col-4">Username</label>
                                        <div class="col col-8">
                                            <input name="username" v-model="credentials.username" type="text" class="form-control-sm form-control">
                                        </div>
                                    </div>
                                    <div class="form-group row justify-content-end py-2">
                                        <label class="col-4">Password</label>
                                        <div class="col col-8">
                                            <input v-model="credentials.password" type="password" class="form-control-sm form-control">
                                        </div>
                                    </div>

                                    <div class="form-group row justify-content-left py-2">
                                        <label class="col-4">Re-enter password</label>
                                        <div class="col col-8">
                                            <input v-model="credentials.password2" type="password" class="form-control-sm form-control">
                                        </div>
                                    </div>
                                   <div class="form-group row justify-content-left py-2">
                                        <label class="col-4">Email</label>
                                        <div class="col col-8">
                                            <input v-model="credentials.email" type="email" class="form-control-sm form-control">
                                        </div>
                                    </div>
                                   <div class="form-group row justify-content-left py-2">
                                        <label class="col-4">First Name</label>
                                        <div class="col col-8">
                                            <input v-model="credentials.first_name" type="text" class="form-control-sm form-control">
                                        </div>
                                    </div>
                                    <div class="form-group row justify-content-left py-2">
                                        <label class="col-4">Last Name</label>
                                        <div class="col col-8">
                                            <input v-model="credentials.last_name" type="text" class="form-control-sm form-control">
                                        </div>
                                    </div>
                                    <div class="row justify-content-around">
                                      <div class="btn-group" role="group" aria-label="Button group with nested dropdown">
                                      <div type="button" class="btn btn-primary col-4" @click="login" style="background-color: lightblue; color: white; margin-bottom: 10px;">Back to Login</div>
                                      
                                      <div type="button" class="btn btn-primary col-4" @click="registerUser" style="background-color: lightblue; color: white;">Register</div>
                                      </div>
                           </div>
                        </form>
                   </div>
                </div>
            </div>
        </div>

    </div>
</template>



<script>
  import router from '../router';
  import { APIService } from '../http/APIService';

  const apiService = new APIService();

  export default {
    name: 'Register',

    data: () => ({
      credentials: {},
      password: '',
      repassword: '',
      valid: true,
      showMsg: '',
      loading: false,
      rules: {
        username: [
          v => !!v || 'Username is required',
          v => (v && v.length > 3) || 'A username must be more than 3 characters long',
          v => /^[a-z0-9_]+$/.test(v) || 'A username can only contain letters and digits',
        ],
        password: [
          v => !!v || 'Password is required',
          v => (v && v.length > 7) || 'The password must be longer than 7 characters',
        ],
        email: [v => !!v || 'Email is required'],
        repassword: [v => (v == this.password) || 'Passwords must match'],
      },
      showPassword: false,
    }),

    methods: {
      registerUser() {
        this.loading = true;
        apiService
          .registerUser(this.credentials)
          .then((response) => {
            if (response.status === 201) {
              this.showMsg = '';
              router.push({name: 'Auth'});
            } else {
              this.showMsg = 'error';
            }
            this.loading = false;
          })
          .catch((error) => {
            if (error.response.status === 401) {
              router.push({name: 'Auth'});
            } else if (error.response.status === 400) {
              this.showMsg = 'error';
            }
            this.loading = false;
          });
      },

      login() {
        router.push({ name: 'Auth' });
      },
    },
  };
</script>
