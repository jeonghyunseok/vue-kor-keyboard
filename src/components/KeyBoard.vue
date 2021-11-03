<template>
  <div>
    <textarea :value="resultTxt"></textarea>
    <div class="keypad">
      <div
        v-for="(keyLine, index) in keyList"
        :key="`keyLine_${index}`"
        class="key__row"
      >
        <div
          v-for="(key, index2) in keyLine"
          :key="`key_${index}_${index2}`"
          :class="[
            { key: true, key__disabled: key == '한/영' && boardType == 'num' }
          ]"
          @click="keyClick(key)"
          :data-name="key"
        >
          <span>{{ key }} </span>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "VueKorKeyboard",
  data() {
    return {
      KeyData: [
        ["q", "w", "e", "r", "t", "y", "u", "i", "o", "p"],
        ["a", "s", "d", "f", "g", "h", "j", "k", "l"],
        ["Shift", "z", "x", "c", "v", "b", "n", "m", "Delete"],
        ["123", "한/영", "Space", "Enter"],

        ["Q", "W", "E", "R", "T", "Y", "U", "I", "O", "P"],
        ["A", "S", "D", "F", "G", "H", "J", "K", "L"],
        ["Shift", "Z", "X", "C", "V", "B", "N", "M", "Delete"],
        ["123", "한/영", "Space", "Enter"],

        ["ㅂ", "ㅈ", "ㄷ", "ㄱ", "ㅅ", "ㅛ", "ㅕ", "ㅑ", "ㅐ", "ㅔ"],
        ["ㅁ", "ㄴ", "ㅇ", "ㄹ", "ㅎ", "ㅗ", "ㅓ", "ㅏ", "ㅣ"],
        ["Shift", "ㅋ", "ㅌ", "ㅊ", "ㅍ", "ㅠ", "ㅜ", "ㅡ", "Delete"],
        ["123", "한/영", "간격", "Enter"],

        ["ㅃ", "ㅉ", "ㄸ", "ㄲ", "ㅆ", "ㅛ", "ㅕ", "ㅑ", "ㅒ", "ㅖ"],

        ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0"],
        ["+", "×", "÷", "=", "⁓", "!", "@", "#", "%"],
        ["Shift", "_", `'`, '"', ":", ";", "/", "?", "Delete"],
        ["123", "Space", "Enter"]
      ],
      language: "Eng",
      //한글 키패드 키
      chosungKey: [
        "ㄱ",
        "ㄲ",
        "ㄴ",
        "ㄷ",
        "ㄸ",
        "ㄹ",
        "ㅁ",
        "ㅂ",
        "ㅃ",
        "ㅅ",
        "ㅆ",
        "ㅇ",
        "ㅈ",
        "ㅉ",
        "ㅊ",
        "ㅋ",
        "ㅌ",
        "ㅍ",
        "ㅎ"
      ],
      joongsungKey: [
        "ㅏ",
        "ㅐ",
        "ㅑ",
        "ㅒ",
        "ㅓ",
        "ㅔ",
        "ㅕ",
        "ㅖ",
        "ㅗ",
        "ㅘ",
        "ㅙ",
        "ㅚ",
        "ㅛ",
        "ㅜ",
        "ㅝ",
        "ㅞ",
        "ㅟ",
        "ㅠ",
        "ㅡ",
        "ㅢ",
        "ㅣ"
      ],
      jongsungIdx: [
        "",
        "ㄱ",
        "ㄲ",
        "ㄳ",
        "ㄴ",
        "ㄵ",
        "ㄶ",
        "ㄷ",
        "ㄹ",
        "ㄺ",
        "ㄻ",
        "ㄼ",
        "ㄽ",
        "ㄾ",
        "ㄿ",
        "ㅀ",
        "ㅁ",
        "ㅂ",
        "ㅄ",
        "ㅅ",
        "ㅆ",
        "ㅇ",
        "ㅈ",
        "ㅊ",
        "ㅋ",
        "ㅌ",
        "ㅍ",
        "ㅎ"
      ],
      jcomboKey: [
        "ㄳ",
        "ㄵ",
        "ㄶ",
        "ㄺ",
        "ㄻ",
        "ㄼ",
        "ㄽ",
        "ㄾ",
        "ㄿ",
        "ㅀ",
        "ㅄ"
      ],
      jcomboTxt: [
        "ㄱㅅ",
        "ㄴㅈ",
        "ㄴㅎ",
        "ㄹㄱ",
        "ㄹㅁ",
        "ㄹㅂ",
        "ㄹㅅ",
        "ㄹㅌ",
        "ㄹㅍ",
        "ㄹㅎ",
        "ㅂㅅ"
      ],
      mComboKey: ["ㅘ", "ㅙ", "ㅚ", "ㅝ", "ㅞ", "ㅟ", "ㅢ"],
      mComboTxt: ["ㅗㅏ", "ㅗㅐ", "ㅗㅣ", "ㅜㅓ", "ㅜㅔ", "ㅜㅣ", "ㅡㅣ"],
      shiftkey: false,
      boardType: "text",
      visibleKeypad: "",
      resultTxt: ""
    };
  },

  computed: {
    keyList() {
      let result = [];
      if (this.boardType == "num") {
        result = this.KeyData.slice(13, 18);
      } else if (this.language == "Eng") {
        result = this.shiftkey
          ? this.KeyData.slice(4, 8)
          : this.KeyData.slice(0, 4);
      } else if (this.language == "Kor") {
        result = this.shiftkey
          ? this.KeyData.slice(12, 13).concat(this.KeyData.slice(9, 12))
          : this.KeyData.slice(8, 12);
      }
      return result;
    }
  },
  methods: {
    changeResultTxt(txt) {
      this.resultTxt = this.resultTxt + txt;
    },
    transLanguage() {
      this.language = this.language == "Kor" ? "Eng" : "Kor";
      this.shiftkey = false;
      this.boardType = "text";
    },
    transKeyboard(v) {
      this.boardType = v;
      if (v == "num") {
        this.keyList[3][0] = this.language == "Kor" ? "한글" : "ABC";
      }
    },
    //자음모음 판별
    chkJM(char_uni) {
      if (char_uni >= 12593 && char_uni <= 12622) {
        return "J";
      } else if (char_uni >= 12623 && char_uni <= 12643) {
        return "M";
      } else {
        return "";
      }
    },
    keyValueChk(text, lasttext) {
      // 마지막 문자 유니코드
      let lasttext_uni = lasttext.charCodeAt(0);
      // 1. 마지막 글자 초/중/종성 구하기
      let chosung, joongsung, jongsung;
      let cho_idx, jong_idx, joong_idx;
      // 하나의 자음만 존재
      if (this.chkJM(lasttext_uni) == "J") {
        chosung = lasttext;
        joongsung = "";
        jongsung = "";
        // 하나의 모음만 존재
      } else if (this.chkJM(lasttext_uni) == "M") {
        chosung = "";
        joongsung = lasttext;
        jongsung = "";
      } else {
        // 마지막 문자 하나의 자음,모음 모두 아님
        // 마지막 문자에서 AC00 제거
        let lastchar_uni_cal = lasttext_uni - 44032;
        // 마지막 문자 초/중/종성 인덱스 구하기
        // 한글음절위치 = ((초성index * 21) + 중성index) * 28 + 종성index
        cho_idx = Math.floor(Math.floor(lastchar_uni_cal / 28) / 21);
        joong_idx = Math.floor(lastchar_uni_cal / 28) % 21;
        jong_idx = lastchar_uni_cal % 28;
        //마지막 문자 초/중/종성 구하기
        chosung = this.chosungKey[cho_idx];
        joongsung = this.joongsungKey[joong_idx];
        jongsung = this.jongsungIdx[jong_idx];
      }
      // 2. 방금 친 글자 모음/자음 구별
      let key_jm = this.chkJM(text.charCodeAt(0));
      let str_uni;
      let key_idx;
      // 3. 글자 재조합
      // 자음만 있는 경우 + 자음
      if (lasttext == chosung && key_jm == "J") {
        let newja = chosung + text;
        // 앞자음 + 뒷자음 = 콤보
        if (this.jcomboTxt.indexOf(newja) != -1) {
          newja = this.jcomboKey[this.jcomboTxt.indexOf(newja)];
          // 기존글자 1개 지우고 글자추가
          this.deleteKeyValue();
          return newja;
          // 앞자음+ 뒷자음 != 콤보
        } else {
          return text;
        }
      } else if (lasttext == chosung && key_jm == "M") {
        // 자음만 있는경우 + 모음
        key_idx = this.joongsungKey.indexOf(text);
        // 자음이 콤보인경우
        if (this.jcomboKey.indexOf(lasttext) != -1) {
          let newja = this.jcomboTxt[this.jcomboKey.indexOf(lasttext)];
          let newja1 = newja.substring(0, 1);
          let newja2 = newja.substring(1, newja.length);
          let newja2_idx = this.chosungKey.indexOf(newja2);
          let str_uni = (newja2_idx * 21 + key_idx) * 28 + 44032;
          let str = String.fromCharCode(str_uni);
          let hangeul = newja1 + str;
          if (this.jcomboKey.indexOf(lasttext) < 5) {
            // 기존글자 1개 지우고 글자추가
            this.deleteKeyValue();
            return hangeul;
          } else {
            return text;
          }
        } else {
          // 앞 자음이 콤보가 아닌 경우 ex) ㄱ + ㅏ  가
          cho_idx = this.chosungKey.indexOf(chosung);
          let str_uni = (cho_idx * 21 + key_idx) * 28 + 44032;
          let hangeul = String.fromCharCode(str_uni);
          // 기존글자 1개 지우고 글자추가
          this.deleteKeyValue();
          return hangeul;
        }
        // 모음만 있는 경우 + 자음
      } else if (lasttext == joongsung && key_jm == "J") {
        return text;
        // 모음만 있는 경우 + 모음
      } else if (lasttext == joongsung && key_jm == "M") {
        let newchar = lasttext + text;
        // 모음이 콤보인 경우
        if (this.mComboTxt.indexOf(newchar) != -1) {
          newchar = this.mComboKey[this.mComboTxt.indexOf(newchar)];
          this.deleteKeyValue();
          return newchar;
          //모음이 콤보 아닌 경우
        } else {
          return text;
        }
        //3-1 종성이 있는경우 + 자음
      } else if (jongsung != "" && key_jm == "J") {
        let newjong_idx = this.jcomboTxt.indexOf(jongsung + text);
        // 종성 + 입력한 자음이 콤보 ex) 갈+ㄱ = 갉
        if (newjong_idx != -1) {
          let newjong = this.jcomboKey[newjong_idx];
          newjong_idx = this.jongsungIdx.indexOf(newjong);
          str_uni = (cho_idx * 21 + joong_idx) * 28 + newjong_idx + 44032;
          let hangeul = String.fromCharCode(str_uni);
          this.deleteKeyValue();
          return hangeul;
          // 종성 + 입력한 자음이 콤보에 없는 경우  ex) 각+ㅇ =각ㅇ
        } else {
          return text;
        }
        // 3-2종성이 있는 경우 + 모음  ex) 강 + ㅏ  = 가아   값 + ㅏ + 갑 사
      } else if (jongsung != "" && key_jm == "M") {
        let newjong_idx = this.jcomboKey.indexOf(jongsung);
        key_idx = this.joongsungKey.indexOf(text);
        // 종성 콤보
        if (newjong_idx != -1) {
          let newjong = this.jcomboTxt[newjong_idx];
          let newjong1 = newjong.substring(0, 1);
          let newjong2 = newjong.substring(1, newjong.length);
          let newjong1_idx = this.jongsungIdx.indexOf(newjong1);
          let newjong2_idx = this.chosungKey.indexOf(newjong2);
          let str_uni1 = (cho_idx * 21 + joong_idx) * 28 + newjong1_idx + 44032;
          let str_uni2 = (newjong2_idx * 21 + key_idx) * 28 + 44032;
          let hangeul =
            String.fromCharCode(str_uni1) + String.fromCharCode(str_uni2);
          this.deleteKeyValue();
          return hangeul;
          // 종성이 콤보 아닌경우 ex 강+ㅏ  가 아
        } else {
          let newcho_idx = this.chosungKey.indexOf(jongsung);
          let str_uni1 = (cho_idx * 21 + joong_idx) * 28 + 44032;
          let str_uni2 = (newcho_idx * 21 + key_idx) * 28 + 44032;
          let hangeul =
            String.fromCharCode(str_uni1) + String.fromCharCode(str_uni2);
          this.deleteKeyValue();
          return hangeul;
        }
        //3-3 이전글자 종성 없는 경우 + 자음 ex) 가 + ㅇ = 강, 가 + ㅉ = 가ㅉ
      } else if (jongsung == "" && key_jm == "J") {
        key_idx = this.jongsungIdx.indexOf(text);
        //입력한 자음이 받침이 될 수 있는 경우
        if (key_idx != -1) {
          str_uni = (cho_idx * 21 + joong_idx) * 28 + key_idx + 44032;
          let hangeul = String.fromCharCode(str_uni);
          this.deleteKeyValue();
          return hangeul;
          // 자음이 받침이 될 수 없는 경우
        } else {
          return text;
        }
        //3-4 종성이 없는 경우 + 모음
      } else if (jongsung == "" && key_jm == "M") {
        let mcom = joongsung + text; // ㅜㅣ
        let mcom_idx = this.mComboTxt.indexOf(mcom);
        // 모음(중성) + 친 글자(모음) = 콤보인경우 ex. 구 + ㅣ = 귀
        if (mcom_idx != -1) {
          mcom = this.mComboKey[mcom_idx];
          mcom_idx = this.joongsungKey.indexOf(mcom);
          str_uni = (cho_idx * 21 + mcom_idx) * 28 + 44032;
          let hangeul = String.fromCharCode(str_uni);
          this.deleteKeyValue();
          return hangeul;
          // 모음(중성) + 친글자(모음) 콤보 아닌경우 ex.구 + ㅏ = 구ㅏ
        } else {
          return text;
        }
      }
      return "";
    },
    keyClick(key) {
      //한글용 키패드
      let text = this.resultTxt;
      let lasttext = text.substring(text.length - 1);
      if (this.boardType == "num" && key == "한/영") {
        return;
      } else if (key == "Shift") {
        this.shiftkey = !this.shiftkey;
      } else if (key == "한/영") {
        this.transLanguage();
      } else if (key == "123") {
        this.transKeyboard("num");
      } else if (key == "한글" || key == "ABC") {
        this.transKeyboard("text");
      } else if (key == "Enter") {
        this.clickEnter();
      } else if (key == "간격" || key == "Space") {
        this.clickSpace();
      } else if (key == "Delete") {
        this.deleteKeyValue();
      } else if (
        this.language == "Kor" &&
        this.boardType == "text" &&
        lasttext.trim() != ""
      ) {
        let korTxt = this.keyValueChk(key, lasttext);
        this.changeResultTxt(korTxt);
      } else {
        this.changeResultTxt(key);
      }
    },
    // 엔터 클릭
    clickEnter() {
      this.resultTxt = this.resultTxt + "\n";
    },
    // 한 칸 띄어쓰기
    clickSpace() {
      this.resultTxt = this.resultTxt + " ";
    },
    //한 글자 지우기
    deleteKeyValue() {
      this.resultTxt = this.resultTxt.substr(0, this.resultTxt.length - 1);
    }
  }
};
</script>

<style lang="scss" scoped>
textarea {
  position: fixed;
  height: 50vh;
  width: 90%;
  left: 5%;
  bottom: 45vh;
  font-size: 3rem;
}
.keypad {
  position: fixed;
  height: 30vh;
  width: 90%;
  left: 5%;
  bottom: 0;
  background-color: gainsboro;
  padding: 1rem;
  z-index: 101;
}
.key {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 8vh;
  height: 5vh;
  margin: 0.6vh;
  border-radius: 0.7rem;
  background-color: #fff;
  font-size: 2vh;
  line-height: 1vh;
  cursor: pointer;
  &__row {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 20%;
    margin-bottom: 1.5vh;
    .key[data-name="Shift"],
    .key[data-name="Delete"],
    .key[data-name="123"],
    .key[data-name="ABC"],
    .key[data-name="한글"] {
      width: 13vh;
      font-size: 2vh;
    }
    .key[data-name="Enter"],
    .key[data-name="한/영"] {
      width: 15vh;
      font-size: 2vh;
    }
    .key[data-name="Space"],
    .key[data-name="간격"] {
      width: 33.08vh;
      font-size: 2vh;
    }
  }
}
</style>
