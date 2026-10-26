<script setup lang="ts">
import { computed, nextTick, onMounted, onUnmounted, ref, watch } from 'vue'

type MenuItem = { id: number; name: string; category: string; detail: string; image: string; tag: string }

const menu = ref<MenuItem[]>([
  { id: 1, name: 'Mango Matcha Latte', category: 'Boissons', detail: 'La rencontre du matcha et de la mangue, fraîche et délicatement fruitée.', image: '/images/arko/matcha-mango.jpg', tag: 'MATCHA · MANGUE' },
  { id: 2, name: 'Iced Coffee & Matcha', category: 'Boissons', detail: 'Une pause glacée, entre café intense et douceur du matcha.', image: '/images/arko/pistachio-matcha.jpg', tag: 'MATCHA · CAFÉ GLACÉ' },
  { id: 3, name: 'Canette Cake Pistache', category: 'Desserts', detail: 'Toute la gourmandise de la pistache dans un format à emporter.', image: '/images/arko/pistachio-cake.jpg', tag: 'PISTACHE' },
  { id: 4, name: 'Canette Cake Framboise', category: 'Desserts', detail: 'Des couches fruitées et une jolie touche de framboise.', image: '/images/arko/raspberry-cake.jpg', tag: 'FRAMBOISE' },
  { id: 5, name: 'Les Canette Cakes', category: 'Desserts', detail: 'Le format généreux à partager… ou à garder pour soi.', image: '/images/arko/canette-cakes.jpg', tag: 'LES FAVORIS DE LA MAISON' },
  { id: 6, name: 'Douceur Pistache', category: 'Desserts', detail: 'Une pause crémeuse aux notes de pistache et de matcha.', image: '/images/arko/pistachio-matcha.jpg', tag: 'PISTACHE · MATCHA' },
])
const filters = ['Tout', 'Desserts', 'Boissons']
const activeFilter = ref('Tout')
const menuOpen = ref(false)
const visibleMenu = computed(() => activeFilter.value === 'Tout' ? menu.value : menu.value.filter(item => item.category === activeFilter.value))
const phone = '+21627888597'
const mapUrl = 'https://www.google.com/maps/place/Arko+twins/@33.5052826,11.1091594,20z/data=!4m6!3m5!1s0x13aaefb8eacf13bf:0xa9b57d6c87c32700!8m2!3d33.5052651!4d11.1093094!16s%2Fg%2F11l6c52fxh?entry=ttu'
const whatsappLink = (name: string) => `https://wa.me/${phone.replace('+', '')}?text=${encodeURIComponent(`Bonjour Arko twins, je souhaite en savoir plus sur ${name}.`)}`

let revealObserver: IntersectionObserver | undefined
function observeRevealElements() {
  document.querySelectorAll<HTMLElement>('.reveal-on-scroll:not(.is-visible)').forEach((element) => revealObserver?.observe(element))
}

onMounted(() => {
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    document.querySelectorAll('.reveal-on-scroll').forEach((element) => element.classList.add('is-visible'))
    return
  }
  revealObserver = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('is-visible')
        revealObserver?.unobserve(entry.target)
      }
    })
  }, { threshold: 0.1, rootMargin: '0px 0px -6% 0px' })
  observeRevealElements()
})

watch(visibleMenu, async () => {
  await nextTick()
  observeRevealElements()
})

onUnmounted(() => revealObserver?.disconnect())
</script>

<template>
  <div class="site-shell">
    <div class="topline"><span>ARKO TWINS · ZARZIS</span><span>Le bonheur se partage à la petite cuillère</span><a href="#visite">NOUS TROUVER <span>↗</span></a></div>
    <header class="header">
      <a href="#accueil" class="brand" aria-label="Arko twins, accueil"><span class="brand-mark brand-logo"><img src="/images/arko/logo.jpg" alt="" /></span><span class="brand-name">arko <i>twins</i><small>PÂTISSERIE · CAFÉ</small></span></a>
      <button class="menu-toggle" type="button" :aria-expanded="menuOpen" aria-label="Ouvrir le menu" @click="menuOpen = !menuOpen"><span></span><span></span></button>
      <nav class="nav" :class="{ 'nav-open': menuOpen }"><a href="#carte" @click="menuOpen = false">La carte</a><a href="#histoire" @click="menuOpen = false">Notre adresse</a><a href="#visite" @click="menuOpen = false">Nous trouver</a></nav>
      <a class="header-order" :href="`tel:${phone}`"><span>Appeler</span><span class="phone-mark">↗</span></a>
    </header>

    <main>
      <section id="accueil" class="hero">
        <div class="hero-copy">
          <div class="hero-label"><span></span> PÂTISSERIE · CAFÉ · ZARZIS</div>
          <h1>La douceur<br />prend son <em>temps.</em></h1>
          <p>Des desserts qui donnent envie de ralentir, des boissons fraîches à savourer et une adresse chaleureuse au cœur de Zarzis.</p>
          <div class="hero-actions"><a class="button button-dark" :href="whatsappLink('la carte du jour et les disponibilités')" target="_blank" rel="noreferrer">Commander sur WhatsApp <span>↗</span></a><a class="quiet-link" href="#carte">Explorer la carte <span>↓</span></a></div>
          <div class="hero-aside"><span class="sunflower">✳</span><span>UNE PAUSE DOUCE<br />AU CŒUR DE ZARZIS</span></div>
          <span class="hero-slogan">Votre pause préférée<br /><em>commence ici.</em></span>
        </div>
        <div class="hero-art">
          <div class="hero-photo-frame"><img src="/images/arko/pistachio-matcha.jpg" alt="Une douceur à la pistache et un matcha glacé photographiés par Arko twins" fetchpriority="high" /></div>
          <div class="floating-note"><span>FAIT POUR</span><span>LES<br />GOURMANDS</span><span>♡</span></div>
          <span class="photo-caption">UNE PETITE DOUCEUR, UN GRAND SOURIRE.</span>
        </div>
        <div class="hero-bottom"><span>AVENUE MOHAMED V · ZARZIS</span><span>OUVERT 7 JOURS SUR 7 <i>✳</i></span></div>
      </section>

      <section id="histoire" class="intro reveal-on-scroll">
        <div class="intro-top"><span>BIENVENUE CHEZ ARKO TWINS</span><span>Le goût des bons moments</span></div>
        <div class="intro-content"><span class="intro-side">UNE PAUSE À VOTRE RYTHME<br />AU CŒUR DE ZARZIS</span><h2>Un café, une douceur,<br />et le plaisir de <em>rester.</em></h2><p>Matcha à la mangue, desserts en canette et autres envies sucrées : choisissez votre pause, puis écrivez-nous pour connaître la carte du jour.</p></div>
        <div class="intro-bottom"><span>DESSERTS · BOISSONS · CRÉATIONS</span><span class="little-flower">✳</span><span>ZARZIS, TUNISIE</span></div>
      </section>

      <section id="carte" class="menu-section">
        <div class="section-head reveal-on-scroll"><div><span class="eyebrow">LES FAVORIS DE LA MAISON</span><h2>À savourer <em>maintenant.</em></h2></div><p>Une sélection de douceurs et de boissons.<br />Écrivez-nous pour la carte du jour.</p></div>
        <div class="menu-bar reveal-on-scroll"><span>VU SUR INSTAGRAM <b>0{{ visibleMenu.length }}</b></span><div class="filters" aria-label="Filtrer la carte"><button v-for="filter in filters" :key="filter" type="button" :class="{ selected: activeFilter === filter }" @click="activeFilter = filter">{{ filter }}</button></div><a :href="`https://wa.me/${phone.replace('+', '')}`" target="_blank" rel="noreferrer">UNE QUESTION ? ↗</a></div>
        <TransitionGroup name="menu" tag="div" class="menu-grid"><article v-for="(item, index) in visibleMenu" :key="item.id" class="menu-card reveal-on-scroll" :style="{ '--reveal-delay': `${index * 65}ms` }"><a class="menu-image" :href="whatsappLink(item.name)" target="_blank" rel="noreferrer" :aria-label="`Demander la carte ${item.name} sur WhatsApp`"><img :src="item.image" :alt="item.name" loading="lazy" /><span class="menu-tag">{{ item.tag }}</span><span class="menu-arrow">↗</span></a><div class="menu-title"><div><span class="menu-category">ARKO TWINS · ZARZIS</span><h3>{{ item.name }}</h3></div></div><p>{{ item.detail }}</p><a class="menu-order-link" :href="whatsappLink(item.name)" target="_blank" rel="noreferrer">Demander la disponibilité <span>↗</span></a></article></TransitionGroup>
        <div class="menu-note"><span>UNE PETITE FAIM ?</span><span>Les saveurs et disponibilités varient selon le jour.</span><a :href="whatsappLink('la carte du jour')" target="_blank" rel="noreferrer">DEMANDER LA CARTE <b>↗</b></a></div>
      </section>

      <section class="favorites reveal-on-scroll" aria-label="Les favoris Instagram"><div class="favorites-title"><span class="eyebrow">LES STORIES À LA UNE</span><a href="https://www.instagram.com/arko_twins/" target="_blank" rel="noreferrer">SUIVRE @ARKO_TWINS ↗</a></div><div class="favorite-list"><span v-for="favorite in ['Matcha','Iced coffee','Bubble cup','Brownies Dubai','Yaourt glacé','Fraisier','Kunafa cheese','San Sebastián','Jwajem','Big Boule']" :key="favorite">{{ favorite }}</span></div></section>

      <section class="treat-section"><div class="treat-image reveal-on-scroll"><img src="/images/arko/canette-cakes.jpg" alt="Les canette cakes Arko twins aux différentes saveurs" loading="lazy" /><span>LES PETITS PLAISIRS FONT LES GRANDS JOURS.</span></div><div class="treat-copy reveal-on-scroll"><span class="eyebrow">UNE PAUSE QUI VOUS RESSEMBLE</span><h2>Le goût du<br />petit plaisir <em>en plus.</em></h2><p>Brownies Dubai, fraisier, kunafa cheese, San Sebastián, jwajem… Les envies changent, les nouveautés aussi. Retrouvez les dernières créations sur Instagram.</p><a href="https://www.instagram.com/arko_twins/" target="_blank" rel="noreferrer" class="text-link">Découvrir sur Instagram <span>↗</span></a><span class="treat-slogan">Promis, il n’en restera pas une miette.</span></div></section>

        <section id="visite" class="visit"><div class="visit-copy reveal-on-scroll"><span class="eyebrow">ON VOUS GARDE UNE PLACE ?</span><h2>À tout de<br /><em>suite.</em></h2><p>Retrouvez-nous au centre-ville de Zarzis, à côté de l’Attijari Banque. Pour une commande à emporter ou une question sur la carte, appelez-nous.</p><a class="button button-light" :href="`tel:${phone}`">Appeler le {{ phone }} <span>↗</span></a><a class="visit-map" :href="mapUrl" target="_blank" rel="noreferrer">Ouvrir l’itinéraire sur Google Maps <span>↗</span></a><span class="visit-slogan">Une bonne adresse. Une très bonne excuse.</span></div><div class="visit-details reveal-on-scroll"><div class="detail-block"><span class="detail-index">01 / ADRESSE</span><p>Centre-ville, à côté de l’Attijari Banque<br />Avenue Mohamed V · Zarzis</p><a :href="mapUrl" target="_blank" rel="noreferrer">G445+4P Zarzis ↗</a></div><div class="detail-block hours-block"><span class="detail-index">02 / HORAIRES</span><div class="hours-content"><div><p><b>14:00 — 02:00</b></p><small>Ouvert tous les jours, selon les horaires indiqués sur Instagram.</small></div><img src="/images/arko/coffee-matcha.jpg" alt="Visuel Arko twins des boissons du jour : iced coffee à 14 h et matcha mangue latte à 18 h 30" loading="lazy" /></div></div><div class="detail-block"><span class="detail-index">03 / SUR PLACE & À EMPORTER</span><p>Sur place, à emporter<br />et livraison indiquée sur la fiche.</p></div><div class="map-card"><iframe title="Emplacement d’Arko twins à Zarzis sur Google Maps" src="https://maps.google.com/maps?q=33.5052651%2C11.1093094&z=17&output=embed" loading="lazy" referrerpolicy="no-referrer-when-downgrade" allowfullscreen></iframe><a class="map-open-link" :href="mapUrl" target="_blank" rel="noreferrer">Ouvrir dans Google Maps <span>↗</span></a></div></div></section>
    </main>

    <footer class="footer"><a href="#accueil" class="brand footer-brand"><span class="brand-mark brand-logo"><img src="/images/arko/logo.jpg" alt="" /></span><span class="brand-name">arko <i>twins</i><small>PÂTISSERIE · CAFÉ</small></span></a><p>Un peu de douceur, au cœur de Zarzis.</p><div class="footer-links"><a href="#carte">La carte</a><a href="#visite">Nous trouver</a><a :href="`tel:${phone}`">+216 27 888 597</a></div><a class="instagram-link" href="https://www.instagram.com/arko_twins/" target="_blank" rel="noreferrer">INSTAGRAM ↗</a><span class="footer-copy">© ARKO TWINS · 2026</span></footer>
    <a class="whatsapp-float" :href="whatsappLink('la carte du jour et les disponibilités')" target="_blank" rel="noreferrer" aria-label="Demander la carte du jour sur WhatsApp"><svg class="whatsapp-icon" viewBox="0 0 32 32" aria-hidden="true"><path d="M16 3.2A12.5 12.5 0 0 0 5.3 22.2L3.7 28l6-1.6A12.5 12.5 0 1 0 16 3.2Zm0 22.7a10.1 10.1 0 0 1-5.2-1.4l-.4-.2-3.5.9.9-3.4-.2-.4A10.1 10.1 0 1 1 16 25.9Zm5.5-7.6c-.3-.2-1.7-.9-1.9-1s-.5-.2-.7.2-.8 1-1 1.2-.4.2-.7.1a8.3 8.3 0 0 1-2.4-1.5 9 9 0 0 1-1.7-2.1c-.2-.3 0-.5.1-.7l.5-.6c.2-.2.2-.4.3-.6s0-.4 0-.6-.7-1.6-.9-2.2c-.3-.6-.5-.5-.7-.5h-.6a1.2 1.2 0 0 0-.9.4 3.7 3.7 0 0 0-1.1 2.7 6.4 6.4 0 0 0 1.3 3.4 14.6 14.6 0 0 0 5.6 4.9c.7.3 1.2.5 1.7.6.7.2 1.4.2 1.9.1.6-.1 1.7-.7 2-1.4.2-.7.2-1.3.2-1.4s-.3-.2-.6-.4Z"/></svg><span>Un petit creux ?<small>Écrivez-nous</small></span></a>
    <div class="footer-line"><span>FAIT AVEC UN PEU DE DOUCEUR</span><a href="#accueil">RETOUR EN HAUT ↑</a><span>À bientôt pour la prochaine bouchée</span></div>
  </div>
</template>
