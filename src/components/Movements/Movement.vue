<template>
    <div class="movement">
        <div class="content"> 
            <h4>{{ title }}</h4>
            <p>{{ description }}</p>
        </div>
        <div class="action">
            <img src="../../assets/trash-icon.svg" alt="Borrar" @click="remove"/>
            <p :class="{ 'red': isNegative, 'green': !isNegative }"> {{ amountCurrency }}</p>
        </div>
    </div>
</template>

<script setup>
    import { toRefs, defineProps, computed, defineEmits } from 'vue';

    const props = defineProps({
        id: {
            type: Number,
            required: true
        },
        title: {
            type: String,
            required: true
        },
        description: {
            type: String,
            required: true
        },
        amount: {
            type: Number,
            required: true
        }
    });

    const { id, title, description, amount } = toRefs(props);
    const emit = defineEmits(['remove']);
    const remove =  () => {
       emit('remove', id.value);
    }
    const currencyFormatter = new Intl.NumberFormat("es-ES", {
        style: "currency",
        currency: "EUR",
    });
    const amountCurrency = computed(()=>{
        return currencyFormatter.format(amount.value)
    })
    const isNegative = computed(()=>{
        return amount.value < 0;
    })
</script>
<style scoped>
    .movement {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px;
  background-color: #e6f9ff;
  border-radius: 8px;
  box-sizing: border-box;
}
.movement .content {
  width: 100%;
}
.movement .action {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-direction: column;
}
h4,
p {
  margin: 0;
  padding: 0;
}
h4 {
  margin-bottom: 8px;
}
.movement .action img {
  margin-bottom: 16px;
  cursor:pointer;
}
.red {
    color: red;
}
.green {
    color: green;
}
</style>