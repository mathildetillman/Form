<template>
  <section>
    <v-layout
      column
      justify-center
      align-center
    >
      <v-flex
        xs12
        sm5
      >
        <div
          class="form"
        >
          <!--v-form for å hente inn brukerdata og validere input: -->
          <v-form
            v-model="valid"
            ref="form"
          >
            <h1 class="header">Fyll inn skjema</h1>
            <v-divider></v-divider>
            <v-card-text>
              <v-text-field
                regular
                v-model="name"
                class="input-field"
                name="name"
                label="Navn*"
                :rules="[rules.required, rules.nameLength, rules.noNumbers]"
                validate-on-blur
                min="2"
                required
                prepend-inner-icon="person"
                color="#00008B"
              ></v-text-field>
              <v-text-field
                regular
                v-model="email"
                class="input-field"
                name="email"
                label="E-post*"
                type="email"
                :rules="[rules.required, rules.email]"
                validate-on-blur
                required
                prepend-inner-icon="mail"
                color="#00008B"
              ></v-text-field>
              <v-text-field
                regular
                v-model="phone"
                class="input-field"
                name="phone"
                label="Telefon*"
                type="number"
                max="99999999"
                :rules="[rules.required, rules.phoneLength]"
                validate-on-blur
                required
                prepend-inner-icon="phone"
                color="#00008B"
              ></v-text-field>
              <v-text-field
                regular
                v-model="areacode"
                class="input-field"
                name="areacode"
                label="Postnummer"
                type="number"
                max="9999"
                :rules="[rules.required, rules.areacodeLength]"
                validate-on-blur
                prepend-inner-icon="location_on"
                color="#00008B"
              ></v-text-field>
              <v-text-field
                regular
                v-model="comment"
                hint="Vil du legge ved en kommentar?"
                class="input-field"
                name="comment"
                label="Kommentar"
                prepend-inner-icon="comment"
                color="#00008B"
              ></v-text-field>
            </v-card-text>
          </v-form>
          <v-divider></v-divider>
          <!-- Gjemt skjema. Har ansvar for å sende inn data: -->
          <form
            action="https://heksemel.no/case/submit.php"
            method="post"
          >
            <input hidden name="applicant" v-model="applicant_name">
            <input hidden name="name" v-model="name">
            <input hidden name="phone" v-model="phone">
            <input hidden name="email" v-model="email">
            <input hidden name="areacode" v-model="areacode">
            <input hidden name="comment" v-model="comment">
            <v-layout
              row
              justify-center
              align-center>
              <!-- Submit knapp kun gyldig når feltene er korrekt fylt ut: -->
              <v-btn :disabled="!valid" color="#33cc33" class="rounded-corners" large block type="submit">Send inn</v-btn>
            </v-layout>
          </form>
        </div>
      </v-flex>
    </v-layout>
  </section>
</template>

<script>
import '../css/style.css'
import '../css/main.css'
import '../css/normalize.css'

export default {
  name: 'form',
  data () {
    return {
      applicant_name: 'Mathilde Tillman Hegdal',
      areacode: '',
      comment: '',
      email: '',
      name: '',
      phone: '',
      rules: { // Regler for gyldig input til ulike felt
        areacodeLength: value => value.length === 4 || 'Postnr må bestå av 4 siffer',
        email: value => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(value) || 'Ugyldig e-post',
        nameLength: value => value.length >= 2 || 'Navn må bestå av minimum 2 bokstaver',
        noNumbers: value => /^([^0-9]*)$/.test(value) || 'Navn kan ikke inneholde tall',
        phoneLength: value => value.length === 8 || 'Telefonnummer må bestå av 8 siffer',
        required: value => !!value || 'Obligatorisk felt'
      },
      valid: false // Sjekker om inputfeltene er gyldige
    }
  }
}

</script>
