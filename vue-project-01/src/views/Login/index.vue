<template>
  <div id="login">
    <div class="login-wrap">
      <ul class="menu-tab">
        <li
          v-for="tab in menuTab"
          :key="tab.id"
          :class="{ current: tab.current }"
          @click="toggleMenu(tab)"
        >
          {{ tab.txt }}
        </li>
      </ul>
      <!-- Form -->
      <el-form
        :model="ruleForm"
        status-icon
        :rules="rules"
        ref="ruleForm"
        class="login-form"
        size="medium"
      >
        <el-form-item prop="email" class="form-item">
          <label>Email</label>
          <el-input
            type="email"
            v-model="ruleForm.email"
            autocomplete="off"
          ></el-input>
        </el-form-item>

        <el-form-item prop="password" class="form-item">
          <label>Password</label>
          <el-input
            type="password"
            v-model="ruleForm.password"
            autocomplete="off"
            minlength="6"
            maxlength="20"
          ></el-input>
        </el-form-item>

        <el-form-item
          prop="confirmPassword"
          class="form-item"
          v-if="model === 'signup'"
        >
          <label>Confirm Password</label>
          <el-input
            type="password"
            v-model="ruleForm.confirmPassword"
            autocomplete="off"
            minlength="6"
            maxlength="20"
          ></el-input>
        </el-form-item>

        <el-form-item prop="vcode" class="form-item">
          <label>Verification Code</label>
          <el-row :gutter="10">
            <el-col :span="15"
              ><el-input
                type="text"
                v-model="ruleForm.vcode"
                minlength="6"
                maxlength="6"
              ></el-input
            ></el-col>
            <el-col :span="9"
              ><el-button type="success" class="block"
                >Get Code</el-button
              ></el-col
            >
          </el-row>
        </el-form-item>

        <el-form-item>
          <el-button
            type="danger"
            @click="submitForm('ruleForm')"
            class="login-btn block"
            >Submit</el-button
          >
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import {
  stripscript,
  validateEmail,
  validatePass,
  validateVCode
} from "@/utils/validate";
export default {
  name: "Login",
  data() {
    var validateUser = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("Email should not be blank"));
      } else if (!validateEmail(value)) {
        callback(new Error("Please enter a valid email"));
      } else {
        callback();
      }
    };
    var validatePassword = (rule, value, callback) => {
      this.ruleForm.password = stripscript(value);
      value = this.ruleForm.password;
      if (value === "") {
        callback(new Error("Password should not be blank"));
      } else if (value.length < 6 || value.length > 20) {
        callback(new Error("Password must be 6-20 characters"));
      } else if (!validatePass(value)) {
        callback(new Error("Password must be alphanumeric"));
      } else {
        // if (this.ruleForm.password !== "") {
        //   this.$refs.ruleForm.validateField("password");
        // }
        callback();
      }
    };
    var validateConfirmPassword = (rule, value, callback) => {
      if (this.model === "login") {
        callback();
      }
      if (value === "") {
        callback(new Error("Confirm Password should not be blank"));
      } else if (value !== this.ruleForm.password) {
        callback(new Error("Passwords do not match"));
      } else {
        // if (this.ruleForm.password !== "") {
        //   this.$refs.ruleForm.validateField("password");
        // }
        callback();
      }
    };
    var checkVerificationCode = (rule, value, callback) => {
      this.ruleForm.vcode = stripscript(value);
      value = this.ruleForm.vcode;
      if (value === "") {
        callback(new Error("Verification code should not be blank"));
      } else if (!validateVCode(value)) {
        callback(new Error("Please enter valid verification code"));
      } else {
        callback();
      }
    };
    return {
      menuTab: [
        { txt: "Login", current: true, type: "login" },
        { txt: "Sign Up", current: false, type: "signup" }
      ],
      model: "login",
      // Form data
      ruleForm: {
        email: "",
        password: "",
        confirmPassword: "",
        vcode: ""
      },
      rules: {
        email: [{ validator: validateUser, trigger: "blur" }],
        password: [{ validator: validatePassword, trigger: "blur" }],
        confirmPassword: [
          { validator: validateConfirmPassword, trigger: "blur" }
        ],
        vcode: [{ validator: checkVerificationCode, trigger: "blur" }]
      }
    };
  },
  created() {},
  mounted() {},
  // Functions
  methods: {
    toggleMenu(data) {
      this.model = data.type;
      this.menuTab.forEach(ele => {
        ele.current = false;
      });
      data.current = true;
    },
    // Form Methods
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    }
  }
};
</script>

<style lang="scss" scoped>
#login {
  height: 100vh;
  background-color: #344a5f;
}
.login-wrap {
  width: 330px;
  margin: auto;
}
.menu-tab {
  text-align: center;
  li {
    display: inline-block;
    width: 88px;
    line-height: 36px;
    font-size: 14px;
    color: #fff;
    border-radius: 2px;
    cursor: pointer;
  }
  .current {
    background-color: rgba(0, 0, 0, 0.1);
  }
}
.login-form {
  margin-top: 30px;
  label {
    display: block;
    margin-bottom: 5px;
    font-size: 14px;
    color: #fff;
  }
  .form-item {
    margin-bottom: 13px;
  }
  .block {
    width: 100%;
    display: block;
  }
  .login-btn {
    margin-top: 20px;
  }
}
</style>
