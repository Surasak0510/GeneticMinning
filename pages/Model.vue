<template>
    <div>
        <div class="container pt-5">
            <div class="p-3"></div>
            <div v-for="(item, indexOfResults) in results" :key="'result-' + indexOfResults" data-aos="fade-up" data-aos-duration="1500" class="row border my-2 border-2 p-2 gap-2 rounded-3">
                <p class="py-0 m-0">เซ็ตที่ {{ indexOfResults + 1 }}</p>
                <div v-for="(i , indexOfSet) in item.set" :key="indexOfSet" class="col py-auto border border-2 rounded-pill">
                    <p class="text-center m-0 py-2">{{ i }}</p>
                </div>
                <div class="col-12 mt-2">
                    <p class="text-center">ค่า Fitness : {{ item.fitness }}</p>
                </div>
            </div>
            <div class="row">
                <p class="text-center">
                    ------------------------ displayset ------------------------
                </p>
                <!-- <pre>
                    {{ displayset }}
                </pre> -->
            </div>
            <div v-for="(item , index) in displayset" :key="index" data-aos="fade-up" data-aos-duration="1500" class="row border my-2 border-2 p-2 gap-2 rounded-3">
                <p class="py-0 m-0">เซ็ตที่ {{ index + 1 }}</p>
                <div v-for="(i , indexOfSet) in item[1]" :key="indexOfSet" class="col py-auto border border-2 rounded-pill">
                    <p class="text-center m-0 py-2">{{ i }}</p>
                </div>
                <div class="col-12 mt-2">
                    <p class="text-center">ค่า Fitness : {{ item[2].fitness }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import AOS from 'aos';
import 'aos/dist/aos.css';
export default {
    layout: 'default',
    data() {
        return {
            length: null,
            volume: null,
            functionValue: null,

            results: [],
            setPopulation: [],
            displayset: [],
            min2Sets: [],
            setMin1: [],
            setMin2: [],
            numbers: [],

            position : 0,

            SingleSet1: [],
            SingleFitness1: 0,
            SingleSet2: [],
            SingleFitness2: 0,
            TwoSet3: [],
            TwoFitness1: 0,
            TwoSet4: [],
            TwoFitness2: 0,

            crossoverSinglePoint: null,
            crossoverTwoPoint: null,

            result5: [],
            result6: []
        }
    },
    mounted() {
        AOS.init();
        this.get();
        this.runModel();
    },
    methods: {
        random_number(lengthOfList) {
            const numbers = Array.from({ length: lengthOfList }, (_, i) => i + 1);
            numbers.sort(() => Math.random() - 0.5);
            return numbers;
        },
        check_list(numbers) {
            const Cost = {
                1: { 1: 1, 2: 2, 3: 3, 4: 4, 5: 5, 6: 4, 7: 3, 8: 2, 9: 1 },
                2: { 1: 2, 2: 3, 3: 4, 4: 5, 5: 6, 6: 5, 7: 4, 8: 3, 9: 2 },
                3: { 1: 3, 2: 4, 3: 5, 4: 6, 5: 7, 6: 6, 7: 5, 8: 4, 9: 3 },
                4: { 1: 4, 2: 5, 3: 6, 4: 7, 5: 8, 6: 7, 7: 6, 8: 5, 9: 4 },
                5: { 1: 5, 2: 6, 3: 7, 4: 8, 5: 9, 6: 8, 7: 7, 8: 6, 9: 5 },
                6: { 1: 4, 2: 5, 3: 6, 4: 7, 5: 8, 6: 7, 7: 6, 8: 5, 9: 4 },
                7: { 1: 3, 2: 4, 3: 5, 4: 6, 5: 7, 6: 6, 7: 5, 8: 4, 9: 3 },
                8: { 1: 2, 2: 3, 3: 4, 4: 5, 5: 6, 6: 5, 7: 4, 8: 3, 9: 2 },
                9: { 1: 1, 2: 2, 3: 3, 4: 4, 5: 5, 6: 4, 7: 3, 8: 2, 9: 1 }
            };
            const fitness = numbers.slice(0, -1).reduce((acc, curr, i) => acc + Cost[curr][numbers[i + 1]], 0);
            return { fitness, numbers };
        },
        display(data) {
            const result = { "set": data[1], "fitness": data[2] } ;
            return result;
        },
        createJson(number, set) {
            console.log("jsonData : ",set);
            const fitness = this.check_list(set);
            // const jsonData = this.display([number, set, fitness.fitness]);
            return [number, set, fitness];
        },
        lessFitness2(data) {
            if (data && data.length > 0) {
                const min2Fitness = data.slice().sort((a, b) => a[Object.keys(a)[0]].fitness - b[Object.keys(b)[0]].fitness).slice(0, 2);
                min2Fitness.forEach(dataMin => {
                    console.log(dataMin);
                    this.min2Sets.push(dataMin.set);
                });
                console.log("Sets with the lowest fitness values:",this.min2Sets);
                return this.min2Sets;
            } else {
                console.log("No data available or invalid data format");
                return [];
            }
        },
        SinglePoint(set1, set2) {
            const temp = [...set1.slice(0, 2)];
            set1.splice(0, 2, ...set2.slice(0, 2));
            set2.splice(0, 2, ...temp);
            return [set1, set2];
        },
        TwoPoint(set1, set2) {
            const tempSet1 = [...set1.slice(0, 2)];
            const tempSet2 = [...set2.slice(0, 2)];
            
            // สลับส่วนแรกของ set1 และ set2
            set1.splice(0, 2, ...tempSet2);
            set2.splice(0, 2, ...tempSet1);
            
            // สลับส่วนท้ายของ set1 และ set2
            set1.splice(-2, 2, ...tempSet2);
            set2.splice(-2, 2, ...tempSet1);
            
            return [set1, set2];
        },
        mutation(arr) {
            if (arr.length >= 7) {
            [arr[2], arr[6]] = [arr[6], arr[2]];
            } else {
            console.log("Array ไม่มีขนาดเพียงพอสำหรับการสลับ");
            }
            return arr;
        },
        runModel() {
            for (let i = 0; i < this.volume; i++) {
                const numbers = this.random_number(this.length);
                const result = this.check_list(numbers);
                console.log(this.display([i, numbers, result.fitness]));
                const jsonData = this.display([i, numbers, result.fitness]);
                this.results.push(jsonData);
                this.setPopulation.push(numbers);
            }

            this.min2Sets = this.lessFitness2(this.results);
            this.setMin1 = this.min2Sets[0];
            this.setMin2 = this.min2Sets[1];

            console.log("this.setMin1 : ",this.setMin1)
            const Single = this.SinglePoint(this.setMin1,this.setMin2)
            const Two = this.TwoPoint(this.setMin1,this.setMin2)

            console.log("Single : ",Single)
            console.log("Two : ",Two)
            let sumSing = 0
            let sumTwo = 0

            Single.forEach(set => {
                sumSing += this.check_list(set)
            })

            Two.forEach(set => {
                sumTwo += this.check_list(set)
            })

            if (sumSing > sumTwo) {
                this.result5 = this.createJson(5 , Single[0])
                this.result6 = this.createJson(6 , Single[1])
            } else {
                this.result5 = this.createJson(5 , Single[0])
                this.result6 = this.createJson(6 , Single[1])
            }

            this.setPopulation[this.position] = this.mutation(this.result5[1])
            this.setPopulation[this.position + 1] = this.mutation(this.result6[1])

            this.position = 2

            while (this.position < this.volume) {
                console.log(this.position)
                for (let i = this.position;i < this.volume; i++) {
                    const numbers = this.random_number(9);
                    const result = this.check_list(numbers);
                    console.log("1 :",this.display([i, numbers, result.fitness]));
                    const jsonData = this.display([i, numbers, result.fitness]);
                    this.results[i] = jsonData;
                    console.log("numbers : ",this.position)
                    this.setPopulation[i] = numbers;
                    console.log("this.setPopulation : ",this.setPopulation)
                }

                this.min2Sets = this.lessFitness2(this.results);
                this.setMin1 = this.min2Sets[0];
                this.setMin2 = this.min2Sets[1];

                const Single = this.SinglePoint(this.setMin1,this.setMin2)
                const Two = this.TwoPoint(this.setMin1,this.setMin2)

                let sumSing = 0
                let sumTwo = 0

                Single.forEach(set => {
                    sumSing += this.check_list(set)
                })

                Two.forEach(set => {
                    sumTwo += this.check_list(set)
                })

                if (sumSing > sumTwo) {
                    this.result5 = this.createJson(5 , Single[0])
                    this.result6 = this.createJson(6 , Single[1])
                } else {
                    this.result5 = this.createJson(5 , Single[0])
                    this.result6 = this.createJson(6 , Single[1])
                }

                this.setPopulation[this.position] = this.mutation(this.result5[1])
                this.setPopulation[this.position + 1] = this.mutation(this.result6[1])

                this.position += 2;
            }
            let i = 0
            this.setPopulation.forEach(setData => {
                this.displayset.push(this.createJson(i,setData))
                console.log("--------------------------------",this.displayset)
                i++
            })
        },
        get() {
            this.length = localStorage.getItem("Langth");
            this.volume = localStorage.getItem("Volume");
            this.functionValue = localStorage.getItem("functionValue");
            console.log("set successfully!")
            if (!this.length || !this.volume || !this.functionValue) {
                window.location = 'set'
            }
        }
    }
}
</script>