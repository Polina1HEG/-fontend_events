<script lang="ts">
	import { onMount } from "svelte";
	import { goto } from "$app/navigation";
	import { apiUrl } from "$lib/index";
	let objects: any[];
	async function fetchObjects() {
		try {
			const response = await fetch(`${apiUrl}`, {
				credentials: "same-origin",
				method: "GET",
				headers: { "Content-Type": "application/json" }
			});
			// if (!response.ok) {
			// 	throw new Error(`Ошибка ${response.status}  ${response.statusText}`);
			// }
			// const text = await response.text();
			// if (!text) {
			// 	throw new Error("Ответ от сервера пустой");
			// }
			objects = await response.json();

			// const data = JSON.parse(text);
			// console.log(`Полученные данные: ${data}`);
			// return data;
		} catch (error) {
			console.error("Ошибка загрузки", error);
			throw error;
		}
	}

	let selecte_tag = "";
	onMount(async () => {
		await fetchObjects();
	});

	function goToAddPage() {
		goto("/create_event");
	}

	function goToObjectPage(id) {
		goto(`/${id}`);
	}

	async function deleteEvent(id) {
		try {
			const response = await fetch(`${apiUrl}${id}`, {
				method: "DELETE",
				headers: { "Content-Type": "application/json" }
			});
			if (response.ok) {
				objects = objects.filter(obj => obj.id !== id);
			} else {
				const error = await response.json();
				alert("Error: " + error.message);
			}
		} catch (error) {
			console.error("Error: ", error);
		}
	}
</script>

<header>
	<div class="header">
		<div class="logo"><img src="logo.svg" /></div>
		<nav>
			<button class="nav-btn">Мероприятия</button>
			<button class="nav-btn">Календарь</button>
			<div class="avatar"><img src="avatar.svg" /></div>
		</nav>
	</div>
</header>
<main>
	<h1>Мероприятия</h1>
	<div class="filter-bar">
		<select id="tag_selected" bind:value={selecte_tag}>
			<option value="">Все категории</option>
		</select>
		<select>
			<option selected>Формат проведения</option>
			<option>Онлайн</option>
			<option>Офлайн</option>
		</select>
		<input type="text" placeholder="Поиск..." />
	</div>
	<button on:click={goToAddPage}>Добавить событие</button>
	<ul>
		{#each objects as obj (obj.id)}
			<div class="card">
				<li on:click={() => goToObjectPage(obj.id)}>
					<span class="event-tag">{obj.tags}</span> <br />
					<h2>{obj.name}</h2>
					<p>{obj.content}</p>
					<small>{obj.start_datetime}</small>
				</li>
				<button on:click={() => deleteEvent(obj.id)}><img src="trash.svg" /> </button>
			</div>
		{/each}
	</ul>
</main>

<style>
	header {
		margin-top: 25px;
		margin-left: 23px;
		margin-right: 23px;
		padding: 18px 37px;
		background: linear-gradient(to right, #fccdcd, #c8aae7);
		border-radius: 60px;
		height: 91;
	}
	.header {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
	.logo {
		margin-top: 7.5;
		margin-bottom: 7.5;
		margin-left: 19;
	}
	.nav-btn {
		background: white;
		border: none;
		border-radius: 40px;
		padding: 10px 20px;
		cursor: pointer;
	}
	nav {
		display: flex;
	}
	.avatar {
		margin-left: 16px;
	}
	.event-tag {
		position: absolute;
		top: 23;
		left: 23;
		background: #d9d9d9;
		color: #666666;
		padding: 7px 23px;
		font-size: 12px;
		border-radius: 20;
	}
</style>
