<script lang="ts">
	let active = $state(false);
	let offset = $state({ x: 0, y: 0 });
	let focus = $state(false);

	let move = (ev: any) => (offset = { x: ev.clientX, y: ev.clientY });
</script>

<div
	class="panel"
	role="main"
	onmouseenter={(ev) => {
		active = true;
		ev.target?.addEventListener('mousemove', move);
	}}
	onmouseleave={(ev) => {
		active = false;
		ev.target?.removeEventListener('mousemove', move);
	}}
>
	<h1 onmouseenter={() => (focus = true)} onmouseleave={() => (focus = false)}>Hello World!</h1>
	<h1 onmouseenter={() => (focus = true)} onmouseleave={() => (focus = false)}>你好世界！</h1>

	<div class="cursor" class:active class:focus style="left: {offset.x}px; top: {offset.y}px">
		<div class="spot"></div>
	</div>
</div>

<style lang="scss">
	.panel {
		height: 100%;
		cursor: none;
		background-color: beige;

		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;

		> h1 {
			font-size: 128px;
			font-weight: 800;
		}
	}

	.cursor {
		&.active {
			transform: scale(0.25);
			opacity: 1;
		}

		&.focus {
			transform: scale(1);
			opacity: 1;

			> .spot {
				opacity: 1;
			}
		}

		pointer-events: none; // 防止响应事件导致光标位置跳变

		display: block;

		width: 6rem;
		height: 6rem;
		margin-left: -3rem;
		margin-top: -3rem;

		border-radius: 50%;

		left: 0;
		top: 0;
		position: fixed;
		z-index: 100;

		transition:
			opacity 0.25s cubic-bezier(0.35, 0.35, 0, 1),
			transform 0.25s cubic-bezier(0.35, 0.35, 0, 1),
			backdrop-filter 0.85s cubic-bezier(0.35, 0.35, 0, 1);

		opacity: 0;
		transform: scale(0);
		backdrop-filter: contrast(1.5) brightness(1.15) saturate(1.35);

		&::before {
			content: '';

			position: absolute;

			top: 0;
			right: 0;
			bottom: 0;
			left: 0;

			border-radius: 50%;
			border: 2px solid #fff;
			box-shadow: inset 0 0 0 2px #000;
			transition: border-width 0.25s cubic-bezier(0.35, 0.35, 0, 1);
		}

		> .spot {
			position: absolute;

			width: 0.3rem;
			height: 0.3rem;
			margin-left: -0.15rem;
			margin-top: -0.15rem;

			left: 50%;
			top: 50%;

			border-radius: 100%;
			background-color: #000;
			transition: opacity 0.5s cubic-bezier(0.35, 0.35, 0, 1);

			opacity: 0;
		}
	}
</style>
