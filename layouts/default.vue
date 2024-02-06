<template>
  <div>
    <div class="contaner position-absolute w-100 nav-background">
        <div class="row m-0 px-md-5 py-0">
            <div class="col-12 d-flex">
                <a class="text-container fs-3 fw-bold link-light link-offset-2 link-offset-3-hover link-underline link-underline-opacity-0 me-auto p-2" href="/">Genetic</a>
                <a class="text-container link-light link-offset-2 link-offset-3-hover link-underline link-underline-opacity-0 p-0 me-5 my-auto text-center" href="set">Create Model</a>
                <!-- <a class="text-container link-light link-offset-2 link-offset-3-hover link-underline link-underline-opacity-0 p-0 mx-5 my-auto text-center">Flex item</a> -->
            </div>
        </div>
    </div>
    <nuxt />
  </div>
</template>

<script lang="ts" setup>

</script>

<style>
.text-container {
    display: inline-block;
    transition: transform 0.3s ease, box-shadow 0.3s ease; 
}

.text-container:hover {
    transform: scale(1.09);
}


</style>