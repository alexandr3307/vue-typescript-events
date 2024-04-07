<script setup lang="ts">
    import { ref } from 'vue';
    const supabase = useSupabaseClient()
    const email = ref('')
    const password = ref('')
    const errorString = ref('')
    const router = useRouter()
    const errorElement = ref<HTMLElement | null>(null);
    const signInWithOtp = async () => {
        const { error } = await supabase.auth.signInWithPassword({
            email: email.value,
            password: password.value,
        })
        if (error) {
          errorString.value = error
          errorElement.value.classList.add('shake');

          errorElement.value.addEventListener('animationend', () => {
            if (errorElement.value) {
              errorElement.value.classList.remove('shake');
            }
          });
        } else {
          router.push('/')
        }
    }
    function signUp() {
      router.push('/auth')
    }
</script>
<template>
  <div class="container">
    <div class="buttons">
      <button @click="signInWithOtp">
        Войти
      </button>
      <button @click="signUp">
        Нет аккаунта?
      </button>
    </div>
    <input
        v-model="email"
        type="email"
    />
    <input
        v-model="password"
        type="password"
    />
    <div class="error" ref="errorElement">
      {{ errorString }}
    </div>
  </div>
</template>
<style>
  .buttons {
    display: flex;
    justify-content: space-between;
  }
</style>
<style scoped lang="scss">
  @keyframes shake {
    10%, 90% { transform: translate3d(-1px, 0, 0); }
    20%, 80% { transform: translate3d(2px, 0, 0); }
    30%, 50%, 70% { transform: translate3d(-4px, 0, 0); }
    40%, 60% { transform: translate3d(4px, 0, 0); }
  }

  .error {
    animation: shake 0.6s;
  }
</style>
