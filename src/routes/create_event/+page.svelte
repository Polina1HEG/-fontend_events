<script lang="ts">
	import { goto } from "$app/navigation";
	import { apiUrl } from "$lib/index";
	let name: string = "";
	let content: string = "";
	let start_year: number;
	let start_month: number;
	let start_day: number;
	let start_hour: number;
	let start_minute: number;
	let end_year: number;
	let end_month: number;
	let end_day: number;
	let end_hour: number;
	let end_minute: number;
	let place: string = "";
	let category: string = "";
	let tags: string = "";
	$: newObject = {
		name,
		start_year,
		start_month,
		start_day,
		start_hour,
		start_minute,
		end_year,
		end_month,
		end_day,
		end_hour,
		end_minute,
		place,
		content,
		category,
		tags
	};

	async function saveObject(newObject:any) {
		try {
			const response = await fetch(`${apiUrl}`, {
				method: "POST",
				headers: { "Content-Type": "application/json;charset=utf-8" },
				body: JSON.stringify(newObject)
			});
			if (response.ok) {
				goto("/");
			} else {
				console.error("Ошибка сохранения объекта");
			}
		} catch (error) {
			console.error("Ошибка подключения");
		}
	}

	function cancel() {
		goto("/");
	}

	function handleDatetimeChange(event: Event) {
    const input = event.target as HTMLInputElement;
    const datetimeValue = input.value; 

    if (datetimeValue) {
      const [datePart, timePart] = datetimeValue.split("T");
      const [year, month, day] = datePart.split("-").map(Number);
      const [hour, minute] = timePart.split(":").map(Number);
      start_year = year;
      start_month = month;
      start_day = day;
      start_hour = hour;
      start_minute = minute;
    }
  }
  function handleDatetimeChangeforEND(event: Event) {
    const input = event.target as HTMLInputElement;
    const datetimeValue = input.value; 
    if (datetimeValue) {
      const [datePart, timePart] = datetimeValue.split("T");
      const [year, month, day] = datePart.split("-").map(Number);
      const [hour, minute] = timePart.split(":").map(Number);
      end_year = year;
      end_month = month;
      end_day = day;
      end_hour = hour;
      end_minute = minute;
    }
  }
</script>
<header>
	<div class="header">
		<div class="logo"><img src="logo.svg" alt="ITAM" /></div>
		<nav>
			<button class="nav-btn">Мероприятия</button>
			<button class="nav-btn">Админ-панель</button>
			<div class="avatar"><img src="avatar.svg" alt="avatar"/></div>
		</nav>
	</div>
</header>
<main>
	<h1>Админ-панель</h1>
	<button on:click={cancel} class="back">
	<img src="Стрелка.svg"  alt="back"/></button>
	<form on:submit|preventDefault={()=>saveObject(newObject)}>
		<label for="title">Название</label>
		<input id="title" type="text" bind:value={name} required />
		<input type="datetime-local" id="datetimeInput" on:input={handleDatetimeChange} />
		<label for="day_end">День</label>
		<input type="datetime-local" id="datetimeInput" on:input={handleDatetimeChangeforEND} />
		<div class="split_new">
			<div class="element">
				<label for="tag">Тег</label>
				<input id="tag" type="text" bind:value={tags} required />
			</div>
			<div class="element">
				<label for="format">Формат проведение</label>
				<input id="format" type="text" bind:value={category} required />
			</div>
			<div class="element">
				<label for="place">Место проведения</label>
				<input id="place" type="text" bind:value={place} required />
			</div>
		</div>
		<label for="content">Описание</label>
		<textarea bind:value={content} id="content"></textarea>
		<button type="submit" class="but">Сохранить</button>
	</form>
</main>

<style>
	.split {
		display: grid;
		grid-template-columns: 2fr 3fr 3fr 3fr 3fr;
		gap: 15px;
	}
	.split_new {
		display: grid;
		grid-template-columns: 5fr 5fr 5fr;
		gap: 15px;
	}
	main {
		margin-left: 60px;
		margin-right: 60px;
		font-family: "ttnormspro-regular", sans-serif;
		font-size: 27px;
	}
	label {
		margin-bottom: 3px;
	}
	input {
		border-radius: 20px;
		height: 52px;
		padding: 10px 18px;
		font-size: 27px;
		color: #787878;
		border: 2px solid #808080;
	}
	textarea {
		border-radius: 20px;
		height: 104px;
		padding: 10px 18px;
		font-size: 27px;
		color: #787878;
		border: 2px solid #808080;
	}
	form {
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}
	.element{
		display: flex;
		flex-direction: column;
	}
	header {
		margin-top: 25px;
		margin-left: 23px;
		margin-right: 23px;
		padding: 18px 37px;
		background: linear-gradient(to right, rgba(252, 205, 205, 1), rgba(200, 170, 231, 1));
		border-radius: 60px;
	}
	.header {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
	.logo {
		margin-top: 7.5px;
		margin-bottom: 7.5px;
		margin-left: 19px;
	}
	.nav-btn {
		background: white;
		border: none;
		border-radius: 40px;
		padding: 10px 20px;
		cursor: pointer;
		font-weight: 500px;
        font-family: "ttnormspro-regular", sans-serif;
        font-size: 18px; 
	}
	nav {
		display: flex;
	}
	.avatar {
		margin-left: 16px;
	}
	h1 {
		color: rgba(60, 51, 64, 1);
		font-family: "cygre-medium", sans-serif;
		margin-bottom: 16px;
	}
	.back {
		margin-bottom: 21px;
	}
	button {
		background: none;
		border: none;}
	.but {
		background: linear-gradient(135deg, rgba(250, 202, 206, 0.5), rgba(200, 170, 231, 0.5));
		border: none;
		padding: 10.5px;
		color: rgba(60, 51, 64, 1);
		border-radius: 10px;
		font-size: 18px;
		cursor: pointer;
		height: 70px;
		transition:
			transform 0.3s,
			box-shadow 0.3s;
	}

	.but:hover {
		transform: translateY(-3px);
		box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
	}
</style>
