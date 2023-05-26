<script setup>
    import Pagination from 'vue-instantsearch/vue3/es/src/components/Pagination.vue';
</script>

<template>
    <Pagination>
        <template
            v-slot="{
                currentRefinement,
                nbPages,
                pages,
                isFirstPage,
                isLastPage,
                refine,
                createURL,
            }"
        >
            <ul
                class="container flex flex-wrap py-6 sm:justify-center"
                v-show="nbPages > 1"
            >
                <a
                    :disabled="isFirstPage"
                    class="border border-gray-300 px-3 py-2 leading-tight dark:border-gray-700 dark:bg-gray-800 dark:text-gray-400"
                    :class="
                        !isFirstPage
                            ? 'bg-white text-gray-500 hover:bg-gray-100 hover:text-gray-700'
                            : 'hover:bg-gray-100 hover:text-gray-700 dark:hover:bg-gray-700 dark:hover:text-white'
                    "
                    :href="createURL(currentRefinement - 1)"
                    @click.prevent="refine(currentRefinement - 1)"
                >
                    First
                </a>
                <a
                    :disabled="isFirstPage"
                    class="border border-gray-300 px-3 py-2 leading-tight dark:border-gray-700 dark:bg-gray-800 dark:text-gray-400"
                    :class="
                        !isFirstPage
                            ? 'bg-white text-gray-500 hover:bg-gray-100 hover:text-gray-700'
                            : 'hover:bg-gray-100 hover:text-gray-700 dark:hover:bg-gray-700 dark:hover:text-white'
                    "
                    :href="createURL(currentRefinement - 1)"
                    @click.prevent="refine(currentRefinement - 1)"
                >
                    Previous
                </a>

                <span class="mb-2 w-full flex-grow sm:mb-0 sm:w-auto"></span>

                <a
                    v-for="page in pages"
                    :key="`link-${page}`"
                    class="border border-gray-300 px-3 py-2 leading-tight hover:text-gray-700 dark:border-gray-700 dark:bg-gray-800 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white"
                    :class="{
                        'bg-blue-50 text-blue-600 hover:bg-blue-100 hover:text-blue-700':
                            currentRefinement === page,
                        'bg-white text-gray-500 hover:bg-gray-100  hover:text-gray-700':
                            currentRefinement !== page,
                    }"
                    :href="createURL(page)"
                    @click.prevent="refine(page)"
                    v-html="page + 1"
                />

                <span class="mb-2 w-full flex-grow sm:mb-0 sm:w-auto"></span>

                <a
                    :disabled="isLastPage"
                    class="border border-gray-300 px-3 py-2 leading-tight dark:border-gray-700 dark:bg-gray-800 dark:text-gray-400"
                    :class="
                        !isLastPage
                            ? 'bg-white text-gray-500 hover:bg-gray-100 hover:text-gray-700'
                            : 'hover:bg-gray-100 hover:text-gray-700 dark:hover:bg-gray-700 dark:hover:text-white'
                    "
                    :href="createURL(currentRefinement + 1)"
                    @click.prevent="refine(currentRefinement + 1)"
                >
                    Next
                </a>
                <a
                    :disabled="isLastPage"
                    class="border border-gray-300 px-3 py-2 leading-tight dark:border-gray-700 dark:bg-gray-800 dark:text-gray-400"
                    :class="
                        !isLastPage
                            ? 'bg-white text-gray-500 hover:bg-gray-100 hover:text-gray-700'
                            : 'hover:bg-gray-100 hover:text-gray-700 dark:hover:bg-gray-700 dark:hover:text-white'
                    "
                    :href="createURL(nbPages)"
                    @click.prevent="refine(nbPages)"
                >
                    Last
                </a>
            </ul>
        </template>
    </Pagination>
</template>
