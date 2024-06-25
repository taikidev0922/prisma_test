<template>
  <div>
    <h1>User Management</h1>
    <form @submit.prevent="createUser">
      <TextInput name="name" />
      <TextInput name="email" type="email" />
      <ComboBox :items="departments" itemText="name" name="departmentId" />
      <button type="submit">Add User</button>
    </form>
    <ul>
      <li v-for="user in users" :key="user.id">
        {{ user.name }} ({{ user.email }}) - {{ user.department.name }}
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import type { User } from "@/server/api/users";
import type { Department } from "@/server/api/departments";
import { useForm } from "vee-validate";
import * as yup from "yup";

const schema = yup.object().shape({
  name: yup.string().required(),
  email: yup.string().email().required(),
  departmentId: yup.number().required(),
});

const { handleSubmit } = useForm({
  validationSchema: schema,
});

const users = ref<User[]>([]);
const departments = ref<Department[]>([]);

onMounted(async () => {
  users.value = await $fetch<User[]>("/api/users");
  departments.value = await $fetch<Department[]>("/api/departments");
});

const createUser = handleSubmit(async (values, { resetForm }) => {
  try {
    const createdUser = await $fetch<User>("/api/users", {
      method: "POST",
      body: values,
    });

    // 新しいユーザーをリストに追加
    users.value.push(createdUser);

    // フォームをリセット
    resetForm();
  } catch (error) {
    console.error("Error creating user:", error);
    // エラーハンドリングをここに追加（例：エラーメッセージの表示）
  }
});
</script>
