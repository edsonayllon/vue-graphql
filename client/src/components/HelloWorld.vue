<template>
  <div class="hello">
    <div v-for="factory in uniswapFactories" :key="factory.id">
      {{ factory.id }}
      {{ factory.totalVolumeUSD }}
    </div>
    <ApolloQuery :query="require('../graphql/tokenDayDatas.gql')" :variables="{ first }"> 
      <template v-slot="{ result: { loading, error, data} }">
        <div v-if="data">
          <div v-for="tokenDay in data.tokenDayDatas" :key="tokenDay.id">
            {{ tokenDay.token.symbol }}
          </div>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
import gql from "graphql-tag"

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  apollo: {
    uniswapFactories: gql`
      query {
        uniswapFactories (first:1) {
          id,
          totalVolumeUSD
        }
      }
    `
  },
  data () {
    return {
      first: 5
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
