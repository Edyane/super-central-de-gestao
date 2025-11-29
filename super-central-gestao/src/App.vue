<script setup>
import { ref, computed } from 'vue'
import Sidebar from './components/layout/Sidebar.vue'
import Header from './components/layout/Header.vue'
import Card from './components/ui/Card.vue'
import CardHeader from './components/ui/CardHeader.vue'
import CardTitle from './components/ui/CardTitle.vue'
import CardContent from './components/ui/CardContent.vue'
import Button from './components/ui/Button.vue'
import Badge from './components/ui/Badge.vue'
import BarChart from './components/charts/BarChart.vue'
import DonutChart from './components/charts/DonutChart.vue'
import { Info, ArrowUp, FileText, TrendingUp, Eye, ArrowRight, Filter, ChevronDown, Bell, Trash2, Calendar, Download, HelpCircle, Plus } from 'lucide-vue-next'

// Dados do gráfico de faturamento (31 dias)
const faturamentoData = ref(Array.from({ length: 31 }, (_, i) => {
	const day = i + 1
	let value

	// Valores variados para diferentes dias
	if (day >= 5 && day <= 8) {
		value = Math.floor(Math.random() * 100000) + 50000 // 50K-150K
	} else if (day >= 15 && day <= 17) {
		value = Math.floor(Math.random() * 100000) + 60000 // 60K-160K
	} else if (day === 28) {
		value = Math.floor(Math.random() * 50000) + 50000 // 50K-100K
	} else {
		value = Math.floor(Math.random() * 80000) + 10000 // 10K-90K
	}

	return {
		name: String(day),
		value
	}
}))

// Dados de conversão por modalidade
const conversaoModalidade = ref([
	{ name: 'Crédito', value: 92, color: '#0641FC' },
	{ name: 'Débito', value: 95, color: '#852DF6' },
	{ name: 'Boleto', value: 42, color: '#B882FE' },
	{ name: 'Pix', value: 98, color: '#9DB5FF' }
])

// Dados de bandeiras
const bandeirasData = ref([
	{ name: 'VISA', value: 75, logo: 'VISA', color: '#1A1F71' },
	{ name: 'Mastercard', value: 72, logo: 'MC', color: '#EB001B' },
	{ name: 'ELO', value: 42, logo: 'ELO', color: '#00A4E0' },
	{ name: 'Hipercard', value: 22, logo: 'HC', color: '#D52B1E' },
	{ name: 'AM EX', value: 18, logo: 'AM', color: '#006FCF' },
	{ name: 'G Pay', value: 17, logo: 'GP', color: '#4285F4' },
	{ name: 'Samsung Pay', value: 32, logo: 'SP', color: '#1428A0' },
	{ name: 'Apple Pay', value: 52, logo: 'AP', color: '#000000' },
	{ name: 'Hiper', value: 28, logo: 'HP', color: '#FF6B35' }
])

const isAmountVisible = ref(true)
</script>

<template>
	<div class="min-h-screen bg-[#EDEFF4]">
		<!-- Header apenas no desktop -->
		<div class="hidden lg:block">
			<Header />
		</div>

		<!-- Sidebar apenas no desktop -->
		<div class="hidden lg:block">
			<Sidebar />
		</div>

		<div class="lg:pl-64 lg:pt-16">

			<!-- Header Mobile -->
			<header class="lg:hidden sticky top-0 z-30 w-full border-b bg-white px-4 py-3">
				<div class="flex items-center justify-between">
					<div class="flex items-center gap-2">
						<div class="h-10 w-10 rounded-full bg-black flex items-center justify-center">
							<span class="text-white font-bold text-lg">S</span>
						</div>
						<button
							class="flex items-center gap-2 bg-white border border-gray-200 rounded-full px-3 py-1.5">
							<div class="text-left">
								<div class="text-sm font-medium text-black">Empresarial</div>
								<div class="text-xs text-gray-500">Astra Pagamentos</div>
							</div>
							<ChevronDown class="h-4 w-4 text-gray-500" />
						</button>
					</div>
					<Button variant="ghost" size="icon">
						<Bell class="h-5 w-5 text-gray-700" />
					</Button>
				</div>
			</header>

			<main class="px-4 py-4 lg:px-6 lg:py-6 lg:pt-6">
				<!-- Seção de Faturamento Mobile -->
				<div class="lg:hidden mb-4">
					<h2 class="text-2xl font-bold text-gray-900 mb-3">Faturamento</h2>
					<div class="flex items-center justify-between mb-2">
						<div class="text-2xl font-bold text-gray-900">
							{{ isAmountVisible ? 'R$ 1.249.651,14' : '••••••••••' }}
						</div>
						<button @click="isAmountVisible = !isAmountVisible" class="p-2">
							<Eye class="h-5 w-5 text-gray-600" />
						</button>
					</div>
					<a href="#" class="flex items-center gap-1 text-blue-600 text-sm">
						Ir para a carteira
						<ArrowRight class="h-4 w-4" />
					</a>
				</div>

				<!-- Botão Nova Cobrança Mobile -->
				<Button class="lg:hidden w-full mb-4 bg-[#0641FC] hover:bg-blue-700 text-white h-12 rounded-lg flex items-center justify-center gap-3 px-4">
					<span class="font-medium">Nova cobrança</span>
					<div class="h-8 w-8 rounded-full bg-white flex items-center justify-center">
						<Plus class="h-5 w-5 text-[#0641FC] font-bold" />
					</div>
				</Button>


				<!-- Filtros e Ações Desktop -->
				<div class="hidden lg:flex mb-6 flex-col sm:flex-row gap-4 items-start sm:items-center justify-between">
					<div class="flex flex-wrap gap-3 items-center">
						<Button class="bg-[#0641FC] hover:bg-blue-700 text-white flex items-center gap-2">
							Nova cobrança
							<div class="h-6 w-6 rounded-full bg-white flex items-center justify-center">
								<p class="text-[#0641FC] font-semibold text-lg mb-1">+</p>
							</div>
						</Button>
						<div class="relative">
							<select
								class="pl-10 pr-3 py-2 border border-gray-300 rounded-md text-sm bg-white appearance-none">
								<option>Período: Específico</option>
							</select>
							<Calendar
								class="absolute left-3 top-1/2 transform -translate-y-1/2 h-4 w-4 text-gray-400 pointer-events-none" />
						</div>
						<input type="date" value="2020-06-10"
							class="px-3 py-2 border border-gray-300 rounded-md text-sm bg-white" />
						<input type="date" value="2025-01-30"
							class="px-3 py-2 border border-gray-300 rounded-md text-sm bg-white" />
						<div class="relative">
							<select
								class="pl-10 pr-3 py-2 border border-gray-300 rounded-md text-sm bg-white appearance-none">
								<option>Tipo de cobrança</option>
							</select>
							<FileText
								class="absolute left-3 top-1/2 transform -translate-y-1/2 h-4 w-4 text-gray-400 pointer-events-none" />
						</div>
						<Button variant="ghost" size="icon">
							<Download class="h-5 w-5 text-gray-600" />
						</Button>
					</div>
				</div>

				<!-- Estatísticas do Período Mobile -->
				<Card class="lg:hidden mb-4">
					<CardHeader class="pb-3">
						<div class="flex items-center justify-between">
							<CardTitle class="text-base font-semibold">Estatísticas do período</CardTitle>
							<Filter class="h-4 w-4 text-gray-400" />
						</div>
					</CardHeader>
					<CardContent class="pt-0">
						<div class="text-3xl font-bold mb-2">R$ 760.102,06</div>
						<div class="flex items-center gap-1 text-green-600 mb-4">
							<ArrowUp class="h-4 w-4" />
							<span class="text-sm font-medium">123,9%</span>
						</div>
						<div class="h-40 mb-3 w-full -mx-4 px-4 overflow-x-auto">
							<BarChart :data="faturamentoData" dataKey="value" height="200" />
						</div>
						<div class="text-xs text-gray-500">De 15 de Dez. de 2024 à 07 de Fev. de 2025</div>
					</CardContent>
				</Card>

				<!-- Cards Faturamento Mobile -->
				<div class="lg:hidden grid grid-cols-2 gap-3 mb-4">
					<Card>
						<CardContent class="pt-4">
							<div class="flex items-start gap-2 mb-2">
								<div class="h-2 w-2 rounded-full bg-blue-600 mt-1.5"></div>
								<div class="text-xs text-gray-500">Faturamento recebido</div>
							</div>
							<div class="text-xl font-semibold text-gray-900">R$ 245.340,90</div>
						</CardContent>
					</Card>
					<Card>
						<CardContent class="pt-4">
							<div class="flex items-start gap-2 mb-2">
								<div class="h-2 w-2 rounded-full bg-purple-600 mt-1.5"></div>
								<div class="text-xs text-gray-500">Faturamento a receber</div>
							</div>
							<div class="text-xl font-semibold text-gray-900">R$ 815.210,24</div>
						</CardContent>
					</Card>
				</div>

				<!-- Seção de Faturamento Desktop -->
				<Card class="hidden lg:block mb-6 bg-white">
					<CardHeader class="pb-4">
						<div class="flex items-center gap-2">
							<CardTitle class="text-lg font-bold text-gray-900 leading-none">Faturamento</CardTitle>
							<button @click="isAmountVisible = !isAmountVisible"
								class="p-1 hover:bg-gray-100 rounded flex items-center justify-center">
								<Eye class="h-4 w-4 text-gray-400" />
							</button>
						</div>
					</CardHeader>
					<CardContent class="pt-0">
						<!-- Valor Principal e Indicador -->
						<div class="mb-8">
							<div class="flex items-center gap-4 flex-wrap">
								<div class="flex items-center gap-1 leading-tight">
									<span class="text-sm font-bold text-[#86898B]">R$</span>
									<span v-if="isAmountVisible"
										class="text-5xl font-bold text-[#0641FC]">1.060.551,14</span>
									<span v-else class="text-5xl font-bold text-[#0641FC]">••••••••••</span>
								</div>
								<div class="flex flex-col">
									<div class="flex items-center gap-1.5">
										<ArrowUp class="h-5 w-5 text-[#34A853] flex-shrink-0 font-bold" />
										<span class="text-sm font-bold text-[#34A853]">123,9%</span>
									</div>
									<span class="text-sm text-[#86898B]">Em crescimento</span>
								</div>
							</div>
						</div>

						<!-- Gráfico de Barras -->
						<div class="mb-6 w-full">
							<BarChart :data="faturamentoData" dataKey="value" />
						</div>

						<!-- Cards de Resumo -->
						<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-5 gap-6">
							<div class="flex items-start gap-3 border border-[##EEEEEE] rounded-[12px] p-2">
								<div class="h-3 w-3 rounded-full bg-blue-600 mt-1.5 flex-shrink-0"></div>
								<div class="min-w-0">
									<div class="text-xs text-gray-500 mb-1">Faturamento recebido</div>
									<div class="text-xl font-semibold text-gray-900">R$ 245.340,90</div>
								</div>
							</div>

							<div class="flex items-start gap-3 border border-[##EEEEEE] rounded-[12px] p-2">
								<div class="h-3 w-3 rounded-full bg-purple-600 mt-1.5 flex-shrink-0"></div>
								<div class="min-w-0">
									<div class="text-xs text-gray-500 mb-1 flex items-center gap-1.5 flex-wrap">
										Faturamento previsto
										<span
											class="text-[10px] px-1.5 py-0 border border-gray-300 rounded bg-white text-gray-600 font-medium">D+2</span>
									</div>
									<div class="text-xl font-semibold text-gray-900">R$ 815.210,24</div>
								</div>
							</div>

							<div class="flex items-start gap-3 border border-[##EEEEEE] rounded-[12px] p-2">
								<div class="h-3 w-3 rounded-full bg-orange-500 mt-1.5 flex-shrink-0"></div>
								<div class="min-w-0">
									<div class="text-xs text-gray-500 mb-1">Vendas pendentes</div>
									<div class="text-xl font-semibold text-gray-900">R$ 15.332,18</div>
								</div>
							</div>

							<div class="flex items-start gap-3 border border-[##EEEEEE] rounded-[12px] p-2">
								<div class="h-3 w-3 rounded-full bg-purple-400 mt-1.5 flex-shrink-0"></div>
								<div class="min-w-0">
									<div class="text-xs text-gray-500 mb-1">Ticket médio</div>
									<div class="text-xl font-semibold text-gray-900">R$ 192,30</div>
								</div>
							</div>

							<div class="flex items-start gap-3 border border-[##EEEEEE] rounded-[12px] p-2">
								<div class="h-3 w-3 rounded-full bg-gray-700 mt-1.5 flex-shrink-0"></div>
								<div class="min-w-0">
									<div class="text-xs text-gray-500 mb-1">Número de cobranças</div>
									<div class="text-xl font-semibold text-gray-900">12.349</div>
								</div>
							</div>
						</div>
					</CardContent>
				</Card>

				<!-- Solicitação de Transferência Pendente -->
				<div class="hidden lg:block mb-6 bg-white rounded-[250px] p-8">
					<div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4">
						<div class="flex items-center gap-4 flex-1">
							<!-- Ícone com círculo bege e seta -->
							<div class="relative flex-shrink-0">
								<div class="h-12 w-12 rounded-full bg-[#E6E3DC] flex items-center justify-center">
									<ArrowUp class="h-5 w-5 text-black" />
								</div>
								<!-- Círculo laranja com exclamação sobreposto -->
								<div
									class="absolute -bottom-1 -right-1 h-6 w-6 rounded-full bg-[#FF9D3A] flex items-center justify-center border-2 border-white">
									<span class="text-black font-semibold text-xs">!</span>
								</div>
							</div>
							<!-- Texto -->
							<div class="flex-1">
								<h3 class="text-base font-semibold text-[#2A2E33] mb-1">Solicitação de transferência
									pendente</h3>
								<p class="text-sm text-[#86898B] font-semibold">
									A transação no valor de R$ 29.119,13 está aguardando a sua aprovação.
								</p>
							</div>
						</div>
						<!-- Botão -->
						<Button class="bg-[#0641FC] hover:bg-blue-700 text-white flex-shrink-0 rounded-[250px] p-6">
							<span class="mr-2">Autorizar transferência</span>
							<Trash2 class="h-4 w-4" />
						</Button>
					</div>
				</div>

				<!-- Cards Detalhados -->
				<div class="grid grid-cols-2 lg:grid-cols-4 gap-3 lg:gap-4 mb-4 lg:mb-6">
					<!-- Card Reembolsos -->
					<Card>
						<CardContent class="pt-4 lg:pt-6 relative">
							<!-- Ícone de porcentagem no canto superior direito -->
							<div
								class="absolute top-2 right-2 lg:top-4 lg:right-4 h-4 w-4 lg:h-6 lg:w-6 rounded-full border border-[#D9D9D9] flex items-center justify-center p-1.5 lg:p-4">
								<span class="text-[#2A2E33] text-[10px] lg:text-xs font-bold">%</span>
							</div>
							<!-- Título -->
							<div class="text-xs lg:text-sm text-[#2A2E33] mb-2">Reembolsos</div>
							<!-- Valor principal -->
							<div class="text-xl lg:text-2xl font-bold text-black mb-1">R$ 8.260,10</div>
							<!-- Contagem e Porcentagem com borda superior -->
							<div
								class="border-t border-[#EEEEEE] pt-2 mt-8 -mx-6 px-6 -mb-2 flex items-center justify-between">
								<div class="text-xs lg:text-sm text-gray-900">233 cobranças</div>
								<div class="text-xs text-gray-900 font-semibold">4,5%</div>
							</div>
						</CardContent>
					</Card>
					<!-- Card Chargebacks -->
					<Card>
						<CardContent class="pt-4 lg:pt-6 relative">
							<!-- Título com ícone de alerta -->
							<div class="flex items-center gap-1.5 mb-2">
								<div class="text-xs lg:text-sm text-[#2A2E33]">Chargebacks</div>
								<div
									class="h-5 w-5 rounded-full bg-[#FF9D3A] border border-[#FFE8D1] flex items-center justify-center">
									<span class="text-black font-semibold text-xs">!</span>
								</div>
							</div>
							<!-- Valor principal -->
							<div class="text-xl lg:text-2xl font-bold text-black mb-1">R$ 1.260,10</div>
							<!-- Contagem e Porcentagem com borda superior -->
							<div
								class="border-t border-[#EEEEEE] pt-2 mt-8 -mx-6 px-6 -mb-2 flex items-center justify-between">
								<div class="text-xs lg:text-sm text-gray-900">5 cobranças</div>
								<div class="text-xs text-gray-900 font-semibold">0,3%</div>
							</div>
						</CardContent>
					</Card>
					<!-- Card Cancelados -->
					<Card>
						<CardContent class="pt-4 lg:pt-6 relative">
							<!-- Ícone de porcentagem no canto superior direito -->
							<div
								class="absolute top-2 right-2 lg:top-4 lg:right-4 h-4 w-4 lg:h-6 lg:w-6 rounded-full border border-[#D9D9D9] flex items-center justify-center p-1.5 lg:p-4">
								<span class="text-[#2A2E33] text-[10px] lg:text-xs font-bold">%</span>
							</div>
							<!-- Título -->
							<div class="text-xs lg:text-sm text-[#2A2E33] mb-2">Cancelados</div>
							<!-- Valor principal -->
							<div class="text-xl lg:text-2xl font-bold text-black mb-1">R$ 3.120,60</div>
							<!-- Contagem e Porcentagem com borda superior -->
							<div
								class="border-t border-[#EEEEEE] pt-2 mt-8 -mx-6 px-6 -mb-2 flex items-center justify-between">
								<div class="text-xs lg:text-sm text-gray-900">32 cobranças</div>
								<div class="text-xs text-gray-900 font-semibold">1,5%</div>
							</div>
						</CardContent>
					</Card>
					<!-- Card Não autorizado -->
					<Card>
						<CardContent class="pt-4 lg:pt-6 relative">
							<!-- Ícone de porcentagem no canto superior direito -->
							<div
								class="absolute top-2 right-2 lg:top-4 lg:right-4 h-4 w-4 lg:h-6 lg:w-6 rounded-full border border-[#D9D9D9] flex items-center justify-center p-1.5 lg:p-4">
								<span class="text-[#2A2E33] text-[10px] lg:text-xs font-bold">%</span>
							</div>
							<!-- Título -->
							<div class="text-xs lg:text-sm text-[#2A2E33] mb-2">Não autorizado</div>
							<!-- Valor principal -->
							<div class="text-xl lg:text-2xl font-bold text-black mb-1">R$ 6.120,60</div>
							<!-- Contagem e Porcentagem com borda superior -->
							<div
								class="border-t border-[#EEEEEE] pt-2 mt-8 -mx-6 px-6 -mb-2 flex items-center justify-between">
								<div class="text-xs lg:text-sm text-gray-900">122 cobranças</div>
								<div class="text-xs text-gray-900 font-semibold">3,1%</div>
							</div>
						</CardContent>
					</Card>
				</div>

				<!-- Conversão por Modalidade e Bandeiras -->
				<div class="grid grid-cols-1 lg:grid-cols-2 gap-4 lg:gap-6">
					<!-- Conversão por Modalidade -->
					<Card>
						<CardHeader>
							<CardTitle>Conversão por modalidade</CardTitle>
						</CardHeader>
						<CardContent class="border border-[#EEEEEE] rounded-lg p-4 m-4">
							<div class="grid grid-cols-2 lg:grid-cols-4 gap-4 lg:gap-6">
								<div v-for="modalidade in conversaoModalidade" :key="modalidade.name"
									class="flex flex-col items-center">
									<div class="h-20 w-20 lg:h-24 lg:w-24 mb-2">
										<DonutChart
											:data="[{ name: modalidade.name, value: modalidade.value }, { name: 'resto', value: 100 - modalidade.value }]"
											:color="modalidade.color" :innerRadius="55" :outerRadius="65" />
									</div>
									<div class="text-xs lg:text-sm font-medium text-gray-900 text-center">{{
										modalidade.name }}</div>
								</div>
							</div>
						</CardContent>
					</Card>

					<!-- Bandeiras Mais Utilizadas -->
					<Card>
						<CardHeader>
							<div class="flex items-center justify-between">
								<CardTitle>Bandeiras mais utilizadas</CardTitle>
								<Info class="h-4 w-4 text-gray-400" />
							</div>
						</CardHeader>
						<CardContent class="border border-[#EEEEEE] rounded-lg p-2 m-2 lg:p-4 lg:m-4">
							<div class="flex items-end justify-between gap-1 lg:gap-2 overflow-x-auto pb-2">
								<div v-for="bandeira in bandeirasData" :key="bandeira.name"
									class="flex flex-col items-center flex-shrink-0">
									<!-- Barra vertical -->
									<div
										class="w-6 lg:w-8 mb-2 lg:mb-3 h-[100px] lg:h-[150px] flex flex-col justify-end">
										<div class="w-[10px] lg:w-[12px] rounded-[25px] relative bg-gray-100 h-full">
											<div class="w-full rounded-[25px] bg-[#0641FC] absolute bottom-0 transition-all"
												:style="{ height: `${bandeira.value}%` }"></div>
										</div>
									</div>
									<!-- Logo -->
									<div class="text-[8px] lg:text-[10px] font-semibold text-center leading-tight px-0.5 lg:px-1"
										:style="{ color: bandeira.color }">
										<template v-if="bandeira.name === 'VISA'">
											<svg class="w-[35px] h-[18px] lg:w-[49px] lg:h-[24px]" viewBox="0 0 49 24"
												fill="none" xmlns="http://www.w3.org/2000/svg">
												<path
													d="M21.1265 17.6321H18.3281L20.0785 7.07178H22.8766L21.1265 17.6321Z"
													fill="#1633CA" />
												<path
													d="M31.2707 7.33031C30.7187 7.11666 29.8433 6.88074 28.7608 6.88074C25.9973 6.88074 24.0514 8.31867 24.0394 10.3745C24.0165 11.8912 25.4327 12.7337 26.492 13.2394C27.5746 13.7562 27.9426 14.0935 27.9426 14.5542C27.9316 15.2616 27.0677 15.5877 26.2621 15.5877C25.1449 15.5877 24.5462 15.4195 23.6365 15.0259L23.268 14.8572L22.8765 17.2278C23.5328 17.5195 24.7419 17.7785 25.9973 17.7898C28.9336 17.7898 30.8451 16.3742 30.8678 14.1834C30.8789 12.9812 30.1312 12.0601 28.5189 11.3074C27.5402 10.8242 26.9408 10.4984 26.9408 10.004C26.9522 9.55453 27.4477 9.09424 28.5525 9.09424C29.4623 9.07169 30.1306 9.28507 30.637 9.49853L30.8901 9.61067L31.2707 7.33031Z"
													fill="#1633CA" />
												<path
													d="M34.9914 13.891C35.2219 13.2843 36.1087 10.9363 36.1087 10.9363C36.097 10.9588 36.3386 10.3184 36.4768 9.92522L36.6724 10.8352C36.6724 10.8352 37.2024 13.363 37.3174 13.891C36.8801 13.891 35.5442 13.891 34.9914 13.891ZM38.4456 7.07178H36.2811C35.6136 7.07178 35.1065 7.26261 34.8186 7.94798L30.662 17.6319H33.5982C33.5982 17.6319 34.0816 16.3286 34.1855 16.0478C34.5076 16.0478 37.364 16.0478 37.7783 16.0478C37.8586 16.4186 38.1122 17.6319 38.1122 17.6319H40.7032L38.4456 7.07178Z"
													fill="#1633CA" />
												<path
													d="M15.9912 7.07178L13.2508 14.2729L12.9513 12.8124C12.4446 11.1273 10.8556 9.29627 9.0824 8.38594L11.5926 17.6209H14.5517L18.9503 7.07178H15.9912Z"
													fill="#1633CA" />
												<path
													d="M10.706 7.07312H6.20377L6.15771 7.28652C9.66971 8.16281 11.9957 10.2753 12.9513 12.8142L11.9726 7.96078C11.8115 7.28638 11.3163 7.09537 10.706 7.07312Z"
													fill="#1633CA" />
											</svg>
										</template>

										<template v-else-if="bandeira.name === 'Mastercard'">
											<svg class="w-[28px] h-[18px] lg:w-[37px] lg:h-[24px]" viewBox="0 0 37 24"
												fill="none" xmlns="http://www.w3.org/2000/svg">
												<path d="M21.1824 6.10327H14.6206V17.8957H21.1824V6.10327Z"
													fill="#FF5D11" />
												<path
													d="M15.0372 12.0016C15.036 10.8658 15.2933 9.74451 15.7895 8.72286C16.2857 7.70114 17.008 6.8057 17.9015 6.1044C16.7951 5.23497 15.4666 4.69431 14.0675 4.54425C12.6685 4.39417 11.2554 4.64073 9.98994 5.25573C8.72439 5.87075 7.65737 6.8294 6.9109 8.02214C6.16442 9.21486 5.76855 10.5935 5.76855 12.0006C5.76855 13.4077 6.16442 14.7863 6.9109 15.979C7.65737 17.1718 8.72439 18.1304 9.98994 18.7455C11.2554 19.3605 12.6685 19.607 14.0675 19.4569C15.4666 19.3069 16.7951 18.7662 17.9015 17.8968C17.0082 17.1957 16.2862 16.3006 15.7899 15.2793C15.2937 14.2579 15.0363 13.1371 15.0372 12.0016Z"
													fill="#EC0021" />
												<path
													d="M30.0359 12.001C30.0358 13.4082 29.6399 14.787 28.8933 15.9797C28.1466 17.1725 27.0795 18.1312 25.8138 18.7461C24.5481 19.361 23.1349 19.6074 21.7359 19.4571C20.3367 19.3068 19.0081 18.7659 17.9019 17.8962C18.7947 17.1945 19.5166 16.2991 20.013 15.2778C20.5093 14.2564 20.7672 13.1356 20.7672 12C20.7672 10.8644 20.5093 9.74362 20.013 8.7222C19.5166 7.70085 18.7947 6.80551 17.9019 6.10381C19.0081 5.2341 20.3367 4.69318 21.7359 4.5429C23.1349 4.39261 24.5481 4.639 25.8138 5.25393C27.0795 5.86885 28.1466 6.82749 28.8933 8.02027C29.6399 9.213 30.0358 10.5918 30.0359 11.9989V12.001Z"
													fill="#F89E26" />
												<path
													d="M29.3208 16.6466V16.4049H29.4188V16.355H29.1709V16.4049H29.2771V16.6466H29.3208ZM29.802 16.6466V16.355H29.727L29.6396 16.5633L29.552 16.355H29.4854V16.6466H29.5396V16.4279L29.6208 16.6174H29.6771L29.7583 16.4279V16.6487L29.802 16.6466Z"
													fill="#F89E26" />
											</svg>
										</template>

										<template v-else-if="bandeira.name === 'ELO'">
											<svg class="w-[35px] h-[18px] lg:w-[49px] lg:h-[24px]" viewBox="0 0 49 24"
												fill="none" xmlns="http://www.w3.org/2000/svg">
												<mask id="mask0_1_510" style="mask-type:luminance"
													maskUnits="userSpaceOnUse" x="4" y="5" width="40" height="14">
													<path d="M43.9846 5.80896H4.01562V18.1898H43.9846V5.80896Z"
														fill="white" />
												</mask>
												<g mask="url(#mask0_1_510)">
													<path
														d="M4.01562 10.8797H6.96655C7.49899 8.06817 9.94233 5.80896 13.0574 5.80896C16.1725 5.80896 19.0836 8.33188 19.0836 12.1387C19.0836 12.4913 19.0438 12.8702 19.004 13.1589H9.47455C9.9224 14.8906 11.3456 15.9057 13.0574 15.9057C14.7692 15.9057 15.6251 15.3334 16.4959 14.3382L18.2476 15.9455C17.0881 17.2692 15.3564 18.1848 13.0574 18.1848C10.7584 18.1848 7.49899 15.9654 6.97149 13.1538H4.01562V10.8747V10.8797ZM16.5557 10.8797C16.4313 10.1681 16.1079 9.51624 15.6401 9.04846C15.028 8.43639 14.1323 8.05326 13.0524 8.05326C11.9726 8.05326 11.0968 8.42148 10.4449 9.07333C9.79302 9.72526 9.6288 10.1333 9.44968 10.8847H16.5557V10.8797ZM20.8551 5.88858H23.4179V15.6618H28.0607V18.1052H23.4577C21.7657 18.1052 20.8502 17.1895 20.8502 15.4976V5.88858H20.8551ZM28.5484 11.9994C28.5484 8.68024 31.2554 5.80896 34.8383 5.80896C38.4212 5.80896 40.4962 8.04824 41.0287 10.8797H43.9796V13.1589H41.0287C40.4813 15.9704 37.9932 18.1898 34.8383 18.1898C31.2355 18.1898 28.5484 15.3384 28.5484 11.9994ZM34.8433 15.7863C36.9831 15.7863 38.5705 14.1591 38.5705 12.0193C38.5705 9.87951 36.9432 8.21246 34.8433 8.21246C32.7035 8.21246 31.1161 9.83966 31.1161 11.9994C31.1161 14.1591 32.7433 15.7863 34.8433 15.7863Z"
														fill="#003933" />
												</g>
											</svg>
										</template>

										<template v-else-if="bandeira.name === 'Hipercard'">
											<svg class="w-[34px] h-[18px] lg:w-[48px] lg:h-[24px]" viewBox="0 0 48 24"
												fill="none" xmlns="http://www.w3.org/2000/svg">
												<path d="M8.875 7.70947H38.3017V15.4343H8.875V7.70947Z" fill="white" />
												<path
													d="M20.6472 18.4414L5.76135 18.4461V18.4147C5.76135 18.3973 5.78507 18.2846 5.8138 18.1643C5.843 18.0441 5.88815 17.8528 5.91415 17.7392C5.94037 17.6257 5.98484 17.4326 6.01358 17.3105L6.11848 16.8579C6.14813 16.7312 6.19374 16.5311 6.22065 16.4131C6.24756 16.2954 6.29226 16.1024 6.32031 15.9844C6.39108 15.6856 6.46041 15.3865 6.5283 15.0871C6.5543 14.9736 6.59558 14.7985 6.61998 14.6981C6.64438 14.5977 6.67562 14.4619 6.68931 14.3963C6.70299 14.3308 6.747 14.138 6.78737 13.9675C6.82774 13.7974 6.8722 13.6043 6.88635 13.5388C6.90049 13.4732 6.94515 13.2804 6.9858 13.11C7.02639 12.9398 7.07104 12.747 7.085 12.6812C7.09868 12.6157 7.14362 12.4229 7.18464 12.2525C7.22544 12.0822 7.26951 11.8929 7.28268 11.8318C7.30835 11.7151 7.33475 11.5986 7.36209 11.4822C7.39264 11.3512 7.45075 11.1047 7.49089 10.9345C7.53104 10.7641 7.59991 10.4676 7.64369 10.2754C7.70908 9.98943 7.77504 9.70361 7.84144 9.41787L7.93999 8.99714C8.01024 8.69809 8.07991 8.39889 8.14886 8.09961C8.17482 7.98631 8.21911 7.79692 8.2474 7.67896C8.27562 7.56114 8.32449 7.3538 8.35548 7.21849C8.3914 7.06441 8.43009 6.91084 8.47155 6.75785C8.5042 6.63987 8.55024 6.49708 8.57417 6.44021C8.59788 6.38353 8.66333 6.26051 8.71991 6.16673L8.82253 5.99651L8.91584 5.88881C8.96689 5.82972 9.0586 5.73614 9.11926 5.68108C9.19162 5.61636 9.26748 5.55478 9.3466 5.49654C9.41089 5.45036 9.50399 5.38783 9.55366 5.35778C9.60319 5.32773 9.70057 5.27509 9.76988 5.24081C9.83919 5.20652 9.93635 5.16175 9.98609 5.14118C10.0355 5.1206 10.1371 5.08309 10.2114 5.05788C10.2857 5.03247 10.4276 4.99012 10.5265 4.9635C10.663 4.92897 10.8009 4.89937 10.9401 4.87476C11.0682 4.85237 11.2707 4.82232 11.3905 4.8082L11.6075 4.78259L26.5025 4.77755L41.3977 4.77271V4.80417C41.3977 4.82151 41.3737 4.93405 41.3446 5.05425C41.3154 5.17465 41.2666 5.38037 41.236 5.51127C41.2057 5.64236 41.1612 5.83174 41.1373 5.93217C41.1136 6.03261 41.0682 6.22542 41.0369 6.36095L40.9297 6.82139C40.9022 6.93937 40.8574 7.13217 40.8303 7.25016C40.8034 7.36812 40.7585 7.56092 40.7305 7.67896C40.7026 7.79692 40.6593 7.98267 40.6342 8.09176L40.5336 8.52863C40.5033 8.65969 40.4547 8.86681 40.4255 8.98907C40.3961 9.11125 40.3516 9.30085 40.3265 9.40994L40.2259 9.8466C40.1956 9.97772 40.147 10.1849 40.1179 10.3073C40.0851 10.4448 40.0529 10.5824 40.0211 10.7201L39.9211 11.1567C39.8555 11.4401 39.7897 11.7233 39.7233 12.0065C39.6944 12.1286 39.6451 12.3394 39.6137 12.4749C39.5824 12.61 39.5374 12.8033 39.5137 12.9037L39.4159 13.3244C39.3856 13.4555 39.3374 13.6626 39.3087 13.7851L39.2091 14.2138L39.1075 14.6505L39.0004 15.1111L38.9025 15.5318L38.8031 15.9606C38.773 16.0917 38.724 16.298 38.6941 16.4194L38.6398 16.6401L38.575 16.7925C38.5397 16.8766 38.4862 16.9881 38.4565 17.0404C38.3786 17.1733 38.2875 17.2998 38.1844 17.4185C38.1531 17.4529 38.0592 17.5414 37.9758 17.6154L37.8238 17.7497L37.6774 17.8413C37.5971 17.8917 37.5105 17.9429 37.485 17.9552C37.4596 17.9673 37.3627 18.0111 37.2697 18.0521C37.1742 18.0942 37.0769 18.1329 36.978 18.1682C36.88 18.1998 36.7808 18.2285 36.6806 18.2543C36.5846 18.2789 36.4817 18.3033 36.4521 18.3081C36.4225 18.3132 36.3192 18.3321 36.2229 18.3501C36.1265 18.3681 35.9319 18.3949 35.7905 18.4098L35.5333 18.4369L20.6472 18.4414ZM15.351 15.0475H15.7385L15.7495 15.0224C15.7554 15.0084 15.7604 14.976 15.7604 14.9502C15.7604 14.9243 15.7732 14.8302 15.7887 14.741C15.8042 14.6519 15.8409 14.4467 15.8701 14.2852C15.8993 14.1237 15.9431 13.8806 15.967 13.7453C15.9913 13.61 16.0209 13.4367 16.0327 13.3601C16.0446 13.2837 16.0592 13.2211 16.0651 13.2211C16.0708 13.2211 16.0959 13.255 16.1208 13.2963L16.1657 13.3714L16.2656 13.4601L16.3654 13.5489L16.4886 13.5921L16.6113 13.6354L16.7579 13.6521L16.9043 13.6689L17.0905 13.659L17.2763 13.6489L17.4777 13.603L17.679 13.5569L17.7872 13.5134C17.8465 13.4896 17.9521 13.4374 18.0214 13.3977L18.1475 13.325L18.2651 13.2216C18.3297 13.1647 18.4209 13.0733 18.4677 13.0187C18.5147 12.9638 18.5529 12.9152 18.5529 12.9104C18.5529 12.9053 18.5758 12.8716 18.6041 12.8349C18.6321 12.7986 18.6896 12.6971 18.7318 12.6099C18.7739 12.5225 18.8398 12.3652 18.8784 12.2606L18.9482 12.0699L18.9842 11.9048C19.0041 11.814 19.0298 11.664 19.041 11.5714L19.0615 11.403L19.0497 11.26L19.0378 11.117L19.0002 10.9742L18.9625 10.8313L18.9017 10.7243L18.841 10.6172L18.7233 10.5073L18.6059 10.3976L18.4667 10.3343L18.3276 10.2707L18.1746 10.2385L18.0214 10.2062L17.8683 10.1961L17.7151 10.1865L17.5445 10.2036L17.3739 10.2209L17.2348 10.2522L17.0955 10.2837L16.9746 10.3329C16.9007 10.3635 16.8289 10.3977 16.7593 10.4353C16.7179 10.4578 16.6783 10.4827 16.6407 10.5099C16.627 10.522 16.5949 10.5438 16.5689 10.559L16.5214 10.5862L16.5545 10.4466C16.5728 10.3699 16.5881 10.2982 16.5885 10.2873L16.5891 10.2675H15.9285L15.8706 10.589C15.8026 10.9665 15.7323 11.3436 15.6596 11.7204L15.5606 12.2367C15.531 12.3894 15.4825 12.6324 15.4523 12.7766C15.3502 13.2663 15.2477 13.756 15.1447 14.2455C15.115 14.3852 15.0665 14.6069 15.037 14.7378C15.0169 14.8292 14.9957 14.9206 14.9737 15.0117L14.9636 15.0475H15.351ZM17.0188 13.2115L16.8683 13.2195L16.77 13.204L16.6718 13.1887L16.5718 13.1486L16.472 13.1084L16.4022 13.0497L16.3324 12.991L16.2923 12.9115C16.2664 12.8582 16.2453 12.8031 16.2291 12.7468L16.206 12.6613L16.2152 12.5243L16.2243 12.3876L16.2708 12.1732C16.2963 12.0553 16.3422 11.8338 16.3728 11.6809C16.4031 11.5281 16.4494 11.304 16.4754 11.1832L16.5224 10.9633L16.6257 10.8877C16.6889 10.842 16.7557 10.8005 16.8257 10.7635L16.9224 10.715L17.0485 10.6808L17.1746 10.6465L17.3367 10.638L17.4989 10.6295L17.631 10.6544L17.763 10.6792L17.8563 10.7171L17.9493 10.7548L18.0275 10.8241L18.1055 10.8934L18.1477 10.9665C18.171 11.0069 18.2035 11.0859 18.22 11.142L18.2503 11.2441L18.2412 11.5458L18.2318 11.8475L18.1856 12.026C18.1601 12.124 18.1117 12.274 18.0779 12.3593L18.0163 12.5147L17.9413 12.6401C17.9001 12.7093 17.8364 12.8027 17.7997 12.8476C17.7632 12.8926 17.7049 12.9509 17.6702 12.9771C17.6353 13.0033 17.5774 13.0445 17.5411 13.0683L17.475 13.1118L17.3222 13.1576L17.1694 13.2034L17.0188 13.2115ZM20.7598 13.6479L20.9582 13.6499L21.1562 13.634C21.2652 13.6253 21.4435 13.607 21.5526 13.5934C21.6929 13.5749 21.8324 13.5524 21.971 13.5261L22.1913 13.4835L22.2023 13.4119C22.2082 13.3726 22.2331 13.2554 22.257 13.1515L22.3013 12.9628L22.2915 12.9541L22.2819 12.9457L22.2144 12.9743C22.1304 13.0055 22.0449 13.0334 21.958 13.0578L21.7692 13.1126L21.5708 13.1465L21.3724 13.1804L21.0572 13.181L20.7418 13.1814L20.6337 13.1512C20.5675 13.132 20.5025 13.1092 20.4394 13.083L20.3532 13.0455L20.2821 12.9888L20.2109 12.9321L20.1553 12.8466L20.0998 12.7611L20.065 12.6538L20.0303 12.5463L20.0301 12.3478L20.0297 12.1492L20.0606 11.9721L20.0914 11.795L20.3628 11.784L20.6337 11.7726L21.6361 11.7783L22.6381 11.784L22.6684 11.671C22.6915 11.5764 22.7104 11.481 22.725 11.3851L22.7513 11.2124L22.7517 11.0672L22.7523 10.9219L22.7238 10.8133L22.6951 10.705L22.6481 10.6331C22.618 10.5885 22.5841 10.5459 22.5465 10.5059C22.5106 10.4706 22.4709 10.4386 22.4278 10.4103C22.3779 10.3806 22.3263 10.3532 22.2732 10.3282L22.1831 10.2859L22.0258 10.2526L21.8684 10.2191L21.6746 10.2027L21.4807 10.1865L21.3006 10.1966L21.1204 10.207L20.904 10.2437L20.6878 10.2802L20.5528 10.3278C20.4785 10.354 20.3649 10.4019 20.3006 10.4341C20.236 10.4662 20.1468 10.5186 20.1022 10.5505C20.0428 10.5942 19.9856 10.6401 19.9307 10.6881C19.871 10.7425 19.8163 10.801 19.7671 10.8631C19.727 10.9155 19.6652 11.0083 19.6303 11.0694C19.595 11.1305 19.5491 11.2162 19.5283 11.26C19.5076 11.3038 19.4697 11.3931 19.4444 11.4585C19.4189 11.524 19.379 11.6491 19.3555 11.7363C19.328 11.8422 19.3043 11.9487 19.2843 12.0558L19.2556 12.2166V12.4529L19.2561 12.6893L19.2834 12.7923C19.2985 12.8492 19.3272 12.935 19.3473 12.983C19.3675 13.0312 19.4048 13.1025 19.4301 13.1419C19.4556 13.1812 19.5099 13.2443 19.5505 13.2824C19.5915 13.3206 19.6654 13.3782 19.7149 13.4107C19.7644 13.4432 19.8526 13.489 19.911 13.5123L20.0169 13.5547L20.1646 13.5864C20.2458 13.6035 20.3683 13.6241 20.4369 13.632C20.5056 13.6396 20.6508 13.6469 20.7598 13.6479ZM21.0992 11.4108C20.6004 11.4108 20.1924 11.4076 20.1924 11.4038C20.1924 11.4 20.2137 11.3447 20.2397 11.2808C20.2657 11.2171 20.3113 11.1219 20.3411 11.0696L20.3954 10.9746L20.5147 10.8699L20.6337 10.7657L20.751 10.7163C20.8153 10.689 20.9003 10.6596 20.9403 10.6505C20.9797 10.6417 21.0931 10.6297 21.1923 10.6241L21.3724 10.614L21.5108 10.6295L21.6491 10.6453L21.7496 10.6858L21.8502 10.7264L21.9054 10.7788C21.9357 10.8076 21.972 10.8502 21.9857 10.8731L22.0107 10.9151L22.0276 11.0081L22.0441 11.1013L22.0251 11.256L22.0062 11.4108H21.0992ZM26.4893 13.6481L26.6875 13.6499L26.9126 13.6265C27.0351 13.6138 27.1573 13.5979 27.2788 13.579C27.3563 13.5656 27.482 13.5392 27.5579 13.5204C27.6341 13.5015 27.7027 13.4809 27.7107 13.4749C27.7187 13.4688 27.7404 13.3968 27.7584 13.3147C27.7766 13.2329 27.7997 13.1227 27.8095 13.0703C27.8193 13.0179 27.8243 12.9723 27.8206 12.9689C27.817 12.9654 27.8038 12.9699 27.7913 12.9787C27.7785 12.9876 27.6902 13.0203 27.5947 13.0515L27.4214 13.1084L27.1742 13.1505L26.9272 13.1929L26.7171 13.1865L26.507 13.1802L26.3878 13.1437L26.2685 13.1072L26.1764 13.036L26.0841 12.965L26.0229 12.8668L25.9618 12.7686L25.9326 12.6375L25.9036 12.506V12.3355L25.9038 12.1651L25.9392 11.9588L25.9748 11.7523L26.0122 11.6411C26.0327 11.5801 26.056 11.5085 26.064 11.4822C26.0717 11.4561 26.1048 11.381 26.1374 11.3157C26.1698 11.2501 26.2279 11.1489 26.2662 11.0906L26.3358 10.9847L26.4216 10.9065L26.5073 10.8282L26.5979 10.7786L26.6886 10.7291L26.8051 10.6975C26.8878 10.6763 26.9718 10.6595 27.0567 10.6471L27.1919 10.6287L27.3901 10.6374L27.5883 10.6463L27.8225 10.6876L28.0567 10.729L28.1518 10.7602C28.2043 10.7774 28.2494 10.7915 28.2521 10.7915C28.2551 10.7915 28.2677 10.7326 28.2802 10.6604C28.2958 10.5724 28.3123 10.4846 28.3299 10.397C28.3445 10.3244 28.3531 10.2614 28.3489 10.2579C28.3445 10.2538 28.274 10.2439 28.1924 10.2357C28.0281 10.222 27.8636 10.2113 27.6989 10.2034L27.3541 10.1859L27.1108 10.2024L26.8676 10.2187L26.6875 10.2522L26.5073 10.2854L26.3913 10.3264C26.3274 10.349 26.2339 10.3895 26.1835 10.4166C26.1331 10.4438 26.069 10.4803 26.0409 10.4977C26.0129 10.5152 25.9278 10.5833 25.8518 10.6491L25.7139 10.7689L25.6368 10.8683C25.5944 10.923 25.5212 11.0371 25.4739 11.1219L25.3882 11.2759L25.3223 11.4425C25.2863 11.5343 25.2357 11.6846 25.2101 11.7761L25.1636 11.9429L25.1467 12.0937L25.1299 12.2446L25.1301 12.443V12.6415L25.1472 12.7571L25.1643 12.8726L25.2183 12.9914L25.2721 13.1102L25.3347 13.1959L25.3969 13.2819L25.4959 13.368L25.5951 13.4539L25.7 13.5027L25.8049 13.5515L25.9424 13.5852C26.0184 13.6038 26.1278 13.6251 26.1857 13.6326C26.2437 13.6401 26.3803 13.6471 26.4893 13.6481ZM29.4529 13.6422L29.696 13.636L29.8492 13.6015L30.0022 13.567L30.1014 13.5257C30.156 13.5029 30.2451 13.4581 30.2996 13.4261C30.3542 13.3938 30.4383 13.3329 30.4871 13.2905C30.5357 13.2482 30.6078 13.1755 30.6472 13.1296C30.6867 13.0834 30.7213 13.0477 30.7243 13.0503C30.7273 13.0529 30.7181 13.1264 30.704 13.2137C30.6902 13.2991 30.6816 13.3851 30.6782 13.4714L30.6779 13.5707H31.3391L31.3494 13.3443L31.3594 13.1181L31.4066 12.8243C31.4326 12.6627 31.4746 12.4233 31.4997 12.2922C31.5632 11.9663 31.6291 11.6408 31.6974 11.3157L31.7457 11.0854L31.7475 10.8865L31.7491 10.6874L31.6981 10.5957L31.647 10.5037L31.5779 10.4452L31.5086 10.3867L31.395 10.3359L31.2814 10.2851L31.1122 10.252L30.9427 10.2187L30.7097 10.203L30.4766 10.1875L30.1765 10.2038C30.0114 10.2129 29.779 10.2306 29.6599 10.2433L29.4438 10.2665L29.2964 10.2669L29.1491 10.2675L29.1277 10.3589C29.1158 10.4089 29.0832 10.5283 29.0549 10.6239C29.0266 10.7195 29.0061 10.8001 29.0093 10.803C29.0125 10.8058 29.0914 10.7899 29.1844 10.7679C29.2775 10.7457 29.4752 10.7094 29.6239 10.6873L29.8942 10.6465L30.1375 10.6378L30.3806 10.6289L30.5373 10.6531L30.6939 10.6774L30.7986 10.7227L30.903 10.7683L30.9617 10.8451L31.0203 10.9222L31.0194 11.0434L31.0185 11.1648L30.9875 11.2979L30.9564 11.431L30.3083 11.4331L29.6599 11.435L29.4478 11.4863C29.3311 11.5145 29.202 11.5521 29.1607 11.5696C29.1195 11.5871 29.0782 11.6014 29.0691 11.6014C29.0599 11.6014 28.9904 11.6353 28.9147 11.6769L28.7771 11.7523L28.6688 11.8475C28.6093 11.8999 28.5331 11.9786 28.4996 12.0224C28.4536 12.0871 28.4133 12.1549 28.379 12.2251L28.319 12.3483L28.2914 12.487L28.2638 12.6258V12.9166L28.29 13.0378L28.3165 13.1588L28.3653 13.2379C28.392 13.2814 28.4412 13.3451 28.4745 13.3795L28.5352 13.442L28.643 13.4997L28.7507 13.5574L28.8741 13.588C28.942 13.6049 29.0451 13.6255 29.1035 13.6336L29.2097 13.6483L29.4529 13.6422ZM29.6779 13.1982L29.5338 13.2048L29.4385 13.1834C29.3775 13.1688 29.3182 13.1493 29.2613 13.1249L29.1799 13.0881L29.1352 13.0412C29.1038 13.0062 29.076 12.9688 29.0522 12.9293L29.0134 12.864L29.0059 12.7113L28.9984 12.5586L29.0289 12.4659C29.0458 12.415 29.085 12.3267 29.1165 12.2696L29.1735 12.1657L29.2827 12.0702L29.392 11.9745L29.5081 11.9254L29.6239 11.8761L29.7681 11.8459L29.9121 11.8156H30.813L30.8415 11.8261L30.87 11.8366L30.846 11.9612C30.8329 12.0298 30.8014 12.1537 30.7761 12.2366C30.751 12.3196 30.7062 12.4417 30.6771 12.5076C30.6571 12.5502 30.6394 12.5935 30.6239 12.6375C30.6239 12.6429 30.5943 12.6911 30.558 12.7446L30.4923 12.842L30.3843 12.9363C30.3247 12.9882 30.2695 13.0307 30.2615 13.0307C30.2536 13.0307 30.2173 13.0485 30.1806 13.0705L30.1142 13.11L29.9683 13.1507L29.8221 13.1915L29.6779 13.1982ZM35.5424 13.6479L35.7134 13.6497L35.8755 13.6265C35.9648 13.6138 36.0783 13.5932 36.1278 13.5809C36.1773 13.5684 36.2665 13.5366 36.326 13.5096L36.4341 13.4607L36.5271 13.3887L36.6202 13.3166L36.718 13.2014C36.7718 13.138 36.8206 13.0755 36.8263 13.0625L36.8369 13.0386L36.8277 13.1181C36.8227 13.1617 36.8115 13.2331 36.8028 13.2768C36.7942 13.3206 36.7817 13.4045 36.775 13.4634L36.7632 13.5707H37.4702V13.3563L37.524 12.9513C37.5536 12.7284 37.5974 12.432 37.6211 12.2922C37.6451 12.1524 37.682 11.9523 37.7032 11.8475C37.7244 11.7426 37.762 11.5534 37.7867 11.4268C37.8113 11.3001 37.856 11.0785 37.8857 10.9345C37.9155 10.7903 37.9632 10.5616 37.9917 10.4262L38.0902 9.95772C38.1158 9.83554 38.1618 9.62114 38.1924 9.48136C38.2227 9.34158 38.2674 9.14514 38.2914 9.0447C38.3153 8.94427 38.3349 8.85678 38.3347 8.85009V8.83823H37.5636L37.5523 8.93743C37.5461 8.9919 37.5164 9.17256 37.4865 9.3386C37.4569 9.50434 37.4083 9.76878 37.3787 9.92609L37.3153 10.2649L37.3057 10.3177L37.2752 10.3065C37.2087 10.2878 37.1414 10.2713 37.0736 10.257L36.9025 10.2191L36.6683 10.203L36.4341 10.1868L36.2359 10.2034L36.0377 10.2195L35.8576 10.2598L35.6774 10.2998L35.5062 10.3734L35.3351 10.447L35.209 10.5293L35.0829 10.6116L34.9739 10.7145C34.9053 10.7805 34.8411 10.85 34.7816 10.9226L34.6982 11.0278L34.6003 11.2073C34.5465 11.3062 34.4808 11.4371 34.4546 11.4982C34.4153 11.5971 34.381 11.6977 34.3517 11.7993L34.2965 11.9895L34.2694 12.2361L34.2422 12.4828L34.2627 12.6812L34.283 12.8797L34.3073 12.9513C34.3205 12.9906 34.3485 13.0606 34.3693 13.1068L34.4073 13.1911L34.4767 13.2738L34.546 13.3565L34.6254 13.4137L34.7045 13.4708L34.8126 13.5182C34.8721 13.5443 34.9718 13.5779 35.0343 13.5929C35.0968 13.608 35.198 13.6261 35.2594 13.6332C35.3209 13.6403 35.4484 13.6469 35.5424 13.6479ZM35.8496 13.1974L35.7134 13.2036L35.6215 13.1891C35.5601 13.1782 35.5001 13.1622 35.442 13.1415L35.3542 13.1082L35.2803 13.055L35.2067 13.0015L35.1518 12.9087L35.0968 12.8163L35.0719 12.7131L35.0468 12.6099L35.0489 12.4033L35.0512 12.197L35.0801 12.0143L35.1091 11.8318L35.1542 11.7047C35.1791 11.6347 35.1996 11.5688 35.1996 11.5581C35.1998 11.5474 35.2245 11.4903 35.2542 11.431C35.284 11.3719 35.3337 11.2806 35.365 11.2281C35.396 11.1757 35.4539 11.0942 35.4938 11.0466C35.5337 10.9993 35.6049 10.9276 35.6518 10.8879C35.6988 10.848 35.7737 10.7957 35.8179 10.7715L35.8986 10.728L36.0403 10.6846L36.1819 10.6414H36.7764L36.9477 10.6832C37.0417 10.7062 37.1417 10.7328 37.1698 10.7425L37.2206 10.7601L37.2106 10.8036C37.1941 10.8814 37.1783 10.9595 37.1632 11.0375C37.1025 11.3424 37.039 11.6468 36.973 11.9507C36.9489 12.0626 36.9215 12.1738 36.8907 12.2843C36.8708 12.3499 36.843 12.4404 36.8289 12.486C36.8083 12.5457 36.783 12.6041 36.7531 12.6607C36.7222 12.7172 36.6867 12.7718 36.6471 12.8241C36.6123 12.8673 36.5732 12.9078 36.5303 12.9451C36.48 12.9841 36.4268 13.02 36.3711 13.0527L36.2719 13.1108L36.1287 13.151L35.9855 13.1909L35.8496 13.1974ZM9.3338 13.5705H9.77169L9.80297 13.3998C9.82028 13.306 9.85402 13.1256 9.87795 12.9989L9.97715 12.4749C10.0078 12.3134 10.0534 12.0739 10.0789 11.9429C10.1042 11.8118 10.1446 11.6087 10.1685 11.4911C10.1925 11.3739 10.2166 11.2737 10.2221 11.2689L10.2323 11.26H12.5864L12.5995 11.2715L12.6126 11.283L12.5921 11.3747C12.5809 11.4251 12.5478 11.5809 12.5184 11.7206L12.4087 12.2446C12.3777 12.3931 12.333 12.6038 12.3093 12.7131C12.2853 12.8222 12.2374 13.051 12.2023 13.2214C12.1674 13.3915 12.1387 13.54 12.1389 13.5507V13.5707H13.0181L13.0377 13.4714C13.0484 13.4168 13.0655 13.3185 13.0758 13.253C13.0929 13.1497 13.1113 13.0465 13.1312 12.9434C13.1513 12.8385 13.1916 12.6313 13.2206 12.4828C13.2495 12.3344 13.2981 12.0806 13.3284 11.9191C13.3587 11.7575 13.4037 11.5218 13.4286 11.3949C13.4532 11.2683 13.4972 11.0541 13.5262 10.9185C13.597 10.588 13.6663 10.2571 13.7342 9.92609L13.8354 9.4338C13.8662 9.28536 13.905 9.11191 13.9216 9.04856L13.9522 8.93358H13.0621L13.0497 9.009C13.0432 9.05038 13.0217 9.16652 13.0021 9.26718C12.9825 9.3674 12.9469 9.54609 12.9227 9.66405C12.8988 9.78187 12.855 10.007 12.8254 10.1643C12.7957 10.3216 12.7581 10.5295 12.7414 10.6267L12.7115 10.8034L12.0582 10.8151L11.4048 10.8266L10.8574 10.8153C10.5564 10.8091 10.3088 10.8026 10.3067 10.8011C10.3046 10.7995 10.3149 10.7379 10.3295 10.6641C10.3441 10.59 10.3715 10.4615 10.3907 10.3787C10.4098 10.2955 10.4461 10.1314 10.4714 10.0134C10.4965 9.8954 10.5249 9.749 10.5345 9.68791C10.5443 9.62674 10.5646 9.52492 10.5797 9.46158C10.5947 9.3983 10.6235 9.26478 10.6435 9.16514C10.6634 9.06551 10.6846 8.97249 10.6905 8.9586L10.7017 8.93358H9.81846L9.79017 9.07256C9.77468 9.149 9.75388 9.25787 9.74413 9.3146C9.73431 9.37147 9.70144 9.55358 9.67111 9.71954C9.64057 9.88551 9.59584 10.1322 9.57126 10.2675L9.47249 10.8153C9.44239 10.9813 9.39293 11.2386 9.3626 11.387C9.33199 11.5355 9.28799 11.7462 9.26449 11.8555L9.16529 12.316C9.13402 12.4602 9.08864 12.6639 9.06428 12.7686C9.03984 12.8734 9.01271 12.9949 9.00428 13.0386C8.99562 13.0824 8.96784 13.2083 8.94224 13.3185C8.91671 13.4287 8.89599 13.5307 8.89599 13.5449V13.5707L9.3338 13.5705ZM14.18 13.5705H14.5675L14.5784 13.5454C14.5862 13.5191 14.5899 13.492 14.5894 13.4648C14.5894 13.4345 14.6135 13.276 14.6434 13.1129C14.7115 12.7411 14.7811 12.3697 14.8523 11.9984C14.8769 11.8719 14.9203 11.6609 14.949 11.53C14.9777 11.399 15.0217 11.1953 15.047 11.0773C15.0721 10.9595 15.1214 10.7353 15.1566 10.5789C15.1919 10.4226 15.2251 10.2885 15.2307 10.2809L15.2405 10.2667L14.8484 10.2712L14.4566 10.2754L14.4241 10.4819C14.406 10.5955 14.367 10.8206 14.3367 10.9821L14.2359 11.53C14.2103 11.6696 14.1661 11.8949 14.1371 12.0302L14.039 12.4907C13.9775 12.7768 13.9142 13.0627 13.8491 13.3482C13.8299 13.4313 13.8092 13.5152 13.8035 13.535L13.7923 13.5707L14.18 13.5705ZM23.2434 13.5705H23.6281L23.6395 13.4273C23.6459 13.3482 23.6678 13.1893 23.6881 13.0737C23.7081 12.9584 23.7497 12.7282 23.7802 12.5622C23.8105 12.3963 23.8671 12.12 23.9057 11.9483C23.9444 11.7769 23.9882 11.6061 24.003 11.5691C24.0179 11.5323 24.0299 11.4926 24.0299 11.4804C24.0299 11.4683 24.0587 11.4032 24.094 11.3354C24.1369 11.2558 24.1859 11.1789 24.2404 11.1051L24.3225 10.9976L24.4324 10.9081L24.5424 10.8183L24.6781 10.7612L24.8135 10.7041L25.0209 10.7051L25.2279 10.7062L25.3299 10.733C25.3857 10.7477 25.4366 10.7598 25.4423 10.7598C25.4482 10.7598 25.4532 10.7429 25.4532 10.7223C25.4532 10.7015 25.4733 10.5971 25.4982 10.4898C25.523 10.3825 25.5433 10.2903 25.5433 10.2849C25.5433 10.2792 25.4845 10.259 25.4126 10.2397C25.3403 10.2203 25.2665 10.2054 25.1919 10.1949L25.1018 10.185L24.9848 10.2034C24.9203 10.2134 24.8245 10.2357 24.7718 10.2526C24.7193 10.2697 24.6322 10.3083 24.5787 10.3385C24.5128 10.3774 24.4508 10.4213 24.3934 10.4696C24.3287 10.5283 24.2684 10.5908 24.2129 10.6566L24.0942 10.7995L24.0687 10.8313L24.0842 10.7677C24.0929 10.7328 24.1169 10.6185 24.1376 10.5136C24.1581 10.4087 24.1796 10.3105 24.1848 10.2953L24.1944 10.2675H23.5075V10.3059C23.5075 10.3268 23.487 10.4537 23.4623 10.5876C23.4374 10.7215 23.3966 10.9456 23.3715 11.0854C23.3465 11.2251 23.3017 11.4681 23.2723 11.6252C23.2427 11.7825 23.1982 12.0076 23.1729 12.1254C23.0796 12.5619 22.9834 12.998 22.8842 13.4335C22.874 13.4742 22.8656 13.5153 22.8589 13.5566V13.5707L23.2434 13.5705ZM32.3745 13.5705H32.7679V13.3587L32.813 13.0955C32.8381 12.9507 32.8792 12.7216 32.9045 12.5862C32.9298 12.4509 32.9615 12.2793 32.9747 12.2051C32.988 12.1309 33.023 11.9701 33.0527 11.8477C33.0824 11.7255 33.1195 11.5934 33.1353 11.554C33.1507 11.5147 33.1638 11.4734 33.1638 11.4623C33.164 11.4512 33.1975 11.3798 33.2384 11.3038L33.3127 11.1654L33.4105 11.0507L33.5086 10.9359L33.6158 10.8649C33.6746 10.8259 33.7695 10.7742 33.8263 10.7499L33.9298 10.7056L34.1551 10.707L34.3802 10.7084L34.4689 10.7334L34.5577 10.7584L34.5725 10.7504L34.5875 10.7423L34.5882 10.6917C34.5886 10.6636 34.6078 10.56 34.6311 10.4611L34.6731 10.2812L34.6259 10.2651C34.5804 10.2502 34.5345 10.2363 34.4883 10.2235L34.3982 10.1985L34.2361 10.199L34.0739 10.1993L33.9419 10.2403L33.8097 10.2812L33.7105 10.3341L33.6113 10.3867L33.4822 10.5057L33.3533 10.6249L33.284 10.7175C33.2459 10.7681 33.2123 10.8074 33.2094 10.8048C33.2065 10.8019 33.2272 10.6959 33.2555 10.5694C33.2838 10.4428 33.307 10.323 33.3075 10.3034L33.3083 10.2677H32.6417V10.2778C32.6417 10.2835 32.6251 10.3851 32.6048 10.5041C32.5845 10.6231 32.5437 10.8524 32.514 11.0142C32.4846 11.1755 32.4361 11.4365 32.4064 11.5938C32.3768 11.7511 32.3321 11.976 32.307 12.0939C32.2743 12.2475 32.2414 12.4009 32.2082 12.5544C32.179 12.6899 32.1309 12.9078 32.1013 13.0388L32.0144 13.4241L31.9811 13.5709L32.3745 13.5705ZM14.9419 9.69532L15.0514 9.69576L15.1381 9.66951L15.2251 9.64354L15.302 9.57474L15.3791 9.50594L15.4197 9.41423L15.4601 9.32267L15.4617 9.18754L15.463 9.05256L15.427 8.99998L15.391 8.94732L15.3283 8.913L15.2656 8.87874H14.9946L14.9025 8.91503L14.8101 8.95132L14.7568 9.00194L14.7032 9.05256L14.6612 9.1399L14.619 9.22725L14.611 9.3634L14.6033 9.49954L14.6422 9.56711L14.6815 9.63467L14.757 9.66492L14.8325 9.69518L14.9419 9.69532Z"
													fill="#B3131B" />
											</svg>
										</template>

										<template v-else-if="bandeira.name === 'AM EX'">
											<svg class="w-[18px] h-[18px] lg:w-[24px] lg:h-[24px]" viewBox="0 0 24 24"
												fill="none" xmlns="http://www.w3.org/2000/svg">
												<path fill-rule="evenodd" clip-rule="evenodd"
													d="M2.11938 2.4823H21.1539V21.5168H2.11938V2.4823Z"
													fill="#016FD0" />
												<path fill-rule="evenodd" clip-rule="evenodd"
													d="M9.01428 18.5047V11.684L21.153 11.6946V13.579L19.7498 15.0785L21.153 16.5917V18.5153H18.9133L17.7226 17.2019L16.5403 18.5206L9.01428 18.5047Z"
													fill="white" />
												<path fill-rule="evenodd" clip-rule="evenodd"
													d="M9.82397 17.7562V12.434H14.3373V13.6596H11.2854V14.4918H14.2643V15.6973H11.2854V16.5147H14.3373V17.7562H9.82397Z"
													fill="#016FD0" />
												<path fill-rule="evenodd" clip-rule="evenodd"
													d="M14.314 17.7562L16.8117 15.0914L14.314 12.434H16.2471L17.773 14.1206L19.3031 12.434H21.1527V12.4763L18.7088 15.0914L21.1527 17.68V17.7562H19.2841L17.7317 16.0526L16.1942 17.7562H14.314Z"
													fill="#016FD0" />
												<path fill-rule="evenodd" clip-rule="evenodd"
													d="M9.58055 5.63074H12.5067L13.5345 7.96464V5.63074H17.1468L17.7697 7.37984L18.3946 5.63074H21.1536V12.4514H6.57532L9.58055 5.63074Z"
													fill="white" />
												<path fill-rule="evenodd" clip-rule="evenodd"
													d="M10.1355 6.37219L7.77417 11.6901H9.39424L9.83948 10.6253H12.2536L12.6988 11.6901H14.3591L12.0061 6.37219H10.1355ZM10.3385 9.43138L11.0471 7.73836L11.7545 9.43138H10.3385Z"
													fill="#016FD0" />
												<path fill-rule="evenodd" clip-rule="evenodd"
													d="M14.3353 11.6894V6.37146L16.6121 6.37993L17.7838 9.64859L18.9629 6.37146H21.1539V11.6894L19.7443 11.7022V8.04859L18.4129 11.6894H17.1261L15.7672 8.03696V11.6894H14.3353Z"
													fill="#016FD0" />
											</svg>
										</template>

										<template v-else-if="bandeira.name === 'G Pay'">
											<svg class="w-[32px] h-[18px] lg:w-[44px] lg:h-[24px]" viewBox="0 0 44 24"
												fill="none" xmlns="http://www.w3.org/2000/svg">
												<mask id="mask0_1_527" style="mask-type:luminance"
													maskUnits="userSpaceOnUse" x="5" y="4" width="33" height="16">
													<path d="M37.7381 4.37524H5.72009V19.6239H37.7381V4.37524Z"
														fill="white" />
												</mask>
												<g mask="url(#mask0_1_527)">
													<path
														d="M20.8495 12.2599V15.9821H19.6488V6.77686H22.7706C23.531 6.77686 24.2514 7.05701 24.8118 7.57733C25.372 8.05755 25.6522 8.77797 25.6522 9.53842C25.6522 10.2989 25.372 10.9792 24.8118 11.4995C24.2514 12.0198 23.571 12.3 22.7706 12.3L20.8495 12.2599ZM20.8495 7.89745V11.0993H22.8506C23.2909 11.0993 23.7311 10.9392 24.0113 10.6191C24.6517 10.0187 24.6517 9.01811 24.0513 8.41776L24.0113 8.37773C23.6911 8.05754 23.2909 7.85743 22.8506 7.89745H20.8495Z"
														fill="black" />
													<path
														d="M28.4131 9.49841C29.2936 9.49841 29.974 9.73855 30.4943 10.2188C31.0146 10.6991 31.2547 11.3395 31.2547 12.14V15.9821H30.1341V15.1016H30.094C29.6137 15.822 28.9333 16.1822 28.1329 16.1822C27.4525 16.1822 26.8522 15.9821 26.3719 15.5818C25.9317 15.1817 25.6515 14.6214 25.6515 14.021C25.6515 13.3806 25.8916 12.8603 26.3719 12.4601C26.8522 12.0599 27.5326 11.8998 28.3331 11.8998C29.0534 11.8998 29.6137 12.0199 30.054 12.3V12.0199C30.054 11.6197 29.8939 11.2194 29.5737 10.9793C29.2535 10.6991 28.8533 10.539 28.4131 10.539C27.7327 10.539 27.2124 10.8192 26.8522 11.3795L25.8116 10.7391C26.452 9.89865 27.2925 9.49841 28.4131 9.49841ZM26.8922 14.061C26.8922 14.3812 27.0523 14.6614 27.2925 14.8215C27.5726 15.0216 27.8928 15.1417 28.213 15.1417C28.6932 15.1417 29.1735 14.9415 29.5337 14.5813C29.9339 14.2211 30.1341 13.7809 30.1341 13.3006C29.7738 13.0204 29.2535 12.8603 28.5731 12.8603C28.0929 12.8603 27.6927 12.9804 27.3725 13.2205C27.0523 13.4206 26.8922 13.7008 26.8922 14.061Z"
														fill="black" />
													<path
														d="M37.7379 9.69836L33.7756 18.7835H32.5749L34.0558 15.6217L31.4543 9.73839H32.735L34.6161 14.261H34.6562L36.4972 9.73839H37.7379V9.69836Z"
														fill="black" />
													<path
														d="M16.0874 11.4593C16.0874 11.0991 16.0474 10.7389 16.0074 10.3787H11.0045V12.4198H13.8461C13.7261 13.0602 13.3659 13.6605 12.8056 14.0207V15.3414H14.5265C15.5271 14.421 16.0874 13.0602 16.0874 11.4593Z"
														fill="#4285F4" />
													<path
														d="M11.0024 16.6223C12.4433 16.6223 13.6439 16.142 14.5244 15.3415L12.8034 14.0208C12.3232 14.341 11.7229 14.5411 11.0024 14.5411C9.64165 14.5411 8.44101 13.6206 8.04078 12.3398H6.27979V13.7006C7.20028 15.5016 9.00134 16.6223 11.0024 16.6223Z"
														fill="#34A853" />
													<path
														d="M8.04031 12.3399C7.80017 11.6995 7.80017 10.9791 8.04031 10.2988V8.93799H6.27931C5.51888 10.4189 5.51888 12.1798 6.27931 13.7006L8.04031 12.3399Z"
														fill="#FABB05" />
													<path
														d="M11.0038 8.13775C11.7642 8.13775 12.4847 8.41791 13.0449 8.93822L14.5658 7.4174C13.6053 6.53687 12.3246 6.01657 11.0438 6.0566C9.04271 6.0566 7.20163 7.17726 6.32117 8.97824L8.08215 10.339C8.44236 9.05829 9.64306 8.13775 11.0038 8.13775Z"
														fill="#E94235" />
												</g>
											</svg>
										</template>

										<template v-else-if="bandeira.name === 'Samsung Pay'">
											<svg class="w-[33px] h-[18px] lg:w-[46px] lg:h-[24px]" viewBox="0 0 46 24"
												fill="none" xmlns="http://www.w3.org/2000/svg">
												<mask id="mask0_1_539" style="mask-type:luminance"
													maskUnits="userSpaceOnUse" x="5" y="4" width="35" height="16">
													<path d="M39.0331 4.5H5.625V19.5H39.0331V4.5Z" fill="white" />
												</mask>
												<g mask="url(#mask0_1_539)">
													<mask id="mask1_1_539" style="mask-type:luminance"
														maskUnits="userSpaceOnUse" x="2" y="1" width="40" height="20">
														<path d="M2.96924 1.84131H41.6948V20.2707H2.96924V1.84131Z"
															fill="white" />
													</mask>
													<g mask="url(#mask1_1_539)">
														<path
															d="M7.31496 15.2695H6.58771V13.3884H7.31496C7.88128 13.3884 8.26596 13.8081 8.26596 14.3254C8.26596 14.85 7.88128 15.2695 7.31496 15.2695ZM7.38486 12.5076H5.63672V17.6119H6.58771V16.1505H7.37781C8.44063 16.1505 9.22386 15.3395 9.22386 14.3254C9.22386 13.3187 8.44063 12.5076 7.38486 12.5076Z"
															fill="black" />
														<path
															d="M11.645 16.8291C10.9458 16.8291 10.4076 16.2697 10.4076 15.5566C10.4076 14.8504 10.9458 14.2981 11.645 14.2981C12.3442 14.2981 12.8687 14.8643 12.8687 15.5566C12.8687 16.2628 12.3442 16.8291 11.645 16.8291ZM12.8198 14.0113C12.5261 13.6617 12.0436 13.4377 11.5054 13.4377C10.4704 13.4377 9.47754 14.2141 9.47754 15.5705C9.47754 16.9062 10.5053 17.6962 11.4912 17.6962C12.0296 17.6962 12.5191 17.4585 12.8198 17.0809V17.6122H13.7638V13.5217H12.8198V14.0113Z"
															fill="black" />
														<path
															d="M16.2883 16.2761L15.1486 13.5212H14.1416L15.8199 17.4089L14.9596 19.4996H15.9178L18.3581 13.5212H17.3651L16.2883 16.2761Z"
															fill="black" />
														<path
															d="M33.2811 8.39943H33.2548L32.1575 4.65161H30.3853V9.37683H31.5596L31.4937 5.4997H31.5199L32.6986 9.37683H34.3987V4.65161H33.2163L33.2811 8.39943Z"
															fill="black" />
														<path
															d="M11.7989 5.09392L11.8256 5.0935L12.4621 9.42595H13.7447L12.8647 4.65161H10.7405L9.85547 9.42595H11.146L11.7989 5.09392Z"
															fill="black" />
														<path
															d="M17.9536 4.65161L17.3647 8.30237H17.3375L16.7495 4.65161H14.802L14.6968 9.42595H15.8918L15.9217 5.13396H15.948L16.7456 9.42595H17.957L18.7549 5.13439L18.7813 5.13396L18.8111 9.42595H20.0054L19.9001 4.65161H17.9536Z"
															fill="black" />
														<path
															d="M7.36466 9.6133C8.22859 9.6133 8.93884 9.3187 9.05231 8.5216C9.11044 8.10955 9.06776 7.83956 9.04721 7.73913C8.84569 6.73927 7.03427 6.44107 6.89908 5.88194C6.87636 5.78498 6.88188 5.68441 6.89398 5.631C6.92776 5.47775 7.03151 5.31039 7.33001 5.31039C7.60976 5.31039 7.77364 5.48285 7.77364 5.74303V6.0388H8.96516V5.70247C8.96516 4.66258 8.03096 4.5 7.35521 4.5C6.50668 4.5 5.81248 4.7812 5.686 5.55985C5.65138 5.77277 5.6467 5.96339 5.69693 6.20383C5.90401 7.17967 7.60076 7.46162 7.84669 8.0797C7.89341 8.19513 7.87856 8.34318 7.85614 8.43281C7.81729 8.59188 7.70914 8.75418 7.39099 8.75418C7.09247 8.75418 6.91129 8.58123 6.91129 8.32121L6.91013 7.85845H5.62981L5.62939 8.22648C5.62939 9.29163 6.46739 9.6133 7.36466 9.6133Z"
															fill="black" />
														<path
															d="M22.4318 5.89399C22.4086 5.79862 22.4144 5.69922 22.4258 5.64612C22.4597 5.49448 22.5626 5.3287 22.8576 5.3287C23.1353 5.3287 23.2972 5.49883 23.2972 5.75699V6.04924H24.477V5.71685C24.477 4.68727 23.5518 4.52649 22.8828 4.52649C22.0433 4.52649 21.3558 4.80419 21.2308 5.57539C21.1963 5.7863 21.1915 5.97502 21.2418 6.21268C21.4462 7.1794 23.1264 7.45837 23.3699 8.0704C23.4156 8.18507 23.4014 8.33087 23.3789 8.42012C23.3405 8.57725 23.2337 8.73745 22.9186 8.73745C22.6227 8.73745 22.4432 8.56697 22.4432 8.3089L22.4424 7.85132H21.1746L21.1743 8.21575C21.1743 9.27017 22.0044 9.5884 22.8926 9.5884C23.7479 9.5884 24.4511 9.29695 24.5633 8.50757C24.6207 8.0998 24.5788 7.83272 24.5582 7.7326C24.3584 6.7425 22.565 6.44759 22.4318 5.89399Z"
															fill="black" />
														<path
															d="M29.0543 4.65161H27.8458V8.18371C27.8462 8.24499 27.8445 8.31459 27.8345 8.36604C27.8108 8.48349 27.7087 8.71209 27.3767 8.71209C27.0444 8.71209 26.9417 8.48349 26.9186 8.36604C26.9084 8.31459 26.906 8.24499 26.9072 8.18371V4.65161H25.6996V8.07489C25.6973 8.16294 25.7063 8.34249 25.7106 8.38914C25.7943 9.28021 26.4987 9.56896 27.3767 9.56896C28.2544 9.56896 28.9588 9.28021 29.042 8.38914C29.0468 8.34249 29.0558 8.16294 29.0543 8.07489V4.65161Z"
															fill="black" />
														<path
															d="M37.3451 6.74478V7.44091H37.8346V8.13429C37.8355 8.19466 37.8331 8.26149 37.8232 8.31534C37.8016 8.44306 37.681 8.66199 37.3352 8.66199C36.99 8.66199 36.8695 8.44306 36.847 8.31541C36.8379 8.26156 36.8356 8.19481 36.8359 8.13429V5.95159C36.8359 5.87386 36.8416 5.78848 36.8552 5.72561C36.8815 5.60784 36.9837 5.37919 37.3309 5.37919C37.696 5.37919 37.7867 5.62036 37.8082 5.72561C37.8225 5.79442 37.8232 5.90752 37.8232 5.90752V6.17514H39.0274V6.01606C39.0274 6.01606 39.0316 5.85379 39.0163 5.70172C38.9283 4.80637 38.1871 4.52197 37.3405 4.52197C36.4933 4.52197 35.7709 4.80913 35.6645 5.70172C35.6547 5.78147 35.6443 5.93121 35.6443 6.01606L35.644 8.02464C35.6443 8.11464 35.6468 8.18026 35.6606 8.33904C35.7403 9.20874 36.4933 9.51909 37.34 9.51909C38.1862 9.51909 38.9402 9.20866 39.0191 8.33904C39.0335 8.18026 39.0355 8.11464 39.0364 8.02464V6.74478H37.3451Z"
															fill="black" />
													</g>
												</g>
											</svg>
										</template>

										<template v-else-if="bandeira.name === 'Apple Pay'">
											<svg class="w-[34px] h-[18px] lg:w-[47px] lg:h-[24px]" viewBox="0 0 47 24"
												fill="none" xmlns="http://www.w3.org/2000/svg">
												<path
													d="M11.6907 6.76128C11.2863 7.2381 10.6392 7.61427 9.99202 7.56055C9.91112 6.91576 10.2279 6.23069 10.5987 5.80756C11.0031 5.31728 11.711 4.96803 12.2839 4.94116C12.3514 5.61279 12.0884 6.27099 11.6907 6.76128ZM12.2772 7.6881C11.3402 7.63439 10.538 8.21871 10.0931 8.21871C9.64148 8.21871 8.96065 7.715 8.21912 7.72841C7.25517 7.74189 6.35862 8.28584 5.86653 9.15224C4.85538 10.8851 5.60363 13.4507 6.58107 14.8611C7.05971 15.5596 7.63268 16.3253 8.38762 16.2984C9.10218 16.2716 9.38531 15.835 10.2482 15.835C11.1178 15.835 11.3671 16.2984 12.1222 16.285C12.9041 16.2716 13.3962 15.5865 13.8748 14.888C14.4208 14.0955 14.6433 13.3231 14.6568 13.2828C14.6433 13.2694 13.1467 12.6985 13.1333 10.9791C13.1198 9.54182 14.3129 8.85676 14.3669 8.81646C13.6928 7.82243 12.6412 7.715 12.2772 7.6881ZM17.6902 5.7404V16.2111H19.3215V12.6313H21.5797C23.6424 12.6313 25.0918 11.2209 25.0918 9.17914C25.0918 7.13737 23.6694 5.7404 21.6336 5.7404H17.6902ZM19.3215 7.11055H21.2022C22.6179 7.11055 23.4267 7.86274 23.4267 9.18584C23.4267 10.5089 22.6179 11.2679 21.1955 11.2679H19.3215V7.11055ZM28.0713 16.2917C29.0959 16.2917 30.0464 15.7746 30.4778 14.9552H30.5115V16.2111H32.0215V10.9993C32.0215 9.4881 30.8081 8.5142 28.9409 8.5142C27.2084 8.5142 25.9277 9.50151 25.8804 10.8582H27.35C27.4714 10.2135 28.0713 9.79029 28.8937 9.79029C29.8913 9.79029 30.4508 10.2538 30.4508 11.1067V11.6843L28.4151 11.8052C26.5209 11.9194 25.4962 12.6917 25.4962 14.035C25.4962 15.3917 26.5546 16.2917 28.0713 16.2917ZM28.5094 15.0492C27.6399 15.0492 27.0871 14.6328 27.0871 13.9947C27.0871 13.3365 27.6197 12.9537 28.6375 12.8933L30.4508 12.7791V13.3701C30.4508 14.3507 29.615 15.0492 28.5094 15.0492ZM34.0371 19.0588C35.6279 19.0588 36.3761 18.4544 37.03 16.6208L39.895 8.615H38.2366L36.3155 14.8007H36.2818L34.3606 8.615H32.6551L35.419 16.238L35.2707 16.7014C35.0212 17.4872 34.6168 17.7894 33.8955 17.7894C33.7674 17.7894 33.5179 17.776 33.4169 17.7626V19.0185C33.5112 19.0454 33.9157 19.0588 34.0371 19.0588Z"
													fill="black" />
											</svg>
										</template>

										<template v-else-if="bandeira.name === 'Hiper'">
											<svg class="w-[23px] h-[18px] lg:w-[32px] lg:h-[24px]" viewBox="0 0 32 24"
												fill="none" xmlns="http://www.w3.org/2000/svg">
												<mask id="mask0_1_560" style="mask-type:luminance"
													maskUnits="userSpaceOnUse" x="4" y="3" width="22" height="18">
													<path d="M25.4574 3.63208H4.36646V20.3594H25.4574V3.63208Z"
														fill="white" />
												</mask>
												<g mask="url(#mask0_1_560)">
													<path fill-rule="evenodd" clip-rule="evenodd"
														d="M7.1888 20.1249H24.3889C24.4598 20.0446 24.5145 20.0267 24.612 19.932C24.8461 19.7088 25.0182 19.4317 25.1121 19.1266C25.2728 18.6459 25.3081 18.1341 25.2148 17.637C25.0324 16.7204 24.7575 15.7644 24.5285 14.8442L22.4612 6.47967C22.2226 5.52366 21.6544 4.90329 20.9348 4.5074C19.9396 3.95875 19.1217 4.09 18.0009 4.35177C17.0337 4.57769 16.0954 4.80289 15.1313 5.02091C14.623 5.13638 7.20654 6.80743 6.56301 7.06275C5.77764 7.37329 5.10456 7.99867 4.81937 8.82486C4.48098 9.80667 4.87404 10.8345 5.10308 11.7639L6.49356 17.3286C6.71521 18.2509 6.996 19.1969 7.18807 20.1256L7.1888 20.1249Z"
														fill="#ED6B06" />
													<path fill-rule="evenodd" clip-rule="evenodd"
														d="M22.1178 13.2213L22.0816 12.6232L21.1219 12.6038L21.1514 16.0463L22.2619 16.0628C22.2604 15.4761 22.2471 14.8852 22.2619 14.3165C22.2782 13.6781 22.6705 13.4701 23.3391 13.5619L23.3376 12.5378C22.307 12.4919 22.293 13.0929 22.1178 13.2213ZM20.5634 15.9294L20.4266 15.2122C20.239 15.1835 19.7956 15.3915 19.2061 15.3097C18.8544 15.261 18.4288 15.0523 18.4584 14.688L20.7303 14.6829C20.8212 13.6739 20.5529 12.8842 19.7551 12.6152C18.9741 12.352 18.0143 12.6425 17.6006 13.4336C17.076 14.4376 17.4122 16.0843 19.1522 16.1345C19.7802 16.1525 20.0343 16.0571 20.5634 15.9294ZM7.02197 16.0556H8.14502V14.0819H9.96699V16.0556H11.09V11.2705H9.96699V13.1029H8.14429V11.2705H7.02124V16.0556H7.02197ZM14.4295 13.113C14.4022 12.4697 14.6512 12.6583 13.4417 12.6232L13.4779 15.294C13.2045 15.2825 13.0375 15.1434 12.9378 14.9512C12.7287 14.5509 12.7937 13.6488 12.7937 13.1804L12.793 12.6139H11.6863C11.6197 13.123 11.6515 14.7101 11.8089 15.1384C12.1281 16.009 12.8108 16.0485 13.4713 16.1625V17.4677C13.7254 17.4677 14.3711 17.5036 14.5751 17.4441V16.151C15.6685 16.1496 16.6571 16.1503 17.0132 15.0645C17.1186 14.7203 17.1493 14.3586 17.1034 14.0023C17.0118 13.2134 16.5167 12.6726 15.868 12.5629C15.2437 12.4575 14.9319 12.6583 14.51 13.0442C14.4844 13.0684 14.4575 13.0914 14.4295 13.113Z"
														fill="white" />
													<path fill-rule="evenodd" clip-rule="evenodd"
														d="M15.0499 13.4301C14.5786 13.5972 14.5756 14.0469 14.5756 14.5984C14.5756 14.7389 14.5763 14.8788 14.5741 15.0194C14.5719 15.1915 14.5261 15.2926 14.6672 15.3163C14.8106 15.34 15.3447 15.3249 15.477 15.2833C16.3355 15.0129 16.0961 13.0593 15.0499 13.4301ZM18.4375 13.9429H19.7304C19.6979 12.9976 18.5616 13.0177 18.4375 13.9429Z"
														fill="#ED6B06" />
													<path fill-rule="evenodd" clip-rule="evenodd"
														d="M12.2403 11.0787C12.318 11.0788 12.3949 11.0937 12.4667 11.1226C12.5385 11.1515 12.6037 11.1939 12.6586 11.2472C12.7135 11.3006 12.7571 11.364 12.7868 11.4337C12.8165 11.5034 12.8318 11.5782 12.8317 11.6536C12.8316 11.729 12.8163 11.8037 12.7865 11.8734C12.7567 11.943 12.7131 12.0064 12.6581 12.0597C12.6031 12.113 12.5379 12.1552 12.466 12.184C12.3942 12.2129 12.3172 12.2277 12.2396 12.2277C12.0825 12.2277 11.9321 12.1672 11.8211 12.0594C11.7101 11.9517 11.6477 11.8056 11.6477 11.6532C11.6477 11.5008 11.7101 11.3547 11.8211 11.247C11.9321 11.1392 12.0825 11.0787 12.2396 11.0787H12.2403Z"
														fill="#FFED00" />
												</g>
											</svg>
										</template>

										<template v-else>
											{{ bandeira.logo }}
										</template>
									</div>
								</div>
							</div>
						</CardContent>
					</Card>
				</div>
			</main>

			<!-- Botão de Ajuda Flutuante -->
			<button
				class="fixed bottom-6 right-6 z-50 h-14 w-14 rounded-full bg-blue-600 hover:bg-blue-700 text-white shadow-lg flex items-center justify-center transition-all">
				<HelpCircle class="h-7 w-7" />
			</button>
		</div>
	</div>
</template>
