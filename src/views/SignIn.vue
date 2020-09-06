<template>
    <div class='signin-page'>
        <div class='title'>SignIn</div>
        <form class='signin-form' @submit.prevent='signIn'>
            <el-row class='user-info input-area'>
                <el-col :span='6'>
                    <div class='text'>ID</div>
                </el-col>
                <el-col :span='12'>
                    <el-input v-model='userID'/>
                </el-col>
            </el-row>
            <el-row class='password-info input-area'>
                <el-col :span='6'>
                    <div class='text'>Password</div>
                </el-col>
                <el-col :span='12'>
                    <el-input type='password' v-model='password'/>
                </el-col>
            </el-row>
            <el-button class='auth-button' type='primary' native-type='submit'>サインイン</el-button>
            <el-button class='auth-button' type='info' @click='linkSignUp'>サインアップする</el-button>
        </form>
    </div>
</template>

<script lang='ts'>
import { Component, Prop, Vue } from 'vue-property-decorator';
import { Auth } from 'aws-amplify';
import router from '@/router';

@Component({})
export default class SignIn extends Vue {
  public password: string = '';
  public userID: string = '';

  public signIn() {
    const self = this;
    Auth.signIn(self.userID, self.password)
      .then((user) => {
        return router.push('/');
      }).catch((err) => {
        console.error(err);
      });
  }

  public linkSignUp() {
    return router.push('/signUp');
  }
}
</script>
<style scoped lang='scss'>
.signin-form {
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
