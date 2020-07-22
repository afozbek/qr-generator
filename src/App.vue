<template>
  <div class="app-container">
    <b-field
      label="Enter Your Url Here:"
      :type="inputType"
      :message="errorMessage"
    >
      <b-input
        type="url"
        v-model="url"
        placeholder="Geçerli bir url giriniz."
      ></b-input>
    </b-field>

    <b-button @click="handleUrlSubmit" type="is-primary" outlined expanded
      >Outlined</b-button
    >

    <img v-if="isUrlIsValid && isCalled" :src="callUrl" alt="QR Kodunuz" />
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      hasError: false,
      errorMessage: "",
      url: "https://google.com.tr",
      baseApiUrl: "https://qrtag.net/api/qr_12.svg?url=",
      callUrl: "",
      isCalled: false,
      urlRegex:
        "(?:(?:https?|ftp|file):\/\/|www\.|ftp\.)(?:\([-A-Z0-9+&@#\/%=~_|$?!:,.]*\)|[-A-Z0-9+&@#\/%=~_|$?!:,.])*(?:\([-A-Z0-9+&@#\/%=~_|$?!:,.]*\)|[A-Z0-9+&@#\/%=~_|$])",
    };
  },
  computed: {
    inputType() {
      return this.hasError ? "is-error" : "is-primary";
    },
    isUrlIsValid() {
      console.log(new RegExp(this.urlRegex).test(this.url));
      return new RegExp(this.urlRegex).test(this.url);
    },
  },
  methods: {
    async handleUrlSubmit() {
      console.log("Hello World");

      this.$buefy.toast.open({
        duration: 5000,
        message: `Something's not good, also I'm on bottom`,
        type: "is-danger",
      });

      this.isCalled = true;
      this.callUrl = this.baseApiUrl + this.url;
    },
    validateUrl(url) {
      let isUrlValid = new RegExp(this.urlRegex).test(url);

      if (!isUrlValid) {
        toastMessage("Lütfen düzgün bir url giriniz", true);

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
