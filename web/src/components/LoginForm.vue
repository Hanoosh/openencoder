<template>
  <div id="login-form" class="container">
    <h2>Login</h2>
    <b-form @submit="onSubmit" v-if="show">
      <b-form-group
        label="Username:"
        label-for="input-username"
      >
        <b-form-input
          id="input-username"
          v-model="form.username"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group label="Password:" label-for="input-password">
        <b-form-input
          id="input-password"
          v-model="form.password"
          type="password"
          required
        ></b-form-input>
      </b-form-group>

      <b-button type="submit" variant="primary">Submit</b-button>
    </b-form>

    <b-alert
      class="mt-4"
      :show="dismissCountDown"
      dismissible
      fade
      variant="danger"
      @dismissed="dismissCountDown=0"
      @dismiss-count-down="countDownChanged"
    >
      {{ errorMessage }}
    </b-alert>
  </div>
</template>

<script>
import auth from '../auth';

export default {
  components: {
  },
  data() {
    return {
      form: {
        username: '',
        password: '',
      },
      show: true,
      dismissSecs: 5,
      dismissCountDown: 0,
      showDismissibleAlert: false,
      errorMessage: '',
    };
  },

  methods: {
    countDownChanged(dismissCountDown) {
      this.dismissCountDown = dismissCountDown;
    },
    onSubmit(event) {
      event.preventDefault();
      auth.login(this, this.form, '/', (err) => {
        if (err) {
          this.errorMessage = err.body && err.body.message;
          this.dismissCountDown = this.dismissSecs;
        }
      });
    },
  },
};
</script>
