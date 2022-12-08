<!-- COMPONENTE BOILERPLATE -->
 
  <template>
<div class="container">
  <div class="content-box">
  <div class="header">
      <div class="header-description">
        <h3 class="header-title">Log In</h3>
        <p class="header-subtitle">Start organizing your tasks!</p>
      </div>
    </div>

  <form @submit.prevent="signIn" class="form-sign-in">
      <div class="form">
        <div class="form-input">
          <label class="input-field-label"></label>
          <input
            type="email"
            class="input-field sign"
            placeholder="Your e-mail"
            id="email"
            v-model="email"
            required
          />
        </div>
        <div class="form-input">
          <label class="input-field-label"></label>
          <input
            type="password"
            class="input-field sign"
            placeholder="Password"
            id="password"
            v-model="password"
            required
          />
        </div>
        <button class="button" type="submit">Sign In</button>
        <p>
          Don't have an account?
          <PersonalRouter
            :route="route"
            :buttonText="buttonText"
            class="sign-up-link"
          />
        </p>
      </div>
    </form>

    <div v-show="errorMsg">{{errorMsg}}</div>
  </div>
</div>
</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";

// Route Variables
const route = "/auth/signup";
const buttonText = "Sign Up";

// Input Fields
const email = ref("");
const password = ref("");

// Error Message
const errorMsg = ref("");

// Router to push user once SignedUp to Log In
const redirect = useRouter();

// Arrow function to Signin user to supaBase
const signIn = async () => {
    try {
      // calls the user store and send the users info to backend to logIn
      await useUserStore().signIn(email.value, password.value);
      // redirects user to the homeView
      redirect.push({ path: "/" });
    } catch (error) {
      // displays error message
      errorMsg.value = error.message;
      // hides error message
      setTimeout(() => {
        errorMsg.value = null;
      }, 5000);
    }
    return;
  }
  // errorMsg.value = "error";
</script>

<style></style>
