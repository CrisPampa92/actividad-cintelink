<template>
  <b-row class="login-page" no-gutters>
    <b-col
      xl="4"
      lg="4"
      sm="12"
      cols="12"
      offset-xl="4"
      offset-lg="4"
      class="col-form-card"
    >
      <b-row class="form-card" no-gutters>
        <b-col cols="12" class="d-flex flex-column align-items-center">
          <b-img src="/images/logo.png" width="161" height="37" class="my-4" />
          <b-form @submit.prevent="login">
            <b-form-input
              type="text"
              v-model="user"
              id="user"
              name="user"
              placeholder="Usuario"
              required
              :state="$v.user.$dirty || errorPassword ? !true : null"
            />
            <div class="form-error">
              <span v-if="$v.user.$error" v-html="errorUser" />
            </div>
            <b-form-input
              type="password"
              v-model="password"
              id="password"
              name="password"
              placeholder="Contraseña"
              required
              :state="$v.password.$dirty || errorPassword ? !true : null"
              :title="
                $v.password.$dirty
                  ? 'La contraseña debe tener al menos 8 caracteres'
                  : null
              "
            />
            <div class="form-error">
              <span
                v-if="errorPassword"
                v-html="'Nombre de usuario o contraseña incorrectos'"
              />
            </div>
            <div
              class="w-100 d-flex flex-column justify-content-center align-items-center mt-3 mb-3"
            >
              <b-button type="submit" variant="info">
                <b-img
                  src="/images/unlock.svg"
                  width="10"
                  height="12"
                  class="mr-1 mb-1"
                />
                Login
              </b-button>
              <b-button type="button" variant="link" class="mt-3">
                Olvidaste la contraseña?
              </b-button>
              <b-button type="button" variant="link" class="btn-create"
                >Crear una cuenta</b-button
              >
            </div>
          </b-form>
        </b-col>
      </b-row>
    </b-col>
  </b-row>
</template>

<script>
import { validationMixin } from "vuelidate";
import { minLength, maxLength } from "vuelidate/lib/validators";
export default {
  mixins: [validationMixin],
  data: () => ({
    user: null,
    password: null,
    errorUser: "El nombre de usuario debe tener entre 3 y 20 caracteres",
    errorPassword: "",
  }),
  validations: {
    user: {
      minLength: minLength(3),
      maxLength: maxLength(20),
    },
    password: {
      minLength: minLength(8),
    },
  },
  methods: {
    async login() {
      this.$v.$touch();
      if (this.$v.$anyError) return;
      const data = {
        user_name: this.user,
        user_pass: this.password,
      };
      this.$axios.defaults.headers.post["Content-Type"] =
        "application/x-www-form-urlencoded";
      this.$axios.defaults.headers.post["Access-Control-Allow-Origin"] = "*";
      this.$axios
        .post("https://cintelink.com.ar/login.mod.php", data)
        .then(({ response }) => {
          console.log(response);
          this.$router.push({ name: "app" });
        })
        .catch((e) => {
          console.log(e);
          this.errorPassword = true;
        });
    },
  },
};
</script>
