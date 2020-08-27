<template>
  <v-form v-model="valid">
    <v-container>
      <v-card class="mx-auto pd-20">
        <v-row>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="revenus"
              :rules="numberRules"
              label="Revenus (avant impot)"
              suffix="€"
              @focus="focus='revenus'"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="12" md="4">
            <v-text-field v-model="incomeTax" label="Impôt" suffix="€" :disabled="true"></v-text-field>
          </v-col>

          <v-col cols="12" md="4">
            <v-text-field
              v-model="incomeNet"
              :rules="numberRules"
              @focus="focus='incomeNet'"
              label="Revenus (après impôts)"
              suffix="€"
            ></v-text-field>
          </v-col>

          <v-col cols="5" md="5">
            <v-checkbox v-model="maried" :label="`Vous êtes marié?`"></v-checkbox>
          </v-col>

          <v-col cols="7" md="7">
            <v-text-field v-model="nbKids" :rules="numberRules" label="Nombre d’enfant(s)"></v-text-field>
          </v-col>

          <!--<v-col cols="12" md="12">
            <v-btn class="mr-4" @click="calculate">Calculer votre impôt</v-btn>
          </v-col>-->
          <v-col cols="12" md="4">
            <v-text-field v-model="focus" label="Focus" :disabled="true"></v-text-field>
          </v-col>
        </v-row>
      </v-card>
    </v-container>
  </v-form>
</template>

<script>
export default {
  name: "CalculatorImpot",
  data: () => ({
    focus: "",
    valid: false,
    taxlevel: [
      { max: 10065, level: 0 },
      { max: 15595, level: 11 },
      { max: 47710, level: 30 },
      { max: 84437, level: 41 },
      { max: Infinity, coef: 45 }
    ],
    revenus: "",
    incomeNet: "",
    incomeTax: "",
    maried: false,
    nbKids: 0,
    numberRules: [v => Number(v) == v || "Ce champ doit contenir un nombre."]
  }),

  watch: {
    revenus() {
      this.debouncedCalculate();
    },
    incomeNet() {
      this.debouncedCalculate();
    },
    maried() {
      this.debouncedCalculate();
    },
    nbKids() {
      this.debouncedCalculate();
    }
  },

  created: function() {
    // _.debounce is a function provided by lodash to limit how
    // often a particularly expensive operation can be run.
    // In this case, we want to limit how often we access
    // yesno.wtf/api, waiting until the user has completely
    // finished typing before making the ajax request. To learn
    // more about the _.debounce function (and its cousin
    // _.throttle), visit: https://lodash.com/docs#debounce
    this.debouncedCalculate = _.debounce(this.calculate, 100);
  },

  computed: {
    part() {
      // Nb de part.
      let part = 1;
      if (this.maried) {
        part++;
      }
      if (parseInt(this.nbKids, 0) > 0) {
        // Si nbEnfant > 0
        part += this.nbKids / 2;
      }

      return part;
    }
  },

  methods: {
    calculate() {
      //console.log("calculate impot");

      if (
        this.focus === "revenus" &&
        (this.revenus && this.revenus !== "" && !isNaN(this.revenus))
      ) {
        this.calculateIncomeFromRevenus();
      }

      if (
        this.focus === "incomeNet" &&
        (this.incomeNet && this.incomeNet !== "" && !isNaN(this.incomeNet))
      ) {
        this.calculateRevenusFromIncomeNet();
      }
    },

    calculateIncomeFromRevenus() {
      console.log("calculateIncomeFromRevenus");
    },

    calculateRevenusFromIncomeNet() {
      console.log("calculateRevenusFromIncomeNet");
    }
  }
};
</script>

<style scoped>
.pd-20 {
  padding: 20px;
}
</style>
