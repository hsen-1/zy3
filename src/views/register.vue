<template>
    <div id="background">
        <div id="logo">my notebook</div>
        <div id="login">
            <el-card class="box-card">
                <div id="form">
                    <el-input v-model="id" placeholder="请输入用户名(必填)"></el-input>
                    <el-input v-model="email" placeholder="请输入电子邮件地址(必填)"></el-input>
                    <el-input placeholder="请输入密码(必填)" v-model="password" show-password></el-input>
                    <el-input v-model="phone" placeholder="请输入手机号(选填)"></el-input>
                    <el-input v-model="name_" placeholder="请输入昵称(选填)"></el-input>
                    <el-select v-model="value" placeholder="请选择性别(选填)">
                        <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
                        </el-option>
                    </el-select>
                    <el-button type="warning" round v-on:click="register">注册</el-button>
                </div>
            </el-card>
        </div>
        <div id="clock">
            <div id="time">{{ time }}</div>
            <div id="date">{{ date }}</div>
        </div>
        <div id="copyright">Copyright © 2023 h-sen. ALL Rights Reserved</div>
    </div>
</template>
<style scoped>
#background {
    /* background-color: darkseagreen; */
    position: fixed;
    height: 100%;
    width: 100%;
    top: 0;
    left: 0;
    background-image: linear-gradient(90deg,
            cyan,
            purple);
    background-size: 400%;
    animation: myanimation 10s infinite;
}

@keyframes myanimation {
    0% {
        background-position: 0% 50%;
    }

    50% {
        background-position: 100% 50%;
    }

    100% {
        background-position: 50% 0%;
    }
}

#logo {
    color: aliceblue;
    font-size: 25px;
    font-weight: 800;
    text-transform: uppercase;
    position: absolute;
    top: 15%;
    left: 15%;

}

#login {
    position: absolute;
    top: 18%;
    left: 38%;
}

.el-card {
    width: 373px;
    height: 440px;
    background-color: rgba(255, 255, 255, 0.4);
    border-radius: 15px;
}


.el-button+.el-button,
.el-checkbox.is-bordered+.el-checkbox.is-bordered {
    margin-left: 50px;
}

.el-button--warning {
    color: #FFF;
    background-color: #E6A23C;
    border-color: #E6A23C;
    width: 100px;
}

#form {
    line-height: 60px;
    padding-left: 15px;
    padding-right: 15px;
}

#clock {
    color: aliceblue;
    position: absolute;
    top: 75%;
    left: 15%;
    text-align: left;
}

#time {
    font-size: 80px;
}

#date {
    font-size: 35px;
}

#copyright {
    color: aliceblue;
    position: absolute;
    top: 95%;
    left: 40%;
}
</style>
<script>


export default {
    data() {
        return {
            id: "",
            password: "",
            email: "",
            phone: "",
            name_: "",
            time: "",
            date: "",
            options: [{
                value: '男',
                label: '男'
            }, {
                value: '女',
                label: '女'
            }],
        }
    },
    mounted() {
        this.$nextTick(() => {
            setInterval(this.update_clock, 100);
        })
    },

    methods: {
        update_clock: function (e) {
            let d = new Date();
            let year = d.getFullYear();
            if (year < 10) {
                year = "0" + year
            }
            let mon = d.getMonth();
            if (mon < 10) {
                mon = "0" + mon
            }
            let day = d.getDate();
            if (day < 10) {
                day = "0" + day
            }
            let hour = d.getHours();
            if (hour < 10) {
                hour = "0" + hour
            }
            let minute = d.getMinutes();
            if (minute < 10) {
                minute = "0" + minute
            }
            let sec = d.getSeconds();
            if (sec < 10) {
                sec = "0" + sec
            }
            this.time = hour + ":" + minute + ":" + sec;
            this.date = year + "/" + mon + "/" + day;
        },
        // login: function (e) {
        //     this.$axios({
        //         url: "https://db-api.amarea.cn/users",
        //         method: "GET",
        //         data: {
        //             id: this.username,
        //             password: this.password,
        //         },
        //     })
        //         .then((res) => {
        //             console.log(res.data.length);
        //             for (let i = 0; i < res.data.length; i++) {
        //                 if (this.username == res.data[i].id && this.password == res.data[i].password) {
        //                     console.log("ok");
        //                     let url = "home.html?username=" + this.username;
        //                     window.location.href = url
        //                     break;
        //                 }
        //                 else {
        //                     alert("用户名或密码错误！");
        //                     break;
        //                 }
        //             }
        //         })
        //         .catch((err) => {
        //             console.log(err);
        //         });
        // },
        register: function (e) {
            // this.$router.push('/');
            if (this.id == "" || this.password == "" || this.email == "") {
                alert("必填项不能为空！");
            }
            else {
                let m = 0;
                // debugger;
                this.$axios({
                    url: "http://localhost:3000/users",
                    method: "GET",
                    data: {
                        // id: this.username,
                        // password: this.password,
                    },
                })
                    .then((res) => {
                        console.log(res.data.length);
                        for (let i = 0; i < res.data.length; i++) {
                            if (this.id == res.data[i].id) {
                                m = 1;
                                alert("用户名重复！");
                                break;
                            }
                            if (this.password == res.data[i].password) {
                                m = 1;
                                alert("电子邮件地址重复！");
                                break;
                            }
                            
                        }
                    })
                if (m == 0) {
                    this.$axios({
                        url: "http://localhost:3000/users",
                        method: "POST",
                        data: {
                            id: this.id,
                            password: this.password,
                            email: this.email,
                            phone: this.phone,
                            name: this.name_,
                            sex: this.value,
                        },
                    })
                        .then((res) => {
                            alert("注册成功！");
                            this.$router.push('/');
                        })
                }

            }
        }
    }
}
</script>