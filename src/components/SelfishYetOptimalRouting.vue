<template>
  <div id="selfish-yet-optimal-routing">
    <v-card-title primary-title>
      <h3 class="headline mb-0">Selfish yet optimal routing</h3>
    </v-card-title>
    <v-img>
      <div class="selfish-yet-optimal-routing" style="text-align: center">
      </div>
    </v-img>
    <v-container grid-list-md text-xs-center>
      <v-layout>
        <v-flex xs12 md4>
          <v-text-field v-model="flow_link1" label="flow of link1, e.g., [0,1]" type="number" step="0.01" min="0" max="1" @click="getFlowLink2" @keydown="getFlowLink2" value="getFlowLink1() | round"></v-text-field>
        </v-flex>
        <v-flex xs12 md4>
          <v-text-field v-model="flow_link2" label="flow of link2, e.g., [0,1]" type="number" step="0.01" min="0" max="1" @click="getFlowLink1" @keydown="getFlowLink1" value="getFlowLink2() | round"></v-text-field>
        </v-flex>
        <v-flex xs12 md4>
          <v-btn depressed large @click="clear">Clear</v-btn>
        </v-flex>
      </v-layout>
    </v-container>
    <v-card-text class="headline font-weight-bold">
      <p>Avg. travel cost: {{ getTotalCost() | round }}</p>
      <p>Avg. estimated cost: {{ getTotalEstimatedCost() | round }}</p>
      <p v-if="(flow_link1 === 0.5) && (flow_link2 === 0.5)">User equilibrium (Wardrop equilibrium under users' selfish criteria)</p>
      <p v-if="(flow_link1 === 0.5) && (flow_link2 === 0.5)">Social optimum (Wardrop equilibrium under users' selfish criteria)</p>
    </v-card-text>
  </div>
</template>

<script>
import * as d3 from 'd3'
export default {
  name: 'SelfishYerOptimalRouting',
  data () {
    return {
      total_flow: 1.0,
      flow_link1: 1.0,
      flow_link2: 0,
      fictitious_flow: 0.5,
      nodes: [
        {
          cx: 50,
          cy: 50,
          r: 15
        },
        {
          cx: 200,
          cy: 50,
          r: 15
        }
      ],
      links: [
        [
          [50, 50],
          [100, 30],
          [150, 30],
          [200, 50]
        ],
        [
          [50, 50],
          [100, 75],
          [150, 75],
          [200, 50]
        ]
      ]
    }
  },
  components: {
  },
  methods: {
    getFlowLink1 () {
      const getLabel = (i) => {
        if (i === 1) {
          return 'link1 (t = flow)'
        }
        return 'link2 (t = 1)'
      }
      const getOpacity = (i) => {
        if (i === 1) {
          return this.flow_link1 + this.fictitious_flow
        }
        return this.flow_link2
      }
      this.flow_link1 = this.total_flow - this.flow_link2
      d3.selectAll('.selfish-yet-optimal-routing svg').remove()
      var svg = d3.selectAll('.selfish-yet-optimal-routing').append('svg')
      var g = svg.append('g')
      var lineGenerator = d3.line().curve(d3.curveCardinal)

      g.selectAll('path')
        .data(this.links)
        .enter()
        .append('path')
        .attr('d', (d) => lineGenerator(d))
        .attr('stroke', 'black')
        .attr('stroke-width', '3px')
        .attr('class', 'link')

      g = svg.append('g')
      g.selectAll('path')
        .data(this.links)
        .enter()
        .append('path')
        .attr('d', (d) => lineGenerator(d))
        .attr('stroke', 'red')
        .attr('opacity', (d, i) => getOpacity(i + 1))
        .attr('stroke-width', '3px')
        .attr('class', 'flow')

      g.selectAll('text')
        .data(this.links)
        .enter()
        .append('text')
        .attr('x', (d) => {
          return d[1][0]
        })
        .attr('y', (d) => {
          return d[1][1] - 15
        })
        .attr('class', 'edgelabel')
        .text((d, i) => getLabel(i + 1))
      g.selectAll('circle')
        .data(this.nodes)
        .enter()
        .append('circle')
        .attr('r', (d) => { return d.r })
        .attr('cx', (d) => { return d.cx })
        .attr('cy', (d) => { return d.cy })
        .attr('fill', 'white')
        .attr('class', 'node')
        .attr('stroke', 'black')
        .attr('stroke-width', '3px')
      return this.flow_link1
    },
    getFlowLink2 () {
      const getLabel = (i) => {
        if (i === 1) {
          return 'link1 (t = flow)'
        }
        return 'link2 (t = 1)'
      }
      const getOpacity = (i) => {
        if (i === 1) {
          return this.flow_link1 + this.fictitious_flow
        }
        return this.flow_link2
      }
      this.flow_link2 = this.total_flow - this.flow_link1
      d3.selectAll('.selfish-yet-optimal-routing svg').remove()
      var svg = d3.selectAll('.selfish-yet-optimal-routing').append('svg')
      var g = svg.append('g')
      var lineGenerator = d3.line().curve(d3.curveCardinal)

      g.selectAll('path')
        .data(this.links)
        .enter()
        .append('path')
        .attr('d', (d) => lineGenerator(d))
        .attr('stroke', 'black')
        .attr('stroke-width', '3px')
        .attr('class', 'link')

      g = svg.append('g')
      g.selectAll('path')
        .data(this.links)
        .enter()
        .append('path')
        .attr('d', (d) => lineGenerator(d))
        .attr('stroke', 'red')
        .attr('opacity', (d, i) => getOpacity(i + 1))
        .attr('stroke-width', '3px')
        .attr('class', 'flow')

      g.selectAll('text')
        .data(this.links)
        .enter()
        .append('text')
        .attr('x', (d) => {
          return d[1][0]
        })
        .attr('y', (d) => {
          return d[1][1] - 15
        })
        .attr('class', 'edgelabel')
        .text((d, i) => getLabel(i + 1))

      g.selectAll('circle')
        .data(this.nodes)
        .enter()
        .append('circle')
        .attr('r', (d) => { return d.r })
        .attr('cx', (d) => { return d.cx })
        .attr('cy', (d) => { return d.cy })
        .attr('fill', 'white')
        .attr('class', 'node')
        .attr('stroke', 'black')
        .attr('stroke-width', '3px')
      return this.flow_link2
    },
    getTotalCost () {
      return this.flow_link1 * this.flow_link1 + this.flow_link2 * 1
    },
    getTotalEstimatedCost () {
      var flow = parseFloat(this.flow_link1) + parseFloat(this.fictitious_flow)
      return this.flow_link1 * flow + this.flow_link2 * 1
    },
    clear () {
      this.flow_link1 = 1.0
      this.flow_link2 = 0.0
    }
  },
  filters: {
    round: function (val) {
      return Math.round(val * 100) / 100
    }
  },
  mounted () {
    const getLabel = (i) => {
      if (i === 1) {
        return 'link1 (t = flow)'
      }
      return 'link2 (t = 1)'
    }
    const getOpacity = (i) => {
      if (i === 1) {
        return this.flow_link1 + this.fictitious_flow
      }
      return this.flow_link2
    }
    var svg = d3.select('.selfish-yet-optimal-routing').append('svg')
    var g = svg.append('g')
    var lineGenerator = d3.line().curve(d3.curveCardinal)

    g.selectAll('path')
      .data(this.links)
      .enter()
      .append('path')
      .attr('d', (d) => lineGenerator(d))
      .attr('stroke', 'black')
      .attr('stroke-width', '3px')
      .attr('class', 'link')

    g = svg.append('g')

    g.selectAll('path')
      .data(this.links)
      .enter()
      .append('path')
      .attr('d', (d) => lineGenerator(d))
      .attr('stroke', 'red')
      .attr('opacity', (d, i) => getOpacity(i + 1))
      .attr('stroke-width', '3px')
      .attr('class', 'flow')

    g.selectAll('text')
      .data(this.links)
      .enter()
      .append('text')
      .attr('x', (d) => {
        return d[1][0]
      })
      .attr('y', (d) => {
        return d[1][1] - 15
      })
      .attr('class', 'edgelabel')
      .text((d, i) => getLabel(i + 1))

    g.selectAll('circle')
      .data(this.nodes)
      .enter()
      .append('circle')
      .attr('r', (d) => { return d.r })
      .attr('cx', (d) => { return d.cx })
      .attr('cy', (d) => { return d.cy })
      .attr('fill', 'white')
      .attr('class', 'node')
      .attr('stroke', 'black')
      .attr('stroke-width', '3px')
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
