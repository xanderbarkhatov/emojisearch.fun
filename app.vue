<script setup lang="ts">
import { useCompletion } from "ai/vue";

useServerSeoMeta({
  title: "AI-powered Emoji Search",
  description: "Find emojis for any context",
  ogSiteName: "emojisearch.fun",
  ogTitle: "emojisearch.fun",
  ogDescription: "AI-powered Emoji Search",
  ogImage: "https://emojisearch.fun/og.png",
  ogImageWidth: 1200,
  ogImageHeight: 630,
  ogImageType: "image/png",
  twitterImage: "https://emojisearch.fun/og.png",
  twitterCard: "summary_large_image",
  twitterTitle: "emojisearch.fun",
  twitterDescription: "AI-powered Emoji Search",
  twitterCreator: "@xanderbarkhatov",
});

const { input, completion, isLoading, error, handleSubmit } = useCompletion();

const emojis = computed(() => splitEmojis(completion.value));

const onSubmit = (e: Event) => {
  error.value = undefined;
  input.value = input.value.trim();

  if (input.value) {
    handleSubmit(e);
  }
};
</script>

<template>
  <Html class="h-full">
    <Body class="min-w-[320px] flex flex-col h-full">
      <UContainer as="main" class="mt-36 sm:mt-60 flex-1">
        <h1 class="text-4xl md:text-6xl text-center font-medium">
          Find <span class="text-yellow-400">emojis</span> for any context
        </h1>

        <div class="mt-16 max-w-md mx-auto">
          <form @submit.prevent="onSubmit">
            <UInput v-model="input" :loading="isLoading" size="xl" autofocus>
              <template #leading>
                <span class="text-xs" :class="{ 'animate-spin': isLoading }">
                  {{ isLoading ? "⏳" : "🔍" }}
                </span>
              </template>
            </UInput>
          </form>

          <div v-if="error" class="mt-6">
            <div class="text-2xl text-center">😢</div>
            <p class="mt-2 text-red-500 text-center">Something's not right. Please try again.</p>
          </div>

          <div v-else class="mt-6 grid grid-cols-6 sm:grid-cols-10 justify-items-center">
            <EmojiButton v-for="emoji in emojis" :key="emoji" :emoji="emoji" class="grow-0" />
          </div>
        </div>
      </UContainer>

      <footer>
        <UContainer class="mt-16 mb-8">
          <div class="text-center text-gray-400">
            Made with 💚 by
            <NuxtLink
              to="https://xanderbarkhatov.com"
              target="_blank"
              class="text-sky-500 hover:underline"
              external
            >
              xanderbarkhatov
            </NuxtLink>
          </div>
        </UContainer>
      </footer>
    </Body>
  </Html>
</template>

<style>
#__nuxt {
  @apply h-full flex flex-col;
}

html {
  font-family: Chewy, "Chewy fallback";
}
</style>
