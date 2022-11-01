<template>
  <div class="p-5">
    <div class="inputs">
      <label>
        Point count
        <input type="number" class="form-control" min="3" v-model="count" />
      </label>
      <label>
        Radius of outer points
        <input type="number" class="form-control" :min="innerRadius" v-model="outerRadius" />
      </label>
      <label>
        Radius of inner points
        <input type="number" class="form-control" min="1" v-model="innerRadius" />
      </label>
    </div>
    <table class="table">
      <thead>
        <tr>
          <th>Stop</th>
          <th>Outer X</th>
          <th>Outer Y</th>
          <th>Inner X</th>
          <th>Inner Y</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="n in this.count" :key="n">
          <td>{{n}}</td>
          <td>{{ this.calculateX(n-1, this.outerRadius, false) }}</td>
          <td>{{ this.calculateY(n-1, this.outerRadius, false) }}</td>
          <td>{{ this.calculateX(n, this.innerRadius, true) }}</td>
          <td>{{ this.calculateY(n, this.innerRadius, true) }}</td>
        </tr>
      </tbody>
    </table>
    <div>
      &lt;polygon points="{{this.getPoints()}}" /&gt;
    </div>
    <svg :viewbox="`0 0 ${this.outerRadius * 2} ${this.outerRadius * 2}`">
      <polygon :points="this.getPoints()" />
    </svg>
  </div>
</template>
 
<script>
export default {
  name: 'App',
  data() {
    return {
      count: 5,
      outerRadius: 25,
      innerRadius: 15,
    }
  },
  methods: {
    calculateX(index, radius, shift) {
      return radius * Math.cos(2 * Math.PI * index / this.count + this.getOffset(shift)) + this.outerRadius
    },
    calculateY(index, radius, shift) {
      return radius * Math.sin(2 * Math.PI * index / this.count + this.getOffset(shift)) + this.outerRadius
    },
    getOffset(shifted) {
      return this.count % 2 === 0
        ? (shifted ? 1 / (this.count / 2) + 1 : 3) * Math.PI / 2
        : (shifted ? 1 : 3) * Math.PI / 2;
    },
    getPoints() {
      var result = ""

      for (let i = 0; i < this.count; i++) {
        const outX = this.calculateX(i, this.outerRadius, false).toFixed(2)
        const outY = this.calculateY(i, this.outerRadius, false).toFixed(2)
        const inX = this.calculateX((i + Math.ceil(this.count / 2)) % this.count, this.innerRadius, true).toFixed(2)
        const inY = this.calculateY((i + Math.ceil(this.count / 2)) % this.count, this.innerRadius, true).toFixed(2)

        result = `${result} ${outX},${outY} ${inX},${inY}`
      }

      return result;
    }
  }
}
</script>
 
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.inputs {
  display: grid;
  grid-auto-flow: column;
  grid-column-gap: 1rem;
}
</style>
 

