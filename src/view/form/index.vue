<template>
  <div>
    <van-form @submit="onSubmit" @failed="onFailed">
      <van-field
        v-model="username"
        name="username"
        label="用户名"
        placeholder="用户名"
        :rules="[{ required: true, message: '请填写用户名' }]"
      />
      <van-field
        v-model="password"
        type="password"
        name="password"
        label="密码"
        placeholder="密码"
        :rules="[{ required: true, message: '请填写密码' }]"
      />
      <!-- 通过 validator 进行函数校验 -->
      <van-field
        v-model="value1"
        name="validator"
        placeholder="函数校验"
        :rules="[{ validator, message: '请输入大于10的数字' }]"
      />
      <div style="margin: 16px">
        <van-button round block type="info" native-type="submit">
          提交
        </van-button>
      </div>
    </van-form>
    <van-form @submit="onFormSubmit">
      <van-field name="switch" label="开关">
        <template #input>
          <van-switch v-model="switchChecked" size="20" />
        </template>
      </van-field>
      <van-field name="checkbox" label="复选框">
        <template #input>
          <van-checkbox v-model="checkbox" shape="square" />
        </template>
      </van-field>
      <van-field name="checkboxGroup" label="复选框组">
        <template #input>
          <van-checkbox-group v-model="checkboxGroup" direction="horizontal">
            <van-checkbox name="1" shape="square">复选框 1</van-checkbox>
            <van-checkbox name="2" shape="square">复选框 2</van-checkbox>
          </van-checkbox-group>
        </template>
      </van-field>
      <van-field name="radio" label="单选框">
        <template #input>
          <van-radio-group v-model="radio" direction="horizontal">
            <van-radio name="1">单选框 1</van-radio>
            <van-radio name="2">单选框 2</van-radio>
          </van-radio-group>
        </template>
      </van-field>
      <van-field name="stepper" label="步进器">
        <template #input>
          <van-stepper v-model="stepper" />
        </template>
      </van-field>
      <van-field name="rate" label="评分">
        <template #input>
          <van-rate v-model="rate" />
        </template>
      </van-field>
      <van-field name="slider" label="滑块">
        <template #input>
          <van-slider v-model="slider" />
        </template>
      </van-field>
      <van-field name="uploader" label="文件上传">
        <template #input>
          <van-uploader v-model="uploader" :after-read="afterRead" />
        </template>
      </van-field>
      <van-field
        readonly
        clickable
        name="picker"
        :value="value"
        label="选择器"
        placeholder="点击选择城市"
        @click="showPicker = true"
      />
      <van-popup v-model="showPicker" position="bottom">
        <van-picker
          show-toolbar
          :columns="columns"
          @confirm="onConfirm"
          @cancel="showPicker = false"
        />
      </van-popup>
      <van-field
        readonly
        clickable
        name="datetimePicker"
        :value="time"
        label="时间选择"
        placeholder="点击选择时间"
        @click="showTimePicker = true"
      />
      <van-popup v-model="showTimePicker" position="bottom">
        <van-datetime-picker
          type="datetime"
          @confirm="onTimeConfirm"
          @cancel="showTimePicker = false"
        />
      </van-popup>
      <van-field
        readonly
        clickable
        name="area"
        :value="area"
        label="地区选择"
        placeholder="点击选择省市区"
        @click="showArea = true"
      />
      <van-popup v-model="showArea" position="bottom">
        <van-area
          :area-list="areaList"
          @confirm="onAreaConfirm"
          @cancel="showArea = false"
        />
      </van-popup>
      <div style="margin: 16px">
        <van-button round block type="info" native-type="submit">
          提交
        </van-button>
      </div>
    </van-form>
  </div>
</template>
<script>
import { Form, Field, Button, Toast, Switch, Checkbox, CheckboxGroup, Radio, RadioGroup, Stepper, Rate, Slider, Popup, Picker, Uploader, DatetimePicker, Area } from "vant";
import moment from "moment";
import areas from '../../utils/area';
export default {
  components: {
    [Form.name]: Form,
    [Field.name]: Field,
    [Button.name]: Button,
    [Toast.name]: Toast,
    [Switch.name]: Switch,
    [Checkbox.name]: Checkbox,
    [CheckboxGroup.name]: CheckboxGroup,
    [Radio.name]: Radio,
    [RadioGroup.name]: RadioGroup,
    [Stepper.name]: Stepper,
    [Rate.name]: Rate,
    [Slider.name]: Slider,
    [Popup.name]: Popup,
    [Picker.name]: Picker,
    [Uploader.name]: Uploader,
    [DatetimePicker.name]: DatetimePicker,
    [Area.name]: Area,
  },
  data() {
    return {
      username: '',
      password: '',
      value1: '',
      switchChecked: false,
      checkbox: false,
      checkboxGroup: [],
      radio: '1',
      stepper: 1,
      rate: 3,
      slider: 50,
      value: '',
      columns: ['杭州', '宁波', '温州', '嘉兴', '湖州'],
      showPicker: false,
      uploader: [{ url: 'https://img.yzcdn.cn/vant/leaf.jpg' }],
      time: '',
      showTimePicker: false,
      area: '',
      showArea: false,
      areaList: areas, // 数据格式见 Area 组件文档
    }
  },
  methods: {
    onSubmit(values) {
      console.log('values :>> ', values);
    },
    // 校验函数返回 true 表示校验通过，false 表示不通过
    validator(val) {
      return val > 10;
    },
    onFailed(errorInfo) {
      console.log('failed', errorInfo);
    },
    afterRead(file) {
      // 此时可以自行将文件上传至服务器
      console.log(file);
    },
    onConfirm(value) {
      this.value = value;
      this.showPicker = false;
    },
    onTimeConfirm(value) {
      this.time = moment(value).format('YYYY-MM-DD HH:mm:ss');
      this.showTimePicker = false;
    },
    onAreaConfirm(values) {
      this.area = values
        .filter((item) => !!item)
        .map((item) => item.name)
        .join('/');
      this.showArea = false;
    },
    onFormSubmit(values) {
      console.log('onFormSubmit :>> ', values);
    }
  }
}
</script>