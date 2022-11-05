<template>
  <div class="container mt-3">
    <div class="card">
      <div class="card-header"><h1 class="card-title text-align-start">SVG star Generator</h1></div>
      <div class="card-body">
        <div class="row">
          <div class="col-auto">
          <label>
            Point count
            <input type="number" class="form-control" min="3" v-model="count" />
          </label>
          </div>
          <div class="col-auto">
          <label>
            Radius of outer points
            <input type="number" class="form-control" :min="innerRadius" v-model="outerRadius" />
          </label>
          </div>
          <div class="col-auto">
          <label>
            Radius of inner points
            <input type="number" class="form-control" min="1" v-model="innerRadius" />
          </label>
          </div>
        </div>
        <div class="row">
          <div class="col p-5">
            <div class="table-responsive">
            <table class="table table-striped-columns text-center">
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
            </div>
          </div>
        </div>
        <div class="row mb-2">
          <div class="col text-center">&lt;polygon points="{{this.getPoints()}}" /&gt;</div>
        </div>
        <div class="row">
          <div class="col text-center"><img :src="this.getData()" alt="star image" class="img-fluid" :width="this.outerRadius * 2" :height="this.outerRadius * 2"/></div>
        </div>
      </div>
    </div>
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
    },
    getData() {
      return "data:image/svg+xml, " + `<svg xmlns='http://www.w3.org/2000/svg' viewbox="0 0 ${ this.outerRadius * 2 } ${ this.outerRadius * 2 }"><polygon points="${ this.getPoints() }" /></svg>`
    }
  }
}
</script>
 
<style>
.inputs {
  display: grid;
  grid-auto-flow: column;
  grid-column-gap: 1rem;
}
</style>
 

