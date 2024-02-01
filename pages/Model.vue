
<template>
    <div>
        <div class="container pt-5">
            <div class="p-3"></div>
            <div  v-for="(item , index) in this.results" :key="index" class="row border my-2 border-2 p-2 gap-2 rounded-3">
                <p class="py-0 m-0">เซ็ตที่ {{ index + 1 }}</p>
                <div v-for="(i , index) in item.set" :key="index" class="col py-auto border border-2 rounded-pill">
                    <p class="text-center m-0 py-2">{{ i }}</p>
                </div>
                <div class="col-12 mt-2">
                    <p class="text-center">ค่า Fitness : {{ item.fitness }}</p>
                </div>
            </div>
            <div class="row text-center">
                <p class="p-0 m-0">
                    ---------------------------- Father & Mather --------------------------------
                </p>
            </div>
            <div v-for="(item , index) in this.min2Sets" :key="index" class="row border my-2 border-2 p-2 gap-2 rounded-3">
                <p class="py-0 m-0">เซ็ตที่น้อยเซ็ตที่ {{ index + 1 }}</p>
                <div v-for="(i , index) in item" :key="index" class="col py-auto border border-2 rounded-pill">
                    <p class="text-center m-0 py-2">{{ i }}</p>
                </div>
            </div>
            <div class="row text-center">
                <p class="p-0 m-0">
                    ----------------------------- Single Point -------------------------------
                </p>
            </div>
            <div class="row border my-2 border-2 p-2 gap-2 rounded-3" >
                <p class="py-0 m-0">เซ็ตที่น้อยเซ็ตที่ </p>
                <div v-for="(item , index) in this.SingleSet1" :key="index" class="col py-auto border border-2 rounded-pill">
                    <p class="text-center m-0 py-2">{{ item }}</p>
                </div>
            </div>
            <div class="row border my-2 border-2 p-2 gap-2 rounded-3" >
                <p class="py-0 m-0">เซ็ตที่น้อยเซ็ตที่ </p>
                <div v-for="(item , index) in this.SingleSet2" :key="index" class="col py-auto border border-2 rounded-pill">
                    <p class="text-center m-0 py-2">{{ item }}</p>
                </div>
            </div>
            <div class="row text-center">
                <p class="p-0 m-0">
                    ------------------------------- Two Point -----------------------------
                </p>
            </div>
            <div class="row border my-2 border-2 p-2 gap-2 rounded-3" >
                <p class="py-0 m-0">เซ็ตที่น้อยเซ็ตที่ </p>
                <div v-for="(item , index) in this.TwoSet3" :key="index" class="col py-auto border border-2 rounded-pill">
                    <p class="text-center m-0 py-2">{{ item }}</p>
                </div>
            </div>
            <div class="row border my-2 border-2 p-2 gap-2 rounded-3" >
                <p class="py-0 m-0">เซ็ตที่น้อยเซ็ตที่ </p>
                <div v-for="(item , index) in this.TwoSet4" :key="index" class="col py-auto border border-2 rounded-pill">
                    <p class="text-center m-0 py-2">{{ item }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    layout: 'default',
    data() {
        return {
            length: null, // แก้จาก Langth เป็น length
            volume: null, // แก้จาก Volume เป็น volume
            functionValue: null,

            results: [],
            setPopulation: [], // แก้จาก set_population เป็น setPopulation

            min2Sets: [], // แก้จาก min_2_sets เป็น min2Sets
            setMin1: [], // แก้จาก set_min_1 เป็น setMin1
            setMin2: [], // แก้จาก set_min_2 เป็น setMin2
            numbers: [],

            SingleSet1: [], // 
            SingleSet2: [], // 
            TwoSet3: [], //
            TwoSet4: [],

            crossoverSinglePoint : null,
            crossoverTwoPoint : null
        }
    },
    mounted() {
        this.get()
        this.runModel()
    },
    methods: {
        random_number(lengthOfList) {
            this.numbers = Array.from({ length: lengthOfList }, (_, i) => i + 1);
            this.numbers.sort(() => Math.random() - 0.5); // สลับลำดับของอินเด็กซ์ในอาร์เรย์
            return this.numbers;
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
        createJson(number, set) { // แก้จาก create_json เป็น createJson
            const fitness = this.check_list(set);
            const jsonData = this.display([number, set, fitness.fitness]); // แก้จาก json_data เป็น jsonData
            this.results.push(jsonData);
            console.log(jsonData);
            return [number, set, fitness];
        },
        lessFitness2(data) {
            if (data && Object.keys(data).length > 0) { // ตรวจสอบว่าข้อมูลใน data มีความยาวมากกว่า 0 หรือไม่
                const min2Fitness = data.slice().sort((a, b) => a[Object.keys(a)[0]].fitness - b[Object.keys(b)[0]].fitness).slice(0, 2);
                // const min2Sets = [min2Fitness[0][Object.keys(min2Fitness[0])[0]].set, min2Fitness[1][Object.keys(min2Fitness[1])[0]].set];
                console.log("2 sets with the lowest fitness values:");
                min2Fitness.forEach(dataMin => {
                    console.log(dataMin);
                    this.min2Sets.push(dataMin.set)
                })
                console.log("Sets with the lowest fitness values:");
                console.log(this.min2Sets);
                return this.min2Sets;
            } else {
                console.log("No data available or invalid data format"); // เพิ่มข้อความแจ้งเตือนเมื่อไม่พบข้อมูลหรือข้อมูลไม่ถูกต้อง
                return [];
            }
        },
        runModel() {
            for (let i = 0; i < this.volume; i++) {
                this.numbers = this.random_number(this.length);
                const result = this.check_list(this.numbers);
                console.log(this.display([i, this.numbers, result.fitness]));
                const jsonData = this.display([i, this.numbers, result.fitness]);
                this.results.push(jsonData);
                this.setPopulation.push(this.numbers);
            }
            this.min2Sets = this.lessFitness2(this.results); // แก้จาก min_2_sets เป็น min2Sets
            console.log("+++++++++++++++++",this.min2Sets)
            this.setMin1 = this.min2Sets[0]; // แก้จาก set_min_1 เป็น setMin1
            this.setMin2 = this.min2Sets[1]; // แก้จาก set_min_2 เป็น setMin2

            this.generateSets()

            const result1 = this.createJson(1, this.SingleSet1);
            const result2 = this.createJson(2, this.SingleSet2);
            const result3 = this.createJson(3, this.TwoSet3);
            const result4 = this.createJson(4, this.TwoSet4);

            const crossoverSinglePoint = Math.abs(result1[0].fitness + result2[0].fitness);
            console.log("ผลรวมของ Fitness Single Point :", crossoverSinglePoint);

            const crossoverTwoPoint = Math.abs(result3[0].fitness + result4[0].fitness);
            console.log("ผลรวมของ Fitness Two Point :", crossoverTwoPoint);

        },
        generateSingleSet(set1, set2) {
            return set1.slice(0, 3).concat(set2.slice(3));
        },
        generateTwoSet(set1, set2) {
            return set1.slice(0, 3).concat(set2.slice(3, 7), set1.slice(-2));
        },
        generateSets() {
            this.SingleSet1 = this.generateSingleSet(this.setMin1, this.setMin2);
            this.SingleSet2 = this.generateSingleSet(this.setMin2, this.setMin1);
            this.TwoSet3 = this.generateTwoSet(this.setMin1, this.setMin2);
            this.TwoSet4 = this.generateTwoSet(this.setMin2, this.setMin1);
        },
        get() {
            this.length = localStorage.getItem("Langth");
            this.volume = localStorage.getItem("Volume");
            this.functionValue = localStorage.getItem("functionValue");
            console.log("set successfully!")
            if (!this.Langth && !this.Volume && !this.functionValue) {
                window.location = 'set'
            }
        }
    }
}
</script>

