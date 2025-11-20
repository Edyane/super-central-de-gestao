<script setup>
import { computed } from 'vue'
import VueApexCharts from 'vue3-apexcharts'
import 'apexcharts/dist/apexcharts.css'

const props = defineProps({
  data: {
    type: Array,
    required: true
  },
  color: {
    type: String,
    default: '#0641FC'
  },
  innerRadius: {
    type: Number,
    default: 50
  },
  outerRadius: {
    type: Number,
    default: 80
  }
})

const percentage = computed(() => {
  if (!props.data || props.data.length === 0) return 0
  return props.data[0].value || 0
})

const chartOptions = computed(() => {
  const sizePercent = Math.round((props.innerRadius / props.outerRadius) * 100)
  return {
    chart: {
      type: 'donut',
      toolbar: {
        show: false
      },
      sparkline: {
        enabled: false
      }
    },
    plotOptions: {
      pie: {
        donut: {
          size: `${sizePercent}%`,
          labels: {
            show: false
          }
        },
        startAngle: -90,
        endAngle: 270
      }
    },
    dataLabels: {
      enabled: false
    },
    colors: [props.color, '#f3f4f6'],
    legend: {
      show: false
    },
    tooltip: {
      enabled: false
    },
    stroke: {
      show: false
    }
  }
})

const chartSeries = computed(() => {
  return props.data.map(item => item.value)
})
</script>

<template>
  <div class="relative w-full h-full">
    <VueApexCharts
      type="donut"
      height="100%"
      :options="chartOptions"
      :series="chartSeries"
    />
    <div class="absolute inset-0 flex items-center justify-center pointer-events-none">
      <div class="text-center">
        <div class="text-lg lg:text-xl text-[#2A2E33]">{{ percentage }}%</div>
      </div>
    </div>
  </div>
</template>
