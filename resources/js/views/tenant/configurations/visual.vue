<template>
    <div id="styleSwitcher" class="style-switcher">

        <a id="styleSwitcherOpen" class="style-switcher-open" href="#">
            <i class="fas fa-paint-brush"></i>
        </a>

        <form class="style-switcher-wrap" autocomplete="off">
            <h4>Configuraciones visuales</h4>
            <div v-if="visual == null">
                <h5 class="">No posee ajustes actualmente</h5>
                <a href="" class="text-warning" v-if="typeUser != 'integrator'">cargar ajustes por defecto</a>
                <br>
            </div>
            <div v-if="typeUser != 'integrator'" class="pt-3">
                <div class="text-right">
                    <a v-if="visuals.bg == 'white'"
                        href="/configurations/change-mode"
                        class="notification-icon text-secondary"
                        data-toggle="tooltip"
                        data-placement="bottom"
                        title="Modo noche"
                        style="text-decoration: none;">
                        <i class="fas fa-2x fa-moon"></i>
                    </a>
                    <a v-if="visuals.bg == 'dark'"
                        href="/configurations/change-mode"
                        class="notification-icon text-white"
                        data-toggle="tooltip"
                        data-placement="bottom"
                        title="Modo día"
                        style="text-decoration: none;">
                        <i class="fa-2x wi-sun"></i>
                    </a>
                </div>


                <div class="pt-3">
                    <h5>Menú lateral contraído</h5>
                    <div :class="{'has-danger': errors.compact_sidebar}">
                        <el-switch
                            v-model="form.compact_sidebar"
                            active-text="Si"
                            inactive-text="No"
                            @change="submitForm">
                        </el-switch>
                        <br>
                        <small class="form-control-feedback" v-if="errors.compact_sidebar" v-text="errors.compact_sidebar[0]"></small>
                    </div>
                </div>

                <div class="pt-3">
                    <h5>Cantidad de columnas en POS</h5>
                    <div :class="{'has-danger': errors.amount_plastic_bag_taxes}">
                        <el-slider
                            @change="submitForm"
                            v-model="form.colums_grid_item"
                            :min="3"
                            :max="6">
                        </el-slider>
                        <small class="form-control-feedback" v-if="errors.amount_plastic_bag_taxes" v-text="errors.amount_plastic_bag_taxes[0]"></small>
                    </div>
                </div>

                <div class="pt-3">
                    <h5>Cambiar tema</h5>
                    <div :class="{'has-danger': errors.compact_sidebar}">
                        <el-select
                            v-model="form.skin_id"
                            placeholder="Tema"
                            @change="submitForm"
                            class="pb-3">
                            <el-option
                                v-for="item in skins"
                                :key="item.id"
                                :label="item.name"
                                :value="item.id">
                            </el-option>
                        </el-select>
                        <small class="form-control-feedback" v-if="errors.compact_sidebar" v-text="errors.compact_sidebar[0]"></small>
                        <el-button type="button" @click="dialogSkins()" color="primary">Subir tema</el-button>
                    </div>
                </div>

            </div>
        </form>
        <dialog-skins :showDialog.sync="dialogSkinsVisible" :skins="skins" @update:skins="updateSkins"/>
    </div>
</template>

<script>
    import DialogSkins from './partials/dialog_skins.vue'
    export default {
        props:['visual','typeUser'],
        components: {
            DialogSkins
        },
        data() {
            return {
                loading_submit: false,
                resource: 'configurations',
                errors: {},
                form: {},
                visuals: {},
                skins: {},
                dialogSkinsVisible: false
            }
        },
        async created() {
            await this.initForm()
            await this.getRecords()
        },
        methods: {
            initForm() {
                this.errors = {}
                this.form = {
                    id: 1,
                    compact_sidebar: true,
                    colums_grid_item: 4,
                    enable_whatsapp: true,
                    phone_whatsapp: '',
                    skins: 1,
                }
            },
            getRecords() {
                this.$http.get(`/${this.resource}/record`) .then(response => {
                    if (response.data !== ''){
                        this.visuals = response.data.data.visual;
                        this.form = response.data.data;
                        this.skins = response.data.data.skins;
                    }
                });
            },
            submit() {
                this.visuals.navbar = 'fixed';
                this.$http.post(`/${this.resource}/visual_settings`, this.visuals).then(response => {
                    if (response.data.success) {
                        this.$message.success(response.data.message);
                    }
                    else {
                        this.$message.error(response.data.message);
                    }
                }).catch(error => {
                    if (error.response.status === 422) {
                        this.errors = error.response.data.errors;
                    }
                    else {
                        console.log(error);
                    }
                }).then(() => {
                    location.reload();
                });
            },
            submitForm() {
                this.loading_submit = true;
                this.$http.post(`/${this.resource}`, this.form).then(response => {
                    if (response.data.success) {
                        this.$message.success(response.data.message);
                        location.reload()
                    }
                    else {
                        this.$message.error(response.data.message);
                    }
                }).catch(error => {
                    if (error.response.status === 422) {
                        this.errors = error.response.data.errors;
                    }
                    else {
                        console.log(error);
                    }
                }).then(() => {
                    this.loading_submit = false;
                });
            },
            updateSkins(skins, activeSkinId) {
                this.skins = skins;
                if (activeSkinId !== undefined) {
                    this.form.skin_id = activeSkinId;
                } else if (Array.isArray(skins) && !skins.some(skin => String(skin.id) === String(this.form.skin_id))) {
                    this.form.skin_id = skins.length ? skins[0].id : null;
                }
            },
            dialogSkins() {
                this.dialogSkinsVisible = true
            },
        }
    }
</script>
