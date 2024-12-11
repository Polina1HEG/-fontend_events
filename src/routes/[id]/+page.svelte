<script lang="ts">
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
    import { page } from '$app/stores';
	import {apiUrl} from '$lib/index';
	let object: any;
    object = null;
    let id = $page.params.id;
    const apiUrl1 = `${apiUrl}/${id}`;
    async function fetchObject() {
    try{
      const response = await fetch(apiUrl1, {method: 'GET', headers: { 'Content-Type': 'application/json;charset=utf-8' }});
      if (response.ok) {
        object = await response.json();
        object["start_year"] = Number(object.start_datetime.substr(0,4));
        object["start_month"] = Number(object.start_datetime.substr(5,2));
        object["start_day"] = Number(object.start_datetime.substr(8,2));
        object["start_hour"] = Number(object.start_datetime.substr(11,2));
        object["start_minute"] = Number(object.start_datetime.substr(14,2));
        object["end_year"] = Number(object.end_datetime.substr(0,4));
        object["end_month"] = Number(object.end_datetime.substr(5,2));
        object["end_day"] = Number(object.end_datetime.substr(8,2));
        object["end_hour"] = Number(object.end_datetime.substr(11,2));
        object["end_minute"] = Number(object.end_datetime.substr(14,2));
        delete object["start_datetime"];
        delete object["end_datetime"];
      } else {
        console.error('Ошибка загрузки объекта');
      }
    }
    catch (error){
        console.error('Ошибка подключения по id');
    }
    } 
    async function saveChanges() {
      const response = await fetch(apiUrl1, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(object),
      });
      if (response.ok) {
        goto('/test');
      } else {
        console.error('Ошибка сохранения изменений');
      }
    } 
    onMount(fetchObject);
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
   
	<button class="back-btn">
		<img src="Back.svg" alt="Назад" />
	</button>
    {#if object}
      <form on:submit|preventDefault={saveChanges}>
        <div class="content">
          <div class="left-panel">
            <img src="pic.png" alt="Картинка">
          </div>
          <div class="right-panel">
            <h2>Основы эффективной разработки</h2>
            <p>27 ноября | 19:00–20:30 | Б-3</p>
          </div>
        </div>

        <label>
          Название:
          <input type="text" bind:value={object.name} required />
        </label>
        <label>
          Контент:
          <textarea bind:value={object.content}></textarea>
        </label>
        <label>
        День:
        <input type="number" bind:value={object.start_day} min="1" max="31" required />
      </label>
      <label>
        Месяц:
        <input type="number" bind:value={object.start_month} min="1" max="12" required />
      </label>
      <label>
        Год:
        <input type="number" bind:value={object.start_year} min="2024" required />
      </label>
      <label>
        Время:
        <input type="number" bind:value={object.start_hour} min="0" max="23" required />
      </label>
      <label>
        <input type="number" bind:value={object.start_minute} min="0" max="59" required />
      </label>
      <label>
        День:
        <input type="number" bind:value={object.end_day} min="1" max="31" required />
      </label>
      <label>
        Месяц:
        <input type="number" bind:value={object.end_month} min="1" max="12" required />
      </label>
      <label>
        Год:
        <input type="number" bind:value={object.end_year} min="2024" required />
      </label>
      <label>
        Время:
        <input type="number" bind:value={object.end_hour} min="0" max="23" required />
      </label>
      <label>
        <input type="number" bind:value={object.end_minute} min="0" max="59" required />
      </label>
        <label>
          Место:
          <input type="text" bind:value={object.place} required />
        </label>
        <label>
          тег:
          <input type="text" bind:value={object.tags}  required />
        </label>
        <label>
          категория:
          <input type="text" bind:value={object.category} required />
        </label>

        <button type="submit">Сохранить изменения</button>
      </form>
    {/if}
  </main>
  
  <style>

	header{
		margin-top: 25px;
		margin-left: 23px;
		margin-right: 23px;
		padding: 18px 37px;
		background: linear-gradient(to right, rgba(252, 205, 205, 1), rgba(200, 170, 231, 1));
		border-radius: 60px;
    font-weight: 500px;
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
	.avatar {
		margin-left: 16px;
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
    main {
		margin-left: 60px;
		margin-right: 60px;
    font-family: "ttnormspro-regular", sans-serif;
    }
	button {
		background: none;
		border: none;}
  
    form {
      display: flex;
      flex-direction: column;
      gap: 1rem;
    }
 
	h1 {
		color: rgba(60, 51, 64, 1);
		font-family: "cygre-medium", sans-serif;
		margin-bottom: 28px;
	}
	.back-btn{
		margin-top: 29px;
	}
  </style>
  