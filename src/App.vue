<script setup>
	import { ref } from "vue";
	let header = ref("רשימת קניות 🛒");
	let items = ref([]);
	let newItem = ref();
	let quantity = ref();
	let newItemPriority = ref("");
	let addItemToArray = () => {
		if (!newItem.value) return alert(`שגיאה ❌: ציין בבקשה מוצר להוספה 🛒`);
		items.value.push({
			id: items.value.length + 1,
			item: newItem.value,
			quantity: quantity.value,
			priority: {
				isPriority: newItemPriority.value === "גבוהה" ? true : false,
				value: newItemPriority.value,
			},
		});
		newItem.value = "";
	};
	let togglePurchase = (item) => {
		item.isPurchase = !item.isPurchase;
	};
	let deleteItem = (itemId) => {
		if (items.length === 0) return;
		items.value = items.value.filter((item) => item.id !== itemId);
	};
</script>

<template>
	<div class="md:flex md:flex-row flex-col gap-8">
		<div class="container max-h-max font-sans w-[21rem] rounded-xl bg-sky-50 flex flex-col justify-start shadow-xl py-10 px-12">
			<h1 class="text-[1.9rem] mb-7 font-bold max-w-max">
				{{ header }}
			</h1>
			<form
				class="flex flex-col gap-4 max-w-max placeholder:italic"
				@submit.prevent="addItemToArray()"
			>
				<label for="item">מה להוסיף?</label>
				<input
					v-model="newItem"
					class="text-sm w-40 pr-2 bg-indigo-200 border-indigo-300 placeholder:text-slate-500 rounded-lg mb-2 focus:outline-none focus:ring focus:ring-violet-600 h-6"
					type="text"
					name="item"
					autocomplete="off"
					placeholder="במבה"
				/>
				<label for="quantity"> כמה {{ newItem }}?</label>
				<input
					v-model="quantity"
					class="text-sm w-10 bg-indigo-200 border-indigo-300 placeholder:text-slate-500 rounded-lg mb-2 focus:outline-none focus:ring focus:ring-violet-600 h-6 pr-2"
					type="number"
					name="quantity"
					pattern="\d*"
					autocomplete="off"
					placeholder="2"
				/>
				<ul class="flex flex-col">
					<label for="priority"> רמת דחיפות</label>
					<select
						name="priority"
						id="priority"
						v-model="newItemPriority"
						class="text-sm bg-indigo-200 border-indigo-300 rounded-lg mb-2 focus:outline-none focus:ring focus:ring-violet-600 h-6 pr-2"
					>
						<option value="נמוכה">נמוכה</option>
						<option value="גבוהה">גבוהה</option>
					</select>
				</ul>
				<span>
					נוסיף
					{{ quantity }} {{ newItem }}
					בחשיבות
					{{ newItemPriority }}</span
				>
				<button
					type="submit"
					class="flex gap-3 max-w-max text-sm pt-2 px-4 bg-indigo-400 text-white h-10 rounded-lg focus:outline-none focus:ring focus:ring-violet-600 shadow-md active:shadow-sm active:translate-y-[0.1rem]"
					@keyup.enter="addItemToArray()"
				>
					<img
						src="../public/134224_add_plus_new_icon.svg"
						alt=""
					/>
					<span>הוסף לרשימה</span>
				</button>
			</form>
		</div>
		<div class="container max-h-max mx-auto font-sans max-w-max md:mt-0 mt-8 rounded-xl bg-sky-50 flex flex-col justify-start shadow-xl py-10 px-12">
			<ul>
				<li
					class="mb-2 text-[1.3rem] flex flex-row gap-2"
					v-for="item in items"
					:key="item.id"
				>
					<span :class="{ buyDone: item.isPurchase, highPriority: item.priority.isPriority }">{{ item.quantity }} {{ item.item }}</span> |
					<button
						@click.prevent="deleteItem(item.id)"
						class="text-sm bg-red-400 text-w font-medium px-2 py-1 rounded-lg"
						type="button"
					>
						מחק
					</button>
					<button
						@click.prevent="togglePurchase(item)"
						class="text-sm bg-lime-400 text-w px-2 font-medium py-1 rounded-lg"
						type="button"
					>
						נקנה
					</button>
				</li>
			</ul>
		</div>
	</div>
</template>

<style scoped></style>
