<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-toolbar-title>Biologist Calculator</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <v-container>
        <v-card>
          <v-container>
            <div class="text-h2">Cell</div>
            <div class="text-h6">Culture Concentration</div>
            <div class="text-body-1">
              Enter cell count result:
              <v-text-field type="number" v-model="cellCount" suffix="cells" />
            </div>
            <div class="text-body-1">
              Cell concentration =
              <span>{{ stockConcentration }}</span>
              cells/mL
            </div>
            <div class="text-body-1">
              Enter desired concentration:
              <v-text-field
                type="number"
                v-model="desiredCellsPerML"
                suffix="cells/mL"
              />
            </div>
            <div class="text-body-1">
              Enter desired concentration:
              <v-text-field
                type="number"
                v-model="desiredVolumeML"
                suffix="mL"
              />
            </div>
            <div class="text-h6">
              Required stock volume: {{ requiredStockVolume }} μL
            </div>
            <div class="text-h6">
              Required buffer volume: {{ requiredBufferVolume }} μL
            </div>
          </v-container>
        </v-card>
        <div class="my-2"></div>
        <v-card>
          <v-container>
            <div class="text-h2">Antibody</div>
            <div class="text-body-1">
              AB1
              <v-select
                :items="antibodySelectData"
                v-model="ab1Name"
              ></v-select>
            </div>
            <div class="text-body-1">
              AB2
              <v-select
                :items="antibodySelectData"
                v-model="ab2Name"
              ></v-select>
            </div>
            <div class="text-body-1">
              {{ ab1Name }} stock concentration

              <v-text-field
                type="number"
                suffix="mg/mL"
                v-model="ab1StockConcPerML"
              ></v-text-field>
            </div>
            <div class="text-body-1">
              {{ ab2Name }} stock concentration

              <v-text-field
                type="number"
                suffix="mg/mL"
                v-model="ab2StockConcPerML"
              ></v-text-field>
            </div>
            <div class="text-body-1">
              {{ ab1Name }} desired concentration

              <v-text-field
                type="number"
                suffix="μg/mL"
                v-model="ab1DesiredConcPerML"
              ></v-text-field>
            </div>
            <div class="text-body-1">
              {{ ab2Name }} desired concentration

              <v-text-field
                type="number"
                suffix="μg/mL"
                v-model="ab2DesiredConcPerML"
              ></v-text-field>
            </div>
            <div class="text-body-1">
              Total volume

              <v-text-field
                type="number"
                suffix="μL"
                v-model="abTotalVolume"
              ></v-text-field>
            </div>
            <div class="d-flex justify-space-around">
              <div class="text-h6 text-center">
                <div>{{ ab1Name }}</div>
                <div>{{ ab1Dilution }}</div>
                <div>{{ requiredAb1Volume }}μl</div>
              </div>
              <div class="text-h6 text-center">
                <div>{{ ab2Name }}</div>
                <div>{{ ab2Dilution }}</div>
                <div>{{ requiredAb2Volume }}μl</div>
              </div>
              <div class="text-h6 text-center">
                <div>Diluent</div>
                <div>N/A</div>
                <div>{{ requiredDiluentVolume }}μl</div>
              </div>
            </div>
          </v-container>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'App',

  components: {},

  data: () => ({
    cellCount: 0,
    desiredCellsPerML: 50000,
    desiredVolumeML: 1,
    ab1StockConcPerML: 1,
    ab2StockConcPerML: 1,
    ab1DesiredConcPerML: 50,
    ab2DesiredConcPerML: 50,
    abTotalVolume: 80,
    ab1Name: 'beta-tublin',
    ab2Name: 'beta-tublin',
    antibodySelectData: ['beta-tublin', 'F4/80'],
  }),
  computed: {
    stockConcentration() {
      return this.cellCount * (10 / 6) * 10000;
    },
    requiredStockVolume() {
      if (this.cellCount) {
        return (
          ((this.desiredCellsPerML * this.desiredVolumeML) /
            this.stockConcentration) *
          1000
        );
      } else return 0;
    },
    requiredBufferVolume() {
      return this.desiredVolumeML * 1000 - this.requiredStockVolume;
    },
    requiredAb1Volume() {
      return (
        (this.abTotalVolume * this.ab1DesiredConcPerML) /
        (this.ab1StockConcPerML * 1000)
      );
    },
    requiredAb2Volume() {
      return (
        (this.abTotalVolume * this.ab2DesiredConcPerML) /
        (this.ab2StockConcPerML * 1000)
      );
    },
    requiredDiluentVolume() {
      return (
        this.abTotalVolume - this.requiredAb1Volume - this.requiredAb2Volume
      );
    },
    ab1Dilution() {
      return `1:${this.abTotalVolume / this.requiredAb1Volume}`;
    },
    ab2Dilution() {
      return `1:${this.abTotalVolume / this.requiredAb2Volume}`;
    },
  },
};
</script>
