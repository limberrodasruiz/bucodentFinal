<template>
    <main class="main">
    <!-- Breadcrumb -->
    <ol class="breadcrumb">
        <li class="breadcrumb-item"><a href="/">Escritorio</a></li>
    </ol>
    <div class="container-fluid">
        <!-- Ejemplo de tabla Listado -->
        <div class="card">
            <div class="card-header">
                <i class="fa fa-align-justify"></i> Paciente
                <button type="button" @click="abrirModal('paciente', 'registrar')" class="btn btn-secondary">
                    <i class="icon-plus"></i>&nbsp;Nuevo
                </button>
            </div>
            <div class="card-body">
                <div class="form-group row">
                    <div class="col-md-6">
                        <div class="input-group">
                            <select class="form-control col-md-3" v-model="criterio">
                              <option value="nombre">Nombre</option>
                              <option value="apellido">Apellido</option>
                              <option value="ci">CI</option>
                              <option value="fechanacimiento">FechaNacimiento</option>
                              <option value="direccion">Direccion</option>
                              <option value="telefono">Telefono</option>
                              <option value="email">Email</option>
                            </select>
                            <input type="text" v-model="buscar" @keyup.enter="listarPaciente(1,buscar,criterio)" class="form-control" placeholder="Texto a buscar">
                            <button type="submit" @click="listarPaciente(1,buscar,criterio)" class="btn btn-primary"><i class="fa fa-search"></i> Buscar</button>
                        </div>
                    </div>
                </div>
                <table class="table table-bordered table-striped table-sm">
                    <thead>
                        <tr>
                            <th>Opciones</th>
                            <th>Nombre</th>
                            <th>Apellido</th>
                            <th>Ci</th>
                            <th>FechaNacimiento</th>
                            <th>Direccion</th>
                            <th>Telefono</th>
                            <th>Email</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="paciente in arrayPaciente" :key="paciente.id">
                            <td>
                                <button type="button" @click="abrirModal('paciente', 'actualizar', paciente)" class="btn btn-warning btn-sm">
                                  <i class="icon-pencil"></i>
                                </button> &nbsp;
                            </td>
                            <td v-text="paciente.nombre"></td>
                            <td v-text="paciente.apellido"></td>
                            <td v-text="paciente.ci"></td>
                            <td v-text="paciente.fechanacimiento"></td>
                            <td v-text="paciente.direccion"></td>
                            <td v-text="paciente.telefono"></td>
                            <td v-text="paciente.email"></td>
                            
                        </tr>
                        
                    </tbody>
                </table>
                <nav>
                    <ul class="pagination">
                        <li class="page-item" v-if="pagination.current_page > 1">
                            <a class="page-link" href="#" @click.prevent="cambiarPagina(pagination.current_page - 1,buscar,criterio)">Ant</a>
                        </li>
                        <li class="page-item" v-for="page in pagesNumber" :key="page" :class="[page == isActived ? 'active' : '']">
                            <a class="page-link" href="#" @click.prevent="cambiarPagina(page,buscar,criterio)" v-text="page"></a>
                        </li>
                        <li class="page-item" v-if="pagination.current_page < pagination.last_page">
                            <a class="page-link" href="#" @click.prevent="cambiarPagina(pagination.current_page + 1,buscar,criterio)">Sig</a>
                        </li>
                    </ul>
                </nav>
            </div>
        </div>
        <!-- Fin ejemplo de tabla Listado -->
    </div>
    <!--Inicio del modal agregar/actualizar-->
    <div class="modal fade" tabindex="-1" :class="{'mostrar' : modal}" role="dialog" aria-labelledby="myModalLabel" style="display: none;" aria-hidden="true">
        <div class="modal-dialog modal-primary modal-lg" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h4 class="modal-title" v-text="tituloModal"></h4>
                    <button type="button" class="close" @click="cerrarModal()" aria-label="Close">
                      <span aria-hidden="true">??</span>
                    </button>
                </div>
                <div class="modal-body">
                    <form action="" method="post" enctype="multipart/form-data" class="form-horizontal">
                        <div class="form-group row">
                            <label class="col-md-3 form-control-label" for="text-input">Nombre</label>
                            <div class="col-md-9">
                                <input type="text" v-model="nombre" class="form-control" placeholder=" enter nombre del paciente">
                                
                            </div>
                        </div>
                        <div class="form-group row">
                            <label class="col-md-3 form-control-label" for="text-input">Apellido</label>
                            <div class="col-md-9">
                                <input type="text" v-model="apellido" class="form-control" placeholder=" enter apellido del paciente">
                                
                            </div>
                        </div>
                        <div class="form-group row">
                            <label class="col-md-3 form-control-label" for="email-input">Ci</label>
                            <div class="col-md-9">
                                <input type="text" v-model="ci" class="form-control" placeholder="Enter ci">
                            </div>
                        </div>
                        <div class="form-group row">
                            <label class="col-md-3 form-control-label" for="email-input">Fecha de Nacimiento</label>
                            <div class="col-md-9">
                                <input type="date" v-model="fechanacimiento" class="form-control" placeholder="Enter fecha de Nacimiento">
                            </div>
                        </div>
                        <div class="form-group row">
                            <label class="col-md-3 form-control-label" for="email-input">Direccion</label>
                            <div class="col-md-9">
                                <input type="text" v-model="direccion" class="form-control" placeholder="Enter direccion">
                            </div>
                        </div>
                        <div class="form-group row">
                            <label class="col-md-3 form-control-label" for="email-input">Telefono</label>
                            <div class="col-md-9">
                                <input type="number" v-model="telefono" class="form-control" placeholder="Enter telefono">
                            </div>
                        </div>
                        <div class="form-group row">
                            <label class="col-md-3 form-control-label" for="email-input">Email</label>
                            <div class="col-md-9">
                                <input type="email" v-model="email" class="form-control" placeholder="Enter email">
                            </div>
                        </div>
                        <div v-show="errorPaciente" class="form-group row div-error">
                            <div class="text-center text-error">
                                <div v-for="error in errorMostrarMsjPaciente" :key="error" v-text="error">

                                </div>
                            </div>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" @click="cerrarModal()">Cerrar</button>
                    <button type="button" v-if="tipoAccion==1" class="btn btn-primary" @click="registrarPaciente()">Guardar</button>
                    <button type="button" v-if="tipoAccion==2" class="btn btn-primary" @click="actualizarPaciente()">Actualizar</button>
                </div>
            </div>
            <!-- /.modal-content -->
        </div>
        <!-- /.modal-dialog -->
    </div>
    <!--Fin del modal-->
    
</main>
</template>

<script>
import Axios from 'axios';

    export default {
    data() {
        return {
            paciente_id: 0,
            nombre : '',
            apellido : '',
            ci : '',
            fechanacimiento : '',
            direccion : '',
            telefono : '',
            email : '',
            arrayPaciente : [],
            modal : 0,
            tituloModal : '',
            tipoAccion : 0,
            errorPaciente : 0,
            errorMostrarMsjPaciente : [],
            pagination : {
                'total' : 0,
                'current_page' : 0,
                'per_page' : 0,
                'last_page' : 0,
                'from' : 0,
                'to' : 0,
            },
            offset : 3,
            criterio : 'nombre',
            buscar : ''
        }
    },
    computed:{
        isActived: function(){
                return this.pagination.current_page;
            },
            //Calcula los elementos de la paginaci??n
            pagesNumber: function() {
                if(!this.pagination.to) {
                    return [];
                }
                
                var from = this.pagination.current_page - this.offset; 
                if(from < 1) {
                    from = 1;
                }

                var to = from + (this.offset * 2); 
                if(to >= this.pagination.last_page){
                    to = this.pagination.last_page;
                }  

                var pagesArray = [];
                while(from <= to) {
                    pagesArray.push(from);
                    from++;
                }
                return pagesArray;             

            }
    },
    methods : {
        listarPaciente (page,buscar,criterio) {
            let me=this;
            var url= '/paciente?page=' + page + '&buscar='+ buscar + '&criterio='+ criterio;
            axios.get(url).then(function (response) {
                var respuesta= response.data;
                me.arrayPaciente = respuesta.paciente.data;
                me.pagination= respuesta.pagination;
            })
            .catch(function(error) {
                console.log(error);
            });
           
        },
        cambiarPagina(page,buscar,criterio){
                let me = this;
                //Actualiza la p??gina actual
                me.pagination.current_page = page;
                //Envia la petici??n para visualizar la data de esa p??gina
                me.listarPaciente(page,buscar,criterio);
            },
        registrarPaciente(){
            if (this.validarPaciente()){
                return;
            }

            let me = this;


            axios.post('/paciente/registrar',{
                'nombre':this.nombre,
                'apellido':this.apellido,
                'ci':this.ci,
                'fechanacimiento':this.fechanacimiento,
                'direccion': this.direccion,
                'telefono': this.telefono,
                'email':this.email,
            }).then(function(response){
                me.cerrarModal();
                me.listarPaciente(1,'','nombre');
            }).catch(function (error) {
                console.log(error);
            });
        },
        actualizarPaciente(){
            if (this.validarPaciente()){
                return;
            }

            let me = this;


            axios.put('/paciente/actualizar',{
                'nombre':this.nombre,
                'apellido':this.apellido,
                'ci':this.ci,
                'fechanacimiento':this.fechanacimiento,
                'direccion': this.direccion,
                'telefono': this.telefono,
                'email':this.email,
                'id': this.paciente_id
            }).then(function(response){
                me.cerrarModal();
                me.listarPaciente(1,'','nombre');
            }).catch(function (error) {
                console.log(error);
            });
        },
        desactivarPaciente(id){
            swal({
                title: 'Esta seguro de desactivar este paciente?',
                type: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Aceptar!',
                cancelButtonText: 'Cancelar',
                confirmButtonClass: 'btn btn-success',
                cancelButtonClass: 'btn btn-danger',
                buttonsStyling: false,
                reverseButtons: true
                }).then((result) => {
                if (result.value) {
                    let me = this;

                    axios.put('/paciente/desactivar',{
                        'id': id
                    }).then(function (response) {
                        me.listarPaciente(1,'','nombre');
                        swal(
                        'Desactivado!',
                        'El registro ha sido desactivado con ??xito.',
                        'success'
                        )
                    }).catch(function (error) {
                        console.log(error);
                    });
                    
                    
                } else if (
                    // Read more about handling dismissals
                    result.dismiss === swal.DismissReason.cancel
                ) {
                    
                }
                }) 
        },
        activarPaciente(id){
            swal({
                title: 'Esta seguro de activar este paciente?',
                type: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Aceptar!',
                cancelButtonText: 'Cancelar',
                confirmButtonClass: 'btn btn-success',
                cancelButtonClass: 'btn btn-danger',
                buttonsStyling: false,
                reverseButtons: true
                }).then((result) => {
                if (result.value) {
                    let me = this;

                    axios.put('/paciente/activar',{
                        'id': id
                    }).then(function (response) {
                        me.listarPaciente(1,'','nombre');
                        swal(
                        'Activado!',
                        'El registro ha sido activado con ??xito.',
                        'success'
                        )
                    }).catch(function (error) {
                        console.log(error);
                    });
                    
                    
                } else if (
                    // Read more about handling dismissals
                    result.dismiss === swal.DismissReason.cancel
                ) {
                    
                }
                }) 
        },
        validarPaciente(){
            this.errorPaciente =0;
            this.errorMostrarMsjPaciente =[];

            if (!this.nombre) this.errorMostrarMsjPaciente.push("El nombre del paciente no puede estar vacio");
            if (!this.apellido) this.errorMostrarMsjPaciente.push("El apellido del paciente no puede estar vacio");
            if (!this.ci) this.errorMostrarMsjPaciente.push("El ci del paciente no puede estar vacio");
            if (!this.fechanacimiento) this.errorMostrarMsjPaciente.push("La fecha de nacimiento del paciente no puede estar vacio");
            if (!this.direccion) this.errorMostrarMsjPaciente.push("La direccion del paciente no puede estar vacio");
            if (!this.telefono) this.errorMostrarMsjPaciente.push("El telefono del paciente no puede estar vacio");
            if (!this.email) this.errorMostrarMsjPaciente.push("El email del paciente no puede estar vacio");
            if (this.errorMostrarMsjPaciente.length) this.errorPaciente = 1;
            return this.errorPaciente;
        },
        cerrarModal(){
            this.modal = 0;
            this.tituloModal = '';
            this.nombre = '';
            this.apellido = '';
            this.ci = '';
            this.fechanacimiento = '';
            this.direccion = '';
            this.telefono = '';
            this.email = '';
            this.errorPaciente=0;
        },
        abrirModal(modelo, accion, data = []){
            switch(modelo){
                case "paciente":
                {
                    switch(accion){
                        case 'registrar':
                        {
                            this.modal = 1;
                            this.tituloModal = 'Registrar Paciente';
                            this.nombre = '';
                            this.apellido = '';
                            this.ci = '';
                            this.fechanacimiento = '';
                            this.direccion = '';
                            this.telefono = '';
                            this.email = '';
                            this.tipoAccion = 1;
                            break;
                        }
                        case 'actualizar':
                        {
                            //console.log(data);
                            this.modal=1;
                            this.tituloModal='Actualizar paciente';
                            this.tipoAccion=2;
                            this.paciente_id=data['id'];
                            this.nombre = data['nombre'];
                            this.apellido = data['apellido'];
                            this.ci= data['ci'];
                            this.fechanacimiento= data['fechanacimiento'];
                            this.direccion= data['direccion'];
                            this.telefono= data['telefono'];
                            this.email = data['email'];
                            break;
                        }
                    }
                }    
            }
        }
    },
    mounted() {
        this.listarPaciente(1,this.buscar,this.criterio);
}
}
</script>
<style>
    .modal-content{
        width: 100% !important;
        position: absolute !important;
    }

    .mostrar{
        display: list-item !important;
        opacity: 1 !important;
        position: absolute !important;
        background-color: #3c29297a !important;
    }
    .div-error{
        display: flex;
        justify-content: center;
    }
    .text-error{
        color: red !important;
        font-weight: bold;
    }
</style>
