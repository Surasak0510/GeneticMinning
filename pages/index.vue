<template>
  <div>
    <Home/>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  layout: 'default'
}
</script>

<style>
        /* CSS animation to gradually reveal the text */
        @keyframes fadeInText {
            0% {
                opacity: 0;
            }

            100% {
                opacity: 1;
            }
        }

        /* Apply the animation to the text */
        .fadeInText {
            opacity: 0;
            animation: fadeInText 0.9s ease forwards;
        }
        .fadeInText2 {
            opacity: 0;
            animation: fadeInText 1.4s ease forwards;
        }
</style>
