<template>
  <main class="bg-white dark:bg-slate-900">

    <div class="relative flex">

      <!-- Content -->
      <div class="w-full md:w-1/2">
        <div class="min-h-[100dvh] h-full flex flex-col after:flex-1">

          <div class="flex-1">
            <div class="flex items-center justify-between h-16 px-4 sm:px-6 lg:px-8">
              <!-- Logo -->
              <router-link class="block" to="/">
                <svg width="32" height="32" viewBox="0 0 32 32">
                  <defs>
                    <linearGradient x1="28.538%" y1="20.229%" x2="100%" y2="108.156%" id="logo-a">
                      <stop stop-color="#A5B4FC" stop-opacity="0" offset="0%" />
                      <stop stop-color="#A5B4FC" offset="100%" />
                    </linearGradient>
                    <linearGradient x1="88.638%" y1="29.267%" x2="22.42%" y2="100%" id="logo-b">
                      <stop stop-color="#38BDF8" stop-opacity="0" offset="0%" />
                      <stop stop-color="#38BDF8" offset="100%" />
                    </linearGradient>
                  </defs>
                  <rect fill="#6366F1" width="32" height="32" rx="16" />
                  <path d="M18.277.16C26.035 1.267 32 7.938 32 16c0 8.837-7.163 16-16 16a15.937 15.937 0 01-10.426-3.863L18.277.161z" fill="#4F46E5" />
                  <path d="M7.404 2.503l18.339 26.19A15.93 15.93 0 0116 32C7.163 32 0 24.837 0 16 0 10.327 2.952 5.344 7.404 2.503z" fill="url(#logo-a)" />
                  <path d="M2.223 24.14L29.777 7.86A15.926 15.926 0 0132 16c0 8.837-7.163 16-16 16-5.864 0-10.991-3.154-13.777-7.86z" fill="url(#logo-b)" />
                </svg>
              </router-link>
            </div>
          </div>          

          <div class="max-w-sm mx-auto w-full px-4 py-8">
            <h1 class="text-3xl text-slate-800 dark:text-slate-100 font-bold mb-6">Create your Account ✨</h1>
            <!-- Form -->
            <form>
              <div class="space-y-4">
                <div>
                  <label class="block text-sm font-medium mb-1" for="email">Email Address <span class="text-rose-500">*</span></label>
                  <input id="email" class="form-input w-full" type="email" v-model="email"/>
                </div>
                <div>
                  <label class="block text-sm font-medium mb-1" for="name">Full Name <span class="text-rose-500">*</span></label>
                  <input id="username" class="form-input w-full" type="text" v-model="username" />
                </div>
              
                <div>
                  <label class="block text-sm font-medium mb-1" for="password">Password</label>
                  <input id="password" class="form-input w-full" type="password" autoComplete="on" v-model="password" />
                </div>
              </div>
              <div class="flex items-center justify-between mt-6">
            
                <button class="text-slate-800 dark:text-slate-100" @click.prevent="signUpWithEmailAndPassword">Sign Up</button>
              </div>

              <div v-if ="errMsg !== ''" class="text-xs mt-1 text-rose-500">{{ errMsg }}</div>
            </form>
            <!-- Footer -->
            <div class="pt-5 mt-6 border-t border-slate-200 dark:border-slate-700">
              <div class="text-sm">
                Have an account? <router-link class="font-medium text-indigo-500 hover:text-indigo-600 dark:hover:text-indigo-400" to="/signin">Sign In</router-link>
              </div>
            </div>
          </div>

        </div>
      </div>

      <!-- Image -->
      <div class="hidden md:block absolute top-0 bottom-0 right-0 md:w-1/2" aria-hidden="true">
        <img class="object-cover object-center w-full h-full" src="../images/auth-image.jpg" width="760" height="1024" alt="Authentication" />
        <img class="absolute top-1/4 left-0 -translate-x-1/2 ml-8 hidden lg:block" src="../images/auth-decoration.png" width="218" height="224" alt="Authentication decoration" />
      </div>

    </div>

  </main>
</template>


<script setup>
import { ref } from "vue";
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";
import { useAuthStore } from "../stores/useAuthStore";
import { useRouter } from "vue-router";

const email = ref("");
const username = ref("");
const password = ref("");
const errMsg = ref("");
const auth = getAuth();
  const router = useRouter();
const signUpWithEmailAndPassword = () => {

  const authStore = useAuthStore();

  createUserWithEmailAndPassword(auth, email.value, password.value)
  .then((userCredential) => {
    // Signed up 
    const user = userCredential.user;
    authStore.createAndSetUser(user);
    router.push("/dashboard");
    // ...
  })
  .catch((error) => {
    // const errorCode = error.code;
    errMsg.value = error.message;
    // ..
  });
}


</script>