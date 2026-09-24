<template>
  <el-dialog title="Accesos Directos" :visible.sync="showDialog" @close="close" top="20vh" width="40%">
    <el-table :data="skins">
      <el-table-column prop="name" label="Nombre" width="150"></el-table-column>
      <el-table-column prop="filename" label="Archivo" width="150"></el-table-column>
      <el-table-column prop="" label="Acciones" width="100">
        <template slot-scope="scope">
          <a class="btn btn-info btn-sm"
             :href="'/configurations/visual/download_skin/'+scope.row.id"
             :download="scope.row.filename">
            <i class="fas fa-download"></i>
          </a>
          <button v-if="scope.row.name !== 'Default' && skins.length > 1" type="button" class="btn btn-sm btn-danger" @click="deleteSkin(scope.row.id)">
            <i class="fas fa-trash"></i>
          </button>
        </template>
      </el-table-column>
    </el-table>
    <el-upload
      :headers="headers"
      :multiple="false"
      :on-remove="handleRemove"
      class="upload-demo pt-3"
      ref="upload"
      accept=".css,text/css"
      :action="`/configurations/visual/upload_skin`"
      :show-file-list="true"
      :on-success="onSuccess"
      :on-error="errorUpload"
      :limit="1">
      <el-button slot="trigger" size="small" type="primary">Selecciona un archivo</el-button>
      <div slot="tip" class="el-upload__tip">Solo archivos css</div>
    </el-upload>
    <span slot="footer" class="dialog-footer">
      <el-button @click.prevent="close()">Cerrar</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    props:['showDialog', 'skins'],
    data() {
      return {
        form: { },
        formLabelWidth: '120px',
        modules: [],
        headers: headers_token,
        index_file: null,
        records: [],
      };
    },
    created() {
    },
    methods: {
      close() {
          this.$emit('update:showDialog', false)
      },
      handleRemove(file, fileList) {
        this.records[this.index_file].filename = null
        this.records[this.index_file].temp_path = null
        this.fileList = []
        this.index_file = null
      },
      errorUpload(error)
      {
          const message = error && error.message
              ? error.message
              : 'Error al subir el archivo';
          this.$message({message: message, type: 'error'});
      },
      onSuccess(response, file, fileList) {
        this.fileList = fileList
        if (response.success) {
          // this.index_file = response.data.index
          this.$message.success(response.message)
          if (response.skins !== undefined) {
            this.skins = response.skins;
            this.$emit("update:skins", response.skins, response.skin_id);
          }
        } else {
          this.cleanFileList()
          this.$message.error(response.message)
          console.log(response.message);
        }
      },
      cleanFileList(){
        this.fileList = []
      },
      deleteSkin(id) {
        this.form.id = id;
        this.$http.post(`/configurations/visual/delete_skin`, this.form).then(response => {
          let data = response.data;
          if (data.success) {
            this.$message.success(data.message);
          } else {
            this.$message.error(data.message);
          }
          if (data !== undefined && data.skins !== undefined) {
            this.skins = data.skins;
            this.$emit("update:skins", data.skins, data.skin_id);
          }

        }).catch(error => {
          if (error.response.status === 422) {
            this.errors = error.response.data.errors;
          } else {
            console.log(error);
          }
        });
      },
    }
  };
</script>

<style>
.v-modal {
  z-index: 1 !important;
}
</style>