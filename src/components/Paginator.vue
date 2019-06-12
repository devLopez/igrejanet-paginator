<template>
    <nav>
        <ul class="pagination" :class="align">

            <li class="page-item pagination-first"
                :class="{ disabled: isInFirstPage }">
                <a class="page-link" href="#" @click.prevent="onClickFirstPage"></a>
            </li>

            <li class="page-item pagination-prev"
                :class="{ disabled: isInFirstPage }">
                <a class="page-link" href="#" @click.prevent="onClickPreviousPage"></a>
            </li>

            <li class="page-item"
                v-for="page in pages"
                :key="page.name"
                :class="{ active: isPageActive(page.name) }"
            >
                <a href="#" class="page-link" @click.prevent="onClickPage(page.name)">
                    {{ page.name }}
                </a>
            </li>

            <li class="page-item pagination-next"
                :class="{ disabled: isInLastPage }">
                <a class="page-link" href="#" @click.prevent="onClickNextPage"></a>
            </li>

            <li class="page-item pagination-last"
                :class="{ disabled: isInLastPage }">
                <a class="page-link" href="#" @click="onClickLastPage"></a>
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
            },
            pageRange: {
                type: Number,
                required: false,
                default: 3
            },
            totalPages: {
                type: Number,
                required: false
            },
            total: {
                type: Number,
                required: false
            },
            currentPage: {
                type: Number,
                required: false
            }
        },

        computed: {
            pages() {
                let items   = {};
                let range   = this.getPageRange();

                if ( this.totalPages <= range ) {
                    for ( let index = 0; index < range; index++ ) {

                        let page = { name: index + 1 };

                        items[index] = page
                    }
                } else {

                    const halfPageRange = Math.floor(range / 2);

                    let setPageItem = index => {

                        let page = { name: index + 1 };

                        items[index] = page
                    };

                    let selectedRangeLow = 0;

                    if ( this.selected - halfPageRange > 0 ) {
                        selectedRangeLow = this.selected - 1 - halfPageRange;
                    }

                    let selectedRangeHigh = selectedRangeLow + range - 1;

                    if ( selectedRangeHigh >= this.totalPages ) {
                        selectedRangeHigh   = this.totalPages - 1;
                        selectedRangeLow    = selectedRangeHigh - range + 1;
                    }

                    for ( let i = selectedRangeLow; i <= selectedRangeHigh && i <= this.totalPages - 1; i++ ) {
                        setPageItem(i);
                    }
                }

                return items;
            },

            isInFirstPage() {
                return this.currentPage === 1;
            },

            isInLastPage() {
                return this.currentPage === this.totalPages;
            }
        },

        methods: {
            getPageRange() {
                return ( this.totalPages < this.pageRange ) ? this.totalPages : this.pageRange;
            },

            onClickFirstPage() {
                this.$emit('pagechanged', 1);
            },

            onClickPreviousPage() {
                this.$emit('pagechanged', this.currentPage - 1);
            },

            onClickPage(page) {
                this.$emit('pagechanged', page);
            },

            onClickNextPage() {
                this.$emit('pagechanged', this.currentPage + 1);
            },

            onClickLastPage() {
                this.$emit('pagechanged', this.totalPages);
            },

            isPageActive(page) {
                return page === this.currentPage;
            }
        }
    }
</script>
