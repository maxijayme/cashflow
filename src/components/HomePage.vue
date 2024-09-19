<template>
    <Layout>
        <template #header>
            <Header/>
        </template>
        <template #resume>
            <Resume 
                :total-label="'Ahorro total'"
                :label="label"
                :amount="amount"
                :totalAmount= totalAmount
            >
                <template #graphic>
                    <Graphic 
                        :amounts="amounts" 
                        @actualMovement="selectedMovement"
                        @pointerOut="resetMovement"
                    />
                </template>
                <template #action>
                    <Action @addMovement="addMovement">
                        <slot></slot>
                    </Action>
                </template>
            </Resume>
        </template>
        <template #movements>
            <Movements
                :movements="movements"
                @remove="removeMovement"
            />
        </template>
    </Layout>
</template>
<script>
    import Layout from './Layout.vue'
    import Header from './Header.vue'
    import Resume from './Resume/Index.vue'
    import Movements from './Movements/Index.vue'
    import Action from './Movements/Action.vue'
    import Graphic from './Resume/Graphic.vue'

    export default {
        components: {
            Layout,
            Header,
            Resume,
            Movements,
            Action,
            Graphic
        },
        data(){
            return {
                amount: null,
                label: null,
                movements: []
            }
        },
        mounted() {
            const movements = localStorage.getItem('movements')
            if (movements) {
                this.movements = JSON.parse(movements)?.map((x) => {
                    return {...x, time: new Date(x.time)}
                })
            }
        },
        computed: {
            amounts() {
                const lastDays = this.movements.filter((x) => {
                    const today = new Date();
                    const oldDate = today.setDate(today.getDate() - 30);
                    return x.time > oldDate;
                    })
                    .map((y) => y.amount);
                return lastDays.map((x, i) => {
                    const lastMovements = lastDays.slice(0, i);

                    return (
                    x +
                    lastMovements.reduce((suma, movement) => {
                        return suma + movement;
                    }, 0)
                    );
                });
            },
            totalAmount() {
                return this.movements?.reduce((suma, movement) => {
                    return suma + movement.amount;
                }, 0);
            }
        },
        methods: {
            addMovement(movement) {
                this.movements.push(movement)
                this.saveMovements()
            },
            removeMovement(id) {
                this.movements = this.movements.filter(movement => movement.id !== id)
                this.saveMovements()
            },
            saveMovements(){
                localStorage.setItem('movements', JSON.stringify(this.movements))
            },
            selectedMovement(index){
                this.amount = this.movements[index]?.amount
                this.label = this.movements[index]?.time.toLocaleDateString()
            },
            resetMovement(){
                this.amount = null
                this.label = null
            }
        }
    };
</script>