<style>
    .widget-user-header {
        background-position: center;
        background-size: cover;
        height: 300px;
    }
</style>

<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12">
                <div class="box box-widget widget-user">
                    <!--box box-widget widget-user-->
                    <!-- Add the bg color to the header using any of the bg-* classes -->
                    <div class="widget-user-header bg-black" style="background-image:url('./img/user_cover.jpg')">
                        <h3 class="widget-user-username">Anower Hasan</h3>
                        <h5 class="widget-user-desc">Web Developer</h5>
                    </div>
                    <div class="widget-user-image">
                        <img class="img-circle" :src="getUserProfile()" alt="User Avatar">
                    </div>
                    <div class="box-footer">
                        <div class="row">
                            <div class="col-sm-4 border-right">
                                <div class="description-block">
                                    <h5 class="description-header">3,200</h5>
                                    <span class="description-text">SALES</span>
                                </div>
                                <!-- /.description-block -->
                            </div>
                            <!-- /.col -->
                            <div class="col-sm-4 border-right">
                                <div class="description-block">
                                    <h5 class="description-header">13,000</h5>
                                    <span class="description-text">FOLLOWERS</span>
                                </div>
                                <!-- /.description-block -->
                            </div>
                            <!-- /.col -->
                            <div class="col-sm-4">
                                <div class="description-block">
                                    <h5 class="description-header">35</h5>
                                    <span class="description-text">PRODUCTS</span>
                                </div>
                                <!-- /.description-block -->
                            </div>
                            <!-- /.col -->
                        </div>
                        <!-- /.row -->
                    </div>
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-md-12">
                <div class="box">
                    <div class="nav-tabs-custom">
                        <ul class="nav nav-tabs">
                            <li class=""><a href="#activity" data-toggle="tab" aria-expanded="false">Activity</a></li>
                            <li class="active"><a href="#settings" data-toggle="tab" aria-expanded="true">Settings</a>
                            </li>
                        </ul>
                    </div>
                    <div class="card-body">
                        <div class="tab-content">
                            <div class="tab-pane" id="activity">
                                <h3>Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem
                                    Ipsum has been the industry's standard dummy text ever since the 1500s</h3>
                            </div>

                            <div class="tab-pane active" id="settings">
                                <form class="form-horizontal">
                                    <div class="form-group">
                                        <label for="inputName" class="col-sm-2 control-label">Name</label>

                                        <div class="col-sm-10">
                                            <input v-model="form.name" type="email" class="form-control" id="name"
                                                   placeholder="Name">
                                        </div>
                                    </div>
                                    <div class="form-group">
                                        <label for="inputEmail" class="col-sm-2 control-label">Email</label>

                                        <div class="col-sm-10">
                                            <input v-model="form.email" type="email" class="form-control"
                                                   id="inputEmail"
                                                   placeholder="Email">
                                        </div>
                                    </div>
                                    <div class="form-group">
                                        <label for="inputName" class="col-sm-2 control-label">Password</label>

                                        <div class="col-sm-10">
                                            <input type="text" v-model="form.password" class="form-control" id="inputName" placeholder="Name">
                                        </div>
                                    </div>
                                    <div class="form-group">
                                        <label for="inputExperience" class="col-sm-2 control-label">Experience</label>

                                        <div class="col-sm-10">
                                            <textarea class="form-control" v-model="form.bio" id="inputExperience"
                                                      placeholder="Experience"></textarea>
                                        </div>
                                    </div>
                                    <div class="form-group">
                                        <label for="inputName" class="col-sm-2 control-label">Upload File</label>

                                        <div class="col-sm-10">
                                            <input type="file" class="form-control" name="file" @change="updateProfile" id="file">
                                        </div>
                                    </div>
                                    <div class="form-group">
                                        <div class="col-sm-offset-2 col-sm-10">
                                            <button @click.prevent="updateInfo" type="submit" class="btn btn-success">Update</button>
                                        </div>
                                    </div>
                                </form>
                            </div>
                            <!-- /.tab-pane -->
                        </div>
                        <!-- /.tab-content -->
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                form: new Form({
                    id: '',
                    name: '',
                    email: '',
                    password: '',
                    type: '',
                    bio: '',
                    photo: ''
                }),
            }
        },
        mounted() {
            console.log('Component mounted.')
        },
        methods: {
            getUserProfile(){
                return "/img/profile/"+ this.form.photo;
            },
            updateInfo()
            {
                this.$Progress.start();
                this.form.put('api/profile/')
                    .then(()=>{
                        this.$Progress.finish();
                    }).catch(()=>{

                })
            },
            updateProfile(e)
            {
                let file = e.target.files[0];
                // console.log(file);
                let reader = new FileReader();

                if(file['size'] <2111775 ){
                    reader.onloadend = (file) => {
                        // console.log('RESULT', reader.result)
                        this.form.photo = reader.result;
                    }
                    reader.readAsDataURL(file);
                }else {
                    Swal({
                        type: 'error',
                        title: 'oops..... it more than 2mb',
                        text: 'You are uploading large file more than 2 mb'
                    })
                }
            }
        },
        created() {
            axios.get("api/profile").then(({data}) => (this.form.fill(data)));
        }
    }
</script>
