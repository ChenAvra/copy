<template>
        <div>
         Instructions:

         
            <ul>
              <li
                v-for="(r, index) in instructions"
                :key="index"
              >
                {{ r }}
              </li>
            </ul>
        </div>
  
</template>

<script>
export default {
    props: {
    instructions: {
      type: Array,
      required: true
    }

  }
}
</script>

<style>

</style>