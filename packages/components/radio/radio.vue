<template>
    <div :class="[
        inline? 'vsm-label-inline': 'vsm-label-input'
    ]">
        <span v-if="label" class="vsm-label" v-html="label"></span>
        <div :class="[
            optionInline? 'vsm-cols': 'vsm-rows'
        ]">
            <label class="vsm-radio-label"
                :class="{
                    'vsm-disabled': opt.disabled
                }"
                v-for="(opt, i) in opts"
                :key="i">
                <input type="radio" class="vsm-radio"
                    v-model="currentValue"
                    :name="name"
                    :value="opt.value"
                    :checked="opt.value === currentValue"
                    :disabled="opt.disabled"
                    v-on="listeners">
                <span class="vsm-label" v-html="opt.label"></span>
            </label>
        </div>
    </div>
</template>

<script>
export default {
    name: 'vsmRadio',
    props: {
        value: String | Number,
        options: Array,

        name: String,
        label: String,

        inline: Boolean,
        optionInline: Boolean,
    },
    model: {
        prop: 'value',
        event: 'change'
    },
    data () {
        return {
            currentValue: this.value,
            opts: []
        }
    },
    methods: {
        setValue (value, e) {
            this.currentValue = value;
        },
        formatOptions () {
            this.opts = this.options.map(opt => {
            return (typeof opt === 'string' || typeof opt === 'number')? {
                label: opt,
                value: opt
            }: opt;
            });
        }
    },
    computed: {
        listeners () {
            const vm = this;
            return Object.assign({}, this.$listeners, {
                change (e) {
                    vm.$emit('change', vm.currentValue, e);
                }
            });
        },
    },
    created () {
      if (this.options) this.formatOptions();
    },
    watch: {
        value (value) {
            this.setValue(value);
        },
        options (value) {
            this.formatOptions();
        }
    }
}
</script>

<style>

</style>