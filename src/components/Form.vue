<template>
  <div class="text-white mt-5">
    <div class="row justify-content-between">
      <form class="mt-5 col-sm-12 col-md-6 form p-3" @submit.prevent="submitForm">
        <div class="mb-3">
          <!-- Spinner -->
          <div v-if="isLoading" class="d-flex justify-content-center">
            <div class="spinner-border d-flex" role="status">
              <span class="sr-only"></span>
            </div>
          </div>
          <!-- Alert -->
          <div v-if="!message.ok" class="alert alert-warning" role="alert">
            {{ this.message.text }}
          </div>

          <label for="exampleInputEmail1" class="form-label">Enter City:</label>
          <input type="text" class="form-control" v-model="city" required />
        </div>
        <button type="submit" class="w-100 text-white btn btn-primary btn-lg">Submit</button>
      </form>

      <div class="col-sm-12 col-md-5 shadow-lg p-5 weather">
        <div v-if="data" class="d-flex justify-content-between">
          <div>
            <h1>{{ data.name }}</h1>
            <h2>{{ data.main.temp }} &deg;C</h2>
            <p>{{ data.weather[0].description }}</p>
            <p>{{ data.sys.country }}</p>
            <p>{{ getDate }}</p>
            <div id="icon"></div>
          </div>
          <div id="icon"><img id="wicon" width="80" :src="iconURL" alt="Weather icon" /></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: '',
      status: null,
      message: {
        ok: true,
        text: '',
      },
      isLoading: false,
      data: null,
    };
  },
  async mounted() {
    this.isLoading = true;
    this.city = 'lagos';
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&APPID=${
        import.meta.env.VITE_OPENWEATHER_API_KEY
      }&units=metric`,
      {
        method: 'GET',
        parameter: {
          units: 'metrics',
        },
      }
    );

    if (!response.ok) {
      this.message.text = 'Unable to find city.';
      this.message.ok = false;

      this.isLoading = false;
    } else {
      const data = await response.json();
      this.data = { ...data };
      this.isLoading = false;
    }
  },
  methods: {
    async submitForm() {
      this.isLoading = true;
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&APPID=${
          import.meta.env.VITE_OPENWEATHER_API_KEY
        }&units=metric`,
        {
          method: 'GET',
          parameter: {
            units: 'metrics',
          },
        }
      );

      if (!response.ok) {
        const data = await response.json()
        this.message.text = data.message;
        this.message.ok = false;
        this.removeMessage();

        console.log(data);

        this.isLoading = false;
      } else {
        const data = await response.json();
        this.data = { ...data };
        this.isLoading = false;
        console.log(this.data);
      }
    },
    removeMessage() {
      setInterval(() => {
        this.message.ok = true;
      }, 5000);
    },
  },
  computed: {
    getDate() {
      const date = new Date();

      let day = date.getDate();
      let month = date.getMonth() + 1;
      let year = date.getFullYear();

      // This arrangement can be altered based on how we want the date's format to appear.
      let currentDate = `${day}/${month}/${year}`;
      return currentDate;
    },
    iconURL() {
      const iconURL = 'http://openweathermap.org/img/w/' + this.data.weather[0].icon + '.png';

      return iconURL;
    },
  },
};
</script>

<style lang="css" scoped>
@media (max-width: 767px) {
    .form {
        /* margin-top: 20px; */
    }
    .weather {
        margin-top: 30px;
    }
}
</style>
