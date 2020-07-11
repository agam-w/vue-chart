<template>
    <v-container>
        <v-autocomplete
        color="blue"
        full-width
        v-model="select"
        :loading="loading"
        :items="items"
        :search-input.sync="search"
        :item-text="'ticker_name'"
        cache-items
        flat
        hide-no-data
        hide-details
        label="SEARCH"
        append-icon="mdi-magnify"
        rounded
        outlined
        return-object
        @change="didSearch(select, items)"
        @keyup.enter="didSearch(select, items)"
        ref="mytable"
        :auto-select-first="false"
        ></v-autocomplete>
    </v-container>
</template>
<script>
export default {
    name: "SearchWidget",
    props: {
        data: {
            required: true,
            type: Array
        },
    },
    data() {
        return {
            loading: false,
            items: [],
            search: null,
            select: null,
        }
    },
    methods: {
        querySelections (v) {
            this.loading = true
            // Simulated ajax query
            setTimeout(() => {
                this.items = this.data.filter(e => {
                return (e.ticker_name || '').toLowerCase().indexOf((v || '').toLowerCase()) > -1
                })
                this.loading = false
            }, 500)
        },
        didSearch(select, items) {
            if (select) {
                this.$emit('search', [select])
            } else {
                this.$emit('search', items)
                this.$nextTick(() => {
                    this.$refs.mytable.isMenuActive = false
                })  
            }
        }
    },
    mounted () {
        this.items = this.data;
    },
    watch: {
        search (val) {
            val && val !== this.select && this.querySelections(val)
        },
    },
}
</script>
<style>
  .v-autocomplete.v-select--is-menu-active .v-input__icon--append .v-icon {
    transform: none!important;
  }
</style>