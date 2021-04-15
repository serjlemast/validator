<template>
  <form class="sign-up" @submit.prevent="checkForm">
    <div class="form-group">
      <label for="login">Логин:</label>
      <input
        id="login"
        class="form-control"
        :class="$v.form.login.$error ? 'is-invalid' : ''"
        v-model.trim="form.login"
      />
      <p
        v-if="$v.form.login.$dirty && !$v.form.login.required"
        class="invalid-feedback"
      >
        Обязательное поле
      </p>
      <p
        v-if="$v.form.login.$dirty && !$v.form.login.minLength"
        class="invalid-feedback"
      >
        Здесь должно быть больше 5-и символов
      </p>
    </div>
    <div class="form-group">
      <label for="login">Почта:</label>
      <input
        id="email"
        type="email"
        class="form-control"
        :class="$v.form.email.$error ? 'is-invalid' : ''"
        v-model.trim="form.email"
      />
      <p
        v-if="$v.form.login.$dirty && !$v.form.email.required"
        class="invalid-feedback"
      >
        Обязательное поле
      </p>
      <p
        v-if="$v.form.login.$dirty && !$v.form.email.email"
        class="invalid-feedback"
      >
        Email неккоректный
      </p>
    </div>
    <div class="form-group">
      <label for="login">Пароль:</label>
      <input
        id="password"
        type="password"
        class="form-control"
        :class="$v.form.password.$error ? 'is-invalid' : ''"
        v-model.trim="form.password"
      />
      <p
        v-if="$v.form.login.$dirty && !$v.form.password.required"
        class="invalid-feedback"
      >
        Обязательное поле
      </p>
    </div>
    <div class="form-group">
      <label for="country">Страна проживания:</label>
      <select id="country" class="form-control" v-model="form.country">
        <option
          v-for="(country, index) in countries"
          :value="country.value"
          :key="index"
        >
          {{ country.label }}
        </option>
      </select>
    </div>
    <div class="form-group">
      <!-- ///////// -->
      <label for="themes">Любимые темы:</label>
      <select
        id="themes"
        class="form-control"
        :class="$v.form.favoiriteThemes.$error ? 'is-invalid' : ''"
        v-model="form.favoiriteThemes"
        multiple
      >
        <option
          v-for="(theme, index) in themes"
          :value="theme.value"
          :key="index"
        >
          {{ theme.label }}
        </option>
      </select>
      <p
        v-if="$v.form.login.$dirty && !$v.form.favoiriteThemes.maxLength"
        class="invalid-feedback"
      >
        Не больше 3-х тем
      </p>
    </div>
    <div class="form-group form-check">
      <input
        type="checkbox"
        class="form-check-input"
        :class="$v.form.rules.$error ? 'is-invalid' : ''"
        id="rules"
        v-model="form.rules"
      />
      <label class="form-check-label" for="rules">Rules understanding</label>
      <p
        v-if="$v.form.rules.$dirty && !$v.form.rules.truly"
        class="invalid-feedback"
      >
        Read rules!!!
      </p>
    </div>
    <div class="form-group form-check">
      <input
        type="checkbox"
        class="form-check-input"
        id="notification"
        v-model="form.agreeWithSendEmail"
      />
      <label class="form-check-label" for="notification"
        >Уведомлять меня о новых курсах</label
      >
    </div>
    <div class="flex">
      <div class="form-check">
        <input
          class="form-check-input"
          type="radio"
          value="male"
          name="exampleRadios"
          id="male"
          v-model="form.gendere"
        />
        <label class="form-check-label" for="male"> Мужчина </label>
      </div>
      <div class="form-check">
        <input
          class="form-check-input"
          type="radio"
          value="female"
          name="exampleRadios"
          id="female"
          v-model="form.gendere"
        />
        <label class="form-check-label" for="female"> Женщина </label>
      </div>
    </div>
    <button type="submit" class="btn btn-primary">Сохранить</button>
  </form>
</template>
<script>
import { validationMixin } from "vuelidate";
import {
  required,
  minLength,
  email,
  maxLength,
  truly,
} from "vuelidate/lib/validators";
export default {
  mixins: [validationMixin],
  data() {
    return {
      form: {
        login: "",
        email: "",
        password: "",
        country: "Russia",
        favoiriteThemes: ["IT"],
        agreeWithSendEmail: false,
        rules: "",
        gendere: "male",
      },
      countries: [
        {
          label: "Россия",
          value: "Russia",
        },
        {
          label: "Украина",
          value: "Ukraine",
        },
        {
          label: "США",
          value: "USA",
        },
      ],
      themes: [
        {
          label: "Технологии",
          value: "IT",
        },
        {
          label: "Языки",
          value: "languages",
        },
        {
          label: "Математика",
          value: "mathematics",
        },
        {
          label: "История",
          value: "history",
        },
      ],
    };
  },
  validations: {
    form: {
      login: { required, minLength: minLength(5) },
      email: { required, email },
      password: { required },
      rules: {
        truly(value) {
          return value;
        },
      },
      favoiriteThemes: {
        maxLength(value) {
          return value.length <= 3;
        },
      },
    },
  },
  methods: {
    checkForm() {
      this.$v.form.$touch();
      if (!this.$v.form.$error) {
        console.log("Валидация прошла успешно");
      }
    },
  },
};
</script>
<style lang="scss" scoped>
.form-control {
  width: 400px;
}
.form-check {
  margin-right: 10px;
}
button {
  margin-top: 15px;
}
</style>
