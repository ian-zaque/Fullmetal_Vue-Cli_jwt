<template>
    <div class="flex-center">
        <div class="card">
            <div class="card-body">
                <h5 class="card-title" style="font-size:50px;">Entre em <strong>Amestris</strong>!</h5>
                <div v-if="msg==null" class="flex-center">
                    <div class="form-group">
                        <div class="row justify-content-center">
                            <div class="form-group col col-8">
                                <label>Email: </label>
                                <input v-model="user.email" class="form-control" type="email" name="email" id="email" required>
                            </div>
                        </div>
                        <div class="row justify-content-center" >
                            <div class="form-group col col-8">
                                <label>Senha: </label>
                                <input v-model="user.password" class="form-control" type="password" name="senha" id="senha" required>
                            </div>
                        </div>
                        <div class="row justify-content-center">
                            <div class="form-group col col-12">
                                <button @click="logar()" :disabled="isRequesting==true" type="button" class="item btn btn-primary">Entrar</button>                                    
                            </div>
                        </div>          
                    </div>
                    <hr>
                    <div class="row justify-content-center">
                        <router-link to="/"><button type="button" class="item btn btn-outline-primary" replace>Homepage</button></router-link>
                        <router-link to="/cadastro"><button type="button" class="item btn btn-outline-primary" replace>Cadastrar-se</button></router-link>
                    </div>                   
                </div>
                <div v-else>
                    <div v-if="logou==true">
                        <div :class="msg.data.access_token?'alert alert-success':'alert alert-danger'" role="alert">
                                {{msg.statusText}}
                        </div>
                    </div>
                    <div class="row justify-content-center">
                        <router-link to="/"><button type="button" class="item btn btn-outline-primary" replace>Homepage</button></router-link>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name:'LoginUser',
    data() {
        return {
            user:{ email:'', password:'' }, msg:null, logou:false, isRequesting:false,
        }
    },
    computed: {
        logado() { return this.$store.state.Auth.status.loggedIn; }
    },

    created() {
        if (this.logado) { this.$router.replace('/'); }
    },

    methods: {
        logar(){
            this.isRequesting=true;
            this.$store.dispatch('Auth/login',this.user)
                .then(res=>{ this.msg =res; this.logou=true; this.isRequesting=false;
                })
                .catch(error=>{ this.msg=error; this.logou=false; this.isRequesting=false;
                    console.error(error);
                });
        }
    },
}
</script>
<style>
    .item{ position: relative; font-size: 20px; line-height: 14px; text-decoration: none; 
        transition: 0.4s ease; margin: 0px 10px 0px 10px;
    }

    .item:hover{ color: #313238; font-style: italic; 
        text-decoration: none; font-size: 24px; margin: 0px 10px 0px 10px;
    }

    .item:hover::after{ color: #313238; font-style: italic; 
        text-decoration: none; font-size: 24px; margin: 0px 10px 0px 10px;
    }

    .links{
        color: #636b6f; padding: 0 25px; font-size: 13px; font-weight: 600;
        letter-spacing: .1rem; text-decoration: none; text-transform: uppercase;
    }
</style>