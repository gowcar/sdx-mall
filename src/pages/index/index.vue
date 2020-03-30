<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<view>
			<text class="title">{{title}}</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.content {
		text-align: center;
		height: 200px;
	}

	.logo {
		height: 100px;
		width: 100px;
		margin-top: 100px;
	}

	.title {
		font-size: 18px;
		color: #8f8f94;
	}
</style>
