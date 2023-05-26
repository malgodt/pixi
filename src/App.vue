<script setup>
    const ENDPOINT = process.env.ENDPOINT || 'http://localhost:7700';
    const INDEX = process.env.INDEX || 'pixi';
    const API_KEY = process.env.API_KEY || 'masterKey';

    import ProductItem from './components/ProductItem.vue';
    import RefinementList from './components/RefinementList.vue';
    import Pagination from './components/Pagination.vue';

    import InstantSearch from 'vue-instantsearch/vue3/es/src/components/InstantSearch';
    import SearchBox from 'vue-instantsearch/vue3/es/src/components/SearchBox.vue';
    import Hits from 'vue-instantsearch/vue3/es/src/components/Hits.vue';

    import { instantMeiliSearch } from '@meilisearch/instant-meilisearch';

    const searchClient = instantMeiliSearch(ENDPOINT, API_KEY, {
        primaryKey: 'id',
    });
</script>

<template>
    <InstantSearch
        :search-client="searchClient"
        :index-name="INDEX"
    >
        <div class="container px-4 py-8 sm:px-6 sm:py-12 lg:px-8">
            <header>
                <h2 class="mb-4 text-xl font-bold text-gray-900 sm:text-3xl">
                    Flügger PIXI
                </h2>

                <!-- Search Box -->
                <SearchBox
                    :placeholder="`Type...`"
                    autofocus
                >
                    <template v-slot="{ currentRefinement, isSearchStalled, refine }">
                        <label
                            class="relative block overflow-hidden border-b border-gray-200 bg-transparent pt-3 focus-within:border-blue-600"
                        >
                            <input
                                type="search"
                                :value="currentRefinement"
                                @input="refine($event.currentTarget.value)"
                                placeholder="Search..."
                                class="peer h-8 w-full border-none bg-transparent p-0 placeholder-transparent focus:border-transparent focus:outline-none focus:ring-0 sm:text-sm"
                            />

                            <span
                                class="absolute start-0 top-2 -translate-y-1/2 text-xs text-gray-700 transition-all peer-placeholder-shown:top-1/2 peer-placeholder-shown:text-sm peer-focus:top-2 peer-focus:text-xs"
                            >
                                Type product name, key, or item number...
                            </span>
                        </label>
                        <span :hidden="!isSearchStalled">Loading...</span>
                    </template>
                </SearchBox>
            </header>

            <div class="mt-8 block lg:hidden">
                <button
                    class="flex cursor-pointer items-center gap-2 border-b border-gray-400 pb-1 text-gray-900 transition hover:border-gray-600"
                >
                    <span class="text-sm font-medium">Filters & Sorting</span>

                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke-width="1.5"
                        stroke="currentColor"
                        class="h-4 w-4 rtl:rotate-180"
                    >
                        <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            d="M8.25 4.5l7.5 7.5-7.5 7.5"
                        />
                    </svg>
                </button>
            </div>

            <div
                class="mt-4 h-full lg:mt-8 lg:grid lg:grid-cols-4 lg:items-start lg:gap-8"
            >
                <!-- Filters -->
                <div class="sticky hidden space-y-4 lg:block">
                    <!-- Sort by -->
                    <div>
                        <label
                            for="SortBy"
                            class="block text-xs font-medium text-gray-700"
                        >
                            Sort By (Not implemented)
                        </label>

                        <select
                            id="SortBy"
                            class="mt-1 rounded border-gray-300 text-sm"
                        >
                            <option>Sort By</option>
                            <option value="Title, DESC">PIM Sort order</option>
                            <option value="Title, ASC">Product count</option>
                            <option value="Price, DESC">Image count</option>
                            <option value="Price, ASC">Recently updated</option>
                            <option value="Price, ASC">Name, Alphabetical</option>
                            <option value="Price, ASC">Price, DESC</option>
                        </select>
                    </div>

                    <div>
                        <p class="block text-xs font-medium text-gray-700">Filters</p>

                        <div class="mt-1 space-y-2">
                            <RefinementList
                                label="Product Type"
                                attribute="type"
                            />
                            <RefinementList
                                label="Brand"
                                attribute="brand"
                            />
                            <RefinementList
                                label="Substrate"
                                attribute="substrate"
                            />
                            <RefinementList
                                label="H Sentence"
                                attribute="h_sentence"
                            />
                            <RefinementList
                                label="CLP Pictogram"
                                attribute="clp_pictogram"
                            />
                            <RefinementList
                                label="Room"
                                attribute="room_or_building"
                            />
                            <RefinementList
                                label="Category"
                                attribute="product_category"
                            />
                            <RefinementList
                                label="Markets"
                                attribute="markets"
                            />
                        </div>
                    </div>
                </div>

                <div class="col-span-3">
                    <Hits>
                        <template v-slot="{ items, sendEvent }">
                            <div class="space-y-8">
                                <ProductItem
                                    v-for="product in items"
                                    :key="product.id"
                                    :product="product"
                                />
                            </div>
                        </template>
                    </Hits>
                </div>

                <Pagination />
            </div>
        </div>
    </InstantSearch>

    <footer>
        <div class="container flex flex-wrap justify-between p-8">
            <p>
                Developed by Lukas (LUVAN). Project owned by Jens (JAVEL) & Tobias
                (TOWIL).
            </p>
            <p>Flügger Group A/S | Digitalization Department.</p>
        </div>
    </footer>
</template>
