<template>
    <div class="container-scroller">
    <div class="container-fluid page-body-wrapper full-page-wrapper">
      <div class="main-panel">
        <div class="content-wrapper d-flex align-items-center auth">
          <div class="row w-100">
            <div class="col-lg-4 mx-auto">
              <div class="auth-form-light text-left p-5">
                <div class="navbar-brand brand-logo">
                  <img src="assets/img/logo.svg">
                </div>
                <h4>Selamat datang!</h4>
                <h6 class="font-weight-light">Daftar untuk menggunakan aplikasi daily scrum.</h6>
                <form class="pt-3" method="post" action="#" v-on:submit.prevent="Register">
                  <div class="form-group">
                    <div class="input-group">
                      <div class="input-group-prepend bg-transparent">
                        <span class="input-group-text bg-transparent border-right-0">
                          <i class="mdi mdi-account-outline text-primary"></i>
                        </span>
                      </div>
                      <input v-model="firstname" type="text" class="form-control form-control-lg border-left-0" id="firstname" name="firstname" placeholder="First Name" required>
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="input-group">
                      <div class="input-group-prepend bg-transparent">
                        <span class="input-group-text bg-transparent border-right-0">
                          <i class="mdi mdi-account-outline text-primary"></i>
                        </span>
                      </div>
                      <input v-model="lastname" type="text" class="form-control form-control-lg border-left-0" id="lastname" name="lastname" placeholder="Last Name" required>
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="input-group">
                      <div class="input-group-prepend bg-transparent">
                        <span class="input-group-text bg-transparent border-right-0">
                          <i class="mdi mdi-message-outline text-primary"></i>
                        </span>
                      </div>
                      <input v-model="email" type="email" class="form-control form-control-lg border-left-0" id="email" name="email" placeholder="E-Mail" required>
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="input-group">
                      <div class="input-group-prepend bg-transparent">
                        <span class="input-group-text bg-transparent border-right-0">
                          <i class="mdi mdi-lock-outline text-primary"></i>
                        </span>
                      </div>
                      <input v-model="password" type="password" class="form-control form-control-lg border-left-0" name="password" id="password" placeholder="New Password" required>                        
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="input-group">
                      <div class="input-group-prepend bg-transparent">
                        <span class="input-group-text bg-transparent border-right-0">
                          <i class="mdi mdi-lock-outline text-primary"></i>
                        </span>
                      </div>
                      <input v-model="password_confirmation"  type="password" class="form-control form-control-lg border-left-0" name="password_confirmation" id="password_confirmation" placeholder="Retype Your New Password" required>                        
                    </div>
                  </div>
                  <div class="my-3">
                    <input type="submit" name="submit" class="btn btn-block btn-primary btn-lg font-weight-medium auth-form-btn" value="REGISTER">
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- content-wrapper ends -->
    </div>
    <!-- page-body-wrapper ends -->
  </div>
</template>

<script>
export default {
     data() {
      return {
        firstname: '',
        lastname: '',
        email: '',
        password: '',
        password_confirmation: ''

      }
    },
    methods: {
        Register: function(){
            let firstname = this.firstname 
            let lastname = this.lastname 
            let email = this.email 
            let password = this.password
            let password_confirmation = this.password_confirmation
            this.$store.dispatch('register', { firstname, lastname, email, password, password_confirmation})
            .then(() => this.$router.push('/login'))
            .catch(err => console.log(err))
        }
    }
}
</script>