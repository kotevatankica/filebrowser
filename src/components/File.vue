<script >
import axios from "axios";
import FileList from './FileList.vue';

export default {
    components: {
        FileList,
    },
    data() {

        return {
            files: '',
        }
    },

    mounted() {

        const username = 'adamantus';
        const password = 'lemon7';
        axios.get('https://php72.afk.mk/young_blood/tankica/file/index.php', {
            headers: {
                authorization: "Basic " + btoa(username + ":" + password)
            }
        }).then((resp) => {
            this.files = resp.data;
            console.log(resp.data)
        });



    }
}
</script>

<template>

    <FileList v-for="item in files" :item="item" />

</template>

<style scoped>

</style>
