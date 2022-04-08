<template>
	<view
		v-if="showPopup"
		class="uni-popup"
		:class="[popupstyle, isDesktop ? 'fixforpc-z-index' : '']"
		@touchmove.stop.prevent="clear"
	>
		<uni-transition
			v-if="maskShow"
			class="uni-mask--hook"
			:mode-class="['fade']"
			:styles="maskClass"
			:duration="duration"
			:show="showTrans"
			@click="onTap"
		/>
		<uni-transition :mode-class="ani" :styles="transClass" :duration="duration" :show="showTrans">
			<view class="uni-popup__wrapper-box" @click.stop="clear">
				<slot />
			</view>
		</uni-transition>
	</view>
</template>

<script>
import popup from './popup.js'
import UniTransition from '@dcloudio/uni-ui/lib/uni-transition/uni-transition.vue'
/**
 * PopUp 弹出层
 * @description 弹出层组件，为了解决遮罩弹层的问题
 * @tutorial https://ext.dcloud.net.cn/plugin?id=329
 * @property {String} type = [top|center|bottom] 弹出方式
 * 	@value top 顶部弹出
 * 	@value center 中间弹出
 * 	@value bottom 底部弹出
 * 	@value message 消息提示
 * 	@value dialog 对话框
 * 	@value share 底部分享示例
 * @property {Boolean} animation = [ture|false] 是否开启动画
 * @property {Boolean} maskClick = [ture|false] 蒙版点击是否关闭弹窗
 * @event {Function} change 打开关闭弹窗触发，e={show: false}
 */

export default {
	name: 'popup-local',
	components: {
		UniTransition,
	},
	emits: ['change'],
	props: {
		// 开启动画
		animation: {
			type: Boolean,
			default: true
		},
		// 弹出层类型，可选值，top: 顶部弹出层；bottom：底部弹出层；center：全屏弹出层
		// message: 消息提示 ; dialog : 对话框
		type: {
			type: String,
			default: 'center'
		},
		// maskClick
		maskClick: {
			type: Boolean,
			default: false
		}
	},
	provide() {
		return {
			popup: this
		}
	},
	mixins: [popup],
	watch: {
		/**
		 * 监听type类型
		 */
		type: {
			handler: function (newVal) {
				this[this.config[newVal]]()
			},
			immediate: true
		},
		isDesktop: {
			handler: function (newVal) {
				this[this.config[this.type]]()
			},
			immediate: true
		}
	},
	data() {
		return {
			duration: 300,
			ani: [],
			showPopup: false,
			showTrans: false,
			maskClass: {
				'position': 'fixed',
				'bottom': 0,
				'top': 0,
				'left': 0,
				'right': 0,
				'backgroundColor': 'rgba(0, 0, 0, 0.4)'
			},
			transClass: {
				'position': 'fixed',
				'left': 0,
				'right': 0,
			},
			maskShow: true,
			popupstyle: this.isDesktop ? 'fixforpc-top' : 'top'
		}
	},
	created() {
		if (this.animation) {
			this.duration = 300
		} else {
			this.duration = 0
		}
	},
	methods: {
		onTap() {
			if (!this.maskClick) return
			this.close()
		},
		clear(e) {
			// TODO nvue 取消冒泡
			e.stopPropagation()
		},
		async open() {
			this.showPopup = true
			await this.$nextTick()
			this.showTrans = true
			this.$emit('change', {
				show: true,
				type: this.type
			})
			// 自定义打开事件
			// clearTimeout(this.msgtimer)
			// this.msgtimer = setTimeout(() => {
			// 	this.customOpen && this.customOpen()
			// }, 100)
		},
		async close(type) {
			this.showTrans = false
			clearTimeout(this.timer)
			this.timer = setTimeout(() => {
				this.showPopup = false
				this.$emit('change', {
					show: false,
					type: this.type
				})
				// 自定义关闭事件
				// this.customOpen && this.customClose()
			}, this.duration)
		},
		/**
		 * 顶部弹出样式处理
		 */
		top() {
			this.popupstyle = this.isDesktop ? 'fixforpc-top' : 'top'
			this.ani = ['slide-top']
			this.transClass = {
				'position': 'fixed',
				'left': 0,
				'right': 0,
			}
		},
		/**
		 * 底部弹出样式处理
		 */
		bottom() {
			this.popupstyle = 'bottom'
			this.ani = ['slide-bottom']
			this.transClass = {
				'position': 'fixed',
				'left': 0,
				'right': 0,
				'bottom': 0
			}
		},
		/**
		 * 中间弹出样式处理
		 */
		center() {
			this.popupstyle = 'center'
			this.ani = ['zoom-out', 'fade']
			this.transClass = {
				'position': 'fixed',
				/* #ifndef APP-NVUE */
				'display': 'flex',
				'flexDirection': 'column',
				/* #endif */
				'bottom': 0,
				'left': 0,
				'right': 0,
				'top': 0,
				'justifyContent': 'center',
				'alignItems': 'center'
			}
		}
	}
}
</script>
<style scoped>
.uni-popup {
	position: fixed;
	/* #ifndef APP-NVUE */
	z-index: 99;
	/* #endif */
}

.fixforpc-z-index {
	/* #ifndef APP-NVUE */
	z-index: 999;
	/* #endif */
}

.uni-popup__mask {
	position: absolute;
	top: 0;
	bottom: 0;
	left: 0;
	right: 0;
	background-color: rgba(0, 0, 0, 0.5);
	opacity: 0;
}

.mask-ani {
	transition-property: opacity;
	transition-duration: 0.2s;
}

.uni-top-mask {
	opacity: 1;
}

.uni-bottom-mask {
	opacity: 1;
}

.uni-center-mask {
	opacity: 1;
}

.uni-popup__wrapper {
	/* #ifndef APP-NVUE */
	display: block;
	/* #endif */
	position: absolute;
}

.top {
	/* #ifdef H5 */
	top: var(--window-top);
	/* #endif */
	/* #ifndef H5 */
	top: 0;
	/* #endif */
}

.fixforpc-top {
	top: 0;
}

.bottom {
	bottom: 0;
}

.uni-popup__wrapper-box {
	/* #ifndef APP-NVUE */
	display: block;
	/* #endif */
	position: relative;
	/* iphonex 等安全区设置，底部安全区适配 */
	/* #ifndef APP-NVUE */
	padding-bottom: constant(safe-area-inset-bottom);
	padding-bottom: env(safe-area-inset-bottom);
	/* #endif */
}

.content-ani {
	transition-property: transform, opacity;
	transition-duration: 0.2s;
}

.uni-top-content {
	transform: translateY(0);
}

.uni-bottom-content {
	transform: translateY(0);
}

.uni-center-content {
	transform: scale(1);
	opacity: 1;
}
</style>
