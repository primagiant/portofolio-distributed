<template>
    <nav class="navbar col-lg-12 col-12 p-0 fixed-top d-flex flex-row">
        <div class="text-center navbar-brand-wrapper d-flex align-items-center justify-content-center">
            <a class="navbar-brand brand-logo" href="">
                <img :src="'/portofolio.terdistribusi/images/logo-mini.svg'" alt="logo" />
                <span>Portofolio</span>
            </a>
            <a class="navbar-brand brand-logo-mini" href="">
                <img :src="'/portofolio.terdistribusi/images/logo-mini.svg'" alt="logo" />
            </a>
        </div>
        <div class="navbar-menu-wrapper d-flex align-items-center justify-content-end">
            <button class="navbar-toggler navbar-toggler align-self-center" type="button" data-toggle="minimize">
                <span class="icon-menu"></span>
            </button>
            <ul class="navbar-nav navbar-nav-right">
                <li class="nav-item dropdown">
                    <a @click.prevent="readNotif" class="nav-link count-indicator dropdown-toggle" id="notificationDropdown" href="#" data-toggle="dropdown">
                        <i class="icon-bell mx-0"></i>
                        <span class="" ref="indicator"></span>
                    </a>
                    <div class="dropdown-menu dropdown-menu-right navbar-dropdown preview-list" aria-labelledby="notificationDropdown">
                        <p class="mb-0 font-weight-normal float-left dropdown-header">Notifications</p>
                        <template v-if="notifikasi.length !== 0">
                            <router-link :to="{ name: 'portofolio' }" v-for="(notif, index) in notifikasi" :key="index" class="dropdown-item preview-item">
                                <div class="preview-thumbnail">
                                    <div class="preview-icon bg-success">
                                        <i class="ti-info-alt mx-0"></i>
                                    </div>
                                </div>
                                <div class="preview-item-content">
                                    <h6 class="preview-subject font-weight-normal">{{ notif.title }}</h6>
                                    <p class="font-weight-light small-text mb-0 text-muted">1 hari yang lalu</p>
                                </div>
                            </router-link>
                        </template>
                        <template v-else>
                            <div class="dropdown-item preview-item">
                                <p class="font-weight-light small-text mb-0 text-muted">Yahh, tidak ada Notifikasi untuk hari ini</p>
                            </div>
                        </template>
                    </div>
                </li>
                <li class="nav-item nav-profile dropdown">
                    <a class="nav-link dropdown-toggle" href="#" data-toggle="dropdown" id="profileDropdown">
                        <img :src="'/portofolio.terdistribusi/images/default.svg'" alt="profile" />
                    </a>
                    <div class="dropdown-menu dropdown-menu-right navbar-dropdown" aria-labelledby="profileDropdown">
                        <button @click.prevent="logout" class="dropdown-item">
                            <i class="ti-power-off text-primary"></i>
                            Logout
                        </button>
                    </div>
                </li>
            </ul>
            <button class="navbar-toggler navbar-toggler-right d-lg-none align-self-center" type="button" data-toggle="offcanvas">
                <span class="icon-menu"></span>
            </button>
        </div>
    </nav>
</template>

<script>
export default {
    data() {
        return {
            notifikasi: {},
            csrf: document.querySelector('meta[name="csrf-token"]').getAttribute("content"),
        };
    },
    created() {
        axios.get("http://localhost:3000/notifikasi").then((response) => {
            this.notifikasi = response.data;
            console.log(this.notifikasi);

            if (this.notifikasi.length != 0) {
                this.$refs.indicator.classList.add("count");
            }
            readNotif();
        });
    },
    methods: {
        logout: function () {
            axios.post("/portofolio.terdistribusi/logout").then((response) => {
                window.location.href = "/portofolio.terdistribusi/login";
            });
        },
        readNotif: function () {
            this.$refs.indicator.classList.remove("count"); //menghapus indicator notif
            console.log("sukses");
        },
    },
};
</script>

<style scoped>
.navbar .brand-logo img {
    width: 35px;
    height: 35px;
}
.navbar .brand-logo span {
    font-family: "Nunito", sans-serif;
    font-weight: 700;
    font-size: 20px;
    margin-left: 6px;
}
</style>
