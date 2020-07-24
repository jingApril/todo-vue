<template>
    <div>
        <label>
            <input type="checkbox" :checked="!anyRemaining"  @change="allChecked"> 全选
        </label>
    </div>
</template>

<script>
    export default {
        name: "todo-check-all",
        computed: {
            anyRemaining() {
               return this.$store.getters.anyRemaining
            }
        },
        methods: {
            allChecked() {
                this.$store.dispatch('checkAll', event.target.checked)
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>