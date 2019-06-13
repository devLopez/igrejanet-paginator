<template>
    <nav>
        <ul class="pagination" :class="align">
            <li class="page-item pagination-prev">
                <a class="page-link" href="#" @click.prevent="onClickPreviousPage"></a>
            </li>
            <li class="page-item pagination-next">
                <a class="page-link" href="#" @click.prevent="onClickNextPage"></a>
            </li>
        </ul>
    </nav>
</template>
<script>
    export default {
        props: {
            align: {
                type: String,
                default: 'justify-content-center'
            }
        },

        methods: {
            onClickPreviousPage() {
                this.$emit('pageChanged')
            },
            onClickNextPage() {
                this.$emit('pageChanged')
            }
        }
    }
</script>