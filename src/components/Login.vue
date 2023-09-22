<template>
  <!-- <h1>LOGIN</h1> -->
  <v-card>
    <v-card-title style="font-size: 18px !important">เข้าสู่ระบบ </v-card-title>
    <v-card-text>
      <v-form ref="form" v-model="valid" lazy-validation>
        <v-text-field
          v-model="name"
          :rules="nameRules"
          :counter="20"
          placeholder="ชื่อผู้ใช้งาน"
          required
          outlined
        ></v-text-field>

        <v-text-field
          v-model="password"
          placeholder="รหัสผ่าน"
          required
          outlined
        ></v-text-field>

        <v-btn
          :disabled="!valid"
          color="success"
          class="mr-4"
          @click="Login()"
          block
        >
          เข้าสู่ระบบ
        </v-btn>
      </v-form>
    </v-card-text>
  </v-card>
</template>

<script>
// import LoginView from '@/views/LoginView.vue'

export default {
  data: () => ({
    valid: true,
    name: '',
    nameRules: [
      (v) => !!v || 'กรุณากรอกชื่อผู้ใช้งาน',
      (v) => (v && v.length <= 10) || 'ห้ามกรอกชื่อผุ้ใช้งานเกิน20ตัวอักษร'
    ],
    password: '',
    passwordRules: [(v) => !!v || 'กรุณากรอกรหัสผ่าน']
  }),

  methods: {
    Login() {
      if (this.$refs.form.validate(true)) {
        localStorage.setItem('username', this.name)
        this.$EvenBus.$emit('getUsername')
        this.$router.push('/')
      }
    }
  }
}
</script>
<style></style>
s
