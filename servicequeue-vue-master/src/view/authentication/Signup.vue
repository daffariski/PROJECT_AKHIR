<template>
  <a-row justify="center">
    <a-col :xxl="6" :xl="12" :md="12" :sm="18">
      <AuthWrapper>
        <div class="ninjadash-authentication-top">
          <h2 class="ninjadash-authentication-top__title">Sign Up</h2>
        </div>
        <div class="ninjadash-authentication-content">
          <a-form
            name="register"
            :model="formState"
            @finish="handleSubmit"
            layout="vertical"
          >
            <a-form-item
              label="Name"
              name="name"
              :rules="[
                { required: true, message: 'Please input your Full name!' },
              ]"
            >
              <a-input v-model:value="formState.name" placeholder="Full name" />
            </a-form-item>
            <a-form-item
              name="email"
              label="Email Address"
              :rules="[
                {
                  required: true,
                  message: 'Please input your email!',
                  type: 'email',
                },
              ]"
            >
              <a-input
                type="email"
                v-model:value="formState.email"
                placeholder="name@example.com"
              />
            </a-form-item>
            <a-form-item
              label="Phone"
              name="phone"
              :rules="[
                { required: true, message: 'Please input your Phone Number!' },
              ]"
            >
              <a-input v-model:value="formState.phone" placeholder="08xxxxxxxx" />
            </a-form-item>
            <a-form-item
              label="Address"
              name="address"
              :rules="[
                { required: true, message: 'Please input your Address!' },
              ]"
            >
              <a-input v-model:value="formState.address" placeholder="Address" />
            </a-form-item>
            <a-form-item
              label="Password"
              name="password"
              :rules="[
                { required: true, message: 'Please input your password!' },
              ]"
            >
              <a-input
                type="password"
                v-model:value="formState.password"
                placeholder="Password"
              />
            </a-form-item>
            <a-form-item
              label="Password Confirmation"
              name="password_confirmation"
              :rules="[
                { required: true, message: 'Please input your password confirmation!' },
              ]"
            >
              <a-input
                type="password"
                v-model:value="formState.password_confirmation"
                placeholder="Password Confirmation"
              />
            </a-form-item>
            <div class="ninjadash-auth-extra-links">
              <a-checkbox @change="onChange">
                Creating an account means you’re okay with our Terms of Service
                and Privacy Policy
              </a-checkbox>
            </div>
            <a-form-item>
              <sdButton
                class="btn-create"
                htmlType="submit"
                type="primary"
                size="lg"
              >
                Create Account
              </sdButton>
            </a-form-item>
          </a-form>
        </div>
        <div class="ninjadash-authentication-bottom">
          <p>
            Already have an account?<router-link to="/auth/login"
              >Sign In</router-link
            >
          </p>
        </div>
      </AuthWrapper>
    </a-col>
  </a-row>
</template>
<script>
import { AuthWrapper } from "./style";
import { reactive, ref, defineComponent } from "vue";

const SignUp = defineComponent({
  name: "SignUp",
  components: { AuthWrapper },
  setup() {
    const values = ref(null);
    const checked = ref(null);
    const handleSubmit = (value) => {
      values.value = value;
    };

    const onChange = (check) => {
      checked.value = check;
    };
    const formState = reactive({
      name: "",
      username: "",
      email: "",
      password: "",
    });
    return {
      onChange,
      handleSubmit,
      formState,
    };
  },
});

export default SignUp;
</script>
