<template>
  <div class="default-layout">
    <header-partial></header-partial>
    <section class="section__hero py-6 bg-black bg-cover bg-center">
      <div class="container">
        <div class="section__form bg-white p-4 w-1/2 shadow-md">
          <h1 class="mb-2 text-4xl font-light text-grey-darkest">Find homes on Platzi Rooms</h1>
          <h2 class="mb-6 text-base text-grey-dark font-normal">
            Discover entire homes and private rooms perfect for any trip.
          </h2>
          <form class="form__search">
            <div class="mb-4">
              <label class="input__label" for="where">Where</label>
              <div class="form__field relative">
                <i class="input-icon material-icons absolute text-grey-darker">search</i>
                <input
                  class="input__search"
                  id="where"
                  type="text"
                  placeholder="Mexico City, Mexico">
              </div>
            </div>
            <button
              class="px-2 py-4 bg-yellow-dark font-semibold w-full rounded text-yellow-darker">
              Search</button>
          </form>
        </div>
      </div>
    </section>
    <main class="main">
      <slot></slot>
    </main>
    <footer-partial></footer-partial>
    <!-- Modal -->
    <modal :show="modals.login" @closeModal="closeModal('login')">
      <h2 class="text-grey-darkest font-semibold text-center mb-6">
        Welcome to Platzi Rooms
      </h2>
      <form @submit.prevent="login">
        <div class="mb-4">
          <label for="input_email" class="input__label">Email</label>
          <div class="form__field relative">
            <input
              v-model="formLogin.email"
              class="input__field"
              type="text"
              name="input_email"
              id="input_email"
              placeholder="bruce.wayne@imnotbatman.org"
            >
          </div>
        </div>

        <div class="mb-4">
          <label for="input_password" class="input__label">Password</label>
          <div class="form__field relative">
            <input
              v-model="formLogin.password"
              class="input__field"
              type="password"
              name="input_password"
              id="input_password"
              placeholder="**************"
            >
          </div>
        </div>

        <div class="mb-4 flex items-center">
          <toggle-input id="input_remember_me" v-model="formLogin.rememberMe"></toggle-input>
          <label for="input_remember_me" class="input__label__toggle">Remember Me</label>
        </div>

        <div class="mb-4">
          <button class="btn btn-primary mr-3 w-full">Login</button>
        </div>
      </form>
    </modal>
    <modal :show="modals.register" @closeModal="closeModal('register')">
      <form class="form" @submit.prevent="registerUser">
        <div class="mb-4">
          <label class="input__label" for="email">Email</label>
          <div class="form__field relative">
            <input
              class="input__field"
              id="email"
              v-model="formRegister.email"
              type="email"
              placeholder="bruce.wayne@imnotbatman.org">
          </div>
        </div>
        <div class="mb-4">
          <label class="input__label" for="email">Name</label>
          <div class="form__field relative">
            <input
              class="input__field"
              id="name"
              v-model="formRegister.name"
              type="text"
              placeholder="Bruce Wayne">
          </div>
        </div>
        <div class="mb-4">
          <label class="input__label" for="password">Password</label>
          <div class="form__field relative">
            <input
              class="input__field"
              id="password"
              v-model="formRegister.password"
              type="password"
              placeholder="Create a Password">
          </div>
        </div>
        <div class="mb-4">
          <button class="btn w-full">Create account</button>
        </div>
      </form>
    </modal>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
import HeaderPartial from '@/partials/HeaderPartial.vue';
import FooterPartial from '@/partials/FooterPartial.vue';
import Modal from '@/components/Modal.vue';
import ToggleInput from '@/components/ToggleInput.vue';

export default {
  name: 'DefaultLayout',
  components: {
    HeaderPartial,
    FooterPartial,
    ToggleInput,
    Modal,
  },
  data() {
    return {
      formLogin: {
        email: '',
        password: '',
        rememberMe: false,
      },
      formRegister: {
        email: '',
        name: '',
        password: '',
      },
    };
  },
  computed: {
    ...mapGetters(['modals']),
  },
  methods: {
    closeModal(name) {
      this.$store.dispatch('TOGGLE_MODAL', {
        name,
        value: false,
      });
    },
    registerUser() {
      this.$store.dispatch('CREATE_USER', this.formRegister)
        .then(() => this.closeModal('register'));
    },
    login() {
      this.$store.dispatch('SIGN_IN', this.formLogin)
        .then(() => this.closeModal('login'));
    },
  },
};
</script>

<style>
  .section__hero {
    min-height: 450px;
    background-image: url("https://images.unsplash.com/photo-1504202302068-15fc2055f7f9?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1330&q=80");
  }

  .form__field .input-icon {
    top: 7px;
    left: 9px;
  }

  .form__field > .input__search {
    @apply pl-10;
  }

  @media(max-width: 576px) {
    .section__hero {
      min-height: 250px;
    }

    .section__form {
      @apply w-full;
    }
  }
</style>
