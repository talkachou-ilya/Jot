<template>
    <div>
        <div v-if="loading">Loading...</div>
        <div v-else>
            <div class="flex justify-between">
                <a href="#" class="text-blue-400" @click="$router.back()">< Back</a>
                <div class="relative">
                    <router-link class="px-4 py-2 rounded text-sm text-green-500 border border-green-500 mr-2"
                                 :to="`/contacts/${contact.contact_id}/edit`">Edit</router-link>

                    <a href="#" class="px-4 py-2 rounded text-sm text-red-500 border border-red-500 font-bold" @click="modal=!modal">Delete</a>

                    <div class="absolute bg-blue-900 text-white rounded-xl z-20 p-6 w-64 right-0 mt-3 mr-6" v-if="modal">
                        <p class="">Are you sure you want to delete this record?</p>

                        <div class="flex items-center mt-4 justify-between">
                            <button @click="modal=false" class="mr-4 pr-4 py-2 focus:outline-none">Cancel</button>
                            <button class="px-4 py-2 bg-red-500 rounded text-sm font-bold tracking-wide focus:outline-none" @click="destroy">Delete</button>
                        </div>
                    </div>

                    <div class="bg-black opacity-20 fixed left-0 top-0 h-screen w-screen z-10" v-if="modal" @click="modal=false"></div>
                </div>
            </div>

            <div class="flex items-center pt-6">
                <UserCircle :name="contact.name"/>
                <p class="pl-5 text-xl">{{ contact.name }}</p>
            </div>

            <div>
                <p class="pt-6 text-gray-600 font-bold uppercase text-sm">Contact</p>
                <p class="pt-2 text-blue-500">{{ contact.email }}</p>
            </div>
            <div>
                <p class="pt-6 text-gray-600 font-bold uppercase text-sm">Company</p>
                <p class="pt-2 text-blue-500">{{ contact.company }}</p>
            </div>
            <div>
                <p class="pt-6 text-gray-600 font-bold uppercase text-sm">Birthday</p>
                <p class="pt-2 text-blue-500">{{ contact.birthday }}</p>
            </div>
        </div>
    </div>
</template>

<script>
import UserCircle from "../../components/UserCircle";

export default {
    name : "ContactsShow",

    components : {
        UserCircle,
    },

    data : function () {
        return {
            loading : true,
            modal : false,
            contact : null,
        };
    },

    mounted () {
        axios.get( `/api/contacts/${ this.$route.params.id }` )
            .then( response => {
                this.contact = response.data.data;
                this.loading = false;
            } )
            .catch( errors => {
                this.loading = false;

                if ( errors.response.status === 404 )
                    this.$router.push( '/contacts/' );
            } );
    },

    methods : {
        destroy () {
            axios.delete( `/api/contacts/${ this.$route.params.id }` )
                .then( response => {
                    this.$router.push( '/contacts/' );
                } )
                .catch( errors => {
                    alert( 'Internal Error. Unable to delete a contact' )
                } );
        },
    },
}
</script>

<style scoped>

</style>
