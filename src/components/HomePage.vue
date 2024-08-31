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
                    graphic
                </template>
                <template #action>
                    actions
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

    export default {
        components: {
            Layout,
            Header,
            Resume,
            Movements
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
                    amount: "1000",
                    },
                    {
                    id: 2,
                    title: "Movimiento 2",
                    description: "Deposito de honorarios",
                    amount: "500",
                    },
                    {
                    id: 3,
                    title: "Movimiento 3",
                    description: "Comida",
                    amount: "-100",
                    },
                    {
                    id: 4,
                    title: "Movimiento 4",
                    description: "Colegiatura",
                    amount: "1000",
                    }
                ]
            }
        }
};
</script>