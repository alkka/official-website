<template>
  <div class="container">
    <div class="center-align">
      <h4>Our Contributors</h4>
    </div>
    <br>
    <br>
    <div class="row">
      <section v-for="c in contributors.slice().reverse()" :key="c.id">
        <div class="col s12 m4 l4">
          <div class="card-panel hoverable">
            <div class="card-image center-align">
              <progressive-img
                class="contributor-img circle responsive-image"
                :src="`${c.attribute.avatar_url || '/profilepic.png'}`"
                placeholder="/imageplaceholder1x1.png"
                :alt="`${c.attribute.login}`"
                :blur="30"
              />
            </div>
            <div class="card-content center-align text">
              <p v-if="c.attribute.name" style="font-size:20px">
                {{ c.attribute.name }}
              </p>
              <p v-else style="font-size:20px">
                {{ c.attribute.login }}
              </p>
              <a :href="c.attribute.html_url">
                View Profile
                <i class="fa fa-github git circle" />
              </a>
              &nbsp;
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  fetch ({ store }) {
    return axios.get('https://api.github.com/repos/stacsnssce/official-website/stats/contributors')
      .then(async ({ data }) => {
        /* eslint-disable no-console */
        store.commit('Contributors', await Promise.all(data.map(async (element) => {
          return await axios.get(element.author.url)
            .then((res) => {
              // eslint-disable-next-line
              // console.log(mdf)
              return {
                attribute: res.data
              }
            })
            // store.commit('Contributors', contributors)
        })))
      })
      .then(() => {
      })
  },
  computed: {
    contributors () {
      return this.$store.state.contributors
    }
  },
  head () {
    return {
      title: 'Contributors - STACS Website'
    }
  }
}
</script>
<style lang="scss">
.contributor-img {
  width: 225px !important;
  height: 225px !important;

  img {
    width: 225px!important;
    height: 225px!important;
    object-fit: cover;
  }
}
</style>
<style lang="scss" scoped>
h4{
  font-size: 32px;
  font: Bolder 40px/43px Source Sans Pro;
}
.card-panel {
  border-radius: 16px 16px 16px 16px;
  box-shadow: 0 4px 8px grey;
  height: 400px;
}
.text {
  text-align: center;
  font-weight: bold;
  font-size: 16px;
}
.middle {
  position: absolute;
  font-size: 28px;
  overflow: hidden;
  text-align: center;
}
.git{
  color: #211F1F;
}
</style>
