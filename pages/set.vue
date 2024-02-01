<template>
  <div>
    <div class="d-flex align-items-center w-100" style="height: 100vh;">
      <div class="container">
        <div class="row justify-content-center d-flex gap-3">
          <div class="col-12 col-md-3 d-flex flex-column border border-5 rounded-4 p-4 gap-3 fade1">
            <strong>
              <p id="langth" class="text-center fs-2" >Langth</p>
            </strong>
            <input v-model="Langth" type="number" class="form-control text-center" style="border-color: var(--primary-color);" placeholder="Langth" >
          </div>
          <div class="col-12 col-md-3 d-flex flex-column border border-5 rounded-4 p-4 gap-3 fade2">
            <strong>
              <p  id="volume" class="text-center fs-2">Volume</p>
            </strong>
            <input v-model="Volume" type="number" class="form-control text-center" style="border-color: var(--primary-color);" placeholder="Volume">
          </div>
          <div class="col-12 col-md-3 d-flex flex-column border border-5 rounded-4 p-4 gap-3 fade3">
            <strong>
              <p id="function" class="text-center fs-2" >Function</p>
            </strong>
            <input v-model="functionValue" type="text"  class="form-control text-center" style="border-color: var(--primary-color);" placeholder="Function" >
          </div>
        </div>
        <div class="row">
          <div class="col-12 d-flex justify-content-center mt-5">
            <button type="button" class="btn btn-lg rounded rounded-pill btn-outline-primary fw-bold mt-4 px-4 btn-l fade1" @click="set()">start</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const Swal = require('sweetalert2')
export default {
  data() {
    return {
      Langth: null,
      Volume: null,
      functionValue: null 
    }
  },
  mounted() {
    this.get();
  },
  methods: {
    set() {
      if (this.Langth !== 0 && this.Volume !== 0 && this.functionValue !== 0 && this.Langth !== null && this.Volume !== null && this.functionValue !== null) {
        localStorage.setItem("Langth", this.Langth);
        localStorage.setItem("Volume", this.Volume);
        localStorage.setItem("functionValue", this.functionValue);
        // console.log(this.Langth , this.Volume, this.functionValue);
        window.location = 'Model';
      } else {
        const title = () => {
          if (this.Langth === 0) {
            return 'กรุณากรอก Langth'
          }
          if (this.Volume === 0) {
            return 'กรุณากรอก Volume'
          }
          if (this.functionValue === 0) {
            return 'กรุณากรอก function'
          }
        }
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: title()
        });
      }
    },
    get() {
      this.Langth = localStorage.getItem("Langth");
      this.Volume = localStorage.getItem("Volume");
      this.functionValue = localStorage.getItem("functionValue");

      if (!this.Langth && !this.Volume && !this.functionValue) {
        this.Langth = 0
        this.Volume = 0
        this.functionValue = 0
      }
    }
  },
}
</script>




<style scoped>
        /* CSS animation to gradually reveal the text */
        @keyframes fade {
            0% {
                opacity: 0;
            }

            100% {
                opacity: 1;
            }
        }

        /* Apply the animation to the text */
        .fade1 {
            opacity: 0;
            animation: fade 0.9s ease forwards;
        }
        .fade2 {
            opacity: 0;
            animation: fade 1.4s ease forwards;
        }
        .fade3 {
            opacity: 0;
            animation: fade 1.9s ease forwards;
        }
</style>
