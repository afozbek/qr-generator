<template>
  <div class="app-container">
    <b-field
      label="Kod Almak İstediğiniz URL'i giriniz:"
      :type="inputType"
      :message="errorMessage"
    >
      <b-input v-model="url" placeholder="Geçerli bir url giriniz."></b-input>
    </b-field>

    <b-button @click="handleUrlSubmit" :type="inputType" outlined expanded
      >QR Kodu Getir</b-button
    >

    <img
      v-if="isUrlIsValid && isCalled"
      :src="callUrl"
      alt="QR Kodunuz"
      width="400"
      height="400"
    />
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      hasError: false,
      errorMessage: "",
      url: "https://furkanozbek.com",
      baseApiUrl: "https://qrtag.net/api/qr_12.svg?url=",
      callUrl: "",
      isCalled: false,
      urlRegex:
        "(?:(?:https?|ftp|file):\/\/|www\.|ftp\.)(?:\([-A-Z0-9+&@#\/%=~_|$?!:,.]*\)|[-A-Z0-9+&@#\/%=~_|$?!:,.])*(?:\([-A-Z0-9+&@#\/%=~_|$?!:,.]*\)|[A-Z0-9+&@#\/%=~_|$])",
    };
  },
  computed: {
    inputType() {
      return this.hasError ? "is-danger" : "is-primary";
    },
    isUrlIsValid() {
      return new RegExp(this.urlRegex).test(this.url);
    },
  },
  methods: {
    async handleUrlSubmit() {
      this.validateUrl(this.url);

      this.isCalled = true;
      this.callUrl = this.baseApiUrl + this.url;
    },
    validateUrl(url) {
      let isUrlValid = new RegExp(this.urlRegex).test(url);

      if (!isUrlValid) {
        this.toastMessage("Lütfen düzgün bir url giriniz", true);

        this.hasError = true;
        this.errorMessage = "Girdiğiniz url düzgün formatta değil.";
        setTimeout(() => {
          this.hasError = false;
          this.errorMessage = "";
        }, 5000);
      }
    },
    toastMessage(message, isError) {
      this.$buefy.toast.open({
        duration: 5000,
        message: message,
        type: isError ? "is-danger" : "is-primary",
      });
    },
  },
};
</script>

<style lang="scss">
.app-container {
  width: 400px;
  margin: 0 auto;

  padding-top: 100px;
}
</style>
