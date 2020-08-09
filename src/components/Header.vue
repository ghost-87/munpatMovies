<script>
	export default{
		data() {
			return {
				isActive : false,
			}
		},
		methods : {
			backToTop () {
	        	window.scrollTo(0,0);
	      	},
		}
	}
</script>

<template>
	<header>		
		<div class="container headerContent">
			<div class="headLeft">
				<router-link to="/" @click.native="backToTop" title="Database Search Movie">
					<span>
						<span>Fireworks</span>
						<span>E N T E R T A I N M E N T</span>
					</span>
				</router-link>
			</div>
			<div class="headRight">
				<a href="javascript:;" class="mobileMenuBtn" @click="isActive = !isActive">
					<i class="ion-md-menu"></i>
				</a>
				<div class="mobileMenu" :class="{ showMobileMenu: isActive }">
					<div class="mobileMenuContent">										
						<a href="javascript:;" class="mobileMenuCloseBtn" @click="isActive = !isActive"><i class="ion-md-close"></i></a>
						<ul class="mobileMenuLinks">		
							<li>
								<router-link @click.native="isActive = !isActive, backToTop()" to="/popular">Popular Movies</router-link>	
							</li>
						</ul>	
					</div>
					<div class="mobileMenuOverlay" @click="isActive = !isActive"></div>
				</div>
				<ul class="headerLinks">		
					<li>
						<router-link to="/popular" title="Popular Movies">Popular Movies</router-link>	
					</li>
				</ul>			
			</div>
		</div>
	</header>
</template>