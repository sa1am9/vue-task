<template>
    <div>
            <li v-for="task in tasks" :class="{complete : task.complete}">
                <label>
                    <ui-checkbox type="checkbox" v-model="task.complete" >
                        <a v-if="task.complete==false">{{task.name}}</a>
                        <strike v-else> {{task.name}}</strike>
                    </ui-checkbox>
                </label>
            </li>
    </div>

</template>

<script>
export default {
    props: ['tasks'],
    data(){
        return {
        }
    }
}
</script>

<style scoped>

</style>
