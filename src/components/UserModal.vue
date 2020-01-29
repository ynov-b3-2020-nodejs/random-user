<script>
    export default {
        name: 'UserModal',
        props: {
            user: {
                type: Object,
            }
        },
        methods: {
            close() {
                this.$emit('close');
            },
            showUserDetails(u){
                return u.location.city;
            }
        },
        data (){
            return {

            }
        }
    };
</script>
<template>
    <transition name="modal-fade">
        <div class="modal-backdrop">
            <div class="UserModal"
                 role="dialog"
                 aria-labelledby="modalTitle"
                 aria-describedby="modalDescription"
            >
                <header
                        class="modal-header"
                        id="modalTitle"
                >
                    <slot name="header">
                        <b>User Profile</b>

                        <button
                                type="button"
                                class="btn-close"
                                @click="close"
                                aria-label="Close modal"
                        >
                            x
                        </button>
                    </slot>
                </header>
                <section
                        class="modal-body"
                        id="modalDescription"
                >
                    <slot name="body">
                        <img class="avatar" :src="user.picture.large"/>
                        <div class="profileCard">
                                <div class="profileDetail">
                                    <b>First name:</b> {{ user.name.first }}<br>
                                    <b>Last name:</b> {{ user.name.last }}<br>
                                    <b>Email:</b> {{ user.email }}<br>
                                    <b>Phone:</b> {{ user.phone }}<br>
                                    <b>City:</b> {{ user.location.city }}<br>
                                    <b>Country:</b> {{ user.location.country }}<br>
                            </div>
                        </div>
                    </slot>
                    <slot name="map">
                        <div class="mapFrame">

                        </div>
                    </slot>
                </section>
                <footer class="modal-footer">
                    <slot name="footer">
<!--                        I'm the default footer!-->
                        <br>
                        <button
                                type="button"
                                class="btn-orange"
                                @click="close"
                                aria-label="Close modal"
                        >
                            Fermer
                        </button>
                    </slot>
                </footer>
            </div>
        </div>
    </transition>
</template>

<style>
    .profileCard {
        position: relative;
        display: flex;
        /*padding: 30px;*/

    }
    .profileDetail {
        padding: 30px;
        text-align: left
    }
    .avatar {
        position: relative;
        display: flex;
        margin-left: 30px;
        margin-top: 30px;
    }
    .modal-backdrop {
        position: relative;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        background-color: rgba(0, 0, 0, 0.3);
        /*background-color: transparent;*/
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .UserModal {
        background: #FFFFFF;
        box-shadow: 2px 2px 20px 1px;
        overflow-x: auto;
        display: flex;
        flex-direction: column;
        width: 700px;
        height: 500px;
    }

    .modal-header,
    .modal-footer {
        padding: 15px;
        display: flex;
        height: 50px;
    }

    .modal-header {
        border-bottom: 1px solid #eeeeee;
        color: #D78900;
        justify-content: space-between;
    }

    .modal-footer {
        border-top: 1px solid #eeeeee;
        justify-content: flex-end;
    }

    .modal-body {
        position: relative;
        padding: 20px 10px;
    }

    .btn-close {
        border: none;
        font-size: 25px;
        cursor: pointer;
        font-weight: bold;
        margin-top: -10px;
        color: #D78900;
        background: transparent;
    }

    .btn-orange {
        color: white;
        background: #D78900;
        border: 1px solid #D78900;
        border-radius: 2px;
    }
</style>