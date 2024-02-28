<template>
  <v-container>
    <h1>Личный кабинет</h1>
  </v-container>
  <v-container style="
    display: flex;
    flex-wrap: wrap;
    gap: 1.5rem;">
    <v-card variant="tonal" style="flex: 1 1 20rem;">
      <v-card-title>{{ companyData.name }}</v-card-title>
        <v-card-text>
          <p>Today: {{ formatDate(companyData.status.today) }}</p>
          <p>Total Products: {{ companyData.status.totalProducts }}</p>
          <p>Available Products: {{ companyData.status.availableProducts }}</p>
          <p>Non-Available Products: {{ companyData.status.nonAvailableProducts }}</p>
        </v-card-text>
    </v-card>
  
    <v-card variant="tonal" style="flex: 1 1 20rem;">
      <v-card-title>Выберите город</v-card-title>
      <v-card-text>
        <v-select
          v-model="selectedCity"
          :items="cities"
          item-title="name"
          item-value="code"
          label="Ваш город"
        ></v-select>
      </v-card-text>
    </v-card>
  </v-container>
</template>
<script>
  export default {
    name: 'HomePage',
    data: () => ({
      companyData: {
        name: '',
        status: {
          today: '',
          totalProducts: 0,
          availableProducts: 0,
          nonAvailableProducts: 0
        }
      },
      cities: [
        {
          name: '',
          code: ''
        }
      ],
      selectedCity: null
    }),
    methods: {
      formatDate(dateString) {
        const date = new Date(dateString);
        const formattedDate = `${date.getDate()}.${date.getMonth() + 1}.${date.getFullYear()} ${date.getHours()}:${date.getMinutes()}:${date.getSeconds()}`;
        return formattedDate;
      }
    },
    created() {
      fetch('https://bitbucket.org/ilakhmotkin/front-end-assesment-ru/raw/09c0e12a2b5325e71f36c08e7e29dc8eefbdb081/company-response.json')
        .then(response => response.json())
        .then(data => {
          this.companyData = data;
        })
        .catch(error => {
          console.error('Error fetching company data:', error);
        });
        
      fetch('https://bitbucket.org/ilakhmotkin/front-end-assesment-ru/raw/7bc28b7ede222e25fe44fec52ad1e74a03168501/cities-response.json')
        .then(response => response.json())
        .then(data => {
          this.cities = data.cities;
          const savedCity = localStorage.getItem('selectedCity');
          if (savedCity) {
            this.selectedCity = savedCity;
          }
        })
        .catch(error => {
          console.error('Error fetching cities data:', error);
        });
    },
    watch: {
      selectedCity(city) {
        localStorage.setItem('selectedCity', city);
      }
    }
  }
</script>  
