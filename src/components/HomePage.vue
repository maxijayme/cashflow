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
                movements: [
                    {
                    id: 1,
                    title: "Movimiento 1",
                    description: "Deposito de salario",
                    amount: -200,
                    time: new Date("09/02/2024"),
                    },
                    {
                    id: 2,
                    title: "Movimiento 2",
                    description: "Deposito de honorarios",
                    amount: 500,
                    time: new Date("08/21/2024"),
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
                    amount: -1000,
                    time: new Date("08/21/2024"),
                    }
                ]
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
            }
        },
        methods: {
            addMovement(movement) {
                this.movements.push(movement)
            },
            removeMovement(id) {
                this.movements = this.movements.filter(movement => movement.id !== id)
            }
        }
    };
</script>