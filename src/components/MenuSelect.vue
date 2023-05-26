<script setup>
    // https://www.algolia.com/doc/api-reference/widgets/menu-select/vue/
    import MenuSelect from 'vue-instantsearch/vue3/es/src/components/MenuSelect.vue';

    defineProps({
        attribute: {
            type: String,
            required: true,
        },
        transformItems: {
            type: Function,
            required: false,
        },
        label: {
            type: String,
            required: true,
        },
        limit: {
            type: Number,
            required: false,
            default: 15,
        },
    });

    function sortByCount(items) {
        return items.sort((a, b) => b.count - a.count);
    }
</script>

<template>
    <MenuSelect
        :attribute="attribute"
        :transformItems="transformItems || sortByCount"
        :limit="limit"
    >
        <template v-slot="{ items, canRefine, refine, sendEvent }">
            <details
                class="overflow-hidden rounded border border-gray-300 [&_summary::-webkit-details-marker]:hidden"
            >
                <summary
                    class="flex cursor-pointer items-center justify-between gap-2 p-4 text-gray-900 transition"
                >
                    <span class="text-sm font-medium">{{ label }}</span>

                    <span class="transition group-open:-rotate-180">
                        <svg
                            xmlns="http://www.w3.org/2000/svg"
                            fill="none"
                            viewBox="0 0 24 24"
                            stroke-width="1.5"
                            stroke="currentColor"
                            class="h-4 w-4"
                        >
                            <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                d="M19.5 8.25l-7.5 7.5-7.5-7.5"
                            />
                        </svg>
                    </span>
                </summary>

                <div class="border-t border-gray-200 bg-white">
                    <header class="flex items-center justify-between p-4">
                        <span class="text-sm text-gray-700">
                            {{ items.filter(i => i.isRefined).length }} Selected
                        </span>

                        <button
                            :disabled="!canRefine"
                            @click="refine()"
                            type="button"
                            class="text-sm text-gray-900 underline underline-offset-4"
                        >
                            Reset
                        </button>
                    </header>

                    <ul class="space-y-1 border-t border-gray-200 p-4">
                        <li
                            v-for="item in items"
                            :key="item.value"
                        >
                            <label class="inline-flex items-center gap-2">
                                <input
                                    type="checkbox"
                                    class="h-5 w-5 rounded border-gray-300"
                                    @change="refine(item.value)"
                                    v-model="item.isRefined"
                                />

                                <span class="text-sm font-medium text-gray-700">
                                    {{ item.label }} ({{ item.count }})
                                </span>
                            </label>
                        </li>
                    </ul>
                </div>
            </details>
        </template>
    </MenuSelect>
</template>
