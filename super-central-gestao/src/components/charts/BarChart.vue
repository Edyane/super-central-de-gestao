<!-- <script setup>
import { computed, ref, onMounted, onUnmounted, nextTick, watch } from 'vue'
import VueApexCharts from 'vue3-apexcharts'
import 'apexcharts/dist/apexcharts.css'

const props = defineProps({
  data: {
    type: Array,
    required: true
  },
  dataKey: {
    type: String,
    default: 'value'
  },
  colors: {
    type: Array,
    default: null
  },
  height: {
    type: [String, Number],
    default: 'auto'
  }
})

const isMobile = ref(false)

const checkMobile = () => {
  isMobile.value = window.innerWidth < 1024
}

onUnmounted(() => {
  window.removeEventListener('resize', checkMobile)
})

const chartOptions = computed(() => {
  const mobile = isMobile.value
  const chartHeight = mobile ? 200 : 250
  
  const options = {
    chart: {
      type: 'bar',
      toolbar: {
        show: false
      },
      height: chartHeight,
      zoom: {
        enabled: false
      },
      stacked: false
    },
    plotOptions: {
      bar: {
        borderRadius: 50,
        columnWidth: mobile ? '8%' : '4%',
        distributed: false,
        horizontal: false,
        rangeBarOverlap: true,
        dataLabels: {
          position: 'top'
        }
      }
    },
    dataLabels: {
      enabled: false
    },
    stroke: {
      show: false
    },
    xaxis: {
      categories: props.data.map(item => item.name),
      labels: {
        style: {
          fontSize: mobile ? '9px' : '11px',
          colors: '#666'
        },
        rotate: mobile ? -45 : 0,
        rotateAlways: mobile,
        hideOverlappingLabels: true,
        maxHeight: mobile ? 40 : undefined
      },
      axisBorder: {
        show: false
      },
      axisTicks: {
        show: false
      }
    },
    yaxis: {
      labels: {
        style: {
          fontSize: mobile ? '9px' : '11px',
          colors: '#666'
        },
        formatter: (val) => {
          // Mostrar apenas valores específicos: 0, 50K, 100K, 200K
          const tolerance = 5000
          if (Math.abs(val - 0) <= tolerance) return '0'
          if (Math.abs(val - 50000) <= tolerance) return '50K'
          if (Math.abs(val - 100000) <= tolerance) return '100K'
          if (Math.abs(val - 200000) <= tolerance) return '200K'
          return ''
        }
      },
      min: 0,
      max: 200000,
      tickAmount: 4,
      forceNiceScale: false,
      decimalsInFloat: 0
    },
    grid: {
      borderColor: '#f0f0f0',
      strokeDashArray: 3,
      xaxis: {
        lines: {
          show: false
        }
      },
      yaxis: {
        lines: {
          show: true,
          showForNullSeries: true
        }
      },
      padding: {
        left: mobile ? 0 : 10,
        right: mobile ? 0 : 10
      }
    },
    tooltip: {
      theme: 'light',
      y: {
        formatter: (val) => `R$ ${val.toLocaleString('pt-BR', { minimumFractionDigits: 2, maximumFractionDigits: 2 })}`
      }
    },
    legend: {
      show: false
    }
  }

  // Sempre usar a cor #0641FC para todas as barras
  options.colors = ['#0641FC']

  return options
})

const chartHeight = computed(() => {
  return isMobile.value ? 200 : (props.height === 'auto' ? 250 : props.height)
})

const chartSeries = computed(() => [{
  name: 'Faturamento',
  data: props.data.map(item => item[props.dataKey] || item.value)
}])

const chartRef = ref(null)

// Função para adicionar fundo cinza nas barras e ocultar linhas intermediárias
const customizeBars = () => {
  nextTick(() => {
    setTimeout(() => {
      const chartContainer = chartRef.value?.$el
      if (!chartContainer) return
      
      const svg = chartContainer.querySelector('.apexcharts-svg')
      if (!svg) return
      
      // Ocultar linhas intermediárias
      const gridlines = chartContainer.querySelectorAll('.apexcharts-gridline')
      const chartHeight = parseFloat(svg.getAttribute('height') || '250')
      const maxValue = 200000
      const tolerance = 3000
      
      gridlines.forEach((line) => {
        const y1 = parseFloat(line.getAttribute('y1') || '0')
        const y2 = parseFloat(line.getAttribute('y2') || '0')
        const y = y1 || y2
        
        if (y === 0) return // Linha de borda
        
        const value = maxValue - (y / chartHeight) * maxValue
        
        const isExactValue = 
          Math.abs(value - 0) <= tolerance ||
          Math.abs(value - 50000) <= tolerance ||
          Math.abs(value - 100000) <= tolerance ||
          Math.abs(value - 200000) <= tolerance
        
        if (!isExactValue) {
          line.style.opacity = '0'
          line.style.display = 'none'
          line.style.visibility = 'hidden'
        } else {
          line.style.opacity = '1'
          line.style.display = ''
          line.style.visibility = 'visible'
        }
      })
      
      // Adicionar gradiente SVG customizado
      let defs = svg.querySelector('defs')
      if (!defs) {
        defs = document.createElementNS('http://www.w3.org/2000/svg', 'defs')
        svg.insertBefore(defs, svg.firstChild)
      }
      
      // Criar gradiente de #0641FC até #2FCD66 (de baixo para cima)
      let gradient = defs.querySelector('#barGradient')
      if (gradient) {
        defs.removeChild(gradient)
      }
      gradient = document.createElementNS('http://www.w3.org/2000/svg', 'linearGradient')
      gradient.setAttribute('id', 'barGradient')
      gradient.setAttribute('x1', '0%')
      gradient.setAttribute('y1', '100%')
      gradient.setAttribute('x2', '0%')
      gradient.setAttribute('y2', '0%')
      
      const stop1 = document.createElementNS('http://www.w3.org/2000/svg', 'stop')
      stop1.setAttribute('offset', '0%')
      stop1.setAttribute('stop-color', '#0641FC') // Azul na base
      
      const stop2 = document.createElementNS('http://www.w3.org/2000/svg', 'stop')
      stop2.setAttribute('offset', '100%')
      stop2.setAttribute('stop-color', '#2FCD66') // Verde no topo
      
      gradient.appendChild(stop1)
      gradient.appendChild(stop2)
      defs.appendChild(gradient)
      
      // Adicionar fundo cinza e aplicar gradiente nas barras
      const bars = chartContainer.querySelectorAll('.apexcharts-bar-area')
      const chartArea = chartContainer.querySelector('.apexcharts-plot-area')
      const plotHeight = chartArea ? parseFloat(chartArea.getAttribute('height') || chartHeight) : chartHeight
      const plotY = chartArea ? parseFloat(chartArea.getAttribute('y') || '0') : 0
      
      bars.forEach((bar) => {
        const rect = bar.querySelector('rect:not(.bar-bg-gray)')
        if (!rect) return
        
        const x = rect.getAttribute('x')
        const width = rect.getAttribute('width')
        
        // Criar ou atualizar elemento de fundo cinza
        let bgRect = bar.querySelector('.bar-bg-gray')
        if (!bgRect) {
          bgRect = document.createElementNS('http://www.w3.org/2000/svg', 'rect')
          bgRect.setAttribute('class', 'bar-bg-gray')
          bgRect.setAttribute('fill', '#f3f4f6') // bg-gray-100 equivalente
          bgRect.setAttribute('rx', '50')
          bgRect.setAttribute('ry', '50')
          // Inserir antes do rect colorido para ficar atrás
          bar.insertBefore(bgRect, rect)
        }
        
        // Atualizar fundo cinza (sempre 100% da altura do gráfico)
        bgRect.setAttribute('x', x)
        bgRect.setAttribute('width', width)
        bgRect.setAttribute('height', plotHeight.toString())
        bgRect.setAttribute('y', plotY.toString())
        
        // Aplicar gradiente e bordas arredondadas
        rect.setAttribute('fill', 'url(#barGradient)')
        rect.style.fill = 'url(#barGradient)'
        rect.setAttribute('rx', '50')
        rect.setAttribute('ry', '50')
      })
      
      // Garantir que o gradiente seja aplicado novamente após um pequeno delay
      setTimeout(() => {
        const barsAfter = chartContainer.querySelectorAll('.apexcharts-bar-area')
        barsAfter.forEach((bar) => {
          const rect = bar.querySelector('rect:not(.bar-bg-gray)')
          if (rect) {
            // Remover qualquer fill inline que o ApexCharts possa ter adicionado
            rect.removeAttribute('fill')
            rect.style.fill = ''
            // Aplicar gradiente
            rect.setAttribute('fill', 'url(#barGradient)')
            rect.style.fill = 'url(#barGradient)'
          }
        })
      }, 200)
      
      // Aplicar novamente após mais tempo para garantir
      setTimeout(() => {
        const barsFinal = chartContainer.querySelectorAll('.apexcharts-bar-area')
        barsFinal.forEach((bar) => {
          const rect = bar.querySelector('rect:not(.bar-bg-gray)')
          if (rect) {
            rect.setAttribute('fill', 'url(#barGradient)')
            rect.style.fill = 'url(#barGradient)'
          }
        })
      }, 500)
    }, 500)
  })
}

watch(() => props.data, () => {
  setTimeout(customizeBars, 500)
}, { deep: true })

watch(() => isMobile.value, () => {
  setTimeout(customizeBars, 500)
})

onMounted(() => {
  checkMobile()
  window.addEventListener('resize', () => {
    checkMobile()
    setTimeout(customizeBars, 500)
  })
  setTimeout(customizeBars, 800)
})
</script>

<template>
  <div class="w-full overflow-x-auto">
    <div :style="{ minWidth: isMobile ? '600px' : '100%' }">
      <VueApexCharts
        ref="chartRef"
        type="bar"
        :height="chartHeight"
        :options="chartOptions"
        :series="chartSeries"
        @dataPointSelection="customizeBars"
        @rendered="customizeBars"
      />
    </div>
  </div>
</template> -->

<script setup>
import { computed, ref, onMounted, onUnmounted, nextTick, watch } from 'vue'
import VueApexCharts from 'vue3-apexcharts'
import 'apexcharts/dist/apexcharts.css'

// ---------- Props ----------
const props = defineProps({
	data: {
		type: Array,
		required: true
	},
	dataKey: {
		type: String,
		default: 'value'
	},
	colors: {
		type: Array,
		default: () => ['#0641FC'] // azul padrão
	},
	height: {
		type: [String, Number],
		default: 'auto'
	}
})

// ---------- Mobile ----------
const isMobile = ref(false)

const checkMobile = () => {
	isMobile.value = window.innerWidth < 1024
}

onUnmounted(() => {
	window.removeEventListener('resize', checkMobile)
})

// ---------- Cálculo do eixo Y ----------
const maxValueData = computed(() =>
	Math.max(...props.data.map(item => Number(item[props.dataKey] ?? item.value) || 0))
)

const maxY = computed(() => {
	if (!maxValueData.value) return 50000
	return Math.ceil(maxValueData.value / 50000) * 50000
})

// ---------- Options ----------
const chartOptions = computed(() => {
	const mobile = isMobile.value

	return {
		chart: {
			type: 'bar',
			toolbar: { show: false },
			zoom: { enabled: false },
		},

		plotOptions: {
			bar: {
				borderRadius: 4,
				columnWidth: mobile ? '35%' : '25%',
				distributed: false,
				horizontal: false,
			}
		},

		fill: {
			opacity: 1,
			type: 'solid'
		},

		colors: props.colors,

		dataLabels: {
			enabled: false
		},

		stroke: {
			show: false
		},

		xaxis: {
			categories: props.data.map(item => item.name),
			labels: {
				style: {
					fontSize: mobile ? '9px' : '11px',
					colors: '#606060'
				},
				rotate: mobile ? -45 : 0
			},
			axisBorder: { show: false },
			axisTicks: { show: false }
		},

		yaxis: {
			min: 0,
			max: maxY.value,
			tickAmount: 4,
			labels: {
				style: {
					fontSize: mobile ? '9px' : '11px',
					colors: '#606060'
				},
				formatter: (val) => {
					if (val === 0) return '0'
					if (val >= 1000) return `${(val / 1000).toFixed(0)}K`
					return val
				}
			}
		},

		grid: {
			borderColor: '#e5e7eb',
			strokeDashArray: 3,
			xaxis: { lines: { show: false } },
			yaxis: { lines: { show: true } }
		},

		tooltip: {
			theme: 'light',
			y: {
				formatter: (val) =>
					`R$ ${val.toLocaleString('pt-BR', {
						minimumFractionDigits: 2,
						maximumFractionDigits: 2
					})}`
			}
		},

		legend: { show: false }
	}
})

// ---------- Series ----------
const chartSeries = computed(() => [
	{
		name: 'Faturamento',
		data: props.data.map(item => Number(item[props.dataKey] ?? item.value) || 0)
	}
])

// ---------- Ajuste visual mínimo ----------
const chartRef = ref(null)

const customizeBars = () => {
	nextTick(() => {
		setTimeout(() => {
			const chart = chartRef.value?.$el
			if (!chart) return

			// Arredondar bordas manualmente (Apex às vezes ignora)
			const rects = chart.querySelectorAll('.apexcharts-bar-area rect')
			rects.forEach(rect => {
				rect.setAttribute('rx', '8')
				rect.setAttribute('ry', '8')
			})
		}, 150)
	})
}

watch(() => props.data, customizeBars, { deep: true })
watch(isMobile, customizeBars)

onMounted(() => {
	checkMobile()
	window.addEventListener('resize', checkMobile)
	customizeBars()
})
</script>

<template>
	<div class="w-full overflow-x-auto">
		<div :style="{ minWidth: isMobile ? '600px' : '100%' }">
			<VueApexCharts ref="chartRef" type="bar" :height="isMobile ? 200 : (height === 'auto' ? 250 : height)"
				:options="chartOptions" :series="chartSeries" @rendered="customizeBars" />
		</div>
	</div>
</template>
