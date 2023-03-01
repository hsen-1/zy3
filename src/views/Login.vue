<template>
    <div id="background">
        <div id="logo">my notebook</div>
        <div id="login">
            <el-card class="box-card">
                <div id="form">
                    <el-input v-model="username" placeholder="请输入用户名"></el-input>
                    <el-input placeholder="请输入密码" v-model="password" show-password></el-input>
                    <el-button type="primary" round v-on:click="login">登录</el-button>
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
    top: 30%;
    left: 38%;
}

.el-card {
    width: 373px;
    height: 210px;
    background-color: rgba(255, 255, 255, 0.4);
    border-radius: 15px;
}

.el-button--primary {
    color: #FFF;
    background-color: #409EFF;
    border-color: #409EFF;
    width: 80px;
}

.el-button+.el-button, .el-checkbox.is-bordered+.el-checkbox.is-bordered {
    margin-left: 50px;
}

.el-button--warning {
    color: #FFF;
    background-color: #E6A23C;
    border-color: #E6A23C;
    width: 80px;
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
    left: 38%;
}
</style>
<script>


export default {
    data() {
        return {
            username: "",
            password: "",
            time: "",
            date: "",
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
        login: function (e) {
            this.$axios({
                url: "https://db-api.amarea.cn/users",
                // url: "http://localhost:3000/users",
                method: "GET",
                data: {
                    // id: this.username,
                    // password: this.password,
                },
            })
                .then((res) => {
                    // debugger;
                    console.log(res.data.length);
                    for (let i = 0; i < res.data.length; i++) {
                        if (this.username == res.data[i].id && this.password == res.data[i].password) {
                            // console.log("ok");
                            alert("登录成功！");
                            let url = "home.html?username=" + this.username;
                            window.location.href = url
                            break;
                        }
                        if(i==res.data.length-1) {
                            alert("用户名或密码错误！");
                        }
                    }
                })
                .catch((err) => {
                    console.log(err);
                });
        },
        register: function (e) {
            this.$router.push('/register');
        }
    }
}
</script>
