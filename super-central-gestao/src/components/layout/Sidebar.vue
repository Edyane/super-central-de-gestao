<script setup>
import { ref } from 'vue'
import { Menu, X, Home, Users, Settings, BarChart3, FileText } from 'lucide-vue-next'
import Button from '@/components/ui/Button.vue'

const isMobileMenuOpen = ref(false)

const menuItems = [
	{ icon: Home, label: 'Dashboard', href: '/' },
	{ icon: BarChart3, label: 'Relatórios', href: '/relatorios' },
	{ icon: Users, label: 'Usuários', href: '/usuarios' },
	{ icon: FileText, label: 'Documentos', href: '/documentos' },
	{ icon: Settings, label: 'Configurações', href: '/configuracoes' }
]

const toggleMobileMenu = () => {
	isMobileMenuOpen.value = !isMobileMenuOpen.value
}
</script>

<template>
	<!-- Mobile Menu Button - Oculto no mobile conforme imagem -->
	<div class="hidden lg:hidden fixed top-4 left-4 z-50">
		<Button variant="ghost" size="icon" @click="toggleMobileMenu">
			<Menu v-if="!isMobileMenuOpen" class="h-6 w-6" />
			<X v-else class="h-6 w-6" />
		</Button>
	</div>

	<!-- Sidebar -->
	<aside :class="[
		'fixed top-16 left-0 z-40 h-[calc(100vh-4rem)] transition-transform bg-card border-r border-border',
		'w-64 transform',
		isMobileMenuOpen ? 'translate-x-0' : '-translate-x-full',
		'lg:translate-x-0'
	]">
		<div class="h-full px-3 py-4 overflow-y-auto">
			<!-- Navigation -->
			<nav class="space-y-2">
				<a v-for="item in menuItems" :key="item.href" :href="item.href"
					class="flex items-center px-3 py-2 text-sm font-medium rounded-lg transition-colors hover:bg-accent hover:text-accent-foreground text-muted-foreground active:bg-accent"
					@click="isMobileMenuOpen = false">
					<component :is="item.icon" class="mr-3 h-5 w-5 flex-shrink-0" />
					<span>{{ item.label }}</span>
				</a>
			</nav>
		</div>
	</aside>

	<!-- Overlay for mobile -->
	<div v-if="isMobileMenuOpen" class="fixed inset-0 bg-black/50 z-30 lg:hidden" @click="toggleMobileMenu" />
</template>
