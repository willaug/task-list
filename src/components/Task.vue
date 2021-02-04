<template>
    <div>
        <label class="task pr" v-wave @change="$emit('taskChanged', list)">
            <input type="checkbox" :checked="list.concluded == true">
            <span class="checkmark middle-y"></span>
            <span class="description">{{ list.description }}</span>
            <div class="close middle-y" @click.stop="$emit('taskDeleted', list)">&times;</div>
        </label>
    </div>
</template>

<script>
export default {
   props: {
       list: { type: Object, required: true }
   }
}
</script>

<style>

</style>