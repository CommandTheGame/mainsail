<template>
    <v-row dense class="mt-3 align-center">
        <v-col cols="3">
            <v-btn
                :disabled="['printing'].includes(printer_state)"
                :loading="loadings.includes('homeW')"
                :color="wAxisHomed ? 'primary' : 'warning'"
                tile
                height="30"
                class="btnMinWidthAuto w-100"
                @click="doHomeW">
                <v-icon small left>{{ mdiHome }}</v-icon>
                W
            </v-btn>
        </v-col>
        <v-col cols="4">
            <v-btn class="btnMinWidthAuto w-100" :disabled="moveDisabled" @click="doSendMove(`W-${stepSize}`, feedrateXY)">
                <v-icon>{{ mdiChevronLeft }}</v-icon>
            </v-btn>
        </v-col>
        <v-col cols="1"></v-col>
        <v-col cols="4">
            <v-btn class="btnMinWidthAuto w-100" :disabled="moveDisabled" @click="doSendMove(`W+${stepSize}`, feedrateXY)">
                <v-icon>{{ mdiChevronRight }}</v-icon>
            </v-btn>
        </v-col>
    </v-row>
</template>

<style scoped>
.btnMinWidthAuto {
    min-width: auto !important;
}
</style>

<script lang="ts">
import { Component, Mixins } from 'vue-property-decorator'
import BaseMixin from '@/components/mixins/base'
import ControlMixin from '@/components/mixins/control'
import { mdiChevronLeft, mdiChevronRight, mdiHome } from '@mdi/js'

@Component
export default class WAxisControl extends Mixins(BaseMixin, ControlMixin) {
    mdiChevronLeft = mdiChevronLeft
    mdiChevronRight = mdiChevronRight
    mdiHome = mdiHome

    get stepsReversed(): number[] {
        const steps: number[] = this.$store.state.gui.control?.stepsAll ?? []
        return Array.from(new Set(steps)).sort((a, b) => a - b)
    }

    get stepSize(): number {
        const idx = this.$store.state.gui.control?.selectedCrossStep ?? 0
        return this.stepsReversed[idx] ?? this.stepsReversed[0] ?? 1
    }

    get moveDisabled(): boolean {
        return !this.wAxisHomed || ['printing'].includes(this.printer_state)
    }
}
</script>
