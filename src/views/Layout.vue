<template>
    <Nav/>
    <main>
        <Header v-if="showHeader"/>

        <div class="album py-5 bg-body-tertiary">
            <div class="container">
                <router-view/>

            </div>
        </div>

    </main>
    
</template>

<script>
    import Header from "../components/Header";
    import Nav from "@/components/Nav";
    import {onMounted} from "vue";
    import {useStore} from "vuex";
    import axios from "axios";
    import {useRoute} from "vue-router";
    import {computed} from "vue";

    export default {
        name: "Layout",
        components: {Nav,Header},
        setup(){
            const store = useStore();
            const route = useRoute();
            const showHeader = computed(() => route.path === '/' || route.path === '/backend');

            onMounted(async () => {
                try{
                    const {data} = await axios.get('user');

                    await store.dispatch('setUser',data);
                } catch (e) {
                    await store.dispatch('setUser',null);
                }


            });

            return {
                showHeader
            }
        }
    }
</script>

<style scoped>

</style>