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
                :totalAmount= 100000
            >
                <template #graphic>
                    <Graphic :amounts="amounts"/>
                </template>
                <template #action>
                    <Action>
                        <slot></slot>
                    </Action>
                </template>
            </Resume>
        </template>
        <template #movements>
            <Movements
                :movements="movements"
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
                movements: [
                    {
                    id: 1,
                    title: "Movimiento 1",
                    description: "Deposito de salario",
                    amount: 1000,
                    time: new Date("09/02/2024"),
                    },
                    {
                    id: 2,
                    title: "Movimiento 2",
                    description: "Deposito de honorarios",
                    amount: 500,
                    time: new Date("08/21/02024"),
                    },
                    {
                    id: 3,
                    title: "Movimiento 3",
                    description: "Comida",
                    amount: -100,
                    time: new Date("08/30/2024"),
                    },
                    {
                    id: 4,
                    title: "Movimiento 4",
                    description: "Colegiatura",
                    amount: 1000,
                    time: new Date("02/08/2024"),
                    }
                ]
            }
        },
        computed: {
            amounts() {
                const lastDays = this.movements
                    .filter( movement => {
                        const today = new Date()
                        const oldDate = new Date(today.setDate(today.getDate() - 30))
                        console.log( movement.time)
                        return movement.time > oldDate
                    })
                    .map( movement => movement.amount )
                console.log(lastDays)

                return lastDays.map((m, i) => {
                    const lastMovements = lastDays.slice(0, i)

                    return lastMovements.reduce((suma, movement) => {
                        return suma + movement
                    }, 0);
                });
            }
        }
    };
</script>