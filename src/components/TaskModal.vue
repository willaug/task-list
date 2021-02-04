<template>
    <transition name="fade">
        <div class="modal-wrapper" v-if="modal == true">
            <div class="modal-container center pa">
                <h3>Adicionar tarefa</h3>
                <button type="button" class="modal-close" v-wave @click="changeModal"></button>
                <input type="text" id="text" placeholder="Adicione uma nova tarefa" maxlength="60" v-model="text" @keyup.enter="add">
                <small v-if="existent">Tarefa já existente.</small>
                <div class="buttons-modal">
                    <button type="button" class="modal-1" v-wave @click="add">Salvar</button>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>

export default {
    props: ['list', 'modal'],
    data() {
        return{
            text: '',
            existent: false
        }
    },
    watch: {
        text() {
            const reallyNew = this.list.filter(t => t.description === this.text).length == 0
            
            if(!reallyNew){
                this.existent = true
            }
            else{
                this.existent = false
            }
        }
    },
    methods: {
        changeModal() {
            this.$emit('changeModal', false)
        },
        add() {
            const reallyNew = this.list.filter(t => t.description === this.text).length == 0

            if(this.text != '' && reallyNew)
            {
                this.$emit('taskAdded', { text: this.text })
                this.changeModal()
                this.text = ''
            }
            else{
                document.getElementById('text').focus()
            }
        }
    }
}
</script>

<style scoped>
    .fade-enter-active, .fade-leave-active {
        transition: opacity .3s;
    }
    .fade-enter, .fade-leave-to{
        opacity: 0;
    }
</style>