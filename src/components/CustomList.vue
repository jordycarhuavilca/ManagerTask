<template>
  <li class="flex">
    <div>
      {{ description }}
    </div>
    <div class="flex">
      <button @click="remove">Remove</button>
      <button @click="update">Update</button>
    </div>
  </li>
</template>

<script>
export default {
  props: {
    description: {
      type: String,
      required: true,
    },
  },
  methods: {
    remove() {
      this.$emit("remove");
    },
    update() {
      this.$emit("update");
    },
  },
};
</script>

<style>
.flex {
  display: flex;
  flex-direction: row;
  gap: 1rem;
}
</style>
