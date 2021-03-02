<template>
  <div>
    <div class="wraper-popup choice" v-bind:class="getClassPopup_choice">
      <p>Bạn có chắc chắc không?</p>
      <button class="button confirm" @click="yes">Yes</button>
      <button class="button exit" @click="no">No</button>
    </div>
    <div class="wraper-popup" v-bind:class="getClassPopup">
      <div class="rule">
        <ul>
          <li class="notice">Luật chơi</li>
          <li>1. Nhấn vào ROLL DICE để xoay xúc xắc</li>
          <li>2. Điểm chơi là số điểm trên xúc xắc sẽ được cộng lại</li>
          <li>3. Người đạt đến điểm tối đa trước là người chiến thắng</li>
          <li class="notice">Đánh Cược</li>
          <li>1. Đánh cược bằng 1/3 số điểm để chiến thắng</li>
          <li>2. Điểm số sẽ được nhân 3</li>
        </ul>
        <ul>
          <li class="notice">Lưu ý</li>
          <li>1. Nhấn vào Hold sẽ bảo lưu số điểm và mất lượt</li>
          <li>
            2. Người chơi quay vào xúc xắc có điểm số là 1 sẽ bị mất lượt, đồng
            thời sẽ mất luôn điểm ở lượt chơi đó
          </li>
          <li>3. Điểm đánh cược là điểm thực</li>
          <li>4. Khi đánh cược sẽ bị mất số điểm tương ứng</li>
        </ul>
        <button class="button confirm" @click="confirm">Đã hiểu</button>
        <button class="button exit" @click="exit">Hủy</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "popup-rule",
  props: {
    isOpenPopup: { type: Boolean, default: () => false },
    isOpenPopup_choice: { type: Boolean, default: () => false },
  },
  data() {
    return {};
  },
  methods: {
    confirm() {
      // console.log("popuprule");
      this.$emit("handleConfirm");
    },
    exit() {
      // console.log("exit-rule");
      this.$emit("handleExit");
    },
    yes() {
      this.$emit("handleYes");
    },
    no() {
      this.$emit("handleNo");
    },
  },
  computed: {
    getClassPopup: function () {
      return {
        "open-popup": this.isOpenPopup,
      };
    },
    getClassPopup_choice: function () {
      return {
        "open-popup-choice": this.isOpenPopup_choice,
      };
    },
  },
};
</script>

<style>
ul {
  margin-bottom: 0;
}
ul li {
  list-style: none;
  padding: 2px 0;
  text-align: flex-end;
}
ul li.notice {
  font-size: 18px;
  color: #333;
  font-weight: 700;
}
.wraper-popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.3);
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease-in;
}
.choice {
  background: rgb(212, 199, 199);
  position: absolute;
  width: 230px;
  height: 120px;
  overflow: auto;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(1.2);
  padding: 20px;
  font-family: Arial, Helvetica, sans-serif;
  transition: all 0.3s ease-in;
}
.choice.open-popup-choice {
  opacity: 1;
  visibility: visible;
}
.wraper-popup.open-popup {
  opacity: 1;
  visibility: visible;
}

.open-popup .rule {
  transform: translate(-50%, -50%) scale(1);
}
.rule {
  background: white;
  position: absolute;
  width: 350px;
  height: 500px;
  overflow: auto;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(1.2);
  padding: 20px;
  font-family: Arial, Helvetica, sans-serif;
  transition: all 0.3s ease-in;
}
.rule h3 {
  margin-top: 10px;
}
.button {
  width: 80px;
  height: 40px;
  transition: all 0.3s;
  border-radius: 3px;
}
.confirm {
  background: #333;
  border: 1px solid #333;
  color: white;
  opacity: 0.8;
}
.exit {
  background: rgb(224, 33, 33);
  margin-left: 25px;
  border: 1px solid rgb(235, 4, 4);
  opacity: 0.9;
}
.rule .confirm:hover,
.exit:hover {
  opacity: 1;
}
</style>