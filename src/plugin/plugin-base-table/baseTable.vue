<template>
	<div class="base_table">
		<div class="base_table_header">
			<div class="base_table_header_title">
				<slot name="slot-title">默认标题</slot>
			</div>
			<div class="base_table_header_btn">
				<slot name="slot-title-btn"></slot>
			</div>
		</div>

		<div class="base_table_body">
			<div class="base_table_top_btn">
				<div class="base_table_top_btn_box" isexpand="false">
					<div class="base_table_top_btn_form">
						<slot name="slot-form"></slot>
					</div>
					<div class="base_table_top_btn_btn">
						<slot name="slot-form-btn"></slot>
						<el-button v-if="formDomFlag" type="text" icon="el-icon-caret-bottom" @click="formOpen">展开</el-button>
					</div>
				</div>
			</div>

			<div class="base_table_mid_btn">
				<slot name="slot-mid-btn"></slot>
			</div>

			<div class="base_table_content">
				<slot name="slot-table">
					<el-table
						:data="[]"
						height="100%"
						style="width: 100%"
						border
					>
					</el-table>
				</slot>
			</div>

			<div class="base_table_footer" :style="`--pageBcColor:${ pageBcColor }; --pageTxtColor:${ pageTxtColor }`">
				<el-pagination
					background
					layout="total, prev, pager, next, jumper"
					:total="pageTotal"
					:page-size="pageSize"
					:current-page="currentPage"
					@current-change="currentChange"
				>
				</el-pagination>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'phoenixtree-base-table',
	props: {
		'pageTotal': {
			type: Number,
			default: 100
		},
		'pageSize': {
			type: Number,
			default: 30
		},
		'currentPage': {
			type: Number,
			default: 1
		},
		'pageBcColor': {
			type: String,
			default: '#409eff'
		},
		'pageTxtColor': {
			type: String,
			default: '#ffffff'
		}
	},
	data() {
		return {
			formDomFlag: false, // 表单区域，显示下拉标识
		}
	},
	mounted() {
		this.computedHeight();
		window.addEventListener('resize', () => {
			this.computedHeight();
		});
	},
	methods: {
		// 动态计算高度
		computedHeight() {
			// 动态计算高度
			let formDom = document.getElementsByClassName('base_table_top_btn')[0];
			let formDomChildren = document.getElementsByClassName('base_table_top_btn_box')[0].children;
			let midDom = document.getElementsByClassName('base_table_mid_btn')[0];
			let tableDom = document.getElementsByClassName('base_table_content')[0];
			let subNumber = 0;
			// 表单区域计算
			if (
				formDomChildren[0].children.length === 0
				&&
				formDomChildren[1].children.length === 0
			) {
				formDom.setAttribute('style', `display: none; height: 0px;`);
				this.formDomFlag = false;
			} else if (
				(formDomChildren[0].scrollHeight > 0 || formDomChildren[1].scrollHeight > 0)
				&&
				formDomChildren[0].scrollHeight <= (50+16)
			) {
				formDom.setAttribute('style', `display: flex;`);
				subNumber += 50;
				this.formDomFlag = false;
			} else if (formDomChildren[0].scrollHeight > (50+16)) {
				formDom.setAttribute('style', `display: flex;`);
				subNumber += 50;
				this.formDomFlag = true;
			}
			// 按钮区域计算
			if (midDom.children.length === 0) {
				midDom.setAttribute('style', `display: none; height: 0px;`);
			} else {
				midDom.setAttribute('style', `display: inline-flex; height: 50px;`);
				subNumber += 50;
			}
			tableDom.setAttribute('style', `height: calc(100% - ${50 + subNumber}px )`)
		},
		// 表单区域展开函数
		formOpen() {
			let formDom = document.getElementsByClassName('base_table_top_btn_box')[0];
			let iconDom = document.getElementsByClassName('el-icon-caret-bottom')[0];
			let isexpand = formDom.getAttribute('isexpand');
			if(isexpand === 'false') {
				formDom.parentElement.setAttribute('style', 'overflow: inherit;')
				formDom.setAttribute('style', 'border: 1px solid #eaeaea; box-shadow: 5px 5px 5px rgb(223, 223, 223);')
				formDom.setAttribute('isexpand', 'true');
				iconDom.setAttribute('style', 'transform: rotate(180deg);')
			} else {
				formDom.parentElement.setAttribute('style', 'overflow: hidden;')
				formDom.setAttribute('style', 'border: 1px solid #fff; box-shadow: 0px 0px 0px rgb(223, 223, 223);')
				formDom.setAttribute('isexpand', 'false');
				iconDom.setAttribute('style', 'transform: rotate(0deg);')
			}
		},
		// 页码改变
		currentChange(num){
			this.$emit('currentChange', num)
		}
	},
}
</script>

<style lang="less" scoped>
.base_table {
	width: 100%;
	height: 100%;
	.base_table_header {
		width: 100%;
		height: 50px;
		display: flex;
		box-sizing: border-box;
		border-bottom: 1px solid #eaeaea;
		box-sizing: border-box;
		padding: 0 8px;
		.base_table_header_title {
			width: 50%;
			height: 100%;
			display: flex;
			align-items: center;
		}
		.base_table_header_btn {
			width: 50%;
			height: 100%;
			display: flex;
			align-items: center;
			justify-content: flex-end;
		}
	}

	.base_table_body {
		width: 100%;
		height: calc( 100% - 50px);
		box-sizing: border-box;
		padding-top: 8px;
		.base_table_top_btn {
			width: 100%;
			height: 50px;
			position: relative;
			z-index: 2;
			overflow: hidden;
			.base_table_top_btn_box {
				width: 100%;
				height: auto;
				position: absolute;
				left: 0;
				top: 0;
				background: #fff;
				box-sizing: border-box;
				border: 1px solid #fff;
				box-shadow: 0px 0px 0px rgb(223, 223, 223);
				display: flex;
				.base_table_top_btn_form {
					width: calc(100% - 200px);
					line-height: 50px;
					box-sizing: border-box;
					padding: 8px;
					display: inline-flex;
					justify-content: flex-start;
					.el-form {
						text-align: left;
						/deep/.el-form-item {
							height: 32px;
							display: inline-flex;
							align-items: center;
							.el-form-item__label,.el-form-item__content {
								height: 100%;
								display: inline-flex;
								align-items: center;
								line-height: 1;
								.el-input,.el-input__inner {
									height: 100%;
									line-height: 1;
									display: inline-flex;
									align-items: center;
									input {
										height: 100%;
									}
									span {
										height: 100%;
										line-height: 1;
										display: inline-flex;
										align-items: center;
										.el-input__icon {
											line-height: 1;
										}
									}
									i {
										display: inline-flex;
										align-items: center;
									}
								}
								.el-select {
									height: 100%;
								}
							}
						}
					}
				}
				.base_table_top_btn_btn {
						width: 200px;
						height: 50px;
						display: inline-block;
						line-height: 50px;
						display: inline-flex;
						align-items: center;
						justify-content: flex-end;
						box-sizing: border-box;
						padding-right: 8px;
						.el-button {
							padding: 7px 15px;
							font-size: 12px;
							border-radius: 3px;
						}
				}
			}
		}

		.base_table_mid_btn {
			width: 100%;
			height: 50px;
			box-sizing: border-box;
			padding: 0 8px;
			display: inline-flex;
			align-items: center;
			.el-button {
				padding: 7px 15px;
				font-size: 12px;
				border-radius: 3px;
			}
		}

		.base_table_content {
			width: 100%;
			height: calc(100% - 200px);
			box-sizing: border-box;
			padding: 0 8px;
		}

		.base_table_footer {
			width: 100%;
			height: 50px;
			display: flex;
			align-items: center;
			justify-content: center;
		}
	}
}
/deep/.el-form-item {
	margin-bottom: 0 !important;
}
/deep/.active {
	background: var(--pageBcColor) !important;
	color: var(--pageTxtColor) !important;
}
</style>
