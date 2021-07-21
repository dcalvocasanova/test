<template>
    <div>
        <button @click="exportAsExcel"> Export Excel </button>
        <button @click="add"> Agregar  </button>
        <button @click="exportAsCsv"> Export CSV </button>

        <input type="file" id="file" ref="myFiles" class="custom-file-input"  @change= "jalar" multiple>

    <vue-excel-editor v-model="jsondata" filter-row ref="grid" no-footer>
        <vue-excel-column field="user"   label="User ID"       type="string" width="80px" key-field />
        <vue-excel-column field="name"   label="Name"          type="string" width="150px" />
        <vue-excel-column field="phone"  label="Contact"       type="string" width="130px" :validate="validPhoneNum"  />
        <vue-excel-column field="gender" label="Gender"        type="map"    width="150px"  :allow-keys="[ 'M' , 'F' ,'U']" :options="{'M':'Male','F':'Female','U':'Unknown'}" />
        <vue-excel-column field="age"    label="Age"           type="number" width="70px" />
        <vue-excel-column field="birth"  label="Date Of Birth" type="date"   width="80px"  />
         <vue-excel-column field="rol" label="ROL"        type="select"    width="150px"  :allow-keys="[ 'Admistrador' , 'Ligia' ]" :options="['Admistrador' , 'Ligia']" />
    </vue-excel-editor>
    </div>
</template>

<script>

 export default {
    name: 'app',
    data() {
        return {
            jsondata: []
            /*    {user: 'hc', name: 'Harry Cole',    rol: '',  phone: '1-415-2345678', gender: 'M', age: 25, birth: '1997-07-01'},
                {user: 'sm', name: 'Simon Minolta', rol: '', phone: '1-123-7675682', gender: 'U', age: 20, birth: '1999-11-12'},
                {user: 'ra', name: 'Raymond Atom',  rol: '',phone: '1-456-9981212', gender: 'M', age: 19, birth: '2000-06-11'},
                {user: 'ag', name: 'Mary George',   rol: '',phone: '1-556-1245684', gender: 'F', age: 22, birth: '2002-08-01'},
                {user: 'kl', name: 'Kenny Linus',   rol: '', phone: '1-891-2345685', gender: 'M', age: 29, birth: '1990-09-01'}
            ]*/
        }        
    },
    methods: {

    add () {
        const rec = {
            user: ' ',
            name: ' ',
            phone: ' ',
            gender: ' ',
            age: 0,
            birth: ' '
        }
        // Call this to new record
        this.$refs.grid.newRecord(rec)
    },
    exportAsExcel () {
        const format = 'xlsx'
        const exportSelectedOnly = false
        const filename = 'test'
        this.$refs.grid.exportTable(format, exportSelectedOnly, filename)
    },
    exportAsCsv () {
        const format = 'csv'
        const exportSelectedOnly = false
        const filename = 'test'
        this.$refs.grid.exportTable(format, exportSelectedOnly, filename)
    },
    jalar(file){      
            this.$refs.grid.doImport(file) 
            this.$refs.grid.selected = {}
            
    },
    validPhoneNum (content, oldContent, record, field) {
        if (content === '') return 'Campo requerido'
        if (!/^[0-9]{1}-[0-9]{3}-[0-9]{7}$/.test(content)) return 'Este no es un número telefónico'
        return '' // return empty string if there is no error
    }
}
}


</script>
