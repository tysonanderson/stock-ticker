<template>
  <div class="period-picker">
    <b-form-group>
      <b-form-radio-group
        id="btn-radios-1"
        v-model="selected"
        :options="options"
        buttons
        name="radios-btn-default"
      ></b-form-radio-group>
    </b-form-group>
  </div>
</template>

<script>
export default {
  name: 'PeriodPicker',
  props: {
  },
  data () {
    return {
        selected: '1d',
        options: [
          { text: '1d', value: '1d' },
          { text: '5d', value: '5d' },
          { text: '30d', value: '30d' }
        ]
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
