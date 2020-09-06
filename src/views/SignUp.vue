<template>
    <div class='signup-page'>
        <div class='title'>SignUp</div>
        <form class='signup-form' @submit.prevent='signUp' v-show='signupForm'>
            <el-row class='email-info input-area'>
                <el-col :span='6'>
                    <div class='text'>EmailAddress</div>
                </el-col>
                <el-col :span='12'>
                    <el-input v-model='email'/>
                </el-col>
            </el-row>
            <el-row class='user-info input-area'>
                <el-col :span='6'>
                    <div class='text'>ID</div>
                </el-col>
                <el-col :span='12'>
                    <el-input v-model='uesrId'/>
                </el-col>
            </el-row>
            <el-row class='password-info input-area'>
                <el-col :span='6'>
                    <div class='text'>Password</div>
                </el-col>
                <el-col :span='12'>
                    <el-input type='password' v-model='password'>
                </el-col>
            </el-row>
            <el-button class='auth-button' type='primary' native-type='submit'>サインアップ</el-button>
        </form>
        <form class='signup-form' @submit.prevent='userVerify' v-show='!signupForm'>
            <el-row class='email-info input-area'>
                <el-col :span='6'>
                    <div class='text'>VerifyCode</div>
                </el-col>
                <el-col :span='12'>
                    <el-input v-model='verifyCode'/>
                </el-col>
            </el-row>
            <el-button class='auth-button' type='primary' native-type='submit'>ユーザーの有効化</el-button>
        </form>
    </div>
</template>

<script lang='ts'>
import { Component, Prop, Vue } from 'vue-property-decorator';
import { Auth } from 'aws-amplify';
import router from '@/router';

@Component({})
export default class SignUp extends Vue {
  public email: string = '';
  public password: string = '';
  public signupForm: boolean = true;
  public userID: string = '';
  public verifyCode: string = '';

  public signUp() {
    const self = this;
    Auth.signUp(self.userID, self.password, self.email)
      .then((user) => {
        self.signupForm = false;
      }).catch((err) => {
        console.error(err);
      });
  }

  public userVerify() {
    console.log('verify!');
    const self = this;
    Auth.confirmSignUp(self.userID, self.verifyCode)
      .then((data) => {
        alert('登録完了しました');
        return router.push('/signIn');
      }).catch((err) => {
        console.error(err);
      });
  }
}
</script>

<style scoped lang='scss'>
.signup-form {
  margin: 40px auto 0;
  width: 40vw;

  .input-area {
    line-height: 60px;
    margin: 10px 0;
  }

  .auth-button {
    font-weight: bold;
    margin-top: 20px;
    width: 40%;
  }
}
</style>
