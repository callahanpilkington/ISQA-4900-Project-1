<template>
  <!-- Welcome Text -->
  <div class="container-fluid">
    <div class="row">
      <div class="col col-12 align-items-center justify-content-center">
        <blockquote>
          &ldquo;Welcome {{ validUserName }}!&rdquo;
          <footer>
            <small>
              <em>&mdash;The Office Office Supply Store.</em>
            </small>
          </footer>
        </blockquote>
      </div>
    </div>
  </div>
  <img :src="require('@/assets/office.jpg')" class="card-img-top" style="height: 15rem">
</template>



<script>
import router from '../router'

export default {
  name: 'Home',
  data() {
    return {
      validUserName: 'Guest'
    }
  },
  mounted() {
    this.getUser()
  },
  methods: {
    getUser() {
      if (localStorage.getItem('isAuthenticated') && JSON.parse(localStorage.getItem('isAuthenticated')) === true) {
        this.validUserName = JSON.parse(localStorage.getItem('log_user'))
      }
    }
  }
}
</script>

