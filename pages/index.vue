<template>
	<div>
		<Navigation />
		<header class="hero">
			<h1>Welcome to Our Product</h1>
			<p>Your solution for [problem your product solves]</p>
			<NuxtLink to="/about" class="cta-button">Learn More</NuxtLink>
		</header>
		<section class="features">
			<h2>Features</h2>
			<div class="feature">
				<h3>Feature One</h3>
				<p>Explanation of feature one.</p>
			</div>
			<div class="feature">
				<h3>Feature Two</h3>
				<p>Explanation of feature two.</p>
			</div>
			<div class="feature">
				<h3>Feature Three</h3>
				<p>Explanation of feature three.</p>
			</div>
		</section>
		<footer class="footer">
			<p>&copy; 2024 Your Company. All rights reserved.</p>
		</footer>
	</div>
</template>

<script setup>
import Navigation from '~/components/Navigation.vue'
</script>

<style scoped>
.hero {
	background: url('/path-to-your-hero-image.jpg') no-repeat center center;
	background-size: cover;
	text-align: center;
	padding: 4rem 1rem;
}

.cta-button {
	background-color: #ff6600;
	color: white;
	padding: 1rem 2rem;
	text-decoration: none;
	border-radius: 5px;
	display: inline-block;
	margin-top: 1rem;
}

.features {
	padding: 2rem 1rem;
	text-align: center;
}

.feature {
	margin-bottom: 2rem;
}

.footer {
	background-color: #333;
	color: white;
	text-align: center;
	padding: 1rem;
}
</style>
