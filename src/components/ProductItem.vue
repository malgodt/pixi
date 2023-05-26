<script setup>
    import { ref } from 'vue';

    const selectedFile = ref('');
    const { product } = defineProps({
        product: {
            type: Object,
            required: true,
        },
    });

    const textTypes = {
        description: 'Description',
        application: 'Application',
        product_use: 'Product Use',
        treatment: 'Treatment',
        substrate: 'Substrate',
        h_sentence: 'Health Safety Sentence',
        clp_pictogram: 'CLP Pictogram',
        expected_result: 'Expected Result',
        please_note: 'Please Note',
        supplementary_info: 'Supplementary Information',
        environmental_information: 'Environmental Information',
        dilution: 'Dilution',
        storage: 'Storage',
        usp: 'Unique Selling Points',
    };

    const fileTypes = {
        sds: 'Safety Data Sheet',
        tds: 'Technical Data Sheet',
        dgnbdk: 'DGNB Certificate',
        ralCertificate: 'RAL Certificate',
        dop: 'Declaration of Performance',
        fdv: 'FDV Documentation',
        breeamnor: 'BREEAM NOR',
        co2md: 'CO₂ MD',
    };

    const hasDownloads = Object.keys(fileTypes).some(
        type => product[type] && product[type].length > 0
    );

    const langCodeToName = {
        en: 'English',
        de: 'German',
        fr: 'French',
        it: 'Italian',
        es: 'Spanish',
        nl: 'Dutch',
        pl: 'Polish',
        pt: 'Portuguese',
        ru: 'Russian',
        tr: 'Turkish',
        zh: 'Chinese',
        da: 'Danish',
        fi: 'Finnish',
        el: 'Greek',
        hu: 'Hungarian',
        ja: 'Japanese',
        sv: 'Swedish',
        ua: 'Ukrainian',
        sk: 'Slovak',
        ro: 'Romanian',
        cs: 'Czech',
        no: 'Norwegian',
        is: 'Icelandic',
        lv: 'Latvian',
        lt: 'Lithuanian',
        ee: 'Estonian',
        cz: 'Czech',
        bg: 'Bulgarian',
    };
</script>

<template>
    <article class="flex flex-col rounded-lg bg-white p-6 shadow-md">
        <header class="mb-4">
            <h1
                class="mb-2 text-2xl font-semibold"
                :title="product.seo_title || product.name"
            >
                {{ product.b2c_product_name || product.name }} ({{ product.product_key }})
            </h1>
            <div class="mb-4 flex flex-wrap gap-2">
                <span
                    v-for="code in product.items"
                    class="rounded-lg border border-gray-200 bg-gray-100 px-2 py-1 text-xs font-semibold text-gray-800 dark:border-gray-500 dark:bg-gray-600 dark:text-gray-100"
                >
                    {{ code }}
                </span>
            </div>
        </header>

        <main class="flex flex-col space-y-4">
            <div class="flex flex-row space-x-4">
                <div
                    v-for="img in product.images"
                    class="max-h-96 w-1/3"
                >
                    <img
                        :src="img"
                        :alt="product.name"
                        class="h-full rounded-lg object-cover"
                    />
                </div>
                <div>
                    <p class="text-gray-700">{{ product.description }}</p>
                    <ul class="mt-2 list-inside list-disc text-gray-700">
                        <li v-if="product.usp_1">
                            {{ product.usp_1 }}
                        </li>
                        <li v-if="product.usp_2">
                            {{ product.usp_2 }}
                        </li>
                        <li v-if="product.usp_3">
                            {{ product.usp_3 }}
                        </li>
                    </ul>
                </div>
            </div>

            <template v-for="(label, field) in textTypes">
                <details v-if="field in product">
                    <summary class="cursor-pointer font-semibold text-gray-800">
                        {{ label }}
                    </summary>
                    <p class="mt-2 text-gray-700">
                        {{
                            Array.isArray(product[field])
                                ? product[field].join(', ')
                                : product[field]
                        }}
                    </p>
                </details>
            </template>

            <!-- Products -->
            <details>
                <summary class="cursor-pointer font-semibold text-gray-800">
                    Products in this family
                </summary>
                <section class="flex flex-wrap">
                    <article
                        v-for="p in product.products"
                        class="w-full bg-white p-4 sm:w-full lg:w-1/3"
                    >
                        <h3 class="mb-2 text-lg font-semibold">{{ p.name }}</h3>
                        <p class="mb-1 flex justify-between text-sm">
                            <span>Number:</span>
                            <span class="">{{ p.number }}</span>
                        </p>
                        <p
                            v-if="p.base"
                            class="mb-1 flex justify-between text-sm"
                        >
                            <span>Base:</span>
                            <span class="">{{ p.base }}</span>
                        </p>
                        <p
                            v-if="p.ean13"
                            class="mb-1 flex justify-between text-sm"
                        >
                            <span>EAN13:</span>
                            <span class="">{{ p.ean13 }}</span>
                        </p>
                        <p class="mb-1 flex justify-between text-sm">
                            <span>Net Weight (kg):</span>
                            <span class="">{{ p.net_weight }}</span>
                        </p>
                        <p class="mb-1 flex justify-between text-sm">
                            <span>Size:</span>
                            <span class="">{{ p.size }}{{ p.unit?.toUpperCase() }}</span>
                        </p>
                    </article>
                </section>
            </details>
        </main>

        <!-- Files section -->
        <footer
            v-if="hasDownloads"
            class="mt-4"
        >
            <label
                for="downloads"
                class="font-semibold text-gray-800"
            >
                Documents:
            </label>
            <select
                class="ml-2 w-full max-w-lg rounded-md border border-gray-300 px-3 py-2 text-gray-700"
                v-model="selectedFile"
            >
                <option value="">Select a file</option>
                <template v-for="cat in Object.keys(fileTypes)">
                    <!-- group by category -->
                    <optgroup
                        :label="fileTypes[cat]"
                        v-if="product[cat].length"
                    >
                        <option
                            v-for="file in product[cat]"
                            :key="file.url"
                            :value="file.url"
                        >
                            {{
                                langCodeToName[file.language?.toLowerCase()] ||
                                file.language ||
                                file.name
                            }}
                        </option>
                    </optgroup>
                </template>
            </select>

            <a
                v-if="selectedFile"
                :href="selectedFile"
                target="_blank"
                class="ml-2 rounded-md border border-gray-300 px-3 py-2 text-gray-700"
            >
                Download
            </a>
        </footer>
    </article>
</template>
