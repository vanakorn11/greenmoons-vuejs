# greenmoons-vuejs

# Hide element

v-if : hide => hide dom

vue = <div v-if="isToggle">hello</div>

react = {isToggle && <div>hello</div>}

v-show : hide => display none

vue = <div v-show="isToggle">hello</div>

react = <div :style="`display : ${isToggle ? 'block' : 'none'}`">hello</div>

# Loob

vue = <div v-for="(item,index) in arr" :key="index"> {{ item }} </div>

react = {arr.map((item,index) => ( <div key={index}> {item} </div> ))}

# Mount Component

vue = onMounted(() => {})

react = useEffect(() => {})

# state

const count = 0 // state not rerender from ui

const countRef = ref<sometype>(0) // use for state rerender ui

const state = reactive<sometype>({}) // use for object
