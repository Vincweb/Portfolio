<template>
  <section class="contact">
    <div class="form-container reveal">
      <h2>Me Contacter</h2>
      <form @submit.prevent="submitForm">
        <div class="form-group">
          <label for="name">Nom / Prénom</label>
          <input type="text" id="name" name="name" required v-model="name">
        </div>
        <div class="form-group">
          <label for="email">Email</label>
          <input type="email" id="email" name="email" required v-model="email" pattern="[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,4}$">
        </div>
        <div class="form-group">
          <label for="message">Message</label>
          <textarea id="message" name="message" rows="10" required v-model="message"></textarea>
        </div>
        <div class="form-group">
          <VueRecaptcha 
            ref="recaptcha"
            :sitekey="sitekey"
            size="invisible"
            class="recaptcha"
            @verify="onCaptchaVerified"
            @expired="onCaptchaExpired"
          />
        </div>
        <div class="form-group">
          <button type="submit" :class="{'valid': sended, 'error': error}">
            <i v-if="loading" class="fa fa-spinner fa-spin"/>
            <span v-else>Envoyer</span>
          </button>
          <button type="reset" class="reset" @click="reset">
            Réinitialiser
          </button>
        </div>
      </form>
      <div class="social-media">
        <button href="https://www.facebook.com/vincentcaud" target="_blank" rel="noopener noreferrer"><i class="fab fa-facebook-f"></i></button>
        <button href="https://twitter.com/vincwebb" target="_blank" rel="noopener noreferrer"><i class="fab fa-twitter"></i></button>
        <button href="https://www.linkedin.com/in/vincentcaudron" target="_blank" rel="noopener noreferrer"><i class="fab fa-linkedin"></i></button>
        <button href="https://github.com/Vincweb" target="_blank" rel="noopener noreferrer"><i class="fab fa-github"></i></button>
        <button href="https://gitlab.com/Vincweb" target="_blank" rel="noopener noreferrer"><i class="fab fa-gitlab"></i></button>
      </div>
    </div>
  </section>
  <footer class="footer">
      <p class="caption">VINCENT CAUDRON - VUEJS3 - MADE WITH CHATGPT</p>
  </footer>
</template>

<script>
import axios from 'axios';
import { VueRecaptcha } from 'vue-recaptcha';

export default {
  name: 'ContactSection',
  components: {
    VueRecaptcha
  },
  data() {
    return {
      name: '',
      email: '',
      message: '',
      sended: false,
      error: false,
      loading: false,
      apiKey: 'aFiJlzLiJK3rdhqVrDm4C7nYpOS8PHET2auBmY7w',
      sitekey: '6Lch2pcUAAAAAK-uglxyW9ljSwhgJLXTlxYVEjkY',
      url: 'https://acdx69s8v8.execute-api.eu-west-3.amazonaws.com/dev/sendemail'
    }
  },
  mounted() {
    const observer = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if(entry.intersectionRatio > .3) {
          entry.target.classList.add('reveal-visible');
          observer.unobserve(entry.target);
        }
      }, { threshold: .3})
    })
    document.querySelectorAll('.reveal').forEach((r) => {
      observer.observe(r);
    })
  },
  methods: {
    submitForm() {
      if(!this.loading || !this.sended) {
        this.loading = true;
        this.$refs.recaptcha.execute();
      }
    },
    onCaptchaVerified(recaptchaToken) {
      this.$refs.recaptcha.reset();
      this.sended = false;
      this.error = false;

      axios.post(this.url, {
        email: {
          name: this.name,
          email: this.email,
          message: this.message,
        },
        recaptcha: recaptchaToken
      }, {
        headers: {
          'Content-Type': 'application/json',
          'x-api-key': this.apiKey,
        },
      }).then(() => {
        this.sended = true;
      }).catch((error) => {
        this.error = true;
        console.log(error);
      }).finally( () => {
        this.loading = false;
      });
    },
    onCaptchaExpired() {
      this.loading = false;
      this.$refs.recaptcha.reset();
    },
    reset(){
      this.name = '';
      this.email = '';
      this.message = '';
      this.sended = false;
      this.error = false;
    }
  }
}
</script>

<style scoped>
.contact {
  background: linear-gradient(#000000, #384454);
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.reveal{
  opacity: 0;
  transform: translateY(-30px)
}
.reveal-visible{
  opacity: 1;
  transform: translateY(0);
  transition: 2s cubic-bezier(.5, 0, 0, 1);
}
.form-container {
  padding: 40px;
  border-radius: 10px;
  background-color: white;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
}
@media (min-width:960px) { 
  .form-container {
    width: 600px;
  }
}
.form-group {
  margin-bottom: 20px;
}
label {
  display: block;
  margin-bottom: 10px;
  font-weight: bold;
}
input,
textarea {
  width: 100%;
  padding: 10px;
  border-radius: 5px;
  border: none;
  background-color: #f5f5f5;
}
.form-group button {
  background-color: #384454;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}
.form-group button:hover {
  background-color: #28343d;
}
.reset {
  margin-left: 10px;
  color: black !important;
  border: 1px solid !important;
  background-color: white !important;
}
.reset:hover {
  background-color: WhiteSmoke !important;
}
.valid {
  background-color: olivedrab !important;
}
.error {
  background-color: darkred !important;
}
.social-media {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
.social-media button {
  background-color: white;
  color: black;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}
.footer {
  background-color: #384454;
  text-align: center;
}
.caption {
  margin: 0;
  padding-bottom: 10px;
  font-size: x-small;
  color: rgba(250, 250, 250, 0.3);
}
.recaptcha {
  display: none;
}
</style>
