<template>
    <v-row dense class="mt-2">
        <v-col class="text-center">
            <v-item-group class="_btn-group row no-gutters">
                <v-btn
                    v-for="steps of stepsSorted"
                    :key="'w-' + steps"
                    :disabled="moveDisabled"
                    class="btnMinWidthAuto col btnGroup"
                    @click="doSendMove('W-' + steps, feedrateXY)">
                    <span class="body-2">–{{ steps }}</span>
                </v-btn>
                <v-btn
                    :disabled="['printing'].includes(printer_state)"
                    :color="wAxisHomed ? 'primary' : 'warning'"
                    :loading="loadings.includes('homeW')"
                    class="font-weight-bold btnHomeAxis btnGroup"
                    @click="doHomeW">
                    W
                </v-btn>
                <v-btn
                    v-for="steps of stepsSortedReverse"
                    :key="'w+' + steps"
                    :disabled="moveDisabled"
                    class="btnMinWidthAuto col btnGroup"
                    @click="doSendMove('W+' + steps, feedrateXY)">
                    <span class="body-2">+{{ steps }}</span>
                </v-btn>
            </v-item-group>
        </v-col>
    </v-row>
</template>

<script lang="ts">
import { Component, Mixins } from 'vue-property-decorator'
import BaseMixin from '@/components/mixins/base'
import ControlMixin from '@/components/mixins/control'

@Component
export default class WAxisControl extends Mixins(BaseMixin, ControlMixin) {
    get stepsSorted(): number[] {
        return [...(this.$store.state.gui.control?.stepsXY ?? [])].sort((a, b) => b - a)
    }

    get stepsSortedReverse(): number[] {
        return [...(this.$store.state.gui.control?.stepsXY ?? [])].sort((a, b) => a - b)
    }

    get moveDisabled(): boolean {
        return !this.wAxisHomed || ['printing'].includes(this.printer_state)
    }
}
</script>

<style scoped>
.btnHomeAxis {
    width: 36px;
    min-width: 36px !important;
}

.btnGroup {
    height: 28px !important;
}

.btnMinWidthAuto {
    min-width: auto !important;
}

._btn-group {
    border-radius: 4px;
    display: inline-flex;
    flex-wrap: nowrap;
    max-width: 100%;
    min-width: 100%;
    width: 100%;

    .v-btn {
        border-radius: 0;
        border-color: rgba(255, 255, 255, 0.12);
        border-style: solid;
        border-width: thin;
        box-shadow: none;
        height: 28px;
        opacity: 0.8;
        min-width: auto !important;
    }

    .v-btn:first-child {
        border-top-left-radius: inherit;
        border-bottom-left-radius: inherit;
    }

    .v-btn:last-child {
        border-top-right-radius: inherit;
        border-bottom-right-radius: inherit;
    }

    .v-btn:not(:first-child) {
        border-left-width: 0;
    }
}

html.theme--light ._btn-group .v-btn {
    border-color: rgba(0, 0, 0, 0.12);
}
</style>
