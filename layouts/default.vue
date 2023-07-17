<template>
  <div>
    <ElementsLoader :duration="1000" />
    <ClientOnly>
      <SharedNavbar @navLang="getLang" />
      <slot />
      <SharedFooter />
    </ClientOnly>
  </div>
</template>

<script setup>
const lang = ref('en');
const dir = lang.value == 'en' ? ref('ltr') : ref('rtl');

const getLang = (event) => {
  lang.value = event
  dir.value = lang.value == 'en' ? 'ltr' : 'rtl';
}

watchEffect(() => {
  useHead({
    htmlAttrs: {
      dir: dir.value,
      lang: lang.value,
    },
  })
})

onMounted(() => {
  lang.value = localStorage.getItem('lang') ? localStorage.getItem('lang') : ref('en');
})
</script>

<style></style>