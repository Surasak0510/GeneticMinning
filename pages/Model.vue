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
            <div class="row text-center">
                <p class="p-0 m-0 py-2">
                    ---------------------------- Father & Mather --------------------------------
                </p>
            </div>
            <div  v-for="(item , indexOfMin2Set) in min2Sets" :key="indexOfMin2Set" data-aos="fade-up" data-aos-duration="1500"  class="row border my-2 border-2 p-2 gap-2 rounded-3">
                <p class="py-0 m-0">เซ็ตที่น้อยเซ็ตที่ {{ indexOfMin2Set + 1 }}</p>
                <div v-for="(i , indexOfItem) in item" :key="indexOfItem" class="col py-auto border border-2 rounded-pill">
                    <p class="text-center m-0 py-2">{{ i }}</p>
                </div>
            </div>
            <div class="row text-center">
                <p class="p-0 m-0 py-2">
                    ----------------------------- Single Point -------------------------------
                </p>
            </div>
            <div data-aos="fade-up" data-aos-duration="1500" class="row border my-2 border-2 p-2 gap-2 rounded-3" >
                <p class="py-0 m-0">Single Point 1</p>
                <div v-for="(item , index) in SingleSet1" :key="index" class="col py-auto border border-2 rounded-pill">
                    <p class="text-center m-0 py-2">{{ item }}</p>
                </div>
                <div class="col-12 mt-2">
                    <p class="text-center">ค่า Fitness : {{ SingleFitness1.fitness }}</p>
                </div>
            </div>
            <div data-aos="fade-up" data-aos-duration="1500" class="row border my-2 border-2 p-2 gap-2 rounded-3" >
                <p class="py-0 m-0">Single Point 2</p>
                <div v-for="(item , index) in SingleSet2" :key="index" class="col py-auto border border-2 rounded-pill">
                    <p class="text-center m-0 py-2">{{ item }}</p>
                </div>
                <div class="col-12 mt-2">
                    <p class="text-center">ค่า Fitness : {{ SingleFitness2.fitness }}</p>
                </div>
            </div>
            <div class="row text-center">
                <p class="p-0 m-0 py-2">
                    ------------------------------- Two Point -----------------------------
                </p>
            </div>
            <div data-aos="fade-up" data-aos-duration="1500" class="row border my-2 border-2 p-2 gap-2 rounded-3" >
                <p class="py-0 m-0">Two Point 1</p>
                <div v-for="(item , index) in TwoSet3" :key="index" class="col py-auto border border-2 rounded-pill">
                    <p class="text-center m-0 py-2">{{ item }}</p>
                </div>
                <div class="col-12 mt-2">
                    <p class="text-center">ค่า Fitness : {{ TwoFitness1.fitness }}</p>
                </div>
            </div>
            <div data-aos="fade-up" data-aos-duration="1500" class="row border my-2 border-2 p-2 gap-2 rounded-3" >
                <p class="py-0 m-0">Two Point 2</p>
                <div v-for="(item , index) in TwoSet4" :key="index" class="col py-auto border border-2 rounded-pill">
                    <p class="text-center m-0 py-2">{{ item }}</p>
                </div>
                <div class="col-12 mt-2">
                    <p class="text-center">ค่า Fitness : {{ TwoFitness2.fitness }}</p>
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
            min2Sets: [],
            setMin1: [],
            setMin2: [],
            numbers: [],

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
            const result = { set: data[1], fitness: data[2] } ;
            return result;
        },
        createJson(number, set) {
            const fitness = this.check_list(set);
            const jsonData = this.display([number, set, fitness.fitness]);
            console.log(jsonData);
            return [number, set, fitness];
        },
        lessFitness2(data) {
            if (data && data.length > 0) {
                const min2Fitness = data.slice().sort((a, b) => a[Object.keys(a)[0]].fitness - b[Object.keys(b)[0]].fitness).slice(0, 2);
                min2Fitness.forEach(dataMin => {
                    console.log(dataMin);
                    this.min2Sets.push(dataMin.set);
                });
                console.log("Sets with the lowest fitness values:");
                console.log(this.min2Sets);
                return this.min2Sets;
            } else {
                console.log("No data available or invalid data format");
                return [];
            }
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
            console.log("+++++++++++++++++", this.min2Sets);
            this.setMin1 = this.min2Sets[0];
            this.setMin2 = this.min2Sets[1];

            this.generateSets();

            const result1 = this.createJson(1, this.SingleSet1);
            const result2 = this.createJson(2, this.SingleSet2);
            const result3 = this.createJson(3, this.TwoSet3);
            const result4 = this.createJson(4, this.TwoSet4);

            this.crossoverSinglePoint = Math.abs(result1[2].fitness + result2[2].fitness);
            console.log("ผลรวมของ Fitness Single Point :", this.crossoverSinglePoint);

            this.crossoverTwoPoint = Math.abs(result3[2].fitness + result4[2].fitness);
            console.log("ผลรวมของ Fitness Two Point :", this.crossoverTwoPoint);

            if (this.crossoverSinglePoint < this.crossoverTwoPoint) {
                this.Offspring1 = this.SingleSet1;
                this.Offspring2 = this.SingleSet2;
            } else {
                this.Offspring1 = this.TwoSet3;
                this.Offspring2 = this.TwoSet4;
            }

            [this.Offspring1[2], this.Offspring1[6]] = [this.Offspring1[6], this.Offspring1[2]];
            [this.Offspring2[2], this.Offspring2[6]] = [this.Offspring2[6], this.Offspring2[2]];

            this.result5 = this.createJson(5, this.Offspring1);
            this.result6 = this.createJson(6, this.Offspring2);
        },
        generateSingleSet(set1, set2) {
            return set1.slice(0, 3).concat(set2.slice(3));
        },
        generateTwoSet(set1, set2) {
            return set1.slice(0, 3).concat(set2.slice(3, 7), set1.slice(-2));
        },
        generateSets() {
            this.SingleSet1 = this.generateSingleSet(this.setMin1, this.setMin2);
            this.SingleFitness1 = this.check_list(this.SingleSet1);

            this.SingleSet2 = this.generateSingleSet(this.setMin2, this.setMin1);
            this.SingleFitness2 = this.check_list(this.SingleSet2);

            this.TwoSet3 = this.generateTwoSet(this.setMin1, this.setMin2);
            this.TwoFitness1 = this.check_list(this.TwoSet3);
            
            this.TwoSet4 = this.generateTwoSet(this.setMin2, this.setMin1);
            this.TwoFitness2 = this.check_list(this.TwoSet4);
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
