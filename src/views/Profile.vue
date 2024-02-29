<template>
        <h3>Account Information</h3>
        <form @submit.prevent="infoSubmit">
            <div class="mb-3">
                <label>First Name</label>
                <input name="first_name"  class="form-control" v-model="infoData.first_name">
            </div>
            <div class="mb-3">
                <label>Last Name</label>
                <input name="last_name"  class="form-control" v-model="infoData.last_name">
            </div>
            <div class="mb-3">
                <label>Email</label>
                <input name="email"  class="form-control" v-model="infoData.email">
            </div>
            <button class="btn btn-outline-secondary">Save</button>
        </form>

        <h3 class="mt-4">Change Password</h3>
        <form @submit.prevent="passwordSubmit">
            <div class="mb-3">
                <label>Password</label>
                <input name="password" type="password"  class="form-control" v-model="passwordData.password">
            </div>
            <div class="mb-3">
                <label>Confirm Password</label>
                <input name="password" type="password"  class="form-control" v-model="passwordData.password_confirm">
            </div>
            <button class="btn btn-outline-secondary">Save</button>
        </form>
</template>

<script>
    import {useStore} from "vuex";
    import {computed} from "vue";
    import axios from "axios";
    import {reactive} from "vue";
    import {watch} from "vue";

    export default {
        name: "Profile",
        setup() {
            const store = useStore();
            const user = computed(()=>store.state.user);

            const infoData = reactive({
                first_name: user.value.first_name,
                last_name: user.value.last_name,
                email: user.value.email
            });

            const passwordData = reactive({
                password: '',
                password_confirm: ''
            })



            watch(
                user, () =>{
                    infoData.first_name = user.value.first_name
                    infoData.last_name = user.value.last_name
                    infoData.email = user.value.email
                }
            );

            const infoSubmit = async () => {
                const {data} = await axios.put('users/info', infoData);

                await store.dispatch('setUser', data);
            }

            const passwordSubmit = async () => {
                await axios.put('users/password',passwordData);

            }

            return {
                infoData,
                passwordData,
                infoSubmit,
                passwordSubmit
            }
        }
    }
</script>

<style scoped>

</style>