<template>
  <div class="pure-u-5-5">
    <form class="pure-form" @submit.prevent="submit">
      <fieldset>
        <legend>Sign in <span v-if="errors > 0" class="u-textRed">({{errors}}) ({{fields}})</span></legend>
        <input
          :class="{'is-invalid': $v.email.$dirty}"
          type="text"
          placeholder="Email"
          v-model.trim="email"
        />
        <input
          :class="{'is-invalid': $v.password.$dirty}"
          type="password"
          placeholder="Password"
          v-model.trim="password"
        />
        <button type="submit" class="pure-button pure-button-primary">Sign in</button>
      </fieldset>
    </form>
  </div>
</template>
<script>
import { validationMixin } from 'vuelidate'
import { required, minLength, maxLength } from 'vuelidate/lib/validators'

export default {
  mixins: [validationMixin],
  data () { return { email: null, password: null } },
  computed: {
    errors () {
      return Object.values(this.$v)
        .filter(v => !!v)
        .filter(f => f.hasOwnProperty('$dirty'))
        .filter(v => v.$dirty)
        .length
    },
    fields () {
      return Object.entries(this.$v)
        .filter(([key, value]) => !!value)
        .filter(([key, value]) => value.hasOwnProperty('$dirty'))
        .filter(([key, value]) => value.$dirty)
        .map(([key, value]) => key)
    }
  },
  validations: {
    email: {
      required
    },
    password: {
      required,
      minLength: minLength(6),
      maxLength: maxLength(12)
    }
  },
  methods: {
    submit () {
      this.$v.$touch()

      Object.values(this.$v)
        .filter(f => !!f)
        .filter(f => f.hasOwnProperty('$invalid'))
        .filter(f => !f.$invalid)
        .forEach(f => f.$reset())

      if (this.$v.$anyError) {
        return
      }

      alert('Good!')
    }
  }
}
</script>
