<template>
    <div
        v-if="address"
        class="account-wrapper"
        @click="openAccountModal"
    >
        <div class="account-meta">
            <Jazzicon
                class="account-icon"
                :address="address"
                :size="28"
            />
            <div v-if="name" class="account-address">
                {{ name }}
            </div>
            <div v-else class="account-address">
                {{ formatAddress(address) }}
            </div>
        </div>
        <Icon
            class="chevron-icon"
            :title="'chevron'"
        />
    </div>
    <Button
        v-else
        :text="'Connect'"
        :primary="false"
        :loading="loading"
        :disabled="loading"
        @click="openConnectorModal"
    />
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue';
import { useStore } from 'vuex';

import { RootState } from '@/store';
import { formatAddress } from '@/utils/helpers';

import Button from '@/components/Button.vue';
import Icon from '@/components/Icon.vue';
import Jazzicon from '@/components/Jazzicon.vue';

export default defineComponent({
    components: {
        Button,
        Icon,
        Jazzicon,
    },
    setup() {
        const store = useStore<RootState>();

        const address = computed(() => {
            const { connector, address } = store.state.account;
            if (!connector || !connector.id || !address) {
                return '';
            }
            return address;
        });

        const name = computed(() => {
            const { name } = store.state.account;
            return name;
        });

        const loading = computed(() => {
            const { connector, address } = store.state.account;
            return !!connector && !!connector.id && !address;
        });

        function openAccountModal(): void {
            store.dispatch('ui/openAccountModal');
        }

        function openConnectorModal(): void {
            store.dispatch('ui/openConnectorModal');
        }

        return {
            address,
            name,
            loading,
            formatAddress,
            openAccountModal,
            openConnectorModal,
        };
    },

});
</script>

<style scoped>
.account-wrapper {
    height: var(--block-height);
    width: 186px;
    padding: 8px;
    box-sizing: border-box;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border: 1px solid var(--border-input);
    border-radius: var(--border-radius-small);
    background: var(--background-secondary);
    cursor: pointer;
}

.account-wrapper:hover {
    background: var(--background-hover);
}

.account-meta {
    display: flex;
    align-items: center;
}

.account-icon {
    width: 28px;
    height: 28px;
    box-sizing: border-box;
    border-radius: 50%;
}

.account-address {
    margin-left: 8px;
}

.chevron-icon {
    width: 12px;
    height: 12px;
}
</style>
