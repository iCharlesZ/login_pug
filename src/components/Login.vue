<template lang='pug'>
  .login-block
    .login-animate(:class="{'username-animate': accFocus, 'password-animate': pwdFocus}")
      .left-ear(:style="{left: accFocus ? getPos.calcLeftEar + 'em' : ''}")
      .right-ear(:style="{left: accFocus ? getPos.calcRightEar + 'em' : ''}")
      .head
        .left-eye(:class="{doe: accLen}"  :style="{left: accFocus ? getPos.calcLeftEye + 'em' : ''}")
        .right-eye(:class="{doe: accLen}"  :style="{left: accFocus ? getPos.calcRightEye + 'em' : ''}")
        .face(:style="{left: accFocus ? getPos.calcFace + 'em' : ''}")
          .nose(:style="{left: accFocus ? getPos.calcNose + 'em' : ''}")
          .mouth(:class="{doe: accLen, show: pwdEyeObj.pwdHide}")
      .body
      .left-arm(:class="{show: pwdEyeObj.pwdHide}")
      .right-arm(:class="{show: pwdEyeObj.pwdHide}")
    .login-form
      .username
        input(type="text" name="username" autocomplete="off"
          v-model="account" @focus="accountFocus" @blur="accountBlur")
      .password
        input(:type="pwdIptType" name="password" autocomplete="off"
          v-model="password" @focus="passwordFocus" @blur="passwordBlur")
        .pwd-eye(v-if="shoPwdEye" @click="showPwd" :class="pwdEyeObj" @blur="eyeBlur")
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      account: "",
      password: "",
      accFocus: false,
      pwdFocus: false,
      accLen: false,
      getPos: {
        calcFace: 0,
        calcNose: 0,
        calcLeftEye: 0,
        calcRightEye: 0,
        calcLeftEar: 0,
        calcRightEar: 0
      },
      pwdIptType: "password",
      shoPwdEye: false,
      pwdEyeObj: {
        pwdShow: true,
        pwdHide: false
      }
    };
  },
  watch: {
    account(val) {
      if (val) {
        this.password = "";
        this.shoPwdEye = false;
        this.getposition(val);
        this.pwdIptType = "password";
        this.pwdEyeObj = {
          pwdShow: true,
          pwdHide: false
        };
      }
    }
  },
  methods: {
    accountFocus() {
      this.accFocus = true;
      this.pwdFocus = false;
      this.getposition(this.account);
    },
    accountBlur() {
      this.accFocus = false;
    },
    passwordFocus() {
      this.pwdFocus = true;
      this.shoPwdEye = true;
    },
    passwordBlur() {
      if (!this.password) {
        this.shoPwdEye = false;
        this.pwdFocus = false;
      }
    },
    eyeBlur() {
      console.log("eyeBlur");
    },
    showPwd() {
      if (this.password) {
        this.pwdIptType = this.pwdIptType === "password" ? "text" : "password";
        this.pwdEyeObj.pwdShow = [
          this.pwdEyeObj.pwdHide,
          (this.pwdEyeObj.pwdHide = this.pwdEyeObj.pwdShow)
        ][0];
      } else {
        this.shoPwdEye = false;
      }
    },
    getposition(val) {
      let face = parseFloat((1.5 / 36) * val.length);
      let nose = parseFloat((1 / 36) * val.length);
      let left_eye = parseFloat((1.5 / 36) * val.length);
      let right_eye = parseFloat((2 / 36) * val.length);
      let left_ear = parseFloat((1 / 36) * val.length);
      let right_ear = parseFloat((1 / 36) * val.length);
      this.getPos.calcFace = 1 + (face > 1.5 ? 1.5 : face);
      this.getPos.calcNose = 0.9 + (nose > 1 ? 1 : nose);
      this.getPos.calcLeftEye = 0.5 + (left_eye > 1.5 ? 1.5 : left_eye);
      this.getPos.calcRightEye = 4 + (right_eye > 2 ? 2 : right_eye);
      this.getPos.calcLeftEar = 1.5 - (left_ear > 1 ? 1 : left_ear);
      this.getPos.calcRightEar = 7.5 - (right_ear > 1 ? 1 : right_ear);
      if (val.length >= 6) {
        this.accLen = true;
      } else {
        this.accLen = false;
      }
    }
  }
};
</script>

<style lang="sass" scoped>
.login-block
  display: flex
  flex-direction: column
  align-items: center
  margin-top: 5rem
  width: 20em
  height: 25em
  background: #3dceba
  border-radius: 10px
  box-shadow: 0 0 16px #333

  .login-animate
    &, &.password-animate
      width: 10em
      height: 10em
      border-radius: 5em
      background: #cefefb
      margin: 20px 0
      position: relative
      overflow: hidden

    div
      transition: all 0.5s
      -o-transition: all 0.5s
      -moz-transition: all 0.5s
      -webkit-transition: all 0.5s

    .body
      background-image: url(../assets/body.png)
      background-size: 100%
      background-repeat: no-repeat
      background-position: center
      width: 6.5em
      height: 6em
      position: absolute
      bottom: -2em
      left: calc(50% - 3.25em)
      z-index: 0

    .left-ear
      background-image: url(../assets/left-ear.png)
      background-size: 100%
      background-repeat: no-repeat
      background-position: center
      width: 2em
      height: 2em
      position: absolute
      bottom: 5.5em
      left: calc(50% - 4em)

    .right-ear
      background-image: url(../assets/right-ear.png)
      background-size: 100%
      background-repeat: no-repeat
      background-position: center
      width: 2em
      height: 2em
      position: absolute
      bottom: 5.5em
      left: calc(50% + 2em)

    .head
      position: relative
      background-image: url(../assets/face.png)
      background-size: 100%
      background-repeat: no-repeat
      background-position: center
      width: 7.5em
      height: 7.25em
      position: absolute
      bottom: 1em
      left: calc(50% - 3.75em)
      z-index: 1

      .left-eye
        background-image: url(../assets/eye.png)
        background-size: 100%
        background-repeat: no-repeat
        background-position: center
        width: 1em
        height: 1em
        position: absolute
        bottom: 4em
        left: calc(50% - 2.5em)

      .right-eye
        background-image: url(../assets/eye.png)
        background-size: 100%
        background-repeat: no-repeat
        background-position: center
        width: 1em
        height: 1em
        position: absolute
        bottom: 4em
        left: calc(50% + 1.5em)

      .face
        background-image: url(../assets/muzzle.png)
        background-size: 100%
        background-repeat: no-repeat
        background-position: center
        width: 4em
        height: 4em
        position: absolute
        bottom: 0em
        left: calc(50% - 2em)

        .nose
          background-image: url(../assets/nose.png)
          background-size: 100%
          background-repeat: no-repeat
          background-position: center
          width: 1em
          height: 1em
          position: absolute
          top: 0.5em
          left: calc(50% - 0.5em)

        .mouth
          background-image: url(../assets/mouth-smile.png)
          background-size: 100%
          background-repeat: no-repeat
          background-position: center
          width: 2em
          height: 1em
          position: absolute
          bottom: 1em
          left: calc(50% - 1em)

    .left-arm
      background-image: url(../assets/left-arm.png)
      background-size: 100%
      background-repeat: no-repeat
      background-position: center
      width: 3em
      height: 7em
      position: absolute
      bottom: -7.5em
      left: 1em
      z-index: 2
      transition: bottom 0.5s

    .right-arm
      background-image: url(../assets/right-arm.png)
      background-size: 100%
      background-repeat: no-repeat
      background-position: center
      width: 3em
      height: 7em
      position: absolute
      bottom: -7.5em
      left: 6em
      z-index: 2
      transition: bottom 0.5s

    &.username-animate
      .left-ear
        left: 1.5em

      .right-ear
        left: 7.5em

      .left-eye
        left: 0.5em
        bottom: 3.5em
        &.doe
          background-image: url(../assets/eye-doe.png)
      .right-eye
        left: 4em
        bottom: 3.5em
        &.doe
          background-image: url(../assets/eye-doe.png)

      .face
        left: 1em

        .nose
          left: 0.9em
          top: 0.65em

        .mouth
          bottom: 0.75em
          background-image: url(../assets/mouth-half.png)
          &.doe
            background-image: url(../assets/mouth-open.png)
            height: 2em
            bottom: 0.5em

    &.password-animate
      .left-arm,
      .right-arm
        bottom: -0.5em
        &.show
          bottom: -1.7em !important

      .mouth.show
        width: 1em
        left: calc(50% - 0.5em)
        background-image: url(../assets/mouth-circle.png)

  .login-form
    width: calc(100% - 5em)
    padding: 0 2.5em

    .username,
    .password
      position: relative
      width: 100%
      margin: 20px 0
      border-radius: 5px
      overflow: hidden

      input
        width: calc(100% - 20px)
        padding: 0 10px
        height: 2.5em
        border: 0
        border-radius: 5px
        outline: none

    .password
      > input
        width: calc(100% - 3.3em)
        padding: 0 2.3em 0 1em

      .pwd-eye
        width: 1.2em
        height: 1.2em
        position: absolute
        top: calc(50% - 0.6em)
        right: 0.5em
        background-size: 100%
        background-repeat: no-repeat
        background-position: center
        cursor: pointer
        &.pwdShow
          background-image: url("../assets/password-show.png")
        &.pwdHide
          background-image: url("../assets/password-hide.png")
</style>
