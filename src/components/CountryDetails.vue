<template>
    <!-- Is the component that we will render via the vue-router's Route and will be receiving the country code (alpha3Code) via the URL. -->
    <!-- <div class="col-7"> -->
    <img
        :src="`https://flagcdn.com/${alpha2Code.toLowerCase()}.svg`"
        alt="country flag"
        style="width: 300px"
    />
    <h1>
        <!-- {{ item.name.common }} -->
        {{ name }}
    </h1>
    <table class="table">
        <thead></thead>
        <tbody>
            <tr>
                <td style="width: 30%">Capital</td>
                <!-- <td>{{ item.capital[0] }}</td> -->
                <td>{{ capital }}</td>
            </tr>
            <tr>
                <td>Area</td>
                <!-- <td>{{ item.area }} km <sup>2</sup></td> -->
                <td>{{ area }} km <sup>2</sup></td>
            </tr>
            <tr>
                <td>Borders</td>
                <td v-if="borders.length === 0">
                    <ul>
                        <li>This Coutry has not borders</li>
                    </ul>
                </td>
                <td v-else>
                    <ul v-for="(countryBorder, idx) in borders" :key="idx">
                        <li>
                            <router-link :to="`${countryBorder}`">
                                {{ countryBorder }}
                                <!-- {{this.newObj.get(countryBorder)}}-->
                            </router-link>
                        </li>
                    </ul>
                </td>
            </tr>
        </tbody>
    </table>
    <!-- </div> -->
</template>

<script>
export default {
    name: "CountryDetails",
    data() {
        return {
            countryData: {},
            name: "",
            area: "",
            capital: "",
            borders: [],
            alpha3Code: "",
            alpha2Code: "",

            // item: countryData.filter(
            //     (element) => element.alpha3Code === this.$route.params.id
            // )[0],

            // newObj: new Map(),
            // countryName: countryData.forEach((item) => {
            //     if (item.alpha3Code === this.$route.params.id) {
            //         return (this.name = item.name.common);
            //     }
            // }),
        };
    },
    methods: {
        async getCountryByAlphaCode() {
            // try {
            this.alpha3Code = this.$route.params.alpha3Code;
            // const response = await fetch(this.url);
            const response = await fetch(
                `https://ih-countries-api.herokuapp.com/countries/${this.alpha3Code}`
            );
            const finalResponse = await response.json();
            // console.log(finalResponse);
            this.countryData = finalResponse;
            this.name = finalResponse.name.common;
            this.area = finalResponse.area;
            this.capital = finalResponse.capital[0];
            this.borders = finalResponse.borders;
            this.alpha2Code = finalResponse.alpha2Code;
            // } catch (err) {
            //     console.log(err);
            //     console.log(error.name === "AbortError");
            // } finally {
            //     console.log(`finally loading after try{{} and catch}`);
            // }
        },
    },
    mounted() {
        this.getCountryByAlphaCode();
        // this.country.map((item) => {
        //     this.newObj.set(item.alpha3Code, item.name.common);
        // });
        // console.log(this.newObj.get("IRN"));
    },
    computed: {
        countryCode() {
            return this.$route.params.alpha3Code;
        },
    },
    watch: {
        countryCode(newCountryCode) {
            this.getCountryByAlphaCode();
        },
    },
};
</script>

<style scoped>
li {
    list-style-type: none;
    padding: 0;
    margin: 0;
}
</style>
