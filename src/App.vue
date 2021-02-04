<template>
    <div id="app">
        <div class="mainContainer middle-x pr">
            <h1>Olá, {{ greeting }}</h1>
            <!-- status -->
            <p v-if="this.list != ''">{{ status }} {{ completed }}</p>
            <p v-else>Para começar, adicione uma tarefa.</p>
            <!-- tasks -->
            <task-list 
                :list="list" 
                @taskDeleted="deleteTask" 
                @taskChanged="toggleTaskChanged"
            />
            <!-- btn add -->
            <button type="button" class="callModal" v-wave @click="modal = true">Adicionar tarefa</button>
            <a class="c" href="https://github.com/willaug" target="_blank">Feito com ❤️ por William Augusto</a>
        </div>
        <!-- modal -->
        <task-modal 
            :modal="modal" 
            @changeModal="modal = $event" 
            @taskAdded="addTask" 
            :list="list"
        />
    </div>
</template>

<script>
import TaskList from './components/TaskList.vue'
import TaskModal from './components/TaskModal.vue'

export default {
    name: 'app',
    components: { TaskList, TaskModal },
    data() {
        return{
            greeting: '',
            modal: false,
            list: []
        }
    },
    computed: {
        status() {
            const total = this.list.length

            if(total == 0)
            {
                return ''
            }
            else if(total == 1){
                return 'Você possui uma tarefa adicionada.'
            }
            else{
                return 'Você possui ' + total + ' tarefas adicionadas.'
            }
        },
        completed() {
            const done = (this.list.filter((item) => item.concluded == true)).length
            
            if(done == 0)
            {
                return ''
            }
            else if(done == 1){
                return '1 concluída.'
            }
            else{
                return done + ' concluídas.'
            }
        }
    },
    watch: {
        list: {
            deep: true,
            handler() {
                localStorage.setItem('list', JSON.stringify(this.list))
            }
        }
    },
    methods: {
        addTask(task) {
            this.list.push({
                description: task.text,
                concluded: false
            })
        },
        deleteTask(i) {
            this.list.splice(i, 1)
        },
        toggleTaskChanged(i) {
            this.list[i].concluded = !this.list[i].concluded
        }
    },
    created() {
        var hour = new Date().getHours()
        
        //get hour
        if( hour >= 6 && hour < 12)
        {
            this.greeting = 'bom dia!'
        }
        else if( hour >= 12 && hour < 18)
        {
            this.greeting = 'boa tarde!'
        }
        else
        {
            this.greeting = 'boa noite!'
        }

        //get item saved
        const array = JSON.parse(localStorage.getItem('list'))
        this.list = Array.isArray(array) ? array : []
    }
}
</script>