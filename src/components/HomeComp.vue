<template>
  <LayoutComp>
    <template #header>
      <HeaderComp></HeaderComp>
    </template>
    <template #resume>
      <Resume
        :totalLabel="'Ahorro total'" 
        :label="label" 
        :totalAmount="totalAmount"
        :amount="amount"
        >
        <template #graphic>
          <GraphicComp 
            :amounts="amounts" 
            @select="select"></GraphicComp>
        </template>
        <template #action>
          <ActionComp @create="create"></ActionComp>
        </template>
      </Resume>
    </template>
    <template #movements>
      <Movements 
        :movements="movements"
        @remove="remove"></Movements>
    </template>
  </LayoutComp>
</template>

<script>
import HeaderComp from "./HeaderComp.vue";
import LayoutComp from "./LayoutComp.vue";
import Resume from "./Resume/IndexComp.vue";
import Movements from "./Movements/IndexComp.vue";
import ActionComp from "./ActionComp.vue";
import GraphicComp from "./Resume/GraphicComp.vue";

export default {
    components:{
    HeaderComp,
    LayoutComp,
    Resume,
    Movements,
    ActionComp,
    GraphicComp
},
    data() {
      return {
        amount: null,
        label: null,
        movements: []
      }
    },
    computed: {
      amounts() {
        const lastDays = this.movements.filter(m => {
          const today = new Date();
          const oldDate = today.setDate(today.getDate() - 30);

          return m.time > oldDate;
        }).map(m => m.amount);

        return lastDays.map((m,i) => {
          const lastMovements = lastDays.slice(0,i+1);

          return lastMovements.reduce((suma, movement) => {
            return suma + movement;
          }, 0);
        })
      },
      totalAmount() {
        return this.movements.reduce((suma, m) => suma + m.amount, 0)
      }
    },
    mounted() {
      const movements = JSON.parse(localStorage.getItem("movements"));
      if (Array.isArray(movements)) {
        this.movements = movements.map(m => {
        return {...m, time: new Date(m.time)}
      });
      }
      
    },
    methods: {
      create(movement) {
        this.movements.push(movement);
        this.save();
      },
      remove(id) {
        const index = this.movements.findIndex(m => m.id === id);
        this.movements.splice(index, 1);
        this.save();
      },
      save() {
        localStorage.setItem("movements", JSON.stringify(this.movements))
      },
      select(el) {
        this.amount = el;
      }
    }

}
</script>