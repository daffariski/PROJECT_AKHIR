<template>
  <a-row justify="center">
    <a-col :xxl="6" :xl="12" :md="12" :sm="18">
      <AuthWrapper>
        <div class="ninjadash-authentication-top">
          <h2 class="ninjadash-authentication-top__title">Sign in</h2>
        </div>
        <div class="ninjadash-authentication-content">
          <a-form @finish="handleSubmit" :model="formState" layout="vertical">
            <a-form-item name="username" label="Email Address">
              <a-input type="email" v-model:value="formState.email" />
            </a-form-item>
            <a-form-item name="password" initialValue="123456" label="Password">
              <a-input
                type="password"
                v-model:value="formState.password"
                placeholder="Password"
              />
            </a-form-item>
            <div class="ninjadash-auth-extra-links">
              <a-checkbox @change="onChange">Keep me logged in</a-checkbox>
              <router-link class="forgot-pass-link" to="/auth/forgotPassword">
                Forgot password?
              </router-link>
            </div>
            <a-form-item>
              <sdButton class="btn-signin" htmlType="submit" type="primary">
                {{ isLoading ? "Loading..." : "Sign In" }}
              </sdButton>
            </a-form-item>
          </a-form>
        </div>
        <div class="ninjadash-authentication-bottom">
          <p>
            Don't have an account?<router-link to="/auth/register"
              >Sign up</router-link
            >
          </p>
        </div>
      </AuthWrapper>
    </a-col>
  </a-row>
</template>
<script>
import { computed, reactive, ref, defineComponent } from "vue";
import { useStore } from "vuex";
import { AuthWrapper } from "./style";
import { useRouter } from "vue-router";
import { Auth0Lock } from "auth0-lock";
import { auth0options } from "@/config/auth0";

const domain = process.env.VUE_APP_AUTH0_DOMAIN;
const clientId = process.env.VUE_APP_AUTH0_CLIENT_ID;

const SignIn = defineComponent({
  name: "SignIn",
  components: { AuthWrapper },
  setup() {
    const { state, dispatch } = useStore();
    const isLoading = computed(() => state.auth.loading);
    const checked = ref(null);
    const router = useRouter();

    const handleSubmit = () => {
      router.push("/");
      dispatch("login");
    };
    const onChange = (checked) => {
      checked.value = checked;
    };

    const formState = reactive({
      email: "example@email.com",
      password: "1234565",
    });

    const lock = new Auth0Lock(clientId, domain, auth0options);

    lock.on("authenticated", (authResult) => {
      lock.getUserInfo(authResult.accessToken, (error) => {
        if (error) {
          return;
        }

        handleSubmit();
        lock.hide();
      });
    });

    return {
      isLoading,
      checked,
      handleSubmit,
      onChange,
      formState,
      lock,
    };
  },
});

export default SignIn;
</script>
