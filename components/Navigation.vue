<template>
	<nav>
		<ul>
			<li>
				<NuxtLink to="/">Home</NuxtLink>
			</li>
			<li>
				<NuxtLink to="/about">About</NuxtLink>
			</li>
			<li>
				<NuxtLink to="/contact">Contact</NuxtLink>
			</li>
			<li>
				<NuxtLink to="/news">News</NuxtLink>
			</li>
		</ul>
	</nav>
</template>


<style scoped>
nav {
	background-color: #333;
	padding: 1rem;
}

ul {
	list-style: none;
	display: flex;
	gap: 1rem;
}

li {
	margin: 0;
}

a {
	color: white;
	text-decoration: none;
}

a:hover {
	text-decoration: underline;
}
</style>