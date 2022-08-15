<template>
    <!-- Displays the list of links with the country names. Each link should be a vue-router-dom router-link which we will use to send the country code (alpha3Code) via the URL. -->
    <!-- Bootstrap container wrapper div -->
    <div class="container">
        <!-- Bootstrap row wrapper div -->
        <div class="row">
            <!-- Countries List (Bootstrap column) -->
            <div class="col-5" style="max-height: 90vh; overflow: scroll">
                <div
                    class="list-group"
                    v-for="(item, idx) in this.countries"
                    :key="idx"
                >
                    <router-link
                        class="list-group-item list-group-item-action"
                        :to="`${item.alpha3Code}`"
                    >
                        <img
                            :src="`https://flagpedia.net/data/flags/icon/72x54/${item.alpha2Code.toLowerCase()}.png`"
                        />
                        <p>{{ item.name.common }}</p>
                    </router-link>
                </div>
            </div>
            <router-view></router-view>
        </div>
    </div>
</template>

<script>
import countriesData from "../../public/countries.json";
export default {
    name: "CountryList",
    data() {
        return {
            countries: countriesData,
        };
    },
    mounted() {
        // console.log(this.countries[0].alpha2Code);
    },
};
</script>

<style></style>
