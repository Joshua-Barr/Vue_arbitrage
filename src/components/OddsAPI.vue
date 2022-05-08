<template>
  <div class="Odds">
    <button class='fill' @click="scrape('https://api.the-odds-api.com/v4/sports/upcoming/odds/?regions=au&markets=h2h&apiKey=adab6068fc2ab8020c2d7456f167f717')">{{ upcomingText }} </button>
    <button class='fill' @click="scrape('https://api.the-odds-api.com/v4/sports/aussierules_afl/odds/?regions=au&markets=h2h&apiKey=adab6068fc2ab8020c2d7456f167f717')">{{ aflText }} </button>
    <button class='fill' @click="scrape('https://api.the-odds-api.com/v4/sports/baseball_mlb/odds/?regions=au&markets=h2h&apiKey=adab6068fc2ab8020c2d7456f167f717')">{{ mlbText }} </button>
    <button class='fill' @click="scrape('https://api.the-odds-api.com/v4/sports/basketball_nba/odds/?regions=au&markets=h2h&apiKey=adab6068fc2ab8020c2d7456f167f717')">{{ nbaText }} </button>
    <button class='fill' @click="scrape('https://api.the-odds-api.com/v4/sports/cricket_ipl/odds/?regions=au&markets=h2h&apiKey=adab6068fc2ab8020c2d7456f167f717')">{{ iplText }} </button>
    <button class='fill' @click="scrape('https://api.the-odds-api.com/v4/sports/icehockey_nhl/odds/?regions=au&markets=h2h&apiKey=adab6068fc2ab8020c2d7456f167f717')">{{ nhlText }} </button>
    <button class='fill' @click="scrape('https://api.the-odds-api.com/v4/sports/mma_mixed_martial_arts/odds/?regions=au&markets=h2h&apiKey=adab6068fc2ab8020c2d7456f167f717')">{{ mmaText }} </button>
    <button class='fill' @click="scrape('https://api.the-odds-api.com/v4/sports/rugbyleague_nrl/odds/?regions=au&markets=h2h&apiKey=adab6068fc2ab8020c2d7456f167f717')">{{ nrlText }} </button>
    
    
    <ul v-for="game in webdata"  :key="game.id">
      <br><br/>
      <table v-if="isloaded" style="border-collapse: collapse; width: 100%;" border="3" position="center">
        <tbody>
          <tr>
            <td style="width: 10%;"><p>Arbitrage Ratio: </p></td>
            <td style="width: 40%;">
              <ul v-for="odds in gendata" :key="odds.home">
                <ul v-if="odds.home == game.home_team">{{ "Gain on $100: " + odds.profitOn1hun.toPrecision(3) + "  -  Ratio: " + odds.arbvalue.toPrecision(3) }}</ul>
              </ul>
            </td>
          </tr>
          <tr>
            <td style="width: 10%;"><p>Highs and Lows: </p></td>
            <td style="width: 40%;">
              <ul v-for="odds in gendata" :key="odds.home">
                <ul v-if="odds.home == game.home_team">{{ "HomeMax / AwayMax / DrawMax: " + odds.homemax + " / " + odds.awaymax + " / " + odds.drawmax }}</ul>
              </ul>
            </td>
          </tr>
          <tr>
            <td style="width: 10%;"><p>Bet Amounts: </p></td>
            <td style="width: 40%;">
              <ul v-for="odds in gendata" :key="odds.home">
                <ul v-if="odds.home == game.home_team">{{ "Bet on $100 Home Team: " + odds.homebet.toPrecision(3) + "  -  Bet on $100 Away Team: " + odds.awaybet.toPrecision(3) }}</ul>
              </ul>
            </td>
          </tr>
          <tr>
            <td style="width: 10%;"><p>Sport: </p></td>
            <td style="width: 40%;">{{ game.sport_title }}</td>
          </tr>
          <tr>
            <td style="width: 10%;"><p>Teams:</p></td>
            <td style="width: 40%;">{{ game.home_team }} VS {{ game.away_team }}</td>
          </tr>
          <tr>
            <td style="width: 10%;"><p>Commence Time: </p></td>
            <td style="width: 40%;">{{ new Date(game.commence_time) }}</td>
          </tr>
          <tr>
            <td style="width: 10%;"><p>Markets:</p></td>
            <ul v-for="bookies in game.bookmakers"  :key="bookies.key">
              <table style="border-collapse: collapse; width: 98%;" position="center">
                <tbody>
                  <tr>
                    <td style="width: 20%;"><p>{{ bookies.title }}</p></td>
                    <td style="width: 80%;">
                      <ul v-for="market in bookies.markets"  :key="market.key">
                      <ul v-if="market.key == 'h2h'">
                        <ul v-for="price in market.outcomes"  :key="price.key">
                          <table style="border-collapse: collapse; width: 100%;" position="center">
                            <tbody>
                              <tr>
                                <td style="width: 50%;">{{ price.name }}</td>
                                <td style="width: 50%;">{{ price.price }}</td>
                              </tr>
                            </tbody>
                          </table>
                        </ul>
                      </ul>
                      </ul>
                    </td>
                  </tr>
                </tbody>
              </table>
            </ul>
          </tr>
        </tbody>
      </table>
    </ul>
  </div> 
</template>

<script>
const axios = require('axios');

export default {
  name: 'OddsAPI',
  data () {
    return {
      upcomingText: "Get Running and Upcoming",
      aflText: "Get Aussie Rules",
      mlbText: "Get Major League Baseball",
      nbaText: "Get Basketball NBA",
      iplText: "Get Cricket IPL",
      nhlText: "Get Hockey NHL",
      mmaText: "Get Mixed Martial Arts",
      nrlText: "Get Rugby League NRL",
      isloaded: false,
      webdata: [],
      gendata: []
    }
  },
  methods: {
    scrape (url) {
      console.log("Consuming API... " + url)
      this.isloaded = false
      this.webdata = []
      this.gendata = []

      axios({
        method: 'get',
        url: url,
      })
      .then((response) => {
      let returnedJSON = response.data
        if( returnedJSON != "null" ){
          this.buttonText = "Data Returned"
          console.log("Data Returned")
        }
        
        returnedJSON.forEach(element => {
          this.webdata.push(element)

          let away = element.away_team
          let home = element.home_team
          let awayvalue = 0
          let homevalue = 0
          let drawvalue = 0

          element.bookmakers.forEach(element => {
            element.markets.forEach(element => {
              let market = element.key
              if(market == "h2h"){
                element.outcomes.forEach(element => {
                  if (home == element.name){
                    if (element.price > homevalue){
                      homevalue = element.price
                    }
                  }
                  if (away == element.name){
                    if (element.price > awayvalue){
                      awayvalue = element.price
                    }
                  }
                  if ("Draw" == element.name){
                    if (element.price > drawvalue){
                      drawvalue = element.price
                    }
                  }
                })
              }
            });
          });
          let arbvalue = 2
          if(drawvalue == 0){
            arbvalue = ((1/homevalue) + (1/awayvalue))
          }
          else{
            arbvalue = ((1/homevalue) + (1/awayvalue) + (1/drawvalue))
          }
          
          this.gendata.push({
            home: home,
            away: away,
            homemax: homevalue,
            awaymax: awayvalue,
            drawmax: drawvalue,
            homebet: (100*(1/homevalue))/arbvalue,
            awaybet: (100*(1/awayvalue))/arbvalue,
            arbvalue: arbvalue,
            profitOn1hun: ((100/arbvalue)-100)
          })
        });
        console.log(this.gendata)
        console.log(this.webdata)
        this.isloaded = true
        console.log(this.isloaded)
      })
      .catch((error) => {
        console.log(error)
      });
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
  padding-inline-start: 10px;
  padding-inline-end: 10px;
}
td {
    border-left: none;
    border-right: none;
    border-top: none;
}
</style>
