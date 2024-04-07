<script setup lang="ts">
  const supabase = useSupabaseClient()
  const email = ref('')
  const password = ref('')
  const errorString = ref('')
  const router = useRouter()
  const signInWithOtp = async () => {
    const { error } = await supabase.auth.signUp({
      email: email.value,
      password: password.value,
    })
    console.log(error)
    if (error) {
      errorString.value = error
    } else {
      router.push('/')
    }
  }
</script>
<template>
  <div>
    <button @click="signInWithOtp">
      Зарегистрироваться
    </button>
    <input
        v-model="email"
        type="email"
    />
    <input
        v-model="password"
        type="password"
    />
    <div class="error">
      {{ errorString }}
    </div>
  </div>
</template>
<style scoped lang="scss">
  @keyframes shake {
    10%, 90% { transform: translate3d(-1px, 0, 0); }
    20%, 80% { transform: translate3d(2px, 0, 0); }
    30%, 50%, 70% { transform: translate3d(-4px, 0, 0); }
    40%, 60% { transform: translate3d(4px, 0, 0); }
  }

  .error {
    animation: shake 0.6s infinite;
  }
</style>
