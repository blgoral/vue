<template>
    <div class="container-fluid">
        <div class="row">
            <aside :class="asideClass">
                <sidebar
                    :collapsed="sidebarCollaped"
                    :current-category-id="currentCategoryId"
                    :categories="categories"
                    @toggle-collapsed="toggleSidebarCollapsed"
                />
            </aside>
            <div :class="contentClass">
                <catalog
                    :current-category-id="currentCategoryId"
                    :categories="categories"
                />
            </div>
        </div>
    </div>
</template>

<script>
import Catalog from '@/components/catalog';
import Sidebar from '@/components/sidebar';
import { getCurrentCategoryId } from '@/services/page-context';
import { fetchCategories } from '@/services/categories-service';

export default {
    name: 'Products',
    components: {
        Catalog,
        Sidebar,
    },
    data() {
        return {
            sidebarCollaped: false,
            categories: [],
        };
    },
    computed: {
        asideClass() {
            return this.sidebarCollaped ? 'aside-collapsed' : 'col-xs-12 col-3';
        },
        contentClass() {
            return this.sidebarCollaped ? 'col-xs-12 col-11' : 'col-xs-12 col-9';
        },
        currentCategoryId() {
            return getCurrentCategoryId();
        },
    },
    async created() {
        const response = await fetchCategories();

        this.categories = response.data['hydra:member'];
    },
    methods: {
        toggleSidebarCollapsed() {
            this.sidebarCollaped = !this.sidebarCollaped;
        },
    },
};
</script>
