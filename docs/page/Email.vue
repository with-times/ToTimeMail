<template>
    <div id="Email-BigBox">
        <div class="Principal-left">
            <div class="logo-box">
                <img class="time-logo" src="../static/ico/logo-longh.png" alt="与时同行">
            </div>
            <p class="text-p">从前的日色变得慢</p>
            <p class="text-p">车，马，邮件都慢</p>
            <p class="text-p">一生只够爱一个人</p>
            <p class="text-p">而今天的我们日行千里</p>
            <p class="text-p">却总是边走边忘</p>
            <p class="text-p">曾经稚嫩却充满勇气的自己</p>
            <p class="text-p">倔强又柔软的自己</p>
            <p class="text-p">你还记得多少?</p>
            <p class="text-p">用信件记录下此刻的心情与期盼</p>
            <p class="text-p">并寄给未来的自己或那个TA</p>
            <p class="text-p">一起见证时光交错，与未来相遇</p>

            <div class="statistics-box">
                <div class="statistics-text">
                    <img class="statistics-img" src="../static/ico/youxiang.png" alt="ico">
                    <span class="statistics-desc">待发送</span>
                    <span class="statistics-number">{{ data.waitSend }}</span>
                    <span class="statistics-unit">封</span>
                </div>
                <div class="statistics-text">
                    <img class="statistics-img" src="../static/ico/dianziyoujian.png" alt="ico">
                    <span class="statistics-desc">已发送</span>
                    <span class="statistics-number">{{ data.send }}</span>
                    <span class="statistics-unit">封</span>
                </div>
                <div class="statistics-text">
                    <img class="statistics-img" src="../static/ico/shijian.png" alt="ico">
                    <span class="statistics-desc">已运营</span>
                    <span class="statistics-number">{{ data.operation }}</span>
                    <span class="statistics-unit">天</span>
                </div>
            </div>

            <div class="gzh-box">
                <p class="gzh-desc">微信公众号</p>
                <img class="gzh-img" src="../static/img/gzh.jpg" alt="公众号">
                <p class="gzh">扫码关注【与时同行】</p>
            </div>
        </div>
        <div class="Principal-right">
            <h2 class="right-title">云寄 • 时光邮局</h2>
            <p class="title-p">见证时光的交错，与未来相遇</p>

            <div class="from-email">
                <div class="email-info">
                    <span class="span-desc">邮件标题：</span>
                    <el-input class="title-el-input" v-model="data.form.title" placeholder="请输入标题"
                        style="width: 100%" />
                </div>

                <div class="email-info">
                    <span class="span-desc">邮件类型：</span>
                    <el-select v-model="data.form.type" class="m-2" placeholder="选择类型">
                        <el-option v-for="item in emailOptions" :key="item.value" :label="item.label"
                            :value="item.value" />
                    </el-select>
                </div>

                <div class="email-info">
                    <span class="span-desc">收件信息：</span>
                    <el-input class="title-el-input" v-model="data.form.addressee" placeholder="姓名" style="width: 30%"
                        :disabled="data.isDisabled" />
                    <el-input class="title-el-input" v-model="data.form.phone" placeholder="手机号"
                        style="width: 60%;margin-left: 5%;" :disabled="data.isDisabled" />
                </div>

                <div class="email-info">
                    <span class="span-desc"></span>
                    <el-input class="title-el-input" v-model="data.addressSpecific" placeholder="省、市、区、街道"
                        style="width: 60%" :disabled="data.isDisabled" />
                </div>

                <div class="email-info">
                    <span class="span-desc"></span>
                    <el-input resize="none" maxlength="25" :rows="2" type="textarea" class="title-el-input"
                        v-model="data.addressDetailed" placeholder="小区楼栋/村镇名称" style="width: 80%;"
                        :disabled="data.isDisabled" />
                </div>

                <div class="email-info email-vditor">
                    <span class="span-desc">信件内容：</span>
                    <div class="title-el-input" id="vditor">
                    </div>
                </div>

                <div class="email-info">
                    <span class="span-desc">约定时间：</span>
                    <div class="demo-date-picker">
                        <el-date-picker v-model="data.form.dataTime" type="date" placeholder="选择未来"
                            format="YYYY-MM-DD" value-format="YYYY-MM-DD">
                            <template #default="cell">
                                <div class="cell" :class="{ current: cell.isCurrent }">
                                    <span class="text">{{ cell.text }}</span>
                                    <span v-if="isHoliday(cell)" class="holiday" />
                                </div>
                            </template>
                        </el-date-picker>
                    </div>
                </div>

                <div class="email-info">
                    <span class="span-desc">写给他人：</span>
                    <el-switch style="--el-switch-on-color: #13ce66; --el-switch-off-color: #bebbbb"
                        v-model="data.form.isTa" inline-prompt active-text="是" inactive-text="否" />
                </div>

                <div class="email-info">
                    <span class="span-desc">收信邮箱：</span>

                    <el-input class="title-el-input" v-model="data.form.toEmail[1]" placeholder="收信邮箱"
                        style="width: 50%;" />
                </div>

                <div class="email-info">
                    <span class="span-desc">同意条款：</span>
                    <el-switch style="--el-switch-on-color: #13ce66; --el-switch-off-color: #bebbbb"
                        v-model="data.isAgreeTerms" inline-prompt active-text="是" inactive-text="否" />
                    <span class="treaty-text">查看<a href="/PrivacyPolicy.html">《隐私条款》</a></span>
                </div>

                <div class="email-but-action">
                    <el-button color="#626aef" size="large" @click="deliveryEmail">投递信件</el-button>
                    <el-button color="#F4F4F5" size="large" @click="discardEmail">舍弃信件</el-button>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts" setup>
import http from '../utils/http/http'
import '../static/css/email.css'
import { reactive, watch,onMounted } from "vue";
import { ElInput, ElSelect, ElOption, ElDatePicker, ElSwitch, ElButton, ElNotification, ElLoading, ElMessage, ElMessageBox } from 'element-plus';
import 'element-plus/dist/index.css';
import Vditor from 'vditor'
import 'vditor/dist/index.css'

const emailOptions = [
    { value: 0, label: '电子邮件' },
    { value: 1, label: '线下邮寄' },
]

onMounted(() => {
    new Vditor("vditor", {
        height: 360,
        toolbar: [
            "emoji",
            "headings",
            "bold",
            "italic",
            "strike",
            "link",
            "list",
            "ordered-list",
            "check",
            "outdent",
            "indent",
            "quote",
            "line",
            "insert-before",
            "insert-after",
            "table",
            "fullscreen",
            {
                name: 'sponsor',
                tipPosition: 's',
                tip: '赞助',
                className: 'right',
                icon: '<svg t="1589994565028" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2808" width="32" height="32"><path d="M506.6 423.6m-29.8 0a29.8 29.8 0 1 0 59.6 0 29.8 29.8 0 1 0-59.6 0Z" fill="#0F0F0F" p-id="2809"></path><path d="M717.8 114.5c-83.5 0-158.4 65.4-211.2 122-52.7-56.6-127.7-122-211.2-122-159.5 0-273.9 129.3-273.9 288.9C21.5 562.9 429.3 913 506.6 913s485.1-350.1 485.1-509.7c0.1-159.5-114.4-288.8-273.9-288.8z" fill="#FAFCFB" p-id="2810"></path><path d="M506.6 926c-22 0-61-20.1-116-59.6-51.5-37-109.9-86.4-164.6-139-65.4-63-217.5-220.6-217.5-324 0-81.4 28.6-157.1 80.6-213.1 53.2-57.2 126.4-88.8 206.3-88.8 40 0 81.8 14.1 124.2 41.9 28.1 18.4 56.6 42.8 86.9 74.2 30.3-31.5 58.9-55.8 86.9-74.2 42.5-27.8 84.3-41.9 124.2-41.9 79.9 0 153.2 31.5 206.3 88.8 52 56 80.6 131.7 80.6 213.1 0 103.4-152.1 261-217.5 324-54.6 52.6-113.1 102-164.6 139-54.8 39.5-93.8 59.6-115.8 59.6zM295.4 127.5c-72.6 0-139.1 28.6-187.3 80.4-47.5 51.2-73.7 120.6-73.7 195.4 0 64.8 78.3 178.9 209.6 305.3 53.8 51.8 111.2 100.3 161.7 136.6 56.1 40.4 88.9 54.8 100.9 54.8s44.7-14.4 100.9-54.8c50.5-36.3 108-84.9 161.7-136.6 131.2-126.4 209.6-240.5 209.6-305.3 0-74.9-26.2-144.2-73.7-195.4-48.2-51.9-114.7-80.4-187.3-80.4-61.8 0-127.8 38.5-201.7 117.9-2.5 2.6-5.9 4.1-9.5 4.1s-7.1-1.5-9.5-4.1C423.2 166 357.2 127.5 295.4 127.5z" fill="#141414" p-id="2811"></path><path d="M353.9 415.6m-33.8 0a33.8 33.8 0 1 0 67.6 0 33.8 33.8 0 1 0-67.6 0Z" fill="#0F0F0F" p-id="2812"></path><path d="M659.3 415.6m-33.8 0a33.8 33.8 0 1 0 67.6 0 33.8 33.8 0 1 0-67.6 0Z" fill="#0F0F0F" p-id="2813"></path><path d="M411.6 538.5c0 52.3 42.8 95 95 95 52.3 0 95-42.8 95-95v-31.7h-190v31.7z" fill="#5B5143" p-id="2814"></path><path d="M506.6 646.5c-59.6 0-108-48.5-108-108v-31.7c0-7.2 5.8-13 13-13h190.1c7.2 0 13 5.8 13 13v31.7c0 59.5-48.5 108-108.1 108z m-82-126.7v18.7c0 45.2 36.8 82 82 82s82-36.8 82-82v-18.7h-164z" fill="#141414" p-id="2815"></path><path d="M450.4 578.9a54.7 27.5 0 1 0 109.4 0 54.7 27.5 0 1 0-109.4 0Z" fill="#EA64F9" p-id="2816"></path><path d="M256 502.7a32.1 27.5 0 1 0 64.2 0 32.1 27.5 0 1 0-64.2 0Z" fill="#EFAFF9" p-id="2817"></path><path d="M703.3 502.7a32.1 27.5 0 1 0 64.2 0 32.1 27.5 0 1 0-64.2 0Z" fill="#EFAFF9" p-id="2818"></path></svg>',
                click() { alert('联系QQ：747945307 / 1345561377') },
            },
            {
                name: "more",
                toolbar: [
                    "both",
                    "code-theme",
                    "content-theme",
                    "export",
                ],
            },
        ],
    });
})

const data = reactive({
    isDisabled: true,
    waitSend: 52,
    send: 15,
    operation: 15,
    isAgreeTerms: false,
    addressDetailed: '',
    addressSpecific: '',
    form: {
        title: '',
        // 类型
        type: 0,
        //获取现在时间
        dataTime: new Date(),
        isTa: false,
        toEmail: [
            "747945307@qq.com",
            ''
        ],
        addressee: '',
        phone: '',
        content: '',
        isHtml: true
    }
})

watch(() => data.form.type, (val) => {
    if (val == 1) {
        data.isDisabled = false
    } else {
        data.isDisabled = true
    }
})
const holidays = [
    '2021-10-01',
    '2021-10-02',
    '2021-10-03',
    '2021-10-04',
    '2021-10-05',
    '2021-10-06',
    '2021-10-07',
]

const isHoliday = ({ dayjs }) => {
    return holidays.includes(dayjs.format('YYYY-MM-DD'))
}

// 投递信件
const deliveryEmail = () => {
    if (data.form.title == '') {
        ElMessage({
            message: '请输入标题',
            type: 'error'
        });
        return
    }
    if (data.form.content == '') {
        ElMessage({
            message: '请输入内容',
            type: 'error'
        });
        return
    }
    if (data.form.toEmail[1] == '') {
        ElMessage({
            message: '请输入收信邮箱',
            type: 'error'
        });
        return
    }
    if (data.isAgreeTerms == false) {
        ElMessage({
            message: '请同意条款',
            type: 'error'
        });
        return
    }

    if (data.form.type == 1) {
        if (data.form.addressee == '') {
            ElMessage({
                message: '请输入收件人',
                type: 'error'
            });
            return
        }
        if (data.form.phone == '') {
            ElMessage({
                message: '请输入电话',
                type: 'error'
            });
            return
        }
        if (data.addressDetailed == '') {
            ElMessage({
                message: '请输入详细地址',
                type: 'error'
            });
            return
        }
        if (data.addressSpecific == '') {
            ElMessage({
                message: '请输入具体地址',
                type: 'error'
            });
            return
        }

        //判断数据格式
        if (!(/^1[3456789]\d{9}$/.test(data.form.phone))) {
            ElMessage({
                message: '请输入正确的手机号码',
                type: 'error'
            });
            return
        }

        if (!(/^[\u4e00-\u9fa5]{2,4}$/.test(data.form.addressee))) {
            ElMessage({
                message: '请输入正确的姓名',
                type: 'error'
            });
            return
        }
    }

    if (!(/^(\w-*\.*)+@(\w-?)+(\.\w{2,})+$/.test(data.form.toEmail[1]))) {
            ElMessage({
                message: '请输入正确的邮箱',
                type: 'error'
            });
            return
    }

    http.post('/sendEmail/toTimeMsg', data.form).then(res => {
        const loading = ElLoading.service({
            lock: true,
            text: '正在投递信件...',
            background: 'rgba(255, 255, 255, 0.7)'
        });
        if (res.data.code == 200) {
            setTimeout(() => {
                loading.close();
            }, 2000);
            setTimeout(() => {
                ElNotification({
                    title: '成功',
                    message: '投递成功',
                    type: 'success'
                })
            }, 2000);
        } else {
            setTimeout(() => {
                loading.close();
            }, 2000)

            setTimeout(() =>
                ElNotification({
                    title: '失败',
                    message: res.data.msg,
                    type: 'error',
                    duration: 3000
                })
                , 2000)
        }
    })
}

// 舍弃信件
const discardEmail = () => {
    //刷新页面 TODO: 优化，使用弹出提示
    window.location.reload()
}
</script>