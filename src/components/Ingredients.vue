<template>
        <div>
         Ingredients:
            <ul>
              <li
                v-for="(r, index) in ingredients"
                :key="index"
              >
                {{ r.name_and_amount }}
              </li>
            </ul>
        </div>
  
</template>

<script>
export default {
    props: {
    ingredients: {
      type: Array,
      required: true
    }

  }
}
</script>

<style>

</style>