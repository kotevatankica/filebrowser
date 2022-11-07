<script >

export default {
    name: 'file-sub-list',
    props: ['item'],

}
</script>

<template>
    <ul>

        <li>
            <p>{{ item.name }}</p>
            <p>{{ item.type }}</p>

            <file-sub-list v-if="typeof item.data.subs != 'undefined'" v-for="sub in item.data.subs" :item="sub">
                <p>{{ item.name }}</p>
                <p>{{ item.type }}</p>
                git init
            </file-sub-list>
        </li>
    </ul>
</template>

<style scoped>

</style>
