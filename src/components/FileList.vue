<script >
import { ref } from 'vue';
export default {
    name: 'file-sub-list',
    props: ['item'],

    setup(props) {
        const files = ref(props);
        const show = ref(false);
        const search_key = ref('');

        return {
            files: files,
            show: show,
            search_key: search_key
        }
    },

    methods: {

    }
}
</script>

<template>

    <template v-for="item in files" :item="item">

        <template v-if="item.type == 'dir'">
            <div class="mainName">
                <a href="#" @click="show = !show">
                    <img src="../assets/icoooon.svg" alt="" class="dir-img"> </a>
                <div>
                    <div class="name">{{ item.name }} </div>
                    <div class="files"> {{ item.data.count_dir_files }} files</div>
                    <div> Size: <span> {{ item.data.get_directory_size }}</span></div>
                </div>

            </div>
            <div v-if="item.subs && show">
                <file-sub-list v-if="typeof item.subs != 'undefined'" v-for="sub in item.subs" :item="sub">
                </file-sub-list>
            </div>

        </template>

        <template v-else>

            <div class="mainName">
                <img src="../assets/file.svg" alt="" class="dir-img">
                <div>
                    <div class="name bold">{{ item.name }}</div>
                    <div> Size: <span> {{ item.data.file_size }}</span></div>
                    <div><span>Last modified:</span>{{ item.data.last_modified }}</div>
                </div>
            </div>
        </template>


    </template>
</template>

<style scoped>
.mainName {
    clear: both;
    min-height: 70px;
    border-top: solid 1px #ECE9E9;
    display: flex;
    flex-flow: row wrap;
    margin-left: 15px;
    margin-top: 9px;
    color: black;
}

.name {
    color: rgb(62, 62, 142);
    font-weight: bolder;
}

.files {
    margin-right: 50px;
}

.bold {
    font-weight: 900;
}

.dir-img {
    width: 50px;
    height: 50px;
    margin-right: 10px;
}
</style>