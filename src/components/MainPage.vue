<template>
  <v-container>
    <v-row
    align="center"
    justify="center"
    >
      <h1>Search</h1><br>
    </v-row>
    <v-row
      class="mb-6"
    >
      <v-col
        v-for="n in 3"
        :key="n"
        :cols="n === 2 ? 10 : undefined"
      >
        <SearchWidget
        style="max-width:600px"
        v-if="n===2"
        :data="dataSend"
        @search="coba"
        >
        </SearchWidget>
      </v-col>
    </v-row>
    <v-row
    class="mb-6"
    v-if="dataCarousel.length>0"
    align="center"
    justify="center"
    >
      <v-col
        v-for="n in 3"
        :key="n"
        :cols="n === 2 ? 10 : undefined"
      >
        <CarouselWidget
        v-if="n===2"
        :data="dataCarousel"
        @selectTicker="(ticker, month)=>getChartData(ticker, month)"
        >
        </CarouselWidget>
      </v-col>
    </v-row>
    <v-row>
      <!-- <v-progress-circular
      :size="70"
      :width="7"
      color="purple"
      indeterminate
      ></v-progress-circular> -->
      <ChartWidget
      v-if="dataChart.length>0"
      :data="dataChart"
      :categories="categoriesChart"
      @token="getToken"
      >
      </ChartWidget>
    </v-row>
    <v-row
    align="center"
    justify="center"
    >
      <h1>Calculation Form</h1><br>
    </v-row>
    <v-row>
      <v-col>
        <v-card class="center" width="400" height="200" style="background-color:#82b1ff;">
            <h1>Type a number</h1><br><br><br>
            <v-form>
              <v-text-field
                type="number"
                v-model="inputNumber"
                @input="calculation(inputNumber)"
              ></v-text-field>
            </v-form>
        </v-card>
      </v-col>
      <v-col>
        <v-card class="center" width="400" height="200" style="background-color:#e25ebade;">
          <h1>Result!</h1><br><br><br>
          <v-form>
            <v-text-field
              type="number"
              v-model="outputNumber"
              :readonly="true"
            ></v-text-field>
          </v-form>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios"
import SearchWidget from "./widget/SearchWidget";
import CarouselWidget from "./widget/CarouselWidget";
import ChartWidget from "./widget/ChartWidget";

export default {
  name: "MainPage",
  components: {
    SearchWidget,
    CarouselWidget,
    ChartWidget
  },
  props: {

  },
  data() {
    return {
      inputNumber: null,
      outputNumber: null,
      selectedTicker: '',
      categoriesChart: [],
      dataChart: [],
      dataCarousel: [],
      dataSend: [],
      states: [
          'Alabama',
          'Alaska',
          'American Samoa',
          'Arizona',
          'Arkansas',
          'California',
          'Colorado',
          'Connecticut',
          'Delaware',
          'District of Columbia',
          'Federated States of Micronesia',
          'Florida',
          'Georgia',
          'Guam',
          'Hawaii',
          'Idaho',
          'Illinois',
          'Indiana',
          'Iowa',
          'Kansas',
          'Kentucky',
          'Louisiana',
          'Maine',
          'Marshall Islands',
          'Maryland',
          'Massachusetts',
          'Michigan',
          'Minnesota',
          'Mississippi',
          'Missouri',
          'Montana',
          'Nebraska',
          'Nevada',
          'New Hampshire',
          'New Jersey',
          'New Mexico',
          'New York',
          'North Carolina',
          'North Dakota',
          'Northern Mariana Islands',
          'Ohio',
          'Oklahoma',
          'Oregon',
          'Palau',
          'Pennsylvania',
          'Puerto Rico',
          'Rhode Island',
          'South Carolina',
          'South Dakota',
          'Tennessee',
          'Texas',
          'Utah',
          'Vermont',
          'Virgin Island',
          'Virginia',
          'Washington',
          'West Virginia',
          'Wisconsin',
          'Wyoming',
        ],
      form: {
        email:"tes@asklora.ai",
	      password:"H@kbL0m$&"
      }
    }
  },
  methods: {
    calculation(input) {
      this.outputNumber = input*10;
      console.log(this.outputNumber)
    },
    async getToken() {
      let token ="";
      await axios.post("https://services.asklora.ai/user-api/token-auth/", this.form)
      .then((response) => {
        if (response.status == 200) {
          token = response.data.token;
        }
      })
      .catch((error) => {
        console.log(error);
      })
      return token
    },
    coba(data) {
      this.dataCarousel = data;
    },
    async getChartData(ticker, month){
      let number_of_month = 12;
      if (ticker) {
        this.selectedTicker = ticker;
      }
      if (month) {
        number_of_month = month;
      }
      let token = await this.getToken();
      axios.get(`https://services.asklora.ai/api-tac/?ticker=${ticker}&month=${number_of_month}`,
      {
          headers: 
          {
            "Content-Type": "Content-Type",
            "Authorization": `Bearer ${token}`
          }
      })
      .then((response) => {
          if (response.status == 200) {
            console.log(response.data);
            let dataToChart = [];
            let categoriesToChart = []
            response.data.forEach(element => {
              console.log(typeof element.tri_adjusted_price)
              categoriesToChart.push(element.trading_day)
              dataToChart.push([element.trading_day,element.tri_adjusted_price])
            });
            console.log(dataToChart);
            this.categoriesChart = dataToChart
            this.dataChart = dataToChart
          }         
      }).catch(error => {
          console.log(error);
      });
    }
  },
  async mounted () {
    // this.states = this.states;
    let token = await this.getToken();
    axios.get(`https://services.asklora.ai/api-universe/universe/?fields=ticker,ticker_name,latest_price_change,currency,stock_image&page_size=50`,
    {
        headers: 
        {
          "Content-Type": "Content-Type",
          "Authorization": `Bearer ${token}`
        }
    })
    .then((response) => {
        if (response.status == 200) {
          console.log(response.data);
          this.dataSend = response.data.results
        }         
    }).catch(error => {
        console.log(error);
    });
  },
};
</script>
<style scoped>
.center {
  margin: auto;
  width: 60%;
  padding: 10px;
}
</style>
