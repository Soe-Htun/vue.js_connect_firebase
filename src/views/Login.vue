<template>
  <div class="cont">
    <div class="login">
      <el-input id="name" v-model="name" placeholder="EmailAddress"></el-input>
      <el-input type="password" id="password" v-model="password" placeholder="Password"></el-input>
      <br />
      <span @click="show_hide()">
        <i v-if="hide" class="far fa-eye-slash"></i>
        <i v-if="show" class="fas fa-eye"></i>
      </span>
      <el-button id="btn" type="primary" @click="login">Login(Ent)</el-button>
      <el-button type="primary" @click="Sign_up()" id="sign">Sign_up(↑)</el-button>
    </div>

    <div class="dialog" v-if="dialog==1">
      <el-input id="regname" v-model="reg_name" placeholder="Email Address"></el-input>
      <el-input type="password" id="regpassword" v-model="reg_password" placeholder="Password"></el-input>
      <el-button id="signbtn" type="primary" @click="Signup_here">Sign_up</el-button>
      <el-button id="signcancel" @click="cancel()">Cancel(-)</el-button>
    </div>
  </div>
</template>

<script>
import firebase from "firebase";
export default {
  name: "Login",
  data() {
    return {
      name: "",
      password: "",
      reg_name: "",
      reg_password: "",
      dialog: 0,
      hide:false,
      show:true
    };
  },
  methods: {
    show_hide() {
      var pass = document.getElementById("password");
      if (pass.type == "password") {
        pass.type = "text";
        this.hide =true;
        this.show=false;
      } else {
        pass.type = "password";
        this.show=true;
        this.hide=false;
      }
    },
    login() {
      firebase
        .auth()
        .signInWithEmailAndPassword(this.name, this.password)
        .then(() => {
          this.$message({
            message: "Login Successful",
            type: "success"
          });
          this.$router.replace({ name: "layout" });
        })
        .catch(e => {
          this.$message({
            message: e.message,
            type:'warning'
          });
        });
    },
    Sign_up() {
      this.dialog = 1;
    },
    Signup_here() {
      firebase
        .auth()
        .createUserWithEmailAndPassword(this.reg_name, this.reg_password)
        .then(() => {
          this.$message({
            message: "Successful Sign up",
            type: "success"
          });
          this.$router.replace("/");
          this.dialog = 0;
        })
        .catch(e => {
          // alert('Oops' + e.message);
          this.$message({
            message: e.message,
            type:'warning'
          });
        });
    },
    cancel() {
      this.dialog = 0;
    },
    keydownClick(e) {
      if (e.keyCode === 13) {
        if(this.dialog ===1){
          this.Signup_here();
        }else{
          this.login();
        }   
      }
      if (e.keyCode === 38) {
        this.Sign_up();
      }
      if (e.keyCode === 109) {
        this.cancel();
      }
    }
  },
  created(){
    window.addEventListener("keydown", this.keydownClick)
  }
};
</script>

<style>
.dialog {
  position: fixed;
  top: 50%;
  left: 50%;
  margin: -180px 0px 0px -150px;
  width: 280px;
  height: 260px;
  background-color: rgb(240, 240, 240);
  border-radius: 10px;
   background: url("./../assets/football.jpg") no-repeat;
  background-size: 100% 100%;
}
#regname {
  margin-top: 50px;
  width: 200px;
  height: 35px;
}
#regpassword{
  margin-top: 25px;
  width: 200px;
  height: 35px;
}
#signbtn {
  margin-top: 30px;
  height: 38px;
  border-radius: 5px;
  width: 90px;
}
#signcancel {
  margin-top: 30px;
  color: white;
  width: 90px;
  height: 38px;
  border-radius: 5px;
  border: 1px solid #409eff;
  background-color: #409eff;
}
#nav {
  padding: 0px;
}
.cont {
  position: fixed;
  width: 100%;
  height: 100%;
  background: url("./../assets/realmadrid.jpg") no-repeat;
  background-size: 100% 100%;
}
.login {
  position: absolute;
  top: 50%;
  left: 50%;
  margin: -100px 0px 0px -220px;
}
#name {
  text-indent: 20px;
  width: 220px;
  height: 39px;
}
#password {
  margin: 25px 0px;
  text-indent: 20px;
  width: 220px;
  height: 39px;
}
#btn {
  margin-top:5px ;
  margin-left: 0px;
  width: 95px;
}
#sign {
  margin-left: 20px;
  width: 92px;
  min-width: 100px;
}
i {
  position: absolute;
  top: 45%;
  left: 69%;
}
</style>

