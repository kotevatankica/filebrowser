<script >
import axios from "axios";
import FileList from './FileList.vue';
import { ref } from "vue";
import Datepicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';

export default {
    components: {
        FileList,
        Datepicker
    },
    setup() {
        const files = ref([]);
        const starting_date = ref(null);
        const ending_date = ref(null);
        const loaded = ref(false);

        const componentKey = ref(0);


        return {
            files, starting_date, ending_date, loaded, componentKey
        }
    },
    methods: {
        async grabData() {
            let starting_date_local = null;
            let ending_date_local = null;

            if (this.starting_date) {
                starting_date_local = new Date(this.starting_date) / 1000;
            }

            if (this.ending_date) {
                ending_date_local = new Date(this.ending_date) / 1000;
            }
            let params = {
                cut_date: starting_date_local,
                cut_date_end: ending_date_local
            };


            const username = 'adamantus';
            const password = 'lemon7';
            const resp = await axios.get('https://php72.afk.mk/images/api.php', {
                params: params,
                headers: {
                    authorization: "Basic " + btoa(username + ":" + password)
                }
            });
            this.files = resp.data;
            this.loaded = true;

        },

        async handleData() {
            this.loaded = false;
            await this.grabData();
        }

    },

    async mounted() {

        this.loaded = false;
        await this.grabData();
        this.componentKey += 1;
    }
}
</script>

<template>

    <div class="main">

        <div class="footer">
            <a href="https://adamantus.com/"><img
                    src="https://php72.afk.mk/filebrowser/dist/assets/logo-adamantus-2022.4d63995b.svg" alt=""
                    class="logo"></a>
        </div>
    </div>
    <div class="main">
        <div v-if="loaded">
            <div class="datepickers">
                <div class="data-wrap">
                    <span>Older than:</span>
                    <Datepicker v-model="starting_date" @update:modelValue="handleData" :enableTimePicker="false" />
                </div>
                <br>
                <div class="data-wrap">
                    <span>Younger than: </span>
                    <Datepicker v-model="ending_date" @update:modelValue="handleData" :enableTimePicker="false" />
                </div>
            </div>
            <br>
            <br>
            <FileList :files="files" :key="componentKey" />
        </div>
        <div v-else>

            <p class="loading">Loading data ....</p>
        </div>
    </div>
    <div class="main">

        <div class="footer">
            <span>
                © 2022 Adamantus
            </span>
            <span>
                <a href="https://adamantus.com/"> adamantus.com </a>
            </span>
        </div>
    </div>

</template>

<style scoped>
.main {
    max-width: 100%;
    width: 650px;
    margin: 20px auto;
    background: white;
    padding: 1.3em;
    text-align: left;
    color: black;
}

.footer {
    text-align: center;
}

.datepickers {
    display: flex;
    flex-flow: row wrap;
    gap: 1em;
}

.data-wrap {
    flex: 1 1 0;

}

.loading {
    text-align: center;
    font-weight: 900;
    font-size: 20px;
}

.logo {
    height: 4rem;
}
</style>