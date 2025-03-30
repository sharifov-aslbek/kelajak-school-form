<template>
  <div class="min-h-screen bg-white">
  
    <!-- Logo and Form Container -->
    <div class="px-4 py-6">
      <!-- Logo -->
      <img class="w-24 rounded-full mx-auto my-4" src="https://kelajakschoolqarshi.uz/assets/logo-2-DDoOoWgd.jpg" alt="Error">

      <!-- Form Container -->
      <div v-if="!formSubmitted" class="max-w-md mx-auto bg-white rounded-xl overflow-hidden border border-blue-100 shadow-sm">
        <!-- Form Header -->
        <div class="p-6 text-center border-b">
          <h1 class="text-xl font-bold mb-2">Maktabimizda 2025-yil uchun qabul ochiq</h1>
          <p class="text-sm text-gray-600">
            Quyida ma'lumotlaringizni qoldiring. Tez orada biz siz bilan bog'lanamiz
          </p>
        </div>

        <!-- Form Fields -->
        <div class="p-6">
          <form @submit.prevent="submitForm">
            <!-- Name Field -->
            <div class="mb-4">
              <label class="block text-sm mb-1">
                Ism-familiyangiz? <span class="text-red-500">*</span>
              </label>
              <n-input v-model:value="formData.name" placeholder="Ism-familiyangiz" class="bg-gray-50 rounded-md" />
            </div>

            <!-- Grade Field -->
            <div class="mb-4">
              <label class="block text-sm mb-1">
                Farzandingiz bu yil nechanchi sinfga o'qimoqchi <span class="text-red-500">*</span>
              </label>
              <n-select
                v-model:value="formData.grade"
                :options="gradeOptions"
                placeholder="Sinfni tanlang"
                class="bg-gray-50 rounded-md"
              />
            </div>

            <!-- Phone Field -->
            <div class="mb-4">
              <label class="block text-sm mb-1">
                Telefon raqamingiz? <span class="text-red-500">*</span>
              </label>
              <n-input v-model:value="formData.phone" placeholder="+998 XX XXX XX XX" class="bg-gray-50 rounded-md" />
            </div>


            <div class="mb-4">
              <label class="block text-sm mb-1">
          <a class="text-blue-500 flex items-center gap-2" href="https://kelajakschoolqarshi.uz/">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-help-circle ml-1"><circle cx="12" cy="12" r="10"/><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/><path d="M12 17h.01"/></svg>
            Bizning saytimizga o'tish uchun ustiga bosing</a>
              </label>
            </div>


            <div v-if="showError" class="text-red-500 text-xs mt-1 flex items-center mb-4">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-alert-triangle mr-1"><path d="m21.73 18-8-14a2 2 0 0 0-3.48 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3Z"/><path d="M12 9v4"/><path d="M12 17h.01"/></svg>
                Поле обязательно для заполнения
              </div>

            <!-- Checkbox -->
            <!-- <div class="mb-6 flex items-center">
              <n-checkbox v-model:checked="formData.agree" class="mr-2">
                <span class="text-blue-500">*</span>
              </n-checkbox>
            </div> -->

            <!-- Submit Button -->
            <button 
              type="submit" 
              class="w-full bg-blue-600 text-white py-3 rounded-md font-medium hover:bg-blue-700 transition-colors"
            >
              Yuborish
            </button>
          </form>
        </div>

        <!-- Report Issue Link -->
        <!-- <div class="p-4 text-center border-t">
          <a href="#" class="text-gray-400 text-sm flex items-center justify-center">
            Сообщить о нарушении
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-help-circle ml-1"><circle cx="12" cy="12" r="10"/><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/><path d="M12 17h.01"/></svg>
          </a>
        </div> -->
      </div>

      <!-- Success Screen -->
      <div v-else class="max-w-md mx-auto bg-[#9abb3f] rounded-xl overflow-hidden p-10 text-center flex flex-col items-center justify-center min-h-[400px]">
        <div class="bg-[#9abb3f]/50 p-4 rounded-full mb-6">
          <div class="bg-white rounded-full p-6">
            <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="#9abb3f" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-check"><path d="M20 6 9 17l-5-5"/></svg>
          </div>
        </div>
        <p class="text-xl font-medium text-gray-800">Rahmat! Tez orada siz bilan bog'lanamiz</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref , computed} from "vue";
import { NInput , NSelect } from "naive-ui";

const formData = ref({
  name: "",
  phone: "",
  grade: "",
});

const formSubmitted = ref(false);
const showError = ref(false);

const submitForm = () => {
  if (!formData.value.name || !formData.value.phone || !formData.value.grade) {
    showError.value = true;
    return;
  }

  sendMessageToTelegram();
};

const gradeOptions = computed(() => {
  return Array.from({ length: 11 }, (_, i) => ({
    label: String(i + 1),
    value: i + 1
  }));
});

const sendMessageToTelegram = async () => {
  const botToken = "7428093019:AAGsswL9Ai8ZmZLeFg6HC9LkemZZVKLRuEA"; // Bot tokenni kiriting
  const chatId = -1002150211504; // Chat ID ni kiriting

  const message = `📌 Yangi ariza:\n\n👤 Ism: ${formData.value.name}\n📞 Telefon: ${formData.value.phone}\n📚 Sinf: ${formData.value.grade}`;

  const url = `https://api.telegram.org/bot${botToken}/sendMessage`;
  const params = new URLSearchParams({
    chat_id: chatId,
    text: message,
  });

  try {
    await fetch(url, {
      method: "POST",
      headers: { "Content-Type": "application/x-www-form-urlencoded" },
      body: params,
    });

    // alert("Ma'lumot yuborildi!");
    formData.value = { name: "", phone: "", grade: "" };
    formSubmitted.value = true;
  } catch (error) {
    console.error("Xatolik yuz berdi:", error);
    alert("Xatolik yuz berdi, qayta urinib ko'ring.");
  }
};
</script>

<style scoped>
/* Custom styles to match Naive UI components */
:deep(.n-input) {
  background-color: #f9fafb;
  border-radius: 0.375rem;
}

:deep(.n-select) {
  background-color: #f9fafb;
  border-radius: 0.375rem;
}

:deep(.n-base-selection) {
  background-color: #f9fafb !important;
  border-radius: 0.375rem;
}
</style>