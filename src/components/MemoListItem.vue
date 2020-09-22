<template>
  <li><p @click="$emit('show', memo.id)"> {{ memo.title }} </p></li>
</template>

<script>
export default {
  props: {
    memo: {
      type: Object,
      required: true
    }
  }
}
</script>
